---
title: インスペクターのインストールと要件
description: このドキュメントでは、Xamarin Inspector のインストール方法と、サポートされているオペレーティングシステム、Ide、およびアプリプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 19c4a15fb2490c7bace4798b0cb8e062b1379a04
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029695"
---
# <a name="inspector-installation-and-requirements"></a>インスペクターのインストールと要件

## <a name="download-and-installation"></a>ダウンロードとインストール

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/)をダウンロードしてインストールし、 **[.net を使用したモバイル開発]** ワークロードを選択します。
1. [サインイン](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio)して、エンタープライズサブスクリプションを有効にします。
1. 自分のアプリを[調査](~/tools/inspector/inspect.md)してください。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)をダウンロードしてインストールします。
1. [サインイン](https://docs.microsoft.com/visualstudio/mac/activation)して、エンタープライズサブスクリプションを有効にします。
1. 自分のアプリを[調査](~/tools/inspector/inspect.md)してください。

-----

## <a name="requirements"></a>［要件］

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 以上
- **Windows** -windows 7 以降 (Internet Explorer 11 以降および .net 4.6.1 以上)

### <a name="supported-ides"></a>サポートされている Ide

- Visual Studio for Mac
- .NET ワークロード**を使用したモバイル開発**を使用した Visual Studio 2017

企業のお客様は、ライブアプリの検査を利用できます。

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>サポートされているアプリプラットフォーム

|アプリのプラットフォーム|IDE のサポート|ノート|
|--- |--- |--- |
|Mac|Visual Studio for Mac でのみサポートされています|
|iOS|Visual Studio 2017 および Visual Studio for Mac でサポートされています| リンカーの動作は **[リンクしない]** に設定する必要があります ( **[iOS ビルド]** プロジェクトオプション の下) |
|Android|Visual Studio 2017 および Visual Studio for Mac でサポートされています|**Fastdev**が有効になっている Android > = 4.0.3 を対象とする必要があります。<br />Google、Visual Studio、または Xamarin Android エミュレーターを使用する必要があります。 Android 7 エミュレーターでは、現時点では検査できない場合があります。|
|WPF|Visual Studio 2017 でのみサポートされます|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>バグの報告

バグは、Visual Studio を使用して直接報告する必要があります。

- **フィードバックを送信して問題を報告 > > には、ヘルプを参照してください**

次の情報をすべて含めるようにしてください。

### <a name="platform-version-information"></a>プラットフォームのバージョン情報

この情報は不可欠です。

Visual Studio For Mac

- **Visual studio について > Visual Studio については > 情報のコピー > 詳細の表示**
- バグレポートに貼り付け

Visual Studio

- **Visual Studio > コピー情報の > に関するヘルプ**
- オペレーティングシステムのバージョンと、32ビットまたは64ビットの Windows を実行しているかどうかをお知らせください。

### <a name="log-files"></a>ログ ファイル

常に IDE とインスペクターの両方のクライアントログファイルをアタッチします。

インスペクタークライアント

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4. x では、メインメニューから直接 Finder (macOS) またはエクスプローラー (Windows) のログファイルを選択する機能もあります。

- **ログファイルの表示 > ヘルプ**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio の **[出力]** ウィンドウの内容が有益な場合もあります。

### <a name="project-settings"></a>プロジェクト設定

調べようとしているプロジェクトに **.csproj**をアタッチできる場合は、それが非常に便利です。 これは、個々の設定について質問するよりも簡単です。

また、デバッグ構成であることを確認してください。

### <a name="selected-devices"></a>選択したデバイス

Android と iOS の場合、検査するときに、どのデバイスがデバッグ中であるかを把握しておくことが重要です。 次の点を把握しておく必要があります。

- IDE に表示されているデバイスの名前
- デバイスの OS バージョン
- Android: x86 エミュレーターを使用していることを確認する
- Android: どのエミュレータープラットフォームを使用していますか? Google Emulator ですか? Visual Studio Android Emulator ですか? Xamarin Android Player。
- デバッグ中のアプリがデバイスで正しく表示され、機能しているか。
- デバイスはネットワークに接続されていますか (web ブラウザーで確認してください)。

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
