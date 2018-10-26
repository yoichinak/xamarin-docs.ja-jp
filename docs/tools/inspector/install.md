---
title: インスペクターのインストールと要件
description: このドキュメントでは、Xamarin Inspector をインストールする方法について説明し、サポートされるオペレーティング システムや Ide、アプリのプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 2357003e3a855981f053c48a596b932d9ba36d90
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104961"
---
# <a name="inspector-installation-and-requirements"></a>インスペクターのインストールと要件

## <a name="download-and-installation"></a>ダウンロードとインストール

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. ダウンロードしてインストール[Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/)を選択し、 **.NET によるモバイル開発**ワークロード。
1. [サインイン](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio)エンタープライズ サブスクリプションを有効にします。
1. [検査](~/tools/inspector/inspect.md)独自のアプリです。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. ダウンロードしてインストール[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)します。
1. [サインイン](https://docs.microsoft.com/visualstudio/mac/activation)エンタープライズ サブスクリプションを有効にします。
1. [検査](~/tools/inspector/inspect.md)独自のアプリです。

-----

## <a name="requirements"></a>必要条件

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 以降
- **Windows** -Windows 7 以上 (Internet Explorer 11 以降と .NET 4.6.1 以降)

### <a name="supported-ides"></a>サポートされている Ide

- Visual Studio for Mac
- Visual Studio 2017 と **.NET によるモバイル開発**ワークロード

ライブ アプリの検査は企業のお客様に使用できます。

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>サポートされているアプリのプラットフォーム

|アプリ プラットフォーム|IDE のサポート|メモ|
|--- |--- |--- |
|Mac|Visual studio for Mac のみサポート|
|iOS|Visual Studio 2017 と Visual Studio for Mac のサポート| |
|Android|Visual Studio 2017 と Visual Studio for Mac のサポート|Android をターゲットする必要があります > = 4.0.3、 **fastdev**を有効にします。<br />Google、Visual Studio または Xamarin Android エミュレーターを使用する必要があります。 Android 7 エミュレーターは、この時点での検査を許可しません。|
|WPF|Visual Studio 2017 でのみサポートされています|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>バグの報告

Visual Studio を使用して直接バグを報告する必要があります。

- **ヘルプ > フィードバックを送信 > 問題を報告します。**

すべての次の情報を含めてください。

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

この情報は重要です。

Visual Studio For Mac

- **Visual Studio > Visual Studio のバージョン情報 > 詳細を表示 > 情報のコピー**
- バグ レポートに貼り付けます

Visual Studio

- **ヘルプ > Visual Studio のバージョン情報 > 情報のコピー**
- オペレーティング システムのバージョンと 32 ビットまたは 64 ビットの Windows を実行しているかどうかを通知しています。

### <a name="log-files"></a>ログ ファイル

常に IDE およびインスペクターの両方のクライアント ログ ファイルをアタッチします。

Inspector クライアント

- Mac の場合: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x では、メイン メニューの検索 (macOS) またはエクスプ ローラー (Windows) 直接ログ ファイルを選択する機能も機能します。

- **ヘルプ > ログ ファイルの表示**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio の内容**出力**ウィンドウできない可能性がありますも有益です。

### <a name="project-settings"></a>プロジェクト設定

アタッチすることができる場合、 **.csproj**を検査しようとして、プロジェクトの非常に役に立ちます。 これは、個々 の設定について質問するより簡単です。

デバッグ構成であることを確認してください。

### <a name="selected-devices"></a>選択したデバイス

Android と iOS の場合は、検査するときでデバッグしているデバイスはどのような把握している点が重要です。 知る必要があります。

- お使いの IDE で示すようにデバイスの名前
- デバイスの OS バージョン
- Android: は、x86 を使用していることを確認しますエミュレーター。
- Android: どのようなエミュレーターのプラットフォームを使用していますか。 Google エミュレーターでしょうか。 Visual Studio Android Emulator でしょうか。 Xamarin Android Player でしょうか。
- 適切にデバッグしているアプリが表示され、デバイスで機能しますか。
- デバイスにはネットワーク接続 (web ブラウザーを使用してチェック) がありますか。

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
