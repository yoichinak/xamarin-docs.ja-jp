---
title: Xamarin で NSUserActivity を検索します。 iOS
description: このドキュメントでは、NSUserActivity にインデックスを作成し、スポットライトや Safari で検索できるようにする方法について説明します。 ここでは、検索結果での NSUserActivity の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: c6ceb6e10abc4dbd26bffecbb6fefa5835f3d630
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031554"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Xamarin で NSUserActivity を検索します。 iOS

`NSUserActivity` は、iOS 8 で導入され、ハンドオフ用のデータを提供するために使用されます。
これにより、アプリの特定の部分でアクティビティを作成し、別の iOS デバイスで実行されているアプリの別のインスタンスに渡すことができます。 受信デバイスは、前のデバイスで開始されたアクティビティを続行して、ユーザーが中断した場所を右に移動できます。 ハンドオフの使用の詳細については、「[ハンドオフ](~/ios/platform/handoff.md)ドキュメントの概要」を参照してください。

IOS 9 を初めて使用する場合は、`NSUserActivity` (パブリックとプライベートの両方) にインデックスを作成し、スポットライト検索と Safari から検索することができます。 `NSUserActivity` を検索可能としてマークし、インデックス可能なメタデータを追加することにより、アクティビティは iOS デバイスの検索結果に一覧表示されます。

