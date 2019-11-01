---
title: Xamarin Android を使用したファイルアクセス
description: このガイドでは、Xamarin. Android でのファイルアクセスについて説明します。
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 1bb0fae73a1e3647cdc0e3266c7b44ac04fcc1ee
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020426"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Xamarin Android を使用した File Storage とアクセス

Android アプリの一般的な要件として、画像の保存、ドキュメントのダウンロード、他のプログラムと共有するためのデータのエクスポートなど、ファイルの操作 &ndash; 操作があります。 Android (Linux ベース) は、ファイルストレージ用の領域を提供することで、これをサポートしています。 Android は、次の2種類の記憶域にファイルシステムをグループ化します。

* **内部ストレージ**&ndash; これは、アプリケーションまたはオペレーティングシステムによってのみアクセス可能なファイルシステムの一部です。
* **外部ストレージ**&ndash; これは、すべてのアプリ、ユーザー、および場合によっては他のデバイスからアクセスできるファイルを格納するためのパーティションです。 一部のデバイスでは、外部記憶装置をリムーバブル (SD カードなど) にすることができます。

これらのグループ化は概念的なものであり、必ずしもデバイス上の単一のパーティションまたはディレクトリを参照するわけではありません。 Android デバイスは、常に内部ストレージと外部ストレージのパーティションを提供します。 特定のデバイスに、外部ストレージと見なされる複数のパーティションがある可能性があります。 パーティションに関係なく、ファイルの読み取り、書き込み、または作成のための Api は同じです。 Xamarin Android アプリケーションでは、次の2つのセットの Api を使用してファイルアクセスを行うことができます。

1. **(Mono によって提供され、xamarin によってラップされる) .Net Api には、** [xamarin. Essentials](~/essentials/index.md?context=xamarin/android)によって提供される[ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)が含まれて &ndash; ます。 .NET Api は、プラットフォーム間の互換性を確保するため、このガイドではこれらの Api に焦点を当てます。
1. Java によって**提供され、Xamarin Android によってラップされるネイティブ java ファイルアクセス Api は、** ファイルの読み取りと書き込み用に独自の api を提供 &ndash; ます。 これらは .NET Api には完全に許容されますが、Android に固有のものであり、クロスプラットフォーム用のアプリには適していません。

ファイルの読み取りと書き込みは、他の .NET アプリケーションと同様に、Xamarin. Android でもほぼ同じです。 Xamarin Android アプリでは、操作するファイルへのパスを決定し、ファイルアクセスに標準の .NET 表現方式を使用します。 内部および外部のストレージへの実際のパスは、デバイスやデバイスによって異なる場合があるため、ファイルへのパスをハードコーディングしないことをお勧めします。 代わりに、Xamarin Api を使用してファイルへのパスを決定します。 これにより、ファイルの読み取りと書き込みのための .NET Api は、内部および外部のストレージ上のファイルへのパスを決定するのに役立つネイティブ Android Api を公開します。

ファイルへのアクセスに関連する Api について説明する前に、内部および外部のストレージに関する詳細を理解しておくことが重要です。 これについては、次のセクションで説明します。

## <a name="internal-vs-external-storage"></a>内部および外部ストレージ

概念的には、内部ストレージと外部ストレージが似ていますが、どちらの場所にも、Xamarin Android アプリでファイルを保存することが &ndash; ます。 この類似性は、アプリが内部ストレージと外部ストレージのどちらを使用する必要があるかが明確ではないため、Android に慣れていない開発者にとって混乱する可能性があります。

