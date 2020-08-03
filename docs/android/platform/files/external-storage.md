---
title: Xamarin.Android における外部ストレージでのファイル アクセス
description: このガイドでは、Xamarin.Android における外部ストレージでのファイル アクセスについて説明します
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: e6eb62def5aeb9e4a4a347becffcae82116c1b11
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997073"
---
# <a name="external-storage"></a>外部ストレージ

外部ストレージとは、内部ストレージ上になく、ファイルの処理に使用されるアプリ以外でもファイルにアクセスできる、ファイル ストレージのことです。 外部ストレージの主な目的は、アプリ間で共有されるファイルや、大きすぎて内部ストレージに収まらないファイルを配置する場所を提供することです。

これまで、外部ストレージとは、SD カードなどのリムーバブル メディア上のディスク パーティションのことでした ("_ポータブル ストレージ_" とも呼ばれていました)。 Android デバイスが進化し、多くの Android デバイスではリムーバブル ストレージがサポートされなくなったため、この区別は意味がなくなっています。 代わりに、一部のデバイスでは、リムーバブル メディアと同じ機能を実行するために、内部の非揮発性メモリの一部が割り当てられます。 これは "_エミュレートされた_" ストレージと呼ばれ、やはり外部ストレージと見なされます。 または、一部の Android デバイスでは、複数の外部ストレージ パーティションが使用される場合もあります。 たとえば、Android タブレットは (内部ストレージに加えて) エミュレートされたストレージと SD カード用の 1 つ以上のスロットを備えていることがあります。 これらのパーティションはすべて、Android によって外部ストレージとして扱われます。

複数のユーザーが使用するデバイスでは、外部ストレージ用のプライマリ外部ストレージ パーティション上に、ユーザーごとの専用ディレクトリがあります。 1 人のユーザーとして実行されるアプリでは、デバイス上の別のユーザーのファイルにアクセスできません。 すべてのユーザー用のファイルは、誰でも読み取ったり書き込んだりできます。ただし、Android では、ユーザー プロファイルは個別にサンドボックス化されます。

ファイルの読み取りと書き込みは、Xamarin.Android でも他の .NET アプリケーションとほぼ同じです。 Xamarin.Android アプリでは、操作するファイルへのパスが決定された後、ファイル アクセスに標準の .NET イディオムが使用されます。 内部ストレージと外部ストレージへの実際のパスは、デバイスによって、または Android のバージョンによって異なる場合があるため、ファイルへのパスをハード コーディングしないことをお勧めします。 代わりに、Xamarin.Android では、内部ストレージと外部ストレージのファイルへのパスを決定するのに役立つ、ネイティブの Android API が公開されています。

このガイドでは、Android での外部ストレージに固有の概念と API について説明します。

## <a name="public-and-private-files-on-external-storage"></a>外部ストレージ上のパブリック ファイルとプライベート ファイル

アプリで外部ストレージに保持できるファイルには、次の 2 種類があります。

* **プライベート** ファイル &ndash; プライベート ファイルは、アプリケーションに固有のファイルです (ただし、誰でも読み書きできます)。 Android では、プライベート ファイルは外部ストレージの特定のディレクトリに格納されていることが想定されています。 ファイルは "プライベート" と呼ばれていますが、デバイス上の他のアプリから表示したりアクセスしたりでき、Android によって特別な保護を受けることはできません。

* **パブリック** ファイル &ndash; これらのファイルは、アプリケーションに固有のものとは見なされず、自由に共有できます。

これらのファイルの違いは、主に概念的なものです。 プライベート ファイルはアプリケーションの一部と見なされるという意味でプライベートであるのに対し、パブリック ファイルは外部ストレージに存在する他のすべてのファイルです。 Android には、プライベート ファイルとパブリック ファイルへのパスを解決するための 2 つの異なる API が用意されていますが、それ以外については、同じ .NET API がこれらのファイルの読み取りと書き込みに使用されます。 これらは、[読み取りと書き込み](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)に関するセクションで説明されているものと同じ API です。

