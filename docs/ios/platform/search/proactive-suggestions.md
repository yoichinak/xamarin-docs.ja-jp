---
title: プロアクティブな推奨事項の概要
description: ここでは、システムが事前に有用な情報をユーザーに自動的に表示できるように、ドライブ エンゲージメントを Xamarin.iOS アプリでプロアクティブな提案を使用する方法を説明します。
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5b06dbf0e8e108616adb4f77910267aaa1ac71f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-proactive-suggestions"></a>プロアクティブな推奨事項の概要

_ここでは、システムが事前に有用な情報をユーザーに自動的に表示できるように、ドライブ エンゲージメントを Xamarin.iOS アプリでプロアクティブな提案を使用する方法を説明します。_

初めて使用する iOS 10、存在のニュース方法をユーザーに適切な時点でのユーザーに自動的に有用な情報を事前に存在して Xamarin.iOS アプリと情報交換にプロアクティブなご提案します。

iOS 10 では、事前対応的に提示する役に立つ情報自動的にユーザーに適切な時点ではシステムによって推進エンゲージメント アプリへの新しい方法を表示します。 同様に iOS 9 がスポット ライト、ハンドオフ Siri 提案を使用して、アプリに詳細な検索を追加する機能を提供 (を参照してください[新しい検索 Api](~/ios/platform/search/index.md))、iOS 10 でアプリを提示するユーザーに、システムによって内から機能を公開できます、次の場所:

- アプリのスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 提案

アプリがなど、テクノロジのコレクションを使用して、システムにこの機能を公開`NSUserActivity`コアのスポット ライト、MapKit、Media Player および UIKit web マークアップ。 さらに、アプリをプロアクティブ提案のサポートを提供する取得 Siri の緊密な統合無料します。

## <a name="location-based-suggestions"></a>場所に基づいて候補

新しい iOS 10 に、`NSUserActivity`クラスが含まれています、`MapItem`により、開発者は他のコンテキストで使用できる場所の情報を提供するプロパティです。 たとえば、アプリには、レストランのレビューが表示されている場合、開発者を設定できます、`MapItem`プロパティをユーザーがアプリで表示してレストランの場所。 場合は、ユーザーは、マップのアプリに切り替え、レストランの場所は自動的に利用可能です。

新しいアドレス コンポーネントを使用できるアプリは、アプリの検索をサポートする場合、`CSSearchableItemAttributesSet`クラスが、ユーザーがアクセスする可能性がある場所を指定します。 設定して、`MapItem`プロパティ、その他のプロパティが自動的に入力します。

設定に加えて、`Latitude`と`Longitude`アドレス コンポーネントのプロパティのことをお勧め、アプリを指定する、`NamedLocation`と`PhoneNumbers`プロパティもため Siri を開始できる場所への呼び出しです。

## <a name="web-markup-based-suggestions"></a>Web マークアップに基づいて候補

