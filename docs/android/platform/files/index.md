---
title: Xamarin.Android でのファイル アクセス
description: このガイドには、Xamarin.Android でのファイル アクセスはについて説明します
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 476f1c50a2f1a4199dfaf1996fc9c16615b40598
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116798"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>ファイル格納処理および Xamarin.Android を使用したアクセス

Android アプリの一般的な要件は、ファイルを操作する&ndash;画像の保存、ドキュメントをダウンロードまたは他のプログラムを共有するデータをエクスポートします。 (これは、Linux に基づいた) android では、ファイルの記憶域スペースを提供することで、これをサポートします。 Android では、2 つの異なる種類のストレージにファイル システムをグループ化します。

* **内部記憶域**&ndash;アプリケーションまたはオペレーティング システムによってのみアクセスできるファイル システムの一部になります。
* **外部ストレージ**&ndash;すべてのアプリ、ユーザー、およびその他のデバイスからアクセスできるファイルの記憶域のパーティションになります。 一部のデバイスでは、外部の記憶域が (SD カード) などのリムーバブル可能性があります。

これらのグループ化はのみ、概念と、1 つのパーティションまたはデバイス上のディレクトリに参照しないとは限りません。 Android デバイスは内部記憶域および外部記憶域のパーティションを常に提供します。 特定のデバイスが外部記憶域と見なされる複数のパーティションがあることができます。 読み取り用の API は、パーティションに関係なく書き込み、またはファイルの作成は、同じです。 これには 2 つのファイルへのアクセス、Xamarin.Android アプリケーションが使用できる API のセットがあります。

1. **.NET API (Mono によって提供され、Xamarin.Android によってラップされた)** &ndash;これらが含まれています、[ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)によって提供される[Xamarin.Essentials](~/essentials/index.md?context=xamarin/android)します。 .NET API が最適なクロス プラットフォームの互換性を提供し、これらの API でこのガイドの焦点となるようします。
1. **ネイティブの Java ファイル アクセス API (Java によって提供され、Xamarin.Android によってラップされた)** &ndash; Java のファイルの読み書きに独自の API を提供します。 .NET api の代わりに完全に許容されるはこれらが、Android に固有し、はクロスプラット フォーム対応にすることを意図したアプリには適しません。

他の .NET アプリケーションには、読み取りと書き込みをファイルは Xamarin.Android でとほぼ同じです。 Xamarin.Android アプリでは、し、使用して標準的な .NET の表現方法ファイル アクセス、ファイル操作されるへのパスを決定します。 内部および外部の記憶域への実際のパスがデバイスからデバイスに異なる場合がありますので、または Android のバージョンの Android バージョンから、これは推奨されませんハード コードするファイルへのパス。 代わりに、Xamarin.Android API を使用して、ファイルへのパスを決定します。 これにより、ファイルの読み取りと書き込み用の .NET API を内部および外部の記憶域上のファイルへのパスを判断できるように、ネイティブの Android API を公開します。

ファイルへのアクセスに関連する API を説明する前に、内部および外部ストレージに関連する詳細の一部を理解しておく必要です。 これについては、次のセクションで説明します。

## <a name="internal-vs-external-storage"></a>内部と外部のストレージ

内部記憶域および外部記憶域が非常に似ていますが概念的には、&ndash;は両方の場所を Xamarin.Android アプリがファイルを保存可能性があります。 この類似性は、アプリが内部ストレージ vs の外部ストレージを使用する必要がありますとクリアはできないためには Android に慣れていない開発者の混乱を招く可能性があります。

