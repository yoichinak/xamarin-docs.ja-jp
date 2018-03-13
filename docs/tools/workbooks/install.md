---
title: "インストールと要件"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: abc9f9402b55a11e313b9938f07f37e5329b55b6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="installation-and-requirements"></a>インストールと要件

<script> var inspectorOnLoad 関数 () = {var primaryTextBase =「Xamarin ブック内の」; var secondaryTextBase =「または用のダウンロード」; var inspectorDownloadUrlMac ="https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin ="https://dl.xamarin.com/interactive/XamarinInteractive.msi"です。

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = 基本です。var aWin aSecondary; を =var macTextBase primaryTextBase; を =var winTextBase secondaryTextBase; を =

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>ダウンロードとインストール

<ol>
  <li>チェック、<a href="#Requirements">要件</a>以下です。</li>
  <li>ダウンロードしてインストール<a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Mac 用の Xamarin ブック</a>(<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">または Windows のダウンロード</a>)。
  </li>
  <li>開始<a href="~/tools/workbooks/workbook.md">いろいろ</a>ブックまたは試すと、<a href="https://developer.xamarin.com/workbooks/">サンプル</a>です。
    </li>
</ol>

## <a name="requirements"></a>必要条件

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 または大きい値
- **Windows** -Windows 7、または大きい (Internet Explorer 11 以降と .NET 4.6.1 またはそれ以上)

#### <a name="supported-app-platforms"></a>サポートされているアプリのプラットフォーム

<table>
<thead>
  <tr>
    <th>アプリ プラットフォーム</th>
    <th>OS のサポート</th>
    <th>メモ</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (統合)</td>
    <td>Mac 上でのみサポート</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (統合)</td>
    <td>Mac および Windows でサポート</td>
    <td>
      <ul>
        <li>Mac 上は、Xamarin.iOS 11.0 と Xcode バージョン 9.0 以降をインストールする必要があります。</li>
        <li>Windows 上の iOS ブックの実行が必要に、上記のすべての操作を実行している Mac ビルド ホストと<a href="~/tools/ios-simulator.md">リモート iOS シミュレーター</a> Windows にインストールされています。</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Mac および Windows でサポート</td>
    <td>仮想デバイスで Google、Visual Studio または Xamarin Android エミュレーターを使用する必要があります > 5.0 を =</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Windows でのみサポート</td>
    <td/>
  </tr>
  <tr>
    <td>コンソール (.NET Framework)</td>
    <td>Mac および Windows でサポート</td>
    <td/>
  </tr>
  <tr>
    <td>コンソール (.NET Core)</td>
    <td>Mac および Windows でサポート</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>バグの報告

ください[GitHub の問題の報告][bugs]、し、次の情報すべてを含めます。

### <a name="log-files"></a>ログ ファイル

ブック クライアント ログ ファイルを常に添付するには。

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

機能 Finder (macOS) またはエクスプ ローラー (Windows) 直接メイン メニューから、ログ ファイルを選択することもあります 1.4.x:

- **ヘルプ] → [ログ ファイルを表示**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>ブック 1.3 および以前のログのパス:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

オペレーティング システムに関する詳細情報を知っておくと便利なと、Xamarin の製品をインストールします。

[ブックのメイン メニュー]。

* **ヘルプ] → [コピーのバージョン情報**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>ブック 1.3 と前の手順:

Visual Studio For Mac

- **Visual Studio] → [Visual Studio] → [表示の詳細] → [コピーの情報について**
- バグのレポートに貼り付けます

Visual Studio

- **Visual Studio] → [情報のコピーに関する → をヘルプします。**
- お知らせ、オペレーティング システムのバージョンと 32 ビットまたは 64 ビットの Windows を実行しているかどうか。

### <a name="samples"></a>サンプル

アタッチしたり、リンクする場合、`.workbooks`ファイルで、問題があるバグをより迅速に解決に役立つ可能性があります。

### <a name="devices"></a>デバイス

IOS または Android のブックの接続に問題があるため、まだチェック アウトして場合[、トラブルシューティングのページ](~/tools/workbooks/troubleshooting/index.md)、知っている必要があります。

- 接続しようとしているデバイスの名前
- デバイスの OS バージョン
- Android: は、x86 を使用していることを確認してくださいエミュレーター。
- Android: エミュレーター プラットフォームを使用していますか。 Google のエミュレーターですか。
  Visual Studio の Android エミュレーターですか。 Xamarin Android Player しますか。
- Windows 上の iOS: Xamarin リモート iOS シミュレーターのバージョンはするがインストールされている (確認`Add/Remove Programs`で`Control Panel`)?
- Windows 上の iOS: Mac ビルド ホストのプラットフォームのバージョン情報のも指定してください
- デバイスはネットワークに接続 (web ブラウザーから確認) しますか。

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>[アンインストール]

### <a name="windows"></a>Windows

どのブック & インスペクターを取得、によっては、次の 2 つのアンインストール手順を実行する必要があります。 ソフトウェアを完全にアンインストールするこれらの両方を確認してください。

#### <a name="visual-studio-installer"></a>Visual Studio インストーラー

Visual Studio 2017 があれば、開く**Visual Studio インストーラー**、ファイルの場所と**個々 のコンポーネント**の**Xamarin ブック**です。 オンの場合は、オフにしてクリックして**変更**をアンインストールします。

#### <a name="system-uninstall"></a>システムのアンインストール

使用してアンインストールする必要がありますをインストールしたブック & インスペクター自分でダウンロードしたインストーラーに場合、**アプリおよび機能**または経由で Windows 10 でのシステム設定] ページ**プログラムの追加/削除**古いバージョンの Windows コントロール パネルの [します。

> **起動] → [設定] → [システム] → [アプリケーションと機能**

![](install-images/windows-remove.png "Xamarin のブックとインスペクターに表示される&quot;アプリ&amp;機能&quot;")

**確認してブックを作成する Visual Studio インストーラー用の手順を実行する必要がありますも & インスペクターは、知らない間に再インストール取得されません。**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

以降で[1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/)、Xamarin ブック & インスペクターを実行しての端末からアンインストールできます。

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

バンドル id`/Applications/Xamarin Workbooks.app`から変更`com.xamarin.Inspector`に`com.xamarin.Workbooks`Xamarin ブック & インスペクターのインストーラーの将来の分割を容易に 1.4 のリリースでします。

古いインストーラーでのバグ、により、1.3.2 または古いインストーラーを使用して 1.4 以降のリリースをダウン グレードすることはできません。

1.4 または 1.3.2 を新しいまたは古いからダウン グレードします。

1. [インスペクター (&)、ブックを手動でアンインストールします。](#macOS)
2. 実行、1.3.2 または古い`.pkg`インストーラー