iOS 9 ユーザーがメディアおよび Safari の検索結果に表示される内容を拡充する web サイトで構造化データのマークアップを含むことのできるに追加 (を参照してください[Web マークアップを含む検索](~/ios/platform/search/web-markup.md))。 iOS 10 場所ベースのマークアップを含むことのできるを追加します (など[住所](http://schema.org/PostalAddress)で定義されている[Schema.org](http://schema.org/)) をさらに、ユーザーのエクスペリエンスを強化します。 たとえば、場所、ユーザー ビューには、web サイトのページが設定されると、システム推測できます。 同じ場所マップを開いたときにします。

## <a name="text-based-suggestions"></a>テキスト ベースの推奨事項

UIKit は iOS に含める 10 で展開されています、 [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype)のプロパティ、 [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits)クラスがテキスト領域の内容の特別な意味を指定します。 この情報、システムできます通常自動的に適切なキーボードの種類を選択、オート コレクトの提案を改善およびその他のアプリと web サイトからの情報を事前に統合します。

たとえば、ユーザーが設定されたテキスト フィールドにテキストを入力する`UITextContentType.FullStreetAddress`システムは自動的に入力フィールドをユーザーが最後に表示する場所を推測できます。

## <a name="media-based-suggestions"></a>メディアに基づいて候補

アプリを使用してメディアを再生する場合、 [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) API、iOS 10 ユーザー アルバム アートを表示して、ロック画面で、アプリを使って、メディアを再生することができます。

## <a name="contextual-siri-reminders"></a>コンテキスト Siri アラーム

Siri を使用して簡単に現在表示しているアプリで、後で、コンテンツの表示を求めるメッセージを作成することができます。 たとえば、アプリでレストランのレビューを表示していた場合、でした Siri を呼び出すし、言う*「通知についてホーム出た場合」。* Siri は、アプリで、レビューへのリンクを使用してアラームを生成します。

## <a name="contact-based-suggestions"></a>連絡先に基づいて候補

アプリの許可に表示される連絡先 (および連絡先の関連する情報)、**にお問い合わせください**システムに格納されているすべてのユーザー、連絡先と、iOS デバイスのアプリです。

## <a name="ride-sharing-based-suggestions"></a>車に基づいて推奨事項の共有

飛行共有アプリで使用する場合、 [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) API、iOS 10 が表示されますが、アプリのスイッチャーのオプションとしてときに、ユーザーは素敵する可能性があります。 指定して、飛行共有アプリとしてアプリが登録もする必要があります、`MKDirectionsModeRideShare`の[MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html)キーをその`Info.plist`ファイル。

アプリのみをサポートしている変更を共有する場合、その提案をシステムで開始*"に変更を Get..."*、ルーティングの方向 (ウォーキングまたは自転車) などの他の種類はサポートされている場合、システムが使用*「するための指示を取得しています...」*

> [!IMPORTANT]
> [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/)アプリを受信するオブジェクトの緯度と経度の情報を含まない場合がありますおよびジオコード化が必要になります。

## <a name="implementing-proactive-suggestions"></a>プロアクティブな推奨事項を実装します。

Xamarin.iOS アプリには、サポートは事前対応型の候補を追加することは通常、いくつかの Api を実装するか、アプリが実装することがある、いくつかの Api を拡張すると同じくらい簡単です。

プロアクティブな推奨事項は、3 つの主な方法で、アプリと使用します。

- **`NSUserActivity` および Schema.org**  -  `NSUserActivity`システムが画面上で、ユーザーが現在処理してどのような情報を理解するのに役立ちます。 Schema.org は、web ページへのような機能を追加します。
- **場所提案**- アプリの提供またはアプリ間でこの情報を共有する場所については、これら API 拡張オファーの新しい方法を使用します。
- **メディア アプリ提案**- システムは、アプリを昇格させることができ、iOS デバイスでのユーザーの操作のコンテキストに基づいて、そのメディア コンテンツ。

次を実装することによって、アプリでサポートされているとします。

- **ハンドオフ** -  `NSUserActivity` handoff ことにより、開発者は、1 つのデバイスでアクティビティを開始し、別の続行をサポートするために iOS 8 で追加されました (を参照してください[ハンドオフの概要](~/ios/platform/handoff.md))。
- **検索のスポット ライト**-iOS 9 を使用して Spotlight 検索で結果内からのアプリのコンテンツを昇格する機能を追加する`NSUserActivity`(を参照してください[コア Spotlight 検索](~/ios/platform/search/corespotlight.md))。
- **コンテキストの Siri アラーム**- iOS 10、`NSUserActivity`が拡張され、ユーザーが現在表示して、アプリで、後で、コンテンツの表示を求めるメッセージを簡単に Siri を使用するをします。
- **場所提案**-iOS 10 の強化`NSUserActivity`をアプリ内での表示場所をキャプチャし、それらをシステム全体で多くの場所で昇格します。
- **コンテキストの Siri 要求** -  `NSUserActivity`ユーザーを取得できます方向、または場所に、呼び出しをする、アプリ内から呼び出す Siri Siri をアプリ内で説明する情報にコンテキストを提供します。
- **相互作用にお問い合わせください**iOS 10 の新機能 -`NSUserActivity`代替通信方法として、連絡先アプリ) の「アドレス帳カードから昇格させる通信アプリを許可します。

1 つの点に共通のすべてのこれらの機能がある、すべての使用`NSUserActivity`で 1 つのフォームまたは別の機能を提供します。 

## <a name="nsuseractivity"></a>NSUserActivity

、前に述べたよう`NSUserActivity`、システムが画面上で、ユーザーが現在処理してどのような情報を理解するのに役立ちます。 `NSUserActivity` 軽量状態は、アプリ間を移動できるように、ユーザーのアクティビティをキャプチャするためのメカニズムをキャッシュします。 たとえば、レストランのアプリを確認します。

[![](proactive-suggestions-images/activity02.png "キャッシュ メカニズム NSUserActivity 軽量の状態")](proactive-suggestions-images/activity02.png#lightbox)

次の相互作用: と

1. ユーザーがアプリを合わせて、`NSUserActivity`が後で、アプリの状態を再作成を作成します。
2. ユーザーは、レストランを検索する場合は、同じパターンのアクティビティを作成するが実行されます。
3. もう一度、ユーザーが、結果を表示するとします。 この最後の場合、ユーザーの場所が表示および 10、iOS では、システムがいくつかの概念 (場所、または通信の相互作用) などのさらに注意してください。

最後の画面で詳しく見てをみましょう。

[![](proactive-suggestions-images/activity03.png "NSUserActivity 詳細")](proactive-suggestions-images/activity03.png#lightbox)

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

検索結果をタップすると、ユーザーに応答する (`NSUserActivity`)、アプリの編集、 **<code>appdelegate.cs</code>**ファイルし、オーバーライド、`ContinueUserActivity`メソッドです。 例えば:

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

開発者が同じアクティビティの型識別子は、このことを確認する必要があります (`com.xamarin.platform`) 上で作成したアクティビティとして。 アプリに格納されている情報を使用して、`NSUserActivity`にユーザーが中断状態を復元します。

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
2. ユーザーは、マルチタスク アプリ スイッチャーを使用してアプリから移動すると、システムが自動的に表示提案を送信 (画面の下部)、お気に入りのナビゲーション アプリを使用してレストランへの道順を取得します。
3. 場合は、ユーザーがメッセージのアプリに切り替え、入力を開始*「では満たしてみましょう」*、QuickType キーボードは、レストランのアドレスを貼り付けることを自動的に表示します。
4. ユーザーは、マップのアプリに切り替え場合、レストランのアドレスが自動的に変換先として推奨されています。
5. これは、サード パーティ製アプリに対しても機能 (をサポートする`NSUserActivity`) ので、飛行共有アプリをユーザーが切り替えることができ、レストランのアドレスが自動的に提案があるの保存先としてもします。
6. コンテキストに提供 Siri、ため、ユーザーがレストランのアプリ内で Siri を呼び出すことができます、依頼して*「... の方向を取得」* Siri はユーザーが表示してレストランへの道順を提供します。

1 つの点を共通の上記の機能のすべてから、修正案を最初の位置を示すため、それらはすべて。 上記の例の場合は、架空のレストランのレビュー アプリを勧めします。

いくつかの変更を加えると既存のフレームワークへの追加を介してアプリについて、この機能を有効にするには、iOS 10 を強化されています。

- `NSUserActivity` アプリ内で表示される場所の情報をキャプチャするための他のフィールドがあります。
- いくつかの点に加えられた MapKit と CoreSpotlight に場所をキャプチャします。
- Siri、地図、キーボード、マルチタス キングとシステム内で他のアプリを場所の対応する機能が追加されました。

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

場合、場所のテキスト ベースの説明が必要 (QuickType キーボード) などのテキスト ベースのインスタンスです。

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

緯度と経度オプションですが、ユーザーが扱う必要がありますので、送信するアプリが求めている正確な場所にルーティングされるようにします。

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

電話番号を設定することにより、アプリがアクセスできる Siri はユーザーが呼び出せる Siri アプリから次のようを言うことにより、 *「呼び出すこの場所は」*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

最後に、アプリでは、インスタンスが移動や電話をかけるのために適切なかどうかを示します。

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>連絡先の相互作用を実装します。

新しい 10、iOS でアプリの通信が密接に統合連絡先カードからアドレス帳アプリにします。 連絡先の相互作用を実装しているアプリの場合、ユーザーは、アドレス帳の特定のユーザーに特定のアプリの情報を追加できます。 およびなど、メッセージを送信するカードの上部にあるクイック アクション ボタンをクリックして場合は、接続されているアプリはからメッセージを送信するオプションとして表示されます。

サード パーティのアプリが選択されている場合は記憶して特定の人物で、次回ユーザーがそれらを接続する必要があるメッセージに既定の方法として表示されます。

アプリを使用して、連絡先の相互作用が実装されている`NSUserActivity`と iOS 10 で導入された新しいインテント フレームワークです。 目的の操作の詳細についてを参照してください、 [SiriKit 概念について](~/ios/platform/sirikit/understanding-sirikit.md)と[実装 SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)ガイドです。

#### <a name="donating-interactions"></a>寄付の相互作用

アプリが相互作用を寄付する方法を参照してください。

[![](proactive-suggestions-images/activity04.png "寄付の相互作用の概要")](proactive-suggestions-images/activity04.png#lightbox)

アプリを作成、`INInteraction`オブジェクトを含む、**インテント**(`INIntent`)、**参加者**と**メタデータ**です。 **インテント**映像通話またはテキスト メッセージを送信するなどのユーザー アクションを表します。 **参加者**通信を受信する人が含まれます。 **メタデータ**メッセージなどを正常に送信するなどの追加情報を定義します。

しない開発者は、直接のインスタンスを作成`INIntent`または`INIntentResponse`、(ユーザーの代理としてアプリを実行するタスクに基づく) 特定の子クラスの 1 つ使用するこれらの親クラスから継承します。 たとえば、`INSendMessageIntent`と`INSendMessageIntentResponse`テキスト メッセージを送信するためです。 

相互作用を完全に作成した後の呼び出し、`DonateInteraction`の相互作用が使用できるシステムに通知するメソッド。

相互作用を取得にバンドルされているユーザーが連絡先カードからアプリケーションを操作するときに、 `NSUserActivity`、アプリの起動に使用されます。

[![](proactive-suggestions-images/activity05.png "相互作用を取得、アプリの起動に使用される NSUserActivity にバンドルされています。")](proactive-suggestions-images/activity05.png#lightbox)

送信メッセージの目的の次の例を参照してください。

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

このコードの詳細を見ると、その作成およびデータ設定のインスタンス`NSUserActivity`(のように、[アクティビティを作成する](#Creating-an-Activity)前のセクション)。 次のインスタンスを作成`INSendMessageIntent`(から継承される`INIntent`) し、送信されるメッセージの詳細を追加します。

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse`が作成され、渡された、`NSUserActivity`上記で作成します。

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction`送信メッセージの目的から構築された (`INSendMessageIntent`) と応答 (`INSendMessageIntentResponse`) 作成しました。

```csharp
var interaction = new INInteraction (intent, response);
```

最後に、システムの相互作用の通知のです。

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

完了ハンドラーは、アプリが donation 成功または失敗に応答できるに渡されます。

### <a name="activities-best-practices"></a>アクティビティのベスト プラクティス

Apple のアクティビティを使用する場合、次のベスト プラクティスを示しています。

- 使用して`NeedsSave`ペイロードの限定的な更新プログラム。
- 現在のアクティビティへの強い参照を保持することを確認します。
- 状態を復元するのに十分な情報を含む小さなペイロードを転送だけです。
- アクティビティの型識別子が一意でわかりやすい名前である逆引き DNS 表記を使用して、それらを指定することを確認します。 

## <a name="schemaorg"></a>Schema.org

、上記のように`NSUserActivity`、システムが画面上で、ユーザーが現在処理してどのような情報を理解するのに役立ちます。 Schema.org は、web ページへのような機能を追加します。

Schema.org には、同じ種類の web サイトに基づく位置の相互作用を提供できます。 Apple には、ネイティブ アプリで同じように、Safari で表示したときに、でも同じように動作する新しい場所の提案が設計されています。

一部の Schema.org 背景:

- 開いている web マークアップ ボキャブラリ標準を提供します。
- Web ページで構造化されたメタデータを含めることによって動作します。
- 使用可能なさまざまな概念を表す 500 以上のスキーマがあります。
- 実装すると、web サイトで、開発者を取得できます。 使用する利点のいくつか`NSUserActivity`ネイティブ アプリでします。

スキーマがツリー構造に固有の仕様がなど型、構造体と同様に*レストラン*などのよりジェネリック型から継承*ローカル ビジネス*です。 詳細についてを参照してください[Schema.org](http://schema.org)です。

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

ユーザーは、Safari でこのページを表示にし、別のアプリに切り替え、ページから位置情報はキャプチャし、提供場所修正案として、システムの他の部分 (上記のアクティビティに表示)。

Safari は、次のスキーマ プロパティのいずれかに準拠している web ページで何も抽出します。

- **PostalAddress**
- **GeoCoordinates**
- 電話のプロパティです。

詳細についてを参照してください、 [Web マークアップを含む検索](~/ios/platform/search/web-markup.md)ガイドです。

## <a name="consuming-location-suggestions"></a>場所の提案を使用

このセクションでは、(マップ アプリ) など、システムまたはその他のサード パーティ製アプリの他の部分からの場所の提案を消費して説明します。

アプリは場所の提案を使用できる 2 つの方法があります。

- QuickType キーボードによる。
- 直接によってルーティング アプリ内の場所情報を使用するには。

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>場所のご意見と QuickType キーボード

アプリがテキスト ベースの形式でアドレスを扱う場合、アプリは、場所は提案 QuickType UI を使用して利用できます。 iOS 10 では、次の機能と共に QuickType UI を展開します。

- アプリは、UI にテキスト フィールドのセマンティックの目的についてのヒントを追加できます。
- アプリは、アプリでプロアクティブな候補を取得することができます。
- アプリは、オート コレクトの強化を活用できます。

新しい`TextContentType`iOS 10 内のテキスト フィールド コントロールのプロパティにより、開発者は、ユーザーが特定のフィールドに入力する予定の値のセマンティックの目的を定義します。 例えば:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

システムに通知、アプリがユーザーに指定されたフィールドの完全住所の入力を期待しています。 これにより、ユーザーがこのフィールドに値を入力するとき、キーボードの場所の提案を自動的に提供する QuickType キーボードが許可されます。

開発者に使用できる一般的な種類のいくつかは、次のとおり、`UITextContentType`静的クラス。

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>ルーティングのアプリと場所の推奨事項

このセクションでは見てルーティング アプリ内から直接場所提案を使用します。 この機能を追加するルーティング、アプリの開発者が活用既存`MKDirectionsRequest`ようフレームワーク。

- マルチタスクでアプリを昇格します。
- ルーティング アプリとしてアプリを登録します。
- MapKit と、アプリの起動を処理するために`MKDirectionsRequest`オブジェクト。
- IOS の適切な時間は、ユーザーにアプリを提案する方法を説明する機能を提供するには、ユーザーの関与に基づいています。

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

新しい 10、iOS でアプリを送信できますアドレスをエンコードする必要がある開発者は、その原因で、地理的座標を持たないアドレス。

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>メディア アプリの推奨事項

アプリが任意の種類のポッド キャストのアプリやオーディオ、ビデオ、10、ios などのストリーミング メディア コンテンツなどのメディアを処理する場合に利用できますメディア提案のシステムで。

メディアを処理するアプリの場合は、iOS には、次の動作がサポートされています。

- iOS は、以前の動作に基づいて、ユーザーが頻繁に使用するアプリを昇格させます。
- アプリに関連する推奨事項は、スポット ライトと今日ビューに表示されます。
- アプリでは、次のトリガーの 1 つを満たして場合、は、ロック画面の修正案に昇格させる可能性があります。
    - ヘッドホンまたは Bluetooth デバイスを接続した後は、接続を作成します。
    - 後車で取得します。
    - 自宅で到着するまたは作業します。 

10、iOS で簡単な API 呼び出しを含めることによって、開発者は、メディア アプリのユーザーに対してより魅力的なロック画面経験を作成できます。 使用して、`MPPlayableContentManager`メディアの再生、フル メディアのようなコントロール音楽アプリによって提示されるもの) を管理するクラスは、アプリのロック画面に表示されます。


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

この記事がプロアクティブな推奨事項を説明し、開発者は、ことができますを使用する方法、Xamarin.iOS アプリへのトラフィックをドライブを示しました。 プロアクティブな推奨事項を実装する手順を説明し、使用方法のガイドラインを提示します。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [SiriKit プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
