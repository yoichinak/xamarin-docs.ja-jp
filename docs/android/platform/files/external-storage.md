---
title: Xamarin.Android での外部ストレージにファイル アクセス
description: このガイドは Xamarin.Android での外部ストレージにファイルへのアクセスについて説明します
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 78051fce44239eea86948988a4d19ac37c5ea0d5
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854900"
---
# <a name="external-storage"></a>外部ストレージ

外部ストレージは、内部ストレージではなく、ファイルを担当するアプリにのみアクセス可能なファイル ストレージを参照します。 外部ストレージの主な目的では、ファイルは、アプリ間で共有するためのものをや内部の記憶域に収まらないファイルを配置する場所を提供します。

SD カードなどのリムーバブル メディア上のディスク パーティションをこれまでは、外部のストレージと呼ばれます (と呼ばれていた_ポータブル ストレージ_)。 この区別は Android デバイスが進化し、多くの Android デバイスがリムーバブル記憶域をサポートしなくほどには関連はありません。 代わりに一部のデバイスが割り当て、内部の非揮発性メモリの一部と同じ関数のリムーバブル メディアを実行するには、どの Android られます。 呼ばれます_エミュレート_記憶域および外部記憶域は依然と見なされます。 代わりに、一部の Android デバイスには、複数の外部ストレージのパーティションがあります。 たとえば、Android タブレット (に加えて、その内部ストレージ) ストレージがエミュレートされた可能性がありますと SD カードの 1 つまたは複数のスロット。 これらのパーティションのすべては Android で外部のストレージとして扱われます。

複数のユーザーがデバイスでは、各ユーザーを外部ストレージの外部ストレージのプライマリ パーティション上の専用ディレクトリとなります。 1 人のユーザーとして実行されているアプリはありませんアクセスのファイルに、デバイス上の別のユーザーから。 すべてのユーザーのファイルは、まだ世界が判読できると世界の書き込み可能です。ただし、Android は、サンド ボックスを他のユーザーからは、各ユーザー プロファイル。

他の .NET アプリケーションには、読み取りと書き込みをファイルは Xamarin.Android でとほぼ同じです。 Xamarin.Android アプリでは、し、使用して標準的な .NET の表現方法ファイル アクセス、ファイル操作されるへのパスを決定します。 内部および外部の記憶域への実際のパスがデバイスからデバイスに異なる場合がありますので、または Android のバージョンの Android バージョンから、これは推奨されませんハード コードするファイルへのパス。 代わりに、Xamarin.Android を内部および外部の記憶域上のファイルへのパスを判断できるように、ネイティブの Android API を公開します。

このガイドは、概念と Android で外部ストレージに固有の API について説明します。

## <a name="public-and-private-files-on-external-storage"></a>外部ストレージにファイルをパブリックおよびプライベート

2 つの異なる種類のアプリは可能性がありますの外部ストレージに保持するファイルがあります。

* **プライベート**ファイル&ndash;プライベート ファイルは、アプリケーション (ただしはまだ世界が判読できる、書き込める) に固有のファイルです。 Android では特定のディレクトリの外部ストレージにプライベート ファイルが保存される必要があります。 ファイルは"private"と呼ばれるも表示され、デバイス上の他のアプリからアクセスできるは、それらでは不十分な特別な保護 Android で。

* **パブリック**ファイル&ndash;ファイル、アプリケーションに固有のものと見なされないは自由に共有するためのものです。

これらのファイル間の相違点は、主に概念です。 プライベート ファイルは、という意味でプライベートをパブリック ファイルの外部ストレージに存在するその他のファイルとは、アプリケーションの一部であると見なされます。 Android プライベートおよびパブリックのファイルへのパスを解決するための 2 つの API を提供しますが、これらのファイルを読み書きする同じ .NET API を使用するそれ以外の場合。 これらは、セクションに記載されているのと同じ API[読み取りと書き込み](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)します。

### <a name="private-external-files"></a>プライベートの外部ファイル

プライベートの外部ファイルは、アプリケーション (内部のファイルに似ています) に固有のものと見なされますが、いくつかの理由 (される内部記憶域に対して大きすぎます) などの外部ストレージに保持されています。 内部のファイルと同様に、これらのファイルは削除されます、ユーザーが、アプリがアンインストールされます。

メソッドを呼び出してプライベート外部ファイルの 1 次拠点が見つかった`Android.Content.Context.GetExternalFilesDir(string type)`します。 このメソッドは、`Java.IO.File`アプリのプライベート外部ストレージ ディレクトリを表すオブジェクト。 渡す`null`このメソッドを返すパスをアプリケーションのユーザーのストレージ ディレクトリ。 たとえば、パッケージ名を持つアプリケーションの`com.companyname.app`、プライベートの外部のファイルの"root"ディレクトリになります。

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

