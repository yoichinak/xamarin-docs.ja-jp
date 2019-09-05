---
title: watchOS Xamarin での通知
description: このドキュメントでは、Xamarin で watchOS 通知と連携する方法について説明します。 作成通知コント ローラー、通知の生成と通知のテストがについて説明します。
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 9ca50171e43ef98e5b4e5fbd7bd236f74d35da8f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286763"
---
# <a name="watchos-notifications-in-xamarin"></a>watchOS Xamarin での通知

Watch アプリは、親 iOS アプリには、それらがサポートしている場合、通知を受け取ることができます。 組み込みの通知を処理しないように*必要*ただし以下に示す追加の通知のサポートを追加する通知の動作をカスタマイズしての外観を読み取る場合です。

参照してください、 [iOS 通知](~/ios/platform/user-notifications/deprecated/index.md)ソリューションで iOS アプリに通知のサポートを追加する方法についてのドキュメント。

## <a name="creating-notification-controllers"></a>通知コント ローラーを作成します。

通知コント ローラーではストーリー ボード上のトリガーとなったセグエの特別な種類があります。 新しいをドラッグすると**通知インターフェイス コント ローラー**アタッチ セグエが自動的があるストーリー ボード上に。

![](notifications-images/notification-storyboard1.png "接続されているセグエを使用には、新しい通知インターフェイス コント ローラー")

選択されている通知のセグエとそのプロパティを編集することができます。

![](notifications-images/notification-storyboard2.png "通知は選択したセグエします。")

コント ローラーをカスタマイズした後、WatchKitCatalog からこの例のように見えます。

![](notifications-images/notifications-segue.png "通知のプロパティ")


2 つの種類の通知があります。

- システムによって定義されて**いる、スクロール不可能な静的**ビューです。

- **時間の長い外観**- スクロール可能なカスタマイズ可能なビューが定義しました。 単純化し、静的バージョンより複雑な動的バージョンを指定することができます。

### <a name="short-look-notification-controller"></a>短い表示通知コント ローラー

短い外観 UI は、アプリのアイコン、アプリ名、および通知のタイトル文字列で構成されます。

ユーザーが通知を無視しない場合は、システムが情報を提供する時間の長い確認通知を自動的に切り替わります。


### <a name="long-look-notification-controller"></a>通知コント ローラーの時間の長い検索

OS は、さまざまな要因に基づく静的または動的なビューを表示するかどうかを決定します。 静的のインターフェイスを提供し、必要に応じてことができます必要がありますも通知の動的なインターフェイスが含まれます。

#### <a name="static"></a>スタティック

静的なビューには、簡単かつ迅速に表示があります。

![](notifications-images/notification-static.png "静的なビュー")

#### <a name="dynamic"></a>動的

動的ビューより多くのデータを表示でき、他のインタラクティビティを提供することができます。

![](notifications-images/notification-dynamic.png "動的ビュー")


## <a name="generating-notifications"></a>通知を生成します。

通知は、リモート サーバーから取得できます ([Apple プッシュ通知サービス](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)、または APNS) または iOS アプリでローカルで生成されたことができます。

参照してください、 [iOS 通知のチュートリアル](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md)ローカル通知を生成する方法の例については、 [WatchNotifications サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)実施例についてはします。

ローカル通知する必要がありますが、 `AlertTitle` Apple Watch に表示される設定、`AlertTitle`文字列が短い検索インターフェイスに表示されます。 両方、`AlertTitle`と`AlertBody`通知の一覧に表示されると、`AlertBody`時間の長い検索インターフェイスに表示されます。

このスクリーン ショットは、`AlertTitle`通知の一覧に表示されていると、`AlertBody`時間の長い検索インターフェイスに表示されます (を使用して、[サンプル コード](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications))。

![](notifications-images/watch-notificationslist-sml.png "このスクリーン ショットは、通知リストに表示されている AlertTitle を示しています") ![](notifications-images/watch-notificationcontroller-sml.png "時間の長い外観インターフェイスに表示される AlertBody。")

## <a name="testing-notifications"></a>テスト通知

通知 (ローカルおよびリモート) 正しくをテストするためのデバイスでを使用してシミュレートできますが、 **.json** iOS シミュレーターでのファイル。

### <a name="testing-on-apple-watch"></a>Apple Watch でのテスト

Apple Watch に通知をテストする場合の点に注意[Apple のドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)次の状態します。

> ユーザーの iphone アプリのローカルまたはリモート通知のいずれかが到着すると、iOS は、iPhone または Apple Watch に通知を表示するかどうかを決定します。

という事実にほのめかしこれは、iOS は、iphone、または Watch で通知が表示されるかどうかを決定します。 ペアになっている iPhone がアクティブな通知を受信したときに、通知は、iPhone で表示される可能性と*いない*ウォッチにルーティングします。

Watch で通知が表示されることを確認するには、するには、(1 回の電源ボタンを押す) iPhone 画面をオフにかスリープ状態にそのまま使用します。 ペアになっているウォッチが範囲内にするには、電源が入っておよび手首に装着されている、通知がルーティングされ、(によって、わかりにくいことを義務付けられて) ウォッチに表示します。

### <a name="testing-on-the-ios-simulator"></a>IOS シミュレーターでのテスト

*する必要があります*iOS シミュレーターで通知モードをテストするときに、テストの JSON ペイロードを提供します。 パスを設定、**カスタム実行引数**Visual Studio for mac でのウィンドウ

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac と watch extension を設定すると追加のオプションが表示されます、**スタートアップ プロジェクト**します。
ウォッチ拡張機能プロジェクトを右クリックし、選択**実行 > カスタム パラメーター.** :

[![](notifications-images/runwith-customparams-sml.png "カスタム プロパティを持つ実行中")](notifications-images/runwith-customparams.png#lightbox)

開き、**実行引数**ウィンドウが含まれていますが、 **WatchKit**タブ。選択**通知**、JSON ペイロードを提供し、キーを押します**Execute** watch アプリをシミュレーターで起動します。

[![](notifications-images/runwith-execargs-sml.png "通知ペイロードの既定値を選択します。")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

ウォッチ拡張機能の編集を Visual Studio の右クリックでテスト通知のペイロードを設定する、**プロジェクト プロパティ**します。 移動して、**デバッグ**セクションし、(これは自動的に一覧表示、プロジェクトに含まれるすべての JSON ファイル) の一覧から通知の JSON ファイルを選択します。

[![](notifications-images/runwith-execargs-sml-vs.png "通知の JSON ファイルを選択します。")](notifications-images/runwith-execargs-vs.png#lightbox)

ウォッチ拡張機能の場合、**スタートアップ プロジェクト**、Visual Studio は、次に示す追加のオプションに表示されます。 いずれかの選択、**通知**で watch アプリを起動するオプション**通知**モード ([プロパティ] ウィンドウで選択されている JSON ファイルを使用)。

![](notifications-images/runwith-vs.png "デバイス メニュー")

-----

既定の通知コント ローラーは、既定のペイロードの JSON ファイルを使用して、シミュレーターでテストするときに、ようになります。

![](notifications-images/notification-debug-sml.png "例の通知")

使用することも、[コマンドライン](~/ios/watchos/troubleshooting.md#command_line)iOS シミュレーターを起動します。

### <a name="example-notification-payload"></a>通知ペイロードの例

[ウォッチ キット カタログ](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルがありますが、サンプル ペイロードの JSON ファイル**NotificationPayload.json** (下記参照)。

```json
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>関連リンク

- [WatchNotifications (ローカル通知) (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)
- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のウォッチ キット通知 docs](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
