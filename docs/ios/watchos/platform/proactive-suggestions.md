---
title: プロアクティブな候補 Xamarin で watchOS
description: この記事では、事前に役に立つ情報をユーザーに自動的に提示することによって、没入感 3 watchOS アプリでプロアクティブな候補を使用する方法を示します。
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 0c0bf6058b2ec7a8e3ef606bef9f725a476abffe
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865914"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>プロアクティブな候補 Xamarin で watchOS

_この記事では、事前に役に立つ情報をユーザーに自動的に提示することによって、没入感 3 watchOS アプリでプロアクティブな候補を使用する方法を示します。_


新しい watchOS 3、適切なタイミングでユーザーに自動的に事前に存在する有用な情報で Xamarin.iOS アプリと情報交換するユーザーの存在するニュースの方法をプロアクティブな候補にします。


## <a name="about-proactive-suggestions"></a>プロアクティブな候補について

Watchos 3、新しい`NSUserActivity`が含まれています、`MapItem`プロパティにより、他のコンテキストで使用できる場所の情報を提供するアプリです。 たとえば、アプリが表示されるホテルのレビューおよび提供、`MapItem`マップ アプリにユーザーが切り替えられた場合は、場所、直前に表示されたホテルの場所は、使用可能なであるとします。

アプリがなどのテクノロジのコレクションを使用してシステムには、この機能を公開`NSUserActivity`MapKit、Media Player および UIKit します。 さらに、アプリ、プロアクティブな提案のサポートを提供し、取得 Siri の緊密な統合を無料。

## <a name="location-based-suggestions"></a>場所ベースの提案

Watchos 3、新しい、`NSUserActivity`クラスが含まれています、`MapItem`により、開発者は他のコンテキストで使用できる場所の情報を提供するプロパティ。 たとえば、アプリには、レストランのレビューが表示されている場合、開発者を設定できます、`MapItem`プロパティをユーザーがアプリで表示される、レストランの場所。 場合は、ユーザーは、マップ アプリに切り替え、レストランの場所は自動的に使用します。

新しいアドレス コンポーネントを使用できる、アプリは、アプリの検索をサポートする場合、`CSSearchableItemAttributesSet`クラスを参照するユーザーの場所を指定します。 設定して、`MapItem`プロパティ、他のプロパティが自動的に入力には。

設定に加え、`Latitude`と`Longitude`アドレス コンポーネントのプロパティのことをお勧め、アプリを指定する、`NamedLocation`と`PhoneNumbers`プロパティ Siri がそのため、場所への呼び出しに開始できます、します。

## <a name="contextual-siri-reminders"></a>コンテキストの Siri アラーム

Siri を使用して、後で、アプリに、表示されているコンテンツを表示するアラームを簡単にできます。 たとえば、アプリでレストランのレビューを表示していた場合、でした Siri を呼び出すと *「通知について帰宅します」。* Siri は、アプリで、レビューへのリンクを使用してアラームを生成します。

## <a name="implementing-proactive-suggestions"></a>プロアクティブな候補を実装します。

Xamarin.iOS アプリをサポートはプロアクティブな提案は通常、一部の Api を実装するか、アプリを実装する可能性がありますは既にいくつかの Api を拡張すると同じくらい簡単です。

プロアクティブな候補は、アプリで次の 3 つの主な方法で使用できます。

- **`NSUserActivity`** -システムの画面で、ユーザーが現在作業にどのような情報を理解することができます。
- **場所の提案**- アプリの提供またはアプリでこの情報を共有する場所については、これら API 拡張機能プラン新しい方法を使用します。

アプリで、次の実装によってサポートされています。

- **コンテキストの Siri アラーム**- 10、iOS で`NSUserActivity`後で、アプリに、表示されているコンテンツを表示する通知を簡単に作成するための Siri を許可するが拡張されています。
- **場所の提案**-iOS 10 の強化`NSUserActivity`をアプリ内で表示する場所をキャプチャし、システム全体でさまざまな場所に昇格させることです。
- **コンテキストの Siri 要求** -  `NSUserActivity`ユーザーを取得できます方向、または呼び出しがある場所から、アプリ内の呼び出し元の Siri、Siri に、アプリ内で表示される情報にコンテキストを提供します。

これらの機能のすべてに共通の 1 つである、すべての使用`NSUserActivity`で 1 つのフォームまたは別の機能を提供します。 

## <a name="nsuseractivity"></a>NSUserActivity

