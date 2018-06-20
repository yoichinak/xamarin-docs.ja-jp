---
title: ブックのインストールと要件
description: このドキュメントでは、ダウンロードして、サポートされているプラットフォームとシステム要件について説明する、Xamarin のブックをインストールする方法について説明します。
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: a35038a948a89889bbf067a453b7465c1a6a7b49
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268993"
---
# <a name="workbooks-installation-and-requirements"></a>ブックのインストールと要件

<a name="install" />

## <a name="download-and-install"></a>ダウンロードとインストール

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. チェック、[要件](#requirements)以下です。
2. ダウンロードしてインストール[Windows 用の Xamarin ブック](https://dl.xamarin.com/interactive/XamarinInteractive.msi)です。
3. 開始[いろいろ](~/tools/workbooks/workbook.md)ブックまたは試すと、[サンプル](https://developer.xamarin.com/workbooks)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. チェック、[要件](#Requirements)以下です。
2. ダウンロードしてインストール[Mac 用の Xamarin ブック](https://dl.xamarin.com/interactive/XamarinInteractive.pkg)です。
3. 開始[いろいろ](~/tools/workbooks/workbook.md)ブックまたは試すと、[サンプル](https://developer.xamarin.com/workbooks)

-----

## <a name="requirements"></a>必要条件

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 または大きい値
- **Windows** -Windows 7、または大きい (Internet Explorer 11 以降と .NET 4.6.1 またはそれ以上)

#### <a name="supported-app-platforms"></a>サポートされているアプリのプラットフォーム

|アプリ プラットフォーム|OS のサポート|メモ|
|--- |--- |--- |
|Mac|Mac 上でのみサポート|
|iOS|Mac および Windows でサポート|Mac 上は、Xamarin.iOS 11.0 と Xcode バージョン 9.0 以降をインストールする必要があります。 Windows 上の iOS ブックの実行が必要に、上記のすべての操作を実行している Mac ビルド ホストと[リモート iOS シミュレーター](~/tools/ios-simulator.md) Windows にインストールされています。|
|Android|Mac および Windows でサポート|仮想デバイスで Google、Visual Studio または Xamarin Android エミュレーターを使用する必要があります > 5.0 を =|
|WPF|Windows でのみサポート|
|コンソール (.NET Framework)|Mac および Windows でサポート|
|コンソール (.NET Core)|Mac および Windows でサポート|


## <a name="reporting-bugs"></a>バグの報告

ください[GitHub の問題の報告][bugs]、し、次の情報すべてを含めます。

### <a name="log-files"></a>ログ ファイル

ブック クライアント ログ ファイルを常に添付するには。

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

機能 Finder (macOS) またはエクスプ ローラー (Windows) 直接メイン メニューから、ログ ファイルを選択することもあります 1.4.x:

- **ヘルプ > ログ ファイルを表示します。**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>ブック 1.3 および以前のログのパス:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

オペレーティング システムに関する詳細情報を知っておくと便利なと、Xamarin の製品をインストールします。

[ブックのメイン メニュー]。

* **ヘルプ > のバージョン情報をコピー**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>ブック 1.3 と前の手順:

Visual Studio For Mac

- **Visual Studio > Visual Studio に関する > 詳細を表示 > 情報のコピー**
- バグのレポートに貼り付けます

Visual Studio

- **ヘルプ > Visual Studio に関する > 情報のコピー**
- お知らせ、オペレーティング システムのバージョンと 32 ビットまたは 64 ビットの Windows を実行しているかどうか。

### <a name="samples"></a>サンプル

アタッチしたり、リンクする場合、 **.workbooks**ファイルで、問題があるバグをより迅速に解決に役立つ可能性があります。

### <a name="devices"></a>デバイス

IOS または Android のブックの接続に問題があるため、まだチェック アウトして場合[、トラブルシューティングのページ](~/tools/workbooks/troubleshooting/index.md)、知っている必要があります。

- 接続しようとしているデバイスの名前
- デバイスの OS バージョン
- Android: は、x86 を使用していることを確認してくださいエミュレーター。
- Android: エミュレーター プラットフォームを使用していますか。 Google のエミュレーターですか。
  Visual Studio の Android エミュレーターですか。 Xamarin Android Player しますか。
- Windows 上の iOS: Xamarin リモート iOS シミュレーターのバージョンはするがインストールされている (確認**プログラムの追加/削除**で**コントロール パネルの**)?
- Windows 上の iOS: Mac ビルド ホストのプラットフォームのバージョン情報のも指定してください
- デバイスはネットワークに接続 (web ブラウザーから確認) しますか。

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>[アンインストール]

### <a name="windows"></a>Windows

ブックを取得する方法によっては、次の 2 つのアンインストール手順を実行する必要があります。 ソフトウェアを完全にアンインストールするこれらの両方を確認してください。

#### <a name="visual-studio-installer"></a>Visual Studio インストーラー

Visual Studio 2017 があれば、開く**Visual Studio インストーラー**、ファイルの場所と**個々 のコンポーネント**の**Xamarin ブック**です。 オンの場合は、オフにしてクリックして**変更**をアンインストールします。

#### <a name="system-uninstall"></a>システムのアンインストール

使用してアンインストールする必要がありますをインストールした場合のブック自分でダウンロードしたインストーラーで、**アプリおよび機能**または経由で Windows 10 でのシステム設定 ページ**プログラムの追加/削除**コントロールで古いバージョンの Windows 上のパネルです。

> **開始 > 設定 > システム > アプリケーションと機能**

![](install-images/windows-remove.png "Xamarin のブックに記載されている&quot;アプリ&amp;機能&quot;")

**Visual Studio インストーラー ブックは、知らない間に再インストール取得されないことを確認するための手順をまだ従う必要があります。**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

以降で[1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/)Xamarin のブックを実行して、ターミナルからアンインストールすることができます。

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

ファイルとディレクトリが削除されに続行する前に確認を求める、アンインストーラーは詳しく説明します。

渡す、`-help`への引数、`uninstall`高度なシナリオ用のスクリプト。

古いバージョンでは、次のものを手動で削除する必要があります。

1. `"/Applications/Xamarin Workbooks.app"` の Workbooks アプリを削除します
2. `"Applications/Xamarin Inspector.app"` の Inspector アプリを削除します
2. アドイン `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` と `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"` を削除します
3. `/Library/Frameworks/Xamarin.Interactive.framework` および `/Library/Frameworks/Xamarin.Inspector.framework` にある Inspector のファイルとサポート ファイルを削除します

## <a name="downgrading"></a>ダウン グレード

バンドル id **/applications/xamarin Workbooks.app**から変更`com.xamarin.Inspector`に`com.xamarin.Workbooks`1.4 のリリースでは、ブックとインスペクターは完全に分割します。

古いインストーラーでのバグ、により、1.3.2 または古いインストーラーを使用して 1.4 以降のリリースをダウン グレードすることはできません。

1.4 または 1.3.2 を新しいまたは古いからダウン グレードします。

1. [インスペクター (&)、ブックを手動でアンインストールします。](#macOS)
2. 実行、1.3.2 または古い`.pkg`インストーラー
