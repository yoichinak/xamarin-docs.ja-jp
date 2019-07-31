---
title: Xamarin でのプロアクティブな提案の watchOS
description: この記事では、watchOS 3 アプリでプロアクティブな提案を使用して、システムが有益な情報をユーザーに事前に自動的に提示できるようにすることで、エンゲージメントを促進する方法について説明します。
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7f5c205f73d29c3751acc351294e3ef66c23bb22
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656400"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>Xamarin でのプロアクティブな提案の watchOS

_この記事では、watchOS 3 アプリでプロアクティブな提案を使用して、システムが有益な情報をユーザーに事前に自動的に提示できるようにすることで、エンゲージメントを促進する方法について説明します。_


WatchOS 3 を初めて使用する場合は、ユーザーが適切なタイミングで有益な情報をユーザーに事前に自動的に提示することによって、ユーザーが Xamarin. iOS アプリを使用する方法についてのニュースを提供しています。


## <a name="about-proactive-suggestions"></a>プロアクティブな提案について

WatchOS 3 の新機能`NSUserActivity`には`MapItem` 、アプリが他のコンテキストで使用できる場所情報を提供できるようにするプロパティが含まれています。 たとえば、ホテルに表示されているアプリで場所`MapItem`を確認し、場所を指定した場合、ユーザーが Maps アプリに切り替えた場合は、そのホテルの場所が表示されています。

このアプリは`NSUserActivity`、、mapkit、Media Player、uikit などのテクノロジのコレクションを使用して、この機能をシステムに公開します。 さらに、アプリの事前提案のサポートを提供することで、Siri 統合を無料で利用できるようになります。

## <a name="location-based-suggestions"></a>場所に基づく提案

WatchOS 3 の新機能で`NSUserActivity`あるクラスに`MapItem`は、開発者が他のコンテキストで使用できる位置情報を提供できるようにするプロパティが含まれています。 たとえば、アプリがレストランのレビューを表示する場合、開発者は`MapItem` 、ユーザーがアプリで表示しているレストランの場所にプロパティを設定できます。 ユーザーが Maps アプリに切り替えた場合、レストランの場所は自動的に使用できるようになります。

アプリがアプリ検索をサポートしている場合は、 `CSSearchableItemAttributesSet`クラスの新しいアドレスコンポーネントを使用して、ユーザーがアクセスする場所を指定できます。 `MapItem`プロパティを設定すると、その他のプロパティは自動的に入力されます。

アドレスコンポーネントのプロパティの`Latitude`および`Longitude`を設定するだけでなく、アプリでもプロパティ`NamedLocation`と`PhoneNumbers`プロパティを指定することをお勧めします。これにより、siri は場所への呼び出しを開始できます。

## <a name="contextual-siri-reminders"></a>コンテキスト Siri リマインダー

ユーザーは、Siri を使用して、後でアプリで現在表示しているコンテンツをすぐに表示することができます。 たとえば、アプリでレストランのレビューを表示していた場合、Siri を呼び出して、 *「自宅にいるときにこのことを通知する*」と言うことができます。 Siri は、アプリ内のレビューへのリンクを含むリマインダーを生成します。

## <a name="implementing-proactive-suggestions"></a>プロアクティブな提案の実装

Xamarin iOS アプリに事前提案サポートを追加するのは、通常、いくつかの Api を実装したり、アプリが既に実装している可能性のあるいくつかの Api を拡張したりするのと同じほど簡単です。

プロアクティブな提案は、次の3つの主な方法でアプリを操作します。

- **`NSUserActivity`** -ユーザーが現在画面上で操作している情報をシステムが理解するのに役立ちます。
- **場所の提案**-アプリが場所ベースの情報を提供または使用する場合、これらの API 拡張機能は、この情報をアプリ間で共有するための新しい方法を提供します。

とは、次のものを実装することによって、アプリでサポートされます。

- **コンテキスト siri リマインダー** -iOS 10 では`NSUserActivity` 、この拡張により、siri は、アプリで現在表示されているコンテンツを後ですぐに表示できるようになりました。
- **場所の提案**-iOS 10 `NSUserActivity`は、アプリ内で表示される場所をキャプチャし、システム全体のさまざまな場所で昇格するように拡張します。
- **コンテキスト siri 要求** -  `NSUserActivity`では、アプリ内に表示される情報を siri に提供するため、ユーザーはアプリ内から siri を呼び出すことができます。

これらの機能はすべて、1つのフォームで使用さ`NSUserActivity`れるか、またはその機能を提供するために使用されます。 

