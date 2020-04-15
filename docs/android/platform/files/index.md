---
title: Xamarin.Android でのファイル アクセス
description: このガイドでは、Xamarin. Android でのファイル アクセスについて説明します
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 1bb0fae73a1e3647cdc0e3266c7b44ac04fcc1ee
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303667"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Xamarin.Android でのファイルストレージとアクセス

Android アプリの一般的な要件として、画像の保存、ドキュメントのダウンロード、他のプログラムと共有するためのデータのエクスポートなど、ファイルの操作 &ndash; があります。 Android (Linux ベース) は、ファイル ストレージ用の領域を提供することで、これをサポートしています。 Android は、次の 2 種類のストレージにファイル システムをグループ化します。

* **内部ストレージ** &ndash; これは、アプリケーションまたはオペレーティング システムによってのみアクセス可能なファイルシステムの一部です。
* **外部ストレージ** &ndash; これは、すべてのアプリ、ユーザー、および場合によっては他のデバイスからアクセスできるファイルを格納するためのパーティションです。 一部のデバイスでは、外部ストレージをリムーバブル (SD カードなど) にすることができます。

これらのグループ化は概念的なものであり、必ずしもデバイス上の単一のパーティションまたはディレクトリを参照するわけではありません。 Android デバイスは、常に内部ストレージと外部ストレージのパーティションを提供します。 特定のデバイスに、外部ストレージと見なされる複数のパーティションがある可能性があります。 パーティションに関係なく、ファイルの読み取り、書き込み、または作成のための API は同じものです。 Xamarin Android アプリケーションがファイル アクセスに使用する場合がある API の 2 つのセットがあります。

1. **NET API (Mono によって提供され、Xamarin Android によってラップされる)**  &ndash; これらの API には [Xamarin. Essentials](~/essentials/index.md?context=xamarin/android)によって提供される [ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android) が含まれています。 .NET API は、最適なプラットフォーム間の互換性を提供し、このガイドではこれらの API に焦点を当てます。
1. **ネイティブ Java ファイルアクセス API (Java によって提供され、Xamarin Android によってラップされる)** &ndash; Java は、ファイルの読み取りと書き込み用の独自 API を提供します。 これらは .NET API に代わるものとして完全に許容されますが、Android に固有のものであり、クロスプラットフォーム用のアプリには適していません。

ファイルの読み取りと書き込みは、Xamarin.Android でも他の .NET アプリケーションとほぼ同じです。 Xamarin.Android アプリでは、操作するファイルへのパスが決定された後、ファイル アクセスに標準の .NET イディオムが使用されます。 内部ストレージと外部ストレージへの実際のパスは、デバイスによって、または Android のバージョンによって異なる場合があるため、ファイルへのパスをハード コーディングしないことをお勧めします。 代わりに、Xamarin.Android API を使用してファイルへのパスを決定します。 そのように、ファイルの読み取りと書き込みを行う .NET API では、内部ストレージと外部ストレージのファイルへのパスを決定するのに役立つ、ネイティブの Android API が公開されています。

ファイルへのアクセスに関連する API について説明する前に、内部ストレージおよび外部ストレージに関する詳細を理解しておくことが重要です。 これについては、次のセクションで説明します。

## <a name="internal-vs-external-storage"></a>内部ストレージおよび外部ストレージ

概念的には、内部ストレージと外部ストレージは似ていますが、&ndash; どちらの場所にも、Xamarin Android アプリでファイルを保存することができます。 この類似性は、アプリが内部ストレージと外部ストレージのどちらを使用する必要があるかが明確ではないため、Android に慣れていない開発者にとって混乱する可能性があります。

