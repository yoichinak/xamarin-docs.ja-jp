---
title: バージョン情報とログを検索する場所は?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cdbe480c45e9c0117f1437b1ee632f6ea8f142e0
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>バージョン情報とログを検索する場所は?

## <a name="outline"></a>アウトライン

- [バージョン情報](#version-information)
    - Windows のバージョン情報
    - Mac バージョン情報
    - Android SDK ツール、プラットフォーム ツール、ビルド ツール
- [IDE およびインストーラーのログ](#ide-and-installer-logs)
    - [Windows ログ](#windows-logs)
        - Xamarin Studio
        - Xamarin for Visual Studio
        - Xamarin ユニバーサル インストーラー
        - 各`.msi`インストーラー、詳細ログ
        - Visual Studio のスタートアップ、詳細ログ
    - [Mac のログ](#mac-logs)
        - ホストを構築します。
    - Visual Studio for Mac
        - Xamarin Studio
        - Xamarin のインストーラー
- [詳細なビルドの出力](#verbose-build-output-logs)
- [Xamarin.Android および Xamarin.iOS アプリのログをデバッグします。](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat ログ
    - iOS シミュレーター (Mac) のログに記録します。
    - iOS デバイスがログオン (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />バージョン情報

これは通常、すべての情報の送信に最適なバックアップ、**コピー情報**ボタン。 それ以外の場合おが、追加情報を要求する必要があります。 たとえば、Xcode のバージョンのオペレーティング システム バージョンには、Android API レベルがインストールされているでき、.NET のバージョンすべて重要なときに、問題のトラブルシューティングに役立ちます。

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows のバージョン情報

#### <a name="xamarin-studio"></a>Xamarin Studio

**ヘルプ > に関する > 詳細の表示 > [ボタン] の情報をコピー**

#### <a name="visual-studio"></a>Visual Studio

**ヘルプ > Microsoft Visual の Studio に関する > 情報のコピー [ボタン]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac バージョン情報

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual Studio > Visual Studio に関する > 詳細の表示 > [ボタン] の情報をコピー**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK ツール、プラットフォーム ツール、ビルド ツール

Android SDK Manager を開き、上部のスクリーン ショットは撮影**ツール**セクションです。

![](https://kb.xamarin.com/customer/portal/attachments/337323 "スクリーン ショットの Android SDK Manager > ツール フォルダー")

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ツール > Android SDK Manager を開く**

#### <a name="visual-studio"></a>Visual Studio

SDK Manager ツールバーのアイコンを選択します。

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Visual Studio での Android SDK Manager ツールバーのアイコンを開始します。")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE およびインストーラーのログ

各ログの場所を必ず zip 圧縮し、全体のログ フォルダーをアタッチしてください。

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows ログ

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Xamarin 用の visual Studio ツール

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Visual Studio のインストール ログを取得する方法](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin「汎用」インストーラー

`%LOCALAPPDATA%\Xamarin\Universal`

これらは、ログが、`XamarinInstaller.exe`インストーラーです。

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />各`.msi`インストーラー、詳細ログ

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

参照:[コマンド ライン オプション](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio のスタートアップ、詳細ログ

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

参照: [/Log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac のログ

選択することができます、**移動 > フォルダーに移動**メニュー Finder で、項目をコピーしてこれらのパスのいずれかのダイアログ ボックスに貼り付けます。

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio for Mac

`~/Library/Logs/VisualStudio/7.0` (この数を使用しているバージョンに応じて変更可能性があります)

このフォルダー開くこともできます経由で"オープン ログ ディレクトリ]-> [Help"です。

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (この数を使用しているバージョンに応じて変更可能性があります)

このフォルダー開くこともできます経由で"オープン ログ ディレクトリ]-> [Help"です。

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin「汎用」インストーラー

`~/Library/Logs/XamarinInstaller/Universal`

これらは、ログが、`XamarinInstaller.dmg`インストーラーです。

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin のビルド ホスト

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />詳細なビルドの出力

1.  有効にする[MSBuild の診断出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)です。

2.  IOS のアプリも有効にする**verbose mtouch 出力**を追加して`-v -v -v -v`**プロジェクトのプロパティ > iOS ビルド > [tab] の [全般] > 追加のオプション > 追加 mtouch 引数**です。

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "追加 mtouch 引数入力フィールドに次の 4 つ dash v を入力してください。")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio for Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "追加 mtouch 引数入力フィールドに次の 4 つ dash v を入力してください。")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  消去し、プロジェクトをリビルドします。

4.  コピーし、IDE からのビルド出力をテキスト ファイルに貼り付けます。
     - Visual Studio (Windows):**ビュー > 出力 > から出力を表示: ビルド**
     - Visual Studio for Mac:**ビュー > パッド > エラー > ビルド出力 [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Xamarin.Android および Xamarin.iOS アプリのログをデバッグします。

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ビュー > パッド > アプリケーションの出力**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Mac 用の Visual Studio で、アプリケーションの出力パッド アイコン")


(このメニュー項目がアプリを起動した後をのみ表示されることに注意してください)。

### <a name="visual-studio"></a>Visual Studio

**ビュー > 出力 > から出力を表示: デバッグ**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Visual Studio でデバッグ オプションを示す出力パッド")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat ログ

実行した後、`adb`コマンドをバックをアタッチ、 **android_logcat.txt**ファイルをデスクトップから。 これらの手順では、1 つだけのデバイスが接続するいると仮定します。

参照、 [Android のデバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)ページ。

#### <a name="visual-studio"></a>Visual Studio

1.  **ツール > Android > Android Adb コマンド プロンプトを起動**
2.  ログをクリーンアップするには。 `adb logcat -c`
3.  問題を再現します。
4.  ログを出力します。 `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  **ツール > Android SDK コマンド プロンプトを開きます**
2.  ログをクリーンアップするには。 `adb logcat -c`
3.  問題を再現します。
4.  ログを出力します。 `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS シミュレーター (Mac) のログに記録します。

*   システム ログにアクセスするには、選択**デバッグ > システム ログを開く.** iOS シミュレーター アプリ。

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "[デバッグ] メニューのシステム ログを開くオプションを表示")

*   シミュレーターからクラッシュ レポートを表示する Console.app を開きに移動`~/Library/Logs > DiagnosticReports`です。

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS デバイスがログオン (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ビュー > パッド > iOS デバイス ログ**

#### <a name="xcode"></a>Xcode 

**ウィンドウ > デバイス > ${devicename}**

クラッシュ レポートは 使用可能な**デバイス ログの表示**ボタンをクリックします。 漏えい矢印の下のウィンドウの下部にあるデバイスのシステム ログが表示されます。 <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />である必要があります。

#### <a name="xcode-5"></a>Xcode 5

**ウィンドウ > オーガナイザー > デバイス [tab] > ${devicename}**
