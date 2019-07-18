---
title: Xamarin.iOS で NSUserActivity で検索
description: このドキュメントでは、スポット ライトや Safari で検索可能なので、NSUserActivity は、インデックスを作成する方法について説明します。 これには、検索結果で、NSUserActivity の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: a3a8872ae54d7b1efb55ee71286ca5ea479616e0
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830164"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Xamarin.iOS で NSUserActivity で検索

`NSUserActivity` iOS 8 で導入され、ハンドオフにデータを提供するために使用します。
別の iOS デバイスで実行されているアプリの別のインスタンス渡すことができるアプリの特定の部分でアクティビティを作成することができます。 受信側のデバイスは、前のデバイスでは、ユーザーのところから右の集荷開始したアクティビティを続行できます。 ハンドオフの使用に関する詳細についてを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)ドキュメント。

IOS 9 に新しい`NSUserActivity`(パブリックとプライベートの両方) をインデックス化し、スポット ライト検索および Safari から検索されることができます。 マークすることによって、`NSUserActivity`として追加し、検索インデックス可能なメタデータでは、iOS デバイスの検索結果に、アクティビティを表示できます。

[![](nsuseractivity-images/apphistory01.png "アプリの履歴の概要")](nsuseractivity-images/apphistory01.png#lightbox)

アプリが起動され、によって、アクティビティが説明されている場合は、ユーザーがアプリからのアクティビティに属している検索結果を選択、`NSUserActivity`再起動し、ユーザーに表示されます。

次のプロパティの`NSUserActivity`アプリの検索をサポートするために使用されます。

- `EligibleForHandoff` – `true`、ハンドオフ操作でこのアクティビティを使用できます。
- `EligibleForSearch` – `true`、このアクティビティは、デバイス上のインデックスに追加し、検索結果に表示されます。
- `EligibleForPublicIndexing` – `true`、このアクティビティが Apple のクラウド ベースのインデックスに追加し、まだインストールしていないアプリ、iOS デバイスで (検索) 経由でユーザーに表示されます。 参照してください、[パブリック検索インデックス作成](#public-search-indexing)詳細については後述します。
- `Title` -アクティビティのタイトルを提供する、検索結果に表示されます。 ユーザーがそれ自体のタイトルのテキストの検索もできます。
- `Keywords` – をインデックス化し、エンドユーザーが、検索可能には、アクティビティの記述に使用される文字列の配列です。
- `ContentAttributeSet` – は、`CSSearchableItemAttributeSet`さらに、アクティビティの詳細を説明し、検索結果のリッチ コンテンツを提供するために使用します。
- `ExpirationDate` 特定の日付にのみ表示されるアクティビティをする場合は、ここで、その日付を行うことができます。
- `WebpageURL` – アクティビティは、web で表示できる場合、または、アプリは、Safari のディープ リンクをサポートしている場合は、ここにアクセスしてへのリンクを設定できます。

## <a name="nsuseractivity-quickstart"></a>NSUserActivity クイック スタート

この手順に従って、検索可能な実装を`NSUserActivity`アプリ。

- [アクティビティの種類の識別子を作成します。](#creatingtypeid)
- [アクティビティを作成します。](#createactivity)
- [アクティビティへの応答](#respondactivity)
- [パブリックの検索インデックスの作成](#indexing)

いくつかあります[特典](#benefits)を使用する`NSUserActivity`コンテンツを検索できるようにします。

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>アクティビティの種類の識別子を作成します。

検索アクティビティを作成するには、作成する必要があります、_アクティビティ型識別子_に識別します。 アクティビティの型識別子は短い文字列に追加、`NSUserActivityTypes`のアプリの配列**Info.plist**ファイルが指定したユーザー アクティビティの種類を一意に識別するために使用します。 配列内のアプリをサポートし、アプリの検索に公開される各アクティビティの 1 つのエントリがあります。 

Apple では、アクティビティの型識別子の逆引き DNS スタイルの表記を使用して競合を避けるためお勧めします。 例:`com.company-name.appname.activity`特定のアプリ ベースのアクティビティまたは`com.company-name.activity`の複数のアプリで実行できるアクティビティ。

作成するときに、アクティビティの型識別子が使用される、`NSUserActivity`アクティビティの種類を識別するインスタンス。 検索結果をタップして、ユーザーの結果として、アクティビティを続行すると、(アプリのチーム ID) と共に、アクティビティの種類は、アクティビティの続行を起動するには、どのアプリを決定します。

この動作をサポートするために必要なアクティビティの型識別子を作成するには、編集、 **Info.plist**ファイルに切り替えると、**ソース**ビュー。 追加、`NSUserActivityTypes`キーし、次の形式の識別子を作成します。

[![](nsuseractivity-images/type01.png "NSUserActivityTypes キーと plist エディターで識別子が必要です。")](nsuseractivity-images/type01.png#lightbox)

上記の例では、1 つの新しいアクティビティ型識別子の検索アクティビティを作成しました (`com.xamarin.platform`)。 内容を置き換える独自のアプリを作成するときに、`NSUserActivityTypes`アプリ、アクティビティに固有のアクティビティ型の識別子を持つ配列をサポートしています。

<a name="createactivity" />

## <a name="creating-an-activity"></a>アクティビティを作成します。

ホストされているデバイスでインデックス検索のアクティビティの作成の例を次に示します。

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

使用して、`ContentAttributeSet`エンドユーザーに連携するように仕向けるの高度な検索結果を作成することができます。

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>アクティビティへの応答

検索結果をタップすると、ユーザーに応答する (`NSUserActivity`)、アプリでは、編集、 **AppDelegate.cs**オーバーライド ファイルを開き、`ContinueUserActivity`メソッド。 例えば:

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

これは同じメソッドのオーバーライド ハンドオフ要求に応答するために使用であることに注意してください。 ここで、ユーザーは、Spotlight 検索結果で、アプリからのリンクをクリックすると、アプリがある前面に移動する (またはまだ実行されていない場合は、開始) とコンテンツ、ナビゲーションまたはそのリンクによって表されるフィーチャーが表示されます。

[![](nsuseractivity-images/apphistory03.png "検索からの以前の状態を復元します。")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>パブリックの検索インデックスの作成

上記のとおり、iOS 9 簡単に検索アクセス権をアプリのコンテンツと、ユーザーが既に探索され、特定の iOS デバイスで使用される機能を提供します。 IOS 9 パブリック インデックス作成では、ユーザーのコンテンツや機能をまだ検出したしていない (または、アプリをインストールしていないでもユーザー) の方法を提供しますすぎる、検索でそれらの結果を取得します。

パブリックのインデックス作成は、次のように動作します。

1. アプリのアクティビティを作成するときに、public としてマークできます。
2. パブリックのアクティビティは Apple に送信され、クラウドでインデックスを作成します。
3. 多くのユーザーは、デバイスでアプリと対話し、特定の機能や、アクティビティによって表されるコンテンツを使用して、そのランクよう増加します。
4. インストールされているアプリがあるない場合でも、人気のあるパブリック結果は、その他のユーザーが使用できるようになります。
5. (アクティビティには、URL が含まれている) 場合、これらのパブリックの結果と Safari の Spotlight 検索で表示されます。

前に作成したプライベート検索の利用状況をパブリックにする展開します。

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

パブリックのインデックス作成を設定して、アクティビティが設定されているからといって`EligibleForPublicIndexing = true`、いることが自動的にインデックスに追加 Apple のパブリック クラウドとは限りません。 まず、次の条件を満たす必要があります。

1. 検索結果に表示する必要があり、多くのユーザーに選択します。 アクティビティの engagement しきい値が満たされるまで、結果が得られたプライベートです。
2. アプリのプロビジョニングには、インデックスが作成され、公開されてから、ユーザーに固有のデータができないようにします。

<a name="benefits" />

## <a name="additional-benefits"></a>その他の特典

使用してアプリの検索を採用することによって`NSUserActivity`アプリでは、取得することも、次の機能。

- **ハンドオフ**- アプリの検索、ナビゲーション コンテンツが公開やハンドオフとして同じメカニズムを使用して機能するため (`NSUserActivity`)、アクティビティを 1 つのデバイスに開始し、別の作業を再開して、アプリケーションのユーザーが簡単に許可することができます。
- **Siri 提案**-Siri の推奨事項は通常、標準の推奨事項、と共に、アプリからの在職者に自動的に推奨します。
- **Siri スマート アラーム**-ユーザーは Siri ことをアプリからのアクティビティに関する通知を確認できます。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索のプログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [ブログの投稿とサンプル](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
