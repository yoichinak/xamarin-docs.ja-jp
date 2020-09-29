---
title: Xamarin のコアスポットライトで検索する
description: このドキュメントでは、Xamarin iOS アプリケーションでコアスポットライトを使用して、アプリ内コンテンツへのリンクを提供する方法について説明します。 検索可能な項目を作成、復元、更新、削除する方法について説明します。
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 684e2b8ad5098dd9cecb14eb9c5e265e3ea53035
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436126"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Xamarin のコアスポットライトで検索する

コアスポットライトは、アプリ内のコンテンツへのリンクを追加、編集、または削除するためのデータベースに似た API を提供する iOS 9 用の新しいフレームワークです。 コアスポットライトを使用して追加された項目は、iOS デバイスのスポットライト検索で利用できます。

コアスポットライトを使用してインデックスを作成できるコンテンツの種類の例については、Apple のメッセージ、メール、予定表、メモアプリを参照してください。 現在、すべてのユーザーがコアスポットライトを使用して検索結果を提供しています。

## <a name="creating-an-item"></a>項目の作成

次に示すのは、項目を作成し、コアスポットライトを使用してインデックスを作成する例です。

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

検索結果には、次のような情報が表示されます。

[![コアスポットライト検索結果の概要](corespotlight-images/corespotlight01.png)](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>項目を復元する

アプリのコアスポットライトを使用して検索結果に追加された項目をユーザーがタップすると、 `AppDelegate` メソッド `ContinueUserActivity` が呼び出されます (このメソッドはにも使用され `NSUserActivity` ます)。 次に例を示します。

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

今度は、アクティビティのがであることを確認 `ActivityType` `CSSearchableItem.ActionType` します。

## <a name="updating-an-item"></a>項目を更新する

重要なスポットライトで作成したインデックス項目を変更する必要がある場合があります。たとえば、タイトルやサムネイル画像の変更が必要になる場合があります。 この変更を行うには、最初にインデックスを作成するときと同じ方法を使用します。
`CSSearchableItem`項目の作成に使用したものと同じ ID を使用して新しいを作成し、変更された属性を含む新しいをアタッチし `CSSearchableItemAttributeSet` ます。

[![項目の更新の概要](corespotlight-images/corespotlight02.png)](corespotlight-images/corespotlight02.png#lightbox)

この項目が検索可能なインデックスに書き込まれると、既存の項目が新しい情報で更新されます。

## <a name="deleting-an-item"></a>項目の削除

コアスポットライトは、不要になったインデックス項目を削除する複数の方法を提供します。

まず、識別子を使用して項目を削除できます。次に例を示します。

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

次に、ドメイン名を指定して、インデックス項目のグループを削除できます。 次に例を示します。

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

最後に、次のコードを使用して、すべてのインデックス項目を削除できます。

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

## <a name="additional-core-spotlight-features"></a>その他の主要なスポットライト機能

コアスポットライトには、インデックスを正確かつ最新の状態に保つために役立つ次の機能があります。

- **バッチ更新のサポート** –アプリで大規模なインデックスのグループを同時に作成または変更する必要がある場合は、バッチ全体を `Index` `CSSearchableIndex` 1 回の呼び出しでクラスのメソッドに送信できます。
- **インデックスの変更に応答する** –アプリを使用して、 `CSSearchableIndexDelegate` 検索可能なインデックスからの変更と通知に応答できます。
- **データ保護の適用** –データ保護クラスを使用して、コアスポットライトを使用して検索可能なインデックスに追加するアイテムにセキュリティを実装できます。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリ検索のプログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)