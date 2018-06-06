---
title: Xamarin.iOS で NSUserActivity で検索
description: このドキュメントでは、メディア、および Safari で検索可能なので、NSUserActivity のインデックスを作成する方法について説明します。 これには、検索結果に、NSUserActivity の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4b053f66e9b6b7715cbe52c4e43d9db32db48f4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788209"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Xamarin.iOS で NSUserActivity で検索

`NSUserActivity` iOS 8 で導入されたし、ハンドオフ用にデータを提供するために使用します。
渡すことのできる、、さまざまな iOS デバイスで実行されているアプリの別のインスタンスに、アプリの特定の部分でアクティビティを作成することができます。 受信デバイスは、前のデバイス、ユーザーが中断権利を手で開始されたアクティビティを続行できます。 ハンドオフの使用に関する詳細についてを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)ドキュメント。

IOS 9 に新しい`NSUserActivity`(パブリックとプライベートの両方) をインデックス化および Spotlight 検索でおよび Safari から検索できます。 マークすることによって、`NSUserActivity`検索と追加のインデックス可能なメタデータとしてアクティビティを表示して、iOS デバイスの検索結果にします。

[![](nsuseractivity-images/apphistory01.png "アプリの履歴の概要")](nsuseractivity-images/apphistory01.png#lightbox)

ユーザーは、アプリからのアクティビティに属する検索結果を選択する場合、アプリを起動して、によって記述されたアクティビティ、`NSUserActivity`再起動し、ユーザーに表示されます。

次のプロパティの`NSUserActivity`アプリの検索をサポートするために使用します。

 - `EligibleForHandoff` – `true`、ハンドオフ操作でこのアクティビティを使用することができます。
 - `EligibleForSearch` – `true`、このアクティビティは、デバイス上のインデックスに追加し、検索結果に表示されます。
 - `EligibleForPublicIndexing` – `true`、このアクティビティは Apple のクラウド ベースのインデックスに追加されが既にインストールされていないアプリ、iOS デバイスで (検索) を使用してユーザーに表示されます。 参照してください、[パブリック検索インデックス](#Public-Search-Indexing)詳細については、後述の「します。
 - `Title` –、活動のタイトルを提供し、検索結果が表示されます。 ユーザーは、それ自体のタイトルのテキストも検索できます。
 - `Keywords` –、インデックスを作成し、エンドユーザーが、検索可能にする、アクティビティの記述に使用される文字列の配列です。
 - `ContentAttributeSet` – は、`CSSearchableItemAttributeSet`をさらに、アクティビティの詳細を説明し、検索結果に豊富なコンテンツを提供するために使用します。
 - `ExpirationDate` – 特定の日付にのみ表示されるアクティビティをする場合は、ここでその日付を指定できます。
 - `WebpageURL` アクティビティは、web で表示できる場合、または、アプリは、Safari のディープ リンクをサポートしている場合は、こちらをご覧へのリンクを設定できます。

## <a name="nsuseractivity-quickstart"></a>NSUserActivity クイック スタート

検索可能な実装を次の手順に従って`NSUserActivity`アプリ。

- [アクティビティの種類の識別子を作成します。](#creatingtypeid)
- [アクティビティを作成します。](#createactivity)
- [アクティビティへの応答](#respondactivity)
- [パブリック検索インデックス作成](#indexing)

いくつかを使用する必要がある[付加的なメリット](#benefits)を使用する`NSUserActivity`コンテンツを検索できるようにします。

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>アクティビティの種類の識別子を作成します。

検索の利用状況を作成するには、作成する必要があります、_アクティビティ型識別子_に識別します。 アクティビティ型識別子は、短い文字列に追加、`NSUserActivityTypes`のアプリの配列**Info.plist**ファイルが指定したユーザー アクティビティの種類を一意に識別するために使用します。 配列内のアプリをサポートします。 アプリの検索に公開される各アクティビティを 1 つのエントリがあります。 

Apple では、競合を避けるためのアクティビティ型識別子の逆引き DNS 形式表記を使用してを提案します。 例:`com.company-name.appname.activity`ベースのアクティビティの特定のアプリまたは`com.company-name.activity`複数のアプリを実行できるアクティビティのためです。

作成するときにアクティビティの型識別子が使用される、`NSUserActivity`アクティビティの種類を識別するインスタンス。 検索結果をタップすると、ユーザーの結果として、アクティビティを続行すると、(アプリのチーム ID) と共に、アクティビティの種類は、アクティビティを続行するを起動するには、どのアプリを決定します。

この動作をサポートするために必要なアクティビティの型識別子を作成するには、編集、 **Info.plist**ファイルに切り替えると、**ソース**ビュー。 追加、`NSUserActivityTypes`キーし、次の形式の識別子を作成します。

[![](nsuseractivity-images/type01.png "NSUserActivityTypes キーと plist エディターで必要な識別子")](nsuseractivity-images/type01.png#lightbox)

上記の例で新しい検索の利用状況のアクティビティ型識別子を 1 つを作成しました (`com.xamarin.platform`)。 内容を置き換える独自のアプリを作成するときに、`NSUserActivityTypes`アプリのアクティビティに固有のアクティビティ型の識別子を持つ配列をサポートしています。

<a name="createactivity" />

## <a name="creating-an-activity"></a>アクティビティを作成します。

ホストされているデバイス上のインデックス検索のアクティビティを作成する例を次に示します。

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

追加でしたさらに詳細を設定して、`ContentAttributeSet`のプロパティ、`NSUserActivity`次のようにします。

[![](nsuseractivity-images/apphistory02.png "追加の検索の詳細の概要")](nsuseractivity-images/apphistory02.png#lightbox)

使用して、`ContentAttributeSet`エンドユーザーがこれらと対話するように仕向ける多機能な検索結果を作成することができます。

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>アクティビティへの応答

検索結果をタップすると、ユーザーに応答する (`NSUserActivity`)、アプリケーションでは、編集、 **<code>appdelegate.cs</code>** ファイルし、オーバーライド、`ContinueUserActivity`メソッドです。 例えば:

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

ハンドオフ要求に応答するために使用同じメソッドのオーバーライドであることに注意してください。 今すぐ、ユーザーは、Spotlight 検索で結果で、アプリからのリンクをクリックすると、アプリが前面に (まだ実行されていない場合、開始) したりするコンテンツ、ナビゲーションやそのリンクによって表される機能が表示されます。

[![](nsuseractivity-images/apphistory03.png "検索からの以前の状態を復元します。")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>パブリック検索インデックス作成

上で見たよう iOS 9 やすく検索コンテンツに対するアクセス アプリと、ユーザーが既に検出および特定の iOS デバイスで使用する機能を提供します。 IOS 9 パブリック インデックスを使用する方法をユーザーのコンテンツや機能をまだ検出したしていない (またはユーザーもアプリがインストールされていない) 提供すぎる検索する場合、それらの結果を取得します。

パブリックのインデックス作成は、次のように動作します。

1. アプリのアクティビティを作成する場合は、パブリックとしてマークできます。
2. パブリックのアクティビティは Apple に送信され、クラウドでインデックスを作成します。
3. 多くのユーザーはデバイスでアプリと対話するとして、特定の機能や、アクティビティによって表されるコンテンツを使用するとのランクが増加します。
4. 人気のあるパブリック結果は、アプリがインストールされていない場合でも、他のユーザーに利用可能なされます。
5. (アクティビティには、URL が含まれている) 場合、これらのパブリックの結果が Spotlight 検索で、Safari で表示されます。

、上で作成したプライベート検索アクティビティを実行して、パブリックにする展開します。

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

パブリックのインデックスを設定して、アクティビティが設定されているからといって`EligibleForPublicIndexing = true`、するには自動的にインデックスに追加する Apple のパブリック クラウドとは限りません。 まず、次の条件を満たす必要があります。

1. 検索結果に表示する必要がありますして多数のユーザーを選択します。 結果までプライベート アクティビティ engagement しきい値が満たされているです。
2. アプリのプロビジョニングには、ユーザー固有のデータをインデックスが作成され、公開されているができないようにします。

<a name="benefits" />

## <a name="additional-benefits"></a>その他の利点

使用してアプリの検索を採用することで`NSUserActivity`アプリで取得することも、次の機能。

- **ハンドオフ**- アプリの検索コンテンツ、ナビゲーションが公開やハンドオフとして同じメカニズムを使用した機能ので (`NSUserActivity`)、アプリケーションのユーザーが 1 つのデバイス上でアクティビティを開始して別の続行を許可することが簡単にします。
- **Siri 提案**-Siri 候補では通常、標準の推奨事項、およびアプリからの在職者できますで自動的に提案します。
- **Siri スマート アラーム**-ユーザー Siri アプリからのアクティビティに関する通知を確認することができます。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [ブログの投稿とサンプル](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
