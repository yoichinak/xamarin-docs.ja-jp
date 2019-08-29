---
title: Xamarin でのプロアクティブな提案の概要
description: この記事では、Xamarin iOS アプリでプロアクティブな提案を使用して、システムが有益な情報をユーザーに事前に自動的に提示できるようにすることで、エンゲージメントを促進する方法について説明します。
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 294616e7766a6613a014495e5f9d124f74465072
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121414"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Xamarin でのプロアクティブな提案の概要

_この記事では、Xamarin iOS アプリでプロアクティブな提案を使用して、システムが有益な情報をユーザーに事前に自動的に提示できるようにすることで、エンゲージメントを促進する方法について説明します。_

IOS 10 を初めて使用する場合は、ユーザーが適切なタイミングで有益な情報をユーザーに事前に自動的に提示することにより、ユーザーが Xamarin. iOS アプリを利用するためのニュースを提示することができます。

iOS 10 では、システムが適切なタイミングで有益な情報をユーザーに事前に自動的に提示できるようにすることで、アプリのエンゲージメントを促進する新しい方法を提供します。 スポットライト、ハンドオフ、Siri 候補 ([新しい検索 api](~/ios/platform/search/index.md)を参照) を使用してアプリにディープ検索を追加できるようになったのと同じように、ios 10 では、アプリは次の場所からシステムによってユーザーに提示される機能を公開することができます。:

- アプリスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 候補

アプリは`NSUserActivity`、web マークアップ、コアスポットライト、mapkit、Media Player、uikit などのテクノロジのコレクションを使用して、この機能をシステムに公開します。 さらに、アプリの事前提案のサポートを提供することで、Siri 統合を無料で利用できるようになります。

## <a name="location-based-suggestions"></a>場所に基づく提案

IOS 10 の新機能で`NSUserActivity`あるクラスに`MapItem`は、開発者が他のコンテキストで使用できる場所情報を提供できるようにするプロパティが含まれています。 たとえば、アプリがレストランのレビューを表示する場合、開発者は`MapItem` 、ユーザーがアプリで表示しているレストランの場所にプロパティを設定できます。 ユーザーが Maps アプリに切り替えた場合、レストランの場所は自動的に使用できるようになります。

アプリがアプリ検索をサポートしている場合は、 `CSSearchableItemAttributesSet`クラスの新しいアドレスコンポーネントを使用して、ユーザーがアクセスする場所を指定できます。 `MapItem`プロパティを設定すると、その他のプロパティは自動的に入力されます。

アドレスコンポーネントのプロパティの`Latitude`および`Longitude`を設定するだけでなく、アプリでもプロパティ`NamedLocation`と`PhoneNumbers`プロパティを指定することをお勧めします。これにより、siri は場所への呼び出しを開始できます。

## <a name="web-markup-based-suggestions"></a>Web マークアップに基づく提案

