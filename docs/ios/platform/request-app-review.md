---
title: Xamarin.iOS でのアプリのレビューを要求します。
description: この記事では、iOS 10 に追加する Apple RequestReview メソッドを記述し、Xamarin.iOS での実装方法について説明します。
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: f72aaa781b0712e206cf02725cfc434594287f41
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365799"
---
# <a name="request-app-review-in-xamarinios"></a>Xamarin.iOS でのアプリのレビューを要求します。

_この記事では iOS 10 と Xamarin.iOS での実装方法に追加する Apple RequestReview メソッドについて説明します。_

Ios 10.3、新しい、`RequestReview()`メソッド iOS アプリに許可することを確認したり評価ユーザーに確認します。 配布アプリケーションでは、ユーザーがアプリ ストアからインストールされているこのメソッドが呼び出されると、iOS 10 が全体の評価を処理し、開発者のプロセスを確認してください。 このプロセスは、アプリ ストアのポリシーによって管理されますが、ため、アラートは可能性があります。 または、表示されない場合があります。

![](request-app-review-images/review01.png "レビューの要求アラートの例")

## <a name="requesting-a-rating-or-review"></a>評価またはレビューを要求します。

中に、`RequestReview()`の静的メソッド、`SKStoreReviewController`クラスを呼び出すことができます、いつでも、ユーザー エクスペリエンスの理にかなってレビュー プロセスの管理し、アプリ ストアのポリシーによって処理されます。 結果として、このメソッド可能性がありますまたはアラートが表示されない場合があり、ボタンをタップするなどのユーザー アクションへの応答では呼び出さないでいます。

たとえば、アプリをレビューした後は、指定された回数を起動するか、ゲーム プレーヤーには、レベルが完了したらレビューの依頼可能性があります要求可能性があります。

要求にレビュー Xamarin.iOS アプリでは、起動が完了するとすぐに、次に変更を加える、`AppDelegate.cs`ファイル。

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
> 呼び出す`RequestReview()`過小の開発アプリは常に、評価の表示をテストするためのダイアログを確認します。 これは、配布されていないメソッドの呼び出しは無視されます、TestFlight でのアプリには適用されません。

ときに、`RequestReview()`配布アプリケーションでは、ユーザーがアプリ ストアからインストールされているメソッドは、iOS 10 は開発者向け評価とレビュー プロセス全体を処理します。 ここでも、ため、このプロセスは、アプリ ストアのポリシーによって管理されますが、アラートが可能性があります。 または表示されない場合があります。

## <a name="linking-to-an-app-store-product-page"></a>App Store の製品ページへのリンク 

新しいに加えて`RequestReview`メソッド、開発者はアプリ内からアプリ ストアでアプリの製品ページへのディープ リンクを提供してできますも。 追加することによって`action=write-review`製品ページの URL の最後に、ページが開きます場所、ユーザーことができます、アプリのレビューを自動的に作成します。 

## <a name="summary"></a>まとめ

この記事では、iOS 10 と Xamarin.iOS での実装方法に追加する Apple RequestReview メソッドをカバーされてです。



## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
