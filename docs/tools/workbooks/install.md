---
title: ブックのインストールと要件
description: このドキュメントでは、Xamarin Workbooks をダウンロードしてインストールする方法について説明します。サポートされるプラットフォームとシステム要件について説明します。
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: a044169f86b46abff4158011e99320c528180ffc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573886"
---
# <a name="workbooks-installation-and-requirements"></a>ブックのインストールと要件

<a name="install"></a>

## <a name="download-and-install"></a>ダウンロードおよびインストールする

<!-- markdownlint-disable MD001 -->

# <a name="windows"></a>[Windows](#tab/windows)

1. 以下の[要件](#requirements)を確認してください。
2. [Windows 用の Xamarin Workbooks を](https://dl.xamarin.com/interactive/XamarinInteractive.msi)ダウンロードしてインストールします。
3. ブックでの[再生](~/tools/workbooks/workbook.md)を開始します。

# <a name="macos"></a>[macOS](#tab/macos)

1. 以下の[要件](#requirements)を確認してください。
2. [Mac 用の Xamarin Workbooks を](https://dl.xamarin.com/interactive/XamarinInteractive.pkg)ダウンロードしてインストールします。
3. [再生](~/tools/workbooks/workbook.md)を開始します。

-----

## <a name="requirements"></a>必要条件

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 以上
- **Windows** -windows 7 以降 (Internet Explorer 11 以降および .net 4.6.1 以上)

#### <a name="supported-app-platforms"></a>サポートされているアプリプラットフォーム

|アプリ プラットフォーム|OS のサポート|メモ|
|--- |--- |--- |
|Mac|Mac でのみサポートされています|
|iOS|Mac および Windows でサポートされています|Mac には、Xamarin iOS 11.0 と Xcode 9.0 以降をインストールする必要があります。 Windows で iOS ブックを実行するには、上記のすべてを実行している Mac ビルドホストと、Windows にインストールされている[リモート Ios シミュレーター](~/tools/ios-simulator/index.md)が必要です。|
|Android|Mac および Windows でサポートされています|仮想デバイス >= 5.0 を使用して、Google、Visual Studio、または Xamarin Android エミュレーターを使用する必要があります|
|WPF|Windows でのみサポートされます|
|コンソール (.NET Framework)|Mac および Windows でサポートされています|
|コンソール (.NET Core)|Mac および Windows でサポートされています|

## <a name="reporting-bugs"></a>バグのレポート

[GitHub で問題を報告][bugs]し、次の情報をすべて含めるようにしてください。

### <a name="log-files"></a>ログ ファイル

常にブックのクライアントログファイルをアタッチする:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4. x では、メインメニューから直接 Finder (macOS) またはエクスプローラー (Windows) のログファイルを選択する機能もあります。

- **ログファイルの表示 > ヘルプ**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>ブック1.3 以前のログパス:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

お使いのオペレーティングシステムとインストールされている Xamarin 製品に関する詳細を知ることが非常に役立ちます。

ブックのメインメニューから、次のようにします。

- **バージョン情報のコピー > ヘルプ**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>ブック1.3 以前のブックの手順:

Visual Studio For Mac

- **Visual studio について > Visual Studio については > 情報のコピー > 詳細の表示**
- バグレポートに貼り付け

Visual Studio

- **Visual Studio > コピー情報の > に関するヘルプ**
- オペレーティングシステムのバージョンと、32ビットまたは64ビットの Windows を実行しているかどうかをお知らせください。

### <a name="samples"></a>サンプル

問題が発生している**ブック**ファイルに添付またはリンクできる場合は、バグの迅速な解決に役立つことがあります。

### <a name="devices"></a>デバイス

IOS または Android ブックの接続に問題があり、[トラブルシューティングのページ](~/tools/workbooks/troubleshooting/index.md)を既に確認している場合は、次の点について理解しておく必要があります。

- 接続しようとしているデバイスの名前
- デバイスの OS バージョン
- Android: x86 エミュレーターを使用していることを確認する
- Android: どのエミュレータープラットフォームを使用していますか? Google Emulator ですか?
  Visual Studio Android Emulator ですか? Xamarin Android Player。
- Windows 上の iOS: どのバージョンの Xamarin リモート iOS シミュレーターがインストールされていますか (**コントロールパネル**の [**プログラムの追加と削除**] を確認してください)。
- Windows 上の iOS: Mac ビルドホストのプラットフォームバージョン情報も指定してください
- デバイスはネットワークに接続されていますか (web ブラウザーで確認してください)。

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>アンインストール

### <a name="windows"></a>Windows

ブックを取得した方法によっては、2つのアンインストール手順を実行する必要があります。 これらの両方を確認して、ソフトウェアを完全にアンインストールしてください。

#### <a name="visual-studio-installer"></a>Visual Studio インストーラー

Visual Studio 2017 を使用している場合は**Visual Studio インストーラー**を開き、**個々のコンポーネント**で**Xamarin Workbooks**を探します。 チェックボックスがオンになっている場合はオフにし、[**変更**] をクリックしてアンインストールします。

#### <a name="system-uninstall"></a>システムのアンインストール

ダウンロードしたインストーラーを使用してブックをインストールした場合は、Windows 10 の**アプリ & 機能**の [システム設定] ページまたは以前のバージョンの Windows のコントロールパネルにある [**プログラムの追加と削除**] を使用してブックをアンインストールする必要があります。

> **システム > アプリ & の機能 > > 設定を開始します**

![](install-images/windows-remove.png "Xamarin Workbooks as listed in &quot;Apps &amp; features&quot;")

**その場合でも、Visual Studio インストーラーの手順に従って、ブックがユーザーの知らないうちに再インストールされないようにする必要があります。**

<a name="uninstall-macos"></a>

### <a name="macos"></a>macOS

[1.2.2](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/interactive/interactive-1.2.md)以降では、次を実行して、Xamarin Workbooks をターミナルからアンインストールできます。

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

アンインストーラーは、削除するファイルとディレクトリの詳細を確認し、続行する前に確認メッセージを表示します。

`-help`より高度なシナリオを実現するには、スクリプトに引数を渡し `uninstall` ます。

古いバージョンでは、次のものを手動で削除する必要があります。

1. `"/Applications/Xamarin Workbooks.app"` の Workbooks アプリを削除します
2. `"Applications/Xamarin Inspector.app"` の Inspector アプリを削除します
3. アドイン `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` と `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"` を削除します
4. `/Library/Frameworks/Xamarin.Interactive.framework` および `/Library/Frameworks/Xamarin.Inspector.framework` にある Inspector のファイルとサポート ファイルを削除します

## <a name="downgrading"></a>ダウングレード

**/Applications/Xamarin Workbooks.app** `com.xamarin.Inspector` `com.xamarin.Workbooks` ブックとインスペクターが完全に分割されているため、1.4 リリースのからに変更されたアプリのバンドル識別子。

以前のインストーラーのバグにより、1.3.2 またはそれ以前のインストーラーを使用して1.4 またはそれ以降のリリースをダウングレードすることはできません。

1.4 以降から1.3.2 またはそれ以降にダウングレードするには、次のようにします。

1. [ブック & インスペクターを手動でアンインストールする](#uninstall-macos)
2. 1.3.2 またはそれ以前のインストーラーを実行する `.pkg`
