---
title: Xamarin Android を使用した外部ストレージでのファイルアクセス
description: このガイドでは、Xamarin. Android の外部ストレージでのファイルアクセスについて説明します。
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 5992be5963c315c4f93fc4e03c24e3776087d418
ms.sourcegitcommit: 621649fb4a119981290fed7a1061cbae30b982a6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2019
ms.locfileid: "71975798"
---
# <a name="external-storage"></a>外部ストレージ

外部ストレージとは、ファイルストレージを指します。このファイルは、ファイルの役割を果たすアプリから排他的にアクセスできるわけではありません。 外部ストレージの主な目的は、アプリ間で共有されるファイルや、内部ストレージには大きすぎるファイルを配置する場所を提供することです。

これまで、外部ストレージは SD カードなどのリムーバブルメディアのディスクパーティションを指していました (_ポータブルストレージ_とも呼ばれていました)。 この区別は、Android デバイスが進化し、多くの Android デバイスでリムーバブル記憶域がサポートされなくなったため、関連するものではなくなりました。 一部のデバイスでは、同じ機能のリムーバブルメディアを実行するために、Android が使用する内部の非揮発性メモリの一部が割り当てられます。 これは_エミュレート_されたストレージと呼ばれ、依然として外部ストレージと見なされます。 また、Android デバイスによっては、複数の外部ストレージパーティションが存在する場合もあります。 たとえば、Android タブレット (内部記憶域に加えて) には、エミュレートされた記憶域と SD カード用の1つ以上のスロットがあります。 これらのパーティションはすべて、Android によって外部ストレージとして扱われます。

複数のユーザーがいるデバイスでは、各ユーザーが外部ストレージのプライマリ外部ストレージパーティションに専用のディレクトリを持つことになります。 1人のユーザーとして実行されているアプリは、デバイス上の別のユーザーのファイルにアクセスできません。 すべてのユーザーのファイルは、世界中でも読み取り可能であり、世界中でも書き込み可能です。ただし、Android では、各ユーザープロファイルをサンドボックスにサンドボックスします。

ファイルの読み取りと書き込みは、他の .NET アプリケーションと同様に、Xamarin. Android でもほぼ同じです。 Xamarin Android アプリでは、操作するファイルへのパスを決定し、ファイルアクセスに標準の .NET 表現方式を使用します。 内部および外部のストレージへの実際のパスは、デバイスやデバイスによって異なる場合があるため、ファイルへのパスをハードコーディングしないことをお勧めします。 代わりに、Xamarin.Android を内部および外部の記憶域上のファイルへのパスを判断できるように、ネイティブの Android API を公開します。

このガイドは、概念と Android で外部ストレージに固有の API について説明します。

## <a name="public-and-private-files-on-external-storage"></a>外部ストレージ上のパブリックファイルとプライベートファイル

アプリで外部ストレージに保持できるファイルには、次の2種類があります。

* **プライベート**ファイル &ndash; プライベートファイルは、アプリケーションに固有のファイルです (ただし、世界でも読み取り可能で、世界中から書き込み可能です)。 Android では、プライベートファイルが外部ストレージの特定のディレクトリに格納されていることを前提としています。 ファイルは "プライベート" と呼ばれていますが、デバイス上の他のアプリからは引き続き表示され、アクセスできます。 Android で特別な保護を行うことはできません。

* **パブリック**ファイル &ndash; これらは、アプリケーションに固有ではないと見なされ、自由に共有することを意図したファイルです。

これらのファイルの違いは、主に概念です。 プライベートファイルは、アプリケーションの一部と見なされますが、パブリックファイルは外部ストレージに存在するその他のファイルであるという意味でプライベートになります。 Android プライベートおよびパブリックのファイルへのパスを解決するための 2 つの API を提供しますが、これらのファイルを読み書きする同じ .NET API を使用するそれ以外の場合。 これらは、セクションに記載されているのと同じ API[読み取りと書き込み](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)します。

### <a name="private-external-files"></a>プライベート外部ファイル

プライベート外部ファイルはアプリケーションに固有であると見なされますが (内部ファイルに似ています)、さまざまな理由で外部ストレージに保持されています (内部ストレージに対して大きすぎるなど)。 内部ファイルと同様に、ユーザーがアプリをアンインストールすると、これらのファイルは削除されます。

プライベート外部ファイルのプライマリロケーションは、メソッド `Android.Content.Context.GetExternalFilesDir(string type)` を呼び出すことによって検出されます。 このメソッドは、アプリのプライベート外部ストレージディレクトリを表す @no__t 0 オブジェクトを返します。 このメソッドに `null` を渡すと、アプリケーションのユーザーのストレージディレクトリへのパスが返されます。 たとえば、パッケージ名が 0 @no__t のアプリケーションの場合、プライベート外部ファイルの "ルート" ディレクトリは次のようになります。

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

このドキュメントでは、private _@ no__t-1external @ no__t-2storage_として、外部ストレージ上のプライベートファイルのストレージディレクトリを参照します。

