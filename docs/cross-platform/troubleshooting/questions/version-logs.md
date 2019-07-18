---
title: バージョン情報とログはどこにありますか
description: このドキュメントでは、Xamarin バージョン情報とログ検索を検索する場所について説明します。 この情報は、バグを送信またはサポートの問題を診断する場合に便利です。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: e389bc33538ec3c3d36eb749c746f5a4723aab3c
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830356"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>バージョン情報とログはどこにありますか

## <a name="outline"></a>外枠

- [バージョン情報](#version-information)
    - Windows のバージョン情報
    - Mac のバージョン情報
    - Android SDK Tools、プラットフォーム ツール、ビルド ツール
- [IDE とインストーラーのログ](#ide-and-installer-logs)
    - [Windows ログ](#windows-logs)
        - Xamarin Studio
        - Xamarin for Visual Studio
        - Xamarin ユニバーサル インストーラー
        - 個別`.msi`インストーラー、詳細ログ
        - Visual Studio の起動、詳細ログ
    - [Mac ログ](#mac-logs)
        - ビルド ホスト
    - Visual Studio for Mac
        - Xamarin Studio
        - Xamarin インストーラー
- [詳細なビルドの出力](#verbose-build-output-logs)
- [Xamarin.Android と Xamarin.iOS アプリのログをデバッグします。](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat ログ
    - iOS シミュレーター (Mac) でログに記録します。
    - iOS デバイスのログを (on Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />バージョン情報

これは通常送信する最適なバックアップからの情報はすべて、**情報のコピー**ボタン。 それ以外の場合が、追加情報を要求する必要があります。 Xcode のバージョンのオペレーティング システム バージョンには、Android API レベルがインストールされているし、.NET のバージョンはすべて重要になる問題のトラブルシューティングを行う際など。

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows のバージョン情報

#### <a name="xamarin-studio"></a>Xamarin Studio

**ヘルプ > に関する > 詳細の表示 > [ボタン] の情報をコピー**

#### <a name="visual-studio"></a>Visual Studio

**ヘルプ > Microsoft Visual Studio > 情報のコピー [ボタン]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac のバージョン情報

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual Studio > Visual Studio に関する > 詳細の表示 > [ボタン] の情報をコピー**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools、プラットフォーム ツール、ビルド ツール

Android SDK Manager を開き、上部のスクリーン ショットは撮影**ツール**セクション。

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ツール > Android SDK マネージャーを開く**

#### <a name="visual-studio"></a>Visual Studio

**ツール > Android > Android SDK マネージャーを開きます.**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE とインストーラーのログ

各ログの場所を必ずを zip 圧縮し、全体のログ フォルダーをアタッチします。

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows ログ

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools for Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Visual Studio のインストール ログを取得する方法](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin の「汎用」インストーラー

`%LOCALAPPDATA%\Xamarin\Universal`

これらは、ログから、`XamarinInstaller.exe`インストーラー。

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />個別`.msi`インストーラー、詳細ログ

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

参照:[コマンド ライン オプション](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio の起動、詳細ログ

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Reference: [/Log (devenv.exe)](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac ログ

選択することができます、**移動 > フォルダーに移動して**メニュー Finder で項目をコピーしてこれらのパスのいずれかのダイアログ ボックスに貼り付けます。

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio for Mac

`~/Library/Logs/VisualStudio/7.0` (この数を使用しているバージョンに応じて変更可能性があります)

このフォルダー開くこともできますを使用して [ログ ディレクトリを開く]-> [ヘルプ]。

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (この数を使用しているバージョンに応じて変更可能性があります)

このフォルダー開くこともできますを使用して [ログ ディレクトリを開く]-> [ヘルプ]。

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin の「汎用」インストーラー

`~/Library/Logs/XamarinInstaller/Universal`

これらは、ログから、`XamarinInstaller.dmg`インストーラー。

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin のビルド ホスト

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />詳細なビルドの出力

1.  有効にする[診断 MSBuild 出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)します。

2.  IOS のアプリを有効にすることも**詳細 mtouch 出力**を追加して`-v -v -v -v`**プロジェクトのプロパティ > iOS ビルド > 全般 (タブ) > 追加のオプション > 追加 mtouch 引数**します。

3.  消去し、プロジェクトをリビルドします。

4.  コピーして、IDE からビルド出力をテキスト ファイルに貼り付けます。
     - Visual Studio (Windows):**ビュー > 出力 > からの出力を表示します。ビルド**
     - Visual Studio for Mac。**ビュー > パッド > エラー > ビルドの出力 (タブ)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Xamarin.Android と Xamarin.iOS アプリのログをデバッグします。

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ビュー > パッド > アプリケーションの出力**

(このメニュー項目が、アプリが起動した後をのみ表示されることに注意してください)。

### <a name="visual-studio"></a>Visual Studio

**ビュー > 出力 > からの出力を表示します。デバッグ**

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpsdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](https://developer.android.com/tools/help/adb.html) logcat ログ

実行後、`adb`コマンドをもう一度アタッチ、 **android_logcat.txt**デスクトップからファイル。 これらの手順では、1 つだけのデバイスを接続している前提としています。

参照してください、 [Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)ページ。

#### <a name="visual-studio"></a>Visual Studio

1. **ツール > Android > Android Adb コマンド プロンプトを起動**
2. ログをクリーンアップするには。 `adb logcat -c`
3. 問題を再現します。
4. ログを出力します。 `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. **ツール > Android SDK コマンド プロンプトを開く**
2. ログをクリーンアップするには。 `adb logcat -c`
3. 問題を再現します。
4. ログを出力します。 `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS シミュレーター (Mac) でログに記録します。

* システム ログにアクセスするには、選択**デバッグ > システム ログを開く.** iOS シミュレーターのアプリでします。

* シミュレーターからクラッシュ レポートを表示する Console.app を開きに移動`~/Library/Logs > DiagnosticReports`します。

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS デバイスのログを (on Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ビュー > パッド > iOS デバイスのログ**

#### <a name="xcode"></a>Xcode

**ウィンドウ > デバイス > ${devicename}**

クラッシュ レポートは 使用可能な**デバイス ログを表示する**ボタンをクリックします。 システム ログ、デバイスでは、矢印、ウィンドウの下部が表示されます。

#### <a name="xcode-5"></a>Xcode 5

**ウィンドウ > オーガナイザー > デバイス (タブ) > ${devicename}**
