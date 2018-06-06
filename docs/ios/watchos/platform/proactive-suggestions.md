---
title: watchOS Xamarin ではプロアクティブ提案
description: ここでは、システムが事前に有用な情報をユーザーに自動的に表示できるように、watchOS 3 アプリケーションではドライブ エンゲージメントをプロアクティブな提案を使用する方法を説明します。
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 33dccd00e07062e040c2707826ef62b764e11a0e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791287"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>watchOS Xamarin ではプロアクティブ提案

_ここでは、システムが事前に有用な情報をユーザーに自動的に表示できるように、watchOS 3 アプリケーションではドライブ エンゲージメントをプロアクティブな提案を使用する方法を説明します。_


初めて使用する watchOS 3、存在のニュース方法をユーザーに適切な時点でのユーザーに自動的に有用な情報を事前に存在して Xamarin.iOS アプリと情報交換にプロアクティブなご提案します。


## <a name="about-proactive-suggestions"></a>プロアクティブな推奨事項について

WatchOS 3 を新しい`NSUserActivity`が含まれています、`MapItem`プロパティにより、他のコンテキストで使用できる場所の情報を提供するアプリです。 たとえば、表示されるアプリのホテルを確認し、提供、`MapItem`マップ アプリにユーザーが切り替えられた場合は、場所、だけを表示していたホテルの場所は、できます。

アプリがなど、テクノロジのコレクションを使用して、システムにこの機能を公開`NSUserActivity`MapKit、Media Player および UIKit です。 さらに、アプリをプロアクティブ提案のサポートを提供する取得 Siri の緊密な統合無料します。

## <a name="location-based-suggestions"></a>場所に基づいて候補

WatchOS 3 を新しい、`NSUserActivity`クラスが含まれています、`MapItem`により、開発者は他のコンテキストで使用できる場所の情報を提供するプロパティです。 たとえば、アプリには、レストランのレビューが表示されている場合、開発者を設定できます、`MapItem`プロパティをユーザーがアプリで表示してレストランの場所。 場合は、ユーザーは、マップのアプリに切り替え、レストランの場所は自動的に利用可能です。

新しいアドレス コンポーネントを使用できるアプリは、アプリの検索をサポートする場合、`CSSearchableItemAttributesSet`クラスが、ユーザーがアクセスする可能性がある場所を指定します。 設定して、`MapItem`プロパティ、その他のプロパティが自動的に入力します。

設定に加えて、`Latitude`と`Longitude`アドレス コンポーネントのプロパティのことをお勧め、アプリを指定する、`NamedLocation`と`PhoneNumbers`プロパティもため Siri を開始できる場所への呼び出しです。

## <a name="contextual-siri-reminders"></a>コンテキスト Siri アラーム

Siri を使用して簡単に現在表示しているアプリで、後で、コンテンツの表示を求めるメッセージを作成することができます。 たとえば、アプリでレストランのレビューを表示していた場合、でした Siri を呼び出すし、言う *「通知についてホーム出た場合」。* Siri は、アプリで、レビューへのリンクを使用してアラームを生成します。

## <a name="implementing-proactive-suggestions"></a>プロアクティブな推奨事項を実装します。

Xamarin.iOS アプリには、サポートは事前対応型の候補を追加することは通常、いくつかの Api を実装するか、アプリが実装することがある、いくつかの Api を拡張すると同じくらい簡単です。

プロアクティブな推奨事項は、3 つの主な方法で、アプリと使用します。

- **`NSUserActivity`** -システムの画面で、ユーザーが現在作業をどのような情報を理解することができます。
- **場所提案**- アプリの提供またはアプリ間でこの情報を共有する場所については、これら API 拡張オファーの新しい方法を使用します。

次を実装することによって、アプリでサポートされているとします。

- **コンテキストの Siri アラーム**- iOS 10、`NSUserActivity`に現在表示しているアプリで、後で、コンテンツの表示を求めるメッセージを簡単に Siri を使用する拡張されました。
- **場所提案**-iOS 10 の強化`NSUserActivity`をアプリ内での表示場所をキャプチャし、それらをシステム全体で多くの場所で昇格します。
- **コンテキストの Siri 要求** -  `NSUserActivity`ユーザーを取得できます方向、または場所に、呼び出しをする、アプリ内から呼び出す Siri Siri をアプリ内で説明する情報にコンテキストを提供します。

1 つの点に共通のすべてのこれらの機能がある、すべての使用`NSUserActivity`で 1 つのフォームまたは別の機能を提供します。 

