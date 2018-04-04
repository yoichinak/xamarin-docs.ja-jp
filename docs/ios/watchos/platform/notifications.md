---
title: 通知
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 1a681c2bda941d8fe015a8d4da8b99f4d85e441b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="notifications"></a>通知

含む iOS アプリには、それらがサポートされている場合、アプリで通知を受信できるをご覧ください。 組み込みの通知は処理しないように*必要があります*ただし以下に示すその他の通知のサポートを追加する場合は通知の動作をカスタマイズして、外観がで読み取られます。

参照してください、 [iOS 通知](~/ios/platform/user-notifications/deprecated/index.md)ソリューションに iOS アプリに通知のサポートを追加する方法についてのドキュメントです。

## <a name="creating-notification-controllers"></a>通知のコント ローラーを作成します。

ストーリー ボード上通知コント ローラーは特殊な種類のトリガーとなった話題があります。 新しいをドラッグすると**通知インターフェイス コント ローラー**ストーリー ボード上にそれに自動的に接続されている segue:

![](notifications-images/notification-storyboard1.png "新しい通知インターフェイス コント ローラーで接続されている segue")

ときに、通知の話題が選択されているプロパティを編集することができます。

![](notifications-images/notification-storyboard2.png "通知は選択した話題します。")

コント ローラーをカスタマイズした後、これは、WatchKitCatalog からこの例のようになります。

![](notifications-images/notifications-segue.png "通知のプロパティ")


通知の 2 つの種類があります。

- **短い外観**-システムによって定義されるスクロールできない静的なビューです。

- **時間の長い外観**スクロール可能な - 定義したカスタマイズ可能なビューです。 単純化し、静的バージョンより複雑な動的バージョンを指定することができます。

### <a name="short-look-notification-controller"></a>短い表示通知のコント ローラー

アプリのアイコンのみ、アプリ名、および通知のタイトル文字列の短い外観 UI で構成されます。

ユーザーが通知を無視しない場合の詳細を提供する時間の長い確認通知をシステムは自動的に切り替え。


### <a name="long-look-notification-controller"></a>時間の長い外観通知コント ローラー

OS は、さまざまな要因に基づく静的または動的なビューを表示するかどうかを決定します。 静的のインターフェイスを提供し、必要に応じてできる必要がありますも通知の動的なインターフェイスが含まれます。

#### <a name="static"></a>スタティック

静的なビューは、単純ですぐに表示する必要があります。

![](notifications-images/notification-static.png "静的なビュー")

#### <a name="dynamic"></a>動的

動的ビューより多くのデータを表示でき、複数の対話機能を提供することができます。

![](notifications-images/notification-dynamic.png "動的ビュー")


## <a name="generating-notifications"></a>通知を生成します。

通知はリモート サーバーから取得できます ([Apple プッシュ通知サービス](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)、または APNS) または iOS のアプリにローカルで生成されたことができます。

参照してください、 [iOS 通知チュートリアル](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md)をローカルの通知を生成する方法の例については、 [WatchNotifications サンプル](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)実施例についてです。

ローカル通知する必要がありますが、 `AlertTitle` Apple Watch - に表示される設定、`AlertTitle`文字列短い外観インターフェイスに表示されます。 両方、`AlertTitle`と`AlertBody`通知リストに表示されると、`AlertBody`時間の長い外観インターフェイスに表示されます。

このスクリーン ショットは、`AlertTitle`通知リストに表示されていると`AlertBody`時間の長い外観インターフェイスに表示されます (を使用して、[のサンプル コード](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/))。

![](notifications-images/watch-notificationslist-sml.png "このスクリーン ショットは、通知リストに表示されている AlertTitle を示しています") ![ ](notifications-images/watch-notificationcontroller-sml.png "時間の長い外観インターフェイスに表示される AlertBody。")

## <a name="testing-notifications"></a>通知のテスト

通知 (ローカルおよびリモート) 正しくをテストするデバイスを使用してシミュレートできますが、 **.json** iOS シミュレーターでのファイルです。

### <a name="testing-on-apple-watch"></a>Apple Watch でのテスト

Apple Watch での通知をテストするときに必ず[Apple のドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)次の状態します。

> ユーザーの iphone アプリのローカルまたはリモートの通知のいずれかが到着すると、iOS は、Apple Watch または iPhone 上、その通知を表示するかどうかを決定します。

という事実にほのめかしこれは、その iOS は、iPhone や、ウォッチ上に、通知が表示されるかどうかを決定します。 ペアの iPhone がアクティブな通知を受信すると、通知は、iPhone で表示される可能性と*いない*ウォッチにルーティングします。

Watch で通知が表示されることを確認するには、するには、(1 回、[電源] ボタンを押して)、iPhone 画面をオフにするかスリープ状態にできるようにします。 ペアのウォッチが範囲内にするには、電源が入っておよび手首に装着されている、通知がルーティングされ、(と共に、微妙な) 監視に表示します。

### <a name="testing-on-the-ios-simulator"></a>IOS シミュレーターでのテスト

*必要があります*iOS シミュレーターで通知モードをテストするときに、テストの JSON ペイロードを提供します。 パスを設定、**カスタム実行時引数**for mac の Visual Studio のウィンドウ

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac としてウォッチ拡張機能を設定すると追加のオプションが表示されます、**スタートアップ プロジェクト**です。
ウォッチ拡張機能プロジェクトを右クリックし、選択**実行 > カスタム パラメーターしています.**:
    
[![](notifications-images/runwith-customparams-sml.png "カスタム プロパティを持つを実行しています。")](notifications-images/runwith-customparams.png#lightbox)
    
開き、**実行時引数**ウィンドウが含まれていますが、 **WatchKit**タブです。選択**通知**、JSON ペイロードを提供し、キーを押します**Execute** watch アプリをシミュレーターで起動します。
    
[![](notifications-images/runwith-execargs-sml.png "通知ペイロードの既定を選択します。")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio を右クリックを編集するウォッチ拡張機能をテスト通知のペイロードを設定する、**プロジェクト プロパティ**です。 移動して、**デバッグ**セクションし、(これは自動的に一覧表示、プロジェクトに含まれるすべての JSON ファイル) の一覧から通知 JSON ファイルを選択します。
    
[![](notifications-images/runwith-execargs-sml-vs.png "通知の JSON ファイルを選択します。")](notifications-images/runwith-execargs-vs.png#lightbox)

ウォッチ拡張機能の場合は、**スタートアップ プロジェクト**、Visual Studio では、次に示すように追加のオプションが表示されます。 いずれかを選択、**通知**で watch アプリを起動するオプション**通知**モード (ファイルを使用して、JSON のプロパティ ウィンドウで選択)。
    
![](notifications-images/runwith-vs.png "デバイス メニュー")

-----

既定のペイロードの JSON ファイルで、シミュレーターでテストするときに次のような既定の通知のコント ローラー。

![](notifications-images/notification-debug-sml.png "例通知")

使用することも、[コマンドライン](~/ios/watchos/troubleshooting.md#command_line)を iOS シミュレーターを開始します。

### <a name="example-notification-payload"></a>通知のペイロードの例

[ウォッチ キット カタログ](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)サンプルがありますがペイロード JSON ファイルの例を示します**NotificationPayload.json** (下記参照)。

```csharp
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

- [WatchNotifications (ローカル通知) (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple のウォッチ キット通知 docs](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
