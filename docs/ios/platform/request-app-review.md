---
title: Xamarin のアプリレビューを要求する
description: この記事では、Apple が iOS 10 に追加した RequestReview メソッドについて説明し、Xamarin で実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 0522e6b1bcf3e0f927a403c12a9c27bb6b3bb3ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031595"
---
# <a name="request-app-review-in-xamarinios"></a>Xamarin のアプリレビューを要求する

_この記事では、Apple が iOS 10 に追加した RequestReview 方法と、Xamarin で実装する方法について説明します。_

IOS 10.3 の新機能である `RequestReview()` 方法では、iOS アプリはユーザーに対して評価またはレビューを要求できます。 ユーザーが App Store からインストールした出荷アプリでこのメソッドが呼び出されると、iOS 10 は開発者の評価およびレビュープロセス全体を処理します。 このプロセスは App Store ポリシーによって管理されているため、アラートが表示されない場合があります。

![](request-app-review-images/review01.png "A sample Review Request alert")

## <a name="requesting-a-rating-or-review"></a>評価またはレビューの要求

`SKStoreReviewController` クラスの `RequestReview()` 静的メソッドは、ユーザーエクスペリエンスに意味がある任意の時点で呼び出すことができますが、レビュープロセスは App Store ポリシーによって制御され、処理されます。 結果として、このメソッドは警告を表示したり、表示したりすることはできません。また、ボタンをタップするなど、ユーザーの操作に応答して呼び出さないでください。

たとえば、アプリが特定の回数を起動した後にレビューを要求したり、プレーヤーがレベルを完了した後にレビューを要求したりすることがあります。

Xamarin iOS アプリの起動が完了したらすぐにレビューを要求するには、`AppDelegate.cs` ファイルに次の変更を加えます。

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
> 開発中のアプリで `RequestReview()` を呼び出すと、テストできるように [評価とレビュー] ダイアログが常に表示されます。 これは、TestFlight によって配布されたアプリには適用されません。この場合、メソッドの呼び出しは無視されます。

ユーザーが App Store からインストールした出荷アプリで `RequestReview()` メソッドが呼び出されると、iOS 10 は、開発者の評価およびレビュープロセス全体を処理します。 このプロセスは App Store ポリシーによって管理されるため、アラートは表示されない場合があります。

## <a name="linking-to-an-app-store-product-page"></a>アプリストア製品ページへのリンク 

開発者は、新しい `RequestReview` メソッドに加えて、アプリ内からアプリストア内のアプリの製品ページへのディープリンクを提供することもできます。 製品ページの URL の末尾に `action=write-review` を追加すると、ユーザーがアプリのレビューを自動的に書き込むことができるページが開きます。 

## <a name="summary"></a>まとめ

この記事では、Apple が iOS 10 に追加した RequestReview 方法と、Xamarin で実装する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