### <a name="private-external-files"></a>プライベート外部ファイル

プライベート外部ファイルは (内部ファイルと同じように) アプリケーションに固有であると見なされますが、いくつかの理由で外部ストレージに保持されています (内部ストレージには大きすぎるなど)。 内部ファイルと同様に、これらのファイルはユーザーがアプリをアンインストールすると削除されます。

プライベート外部ファイルのプライマリの場所は、`Android.Content.Context.GetExternalFilesDir(string type)` メソッドを呼び出すことによって見つかります。 このメソッドでは、アプリのプライベート外部ストレージ ディレクトリを表す `Java.IO.File` オブジェクトが返されます。 このメソッドに `null` を渡すと、アプリケーションに対するユーザーのストレージ ディレクトリへのパスが返されます。 たとえば、パッケージ名が `com.companyname.app` のアプリケーションでは、プライベート外部ファイルの "ルート" ディレクトリは次のようになります。

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

このドキュメントでは、外部ストレージ上のプライベート ファイルのストレージ ディレクトリを _PRIVATE\_EXTERNAL\_STORAGE_ と示します。

`GetExternalFilesDir()` に対するパラメーターは、"_アプリケーション ディレクトリ_" を指定する文字列です。 これは、ファイルの論理編成に対する標準的な場所を提供するためのディレクトリです。 この文字列値は、`Android.OS.Environment` クラスの定数を通じて使用できます。

| Android.OS.Environment | ディレクトリ |
|-|-|
| DirectoryAlarms | **_PRIVATE\_EXTERNAL\_STORAGE_/Alarms** |
| DirectoryDcim | **_PRIVATE\_EXTERNAL\_STORAGE_/DCIM** |
| DirectoryDownloads | **_PRIVATE\_EXTERNAL\_STORAGE_/Download** |
| DirectoryDocuments | **_PRIVATE\_EXTERNAL\_STORAGE_/Documents** |
| DirectoryMovies | **_PRIVATE\_EXTERNAL\_STORAGE_/Movies** |
| DirectoryMusic | **_PRIVATE\_EXTERNAL\_STORAGE_/Music** |
| DirectoryNotifications | **_PRIVATE\_EXTERNAL\_STORAGE_/Notifications** |
| DirectoryPodcasts | **_PRIVATE\_EXTERNAL\_STORAGE_/Podcasts** |
| DirectoryRingtones | **_PRIVATE\_EXTERNAL\_STORAGE_/Ringtones** |
| DirectoryPictures | **_PRIVATE\_EXTERNAL\_STORAGE_/Pictures** |

複数の外部ストレージ パーティションを持つデバイスの場合、パーティションごとにプライベート ファイル用のディレクトリがあります。 `Android.Content.Context.GetExternalFilesDirs(string type)` メソッドでは、`Java.IO.Files` の配列が返されます。 各オブジェクトでは、アプリケーションが所有するファイルを配置できるすべての共有および外部ストレージ デバイス上の、アプリケーション固有のプライベート ディレクトリが表されています。

> [!IMPORTANT]
> プライベート外部ストレージ ディレクトリへの正確なパスは、デバイスにより、および Android のバージョンにより、異なる場合があります。 このため、アプリでは、このディレクトリへのパスをハード コーディングしてはならず、代わりに `Android.Content.Context.GetExternalFilesDir()` などの Xamarin.Android API を使用する必要があります。

### <a name="public-external-files"></a>パブリック外部ファイル