内部記憶域は、Android オペレーティング システム、Apk、および個々 のアプリの割り当てを非揮発性メモリを指します。 この領域は、オペレーティング システムまたはアプリによってアクセスがありません。 Android では、各アプリの内部記憶域のパーティション内のディレクトリを割り当てられます。 アプリがアンインストールされると、そのディレクトリ内での内部ストレージで保持されているすべてのファイルも削除されます。 内部記憶域は、ファイル、アプリにアクセスできるだけおよび他のアプリを共有しないまたはアプリをアンインストールすると、非常に小さい値になりますが最適です。 Android 6.0 以上の内部記憶域上のファイルに自動的にバックアップできる Google を使用して、 [Android 6.0 での自動バックアップ機能](https://developer.android.com/guide/topics/data/autobackup)します。 内部ストレージでは、次の欠点があります。

* ファイルを共有することはできません。
* アプリがアンインストールされるときに、ファイルが削除されます。
* おそらく制限の内部記憶域の空き容量。

外部ストレージが内部ストレージではないファイル ストレージを参照し、アプリにのみアクセスできます。 外部ストレージの主な目的では、ファイルは、アプリ間で共有するためのものをや内部の記憶域に収まらないファイルを配置する場所を提供します。 外部ストレージの利点は、ファイルの内部記憶域よりもはるかに容量を通常です。 ただし、外部ストレージはデバイス上に存在するが保証されない場合、それにアクセスするには、ユーザーから特別なアクセス許可を必要があります。

> [!NOTE]
> 複数のユーザーをサポートするデバイス、Android が提供する各ユーザー独自のディレクトリ内部および外部の記憶域。 このディレクトリでは、デバイス上の他のユーザーにアクセスできません。 内部または外部ストレージ ファイルにパスをハードコーディングしない限りは、この分離をアプリに表示されません。

経験上、として Xamarin.Android アプリを優先するが妥当とき、に、内部記憶域上にファイルを保存し、ファイルは、他のアプリと共有する必要がありますまたは、非常に大きい場合でも、アプリのアンインストールを保持するときに、外部ストレージに依存します。 たとえば、構成ファイルは最適ですが内部ストレージを作成したアプリを除く重要性があるないため。  これに対し、写真は、外部ストレージの有力候補です。 非常に大きな可能性があるし、多くの場合、ユーザーがそれらを共有したり、アプリがアンインストールされた場合でもそれらにアクセスする可能性があります。

このガイドの内部記憶域に焦点を当てます。 このガイドを参照してください[外部ストレージ](~/android/platform/files/external-storage.md)Xamarin.Android アプリケーションでの外部ストレージの使用の詳細について。

## <a name="working-with-internal-storage"></a>内部記憶域の使用

アプリケーションの内部記憶域ディレクトリは、オペレーティング システムによって決まりますして Android アプリに公開される、`Android.Content.Context.FilesDir`プロパティ。 これにより返されます、 `Java.IO.File` Android には、アプリ専用の専用のディレクトリを表すオブジェクト。  たとえば、パッケージ名を持つアプリ**com.companyname**内部ストレージ ディレクトリがあります。

```bash
/data/user/0/com.companyname/files
```

このドキュメントとして内部ストレージ ディレクトリを参照してください_内部\_ストレージ_します。

> [!IMPORTANT]
> 内部ストレージ ディレクトリの正確なパスは、デバイスから、デバイスおよび Android のバージョンとの間に異なります。 このため、アプリケーションする必要がありますハード コードの内部のファイルのストレージ ディレクトリをパス代わりに API を使用して、Xamarin.Android など`System.Environment.GetFolderPath()`します。

Xamarin.Android アプリ (または Xamarin.Android を対象とする Xamarin.Forms アプリ) を使用する必要がありますコードの共有を最大化する、 [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*)メソッド。 Xamarin.Android では、このメソッドは、ディレクトリと同じ場所である文字列を返します`Android.Content.Context.FilesDir`します。 このメソッドは、列挙型`System.Environment.SpecialFolder`を使用して、オペレーティング システムで使用される特別なフォルダーのパスを表す列挙定数のセットを特定します。 すべての`System.Environment.SpecialFolder`値は、Xamarin.Android の有効なディレクトリにマップされます。 次の表の指定した値にどのようなパスが予想される`System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | パス  |
|----------------------|---|
| `ApplicationData` | **_内部\_ストレージ_/.config** |
| `Desktop` | **_内部\_ストレージ_  /デスクトップ** |
| `LocalApplicationData` | **_内部\_ストレージ_/.local/share** |
| `MyComputer` | **_内部\_ストレージ_/.local/share** |
| `MyDocuments` | **_内部\_ストレージ_** |
| `MyMusic` | **_内部\_ストレージ_/Music** |
| `MyPictures` | **_内部\_ストレージ_/Music** |
| `MyVideos` | **_内部\_ストレージ_/Videos** |
| `Personal` | **_内部\_ストレージ_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>内部記憶域上のファイルに対する読み取りまたは書き込み

いずれか、 [ C#書き込み用 API](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file)はファイルに必要な場合は、アプリケーションに割り当てられているディレクトリにあるファイルへのパスを取得するために必要なすべてが。 非同期の可能性のある問題を最小限に抑える .NET API のバージョンが使用されますが、メイン スレッドをブロックしているファイルへのアクセスと関連付けることを強くお勧めします。

このコード スニペットでは、アプリケーションの内部記憶域ディレクトリに utf-8 テキスト ファイルへの整数を書き込みの 1 つの例を示します。

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

次のコード スニペットは、テキスト ファイルに格納された整数値を読み取ることの 1 つの方法を提供します。

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin.Essentials を使用して&ndash;ファイル システム ヘルパー

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)一連の API は、クロス プラットフォームの互換性のあるコードを記述します。 [ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)一連のアプリケーションのキャッシュおよびデータ ディレクトリを検索する簡略化するヘルパー クラスです。 このコード スニペットでは、アプリの内部記憶域ディレクトリとキャッシュのディレクトリを検索する方法の例を示します。

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>ファイルが非表示、 `MediaStore`

`MediaStore` Android デバイスでメディア ファイル (ビデオ、音楽、画像など) についてのメタ データを収集する Android のコンポーネントは、します。 その目的は、デバイス上のすべての Android アプリでこれらのファイル共有を簡略化します。

プライベート ファイルも共有可能なメディアとして表示されません。 など、アプリは、そのプライベート外部ストレージに画像を保存する場合、そのファイルいないによりが取得されますメディア スキャナー (`MediaStore`)。

公開ファイルによってピックアップされます`MediaStore`します。 ゼロ バイト ファイルの名前を持つディレクトリ**します。NOMEDIA**ではスキャンされません`MediaStore`します。

## <a name="related-links"></a>関連リンク

* [外部ストレージ](~/android/platform/files/external-storage.md)
* [デバイス ストレージ上のファイルを保存します。](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin/android)
* [自動バックアップでバックアップのユーザー データ](https://developer.android.com/guide/topics/data/autobackup)
* [記憶域の導入](https://source.android.com/devices/storage/adoptable)
