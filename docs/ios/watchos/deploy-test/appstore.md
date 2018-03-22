---
title: "アプリ ストアへの配置"
description: "アプリ ストアへのウォッチ アプリの展開"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c5b89570fdd3df80d39c6621fcd12a23babed9ee
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="deploying-to-the-app-store"></a>アプリ ストアへの配置

> [!IMPORTANT]
> 必ず確認[Apple のウォッチ キット送信ガイド](https://developer.apple.com/app-store/watch/)、しを参照してください、[トラブルシューティング](#Troubleshooting)セクションの問題がある可能性があります。

- あることを確認します。
  - [**プロビジョニング プロファイルの配布**](#provisioning)プロジェクト用に作成します。
  - **配置ターゲット**(`MinimumOSVersion`)、iOS にアプリ設定を親の**8.2**以前のバージョン (8.3 形式がサポートされていません)。

- [ **ITunes Connect**](#iTunes_Connect):

  - IOS のアプリケーション エントリを作成 (または追加、**新しいバージョン**既存のアプリに)。
  - [ウォッチ] アイコンとスクリーン ショットを追加します。

- その後  [Visual Studio for Mac](#xamarin_studio) (Visual Studio は現在サポートされていません)。

  - IOS アプリを右クリックし、**スタートアップ プロジェクトとして設定**です。
  - 変更、 **App Store**構成します。
  - 使用して、**アーカイブ**機能がアプリケーション アーカイブを作成します。

- 最後に、切り替え[Xcode 6.2 +](#xcode)

  - 移動して、**ウィンドウ > オーガナイザー**選択**アーカイブ**です。
  - 一覧からアプリケーションおよびアーカイブを選択します。
  - (省略可能)**を検証しています.**アーカイブします。
  - **送信...** archive and を iTunes にアップロードする手順を確認および承認の接続に従ってください。

次にこれらの項目に関連する特定のヒントを参照します。 参照してください、[トラブルシューティング](#Troubleshooting)セクションの問題がある場合。

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>プロビジョニング プロファイルの配布

App Store の展開を作成する必要がありますを構築する、**分布プロビジョニング プロファイル**ソリューションでは、各アプリ id。

ワイルドカード アプリ ID を使用していれば*プロビジョニング プロファイルを 1 つだけが必要になります*; が、プロジェクトごとに個別のアプリ ID があるかどうかは、各アプリ ID のプロビジョニング プロファイルが必要があります。

![](appstore-images/provisioningprofile-distribution-sml.png "アプリ ストアのディストリビューション プロファイル")

3 つすべてのプロファイルを作成したら、一覧に表示されます。 ダウンロードしてインストール (ダブルクリック) して、それぞれのいずれかに注意してください。

![](appstore-images/provisioningprofiles-sml.png "使用可能なプロファイルの一覧")

プロビジョニング プロファイルを確認することができます、**プロジェクト オプション** を選択して、**ビルド > iOS バンドル署名** 画面を選択して、 **AppStore | iPhone**構成します。

**プロビジョニング プロファイル**リストが一致するすべてのプロファイルを表示する - このドロップダウン リストで作成した一致のプロファイルが表示されます。

![](appstore-images/options-selectprofile-sml.png "IOS バンドル署名 * ダイアログ")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

以下の[アプリの配布の概要](~/ios/deploy-test/app-distribution/index.md)、具体的には。

- [ITunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

を iTunes Connect でアプリを構成するときに忘れずに [ウォッチ] アイコンとスクリーン ショットを追加します。

![](appstore-images/itunesconnect-watch-sml.png "[ウォッチ] アイコンと iTunes Connect でスクリーン ショット")

アイコン ファイル 1024 × 1024 (ピクセル単位) をする必要がありますされ循環マスクが表示されたらそれを適用できます。 アイコンはアルファ チャネルにはできません。

少なくとも 1 つのスクリーン ショットは必須では最大 5 を送信することができます。
312 x 390 ピクセルにして、ウォッチ アプリの動作を示すする必要があります。
このサイズでスクリーン ショットを撮るに 42 mm ウォッチ シミュレーターを使用できます。


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. IOS のアプリがスタートアップ プロジェクトであることを確認します。 ない場合は、右クリックすると、それを設定します。

  ![](appstore-images/xs-startup.png "スタートアップ プロジェクトの設定")

2. 選択、 **AppStore**構成をビルドします。

  ![](appstore-images/xs-appstore.png "AppStore ビルド構成")

3. 選択、**ビルド > アーカイブ**アーカイブ プロセスを開始するメニュー項目。

  ![](appstore-images/xs-archive.png "[ビルド] メニュー")

選択することもできます、**ビュー > アーカイブしています.**メニュー項目を以前に作成されているアーカイブを参照してください。

  ![](appstore-images/xs-archives-sml.png "アーカイブ ビュー")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode は、for mac を Visual Studio で作成するアーカイブを自動的に表示されます。

1. Xcode を起動し、選択**ウィンドウ > オーガナイザー**:

  ![](appstore-images/xc-organizer.png "[ウィンドウ] メニュー")

2. 切り替えて、**アーカイブ**タブし、Mac を Visual Studio で作成されたアーカイブ を選択します。

  ![](appstore-images/xc-archives.png "[アーカイブ] タブ")

3. 必要に応じて**を検証しています.** 、アーカイブを選択し、**送信しています.** iTunes Connect にアプリをアップロードします。

4. (複数のいずれかに属している) 場合は、開発チームを選択し、送信を確定します。

  ![](appstore-images/xc-submit1.png "開発チームのセクション")

5. ITunes アップロードされたバイナリを参照してください。 もう一度への接続を参照してください。 アプリの構成 ページに移動し、選択**プレリリース**して、上部のメニューから、**ビルド**一覧。

  [![](appstore-images/itc-prerelease-sml.png "ITunes Connect でアプリの構成 ページ")](appstore-images/itc-prerelease.png#lightbox)

承認するようにアプリケーションを送信することができますし、**バージョン**ページ。 参照してください、 [iOS アプリの配布の概要](~/ios/deploy-test/app-distribution/index.md)詳細についてはします。


## <a name="troubleshooting"></a>トラブルシューティング

ここでは、いくつかのエラーが App Store、およびそれらを修正するために行える手順への送信中に発生する可能性があります。

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>アーカイブのメニュー オプションは Visual Studio for Mac に表示されません。

以下の[上記の手順](#xamarin_studio)をアーカイブするためのソリューションを構成します。 スタートアップ プロジェクトを正しく設定することはできない場合、は、ビルド構成が最初にデバッグやリリース スタートアップ プロジェクトを変更する前に設定されていることを確認します。 ビルド構成のバックアップを設定して**AppStore**です。

### <a name="invalid-icon"></a>［無効］アイコン

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

以下の[アルファ チャネルを削除する手順について](~/ios/watchos/troubleshooting.md)アイコンからです。

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion が一致しません

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

-IOS アプリ、ウォッチ拡張機能および Watch アプリ - ソリューション内のすべてのプロジェクトは、同じバージョン番号を使用している必要があります。 各編集**Info.plist**ファイルにバージョン番号が正確に一致します。

### <a name="missing-icons"></a>アイコンがありません。

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

指示に従って、[アイコンの操作](~/ios/watchos/app-fundamentals/icons.md)Watch アプリ プロジェクトに必要なすべてのイメージを追加します。

### <a name="missing-icon"></a>アイコンがありません。

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Mac 用 Visual Studio の最新バージョンであることを確認してください、 **AppIcons.appiconset**イメージの完全なセットが含まれています。 このエラーが引き続き表示される場合のソースを表示、 **Contents.json**に必要なすべてのイメージのエントリが含まれていることを確認します。 代わりに、Xamarin の最新バージョンを使用していることを確認するが、いったん削除して、再作成、 **AppIcons.appiconset**です。

> [!IMPORTANT]
> ある既知のバグを Visual Studio での Mac のウォッチ アイコンのサポート: 88 x 88 ピクセル イメージのことが必要ですが、  **29x29@3x** イメージ 87 x 87 ピクセルにする必要があります)。


Mac で Xcode でのイメージ アセットを編集いずれかの Visual Studio でこれを修正または手動で編集することはできません、 **Contents.json**ファイル (一致するように[このサンプル](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132))。



### <a name="invalid-watchkit-support"></a>無効な WatchKit サポート

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

このメッセージが表示される検証と、送信時または自動の電子メールで iTunes Connect から明らかに正常にアップロードされた後です。
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> 必要な**アーカイブ**Mac しに切り替えると Xcode 6.2 + を検証して、iTunes Connect へのアップロード用の Visual Studio でのアプリです。


安定した Xamarin チャネルと Xcode 6.2 + を使用します。



### <a name="invalid-provisioning-profile"></a>無効なプロビジョニング プロファイル

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**プロビジョニング プロファイルの配布**watch アプリのソリューション内のすべての 3 つのプロジェクトを指定する必要があります: iOS アプリ、ウォッチ拡張機能および Watch アプリのいずれかに明示的に (3 つのプロファイル) またはワイルドカードを 1 つのプロファイルを使用しています。 プロビジョニング プロファイルを iOS デベロッパー センターに存在して、ダウンロードしている mac に追加を確認します。

### <a name="invalid-code-signing-entitlements"></a>無効なコードの権利を署名

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

プロビジョニング プロファイルに、Apple のデベロッパー センターに正しくセットアップされており、ダウンロードおよびインストールことを確認します。 また、Mac の各プロジェクトの [プロパティ] ウィンドウを Visual Studio で設定されているを確認します。

### <a name="invalid-architecture"></a>無効なアーキテクチャ

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Watch アプリを追加することができますのみ[Unified API (64 ビット)](~/cross-platform/macios/unified/index.md) Xamarin.iOS アプリ。
IOS アプリ プロジェクトを右クリックしに移動し、**オプション > ビルド > iOS ビルド > [詳細] タブ**ことを確認して、**アーキテクチャのサポートされている**にはAppStoreiPhoneの構成が含まれます**ARM64**などです。 **常に ARMv7 + ARM64**)。

### <a name="this-bundle-is-invalid"></a>このバンドルが正しくありません。

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

親 iOS アプリケーションには、'8.2' またはそれ以前の設定 MinimumOSVersion 必要があります。

### <a name="non-public-api-usage"></a>非パブリック API の使用方法

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Xcode および Xamarin のツールの最新バージョンを使用していることを確認します。
コードでは、すべての非パブリック Api はアクセスしないでください。

### <a name="build-error-mt5309"></a>ビルド エラー MT5309

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

このエラーは、ある名前を変更から、Xcode インストールの結果である可能性があります**Xcode.app**です。 インストールの名前を変更する場合にこのエラーが発生するインスタンスの**XCode 6.2.app**です。



## <a name="related-links"></a>関連リンク

- [Apple WatchKit 送信ガイド](https://developer.apple.com/app-store/watch/)