[![](nsuseractivity-images/apphistory01.png "The App History overview")](nsuseractivity-images/apphistory01.png#lightbox)

アプリからアクティビティに属する検索結果をユーザーが選択すると、アプリが起動され、`NSUserActivity` によって説明されているアクティビティが再起動され、ユーザーに表示されます。

アプリの検索をサポートするには、次の `NSUserActivity` のプロパティを使用します。

- `EligibleForHandoff` – `true`の場合、このアクティビティはハンドオフ操作で使用できます。
- `EligibleForSearch` – `true`場合、このアクティビティはデバイス上のインデックスに追加され、検索結果に表示されます。
- `EligibleForPublicIndexing` – `true`場合、このアクティビティは Apple のクラウドベースのインデックスに追加され、iOS デバイスにまだアプリがインストールされていないユーザーに (検索を使用して) 表示されます。 詳細については、以下の「[パブリック検索のインデックス作成](#public-search-indexing)」セクションを参照してください。
- `Title` –アクティビティのタイトルを提供し、検索結果に表示されます。 ユーザーは、タイトル自体のテキストを検索することもできます。
- `Keywords` –エンドユーザーがインデックスを作成し、検索可能にするアクティビティを説明するために使用される文字列の配列です。
- `ContentAttributeSet` –アクティビティを詳細に説明したり、検索結果に豊富なコンテンツを提供したりするために使用される `CSSearchableItemAttributeSet` です。
- `ExpirationDate` –特定の日付までアクティビティを表示する場合は、ここでその日付を指定できます。
- `WebpageURL`-web でアクティビティを表示できる場合、またはアプリが Safari のディープリンクをサポートしている場合は、ここにアクセスするようにリンクを設定できます。

## <a name="nsuseractivity-quickstart"></a>NSUserActivity のクイックスタート

アプリで検索可能な `NSUserActivity` を実装するには、次の手順に従います。

- [アクティビティの種類の識別子の作成](#creatingtypeid)
- [アクティビティの作成](#createactivity)
- [アクティビティへの応答](#respondactivity)
- [パブリック検索インデックス作成](#indexing)

`NSUserActivity` を使用してコンテンツを検索可能にすることには、いくつかの[利点](#benefits)があります。

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>アクティビティの種類の識別子の作成

検索活動を作成する前に、_アクティビティの種類_を識別するための識別子を作成する必要があります。 アクティビティの種類の識別子は、特定のユーザーアクティビティの種類を一意に識別するために使用される、アプリの**情報 plist**ファイルの `NSUserActivityTypes` 配列に追加される短い文字列です。 アプリがサポートし、アプリ検索に公開されるアクティビティごとに、配列に1つのエントリがあります。 

Apple では、競合を避けるために、アクティビティの種類の識別子に対して逆引き DNS スタイルの表記を使用することを提案しています。 例: 特定のアプリベースのアクティビティの `com.company-name.appname.activity`、または複数のアプリで実行できるアクティビティの `com.company-name.activity`。

アクティビティの種類の識別子は、アクティビティの種類を識別するために `NSUserActivity` インスタンスを作成するときに使用されます。 ユーザーが検索結果をタップしたときにアクティビティが続行されると、アクティビティの種類 (アプリのチーム ID と共に) によって、アクティビティを続行するために起動するアプリが決まります。

この動作をサポートするために必要なアクティビティの種類の識別子を作成するには、**情報の plist**ファイルを編集し、**ソース**ビューに切り替えます。 `NSUserActivityTypes` キーを追加し、次の形式で識別子を作成します。

[![](nsuseractivity-images/type01.png "The NSUserActivityTypes key and required identifiers in the plist editor")](nsuseractivity-images/type01.png#lightbox)

上の例では、search アクティビティ (`com.xamarin.platform`) の新しいアクティビティの種類の識別子を1つ作成しました。 独自のアプリを作成するときに、`NSUserActivityTypes` 配列の内容を、アプリがサポートするアクティビティに固有のアクティビティの種類の識別子に置き換えます。

<a name="createactivity" />

## <a name="creating-an-activity"></a>アクティビティの作成

次に、デバイス上でホストされる検索のアクティビティを作成する例を示します。

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

`NSUserActivity` の `ContentAttributeSet` プロパティを次のように設定して、詳細を追加できます。

[![](nsuseractivity-images/apphistory02.png "Addition Search Details overview")](nsuseractivity-images/apphistory02.png#lightbox)

`ContentAttributeSet` を使用することにより、エンドユーザーが対話できるようにする、豊富な検索結果を作成できます。

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>アクティビティへの応答

アプリの検索結果 (`NSUserActivity`) をタップしてユーザーに応答するには、 **AppDelegate.cs**ファイルを編集し、`ContinueUserActivity` メソッドをオーバーライドします。 (例:

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

これは、ハンドオフ要求への応答に使用されるメソッドオーバーライドと同じであることに注意してください。 これで、ユーザーがスポットライト検索結果のアプリからリンクをクリックすると、アプリがフォアグラウンドになり (まだ実行されていない場合は開始され)、そのリンクによって表されるコンテンツ、ナビゲーション、機能が表示されます。

[![](nsuseractivity-images/apphistory03.png "Restore Previous State from Search")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>パブリック検索インデックス作成

前述のように、iOS 9 では、ユーザーが特定の iOS デバイスで既に検出および使用しているアプリコンテンツと機能への検索アクセスを簡単に提供できるようになりました。 パブリックインデックス作成では、コンテンツや機能がまだ検出されていないユーザー (または、アプリをインストールしていないユーザー) が検索に結果を取得する方法を提供します。

パブリックインデックス作成は、次のように機能します。

1. アプリのアクティビティを作成するときに、そのアクティビティをパブリックとしてマークできます。
2. パブリックアクティビティは Apple に送信され、クラウドでインデックスが作成されます。
3. デバイス上のアプリと対話するユーザーが増えるにつれて、そのアクティビティによって表される特定の機能やコンテンツを使用できるようになると、順位が上がります。
4. アプリがインストールされていない場合でも、他のユーザーは一般的なパブリック結果を利用できます。
5. これらのパブリックな結果は、スポットライト検索および Safari に表示されます (アクティビティに URL が含まれている場合)。

上記で作成したプライベート検索アクティビティを実行し、それをパブリックに拡張することができます。

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

`EligibleForPublicIndexing = true`を設定することによってパブリックインデックス作成にアクティビティが設定されているので、Apple のパブリッククラウドインデックスに自動的に追加されるわけではありません。 最初に、次の条件を満たす必要があります。

1. 検索結果に表示され、多くのユーザーが選択できる必要があります。 アクティビティのエンゲージメントしきい値に達するまで、結果はプライベートのままです。
2. アプリのプロビジョニングでは、ユーザー固有のデータのインデックスを作成してパブリックにすることはできません。

<a name="benefits" />

## <a name="additional-benefits"></a>その他の利点

アプリで `NSUserActivity` を使用してアプリ検索を導入することで、次の機能も利用できます。

- **ハンドオフ**-アプリの検索では、ハンドオフ (`NSUserActivity`) と同じメカニズムを使用してコンテンツ、ナビゲーション、または機能を公開しているため、アプリのユーザーが1つのデバイスでアクティビティを開始して別のデバイスで続行できるようにすることが簡単にできます。
- **Siri の提案**-Siri の提案によって通常作成される標準の提案と共に、アプリからの在職者を自動的に提案することができます。
- **Siri スマートリマインダー** -ユーザーは、アプリからのアクティビティに関する通知を siri に要求できます。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリ検索のプログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [ブログ投稿 & サンプル](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
