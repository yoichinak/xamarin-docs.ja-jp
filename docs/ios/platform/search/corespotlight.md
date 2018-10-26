---
title: Xamarin.iOS でコア スポット ライト検索
description: このドキュメントでは、Xamarin.iOS アプリケーションでコア スポット ライトを使用して、アプリ内のコンテンツへのリンクを提供する方法について説明します。 これには、作成、復元、更新、および検索可能な項目を削除する方法について説明します。
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: fb9ddcc39bd33199dc370897250cd0d74597612f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110356"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Xamarin.iOS でコア スポット ライト検索

コア スポット ライトは、ios 9 を追加、編集、またはアプリ内のコンテンツへのリンクを削除するデータベースのような API を提供する新しいフレームワークです。 コア スポット ライトを使用して追加された項目は、iOS デバイスでの Spotlight 検索で使用できるになります。

コンテンツの種類の例についてはインデックスを作成できるコア スポット ライトを使用して、アプリを Apple のメッセージ、メール、予定表、ノートを確認します。 現在、これらはすべては、検索結果を提供するのにコア スポット ライトを使用します。

## <a name="creating-an-item"></a>項目を作成します。

項目を作成して、コア スポット ライトを使用してインデックス作成の例を次に示します。

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

[![](corespotlight-images/corespotlight01.png "コア スポット ライト検索結果の概要")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>アイテムを復元します。

ユーザーが、アプリでは、コア スポット ライトを使用して、検索結果に追加の項目をタップしたときに、`AppDelegate`メソッド`ContinueUserActivity`が呼び出されます (このメソッドはの使用も`NSUserActivity`)。 例えば:

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

この時間となっているアクティビティのチェックに注意してください、`ActivityType`の`CSSearchableItem.ActionType`します。

## <a name="updating-an-item"></a>アイテムの更新

タイトルまたはサムネイル画像の変更が必要ななどとコア スポット ライトで作成したインデックス項目が変更する必要がある可能性があります。 この変更を行うには、最初に、インデックスの作成に使用されたに同じメソッドを使用します。
作成、新しい`CSSearchableItem`項目を作成して、新しい接続に使用されたのと同じ ID を使用して`CSSearchableItemAttributeSet`変更された属性を含みます。

[![](corespotlight-images/corespotlight02.png "項目の概要の更新")](corespotlight-images/corespotlight02.png#lightbox)

この項目が検索可能なインデックスに書き込まれると、既存の項目は、新しい情報で更新されます。

## <a name="deleting-an-item"></a>アイテムの削除

コア スポット ライトには、必要でなくなったときに、インデックス アイテムを削除する複数の方法が用意されています。

最初に、その識別子によって項目をたとえば削除できます。

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
## <a name="additional-core-spotlight-features"></a>スポット ライトの追加のコア機能

コア スポット ライトでは、正確で最新の状態は、インデックスを維持するため、次の機能があります。

- **更新プログラムのサポートをバッチ**– にバッチ全体を送信できる場合は、アプリを作成または同時に多数のインデックスのグループを変更する必要があります、`Index`のメソッド、 `CSSearchableIndex` 1 回の呼び出し内のクラス。
- **インデックスの変更に応答する**– を使用して、`CSSearchableIndexDelegate`検索可能なインデックスからアプリの変更と通知に応答できます。
- **データ保護を適用**– データ保護のクラスを使用して、セキュリティを実装できるコア スポット ライトを使用して、検索可能なインデックスに追加する項目。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索のプログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
