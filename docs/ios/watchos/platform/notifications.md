---
title: watchOS Xamarin での通知
description: このドキュメントでは、Xamarin で watchOS 通知と連携する方法について説明します。 作成通知コント ローラー、通知の生成と通知のテストがについて説明します。
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 85a55967446da5cf89e8ce19dadf88d0de16d80a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725070"
---
# <a name="watchos-notifications-in-xamarin"></a>watchOS Xamarin での通知

Watch アプリは、親 iOS アプリには、それらがサポートしている場合、通知を受け取ることができます。 通知の動作と外観をカスタマイズする場合は、次に示す追加の通知のサポートを追加する*必要*はありませんが、通知の処理が組み込まれています。

ソリューションの iOS アプリに通知サポートを追加する方法の詳細については、 [ios の通知](~/ios/platform/user-notifications/deprecated/index.md)ドキュメントを参照してください。

## <a name="creating-notification-controllers"></a>通知コント ローラーを作成します。

通知コント ローラーではストーリー ボード上のトリガーとなったセグエの特別な種類があります。 新しい**Notification Interface コントローラー**をストーリーボードにドラッグすると、自動的にセグエが添付されます。

![](notifications-images/notification-storyboard1.png "A new Notification Interface Controller with a segue attached")

選択されている通知のセグエとそのプロパティを編集することができます。

![](notifications-images/notification-storyboard2.png "The notification segue selected")

コント ローラーをカスタマイズした後、WatchKitCatalog からこの例のように見えます。

![](notifications-images/notifications-segue.png "The Notification Properties")

2 つの種類の通知があります。

- システムによって定義されて**いる、スクロール不可能な静的**ビューです。

- **長い外観**で、カスタマイズ可能なビューが定義されています。 単純化し、静的バージョンより複雑な動的バージョンを指定することができます。

### <a name="short-look-notification-controller"></a>短い表示通知コント ローラー

短い外観 UI は、アプリのアイコン、アプリ名、および通知のタイトル文字列で構成されます。

ユーザーが通知を無視しない場合は、システムが情報を提供する時間の長い確認通知を自動的に切り替わります。

### <a name="long-look-notification-controller"></a>通知コント ローラーの時間の長い検索

OS は、さまざまな要因に基づく静的または動的なビューを表示するかどうかを決定します。 静的のインターフェイスを提供し、必要に応じてことができます必要がありますも通知の動的なインターフェイスが含まれます。

#### <a name="static"></a>静的

静的なビューには、簡単かつ迅速に表示があります。

![](notifications-images/notification-static.png "The static view")

#### <a name="dynamic"></a>動的

動的ビューより多くのデータを表示でき、他のインタラクティビティを提供することができます。

![](notifications-images/notification-dynamic.png "The dynamic view")

## <a name="generating-notifications"></a>通知を生成します。

通知は、リモートサーバーから取得することも、iOS アプリでローカルに生成することもできます。

ローカル通知を生成する方法の例については、 [iOS の通知](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md)に関するチュートリアルを参照してください。

ローカル通知では、`AlertTitle` が Apple Watch に表示されるように設定されている必要があります。 `AlertTitle` 文字列は、ショート表示インターフェイスに表示されます。 `AlertTitle` と `AlertBody` の両方が [通知] の一覧に表示されます。また、`AlertBody` が長い外観のインターフェイスに表示されます。

このスクリーンショットは、[通知] の一覧に表示されている `AlertTitle` と、長い外観のインターフェイスに表示されている `AlertBody` を示しています。

![](notifications-images/watch-notificationslist-sml.png "このスクリーンショットは、通知の一覧に表示されている AlertTitle を示しています。")![](notifications-images/watch-notificationcontroller-sml.png "長い外観のインターフェイスに表示される AlertBody")

## <a name="testing-notifications"></a>テスト通知

通知 (ローカルとリモートの両方) は、デバイスでのみ適切にテストできます。ただし、iOS シミュレーターでは、 **json**ファイルを使用してシミュレートできます。

### <a name="testing-on-apple-watch"></a>Apple Watch でのテスト

Apple Watch で通知をテストするときは、 [Apple のドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)に次のような内容が記載されていることに注意してください。

> ユーザーの iphone アプリのローカルまたはリモート通知のいずれかが到着すると、iOS は、iPhone または Apple Watch に通知を表示するかどうかを決定します。

という事実にほのめかしこれは、iOS は、iphone、または Watch で通知が表示されるかどうかを決定します。 通知を受信したときに、ペアになっている iPhone がアクティブになっている場合、通知は iPhone に表示される可能性があり、ウォッチにはルーティングされ*ません*。

Watch で通知が表示されることを確認するには、するには、(1 回の電源ボタンを押す) iPhone 画面をオフにかスリープ状態にそのまま使用します。 ペアになっているウォッチが範囲内にするには、電源が入っておよび手首に装着されている、通知がルーティングされ、(によって、わかりにくいことを義務付けられて) ウォッチに表示します。

### <a name="testing-on-the-ios-simulator"></a>IOS シミュレーターでのテスト

IOS シミュレーターで通知モードをテストするときは、テスト用の JSON ペイロードを指定する*必要があり*ます。 Visual Studio for Mac の **[カスタム実行引数]** ウィンドウでパスを設定します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[ウォッチ] 拡張が**スタートアッププロジェクト**として設定されている場合、Visual Studio for Mac には追加のオプションが表示されます。
Watch extension プロジェクトを右クリックし、[ **> のカスタムパラメーターを使用して実行**する] を選択します。

[![](notifications-images/runwith-customparams-sml.png "Running with Custom Properties")](notifications-images/runwith-customparams.png#lightbox)

**[実行引数]** ウィンドウが開き、 **[WatchKit]** タブが表示されます。 **[通知]** を選択し、JSON ペイロードを指定してから、 **[実行]** を押してシミュレーターで watch アプリを起動します。

[![](notifications-images/runwith-execargs-sml.png "Select Notification Payload Default")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio でテスト通知ペイロードを設定するには、ウォッチ拡張機能を右クリックして**プロジェクトのプロパティ**を編集します。 **[デバッグ]** セクションにアクセスし、一覧から通知 json ファイルを選択します (プロジェクトに含まれるすべての json ファイルが自動的に一覧表示されます)。

[![](notifications-images/runwith-execargs-sml-vs.png "Select a notifications JSON file")](notifications-images/runwith-execargs-vs.png#lightbox)

ウォッチ拡張機能が**スタートアッププロジェクト**の場合、Visual Studio では次のように追加のオプションが表示されます。 **通知オプションの**1 つを選択して、**通知**モードでウォッチアプリを起動します ([プロパティ] ウィンドウで選択した JSON ファイルを使用します)。

![](notifications-images/runwith-vs.png "The Device menu")

-----

既定の通知コント ローラーは、既定のペイロードの JSON ファイルを使用して、シミュレーターでテストするときに、ようになります。

![](notifications-images/notification-debug-sml.png "An example notification")

また、[コマンドライン](~/ios/watchos/troubleshooting.md#command_line)を使用して iOS シミュレーターを起動することもできます。

### <a name="example-notification-payload"></a>通知ペイロードの例

[Watch Kit カタログ](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、ペイロード Json ファイル**notificationpayload. json**の例があります (以下を参照)。

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

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple の Watch Kit の通知に関するドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
