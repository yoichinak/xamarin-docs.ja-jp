---
title: Apple Watch デバイスでのテスト
description: このドキュメントでは、実際の Apple Watch でテストを行うために、Xamarin でビルドされた watchOS アプリをデプロイする方法について説明します。 デバイス、プロビジョニングプロファイル、テストについて説明し、トラブルシューティングのヒントを提供します。
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c049fb0bd05749db30d99603fb9179e710f815f7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028354"
---
# <a name="testing-on-apple-watch-devices"></a>Apple Watch デバイスでのテスト

[デプロイの手順](~/ios/watchos/deploy-test/index.md)に従ってアプリ Id とアプリグループを作成したら (必要な場合)、このページの手順に従って次の操作を行います。

- Apple デベロッパーセンターで[デバイスをセットアップ](#devices)します。
- [開発プロビジョニングプロファイルを作成](#profiles)し、
- Apple Watch に[配置してテスト](#testing)します。

<a name="devices" />

## <a name="devices"></a>デバイス

実際の iPhone または iPad で iOS アプリをテストするには、常にデベロッパーセンターにデバイスを登録する必要があります。 デバイスの一覧は次のようになります (新しいデバイスを追加するには、プラス記号 **+** をクリックします)。

![](device-images/devices-sml.png "The device list looks like this")

監視は異なります。アプリを展開する前に、Apple Watch デバイスを追加する必要があります。 **Xcode** (**Windows > Devices** list) を使用して、ウォッチの udid を検索します。 ペアリングされた電話が接続されると、ウォッチの情報も表示されます。

[![](device-images/xcode-devices-sml.png "Paired Watch Information")](device-images/xcode-devices.png#lightbox)

ウォッチの UDID がわかったら、デベロッパーセンターのデバイスの一覧に追加します。

![](device-images/devices-watch-sml.png "The Watch's UDID in the device list")

監視デバイスを追加したら、新規または既存の開発またはカスタムプロビジョニングプロファイルで作成したものを選択します。

![](device-images/devices-provisioning.png "Available device list")

既存のプロビジョニングプロファイルを編集してダウンロードして再インストールするかどうかを忘れないでください。

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>開発プロビジョニングプロファイル

デバイスでテスト用にビルドするには、ソリューション内の各アプリ ID の**開発プロビジョニングプロファイル**を作成する必要があります。

ワイルドカードアプリ ID がある場合は、*プロビジョニングプロファイルが1つだけ必要*です。ただし、プロジェクトごとに個別のアプリ ID がある場合は、アプリ ID ごとにプロビジョニングプロファイルが必要になります。

![](device-images/provisioningprofile-development.png "The Development Provisioning Profile")

3つのプロファイルがすべて作成されると、一覧に表示されます。 各ファイルをダウンロードしてインストールすることを忘れないでください。

![](device-images/provisioningprofiles.png "The available Development Provisioning Profiles")

**プロジェクトオプション**でプロビジョニングプロファイルを確認するには、 **[ビルド > iOS バンドル署名]** 画面を選択し、 **[Release]** または **[Debug iPhone]** 構成を選択します。

**[プロビジョニングプロファイル]** の一覧には、一致するすべてのプロファイルが表示されます。このドロップダウンリストで作成した一致するプロファイルが表示されます。

![](device-images/options-selectprofile.png "The Provisioning Profile list")

<a name="testing" />

## <a name="testing-on-a-watch-device"></a>監視デバイスでのテスト

デバイス、アプリ Id、プロビジョニングプロファイルを構成すると、テストする準備が整います。

1. IPhone が接続されていて、ウォッチが既に iPhone とペアリングされていることを確認します。

2. 構成が**Release**または**Debug**に設定されていることを確認します。

3. [ターゲット] ボックスの一覧で、接続されている iPhone デバイスが選択されていることを確認します。

4. (ウォッチまたは拡張機能ではなく) iOS アプリプロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。

5. **[実行]** ボタンをクリックします (または、 **[実行]** メニューの **[開始]** オプションを選択します)。

6. ソリューションがビルドされ、iOS アプリが iPhone にデプロイされます。
  IOS アプリまたは watch 拡張機能のプロビジョニングが正しく設定されていない場合、iPhone への展開は失敗します。

7. 配置が正常に完了した場合、iPhone は、ペアになっている Watch に自動的に watch アプリを送信しようとします。 アプリアイコンが、[ウォッチ] 画面に円形の*インストール*進行状況インジケーターと共に表示されます。

8. Watch アプリが正常にインストールされている場合、このアイコンは、アプリのテストを開始するために [ウォッチ] 画面に表示されます。

## <a name="troubleshooting"></a>トラブルシューティング

デプロイ中にエラーが発生した場合は、**ビュー > 埋め込み > デバイスログに埋め込ま**れ、エラーに関する詳細情報が表示されます。 いくつかのエラーとその原因を以下に示します。

### <a name="error-mt3001-could-not-aot-the-assembly"></a>エラー MT3001: アセンブリを AOT にできませんでした

これは、Apple Watch デバイスに配置するためにデバッグモードでビルドするときに発生する可能性があります。

この問題を*一時的*に回避するには、[ウォッチ拡張機能]**プロジェクトオプション > [ビルド > watchOS ビルド**] ウィンドウで**インクリメンタルビルド**を無効にします。

[![](device-images/disable-incremental-sml.png "The Incremental Builds checkbox")](device-images/disable-incremental.png#lightbox)

これは今後のリリースで修正される予定です。これにより、インクリメンタルビルドを再度有効にして、より高速なビルド時間を利用することができます。

### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>デバイスでのデバッグ中に Watch アプリを起動できない

物理デバイスで watch アプリをデバッグしようとすると、アイコン & の読み込みスピンボタンだけが表示されます (最終的にはタイムアウトになります)。 これについては、今後のリリースで対処されます。回避策としては、デバッグを許可しないリリースビルドを実行する方法があります。

### <a name="invalid-application-executable-or-application-verification-failed"></a>無効なアプリケーション実行可能ファイルまたはアプリケーションの検証に失敗しました

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Invalid Application Executable alert")

アプリをインストールしようとした後に、これらのメッセージが *[ウォッチ] 画面に*表示される場合は、いくつかの問題が考えられます。

- ウォッチデバイス自体は、Apple デベロッパーセンターにデバイスとして追加されていません。 指示に従って、[デバイスを正しく構成](#devices)します。

- テストに使用されている開発プロビジョニングプロファイルに、監視デバイスが含まれていませんでした。または、監視がプロビジョニングプロファイルに追加された後、再ダウンロードされて再インストールされませんでした。 指示に従って、[プロビジョニングプロファイルを正しく構成](#profiles)します。

- **IOS デバイスログ**に `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` が含まれている場合、Watch アプリの**MinimumOSVersion**の**値が正しく**ありません。
  これは**8.2**である必要があります。 Xcode 6.3 がインストールされている場合は、挿入するソースを手動で編集して8.2 に設定する必要がある場合があります。

- Watch アプリの**権利**(アプリグループなど) が正しく設定されていません。

- Watch アプリの**アプリ ID**が、デベロッパーセンターで使用できない権利 (アプリグループなど) を正しく設定していません。

### <a name="install-never-finished"></a>インストールが完了していません

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

このエラーは、Watch アプリの**情報の plist**ファイルで、不要な (および無効な) キーを示している可能性があります。 IOS アプリまたは watch 拡張機能用のキーを Watch アプリに含めないでください。

<!--eg. NSLocationAlwaysUsageDescription -->

### <a name="waiting-for-debugger-to-connect"></a>"デバッガーが接続するのを待機しています"

**[アプリケーション出力]** ウィンドウにスタックが表示されない場合

```csharp
waiting for debugger to connect
```

プロジェクトに含まれている Nuget のいずれかが、 **Microsoft の Bcl. ビルド**に依存しているかどうかを確認します。 これは、一般的な[Microsoft Http クライアントライブラリ](https://www.nuget.org/packages/Microsoft.Net.Http/)を含む一部の microsoft 公開ライブラリと共に自動的に追加されます。

**.Csproj**に追加され**たファイルは**、展開時に iOS 拡張機能のパッケージ化に干渉する可能性があります。 [バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)を追跡することができます。
回避策として、.csproj ファイルを編集し、**最後の要素になるように**手動で変更します。