パブリック ファイルとは、外部ストレージ上に存在するファイルのうち、Android によってプライベート ファイル用に割り当てられたディレクトリに格納されないもののことです。 アプリをアンインストールしても、パブリック ファイルは削除されません。 Android アプリでパブリック ファイルの読み取りまたは書き込みを行うには、アプリにアクセス許可が付与されている必要があります。 パブリック ファイルは外部ストレージ上のどこにでも存在できますが、Android では、慣例により、`Android.OS.Environment.ExternalStorageDirectory` プロパティによって示されるディレクトリにパブリック ファイルが存在することが想定されています。 このプロパティでは、プライマリ外部ストレージ ディレクトリを表す `Java.IO.File` オブジェクトが返されます。 たとえば、`Android.OS.Environment.ExternalStorageDirectory` では次のようなディレクトリが参照されます。

```bash
/storage/emulated/0/
```

このドキュメントでは、外部ストレージ上のパブリック ファイルのストレージ ディレクトリを _PUBLIC\_EXTERNAL\_STORAGE_ と示します。

Android では、_PUBLIC\_EXTERNAL\_STORAGE_ 上のアプリケーション ディレクトリの概念もサポートされています。 これらのディレクトリは、`PRIVATE_EXTERNAL_STORAGE` のアプリケーション ディレクトリとまったく同じであり、前のセクションの表で説明されています。 `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` メソッドでは、パブリック アプリケーション ディレクトリに対応する `Java.IO.File` オブジェクトが返されます。 `directoryType` は必須パラメーターであり、`null` にはできません。

たとえば、`Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` を呼び出すと、次のような文字列が返されます。

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> パブリック外部ストレージ ディレクトリへの正確なパスは、デバイスにより、および Android のバージョンにより、異なる場合があります。 このため、アプリでは、このディレクトリへのパスをハード コーディングしてはならず、代わりに `Android.OS.Environment.ExternalStorageDirectory` などの Xamarin.Android API を使用する必要があります。

## <a name="working-with-external-storage"></a>外部ストレージの操作

Xamarin Android アプリでは、ファイルへの完全なパスを取得した後、ファイルの作成、読み取り、書き込み、削除には、標準的な .NET API を使用する必要があります。 これにより、アプリのプラットフォーム間で互換性のあるコードの量が最大になります。 ただし、ファイルへのアクセスを試みる前に、Xamarin Android アプリでそのファイルにアクセスできることを確認する必要があります。

1. **外部ストレージを検証する** &ndash; 外部ストレージの性質によっては、アプリでマウントして使用できない可能性があります。 すべてのアプリでは、使用を試みる前に、外部ストレージの状態を確認する必要があります。
2. **実行時のアクセス許可のチェックを実行する** &ndash; Android アプリでは、外部ストレージにアクセスするためのアクセス許可をユーザーに要求する必要があります。 これは、ファイル アクセスの前に、実行時のアクセス許可要求を実行する必要があることを意味します。 Android のアクセス許可の詳細については、「[Xamarin.Android のアクセス許可](~/android/app-fundamentals/permissions.md)」を参照してください。

これらの 2 つのタスクについては、以下で説明します。

### <a name="verifying-that-external-storage-is-available"></a>外部ストレージが使用可能であることを確認する

外部ストレージに書き込む前の最初のステップは、読み取りまたは書き込みが可能であることを確認することです。 `Android.OS.Environment.ExternalStorageState` プロパティには、外部ストレージの状態を示す文字列が保持されています。 このプロパティからは、状態を表す文字列が返されます。 次の表は、`Environment.ExternalStorageState` によって返される可能性がある `ExternalStorageState` の値の一覧です。

| ExternalStorageState | 説明  |
|----------------------|---|
| MediaBadRemoval      | メディアは、適切にマウント解除されずに突然削除されました。 |
| MediaChecking        | メディアは存在しますが、ディスク チェックが行われています。  |
| MediaEjecting        | メディアがマウント解除と排出の処理中です。  |
| MediaMounted         | メディアはマウントされており、読み取りまたは書き込みが可能です。  |
| MediaMountedReadOnly | メディアはマウントされていますが、読み取りだけが可能です。 |
| MediaNofs            | メディアは存在しますが、Android に適したファイル システムが含まれていません。 |
| MediaRemoved         | メディアは存在しません。 |
| MediaShared          | メディアは存在しますが、マウントされていません。 USB 経由で別のデバイスと共有されています。|
| MediaUnknown         | メディアは Android で認識できない状態です。 |
| MediaUnmountable     | メディアは存在しますが、Android ではマウントできません。 |
| MediaUnmounted       | メディアは存在しますが、マウントされていません。 |

