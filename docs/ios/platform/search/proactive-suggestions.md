---
title: Xamarin.iOS でプロアクティブな候補の概要
description: この記事では、事前に役に立つ情報をユーザーに自動的に提示することによって、没入感の Xamarin.iOS アプリでプロアクティブな候補を使用する方法を示します。
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2ab0147f918b36dc47ef6eed7d9bf1b6295d9733
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870405"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Xamarin.iOS でプロアクティブな候補の概要

_この記事では、事前に役に立つ情報をユーザーに自動的に提示することによって、没入感の Xamarin.iOS アプリでプロアクティブな候補を使用する方法を示します。_

新しい iOS 10 では、適切なタイミングでユーザーに自動的に事前に存在する有用な情報で Xamarin.iOS アプリと情報交換するユーザーの存在するニュースの方法をプロアクティブな候補にします。

iOS 10 では、事前に提示する役に立つ情報に自動的にユーザーに適切なタイミングでシステムを許可することでアプリに engagement が運転する新しい方法を表示します。 9 iOS と同様には、ディープ検索スポット ライト、ハンドオフ Siri の推奨事項を使用してアプリを追加する機能が提供されている (を参照してください[新しい Search APIs](~/ios/platform/search/index.md))、iOS 10 のアプリができるユーザーに表示する、システム内から機能を公開できます、次の場所:

- アプリケーションのスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 提案

アプリがなどのテクノロジのコレクションを使用してシステムには、この機能を公開`NSUserActivity`、コア スポット ライト、MapKit、Media Player、UIKit、web マークアップ。 さらに、アプリ、プロアクティブな提案のサポートを提供し、取得 Siri の緊密な統合を無料。

## <a name="location-based-suggestions"></a>場所ベースの提案

Ios 10 では、新しい、`NSUserActivity`クラスが含まれています、`MapItem`により、開発者は他のコンテキストで使用できる場所の情報を提供するプロパティ。 たとえば、アプリには、レストランのレビューが表示されている場合、開発者を設定できます、`MapItem`プロパティをユーザーがアプリで表示される、レストランの場所。 場合は、ユーザーは、マップ アプリに切り替え、レストランの場所は自動的に使用します。

新しいアドレス コンポーネントを使用できる、アプリは、アプリの検索をサポートする場合、`CSSearchableItemAttributesSet`クラスを参照するユーザーの場所を指定します。 設定して、`MapItem`プロパティ、他のプロパティが自動的に入力には。

設定に加え、`Latitude`と`Longitude`アドレス コンポーネントのプロパティのことをお勧め、アプリを指定する、`NamedLocation`と`PhoneNumbers`プロパティ Siri がそのため、場所への呼び出しに開始できます、します。

## <a name="web-markup-based-suggestions"></a>Web マークアップに基づいて候補