@No__t-0 のパラメーターは、_アプリケーションディレクトリ_を指定する文字列です。 これは、ファイルの論理編成の標準的な場所を提供するためのディレクトリです。 文字列値は、@no__t 0 クラスの定数を通じて使用できます。

| `Android.OS.Environment` | ディレクトリ |
|-|-|
| DirectoryAlarms | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/アラーム** |
| DirectoryDcim | **_PRIVATE @ NO__T-2EXTERNAL @ NO__T-3STORAGE_/DCIM** |
| DirectoryDownloads | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/Download** |
| DirectoryDocuments | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/Documents** |
| DirectoryMovies | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/映画** |
| DirectoryMusic | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/Music** |
| DirectoryNotifications | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/notification** |
| DirectoryPodcasts | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/ポッドキャスト** |
| DirectoryRingtones 音 | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/着信音** |
| DirectoryPictures | **_PRIVATE @ no__t-2EXTERNAL @ no__t-3STORAGE_/Pictures** |

複数の外部ストレージパーティションを持つデバイスの場合、各パーティションには、プライベートファイル用のディレクトリがあります。 メソッド `Android.Content.Context.GetExternalFilesDirs(string type)` は、`Java.IO.Files` の配列を返します。 各オブジェクトは、アプリケーションが所有するファイルを配置できるすべての共有/外部記憶装置上の、アプリケーション固有のプライベートディレクトリを表します。

> [!IMPORTANT]
> プライベート外部ストレージディレクトリへの正確なパスは、デバイスとデバイス、Android のバージョンによって異なります。 このため、アプリする必要がありますハード コード、このディレクトリのパスを代わりに API を使用して、Xamarin.Android など`Android.Content.Context.GetExternalFilesDir()`します。

### <a name="public-external-files"></a>パブリック外部ファイル

パブリックファイルとは、Android がプライベートファイル用に割り当てたディレクトリに格納されていない外部ストレージ上に存在するファイルのことです。 アプリをアンインストールしても、パブリックファイルは削除されません。 Android アプリでパブリックファイルの読み取りまたは書き込みを行うには、アクセス許可が付与されている必要があります。 パブリックファイルは外部ストレージ上の任意の場所に存在する可能性がありますが、規約によっては、プロパティで識別されるディレクトリ内にパブリックファイルが存在することを Android で想定しています `Android.OS.Environment.ExternalStorageDirectory`。 このプロパティは、プライマリ外部ストレージディレクトリを表す @no__t 0 のオブジェクトを返します。 例として、`Android.OS.Environment.ExternalStorageDirectory` は次のディレクトリを参照する場合があります。

```bash
/storage/emulated/0/
```

このドキュメントでは、外部ストレージ上のパブリックファイルのストレージディレクトリを、 _public @ no__t-1EXTERNAL @ no__t-2storage_と呼びます。

Android では、 _PUBLIC @ no__t-1EXTERNAL @ no__t-2STORAGE_のアプリケーションディレクトリの概念もサポートされています。 これらのディレクトリは `_PRIVATE\_EXTERNAL\_STORAGE_` のアプリケーションディレクトリとまったく同じであり、前のセクションの表で説明されています。 メソッド `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` は、パブリックアプリケーションディレクトリに対応する @no__t 1 オブジェクトを返します。 @No__t-0 パラメーターは必須パラメーターであり、`null` にすることはできません。

たとえば、`Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` を呼び出すと、次のような文字列が返されます。

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> パブリック外部ストレージディレクトリへの正確なパスは、デバイスとデバイス、Android のバージョンによって異なります。 このため、アプリする必要がありますハード コード、このディレクトリのパスを代わりに API を使用して、Xamarin.Android など`Android.OS.Environment.ExternalStorageDirectory`します。

## <a name="working-with-external-storage"></a>外部ストレージの操作

Xamarin.Android アプリがファイルへの完全パスを取得した後の作成、読み取り、書き込み、またはファイルを削除する標準の .NET API のいずれかを利用する必要があります。 これにより、アプリのクロスプラットフォームと互換性のあるコードの量が最大化されます。 ただし、ファイルへのアクセスを試みる前に、Xamarin Android アプリでそのファイルにアクセスできることを確認する必要があります。

1. **外部ストレージ**の &ndash; 外部ストレージの性質によっては、アプリによってマウントされて使用できなくなる可能性があります。 すべてのアプリは、使用を試みる前に外部ストレージの状態を確認する必要があります。
2. 実行**時のアクセス許可のチェックを実行**&ndash; Android アプリは、外部ストレージにアクセスするためにユーザーにアクセス許可を要求する必要があります。 これは、ファイルアクセスの前に実行時のアクセス許可要求を実行する必要があることを意味します。 Android のアクセス許可の詳細については、「 [Xamarin. Android の](~/android/app-fundamentals/permissions.md)ガイド」を参照してください。

これらの2つのタスクについては、以下で説明します。