ほとんどの Android アプリでは、外部ストレージがマウントされているかどうかの確認だけが必要です。 次のコード スニペットでは、外部ストレージが読み取り専用アクセスまたは読み書きアクセス用にマウントされていることを確認する方法を示します。

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>外部ストレージのアクセス許可

Android では、外部ストレージへのアクセスは "_危険なアクセス許可_" と見なされ、通常は、ユーザーがリソースにアクセスするためのアクセス許可を付与する必要があります。 ユーザーは、いつでもこのアクセス許可を取り消すことができます。  これは、ファイル アクセスの前に、実行時のアクセス許可要求を実行する必要があることを意味します。 アプリには、独自のプライベート ファイルの読み取りと書き込みを行うためのアクセス許可が自動的に付与されます。 アプリでは、ユーザーによって[アクセス許可を付与された](~/android/app-fundamentals/permissions.md)後で、他のアプリに属しているプライベート ファイルの読み取りと書き込みを行うことができます。

すべての Android アプリでは、外部ストレージに対する 2 つのアクセス許可のいずれかを、**AndroidManifest.xml** で宣言する必要があります。 アクセス許可を特定するには、次の 2 つの `uses-permission` 要素のいずれかを、**AndroidManifest.xml** に追加する必要があります。

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> ユーザーが `WRITE_EXTERNAL_STORAGE` を許可した場合、`READ_EXTERNAL_STORAGE` も暗黙的に付与されます。 **AndroidManifest.xml** で両方のアクセス許可を要求する必要はありません。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

アクセス許可は、**ソリューションのプロパティ**の **[Android マニフェスト]** タブを使用して追加することもできます:

![ソリューション エクスプローラー - Visual Studio に必要なアクセス許可](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

アクセス許可は、**ソリューションのプロパティ パッド**の **[Android マニフェスト]** タブを使用して追加することもできます:

[![Solution Pad - Visual Studio for Mac に必要なアクセス許可](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

一般に、すべての危険なアクセス許可をユーザーが承認する必要があります。 外部ストレージのアクセス許可は、アプリが実行されている Android のバージョンによってはこのルールに例外があるという点で変則的です。

![外部ストレージのアクセス許可チェックのフローチャート](./images/external-permission-check-flowchart.png)

実行時のアクセス許可要求の実行について詳しくは、「[Xamarin.Android のアクセス許可](~/android/app-fundamentals/permissions.md)」を参照してください。 **monodroid-sample** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) でも、実行時にアクセス許可チェックを実行する 1 つの方法が示されています。

#### <a name="granting-and-revoking-permissions-with-adb"></a>ADB でのアクセス許可の付与と取り消し

Android アプリを開発する過程で、実行時アクセス許可チェックに関係するさまざまなワークフローをテストするため、アクセス許可の付与と取り消しが必要になる場合があります。 ADB を使用してコマンド プロンプトでこれを行うことができます。 次のコマンド ライン スニペットでは、ADB を使用して、パッケージ名が **com.companyname.app** である Android アプリに対するアクセス許可を付与または取り消す方法を示します。

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>ファイルの削除

[`System.IO.File.Delete`](xref:System.IO.File.Delete*) など、任意の標準 C# API を使用して、外部ストレージからファイルを削除できます。 コードの移植性を犠牲にすれば、Java API を使用することもできます。 次に例を示します。

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>関連リンク

* [**monodroid-samples** での Xamarin.Android ローカル ファイル サンプル](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin.Android でのアクセス許可](~/android/app-fundamentals/permissions.md)
