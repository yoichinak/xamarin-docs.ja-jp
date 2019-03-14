---
title: Workbooks のインストールと要件
description: このドキュメントでは、ダウンロードして、サポートされているプラットフォームとシステム要件について説明する、Xamarin Workbooks をインストールする方法について説明します。
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: b1303f21225d3ae7b7d3a796e4845afbfe554a22
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667712"
---
# <a name="workbooks-installation-and-requirements"></a>Workbooks のインストールと要件

<a name="install" />

## <a name="download-and-install"></a>ダウンロードとインストール

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. チェック、[要件](#requirements)以下。
2. ダウンロードしてインストール[ブックの Windows に Xamarin](https://dl.xamarin.com/interactive/XamarinInteractive.msi)します。
3. 開始[いろいろ](~/tools/workbooks/workbook.md)ブックまたは試す、[サンプル](https://developer.xamarin.com/workbooks)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. チェック、[要件](#Requirements)以下。
2. ダウンロードしてインストール[for Mac の Xamarin Workbooks](https://dl.xamarin.com/interactive/XamarinInteractive.pkg)します。
3. 開始[いろいろ](~/tools/workbooks/workbook.md)ブックまたは試す、[サンプル](https://developer.xamarin.com/workbooks)

-----

## <a name="requirements"></a>必要条件

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 以降
- **Windows** -Windows 7 以上 (Internet Explorer 11 以降と .NET 4.6.1 以降)

#### <a name="supported-app-platforms"></a>サポートされているアプリのプラットフォーム

|アプリ プラットフォーム|OS のサポート|メモ|
|--- |--- |--- |
|Mac|Mac でのみサポートされます。|
|iOS|Mac および Windows でサポートされています|Mac は、Xamarin.iOS 11.0 と Xcode 9.0 以上をインストールする必要があります。 Windows で iOS ブックを実行するには、Mac ビルド ホスト上のすべての操作を実行している必要があります、[リモートの iOS シミュレーター](~/tools/ios-simulator/index.md) Windows にインストールされています。|
|Android|Mac および Windows でサポートされています|仮想デバイスで Google、Visual Studio または Xamarin Android のエミュレーターを使用する必要があります > = 5.0|
|WPF|Windows でのみサポートされます。|
|コンソール (.NET Framework)|Mac および Windows でサポートされています|
|コンソール (.NET Core)|Mac および Windows でサポートされています|


## <a name="reporting-bugs"></a>バグの報告

ください[GitHub の問題の報告][bugs]、すべて、次の情報が含まれて。

### <a name="log-files"></a>ログ ファイル

ブックのクライアント ログ ファイルを常に添付するには。

- Mac の場合: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x では、メイン メニューの検索 (macOS) またはエクスプ ローラー (Windows) 直接ログ ファイルを選択する機能も機能します。

- **ヘルプ > ログ ファイルの表示**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>ブックの 1.3 以降のログのパス:

- Mac の場合: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

オペレーティング システムの詳細を知る非常に便利ですし、Xamarin 製品をインストールします。

ブックのメイン メニューでは。

* **ヘルプ > バージョン情報のコピー**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>ブックの 1.3 以降の手順:

Visual Studio For Mac

- **Visual Studio > Visual Studio のバージョン情報 > 詳細を表示 > 情報のコピー**
- バグ レポートに貼り付けます

Visual Studio

- **ヘルプ > Visual Studio のバージョン情報 > 情報のコピー**
- オペレーティング システムのバージョンと 32 ビットまたは 64 ビットの Windows を実行しているかどうかを通知しています。

### <a name="samples"></a>サンプル

アタッチするか、またはリンクする場合、 **.workbooks**バグをより迅速に解決に役立つ可能性があるファイルが使用すると、問題があります。

### <a name="devices"></a>デバイス

IOS または Android のブックの接続に問題があるため、既にオンになっている場合[、トラブルシューティングのページ](~/tools/workbooks/troubleshooting/index.md)を把握する必要があります。

- 接続しようとしているデバイスの名前
- デバイスの OS バージョン
- Android:X86 を使用していることを確認しますエミュレーター。
- Android:どのようなエミュレーターのプラットフォームを使用していますか。 Google エミュレーターでしょうか。
  Visual Studio Android Emulator でしょうか。 Xamarin Android Player でしょうか。
- Windows で iOS の場合:Xamarin リモート iOS シミュレーターのバージョンはインストールした (確認 **プログラムの追加/削除** で **コントロール パネルの **) でしょうか。
- Windows で iOS の場合:Mac ビルド ホストのプラットフォームのバージョン情報のも提供してください。
- デバイスにはネットワーク接続 (web ブラウザーを使用してチェック) がありますか。

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>[アンインストール]

### <a name="windows"></a>Windows

ブックを取得する方法によっては、2 つのアンインストール手順を実行する必要があります。 これらの両方をソフトウェアを完全にアンインストールを確認してください。

#### <a name="visual-studio-installer"></a>Visual Studio インストーラー

Visual Studio 2017 がある場合は開く**Visual Studio インストーラー**、ファイルの場所と**個々 のコンポーネント**の**Xamarin Workbooks**します。 オンの場合、それをオフにし、順にクリックします**変更**をアンインストールします。

#### <a name="system-uninstall"></a>システムのアンインストール

使用してアンインストールする必要がありますをインストールした場合のブック自分でダウンロードしたインストーラー、**アプリと機能**またはを使用して Windows 10 でのシステム設定 ページ**プログラムの追加/削除**コントロール以前のバージョンの Windows パネル。

> **開始 > 設定 > システム > アプリと機能**

![](install-images/windows-remove.png "Xamarin Workbooks に記載されている&quot;アプリ&amp;機能&quot;")

**引き続き、ブックは、知識がなくても再インストール取得されないことを確認する Visual Studio インストーラーの手順に従う必要があります。**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

以降で[1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/)、Xamarin Workbooks を実行して、ターミナルからアンインストールできます。

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

アンインストーラーは、ファイルとディレクトリが削除され続行する前に確認を要求について詳しく説明します。

渡す、`-help`への引数、`uninstall`より高度なシナリオ用のスクリプト。

古いバージョンでは、次のものを手動で削除する必要があります。

1. `"/Applications/Xamarin Workbooks.app"` の Workbooks アプリを削除します
2. `"Applications/Xamarin Inspector.app"` の Inspector アプリを削除します
2. アドイン `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` と `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"` を削除します
3. `/Library/Frameworks/Xamarin.Interactive.framework` および `/Library/Frameworks/Xamarin.Inspector.framework` にある Inspector のファイルとサポート ファイルを削除します

## <a name="downgrading"></a>ダウン グレード

用のバンドル識別子**アプリケーション/Xamarin Workbooks.app**から変更`com.xamarin.Inspector`に`com.xamarin.Workbooks`1.4 のリリースでは、Workbooks と Inspector とは完全に分割します。

古いインストーラーでのバグにより、1.3.2 または古いインストーラーを使用して 1.4 以降のリリースをダウン グレードすることはできません。

1.4 または 1.3.2 に新しいまたは古いからダウン グレードします。

1. [Workbooks と Inspector を手動でアンインストールします。](#uninstall-macos)
2. 実行、1.3.2 以前`.pkg`インストーラー