上記で説明したように`NSUserActivity`画面で、ユーザーが現在作業にどのような情報を理解するのに。 `NSUserActivity` アプリを移動する際に、ユーザーのアクティビティをキャプチャするためのメカニズムをキャッシュする軽量の状態。 たとえば、レストランのアプリを見る。

[![](proactive-suggestions-images/activity02.png "レストランのアプリ")](proactive-suggestions-images/activity02.png#lightbox)

次のやり取りで。

1. ユーザーが、アプリで動作するよう、`NSUserActivity`を後でアプリの状態を再作成が作成されます。
2. ユーザーは、レストランを検索する場合は、アクティビティを作成するのと同じパターンが実行されます。
3. もう一度、ユーザーが、結果を閲覧とします。 この最後の場合では、ユーザーが場所を表示し、ios 10 では、システムが場所または通信の相互作用) などの特定の概念について理解します。

最後の画面に詳しく見てをみましょう。

[![](proactive-suggestions-images/activity03.png "NSUserActivity ペイロード")](proactive-suggestions-images/activity03.png#lightbox)

ここで、アプリを作成する、`NSUserActivity`に状態を後で再作成する情報を設定されているとします。 アプリには、この場所の名前やアドレスなどのいくつかのメタデータも含まれています。 このアクティビティを作成するで、アプリに iOS ユーザーの現在の状態を表すことを知ることができます。

アプリは、アクティビティの無線ハンドオフ用に提供、場所のヒントについては、一時的な値として保存または検索結果に表示するため、デバイス上のスポット ライト インデックスに追加がかどうかを決定します。

ハンドオフとスポット ライト検索の詳細についてを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)と[iOS 9 新しい Search APIs](~/ios/platform/search/index.md)ガイド。

### <a name="creating-an-activity"></a>アクティビティを作成します。

アクティビティを作成する前に、アクティビティの型識別子はそれを識別するために作成する必要です。 アクティビティの型識別子は短い文字列に追加、`NSUserActivityTypes`のアプリの配列`Info.plist`ファイルが指定したユーザー アクティビティの種類を一意に識別するために使用します。 配列内のアプリをサポートし、アプリの検索に公開される各アクティビティの 1 つのエントリがあります。 参照してください、[アクティビティ型識別子の参照の作成](~/ios/platform/search/nsuseractivity.md)の詳細。

アクティビティの例を見てください。

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

アクティビティの型識別子を使用して、新しいアクティビティが作成されます。 次に、この状態は、後日復元できるように、アクティビティを定義するいくつかのメタデータが作成されます。 次活動がわかりやすいタイトルを指定し、ユーザー情報にアタッチされています。 最後に、いくつかの機能が有効になっているし、アクティビティは、システムに送信されます。

上記のコードは、次の変更を加えてアクティビティ コンテキストを提供するメタデータに含めるさらに強化できます。

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

開発者に、アプリと同じ情報を表示するのに対応している web サイトがある場合は、アプリは、URL を含めることができ、(Handoff) を使用してインストールされているアプリがまだないその他のデバイスにコンテンツを表示できます。

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>アクティビティを復元します。

検索結果をタップすると、ユーザーに応答する (`NSUserActivity`) アプリでは、編集、 **AppDelegate.cs**オーバーライド ファイルを開き、`ContinueUserActivity`メソッド。 例えば:

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

これは、同じアクティビティの種類の識別子を確認します (`com.xamarin.platform`) として、上記で作成したアクティビティ。 アプリに格納されている情報を使用して、`NSUserActivity`にユーザーが中断状態を復元します。

### <a name="benefits-of-creating-an-activity"></a>アクティビティを作成する利点

最小限の上に示したコードでは、アプリでは 3 つの新しい iOS 10 の機能を活用するためにできるようになりました。

- **Handoff**
- **スポット ライト検索**
- **コンテキストの Siri アラーム**

次のセクションでは、その他の 2 つの新しい iOS 10 機能の有効化を実行します。

- **場所の提案**
- **コンテキストの Siri 要求**

### <a name="location-based-suggestions"></a>場所ベースの提案 

上記のレストラン検索アプリの例を実行します。 これが実装されている場合`NSUserActivity`正しくすべてのメタデータと属性、ユーザーは、次の操作を実行できます。

