---
title: Xamarin のアプリレビューを要求する
description: この記事では、Apple が iOS 10 に追加した RequestReview メソッドについて説明し、Xamarin で実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: b51eeb3594f1b41f8b8f7fdaedc7577138c4f5ba
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435672"
---
# <a name="request-app-review-in-xamarinios"></a>Xamarin のアプリレビューを要求する

_この記事では、Apple が iOS 10 に追加した RequestReview 方法と、Xamarin で実装する方法について説明します。_

IOS 10.3 を初めて使用する場合、メソッドを使用すると、 `RequestReview()` ios アプリでユーザーに評価または確認を求めることができます。 ユーザーが App Store からインストールした出荷アプリでこのメソッドが呼び出されると、iOS 10 は開発者の評価およびレビュープロセス全体を処理します。 このプロセスは App Store ポリシーによって管理されているため、アラートが表示されない場合があります。

![レビュー要求のサンプルアラート](request-app-review-images/review01.png)

## <a name="requesting-a-rating-or-review"></a>評価またはレビューの要求

`RequestReview()`クラスの静的メソッドは、 `SKStoreReviewController` ユーザーエクスペリエンスに意味がある任意の時点で呼び出すことができますが、レビュープロセスは App Store ポリシーによって制御され、処理されます。 結果として、このメソッドは警告を表示したり、表示したりすることはできません。また、ボタンをタップするなど、ユーザーの操作に応答して呼び出さないでください。

たとえば、アプリが特定の回数を起動した後にレビューを要求したり、プレーヤーがレベルを完了した後にレビューを要求したりすることがあります。

Xamarin iOS アプリの起動が完了したらすぐにレビューを要求するには、ファイルに次の変更を加え `AppDelegate.cs` ます。

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }

        ...

    }
}
```

> [!NOTE]
> `RequestReview()`開発中のアプリでを呼び出すと、テストできるように [評価とレビュー] ダイアログが常に表示されます。 これは、TestFlight によって配布されたアプリには適用されません。この場合、メソッドの呼び出しは無視されます。

ユーザーが `RequestReview()` App Store からインストールした出荷アプリでメソッドが呼び出されると、iOS 10 は開発者の評価およびレビュープロセス全体を処理します。 このプロセスは App Store ポリシーによって管理されるため、アラートは表示されない場合があります。

## <a name="linking-to-an-app-store-product-page"></a>アプリストア製品ページへのリンク 

開発者は、新しいメソッドに加えて、アプリ `RequestReview` 内からアプリストア内のアプリの製品ページへの深いリンクを提供することもできます。 `action=write-review`製品ページの URL の末尾にを追加すると、ユーザーがアプリのレビューを自動的に書き込むことができるページが開きます。 

## <a name="summary"></a>まとめ

この記事では、Apple が iOS 10 に追加した RequestReview 方法と、Xamarin で実装する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](/samples/xamarin/ios-samples/ios10-iostenthree/)