### <a name="verifying-that-external-storage-is-available"></a>外部ストレージが使用可能であることを確認しています

外部ストレージに書き込む前の最初の手順は、読み取り可能または書き込み可能であることを確認することです。 @No__t-0 プロパティは、外部ストレージの状態を識別する文字列を保持します。 このプロパティは、状態を表す文字列を返します。 次の表は、`Environment.ExternalStorageState` で返される可能性がある @no__t 0 の値の一覧です。

| ExternalStorageState | 説明  |
|----------------------|---|
| MediaBadRemoval      | メディアは、正しくマウント解除されずに突然削除されました。 |
| MediaChecking        | メディアは存在しますが、ディスクチェックが行われています。  |
| MediaEjecting        | メディアがマウント解除され、取り出されています。  |
| MediaMounted         | メディアはマウントされていて、読み取りまたは書き込みが可能です。  |
| MediaMountedReadOnly | メディアはマウントされていますが、読み取ることができるのはのみです。 |
| MediaNofs            | メディアは存在しますが、Android に適したファイルシステムが含まれていません。 |
| メディアが削除されました         | メディアが存在しません。 |
| Mediash          | メディアは存在しますが、マウントされていません。 USB 経由で他のデバイスと共有されています。|
| メディア不明         | メディアの状態が Android で認識されません。 |
| MediaUnmountable     | メディアは存在しますが、Android ではマウントできません。 |
| メディアマウント解除       | メディアは存在しますが、マウントされていません。 |

ほとんどの Android アプリでは、外部ストレージがマウントされているかどうかのみを確認する必要があります。 次のコードスニペットは、外部ストレージが読み取り専用アクセスまたは読み取り/書き込みアクセス用にマウントされていることを確認する方法を示しています。

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>外部ストレージのアクセス許可

Android では、外部ストレージへのアクセスを_危険なアクセス許可_と見なします。通常、リソースへのアクセス許可をユーザーに付与する必要があります。 ユーザーは、いつでもこのアクセス許可を取り消すことができます。  これは、ファイルアクセスの前に実行時のアクセス許可要求を実行する必要があることを意味します。 アプリには、独自のプライベートファイルの読み取りと書き込みを行うためのアクセス許可が自動的に付与されます。 アプリは、ユーザーによって[アクセス許可が付与](~/android/app-fundamentals/permissions.md)された後、他のアプリに属するプライベートファイルの読み取りと書き込みを行うことができます。

すべての Android アプリは、 **Androidmanifest .xml**の外部ストレージに対する2つのアクセス許可のいずれかを宣言する必要があります。 アクセス許可を特定するには、次の2つの @no__t 0 要素のいずれかを**Androidmanifest**に追加する必要があります。

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> ユーザーが @no__t 0 を付与した場合、`READ_EXTERNAL_STORAGE` も暗黙的に付与されます。 **Androidmanifest .xml**で両方のアクセス許可を要求する必要はありません。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

アクセス許可は、**ソリューションのプロパティ**の **[Android マニフェスト]** タブを使用して追加することもできます。

![ソリューションエクスプローラー-Visual Studio に必要なアクセス許可](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

アクセス許可は、[**ソリューションのプロパティ] パッド**の **[Android マニフェスト]** タブを使用して追加することもできます。

[![Solution Pad-Visual Studio for Mac に必要なアクセス許可](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

一般に、すべての危険なアクセス許可は、ユーザーが承認する必要があります。 外部ストレージのアクセス許可は、アプリが実行されている Android のバージョンによっては、この規則に例外があるという点で異常です。

![外部ストレージのアクセス許可チェックのフローチャート](./images/external-permission-check-flowchart.png)

ランタイムアクセス許可要求の実行の詳細については、「 [Xamarin Android のアクセス許可](~/android/app-fundamentals/permissions.md)」を参照してください。 また、**モノサンプル**の[localfiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)は、実行時のアクセス許可のチェックを実行する1つの方法も示しています。

#### <a name="granting-and-revoking-permissions-with-adb"></a>ADB を使用したアクセス許可の付与と取り消し

Android アプリを開発する過程で、実行時のアクセス許可のチェックに関係するさまざまな作業フローをテストするためのアクセス許可の付与と取り消しが必要になる場合があります。 この操作は、コマンドプロンプトで ADB を使用して行うことができます。 次のコマンドラインスニペットは、パッケージ名が " **com. companyname. アプリ**" である Android アプリの ADB を使用してアクセス許可を付与または取り消す方法を示しています。

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>ファイルの削除

など、外部のストレージからファイルを削除する (C#) API を使用できる標準的な[ `System.IO.File.Delete`](xref:System.IO.File.Delete*)します。 コードの移植性を犠牲 Java API を使用することもできます。 以下に例を示します。

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>関連リンク

* [モノの Xamarin のローカルファイルのサンプル **-サンプル**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin. Android のアクセス許可](~/android/app-fundamentals/permissions.md)
