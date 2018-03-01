---
title: "インストールと要件"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>インストールと要件

<script> var inspectorOnLoad 関数 () = {var primaryTextBase ="Xamarin ブック & インスペクターを"; var secondaryTextBase =「または用のダウンロード」; var inspectorDownloadUrlMac ="https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; varinspectorDownloadUrlWin ="https://dl.xamarin.com/interactive/XamarinInteractive.msi"です。

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = 基本です。var aWin aSecondary; を =var macTextBase primaryTextBase; を =var winTextBase secondaryTextBase; を =

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>ダウンロードとインストール

<ol>
  <li>ダウンロードしてインストール<a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin ブック & for Mac インスペクター</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">または Windows のダウンロード</a>)。
  </li>
  <li><a href="~/tools/inspector/inspect.md"> 独自のアプリを検査します。</a>
    </li>
</ol>

## <a name="requirements"></a>必要条件

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 または大きい値
- **Windows** -Windows 7、または大きい (Internet Explorer 11 以降と .NET 4.6.1 またはそれ以上)

### <a name="supported-ides"></a>サポートされている Ide

- Xamarin Studio 6.2 以降
- Visual Studio Mac Preview 4 以降
- Xamarin を使用した visual Studio 2015 4.3.x 以上
- Xamarin のワークロードでの visual Studio 2017

ライブ アプリ検査は、企業ユーザーが利用できます。

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>サポートされているアプリのプラットフォーム

<table>
<thead>
  <tr>
    <th>アプリ プラットフォーム</th>
    <th>IDE のサポート</th>
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
    <td>XS および Visual Studio でサポートされています。</td>
    <td>Windows からの iOS アプリを調べることには、同じバージョンの Mac ビルド ホストにインストールされているもの検査が必要です。</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>XS および Visual Studio でサポートされています。</td>
    <td>
      <ul>
        <li>Android を対象にする > 4.0.3 を =</li>
        <li>Fastdev 有効でなければなりません</li>
        <li>Google、Visual Studio または Xamarin Android エミュレーターを使用する必要があります。 Android の 7 エミュレーターは、この時点で検査を許可しません。</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Windows 上の Visual Studio でのみサポートされます。</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>バグの報告

Visual Studio を使用して直接バグを報告する必要があります。

- **→ 送信フィードバック → レポート問題をヘルプします。**

すべての次の情報を記入してください。

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

この情報は重要です。

Visual Studio For Mac

- **Visual Studio] → [Visual Studio] → [表示の詳細] → [コピーの情報について**
- バグのレポートに貼り付けます

Xamarin Studio

- **Xamarin Studio → Xamarin Studio → 表示に関する詳細の → コピーの情報**
- バグのレポートに貼り付けます

Visual Studio

- **ヘルプ > Visual Studio に関する > 情報のコピー**
- お知らせ、オペレーティング システムのバージョンと 32 ビットまたは 64 ビットの Windows を実行しているかどうか。

### <a name="log-files"></a>ログ ファイル

常に IDE およびインスペクターの両方のクライアント ログ ファイルをアタッチします。

インスペクター クライアント

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

機能 Finder (macOS) またはエクスプ ローラー (Windows) 直接メイン メニューから、ログ ファイルを選択することもあります 1.4.x:

- **ヘルプ] → [ログ ファイルを表示**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio の内容`Output`ウィンドウできない可能性がありますも有益です。

### <a name="project-settings"></a>プロジェクト設定

アタッチすることができる場合、`.csproj`検査しようとするプロジェクトでになります非常に役立ちます。 これは、個々 の設定について確認するよりも簡単です。

デバッグ構成であることを確認してください。

### <a name="selected-devices"></a>選択されているデバイス

Android と iOS の場合は、検査する場合のデバッグしているデバイスが分かっている重要です。 知る必要があります。

- IDE で示すようにデバイスの名前
- デバイスの OS バージョン
- Android: は、x86 を使用していることを確認してくださいエミュレーター。
- Android: エミュレーター プラットフォームを使用していますか。 Google のエミュレーターですか。 Visual Studio の Android エミュレーターですか。 Xamarin Android Player しますか。
- 適切にデバッグしているアプリが表示され、デバイスで機能しますか。
- デバイスはネットワークに接続 (web ブラウザーから確認) しますか。

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>[アンインストール]

### <a name="windows"></a>Windows

どのブック & インスペクターを取得、によっては、次の 2 つのアンインストール手順を実行する必要があります。 ソフトウェアを完全にアンインストールするこれらの両方を確認してください。

#### <a name="visual-studio-installer"></a>Visual Studio インストーラー

Visual Studio 2017 をした場合は、「Visual Studio インストーラー」を開き「Xamarin ブック」の「個々 のコンポーネント」を確認します。 オンの場合は、これをオフにしをアンインストールするには、[変更] をクリックします。

#### <a name="system-uninstall"></a>システムのアンインストール

使用してアンインストールする必要がありますをインストールしたブック & インスペクター自分でダウンロードしたインストーラーに場合、**アプリおよび機能**または経由で Windows 10 でのシステム設定] ページ**プログラムの追加/削除**古いバージョンの Windows コントロール パネルの [します。

> **起動] → [設定] → [システム] → [アプリケーションと機能**

![](install-images/windows-remove.png "Xamarin ブックおよびインスペクター 'アプリケーションと機能 に表示されます。")

**確認してブックを作成する Visual Studio インストーラー用の手順を実行する必要がありますも & インスペクターは、知らない間に再インストール取得されません。**

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