内部ストレージとは、Android がオペレーティング システム、APK、および個々のアプリに割り当てる非揮発性メモリを指します。 この領域には、オペレーティング システムまたはアプリ以外ではアクセスできません。 Android では、各アプリの内部ストレージ パーティションにディレクトリが割り当てられます。 アプリがアンインストールされると、そのディレクトリ内の内部ストレージに保持されているすべてのファイルも削除されます。 内部ストレージは、アプリからしかアクセスできず、他のアプリと共有されない、またはアプリがアンインストールされた後の値がほとんどないファイルに最適です。 Android 6.0 以降では、[Android 6.0 の自動バックアップ機能](https://developer.android.com/guide/topics/data/autobackup) を使用して、Google によって内部ストレージ上のファイルが自動的にバックアップされる場合があります。 内部ストレージの欠点は次のとおりです。

* ファイルを共有できません。
* アプリをアンインストールしても、ファイルは削除されません。
* 内部ストレージで使用可能な領域が制限されることがあります。

外部ストレージとは、内部ストレージではなく、アプリから排他的にアクセスできないファイル ストレージのことです。 外部ストレージの主な目的は、アプリ間で共有されるファイルや、大きすぎて内部ストレージに収まらないファイルを配置する場所を提供することです。 外部ストレージの利点は、通常、ファイルの領域が内部ストレージよりもはるかに多くなることです。 ただし、外部ストレージは常にデバイス上に存在するとは限りません。また、アクセスするには、ユーザーからの特別なアクセス許可が必要になる場合があります。

> [!NOTE]
> 複数のユーザーをサポートするデバイスの場合、Android は内部および外部の両方のストレージに独自のディレクトリを提供します。 このディレクトリは、デバイス上の他のユーザーにはアクセスできません。 この分離は、内部ストレージまたは外部ストレージ上のファイルへのパスをハードコーディングしない限り、アプリには表示されません。

経験則として、Xamarin Android アプリでは、適切であれば内部ストレージにファイルを保存することをお勧めします。また、ファイルを他のアプリと共有する必要があり、その容量が非常に大きい場合、またはアプリがアンインストールされた場合でもそのファイルを保持する必要がある場合は、外部ストレージを利用することをお勧めします。 たとえば、構成ファイルは、それを作成するアプリ以外は重要ではないため、内部ストレージに最適です。  これに対して、写真は外部ストレージの候補として適しています。 これらは非常に大きな容量となる可能性があり、多くの場合、アプリがアンインストールされた場合でも、ユーザーはそれらを共有したりアクセスしたりする場合があります。

このガイドでは、内部ストレージに焦点を当てます。 Xamarin Android アプリケーションで外部ストレージを使用する方法の詳細については、ガイド「[外部ストレージ](~/android/platform/files/external-storage.md)」を参照してください。

## <a name="working-with-internal-storage"></a>内部ストレージの操作

アプリケーションの内部ストレージ ディレクトリはオペレーティング システムによって決定され、`Android.Content.Context.FilesDir` プロパティによって Android アプリに公開されます。 これにより、Android がアプリ専用に割り当てたディレクトリを表す `Java.IO.File` オブジェクトが返されます。  たとえば、パッケージ名が **com.companyname** であるアプリなどです。内部ストレージ ディレクトリは次のようになる場合があります。

```bash
/data/user/0/com.companyname/files
```

このドキュメントでは、内部ストレージディレクトリを _内部\_ストレージ_ と呼びます。

> [!IMPORTANT]
> 内部ストレージ ディレクトリへの正確なパスは、デバイスにより、および Android のバージョンにより、異なる場合があります。 このため、アプリでは、この内部ファイル ストレージ ディレクトリへのパスをハード コーディングしてはならず、代わりに `System.Environment.GetFolderPath()` などの Xamarin.Android API を使用する必要があります。

コード共有を最大化するには、Xamarin Android アプリ (または Xamarin Android を対象とする Xamarin.Forms アプリ) で [`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*) メソッドを使用する必要があります。 Xamarin Android では、このメソッドは `Android.Content.Context.FilesDir` と同じ場所にあるディレクトリの文字列を返します。 このメソッドは、列挙型の `System.Environment.SpecialFolder` を受け取ります。これは、オペレーティング システムによって使用される特殊なフォルダーのパスを表す列挙された定数のセットを識別するために使用されます。 すべての `System.Environment.SpecialFolder` 値が、Xamarin. Android 上の有効なディレクトリにマップされるとは限りません。 次の表では、`System.Environment.SpecialFolder` の特定の値に対して想定できるパスについて説明します。

| System.Environment.SpecialFolder | パス  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_INTERNAL\_STORAGE_/Desktop** |
| `LocalApplicationData` | **_INTERNAL\_STORAGE_/.local/share** |
| `MyDocuments` | **_INTERNAL\_STORAGE_** |
| `MyMusic` | **_INTERNAL\_STORAGE_/Music** |
| `MyPictures` | **_INTERNAL\_STORAGE_/Pictures** |
| `MyVideos` | **_INTERNAL\_STORAGE_/Videos** |
| `Personal` | **_INTERNAL\_STORAGE_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>内部ストレージでのファイルの読み取りまたは書き込み

ファイルへの[書き込み用の任意の C# API](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) は十分です。必要なのは、アプリケーションに割り当てられているディレクトリ内のファイルへのパスを取得することだけです。 メイン スレッドをブロックするファイル アクセスに関連する可能性のある問題を最小限に抑えるために、非同期バージョンの .NET API を使用することを強くお勧めします。

このコード スニペットは、アプリケーションの内部ストレージ ディレクトリに UTF-8 テキスト ファイルに整数を書き込む例の 1 つです。

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

次のコード スニペットは、テキスト ファイルに格納されていた整数値を読み取るための 1 つの方法を提供します。

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin.Essentials &ndash; ファイル システム ヘルパーの使用

[Xamarin. Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) は、クロスプラットフォームの互換性のあるコードを記述するための API のセットです。 [ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android) は、アプリケーションのキャッシュとデータ ディレクトリを簡単に検索できるようにする一連のヘルパーを含むクラスです。 このコード スニペットは、アプリの内部ストレージ ディレクトリとキャッシュ ディレクトリを検索する方法の例を示しています。

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>`MediaStore` からファイルを非表示にする

`MediaStore` は、Android デバイス上のメディア ファイル (ビデオ、音楽、イメージ) に関するメタ データを収集する Android コンポーネントです。 その目的は、デバイス上のすべての Android アプリでこれらのファイルを簡単に共有することです。

プライベート ファイルは、共有可能なメディアとして表示されません。 たとえば、アプリが画像を非公開の外部ストレージに保存する場合、そのファイルはメディア スキャナー (`MediaStore`) で取得されません。

パブリック ファイルは `MediaStore`によって取得されます。 0 バイト ファイル名 **のディレクトリ。NOMEDIA** は `MediaStore` によってスキャンされません。

## <a name="related-links"></a>関連リンク

* [外部ストレージ](~/android/platform/files/external-storage.md)
* [デバイス ストレージにファイルを保存する](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)
* [自動バックアップを使用してユーザー データをバックアップする](https://developer.android.com/guide/topics/data/autobackup)
* [採用可能なストレージ](https://source.android.com/devices/storage/adoptable)