iOS 9 は、ユーザーがスポットライトや Safari の検索結果に表示するコンテンツを強化する、構造化されたデータマークアップを web サイトに追加する機能を追加しました (「 [Web マークアップを使用した検索](~/ios/platform/search/web-markup.md)」を参照してください)。 iOS 10 では、場所ベースのマークアップ ( [Schema.org](http://schema.org/)で定義されている[PostalAddress](http://schema.org/PostalAddress)など) を追加して、ユーザーのエクスペリエンスをさらに強化する機能が追加されています。 たとえば、ユーザーが web サイト上のページとしてマークされている場所を表示する場合、マップを開くときに同じ場所を提案できます。

## <a name="text-based-suggestions"></a>テキストベースの候補

UIKit が iOS 10 で拡張され、 [Uitextinputtraits](https://developer.apple.com/reference/uikit/uitextinputtraits)クラスの[textcontenttype](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype)プロパティが含まれるようになりました。これにより、テキスト領域の内容の意味を指定します。 この情報を使用すると、通常、システムは適切なキーボードの種類を自動的に選択し、オートコレクトの提案を改善し、他のアプリや web サイトから情報を事前に統合できます。

たとえば、というテキストフィールド`UITextContentType.FullStreetAddress`にユーザーがテキストを入力する場合、システムは、ユーザーが最近表示した場所でフィールドを自動入力することを提案できます。

## <a name="media-based-suggestions"></a>メディアベースの提案

アプリで[MPPlayableContentManager](xref:MediaPlayer.MPPlayableContentManager) API を使用してメディアを再生する場合、iOS 10 ではユーザーがアルバムアートを表示し、ロック画面でアプリを使用してメディアを再生できます。

## <a name="contextual-siri-reminders"></a>コンテキスト Siri リマインダー

ユーザーは、Siri を使用して、後でアプリで現在表示しているコンテンツをすぐに表示することができます。 たとえば、アプリでレストランのレビューを表示していた場合、Siri を呼び出して、 *「自宅にいるときにこのことを通知する*」と言うことができます。 Siri は、アプリ内のレビューへのリンクを含むリマインダーを生成します。

## <a name="contact-based-suggestions"></a>連絡先に基づく提案

アプリの連絡先 (および連絡先に関連する情報) を、システムに格納されているすべてのユーザーの連絡先と共に、iOS デバイスの**contact**アプリに表示できるようにします。

## <a name="ride-sharing-based-suggestions"></a>乗り物に基づく提案

乗り物共有アプリで[Mkdirection Srequest](xref:MapKit.MKDirectionsRequest) API が使用されている場合、iOS 10 は、ユーザーが乗り物を望む可能性があるときに、アプリスイッチャーにオプションとして表示します。 また、 `Info.plist`ファイルの[mkdirection sapplicationsupportedmodes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html)キーのを指定し`MKDirectionsModeRideShare`て、アプリを乗り物共有アプリとして登録する必要があります。

アプリが乗り物の共有のみをサポートしている場合、システムの提案は *"get a 乗り物.* .." から始まります。他の種類のルーティング方向 (ウォーキングや自転車など) がサポートされている場合、システムは *"案内の表示*" を使用します。

> [!IMPORTANT]
> アプリが受信する[Mkmapitem](xref:MapKit.MKMapItem)オブジェクトには、経度と緯度の情報が含まれておらず、ジオコーディングが必要です。

## <a name="implementing-proactive-suggestions"></a>プロアクティブな提案の実装

Xamarin iOS アプリに事前提案サポートを追加するのは、通常、いくつかの Api を実装したり、アプリが既に実装している可能性のあるいくつかの Api を拡張したりするのと同じほど簡単です。

プロアクティブな提案は、次の3つの主な方法でアプリを操作します。

- **`NSUserActivity`また、Schema.org**  -  `NSUserActivity`を使用すると、ユーザーが現在画面上でどのような情報を操作しているかをシステムが理解するのに役立ちます。 Schema.org は、web ページに同様の機能を追加します。
- **場所の提案**-アプリが場所ベースの情報を提供または使用する場合、これらの API 拡張機能は、この情報をアプリ間で共有するための新しい方法を提供します。
- **メディアアプリの提案**-システムは、iOS デバイスとのユーザー操作のコンテキストに基づいて、アプリとそのメディアコンテンツを昇格させることができます。

とは、次のものを実装することによって、アプリでサポートされます。

-  - ハンドオフがサポートされるように、iOS 8 でハンド`NSUserActivity`オフが追加されました。これにより、開発者は1つのデバイスでアクティビティを開始して、別のデバイスで続行できます (「[ハンドオフの概要」を](~/ios/platform/handoff.md)参照してください)。
- **スポットライト検索**-iOS 9 では、を使用して`NSUserActivity`スポットライト検索結果内からアプリコンテンツを昇格する機能が追加されました (「[コアスポットライトを使用した検索](~/ios/platform/search/corespotlight.md)」を参照してください)。
- **コンテキスト siri リマインダー** -iOS 10 では`NSUserActivity` 、ユーザーが現在表示しているコンテンツを後で表示するために siri がすばやくリマインダーを行うことができるように拡張されました。
- **場所の提案**-iOS 10 `NSUserActivity`は、アプリ内で表示される場所をキャプチャし、システム全体のさまざまな場所で昇格するように拡張します。
- **コンテキスト siri 要求** -  `NSUserActivity`では、アプリ内に表示される情報を siri に提供するため、ユーザーはアプリ内から siri を呼び出すことができます。
- **連絡先相互作用**-iOS 10 の新`NSUserActivity`機能では、通信アプリを連絡先カード (Contacts アプリ内) から別の通信方法として昇格させることができます。

これらの機能はすべて、1つのフォームで使用さ`NSUserActivity`れるか、またはその機能を提供するために使用されます。 

## <a name="nsuseractivity"></a>NSUserActivity

前述のように`NSUserActivity` 、では、ユーザーが現在どのような情報を画面で操作しているかをシステムが理解するのに役立ちます。 `NSUserActivity`は、アプリ内を移動するときにユーザーのアクティビティをキャプチャする軽量の状態のキャッシュメカニズムです。 たとえば、レストランアプリを見ているとします。

[![](proactive-suggestions-images/activity02.png "NSUserActivity のライトウェイト状態キャッシュメカニズム")](proactive-suggestions-images/activity02.png#lightbox)

次のような相互作用があります。

1. ユーザーがアプリ`NSUserActivity`で作業するときに、アプリの状態を後で再作成するためにが作成されます。
2. ユーザーがレストランを検索する場合は、アクティビティの作成と同じパターンが適用されます。
3. ここでも、ユーザーが結果を表示したとき。 この最後の例では、ユーザーが場所を表示していて、iOS 10 では、特定の概念 (場所や通信の相互作用など) を認識しています。

最後の画面を詳しく見てみましょう。

[![](proactive-suggestions-images/activity03.png "NSUserActivity の詳細")](proactive-suggestions-images/activity03.png#lightbox)

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

開発者は、これが上記で作成したアクティビティと同じ`com.xamarin.platform`種類の id () であることを確認する必要があります。 アプリは、 `NSUserActivity`に格納されている情報を使用して、ユーザーが中断した場所に状態を戻します。

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
2. ユーザーがマルチタスクアプリスイッチャーを使用してアプリから移動すると、お気に入りのナビゲーションアプリを使用してレストランへの指示を得るために、(画面の下部に) 提案が自動的に表示されます。
3. ユーザーがメッセージアプリに切り替えて、 *「Let to 会い*in」と入力し始めた場合、quicktype キーボードは自動的にレストランのアドレスへの貼り付けを提案します。
4. ユーザーが Maps アプリに切り替えた場合、レストランの住所は自動的に宛先として提案されます。
5. これはサードパーティ製のアプリ (をサポート`NSUserActivity`) に対しても機能します。そのため、ユーザーは、乗り物共有アプリに切り替えることができ、レストランのアドレスも宛先として自動的に提案されます。
6. また、Siri にコンテキストが提供されるので、ユーザーはレストランアプリ内で Siri を呼び出して、 *"取得方向.* .." を尋ね、siri は、ユーザーが表示しているレストランへの指示を提供します。

上記の機能はすべて共通しています。これらはすべて、提案がもともとどこから来ているかを示しています。 上記の例の場合は、架空のレストランレビューアプリです。

iOS 10 は、いくつかの小さな変更や既存のフレームワークへの追加によってアプリでこの機能を有効にするように強化されています。

- `NSUserActivity`には、アプリ内で表示される場所情報をキャプチャするための追加のフィールドがあります。
- 位置情報をキャプチャするために、MapKit と CoreSpotlight にいくつかの追加が行われています。
- 場所を認識する機能が、システム内の Siri、マップ、キーボード、マルチタスク、およびその他のアプリに追加されました。

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

次に、テキストベースのインスタンス (QuickType キーボードなど) には、場所のテキストベースの説明が必要です。

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

緯度と経度は省略可能ですが、アプリが送信する場所にユーザーがルーティングされていることを確認する必要があります。

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

電話番号を設定することにより、アプリは Siri にアクセスできるようになります。これにより、ユーザーは、 *"次の場所に電話をかける"* のように指示することにより、アプリから siri を呼び出すことができます。

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

最後に、アプリは、インスタンスがナビゲーションと通話に適しているかどうかを示すことができます。

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>連絡先のやり取りの実装

IOS 10 の新方式では、通信アプリは連絡先カードから連絡先アプリに緊密に統合されています。 連絡先の対話が実装されているアプリの場合、ユーザーは特定のアプリの情報を連絡先の特定のユーザーに追加できます。 たとえば、カードの上部にある [クイックアクション] ボタンをクリックしてメッセージを送信した場合、添付されたアプリは、からメッセージを送信するオプションとして表示されます。

サードパーティのアプリが選択されている場合は、ユーザーが次に連絡するときに、指定したユーザーにメッセージを表示する既定の方法として、そのアプリを記憶して表示することができます。

連絡先の相互作用は、および iOS `NSUserActivity` 10 で導入された新しいインテントフレームワークを使用してアプリに実装されます。 インテントの操作の詳細については、「 [SiriKit の概念につい](~/ios/platform/sirikit/understanding-sirikit.md)て」および「 [sirikit](~/ios/platform/sirikit/implementing-sirikit.md)ガイドの実装」を参照してください。

#### <a name="donating-interactions"></a>寄付の相互作用

アプリがどのように相互作用を寄贈できるかを見てみましょう。

[![](proactive-suggestions-images/activity04.png "寄付の相互作用の概要")](proactive-suggestions-images/activity04.png#lightbox)

アプリ`INInteraction`は、**インテント**(`INIntent`)、**参加者**、および**メタデータ**を含むオブジェクトを作成します。 **インテント**は、ビデオ通話の作成やテキストメッセージの送信などのユーザーアクションを表します。 **参加者**には、通信を受けている人が含まれます。 **メタデータ**は、メッセージを正常に送信するなどの追加情報を定義します。

開発者は、または`INIntent` `INIntentResponse`のインスタンスを直接作成することはありません。これらの親クラスから継承する特定の子クラスの1つ (ユーザーの代わりにアプリが実行するタスクに基づく) を使用します。 たとえば、テキスト`INSendMessageIntent`メッセージ`INSendMessageIntentResponse`を送信する場合は、とします。 

相互作用が完全に設定されたら`DonateInteraction` 、メソッドを呼び出して、相互作用が使用可能であることをシステムに通知します。

ユーザーが連絡先カードからアプリを操作すると、相互作用はにバンドル`NSUserActivity`され、その後、アプリを起動するために使用されます。

[![](proactive-suggestions-images/activity05.png "相互作用は、アプリを起動するために使用される NSUserActivity にバンドルされます。")](proactive-suggestions-images/activity05.png#lightbox)

メッセージ送信の目的の次の例を見てみましょう。

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

このコードについて詳しく説明すると、の`NSUserActivity`インスタンスを作成して設定します (前の「アクティビティの[作成](#creating-an-activity)」セクションを参照)。 次に、の`INSendMessageIntent`インスタンス (から`INIntent`継承) を作成し、送信されるメッセージの詳細を設定します。

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

が作成され、上記`NSUserActivity`で作成したが渡されます。 `INSendMessageIntentResponse`

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

は、送信メッセージのインテント (`INSendMessageIntent`) と、次のよう`INSendMessageIntentResponse`に作成された応答 () から構築されます。`INInteraction`

```csharp
var interaction = new INInteraction (intent, response);
```

最後に、システムは相互作用の通知を行います。

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
  // Handle donation error
  ...
});
```

完了ハンドラーは、アプリが成功または失敗した寄付に応答できる場所に渡されます。

### <a name="activities-best-practices"></a>活動のベストプラクティス

Apple では、アクティビティを操作するときに、次のベストプラクティスを提案しています。

- 遅延`NeedsSave`ペイロードの更新に使用します。
- 現在のアクティビティへの強い参照を保持するようにしてください。
- 状態を復元するのに十分な情報だけを含む小さなペイロードのみを転送します。
- アクティビティの種類の識別子が一意であり、逆引き DNS 表記を使用して指定されていることを確認します。 

## <a name="schemaorg"></a>Schema.org

上に示した`NSUserActivity`ように、は、ユーザーが現在どのような情報を画面で操作しているかをシステムが理解するのに役立ちます。 Schema.org は、web ページに同様の機能を追加します。

Schema.org は、web サイトに対して同じ種類の場所での対話を提供できます。 Apple では、ネイティブアプリの場合と同様に、Safari で表示するときにも、新しい場所の提案を行うように設計されています。

Schema.org の背景:

- オープンな web マークアップボキャブラリ標準が用意されています。
- Web ページに構造化されたメタデータを含めることで機能します。
- 使用可能なさまざまな概念を表す500を超えるスキーマがあります。
- 開発者は、web サイトに実装することで、ネイティブアプリでを使用`NSUserActivity`する利点の一部を取得できます。

スキーマは、ツリーのような構造で配置されます。たとえば、*レストラン*などの特定の型は、*ローカルビジネス*などの汎用的な型から継承されます。 詳細については、「 [Schema.org](http://schema.org)」を参照してください。

たとえば、web ページに次のデータが含まれているとします。

```html
<script type="application/ld+json">
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

ユーザーが Safari でこのページにアクセスし、別のアプリに切り替えた場合、ページの場所情報がキャプチャされ、システムの他の部分 (上記のアクティビティに示されているように) で場所の提案として提供されます。

Safari は、次のいずれかのスキーマプロパティに準拠した web ページ上の任意のものを抽出します。

- **PostalAddress**
- **GeoCoordinates**
- 電話のプロパティ。

詳細については、 [Web マークアップを使用した検索に](~/ios/platform/search/web-markup.md)関するガイドを参照してください。

## <a name="consuming-location-suggestions"></a>場所の提案の使用

次のセクションでは、システムの他の部分 (Maps アプリなど) または他のサードパーティ製アプリから取得した場所に関する推奨事項について説明します。

アプリが場所の提案を使用する主な方法は2つあります。

- QuickType キーボードを使用します。
- ルーティングアプリ内の場所情報を直接使用する。

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>場所の提案と QuickType キーボード

アプリがテキストベースの形式でアドレスを扱う場合、アプリは QuickType UI を使用して場所の提案を利用できます。 iOS 10 では、次の機能を使用して QuickType UI が展開されます。

- アプリでは、UI のテキストフィールドのセマンティックインテントに関するヒントを追加できます。
- アプリでは、アプリでプロアクティブな提案を取得できます。
- アプリには、enhanced オートコレクトを利用できます。

IOS 10 `TextContentType`のテキストフィールドコントロールの新しいプロパティを使用すると、開発者は、ユーザーが特定のフィールドに入力する値のセマンティックインテントを定義できます。 例えば:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

は、ユーザーが指定されたフィールドに完全な住所を入力することをアプリケーションが想定していることをシステムに伝えます。 これにより、ユーザーがこのフィールドに値を入力したときに、QuickType キーボードによってキーボードの場所に関する提案が自動的に提供されるようになります。

次に、 `UITextContentType`静的クラスの開発者が使用できる一般的な型をいくつか示します。

- `Name`
- `GivenName`
- `FamilyName`
- `Location`
- `FullStreetAddress`
- `AddressCityAndState`
- `TelephoneNumber`
- `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>アプリと場所をルーティングする提案

このセクションでは、ルーティングアプリ内から直接場所の提案を利用する方法について説明します。 この機能を追加するルーティングアプリでは、開発者は次の`MKDirectionsRequest`ように既存のフレームワークを活用します。

- アプリをマルチタスクで昇格させることができます。
- アプリをルーティングアプリとして登録します。
- Mapkit `MKDirectionsRequest`オブジェクトを使用したアプリの起動を処理する場合は。
- IOS に、ユーザーのエンゲージメントに基づいて、適切なタイミングでユーザーにアプリを提案する機能を提供します。

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

IOS 10 の新動作では、地理的座標のないアドレスをアプリに送信できます。この場合、開発者はアドレスをエンコードする必要があります。

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
  // Handle the display of the address
  
});

```

## <a name="media-app-suggestions"></a>メディアアプリの提案

IOS 10 を使用して、ポッドキャストアプリや、オーディオやビデオなどのストリーミングメディアコンテンツなどのあらゆる種類のメディアをアプリで処理する場合、システムでメディア候補を利用できます。

メディアを処理するアプリでは、iOS は次の動作をサポートしています。

- iOS では、ユーザーが以前の動作に基づいて使用する可能性が高いアプリを昇格させます。
- アプリに関連する提案は、スポットライトと今日ビューに表示されます。
- アプリが次のいずれかのトリガーを満たしている場合は、ロック画面の提案に昇格される可能性があります。
  - ヘッドホンに接続した後、または Bluetooth デバイスが接続を確立します。
  - 車に入った後。
  - 自宅または職場に到着した後。 

IOS 10 に単純な API 呼び出しを含めることにより、開発者はメディアアプリのユーザーに対してより魅力的なロック画面エクスペリエンスを作成できます。 `MPPlayableContentManager`クラスを使用してメディアの再生を管理することで、アプリのロック画面にフルメディアコントロール (音楽アプリによって表示されるものと同様) が表示されます。


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

## <a name="summary"></a>Summary

この記事では、プロアクティブな提案について説明し、開発者が Xamarin. iOS アプリへのトラフィックを促進する方法について説明しました。 ここでは、プロアクティブな提案を実装し、使用ガイドラインを提示する手順について説明します。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [SiriKit プログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