1. フレンドを満たすために扱おうとするアプリであるレストランを検索します。
2. 場合は、ユーザーは、マップ アプリに切り替え、レストランのアドレスが自動的に変換先としてお勧めします。
3. これでもサード パーティ製アプリの機能 (サポートする`NSUserActivity`) ため、ユーザーは、ライドシェア アプリに切り替えることができますおよびレストランのアドレスが自動的に提案が宛先としてもします。
4. それも提供していますコンテキスト Siri、ユーザーがレストランのアプリ内で Siri を呼び出すし、確認できるように *「... の方向を取得」* Siri ユーザーが表示してレストランへの道順が提供されます。

1 つを共通に上記の機能のすべては、候補の当初からの場所を示すすべて。 上記の例の場合は、架空のレストランのレビューのアプリを勧めします。

watchOS 3 は、いくつかのわずかな変更と既存のフレームワークに追加されたものからのアプリについて、この機能を有効にする強化されています。

- `NSUserActivity` アプリ内で表示されている場所の情報をキャプチャするための追加のフィールドがあります。
- いくつかが追加されました MapKit と CoreSpotlight に場所をキャプチャします。
- 場所の認識機能が、Siri、マップ、マルチタス キング、およびシステム内で他のアプリに追加されました。

場所ベースの推奨事項を実装するには、上で示した同じ活動のコードで開始します。

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

MapKit アプリを使用している場合は、現在のマップを追加するだけ`MKMapItem`アクティビティ。

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

アプリが MapKit を使用していない場合、アプリの検索を採用し、場所は次の新しい属性を指定します。

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

詳細で、上記のコードを見てに設定します。 最初に、すべてのインスタンスで、場所の名前が必要です。

```csharp
attributes.NamedLocation = "Apple Inc.";
```

次に、テキスト ベースの説明に必要なテキスト ベースの (QuickType キーボード) などのインスタンス。

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

緯度と経度は省略できますが、ユーザーが、アプリが送信することを望んでいる正確な場所にルーティングされたことを確認します。

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

電話番号を設定することにより、アプリにアクセスできる Siri のため、ユーザーは、アプリから Siri を言っておきたい次のように呼び出すことができます *「この場所を呼び出す」。

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

最後に、アプリは、インスタンスがナビゲーションおよび電話の適切なかどうかを示します。

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>アクティビティのベスト プラクティス

Apple は、アクティビティを使用する場合、次のベスト プラクティスを示しています。

- 使用`NeedsSave`限定的なペイロードの更新プログラム。
- 現在のアクティビティへの強い参照を保持することを確認します。
- 状態を復元するのに十分な情報を含む小さなペイロードを転送するだけです。
- アクティビティの型識別子で指定する逆引き DNS 表記を使用して、一意でわかりやすいが確認します。 

## <a name="consuming-location-suggestions"></a>消費場所の提案

このセクションでは、(マップ アプリ) など、システムまたはその他のサード パーティ製アプリの他の部分から取得されている場所の提案を消費して説明します。

## <a name="routing-apps-and-locations-suggestions"></a>ルーティングのアプリと場所の提案

このセクションを参照してください、ルーティングのアプリ内から直接場所の提案を消費してになります。 既存のこの機能を追加するルーティングのアプリでは、開発者が活用されます`MKDirectionsRequest`フレームワークとして次のとおりです。

- マルチタスクでアプリを昇格します。
- ルーティングのアプリとしてアプリを登録するには
- 処理を MapKit でアプリを起動する`MKDirectionsRequest`オブジェクト。
- WatchOS ユーザー エンゲージメントに基づいてアプリの提案する方法を説明する機能を提供します。

MapKit で、アプリの起動時に`MKDirectionsRequest`オブジェクトの場合に自動的に始めると、要求された場所にユーザーの指示またはを指示の取得を開始するユーザーを容易にする UI を表示する必要があります。 例えば:


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

詳細で、次のコードを見てに設定します。 テストするかどうかは、要求を有効な変換先を参照してください。

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

であるかどうかは、作成、 `MKDirectionsRequest` URL から。

```csharp
var request = new MKDirectionsRequest(url);
```

新しい watchOS 3 で、アプリを送信できますでそのアドレスをエンコードする必要がある開発者に地理座標がないアドレス。

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>まとめ

この記事でがプロアクティブな候補をカバーし、開発者の使用方法に Xamarin.iOS アプリにトラフィックを促進する watchOS で示しました。 プロアクティブな候補を実装する手順について説明し、使用方法のガイドラインを提示します。


## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
- [SiriKit のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
