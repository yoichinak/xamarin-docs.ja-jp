---
title: WatchOS アプリを App Store にデプロイする
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリを App Store にデプロイする方法について説明します。 配布プロビジョニングプロファイルと iTunes Connect について説明し、トラブルシューティングのヒントも提供します。
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 1581a58d9a6851ad880d2631660e261685260e40
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932796"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>WatchOS アプリを App Store にデプロイする

> [!IMPORTANT]
> [Apple の Watch Kit の送信ガイド](https://developer.apple.com/app-store/watch/)を必ず確認してください。問題が発生した場合は、「[トラブルシューティング](#troubleshooting)」セクションを参照してください。

- 次のことを確認します。
  - プロジェクト用に作成された[**配布プロビジョニングプロファイル**](#provisioning)。
  - **Deployment Target** `MinimumOSVersion` **8.2**以前 (8.3 はサポートされていません) に設定されている IOS の親アプリの展開ターゲット ()。

- [**ITunes Connect**](#iTunes_Connect)の場合:

  - IOS アプリのエントリを作成します (または、既存のアプリに**新しいバージョン**を追加します)。
  - ウォッチアイコンとスクリーンショットを追加します。

- その後[Visual Studio for Mac](#xamarin_studio)で (Visual Studio は現在サポートされていません)。

  - IOS アプリを右クリックし、[**スタートアッププロジェクトに設定**] を選択します。
  - **App Store**の構成に変更します。
  - **アーカイブ**機能を使用して、アプリケーションアーカイブを作成します。

- 最後に、 [Xcode 6.2 +](#xcode)に切り替えます。

  - **ウィンドウ > オーガナイザー**にアクセスし、[**アーカイブ**] を選択します。
  - 一覧からアプリケーションとアーカイブを選択します。
  - オプション**検証...** アーカイブ。
  - **送信...** 確認と承認のために、アーカイブし、iTunes Connect にアップロードする手順に従います。

以下の項目に関連する特定のヒントを参照してください。 問題が発生した場合は、「[トラブルシューティング](#troubleshooting)」セクションを参照してください。

<a name="provisioning"></a>

## <a name="distribution-provisioning-profiles"></a>配布プロビジョニングプロファイル

App Store のデプロイ用にビルドするには、ソリューション内の各アプリ ID の**配布プロビジョニングプロファイル**を作成する必要があります。

ワイルドカードアプリ ID がある場合は、*プロビジョニングプロファイルが1つだけ必要*です。ただし、プロジェクトごとに個別のアプリ ID がある場合は、アプリ ID ごとにプロビジョニングプロファイルが必要になります。

![App Store 配布プロファイル](appstore-images/provisioningprofile-distribution-sml.png)

3つのプロファイルがすべて作成されると、一覧に表示されます。 それぞれのファイルをダブルクリックしてダウンロードし、インストールすることを忘れないでください。

![使用可能なプロファイルの一覧](appstore-images/provisioningprofiles-sml.png)

**プロジェクトオプション**でプロビジョニングプロファイルを確認するには、[**ビルド > iOS バンドル署名**] 画面を選択し、[ **appstore | iPhone** ] 構成を選択します。

[**プロビジョニングプロファイル**] の一覧には、一致するすべてのプロファイルが表示されます。このドロップダウンリストで作成した一致するプロファイルが表示されます。

![IOS バンドル署名ダイアログ](appstore-images/options-selectprofile-sml.png)

<a name="iTunes_Connect"></a>

## <a name="itunes-connect"></a>iTunes Connect

アプリの[配布の概要](~/ios/deploy-test/app-distribution/index.md)については、特に次のとおりです。

- [ITunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

ITunes Connect でアプリを構成するときは、忘れずにウォッチアイコンとスクリーンショットを追加してください。

![ITunes Connect の [ウォッチ] アイコンとスクリーンショット](appstore-images/itunesconnect-watch-sml.png)

アイコンファイルは1024x1024 ピクセルである必要があり、表示されると、円マスクが適用されます。 アイコンには、アルファチャネルを含めることはできません。

少なくとも1つのスクリーンショットが必要です。5つまで送信できます。
これらは312x390 ピクセルであり、Watch アプリの動作を示しています。
42 mm watch シミュレーターを使用すると、このサイズでスクリーンショットを撮ることができます。

<a name="xamarin_studio"></a>

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. IOS アプリがスタートアッププロジェクトであることを確認します。 それ以外の場合は、右クリックして設定します。

   ![スタートアッププロジェクトの設定](appstore-images/xs-startup.png)

2. **Appstore**ビルド構成を選択します。

   ![AppStore ビルド構成](appstore-images/xs-appstore.png)

3. [**ビルド > アーカイブ**] メニュー項目を選択してアーカイブプロセスを開始します。

   ![[ビルド] メニュー](appstore-images/xs-archive.png)

また、[ **> アーカイブの表示**] メニュー項目を選択して、以前に作成されたアーカイブを表示することもできます。

  ![アーカイブビュー](appstore-images/xs-archives-sml.png)

<a name="xcode"></a>

## <a name="xcode"></a>Xcode

Xcode Visual Studio for Mac で作成されたアーカイブが自動的に表示されます。

1. Xcode を起動し、[**ウィンドウ > オーガナイザー]** を選択します。

   ![[ウィンドウ] メニュー](appstore-images/xc-organizer.png)

2. [**アーカイブ**] タブに切り替え、Visual Studio for Mac で作成されたアーカイブを選択します。

   ![[アーカイブ] タブ](appstore-images/xc-archives.png)

3. 必要に応じて、アーカイブを**検証**し、[**送信...** ] を選択してアプリを iTunes Connect にアップロードします。

4. (複数のチームに属している場合は) 開発チームを選択し、送信を確認します。

   ![開発チームセクション](appstore-images/xc-submit1.png)

5. アップロードしたバイナリを確認するには、iTunes Connect にもう一度アクセスしてください。 アプリの構成ページにアクセスし、上部のメニューから [**プレリリース**] を選択して、**ビルド**の一覧を表示します。

   [![ITunes Connect の [アプリの構成] ページ](appstore-images/itc-prerelease-sml.png)](appstore-images/itc-prerelease.png#lightbox)

その後、[**バージョン**] ページで、承認のためにアプリを送信できます。 詳細については、 [「iOS アプリの配布の概要」](~/ios/deploy-test/app-distribution/index.md)を参照してください。

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

Visual Studio for Mac の最新バージョンがあることと、 **AppIcon appicons.appiconset**にイメージの完全なセットが含まれていることを確認します。 このエラーが引き続き表示される場合は、 **Contents.js**のソースを表示して、必要なすべてのイメージのエントリが含まれていることを確認します。 または、最新バージョンの Xamarin を使用していることを確認したら、 **AppIcon appicons.appiconset**を削除して再作成します。

> [!IMPORTANT]
> Visual Studio for Mac のウォッチアイコンのサポートには既知のバグがあります。これには、イメージに対してサイズが 88 x 88 ピクセルのイメージ **29x29@3x** (87x87 ピクセル) が必要です。

Xcode のイメージ資産を編集するか、ファイル**のContents.js**を手動で編集する Visual Studio for Mac では、この問題を修正することはできません。

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