このドキュメントとしての外部ストレージにファイルをプライベート ストレージ ディレクトリを参照してください_プライベート\_外部\_ストレージ_します。


パラメーターを`GetExternalFilesDir()`を指定する文字列、_アプリケーション ディレクトリ_します。 これは、ディレクトリのファイルの論理の組織の標準的な場所を指定するためのものです。 文字列値が定数でご利用ください、`Android.OS.Environment`クラス。

| `Android.OS.Environment` | ディレクトリ |
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

デバイスの外部記憶域の複数のパーティションがある場合、各パーティションは、プライベート ファイルが想定されているディレクトリがあります。 メソッド`Android.Content.Context.GetExternalFilesDirs(string type)`の配列を返す`Java.IO.Files`します。 各オブジェクトはプライベート アプリケーション固有のディレクトリを表す、所有するすべての共有/外部ストレージ デバイスをアプリケーションがファイルを配置できます。

> [!IMPORTANT]
> プライベート exteral ストレージ ディレクトリへの正確なパスは、デバイスから、デバイスおよび Android のバージョンとの間に異なります。 このため、アプリする必要がありますハード コード、このディレクトリのパスを代わりに API を使用して、Xamarin.Android など`Android.Content.Context.GetExternalFilesDir()`します。

### <a name="public-external-files"></a>パブリックの外部ファイル

パブリックのファイルは、Android を割り当てるプライベート ファイルのディレクトリに格納されていない外部ストレージに存在するファイルです。 アプリがアンインストールされるときに、パブリックのファイルは削除されません。 すべてのパブリック ファイルを読み書きできる前ににより、android アプリに権限を許可する必要があります。 パブリック ファイルに外部のストレージに任意の場所に存在することはできますが、Android 慣例には、プロパティで識別されたディレクトリに存在するファイルをパブリックが必要ですが`Android.OS.Environment.ExternalStorageDirectory`します。 このプロパティは、`Java.IO.File`外部ストレージのプライマリ ディレクトリを表すオブジェクト。 たとえば、`Android.OS.Environment.ExternalStorageDirectory`が次のディレクトリを参照してください。

```bash
/storage/emulated/0/
```

このドキュメントは外部のストレージとしてのパブリック ファイルのストレージ ディレクトリを参照してください_パブリック\_外部\_ストレージ_します。


Android 上のアプリケーション ディレクトリの概念もサポートされます_パブリック\_外部\_ストレージ_します。 これらのディレクトリは、アプリケーション ディレクトリと同じではまったく`_PRIVATE\_EXTERNAL\_STORAGE_`と前のセクションの表で説明します。 メソッド`Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)`が返されます、`Java.IO.File`パブリック アプリケーション ディレクトリに対応するオブジェクト。 `directoryType`パラメーターは必須パラメーターでありすることはできません`null`します。

たとえば、呼び出し`Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath`のようになります文字列が返されます。

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> パブリックの外部ストレージ ディレクトリの正確なパスは、デバイスから、デバイスおよび Android のバージョンとの間に異なります。 このため、アプリする必要がありますハード コード、このディレクトリのパスを代わりに API を使用して、Xamarin.Android など`Android.OS.Environment.ExternalStorageDirectory`します。

## <a name="working-with-external-storage"></a>外部ストレージの使用

Xamarin.Android アプリがファイルへの完全パスを取得した後の作成、読み取り、書き込み、またはファイルを削除する標準の .NET API のいずれかを利用する必要があります。 これには、クロス プラットフォーム アプリの互換性のあるコードの量が最大化します。 ただし、ファイルへのアクセスを試みる前に、Xamarin.Android アプリはそのファイルにアクセスすることを確認する必要があります。

1. **記憶域を外部確認** &ndash; 、外部の記憶域の性質に応じてその可能性がありますいないマウントを可能であると、アプリで使用可能なが。 すべてのアプリは、それを使用する前に、外部の記憶域の状態を確認する必要があります。
2. **ランタイムのアクセス許可チェックを実行して** &ndash; An Android アプリは外部ストレージにアクセスするために、ユーザーからのアクセス許可を要求する必要があります。 これにより、実行時のファイルにアクセスする前に、アクセス許可要求を実行する必要があります。 このガイド[アクセス許可で Xamarin.Android](~/android/app-fundamentals/permissions.md)の詳細については、Android のアクセス許可が含まれています。

これら 2 つのタスクについては、以下の説明します。

### <a name="verifying-that-external-storage-is-available"></a>外部ストレージの使用可能な可能性の確認

