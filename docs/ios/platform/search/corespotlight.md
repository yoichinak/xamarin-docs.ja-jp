---
title: Xamarin.iOS の中核となるメディアの検索
description: このドキュメントでは、Xamarin.iOS アプリケーションでのコアのスポット ライトを使用して、アプリ内のコンテンツへのリンクを提供する方法について説明します。 これには、作成、復元、更新、および検索可能な項目を削除する方法について説明します。
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a8bc3aaa43d7830b0a3baa0768d495458b1ecfad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788040"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Xamarin.iOS の中核となるメディアの検索

コア メディアは、iOS 9 を追加、編集、またはアプリ内のコンテンツへのリンクを削除するデータベースのような API を表示するための新しいフレームワークです。 スポット ライトのコアを使用して追加された項目は、iOS デバイスで Spotlight 検索で使用できるになります。

コンテンツの種類の例についてはインデックスを作成できるコアのスポット ライトを使用して、Apple のメッセージ、メール、予定表、ノートのアプリを確認します。 これらはすべて現在の検索結果を提供するのにコアのスポット ライトを使用します。

## <a name="creating-an-item"></a>項目を作成します。

項目を作成して、コア メディアを使用してインデックス作成の例を次に示します。

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

この情報は、検索結果では、次のように表示されます。

[![](corespotlight-images/corespotlight01.png "コア Spotlight 検索結果の概要")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>アイテムを復元します。

ユーザーが、アプリの主要なメディアを使用して、検索結果に追加された項目をタップしたときに、`AppDelegate`メソッド`ContinueUserActivity`と呼びます (このメソッドはの使用も`NSUserActivity`)。 例えば:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

この時刻となっているチェックにより、アクティビティに注意してください、`ActivityType`の`CSSearchableItem.ActionType`します。

## <a name="updating-an-item"></a>アイテムの更新

コアのスポット ライトで作成したインデックス項目する必要がある生じる変更するなど、タイトルまたはサムネイル画像の変更が必要である可能性があります。 この変更を行うには、最初に、インデックスの作成に使用されたように同じメソッドを使用します。
作成する新しい`CSSearchableItem`項目を作成し、新しい接続に使用された同じ ID を使用して`CSSearchableItemAttributeSet`変更後の属性を含みます。

[![](corespotlight-images/corespotlight02.png "項目の概要の更新")](corespotlight-images/corespotlight02.png#lightbox)

この項目を検索可能なインデックスに書き込むときに、既存の項目は、新しい情報で更新されます。

## <a name="deleting-an-item"></a>アイテムの削除

コアのスポット ライトは必要なくなりましたときに、インデックスの項目を削除するいくつかの方法を提供します。

最初に、項目を削除する、その識別子によって例。

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

次に、ドメイン名でインデックス項目のグループを削除することができます。 例えば:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

最後に、次のコードを含むすべてのインデックス項目を削除することができます。

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>スポット ライトの他の主要な機能

コアのスポット ライトでは、正確かつ最新の状態は、インデックスを維持するため、次の機能があります。

- **更新プログラムのサポートをバッチ**– にバッチ全体を送信できるアプリを作成または同時に多数のインデックスのグループを変更する場合は、`Index`のメソッド、`CSSearchableIndex`呼び出しは 1 つのクラスです。
- **インデックスの変更に応答**– を使用して、`CSSearchableIndexDelegate`アプリは、インデックスは検索から変更と通知に応答できます。
- **データ保護を適用**– データ保護のクラスを使用して、セキュリティを実装できるコアのスポット ライトを使用して、検索可能なインデックスに追加する項目にします。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
