---
title: インスペクターのインストールと要件
description: このドキュメントでは、Xamarin インスペクターをインストールする方法について説明し、サポートされるオペレーティング システム、Ide、およびアプリのプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: 80bf3cb4e8e27355ccf6213dbfd07a17e992961b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793809"
---
# <a name="inspector-installation-and-requirements"></a>インスペクターのインストールと要件

## <a name="download-and-installation"></a>ダウンロードとインストール

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. ダウンロードしてインストール[Xamarin ブック & インスペクターのウィンドウ](https://dl.xamarin.com/interactive/XamarinInteractive.msi)します。
2. [独自のアプリを検査します。](~/tools/inspector/inspect.md)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. ダウンロードしてインストール[Xamarin ブック & for Mac インスペクター](https://dl.xamarin.com/interactive/XamarinInteractive.pkg)です。
2. [独自のアプリを検査します。](~/tools/inspector/inspect.md)

-----

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

|アプリ プラットフォーム|IDE のサポート|メモ|
|--- |--- |--- |
|Mac (統合)|Mac 上でのみサポート|
|iOS (統合)|XS および Visual Studio でサポートされています。|Windows からの iOS アプリを調べることには、同じバージョンの Mac ビルド ホストにインストールされているもの検査が必要です。|
|Android|XS および Visual Studio でサポートされています。|Android を対象にする > = 4.0.3、 **fastdev**有効にします。<br />Google、Visual Studio または Xamarin Android エミュレーターを使用する必要があります。 Android の 7 エミュレーターは、この時点で検査を許可しません。|
|WPF|Windows 上の Visual Studio でのみサポートされます。|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>バグの報告

Visual Studio を使用して直接バグを報告する必要があります。

- **ヘルプ > フィードバックを送信 > 問題を報告します。**

すべての次の情報を記入してください。

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

この情報は重要です。

Visual Studio For Mac

- **Visual Studio > Visual Studio に関する > 詳細を表示 > 情報のコピー**
- バグのレポートに貼り付けます

Xamarin Studio

- **Xamarin Studio > Xamarin Studio に関する > 詳細を表示 > 情報のコピー**
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

- **ヘルプ > ログ ファイルを表示します。**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio の内容**出力**ウィンドウできない可能性がありますも有益です。

### <a name="project-settings"></a>プロジェクト設定

アタッチすることができる場合、 **.csproj**を検査しようとするプロジェクトでになります非常に役立ちます。 これは、個々 の設定について確認するよりも簡単です。

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

Visual Studio 2017 があれば、開く**Visual Studio インストーラー**、ファイルの場所と**個々 のコンポーネント**の**Xamarin ブック**です。 オンの場合は、これをオフにしをアンインストールするには、[変更] をクリックします。

#### <a name="system-uninstall"></a>システムのアンインストール

使用してアンインストールする必要がありますをインストールしたブック & インスペクター自分でダウンロードしたインストーラーに場合、**アプリおよび機能**または経由で Windows 10 でのシステム設定] ページ**プログラムの追加/削除**古いバージョンの Windows コントロール パネルの [します。

> **開始 > 設定 > システム > アプリケーションと機能**

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
3. アドイン `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` と `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"` を削除します
4. `/Library/Frameworks/Xamarin.Interactive.framework` および `/Library/Frameworks/Xamarin.Inspector.framework` にある Inspector のファイルとサポート ファイルを削除します