外部ストレージに書き込む前に、最初の手順では、読み取り可能または書き込み可能なことを確認します。 `Android.OS.Environment.ExternalStorageState`プロパティは、外部の記憶域の状態を識別する文字列を保持します。 このプロパティでは、状態を表す文字列を返します。 このテーブルは、の一覧、`ExternalStorageState`によって返される値`Environment.ExternalStorageState`:

| ExternalStorageState | 説明  |
|----------------------|---|
| MediaBadRemoval      | メディアが正しくマウント解除せず突然削除されました。 |
| MediaChecking        | メディアが存在するが、中であるため、ディスクを確認します。  |
| MediaEjecting        | メディアは、マウント解除し、排出される予定です。  |
| MediaMounted         | メディアがマウントされていると、読み取りまたは書き込みをすることができます。  |
| MediaMountedReadOnly | マウントされている、メディアがからのみ読み取ることができます。 |
| MediaNofs            | メディアが存在するが、Android に適したファイルシステムが含まれていません。 |
| MediaRemoved         | メディアが存在することはありません。 |
| MediaShared          | メディアはあるが、マウントされていません。 別のデバイスを USB 経由で共有されています。|
| MediaUnknown         | メディアの状態は、Android によって認識されません。 |
| MediaUnmountable     | メディアが存在するが、Android でマウントすることはできません。 |
| MediaUnmounted       | メディアが存在するが、マウントされていません。 |


ほとんどの Android アプリは、外部ストレージがマウントされているかを確認するだけです。 次のコード スニペットでは、外部ストレージが読み取り専用アクセスまたは読み取り/書き込みアクセスでマウントされていることを確認する方法を示します。

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>外部ストレージのアクセス許可

Android を考慮する外部ストレージへのアクセス、_危険なアクセス許可_、通常、ユーザーへのリソースのアクセス許可を付与する必要があります。 ユーザーは、いつでもこのアクセス許可を取り消すことができます。  これにより、実行時のファイルにアクセスする前に、アクセス許可要求を実行する必要があります。 アプリは、独自のプライベート ファイルを読み書きするアクセス許可を自動的に付与されます。 アプリの後に他のアプリに属しているプライベート ファイルを読み書きする可能性があります[アクセス許可を付与](~/android/app-fundamentals/permissions.md)ユーザー。

すべての Android アプリは、外部の記憶域の 2 つの権限のいずれかを宣言する必要があります、 **AndroidManifest.xml**します。 次の 2 つのいずれかのアクセス許可を識別するために`uses-permission`に要素を追加する必要があります**AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> ユーザーによって付与される場合`WRITE_EXTERNAL_STORAGE`、し`READ_EXTERNAL_STORAGE`も暗黙的に与えられます。 両方のアクセス許可を要求する必要はありません**AndroidManifest.xml**します。

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

使用して、アクセス許可を追加することがありますも、 **Android マニフェスト**のタブ、**ソリューションのプロパティ**:

![ソリューション エクスプ ローラー - Visual Studio の必要なアクセス許可](./images/required-permissions.w157.png)

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

使用して、アクセス許可を追加することがありますも、 **Android マニフェスト**のタブ、**ソリューション プロパティ パッド**:

[![Sソリューション パッド - Visual Studio for Mac の必要なアクセス許可](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

一般に、ユーザーがすべて危険なアクセス許可を承認する必要があります。 外部ストレージのアクセス許可は、アプリが実行されている Android のバージョンに応じて、この規則の例外が、異常は。

![外部ストレージのアクセス許可のフローチャートを確認します](./images/external-permission-check-flowchart.png)

ランタイムのアクセス許可要求を実行する方法の詳細については、このガイドを参照してください[アクセス許可で Xamarin.Android](~/android/app-fundamentals/permissions.md)します。 **Monodroid サンプル** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)のランタイム アクセス許可チェックを実行する方法も示します。

#### <a name="granting-and-revoking-permissions-with-adb"></a>許可および ADB を使用した権限の取り消し

Android アプリを作成する過程を与えるし、アクセス許可のチェックをランタイムに関連するさまざまな作業のフローをテストするアクセス許可の取り消しに必要な場合があります。 ADB を使用してコマンド プロンプトでこれを行うことができます。 次のコマンド ライン スニペットまたは ADB を使用して、パッケージ名を持つ Android アプリのアクセス許可を取り消す方法をデモンストレーションする**com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>ファイルを削除します。

など、外部のストレージからファイルを削除する (C#) API を使用できる標準的な[ `System.IO.File.Delete`](xref:System.IO.File.Delete*)します。 コードの移植性を犠牲 Java API を使用することもできます。 例:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>関連リンク

* [ローカル ファイルの Xamarin.Android サンプル**monodroid サンプル**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin.Android でのアクセス許可](~/android/app-fundamentals/permissions.md)