内部ストレージとは、Android がオペレーティングシステム、APKs、および個々のアプリに割り当てる非揮発性メモリを指します。 この領域には、オペレーティングシステムまたはアプリ以外ではアクセスできません。 Android では、各アプリの内部ストレージパーティションにディレクトリが割り当てられます。 アプリがアンインストールされると、そのディレクトリ内の内部ストレージに保持されているすべてのファイルも削除されます。 内部ストレージは、アプリからしかアクセスできず、他のアプリと共有されない、またはアプリがアンインストールされた後の値がほとんどないファイルに最適です。 Android 6.0 以降では、 [android 6.0 の自動バックアップ機能](https://developer.android.com/guide/topics/data/autobackup)を使用して、Google によって内部ストレージ上のファイルが自動的にバックアップされる場合があります。 内部ストレージには、次のような欠点があります。

* ファイルを共有することはできません。
* アプリがアンインストールされると、ファイルは削除されます。
* 内部ストレージで使用可能な領域が制限されることがあります。

外部ストレージとは、アプリから排他的にアクセスできない、内部ストレージではないファイルストレージを指します。 外部ストレージの主な目的は、アプリ間で共有されるファイルや、内部ストレージには大きすぎるファイルを配置する場所を提供することです。 外部ストレージの利点は、通常、ファイルの領域が内部ストレージよりもはるかに多くなることです。 ただし、外部ストレージは常にデバイス上に存在するとは限りません。また、アクセスするには、ユーザーからの特別なアクセス許可が必要になる場合があります。

> [!NOTE]
> 複数のユーザーをサポートするデバイスの場合、Android は内部および外部の両方のストレージに独自のディレクトリを提供します。 このディレクトリは、デバイス上の他のユーザーにはアクセスできません。 この分離は、内部または外部のストレージ上のファイルへのパスをハードコーディングしない限り、アプリには見えません。

経験則として、Xamarin Android アプリでは、適切であれば内部ストレージにファイルを保存することをお勧めします。また、ファイルを他のアプリと共有する必要がある場合、非常に大きい場合、またはアプリがアンインストールされた場合でも保持する必要がある場合は、外部ストレージに依存します。 たとえば、構成ファイルは、それを作成するアプリ以外は重要ではないため、内部ストレージに最適です。  これに対して、写真は外部ストレージの候補として適しています。 これらは非常に大きくなる可能性があり、多くの場合、アプリがアンインストールされた場合でも、ユーザーはそれらを共有したりアクセスしたりすることができます。

このガイドでは、内部ストレージに焦点を当てます。 Xamarin Android アプリケーションで外部ストレージを使用する方法の詳細については、「[外部ストレージ](~/android/platform/files/external-storage.md)のガイド」を参照してください。

## <a name="working-with-internal-storage"></a>内部ストレージの操作

アプリケーションの内部ストレージディレクトリはオペレーティングシステムによって決定され、`Android.Content.Context.FilesDir` プロパティによって Android アプリに公開されます。 これにより、Android が専用のアプリ専用のディレクトリを表す `Java.IO.File` オブジェクトが返されます。  たとえば、パッケージ名が " **com. companyname** " のアプリは、次のようになります。

```bash
/data/user/0/com.companyname/files
```

このドキュメントでは、内部ストレージディレクトリを_内部\_ストレージ_と呼びます。

> [!IMPORTANT]
> 内部ストレージディレクトリへの正確なパスは、デバイス、デバイス、Android のバージョンによって異なります。 このため、アプリは内部ファイルストレージディレクトリへのパスをハードコーディングしないでください。代わりに、`System.Environment.GetFolderPath()`などの Xamarin Api を使用してください。

コード共有を最大化するには、xamarin Android アプリ (または Xamarin Android を対象とする Xamarin. Forms アプリ) で[`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*)メソッドを使用する必要があります。 Xamarin Android では、このメソッドは `Android.Content.Context.FilesDir`と同じ場所にあるディレクトリの文字列を返します。 このメソッドは、列挙型の `System.Environment.SpecialFolder`を受け取ります。これは、オペレーティングシステムによって使用される特殊なフォルダーのパスを表す列挙定数のセットを識別するために使用されます。 すべての `System.Environment.SpecialFolder` 値が、Xamarin. Android 上の有効なディレクトリにマップされるとは限りません。 次の表では、`System.Environment.SpecialFolder`の特定の値に対して想定できるパスについて説明します。

| System.environment.specialfolder | パス  |
|----------------------|---|
| `ApplicationData` | **_内部\_ストレージ_/.config** |
| `Desktop` | **_内部\_ストレージ_/デスクトップ** |
| `LocalApplicationData` | **_内部\_ストレージ_/.local/share** |
| `MyDocuments` | **_内部\_ストレージ_** |
| `MyMusic` | **_内部\_ストレージ_/音楽** |
| `MyPictures` | **_内部\_ストレージ_/画像** |
| `MyVideos` | **_内部\_ストレージ_/ビデオ** |
| `Personal` | **_内部\_ストレージ_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>内部ストレージでのファイルの読み取りまたは書き込み

ファイルに[ C#書き込むための api](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file)のいずれも十分です。必要なのは、アプリケーションに割り当てられているディレクトリ内のファイルへのパスを取得することだけです。 メインスレッドをブロックするファイルアクセスに関連する可能性のある問題を最小限に抑えるために、非同期バージョンの .NET Api を使用することを強くお勧めします。

このコードスニペットは、アプリケーションの内部ストレージディレクトリに UTF-8 テキストファイルに整数を書き込む例の1つです。

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

次のコードスニペットは、テキストファイルに格納されている整数値を読み取るための1つの方法を提供します。

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin. Essentials &ndash; ファイルシステムヘルパーの使用

[Xamarin. Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)は、クロスプラットフォームと互換性のあるコードを記述するための api のセットです。 [ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)は、アプリケーションのキャッシュとデータディレクトリを簡単に検索できるようにする一連のヘルパーを含むクラスです。 次のコードスニペットは、アプリの内部ストレージディレクトリとキャッシュディレクトリを検索する方法の例を示しています。

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>`MediaStore` からのファイルの非表示

`MediaStore` は、Android デバイス上のメディアファイル (ビデオ、音楽、イメージ) に関するメタデータを収集する Android コンポーネントです。 その目的は、デバイス上のすべての Android アプリでこれらのファイルを簡単に共有することです。

プライベートファイルは、共有可能なメディアとして表示されません。 たとえば、アプリが画像をプライベート外部ストレージに保存する場合、そのファイルはメディアスキャナー (`MediaStore`) によって取得されません。

パブリックファイルは `MediaStore`によって取得されます。 ファイル名が0バイトのディレクトリ **。** `MediaStore`では、NOMEDIA はスキャンされません。

## <a name="related-links"></a>関連リンク

* [外部ストレージ](~/android/platform/files/external-storage.md)
* [デバイスストレージにファイルを保存する](https://developer.android.com/training/data-storage/files)
* [Xamarin. Essentials ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)
* [自動バックアップを使用してユーザーデータをバックアップする](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable ストレージ](https://source.android.com/devices/storage/adoptable)
