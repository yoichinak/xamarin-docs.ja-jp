---
title: WatchOS アプリを App Store にデプロイする
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリを App Store にデプロイする方法について説明します。 配布プロビジョニングプロファイルと iTunes Connect について説明し、トラブルシューティングのヒントも提供します。
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 7b80573a728e1868254b5a89254ebc385b3baa12
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768076"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>WatchOS アプリを App Store にデプロイする

> [!IMPORTANT]
> [Apple の Watch Kit の送信ガイド](https://developer.apple.com/app-store/watch/)を必ず確認してください。問題が発生した場合は、「[トラブルシューティング](#troubleshooting)」セクションを参照してください。

- 次のことを確認します。
  - プロジェクト用に作成された[**配布プロビジョニングプロファイル**](#provisioning)。
  - **8.2**以前 (8.3`MinimumOSVersion`はサポートされていません) に設定されている iOS の親アプリの**展開ターゲット**()。

- [**ITunes Connect**](#iTunes_Connect)の場合:

  - IOS アプリのエントリを作成します (または、既存のアプリに**新しいバージョン**を追加します)。
  - ウォッチアイコンとスクリーンショットを追加します。

- その後[Visual Studio for Mac](#xamarin_studio)で (Visual Studio は現在サポートされていません)。

  - IOS アプリを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。
  - **App Store**の構成に変更します。
  - **アーカイブ**機能を使用して、アプリケーションアーカイブを作成します。

- 最後に、 [Xcode 6.2 +](#xcode)に切り替えます。

  - **ウィンドウ > オーガナイザー**にアクセスし、 **[アーカイブ]** を選択します。
  - 一覧からアプリケーションとアーカイブを選択します。
  - オプション**検証...** アーカイブ。
  - **送信...** archive and を iTunes にアップロードする手順を確認および承認の接続に従ってください。

以下の項目に関連する特定のヒントを参照してください。 問題が発生した場合は、「[トラブルシューティング](#troubleshooting)」セクションを参照してください。

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>配布プロビジョニングプロファイル

App Store のデプロイ用にビルドするには、ソリューション内の各アプリ ID の**配布プロビジョニングプロファイル**を作成する必要があります。

ワイルドカードアプリ ID がある場合は、*プロビジョニングプロファイルが1つだけ必要*です。ただし、プロジェクトごとに個別のアプリ ID がある場合は、アプリ ID ごとにプロビジョニングプロファイルが必要になります。

![](appstore-images/provisioningprofile-distribution-sml.png "App Store 配布プロファイル")

3つのプロファイルがすべて作成されると、一覧に表示されます。 それぞれのファイルをダブルクリックしてダウンロードし、インストールすることを忘れないでください。

![](appstore-images/provisioningprofiles-sml.png "使用可能なプロファイルの一覧")

プロビジョニング プロファイルを確認することができます、**プロジェクト オプション** を選択して、**ビルド > iOS バンドル署名** 画面を選択して、 **AppStore | iPhone**構成します。

**[プロビジョニングプロファイル]** の一覧には、一致するすべてのプロファイルが表示されます。このドロップダウンリストで作成した一致するプロファイルが表示されます。

![](appstore-images/options-selectprofile-sml.png "IOS バンドル署名ダイアログ")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

アプリの[配布の概要](~/ios/deploy-test/app-distribution/index.md)については、特に次のとおりです。

- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

ITunes Connect でアプリを構成するときは、忘れずにウォッチアイコンとスクリーンショットを追加してください。

![](appstore-images/itunesconnect-watch-sml.png "ITunes Connect の [ウォッチ] アイコンとスクリーンショット")

アイコンファイルは1024x1024 ピクセルである必要があり、表示されると、円マスクが適用されます。 アイコンには、アルファチャネルを含めることはできません。

少なくとも1つのスクリーンショットが必要です。5つまで送信できます。
これらは312x390 ピクセルであり、Watch アプリの動作を示しています。
42 mm watch シミュレーターを使用すると、このサイズでスクリーンショットを撮ることができます。

<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. IOS アプリがスタートアッププロジェクトであることを確認します。 それ以外の場合は、右クリックして設定します。

   ![](appstore-images/xs-startup.png "スタートアッププロジェクトの設定")

2. **Appstore**ビルド構成を選択します。

   ![](appstore-images/xs-appstore.png "AppStore ビルド構成")

3. **[ビルド > アーカイブ]** メニュー項目を選択してアーカイブプロセスを開始します。

   ![](appstore-images/xs-archive.png "[ビルド] メニュー")

また、 **[> アーカイブの表示]** メニュー項目を選択して、以前に作成されたアーカイブを表示することもできます。

  ![](appstore-images/xs-archives-sml.png "アーカイブビュー")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode Visual Studio for Mac で作成されたアーカイブが自動的に表示されます。

1. Xcode を起動し、[**ウィンドウ > オーガナイザー]** を選択します。

   ![](appstore-images/xc-organizer.png "[ウィンドウ] メニュー")

2. **[アーカイブ]** タブに切り替え、Visual Studio for Mac で作成されたアーカイブを選択します。

   ![](appstore-images/xc-archives.png "[アーカイブ] タブ")

3. 必要に応じて、アーカイブを**検証**し、 **[送信...]** を選択してアプリを iTunes Connect にアップロードします。

4. (複数のチームに属している場合は) 開発チームを選択し、送信を確認します。

   ![](appstore-images/xc-submit1.png "開発チームセクション")

5. アップロードしたバイナリを確認するには、iTunes Connect にもう一度アクセスしてください。 アプリの構成ページにアクセスし、上部のメニューから **[プレリリース]** を選択して、**ビルド**の一覧を表示します。

   [![](appstore-images/itc-prerelease-sml.png "ITunes Connect の [アプリの構成] ページ")](appstore-images/itc-prerelease.png#lightbox)

その後、 **[バージョン]** ページで、承認のためにアプリを送信できます。 詳細については、 [「iOS アプリの配布の概要」](~/ios/deploy-test/app-distribution/index.md)を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

ここでは、App Store への送信中に発生する可能性があるエラーと、それらを修正するために実行できる手順を示します。

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>[アーカイブ] メニューオプションが Visual Studio for Mac に表示されない

上記の[手順](#xamarin_studio)に従って、アーカイブするソリューションを構成します。 スタートアッププロジェクトを正しく設定できない場合は、スタートアッププロジェクトを変更する前に、ビルド構成が最初に [デバッグ] または [リリース] に設定されていることを確認します。 次に、ビルド構成を**Appstore**に戻します。

### <a name="invalid-icon"></a>［無効］アイコン

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

アイコンから[アルファチャネルを削除する手順](~/ios/watchos/troubleshooting.md)に従います。

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion の不一致

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

ソリューション内のすべてのプロジェクト (iOS アプリ、Watch 拡張機能、およびウォッチアプリ) は同じバージョン番号を使用する必要があります。 各**情報の plist**ファイルを編集して、バージョン番号が正確に一致するようにします。

### <a name="missing-icons"></a>アイコンがありません

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

「[作業中](~/ios/watchos/app-fundamentals/icons.md)」の手順に従って、必要なすべてのイメージを Watch アプリプロジェクトに追加します。

### <a name="missing-icon"></a>見つからないアイコン

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Visual Studio for Mac の最新バージョンがあることと、 **AppIcon appicons.appiconset**にイメージの完全なセットが含まれていることを確認します。 このエラーが引き続き表示される場合は、 **json**のソースを表示して、必要なすべてのイメージのエントリが含まれていることを確認します。 または、最新バージョンの Xamarin を使用していることを確認したら、 **AppIcon appicons.appiconset**を削除して再作成します。

> [!IMPORTANT]
> Visual Studio for Mac のウォッチアイコンのサポートには既知のバグがあります。これには、 **29x29@3x** イメージに対してサイズが 88 x 88 ピクセルのイメージ (87x87 ピクセル) が必要です。

Xcode のイメージ資産を編集するか、([このサンプル](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)に一致するように) 手動で**ファイルを**編集すること Visual Studio for Mac で、これを修正することはできません。

### <a name="invalid-watchkit-support"></a>無効な WatchKit サポート

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

このメッセージは、検証時および送信時に表示される場合があります。また、アップロードが正常に完了した後の iTunes Connect からの自動電子メールでも表示されます。
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> アプリを Visual Studio for Mac で**アーカイブ**した後、Xcode 6.2 + に切り替えて検証し、iTunes Connect にアップロードする必要があります。

安定した Xamarin チャネルと Xcode 6.2 + を使用します。

### <a name="invalid-provisioning-profile"></a>プロビジョニングプロファイルが無効です

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**配布プロビジョニングプロファイル**は、監視アプリソリューションの3つのプロジェクトすべてに対して指定する必要があります。 iOS アプリ、Watch 拡張機能、および watch アプリ-明示的に (3 つのプロファイル)、または単一のワイルドカードプロファイルを使用します。 プロビジョニングプロファイルが iOS デベロッパーセンターに存在し、Mac にダウンロードして追加されていることを確認します。

### <a name="invalid-code-signing-entitlements"></a>無効なコード署名権利

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

プロビジョニングプロファイルが Apple デベロッパーセンターで正しくセットアップされていること、およびそれらをダウンロードしてインストールしたことを確認します。 また、プロジェクトごとに Visual Studio for Mac の [プロパティ] ウィンドウで設定されていることも確認します。

### <a name="invalid-architecture"></a>無効なアーキテクチャ

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Watch Apps [Unified API (64 ビット)](~/cross-platform/macios/unified/index.md) Xamarin. iOS アプリのみ追加できます。
IOS アプリプロジェクトを右クリックし、[**オプション] > [> Ios ビルド] > [詳細設定] タブ**で、[appstore-iPhone] 構成で**サポートさ**れているアーキテクチャに**ARM64**が含まれていることを確認します (例: **ARMv7 + ARM64**)。

### <a name="this-bundle-is-invalid"></a>このバンドルは無効です。

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

親 iOS アプリケーションでは、MinimumOSVersion が ' 8.2 ' 以前に設定されている必要があります。

### <a name="non-public-api-usage"></a>パブリックでない API の使用

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

最新バージョンの Xcode および Xamarin のツールを使用していることを確認します。
コードでパブリックでない Api にアクセスすることはできません。

### <a name="build-error-mt5309"></a>ビルドエラー MT5309

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

このエラーは、Xcode インストールの名前が**Xcode**から変更されたことが原因である可能性があります。 たとえば、インストールの名前を**XCode**に変更すると、このエラーが発生します。

## <a name="related-links"></a>関連リンク

- [Apple WatchKit 提出ガイド](https://developer.apple.com/app-store/watch/)