## <a name="nsuseractivity"></a>NSUserActivity

前述のように`NSUserActivity` 、では、ユーザーが現在どのような情報を画面で操作しているかをシステムが理解するのに役立ちます。 `NSUserActivity`は、アプリ内を移動するときにユーザーのアクティビティをキャプチャする軽量の状態のキャッシュメカニズムです。 たとえば、レストランアプリを見ているとします。

[![](proactive-suggestions-images/activity02.png "レストランアプリ")](proactive-suggestions-images/activity02.png#lightbox)

次のような相互作用があります。

1. ユーザーがアプリ`NSUserActivity`で作業するときに、アプリの状態を後で再作成するためにが作成されます。
2. ユーザーがレストランを検索する場合は、アクティビティの作成と同じパターンが適用されます。
3. ここでも、ユーザーが結果を表示したとき。 この最後の例では、ユーザーが場所を表示していて、iOS 10 では、特定の概念 (場所や通信の相互作用など) を認識しています。

最後の画面を詳しく見てみましょう。

[![](proactive-suggestions-images/activity03.png "NSUserActivity ペイロード")](proactive-suggestions-images/activity03.png#lightbox)

ここで、アプリはを`NSUserActivity`作成しており、後で状態を再作成するための情報が設定されています。 アプリには、場所の名前や住所などのいくつかのメタデータも含まれています。 このアクティビティを作成すると、アプリはユーザーの現在の状態を表すことを iOS に通知します。

次に、アプリは、ハンドオフのために活動を提供するかどうかを決定します。これは場所の提案の一時的な値として保存されるか、または検索結果に表示するためにデバイス上のスポットライトインデックスに追加されます。

ハンドオフとスポットライト検索の詳細については、「[ハンドオフ](~/ios/platform/handoff.md)と[IOS 9 の新しい検索 api](~/ios/platform/search/index.md)ガイド」を参照してください。

### <a name="creating-an-activity"></a>アクティビティの作成

アクティビティを作成する前に、アクティビティの種類を識別するための識別子を作成する必要があります。 アクティビティの種類の識別子は、特定のユーザーアクティビティ`NSUserActivityTypes`の種類を一意に`Info.plist`識別するために使用される、アプリのファイルの配列に追加される短い文字列です。 アプリがサポートし、アプリ検索に公開されるアクティビティごとに、配列に1つのエントリがあります。 詳細については、「[アクティビティの種類の識別子の作成](~/ios/platform/search/nsuseractivity.md)」を参照してください。

アクティビティの例を見てみましょう。

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

新しいアクティビティは、アクティビティの種類の識別子を使用して作成されます。 次に、この状態を後で復元できるように、アクティビティを定義するメタデータを作成します。 次に、アクティビティにわかりやすいタイトルを付け、ユーザー情報に添付します。 最後に、一部の機能が有効になり、アクティビティがシステムに送信されます。

上記のコードをさらに拡張して、次のような変更を行うことによってアクティビティにコンテキストを提供するメタデータを含めることができます。

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

アプリと同じ情報を表示できる web サイトが開発者にある場合、アプリには URL を含めることができ、アプリがインストールされていない他のデバイス (ハンドオフ) でコンテンツを表示できます。

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>アクティビティの復元

アプリの検索結果 (`NSUserActivity`) をタップしてユーザーに応答するには、 **AppDelegate.cs** `ContinueUserActivity`ファイルを編集し、メソッドをオーバーライドします。 例えば:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

これが、上記で作成したアクティビティ`com.xamarin.platform`と同じ種類の id () であることを確認します。 アプリは、 `NSUserActivity`に格納されている情報を使用して、ユーザーが中断した場所に状態を戻します。

### <a name="benefits-of-creating-an-activity"></a>アクティビティを作成する利点

上記のコードを最小限に抑えることで、アプリでは次の3つの新しい iOS 10 機能を利用できるようになりました。

- **Handoff**
- **スポットライト検索**
- **コンテキスト Siri リマインダー**

次のセクションでは、その他の2つの iOS 10 の新機能を有効にする方法について説明します。

- **場所の提案**
- **コンテキスト Siri 要求**

### <a name="location-based-suggestions"></a>場所に基づく提案 

上記のレストラン検索アプリの例を見てください。 実装`NSUserActivity`されており、すべてのメタデータと属性が正しく設定されている場合、ユーザーは次の操作を実行できます。

1. アプリ内で友人を満足させるレストランを検索します。
2. ユーザーが Maps アプリに切り替えた場合、レストランの住所は自動的に宛先として提案されます。
3. これはサードパーティ製のアプリ (をサポート`NSUserActivity`) に対しても機能します。そのため、ユーザーは、乗り物共有アプリに切り替えることができ、レストランのアドレスも宛先として自動的に提案されます。
4. また、Siri にコンテキストが提供されるので、ユーザーはレストランアプリ内で Siri を呼び出して、 *"取得方向.* .." を尋ね、siri は、ユーザーが表示しているレストランへの指示を提供します。

上記の機能はすべて共通しています。これらはすべて、提案がもともとどこから来ているかを示しています。 上記の例の場合は、架空のレストランレビューアプリです。

watchOS 3 は、いくつかの小さな変更や既存のフレームワークへの追加によってアプリでこの機能を有効にするように強化されています。

- `NSUserActivity`には、アプリ内で表示される場所情報をキャプチャするための追加のフィールドがあります。
- 位置情報をキャプチャするために、MapKit と CoreSpotlight にいくつかの追加が行われています。
- 場所を認識する機能が、システム内の Siri、マップ、マルチタスク、およびその他のアプリに追加されました。

場所に基づいた提案を実装するには、前に示したのと同じアクティビティコードから開始します。

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

アプリが mapkit を使用している場合は、現在のマップ`MKMapItem`をアクティビティに追加するのと同じように簡単です。

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

アプリが MapKit を使用していない場合は、アプリ検索を採用して、次の場所の新しい属性を指定できます。

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

上記のコードを詳しく見てみましょう。 まず、すべてのインスタンスで場所の名前を指定する必要があります。

```csharp
attributes.NamedLocation = "Apple Inc.";
```

次に、テキストベースのインスタンス (QuickType キーボードなど) に必要なテキストベースの説明を示します。

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

緯度と経度は省略可能ですが、アプリが送信する場所にユーザーがルーティングされていることを確認してください。

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

電話番号を設定することにより、アプリは Siri にアクセスできるようになります。これにより、ユーザーがアプリから Siri を呼び出すことができるようになります。たとえば、* "this place を呼び出してください" と表示されます。

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

最後に、アプリは、インスタンスがナビゲーションと通話に適しているかどうかを示すことができます。

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>活動のベストプラクティス

Apple では、アクティビティを操作するときに、次のベストプラクティスを提案しています。

- 遅延`NeedsSave`ペイロードの更新に使用します。
- 現在のアクティビティへの強い参照を保持するようにしてください。
- 状態を復元するのに十分な情報だけを含む小さなペイロードのみを転送します。
- アクティビティの種類の識別子が一意であり、逆引き DNS 表記を使用して指定されていることを確認します。 

## <a name="consuming-location-suggestions"></a>場所の提案の使用

次のセクションでは、システムの他の部分 (Maps アプリなど) または他のサードパーティ製アプリから取得した場所に関する推奨事項について説明します。

## <a name="routing-apps-and-locations-suggestions"></a>アプリと場所をルーティングする提案

このセクションでは、ルーティングアプリ内から直接場所の提案を利用する方法について説明します。 この機能を追加するルーティングアプリでは、開発者は次の`MKDirectionsRequest`ように既存のフレームワークを活用します。

- アプリをマルチタスクで昇格させることができます。
- アプリをルーティングアプリとして登録します。
- Mapkit `MKDirectionsRequest`オブジェクトを使用したアプリの起動を処理する場合は。
- WatchOS に、ユーザーエンゲージメントに基づいてアプリを提案する機能を提供します。

アプリケーションが mapkit `MKDirectionsRequest`オブジェクトを使用して起動されると、要求された場所へのユーザーの指示が自動的に開始されるか、ユーザーが簡単に方向を開始できるようにするための UI が表示されます。 例えば:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

このコードを詳しく見てみましょう。 これは、有効な送信先要求であるかどうかをテストします。

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

の場合は、URL からを`MKDirectionsRequest`作成します。

```csharp
var request = new MKDirectionsRequest(url);
```

WatchOS 3 の新動作では、アプリに geo 座標のないアドレスを送信できます。この場合、開発者はアドレスをエンコードする必要があります。

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Summary

この記事では、プロアクティブな提案について説明し、開発者が Xamarin. iOS app for watchOS へのトラフィックを促進する方法について説明しました。 ここでは、プロアクティブな提案を実装し、使用ガイドラインを提示する手順について説明します。


## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [SiriKit プログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