## <a name="nsuseractivity"></a>NSUserActivity

、前に述べたよう`NSUserActivity`、システムが画面上で、ユーザーが現在処理してどのような情報を理解するのに役立ちます。 `NSUserActivity` 軽量状態は、アプリ間を移動できるように、ユーザーのアクティビティをキャプチャするためのメカニズムをキャッシュします。 たとえば、レストランのアプリを確認します。

[![](proactive-suggestions-images/activity02.png "レストラン アプリ")](proactive-suggestions-images/activity02.png#lightbox)

次の相互作用: と

1. ユーザーがアプリを合わせて、`NSUserActivity`が後で、アプリの状態を再作成を作成します。
2. ユーザーは、レストランを検索する場合は、同じパターンのアクティビティを作成するが実行されます。
3. もう一度、ユーザーが、結果を表示するとします。 この最後の場合、ユーザーの場所が表示および 10、iOS では、システムがいくつかの概念 (場所、または通信の相互作用) などのさらに注意してください。

最後の画面で詳しく見てをみましょう。

[![](proactive-suggestions-images/activity03.png "NSUserActivity ペイロード")](proactive-suggestions-images/activity03.png#lightbox)

ここで、アプリを作成する、`NSUserActivity`情報と共に、状態を後で再作成に値が表示されているとします。 アプリが、この場所の名前やアドレスなど一部のメタデータにはとも含まれます。 このアクティビティを作成すると、アプリに iOS のユーザーの現在の状態を表すことを知ることができます。

アプリは、アクティビティの無線ハンドオフ用に提供、一時的な値の場所についてのご提案として保存または検索結果に表示するため、デバイス上のスポット ライト インデックスに追加がかどうかを決定します。

ハンドオフと Spotlight 検索の詳細についてを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)と[iOS 9 新しい検索 Api](~/ios/platform/search/index.md)ガイドです。

### <a name="creating-an-activity"></a>アクティビティを作成します。

アクティビティを作成する前に、アクティビティの型識別子は、識別するために作成する必要があります。 アクティビティ型識別子は、短い文字列に追加、`NSUserActivityTypes`のアプリの配列`Info.plist`ファイルが指定したユーザー アクティビティの種類を一意に識別するために使用します。 配列内のアプリをサポートします。 アプリの検索に公開される各アクティビティを 1 つのエントリがあります。 参照してください、[アクティビティ型識別子の参照を作成する](~/ios/platform/search/nsuseractivity.md)詳細についてはします。

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

アクティビティ型識別子を使用して、新しいアクティビティが作成されます。 次に、この状態は、後で復元できるように、いくつかのメタデータをアクティビティの定義が作成されます。 アクティビティはわかりやすいタイトルを指定し、ユーザー情報にアタッチします。 最後に、いくつかの機能が有効になっているし、アクティビティは、システムに送信します。

上記のコードは、次の変更を加えて、アクティビティのコンテキストを提供するメタデータに含めるさらに強化できます。

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

開発者には、アプリと同じ情報を表示できる web サイトがある場合、アプリは、URL を含めることができ、(ハンドオフ) 経由でインストールされているアプリを持たない他のデバイスに、コンテンツを表示することができます。

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>アクティビティを復元します。

検索結果をタップすると、ユーザーに応答する (`NSUserActivity`)、アプリの編集、 **<code>appdelegate.cs</code>** ファイルし、オーバーライド、`ContinueUserActivity`メソッドです。 例えば:

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

これは、同じアクティビティの型識別子を確認してください (`com.xamarin.platform`) 上で作成したアクティビティとして。 アプリに格納されている情報を使用して、`NSUserActivity`にユーザーが中断状態を復元します。

### <a name="benefits-of-creating-an-activity"></a>アクティビティを作成する利点

最小限の上に示したコードの量でのアプリを 3 つの新しい iOS 10 機能の利用できるようになりました。

- **Handoff**
- **Spotlight 検索で**
- **コンテキスト Siri アラーム**

次のセクションでは見て他の 2 つの新しい iOS 10 機能を有効にします。

- **場所の推奨事項**
- **コンテキスト Siri 要求**

### <a name="location-based-suggestions"></a>場所に基づいて候補 

レストランの検索アプリを上記の例を実行します。 実装する場合`NSUserActivity`し、正しく設定のすべてのメタデータと属性をユーザーが、次の操作を実行できます。

1. フレンドを対応するアプリであるレストランを検索します。
4. ユーザーは、マップのアプリに切り替え場合、レストランのアドレスが自動的に変換先として推奨されています。
5. これは、サード パーティ製アプリに対しても機能 (をサポートする`NSUserActivity`) ので、飛行共有アプリをユーザーが切り替えることができ、レストランのアドレスが自動的に提案があるの保存先としてもします。
6. コンテキストに提供 Siri、ため、ユーザーがレストランのアプリ内で Siri を呼び出すことができます、依頼して *「... の方向を取得」* Siri はユーザーが表示してレストランへの道順を提供します。

1 つの点を共通の上記の機能のすべてから、修正案を最初の位置を示すため、それらはすべて。 上記の例の場合は、架空のレストランのレビュー アプリを勧めします。

watchOS 3 は、いくつかの変更を加えると既存のフレームワークへの追加を介してアプリについて、この機能を有効にする強化されています。

- `NSUserActivity` アプリ内で表示される場所の情報をキャプチャするための他のフィールドがあります。
- いくつかの点に加えられた MapKit と CoreSpotlight に場所をキャプチャします。
- Siri、マップ、マルチタスク処理およびその他のアプリ、システム内に、場所の対応する機能が追加されました。

場所ベースの推奨事項を実装するのには、上に示した同じアクティビティのコードを開始します。

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

MapKit アプリを使用している場合は、現在のマップを追加するように単純`MKMapItem`アクティビティに。

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

アプリが MapKit を使用していない場合、アプリの検索を採用し、場所の次の新しい属性を指定します。

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

見て、上記のコードの詳細。 最初に、すべてのインスタンス内の場所の名前が必要です。

```csharp
attributes.NamedLocation = "Apple Inc.";
```

次に、テキスト ベースの説明が必要なテキスト ベースの (QuickType キーボード) などのインスタンス。

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

緯度と経度オプションですが、ユーザーに送信しようとして、アプリの正確な場所にルーティングされるようにします。

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

電話番号を設定することにより、アプリがアクセスできる Siri のため、ユーザーは、アプリから Siri を次のようを言うことにより呼び出すことができます * 電話でこの場所は"。

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

最後に、アプリでは、インスタンスが移動や電話をかけるのために適切なかどうかを示します。

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>アクティビティのベスト プラクティス

Apple のアクティビティを使用する場合、次のベスト プラクティスを示しています。

- 使用して`NeedsSave`ペイロードの限定的な更新プログラム。
- 現在のアクティビティへの強い参照を保持することを確認します。
- 状態を復元するのに十分な情報を含む小さなペイロードを転送だけです。
- アクティビティの型識別子が一意でわかりやすい名前である逆引き DNS 表記を使用して、それらを指定することを確認します。 

## <a name="consuming-location-suggestions"></a>場所の提案を使用

このセクションでは、(マップ アプリ) など、システムまたはその他のサード パーティ製アプリの他の部分からの場所の提案を消費して説明します。

## <a name="routing-apps-and-locations-suggestions"></a>ルーティングのアプリと場所の推奨事項

このセクションでは見てルーティング アプリ内から直接場所提案を使用します。 この機能を追加するルーティング、アプリの開発者が活用既存`MKDirectionsRequest`ようフレームワーク。

- マルチタスクでアプリを昇格します。
- ルーティング アプリとしてアプリを登録します。
- MapKit と、アプリの起動を処理するために`MKDirectionsRequest`オブジェクト。
- WatchOS のユーザー エンゲージメントに基づいてアプリを提案する方法を説明する機能を提供します。

起動すると、アプリは、MapKit`MKDirectionsRequest`オブジェクト、する必要があります自動的に開始要求された場所にユーザーの指示を与えるまたはユーザーが方向の取得を開始しやすくための UI を表示します。 例えば:


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

詳細で、次のコードを見てに設定します。 参照先として有効な要求であるかどうかにテストします。

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

であるかどうかは、作成、 `MKDirectionsRequest` URL から。

```csharp
var request = new MKDirectionsRequest(url);
```

新しい watchOS 3 でアプリを送信できますアドレスをエンコードする必要がある開発者は、その原因で、地理的座標を持たないアドレス。

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>まとめ

この記事がプロアクティブな推奨事項を説明し、どの開発者でも使用できる、Xamarin.iOS アプリへのトラフィックをドライブに watchOS を示しました。 プロアクティブな推奨事項を実装する手順を説明し、使用方法のガイドラインを提示します。


## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
- [SiriKit プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
