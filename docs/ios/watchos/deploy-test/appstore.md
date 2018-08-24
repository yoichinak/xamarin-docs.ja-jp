---
title: WatchOS アプリを App Store に展開します。
description: このドキュメントでは、App Store に Xamarin でビルドされた watchOS アプリをデプロイする方法について説明します。 配布プロビジョニング プロファイルと iTunes Connect がかかり、トラブルシューティングのヒントも用意されています。
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 90058f5074759fdded5d259004cb40c0cb7ea212
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276068"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>WatchOS アプリを App Store に展開します。

> [!IMPORTANT]
> 必ず確認[Apple のウォッチ キット送信ガイド](https://developer.apple.com/app-store/watch/)、しを参照してください、[トラブルシューティング](#Troubleshooting)セクションに関する問題がある可能性があります。

- あることを確認します。
  - [**配布プロビジョニング プロファイル**](#provisioning)プロジェクト用に作成します。
  - **配置ターゲット**(`MinimumOSVersion`)、iOS にアプリ設定を親の**8.2**以前のバージョン (8.3 形式がサポートされていません)。

- [ **ITunes Connect**](#iTunes_Connect):

  - IOS アプリのエントリの作成 (または追加を**新しいバージョン**既存のアプリに)。
  - [ウォッチ] アイコンとスクリーン ショットを追加します。

- 次に[Visual Studio for Mac](#xamarin_studio) (Visual Studio は現在サポートされていません)。

  - IOS アプリを右クリックし **スタートアップ プロジェクトとして設定**します。
  - 変更、 **App Store**構成します。
  - 使用して、**アーカイブ**機能は、アプリケーションのアーカイブを作成します。

- 最後に、切り替える[Xcode 6.2 +](#xcode)

  - 移動して、**ウィンドウ > オーガナイザー**選択**アーカイブ**します。
  - 一覧から、アプリケーションとアーカイブを選択します。
  - (省略可能)**を検証しています.** アーカイブします。
  - **送信...** archive and を iTunes にアップロードする手順を確認および承認の接続に従ってください。

特定のヒントを次にこれらの項目に関連するを参照します。 参照してください、[トラブルシューティング](#Troubleshooting)セクションに問題がある場合。

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>配布プロビジョニング プロファイル

作成する必要があります。 App Store 展開用にビルドする、**配布プロビジョニング プロファイル**ソリューション内のアプリ ID ごとにします。

ワイルドカード アプリ ID がある場合*プロビジョニング プロファイルを 1 つだけが必要になります*; が、プロジェクトごとに個別のアプリ ID があるかどうかは、各アプリ ID のプロビジョニング プロファイルを必要があります。

![](appstore-images/provisioningprofile-distribution-sml.png "App Store 配布プロファイル")

次の 3 つのすべてのプロファイルを作成したら、一覧に表示されます。 ダウンロードしてインストールする (それをダブルクリックして)、1 つ注意してください。

![](appstore-images/provisioningprofiles-sml.png "使用可能なプロファイルの一覧")

プロビジョニング プロファイルを確認することができます、**プロジェクト オプション** を選択して、**ビルド > iOS バンドル署名** 画面を選択して、 **AppStore | iPhone**構成します。

**プロビジョニング プロファイル**表示されるすべての一致するプロファイル - このドロップダウン リストで作成した一致するプロファイルを表示する必要があります。

![](appstore-images/options-selectprofile-sml.png "IOS バンドル署名 ダイアログ")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

に従って、[アプリの配布の概要](~/ios/deploy-test/app-distribution/index.md)、具体的には。

- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

ITunes Connect でアプリを構成するときに、[ウォッチ] アイコンとスクリーン ショットを追加していただきます。

![](appstore-images/itunesconnect-watch-sml.png "[ウォッチ] アイコンと iTunes Connect でのスクリーン ショット")

アイコン ファイルは、1024 × 1024 (ピクセル単位) は、循環マスクが表示されたらそれを適用する必要があります。 アイコンには、アルファ チャネルはありません。

少なくとも 1 つのスクリーン ショットが必要です、最大 5 を送信する場合があります。
312 x 390 ピクセルで、Watch アプリの動作をデモンストレーションする必要があります。
42 mm ウォッチ シミュレーターを使用して、このサイズでスクリーン ショットを撮ることができます。


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. IOS アプリがスタートアップ プロジェクトであることを確認します。 ない場合は、右クリックして設定をします。

  ![](appstore-images/xs-startup.png "スタートアップ プロジェクトの設定")

2. 選択、 **AppStore**ビルド構成。

  ![](appstore-images/xs-appstore.png "AppStore のビルド構成")

3. 選択、**ビルド > アーカイブ**アーカイブ プロセスを開始するメニュー項目。

  ![](appstore-images/xs-archive.png "[ビルド] メニュー")

選択することもできます、**ビュー > アーカイブしています.** メニュー項目を以前に作成されたアーカイブを参照してください。

  ![](appstore-images/xs-archives-sml.png "[アーカイブ] ビュー")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode は Visual studio for mac。 作成されたアーカイブを自動的に表示します。

1. Xcode を起動し、選択**ウィンドウ > オーガナイザー**:

  ![](appstore-images/xc-organizer.png "[ウィンドウ] メニュー")

2. 切り替えて、**アーカイブ**タブし、for Mac に Visual Studio で作成されたアーカイブを選択します。

  ![](appstore-images/xc-archives.png "[アーカイブ] タブ")

3. 必要に応じて**を検証しています.** 、アーカイブを選択し、**送信しています.** アプリを iTunes Connect にアップロードします。

4. (複数のいずれかに属している) 場合は、開発チームを選択し、送信を確定します。

  ![](appstore-images/xc-submit1.png "開発チームのセクション")

5. ITunes Connect は、アップロードしたバイナリを表示するには、もう一度アクセスしてください。 アプリの構成 ページに移動し、選択**プレリリース**、上部のメニューから、**ビルド**一覧。

  [![](appstore-images/itc-prerelease-sml.png "ITunes Connect でアプリの構成 ページ")](appstore-images/itc-prerelease.png#lightbox)

承認用のアプリを送信することができますし、**バージョン**ページ。 参照してください、 [iOS アプリの配布の概要](~/ios/deploy-test/app-distribution/index.md)詳細についてはします。


## <a name="troubleshooting"></a>トラブルシューティング

ここでは、App Store、およびそれらを修正するために行える手順を送信中に発生する可能性がいくつかのエラーです。

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>アーカイブのメニュー オプションが Visual Studio for Mac に表示されません。

に従って、[上記の手順](#xamarin_studio)アーカイブ ソリューションを構成します。 スタートアップ プロジェクトを正しく設定することはできない場合、は、ビルド構成はまずデバッグまたはスタートアップ プロジェクトを変更する前にリリースする設定を確認します。 ビルド構成を設定して**AppStore**します。

### <a name="invalid-icon"></a>［無効］アイコン

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

に従って、[アルファ チャネルを削除する手順について](~/ios/watchos/troubleshooting.md)アイコンから。

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion が一致しません

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

-IOS アプリ、ウォッチ拡張機能と、Watch アプリ - ソリューション内のすべてのプロジェクトには、同じバージョン番号を使用する必要があります。 各編集**Info.plist**ファイルにバージョン番号が正確に一致します。

### <a name="missing-icons"></a>アイコンが表示されません。

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

指示に従って、[操作アイコン](~/ios/watchos/app-fundamentals/icons.md)Watch アプリ プロジェクトに必要なすべてのイメージを追加します。

### <a name="missing-icon"></a>アイコンが見つかりません。

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Mac と Visual Studio の最新バージョンがあることを確認、 **AppIcon.appiconset**イメージの完全なセットが含まれています。 このエラーが引き続き発生する場合のソースを表示、 **Contents.json**に必要なすべてのイメージのエントリが含まれていることを確認します。 代わりに、Xamarin の最新バージョンを使用していることを確認したが、いったん削除して、再作成、 **AppIcon.appiconset**します。

> [!IMPORTANT]
> 既知のバグにある Visual Studio for Mac の [ウォッチ] アイコンのサポート: 88 x 88 ピクセルの画像が必要で、 **29x29@3x** (87 x 87 ピクセルとして) 使用するイメージ。


Visual studio for Mac の Xcode 内のイメージ アセットを編集、これを修正または手動で編集することはできません、 **Contents.json**ファイル (一致するように[このサンプル](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132))。



### <a name="invalid-watchkit-support"></a>無効な WatchKit サポート

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

このメッセージが表示される検証と、送信中に、または自動のメールで iTunes Connect から明らかに正常にアップロード後にします。
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> 必要があります**アーカイブ**アプリでは、Visual Studio for Mac と Xcode 6.2 以上に、スイッチを検証して iTunes Connect にアップロードします。


安定した Xamarin のチャネルと Xcode 6.2 + を使用します。



### <a name="invalid-provisioning-profile"></a>無効なプロビジョニング プロファイル

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**配布プロビジョニング プロファイル**watch アプリ ソリューション内のすべての 3 つのプロジェクトを指定する必要があります: iOS アプリ、ウォッチ拡張機能と、Watch アプリのいずれか明示的に (3 つのプロファイル) または 1 つのワイルドカードのプロファイルを使用しています。 プロビジョニング プロファイルが iOS デベロッパー センターに存在して、ダウンロードして mac に追加することが確認します。

### <a name="invalid-code-signing-entitlements"></a>無効なコードの権利を署名

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

プロビジョニング プロファイルは、Apple のデベロッパー センターで正しくセットアップしをダウンロードしてインストールを確認してください。 Mac の各プロジェクトの [プロパティ] ウィンドウを Visual Studio で設定されていることも確認します。

### <a name="invalid-architecture"></a>無効なアーキテクチャ

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Watch アプリを追加することができますのみ[Unified API (64 ビット)](~/cross-platform/macios/unified/index.md) Xamarin.iOS アプリ。
IOS アプリ プロジェクトを右クリックしに移動**オプション > ビルド > iOS ビルド > 詳細設定 タブ**いることを確認し、**サポートされるアーキテクチャ**にはAppStoreiPhoneの構成が含まれます**ARM64** (例。 **Armv 7 + ARM64**)。

### <a name="this-bundle-is-invalid"></a>このバンドルが無効です。

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

親 iOS アプリケーションには、以前に '8.2' 設定 MinimumOSVersion が必要です。

### <a name="non-public-api-usage"></a>非パブリック API の使用方法

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Xcode と Xamarin のツールの最新バージョンを使用していることを確認します。
コードでは、すべての非パブリック Api はアクセスしないでください。

### <a name="build-error-mt5309"></a>ビルド エラー MT5309

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

このエラーは、変更が、Xcode のインストールからの結果は可能性があります。 **Xcode.app**します。 このエラーが発生、インストールされている名前を変更する場合、 **XCode 6.2.app**します。



## <a name="related-links"></a>関連リンク

- [Apple WatchKit 送信ガイド](https://developer.apple.com/app-store/watch/)
