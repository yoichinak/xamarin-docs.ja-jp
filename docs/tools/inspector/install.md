---
title: インスペクターのインストールと要件
description: このドキュメントでは、Xamarin インスペクターをインストールする方法について説明し、サポートされるオペレーティング システム、Ide、およびアプリのプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: f7c5217a9c2d3881ca29094c3186e448975db6a3
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268970"
---
# <a name="inspector-installation-and-requirements"></a>インスペクターのインストールと要件

## <a name="download-and-installation"></a>ダウンロードとインストール

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. ダウンロードしてインストール[Visual Studio Enterprise](https://www.visualstudio.com/vs/)を選択し、 **.NET を使用したモバイル開発**ワークロード。
1. [サインイン](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio)エンタープライズ サブスクリプションを有効にします。
1. [検査](~/tools/inspector/inspect.md)独自のアプリです。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. ダウンロードしてインストール[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/)です。
1. [サインイン](https://docs.microsoft.com/visualstudio/mac/activation)エンタープライズ サブスクリプションを有効にします。
1. [検査](~/tools/inspector/inspect.md)独自のアプリです。

-----

## <a name="requirements"></a>必要条件

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 または大きい値
- **Windows** -Windows 7、または大きい (Internet Explorer 11 以降と .NET 4.6.1 またはそれ以上)

### <a name="supported-ides"></a>サポートされている Ide

- Visual Studio for Mac
- Visual Studio 2017 **.NET を使用したモバイル開発**ワークロード

ライブ アプリ検査は、企業ユーザーが利用できます。

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>サポートされているアプリのプラットフォーム

|アプリ プラットフォーム|IDE のサポート|メモ|
|--- |--- |--- |
|Mac|Mac 用 Visual Studio でのみサポートされています。|
|iOS|Mac 用 Visual Studio 2017 および Visual Studio でサポートされています。| |
|Android|Mac 用 Visual Studio 2017 および Visual Studio でサポートされています。|Android を対象にする > = 4.0.3、 **fastdev**有効にします。<br />Google、Visual Studio または Xamarin Android エミュレーターを使用する必要があります。 Android の 7 エミュレーターは、この時点で検査を許可しません。|
|WPF|Visual Studio 2017 でのみサポートされます。|

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
