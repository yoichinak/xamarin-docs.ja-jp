---
title: Xamarin Android を使用したファイルアクセス
description: このガイドでは、Xamarin. Android でのファイルアクセスについて説明します。
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: bf4b0f4ed6ade69808314ac7e7a51270aa3a847e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758896"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Xamarin Android を使用した File Storage とアクセス

Android アプリの一般的な要件として、 &ndash;画像の保存、ドキュメントのダウンロード、または他のプログラムと共有するためのデータのエクスポートがあります。 (これは、Linux に基づいた) Android では、ファイルの記憶域スペースを提供することで、これをサポートします。 Android は、次の2種類の記憶域にファイルシステムをグループ化します。

* **内部ストレージ**&ndash;これは、アプリケーションまたはオペレーティングシステムによってのみアクセス可能なファイルシステムの一部です。
* **外部ストレージ**&ndash;これは、すべてのアプリ、ユーザー、および場合によっては他のデバイスからアクセスできるファイルを格納するためのパーティションです。 一部のデバイスでは、外部記憶装置をリムーバブル (SD カードなど) にすることができます。

これらのグループ化は概念的なものであり、必ずしもデバイス上の単一のパーティションまたはディレクトリを参照するわけではありません。 Android デバイスは、常に内部ストレージと外部ストレージのパーティションを提供します。 特定のデバイスに、外部ストレージと見なされる複数のパーティションがある可能性があります。 読み取り用の API は、パーティションに関係なく書き込み、またはファイルの作成は、同じです。 これには 2 つのファイルへのアクセス、Xamarin.Android アプリケーションが使用できる API のセットがあります。

1. **.NET API (Mono によって提供され、Xamarin.Android によってラップされた)** &ndash;これらが含まれています、[ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)によって提供される[Xamarin.Essentials](~/essentials/index.md?context=xamarin/android)します。 .NET API が最適なクロス プラットフォームの互換性を提供し、これらの API でこのガイドの焦点となるようします。
1. **ネイティブの Java ファイル アクセス API (Java によって提供され、Xamarin.Android によってラップされた)** &ndash; Java のファイルの読み書きに独自の API を提供します。 これらは .NET Api には完全に許容されますが、Android に固有のものであり、クロスプラットフォーム用のアプリには適していません。

ファイルの読み取りと書き込みは、他の .NET アプリケーションと同様に、Xamarin. Android でもほぼ同じです。 Xamarin Android アプリでは、操作するファイルへのパスを決定し、ファイルアクセスに標準の .NET 表現方式を使用します。 内部および外部のストレージへの実際のパスは、デバイスやデバイスによって異なる場合があるため、ファイルへのパスをハードコーディングしないことをお勧めします。 代わりに、Xamarin.Android API を使用して、ファイルへのパスを決定します。 これにより、ファイルの読み取りと書き込み用の .NET API を内部および外部の記憶域上のファイルへのパスを判断できるように、ネイティブの Android API を公開します。

ファイルへのアクセスに関連する API を説明する前に、内部および外部ストレージに関連する詳細の一部を理解しておく必要です。 これについては、次のセクションで説明します。

## <a name="internal-vs-external-storage"></a>内部および外部ストレージ

概念的には、内部ストレージと外部ストレージは&ndash;どちらも、Xamarin android アプリがファイルを保存する場所に似ています。 この類似性は、アプリが内部ストレージと外部ストレージのどちらを使用する必要があるかが明確ではないため、Android に慣れていない開発者にとって混乱する可能性があります。

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

アプリケーションの内部ストレージディレクトリはオペレーティングシステムによって決定され、 `Android.Content.Context.FilesDir`プロパティによって Android アプリに公開されます。 これにより、 `Java.IO.File` Android が専用のアプリ専用のディレクトリを表すオブジェクトが返されます。  たとえば、パッケージ名が " **com. companyname** " のアプリは、次のようになります。

```bash
/data/user/0/com.companyname/files
```

このドキュメントでは、内部ストレージディレクトリを_内部\_ストレージ_と呼びます。

> [!IMPORTANT]
> 内部ストレージディレクトリへの正確なパスは、デバイス、デバイス、Android のバージョンによって異なります。 このため、アプリケーションする必要がありますハード コードの内部のファイルのストレージ ディレクトリをパス代わりに API を使用して、Xamarin.Android など`System.Environment.GetFolderPath()`します。

コード共有を最大化するには、xamarin android アプリ (または xamarin を対象とする xamarin) アプリ[`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*)でメソッドを使用する必要があります。 Xamarin Android では、このメソッドはと`Android.Content.Context.FilesDir`同じ場所にあるディレクトリの文字列を返します。 このメソッドは列挙型`System.Environment.SpecialFolder`を受け取ります。これは、オペレーティングシステムによって使用される特殊なフォルダーのパスを表す列挙定数のセットを識別するために使用されます。 すべての値が`System.Environment.SpecialFolder` 、Xamarin. Android 上の有効なディレクトリにマップされるとは限りません。 次の`System.Environment.SpecialFolder`表では、指定された値に対して予想されるパスについて説明します。

| System.environment.specialfolder | パス  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_内部\_ストレージ_/デスクトップ** |
| `LocalApplicationData` | **_内部\_ストレージ_/.local/share** |
| `MyDocuments` | **_内部\_ストレージ_** |
| `MyMusic` | **_内部\_ストレージ_/音楽** |
| `MyPictures` | **_内部\_ストレージ_/画像** |
| `MyVideos` | **_内部\_ストレージ_/ビデオ** |
| `Personal` | **_内部\_ストレージ_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>内部ストレージでのファイルの読み取りまたは書き込み

いずれか、 [ C#書き込み用 API](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file)はファイルに必要な場合は、アプリケーションに割り当てられているディレクトリにあるファイルへのパスを取得するために必要なすべてが。 非同期の可能性のある問題を最小限に抑える .NET API のバージョンが使用されますが、メイン スレッドをブロックしているファイルへのアクセスと関連付けることを強くお勧めします。

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin. Essentials &ndash;ファイルシステムヘルパーの使用

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)一連の API は、クロス プラットフォームの互換性のあるコードを記述します。 [ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)は、アプリケーションのキャッシュとデータディレクトリを簡単に検索できるようにする一連のヘルパーを含むクラスです。 次のコードスニペットは、アプリの内部ストレージディレクトリとキャッシュディレクトリを検索する方法の例を示しています。

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>ファイルを非表示にする`MediaStore`

は`MediaStore` 、android デバイス上のメディアファイル (ビデオ、音楽、イメージ) に関するメタデータを収集する android コンポーネントです。 その目的は、デバイス上のすべての Android アプリでこれらのファイルを簡単に共有することです。

プライベートファイルは、共有可能なメディアとして表示されません。 たとえば、アプリが画像をプライベート外部ストレージに保存する場合、そのファイルはメディアスキャナー (`MediaStore`) によって取得されません。

パブリックファイルは、によって`MediaStore`取得されます。 ファイル名が0バイトのディレクトリ **。NOMEDIA**はによって`MediaStore`スキャンされません。

## <a name="related-links"></a>関連リンク

* [外部ストレージ](~/android/platform/files/external-storage.md)
* [デバイスストレージにファイルを保存する](https://developer.android.com/training/data-storage/files)
* [Xamarin. Essentials ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)
* [自動バックアップを使用してユーザーデータをバックアップする](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable ストレージ](https://source.android.com/devices/storage/adoptable)