iOS 9 の追加と Safari の Spotlight 検索結果でユーザーに表示されるコンテンツを拡充する web サイトで構造化データのマークアップを含めることもできます (を参照してください[Web マークアップを使って検索](~/ios/platform/search/web-markup.md))。 iOS 10 には、場所ベースのマークアップを含める機能が追加されます (など[PostalAddress](http://schema.org/PostalAddress)で定義されている[Schema.org](http://schema.org/)) をさらに、ユーザーのエクスペリエンスを強化します。 など場所をユーザーのビューに、web サイトのページがマークされている場合、マップを開くときに、システムは同じ場所提案できます。

## <a name="text-based-suggestions"></a>テキスト ベースの提案

UIKit は iOS 10 に含めるで拡張されていますが、 [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype)のプロパティ、 [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits)テキスト領域の内容のセマンティックな意味を指定するクラス。 システムが通常は自動的に適切なキーボードの種類を選択できますこの情報には、オート コレクトの提案を改善し、他のアプリや web サイトからの情報を事前に統合します。

たとえば、ユーザーがマークされているテキスト フィールドにテキストを入力する`UITextContentType.FullStreetAddress`システムが自動入力フィールドをユーザーが最後に表示する場所をお勧めします。

## <a name="media-based-suggestions"></a>メディアに基づいて候補

アプリを使用してメディアを再生する場合、 [MPPlayableContentManager](xref:MediaPlayer.MPPlayableContentManager) API、iOS 10 により、ユーザーは、アルバム アートを表示して、ロック画面で、アプリでメディアを再生します。

## <a name="contextual-siri-reminders"></a>コンテキストの Siri アラーム

Siri を使用して、後で、アプリに、表示されているコンテンツを表示するアラームを簡単にできます。 たとえば、アプリでレストランのレビューを表示していた場合、でした Siri を呼び出すと *「通知について帰宅します」。* Siri は、アプリで、レビューへのリンクを使用してアラームを生成します。

## <a name="contact-based-suggestions"></a>連絡先に基づいて候補

アプリの許可に表示される連絡先 (および関連する連絡先の情報)、**にお問い合わせください**システムに格納されているすべてのユーザーの連絡先と iOS デバイス上のアプリ。

## <a name="ride-sharing-based-suggestions"></a>共有の乗り物に基づいて候補

ライドシェア アプリで使用する場合、 [MKDirectionsRequest](xref:MapKit.MKDirectionsRequest) API、iOS 10 が表示されますが、アプリケーションのスイッチャーのオプションとしてときに、ユーザーは、乗車する可能性があります。 指定することによって、ライドシェア アプリとしてアプリが登録もする必要があります、`MKDirectionsModeRideShare`の[MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html)キーでその`Info.plist`ファイル。

のみ、アプリは、乗車の共有をサポートする場合、その提案をシステムで開始 *「への変更を取得...」*、ルーティングの方向 (ウォーキングまたは自転車) などの他の種類はサポートされている場合、システムが使用 *「する方向を取得...」*

> [!IMPORTANT]
> [MKMapItem](xref:MapKit.MKMapItem)アプリが受け取るオブジェクトの緯度と経度の情報を含まない場合がありますおよびジオコーディングが必要になります。

## <a name="implementing-proactive-suggestions"></a>プロアクティブな候補を実装します。

Xamarin.iOS アプリをサポートはプロアクティブな提案は通常、一部の Api を実装するか、アプリを実装する可能性がありますは既にいくつかの Api を拡張すると同じくらい簡単です。

プロアクティブな候補は、アプリで次の 3 つの主な方法で使用できます。

- **`NSUserActivity` Schema.org**  -  `NSUserActivity`画面で、ユーザーが現在作業にどのような情報を理解するのに。 Schema.org では、web ページへと同等の機能を追加します。
- **場所の提案**- アプリの提供またはアプリでこの情報を共有する場所については、これら API 拡張機能プラン新しい方法を使用します。
- **メディア アプリの提案**- システムは、アプリを昇格させることができ、iOS デバイスでのユーザーの操作のコンテキストに基づいて、そのメディア コンテンツ。

アプリで、次の実装によってサポートされています。

- **ハンドオフ** -  `NSUserActivity`により、開発者は 1 つのデバイスでアクティビティを開始し、別の作業を再開するには、ハンドオフをサポートするために iOS 8 で追加されました (を参照してください[ハンドオフの概要](~/ios/platform/handoff.md))。
- **Spotlight 検索**-iOS 9 を使用して、Spotlight 検索結果内からアプリのコンテンツを昇格する機能が追加されました。 `NSUserActivity` (を参照してください[コア スポット ライト検索](~/ios/platform/search/corespotlight.md))。
- **コンテキストの Siri アラーム**- 10、iOS で`NSUserActivity`後で、ユーザーが現在表示アプリのコンテンツを表示する通知を簡単に作成するための Siri を許可するが拡張されています。
- **場所の提案**-iOS 10 の強化`NSUserActivity`をアプリ内で表示する場所をキャプチャし、システム全体でさまざまな場所に昇格させることです。
- **コンテキストの Siri 要求** -  `NSUserActivity`ユーザーを取得できます方向、または呼び出しがある場所から、アプリ内の呼び出し元の Siri、Siri に、アプリ内で表示される情報にコンテキストを提供します。
- **相互作用にお問い合わせください**iOS 10 の新機能 -`NSUserActivity`代替通信メソッドとして (連絡先アプリ) 内の連絡先カードから昇格するアプリの通信を許可します。

これらの機能のすべてに共通の 1 つである、すべての使用`NSUserActivity`で 1 つのフォームまたは別の機能を提供します。 

## <a name="nsuseractivity"></a>NSUserActivity

上記で説明したように`NSUserActivity`画面で、ユーザーが現在作業にどのような情報を理解するのに。 `NSUserActivity` アプリを移動する際に、ユーザーのアクティビティをキャプチャするためのメカニズムをキャッシュする軽量の状態。 たとえば、レストランのアプリを見る。

[![](proactive-suggestions-images/activity02.png "キャッシュ メカニズム NSUserActivity 軽量の状態")](proactive-suggestions-images/activity02.png#lightbox)

次のやり取りで。

1. ユーザーが、アプリで動作するよう、`NSUserActivity`を後でアプリの状態を再作成が作成されます。
2. ユーザーは、レストランを検索する場合は、アクティビティを作成するのと同じパターンが実行されます。
3. もう一度、ユーザーが、結果を閲覧とします。 この最後の場合では、ユーザーが場所を表示し、ios 10 では、システムが場所または通信の相互作用) などの特定の概念について理解します。

最後の画面に詳しく見てをみましょう。

[![](proactive-suggestions-images/activity03.png "NSUserActivity 詳細")](proactive-suggestions-images/activity03.png#lightbox)

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

検索結果をタップすると、ユーザーに応答する (`NSUserActivity`) アプリでは、編集、 **AppDelegate.cs**オーバーライド ファイルを開き、`ContinueUserActivity`メソッド。 例:

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

開発者が同じアクティビティの種類の識別子であるかを確認する必要があります (`com.xamarin.platform`) として、上記で作成したアクティビティ。 アプリに格納されている情報を使用して、`NSUserActivity`にユーザーが中断状態を復元します。

### <a name="benefits-of-creating-an-activity"></a>アクティビティを作成する利点

最小限の上に示したコードでは、アプリでは 3 つの新しい iOS 10 の機能を活用するためにできるようになりました。

- **ハンドオフ**
- **スポット ライト検索**
- **コンテキストの Siri アラーム**

次のセクションでは、その他の 2 つの新しい iOS 10 機能の有効化を実行します。

- **場所の提案**
- **コンテキストの Siri 要求**

### <a name="location-based-suggestions"></a>場所ベースの提案 

上記のレストラン検索アプリの例を実行します。 これが実装されている場合`NSUserActivity`正しくすべてのメタデータと属性、ユーザーは、次の操作を実行できます。

1. フレンドを満たすために扱おうとするアプリであるレストランを検索します。
2. ユーザーは、マルチタスク処理アプリケーションのスイッチャーを使用してアプリから移動すると、システムが自動的に表示提案 (画面の下部) のナビゲーションのお気に入りのアプリを使用してレストランへの道順を取得します。
3. ユーザーがメッセージのアプリに切り替え、入力を開始する場合 *「に対応しましょう」*、QuickType キーボードは、レストランのアドレスを貼り付けることを自動的に提案します。
4. 場合は、ユーザーは、マップ アプリに切り替え、レストランのアドレスが自動的に変換先としてお勧めします。
5. これでもサード パーティ製アプリの機能 (サポートする`NSUserActivity`) ため、ユーザーは、ライドシェア アプリに切り替えることができますおよびレストランのアドレスが自動的に提案が宛先としてもします。
6. それも提供していますコンテキスト Siri、ユーザーがレストランのアプリ内で Siri を呼び出すし、確認できるように *「... の方向を取得」* Siri ユーザーが表示してレストランへの道順が提供されます。

1 つを共通に上記の機能のすべては、候補の当初からの場所を示すすべて。 上記の例の場合は、架空のレストランのレビューのアプリを勧めします。

いくつかのわずかな変更と既存のフレームワークに追加されたものからのアプリについて、この機能を有効にするには、iOS 10 を強化されています。

- `NSUserActivity` アプリ内で表示されている場所の情報をキャプチャするための追加のフィールドがあります。
- いくつかが追加されました MapKit と CoreSpotlight に場所をキャプチャします。
- 場所の認識機能が、Siri、マップ、キーボード、マルチタス キング、およびシステム内で他のアプリに追加されました。

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

次に、場所のテキスト ベースの説明は、テキスト ベースのインスタンス (QuickType キーボード) などの必要です。

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

緯度と経度は省略できますが、ユーザーが含まれる必要があるため、それらを送信するアプリが望んでいる正確な場所にルーティングされたことを確認します。

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

電話番号を設定するには、アプリにアクセスできる Siri ユーザーは、アプリから Siri を言っておきたい次のように呼び出すことができますので *「この場所を呼び出す」*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

最後に、アプリは、インスタンスがナビゲーションおよび電話の適切なかどうかを示します。

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>連絡先の相互作用を実装します。

新しい ios 10 で通信アプリは密接に統合された連絡先カードから連絡先アプリ。 連絡先の相互作用を実装しているアプリの場合は、ユーザーは連絡先で特定のユーザーを特定のアプリの情報を追加できます。 場合、たとえばがメッセージを送信するカードの上部にあるクイック アクション ボタンをクリックして、接続されているアプリからメッセージを送信するオプションとして表示されます。

サード パーティ製アプリが選択されている場合記憶し、既定の方法が、次回、ユーザーがパートナーに連絡する特定の人物をメッセージとして表示することができます。

アプリを使用して、連絡先の相互作用が実装されている`NSUserActivity`と iOS 10 で導入された新しい Intents フレームワーク。 インテントの操作方法の詳細については、「、 [SiriKit の概念について](~/ios/platform/sirikit/understanding-sirikit.md)と[SiriKit の実装](~/ios/platform/sirikit/implementing-sirikit.md)ガイド。

#### <a name="donating-interactions"></a>相互作用を寄付します。

アプリが相互作用を寄付する方法を参照してください。

[![](proactive-suggestions-images/activity04.png "寄付の相互作用の概要")](proactive-suggestions-images/activity04.png#lightbox)

アプリを作成、`INInteraction`オブジェクトを含む、**インテント**(`INIntent`)、**参加者**と**メタデータ**。 **インテント**映像通話を行うやテキスト メッセージの送信などのユーザー アクションを表します。 **参加者**連絡を受け取るユーザーが含まれます。 **メタデータ**メッセージなどを正常に送信するなどの追加情報を定義します。

決して開発者直接のインスタンスを作成`INIntent`または`INIntentResponse`、(ユーザーの代理としてアプリを実行するタスクに基づいて) 特定の子クラスのいずれかを使用するこれらの親クラスから継承します。 たとえば、`INSendMessageIntent`と`INSendMessageIntentResponse`テキスト メッセージを送信します。 

相互作用を完全に作成したらを呼び出して、`DonateInteraction`相互作用が使用できるシステムに通知するメソッド。

相互作用取得バンドルされている、ユーザーが連絡先カードからアプリを`NSUserActivity`、使用してアプリを起動します。

[![](proactive-suggestions-images/activity05.png "相互作用を取得、アプリの起動に使用される NSUserActivity にバンドルされています")](proactive-suggestions-images/activity05.png#lightbox)

メッセージを送信する目的の次の例を参照してください。

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

このコードの詳細を見ると、それを作成し、インスタンスを設定します。 `NSUserActivity` (ように、[アクティビティを作成する](#creating-an-activity)前のセクション)。 次のインスタンスを作成`INSendMessageIntent`(から継承される`INIntent`) して送信されるメッセージの詳細を設定します。

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse`を作成し、`NSUserActivity`上記で作成しました。

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction`送信メッセージの目的から構築された (`INSendMessageIntent`) と応答 (`INSendMessageIntentResponse`) 作成しました。

```csharp
var interaction = new INInteraction (intent, response);
```

最後に、システムには、相互作用の通知です。

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

完了ハンドラーは、アプリが、献血の成功または失敗に応答できるで渡されます。

### <a name="activities-best-practices"></a>アクティビティのベスト プラクティス

Apple は、アクティビティを使用する場合、次のベスト プラクティスを示しています。

- 使用`NeedsSave`限定的なペイロードの更新プログラム。
- 現在のアクティビティへの強い参照を保持することを確認します。
- 状態を復元するのに十分な情報を含む小さなペイロードを転送するだけです。
- アクティビティの型識別子で指定する逆引き DNS 表記を使用して、一意でわかりやすいが確認します。 

## <a name="schemaorg"></a>Schema.org

上記のよう`NSUserActivity`画面で、ユーザーが現在作業にどのような情報を理解するのに。 Schema.org では、web ページへと同等の機能を追加します。

Schema.org は場所ベースの対話処理の web サイトの同じ型を提供できます。 Apple では、ネイティブ アプリでのように、Safari で表示したときと同様に動作する新しい場所の提案が設計されています。

Schema.org 背景:

- オープンな web マークアップ ボキャブラリ標準を提供します。
- Web ページで構造化されたメタデータを含めることによって動作します。
- 使用可能なさまざまな概念を表す 500 以上のスキーマがあります。
- Web サイトを実装すること、により、開発者を取得できますを使用する利点のいくつか`NSUserActivity`ネイティブ アプリでします。

スキーマは、特定の種類などの構造と同様にツリー構造*レストラン*などのより汎用的な型から継承*地元の営業*します。 詳細についてを参照してください[Schema.org](http://schema.org)します。

たとえば、web ページには、次のデータが含まれているとします。

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

ユーザーが Safari では、このページを表示し、別のアプリに切り替え場合、ページから位置情報をキャプチャおよび (アクティビティ上に表示) と、システムの他の部分で、場所の修正候補として提供されています。

次のスキーマ プロパティのいずれかに準拠した web ページで何も safari が抽出されます。

- **PostalAddress**
- **GeoCoordinates**
- 電話プロパティです。

詳細についてを参照してください、 [Web マークアップを使って検索](~/ios/platform/search/web-markup.md)ガイド。

## <a name="consuming-location-suggestions"></a>消費場所の提案

このセクションでは、(マップ アプリ) など、システムまたはその他のサード パーティ製アプリの他の部分から取得されている場所の提案を消費して説明します。

アプリは場所の提案を利用できる 2 つの主な方法はあります。

- QuickType キーボード。
- ルーティングのアプリで位置情報を使用によって直接します。

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>場所の提案や QuickType キーボード

アプリがテキスト ベース形式でアドレスを扱う場合、アプリは、QuickType UI を使用して場所の入力候補の利用できます。 iOS 10 では、次の機能が QuickType UI を展開します。

- アプリは、ui テキスト フィールドのセマンティックな目的についてのヒントを追加できます。
- アプリは、アプリでプロアクティブな候補を取得できます。
- アプリは、強化されたオート コレクトを利用できます。

新しい`TextContentType`iOS 10 でテキストのフィールド コントロールのプロパティにより、開発者は、ユーザーは、特定のフィールドに入力しようとします。 値のセマンティックな目的を定義します。 例:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

システムに通知、アプリが特定のフィールドに完全な住所を入力するユーザーを想定します。 これにより、ユーザーがこのフィールドに値を入力するときは、キーボードの場所の提案を自動的に提供する QuickType キーボードが許可されます。

次にいくつかの開発者に使用できる一般的な型の`UITextContentType`静的クラス。

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>ルーティングのアプリと場所の提案

このセクションを参照してください、ルーティングのアプリ内から直接場所の提案を消費してになります。 既存のこの機能を追加するルーティングのアプリでは、開発者が活用されます`MKDirectionsRequest`フレームワークとして次のとおりです。

- マルチタスクでアプリを昇格します。
- ルーティングのアプリとしてアプリを登録するには
- 処理を MapKit でアプリを起動する`MKDirectionsRequest`オブジェクト。
- IOS については、適切なタイミングに、ユーザーにアプリを提案する機能を提供するには、ユーザー エンゲージメントに基づいています。

MapKit で、アプリの起動時に`MKDirectionsRequest`オブジェクトの場合に自動的に始めると、要求された場所にユーザーの指示またはを指示の取得を開始するユーザーを容易にする UI を表示する必要があります。 例:


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

新しい ios 10 でアプリを送信できますでそのアドレスをエンコードする必要がある開発者に地理座標がないアドレス。

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>メディア アプリの提案

アプリが任意の種類のポッド キャスト アプリまたはオーディオ/ビデオを 10、ios などのストリーミング メディア コンテンツなどのメディアを処理する場合、システムでメディアの提案を活用がかかることです。

メディアを処理するアプリ、iOS では、次の動作をサポートしています。

- iOS では、以前の動作に基づいて、ユーザーが使用する可能性のアプリを昇格します。
- スポット ライトや、[今日] ビューで、アプリに関連する推奨事項が表示されます。
- を、アプリが次のトリガーの 1 つを満たす場合は、ロック画面の提案に昇格させる可能性があります。
    - ヘッドホンまたは Bluetooth デバイスを接続した後は、接続を作成します。
    - 後、車を取得します。
    - 自宅で到着するまたは作業します。 

IOS 10 でシンプルな API 呼び出しを含めることによって、開発者は、メディア アプリのユーザーに対してより魅力的なロック画面を作成できます。 使用して、`MPPlayableContentManager`メディアの再生、フル メディア コントロール (など、Music アプリによって提示されたもの) を管理するクラスは、アプリのロック画面に表示されます。


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>まとめ

この記事では、プロアクティブな候補をカバーし、開発者に使用方法、Xamarin.iOS アプリにトラフィックを促進するを示しましたが。 プロアクティブな候補を実装する手順について説明し、使用方法のガイドラインを提示します。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [SiriKit のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
