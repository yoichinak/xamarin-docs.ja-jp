---
title: Xamarin.iOS でアプリのレビューを要求します。
description: この記事では、RequestReview 方法を説明している Apple iOS 10 に追加し、Xamarin.iOS に実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 2b329ebad5faaa635d9a791f8760bd5f521de591
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788158"
---
# <a name="request-app-review-in-xamarinios"></a>Xamarin.iOS でアプリのレビューを要求します。

_ここでは、Xamarin.iOS に実装する方法、iOS 10 に追加する Apple RequestReview メソッドについて説明します。_

IOS 10.3 に新しい、`RequestReview()`メソッドにより、iOS アプリに転送率のまたは確認をユーザーに確認してください。 ユーザーがアプリ ストアからインストールする配布のアプリでこのメソッドが呼び出されると、iOS 10 が全体の評価を処理し、開発者のプロセスを確認します。 このプロセスは、アプリ ストアのポリシーによって制御されます、ので、アラートは可能性があります。 または表示されない場合があります。

![](request-app-review-images/review01.png "サンプルのレビューの要求アラート")

## <a name="requesting-a-rating-or-review"></a>評価または確認を要求します。

中に、`RequestReview()`の静的メソッド、`SKStoreReviewController`クラスを呼び出すことができます、いつでもユーザー エクスペリエンスの方が効率的なレビュー プロセスの管理し、App Store のポリシーによって処理されます。 その結果、このメソッド可能性がありますまたは警告が表示されない場合があり、ボタンをタップするなどのユーザー アクションへの応答で呼び出さないでです。

たとえば、アプリは、確認された後に、指定された回数を起動するか、ゲームがレビューを要求する、プレーヤーが、レベルを完了後要求可能性があります。

要求にレビュー Xamarin.iOS アプリでは、起動、終了するとすぐに次の変更を`AppDelegate.cs`ファイル。

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
> 呼び出す`RequestReview()`で過少開発アプリは常に評価の表示を確認してダイアログのため、テストすることができます。 これは、メソッドの呼び出しは無視されます TestFlight を通じて配布されたアプリには適用されません。

ときに、`RequestReview()`配布アプリケーションでは、ユーザーがアプリ ストアからインストールされているメソッドは、iOS 10 は、開発者の評価とレビュー プロセス全体を処理します。 もう一度、ため、このプロセスは、アプリ ストアのポリシーによって制御されます、アラートが可能性があります。 または表示されない場合があります。

## <a name="linking-to-an-app-store-product-page"></a>App Store 製品ページへのリンク 

新しいに加えて`RequestReview`メソッド、開発者は、アプリ内から、アプリ ストアでアプリの製品ページへのディープ リンクを提供することができますも。 追加することによって`action=write-review`に製品ページの URL の末尾にページが開かれる、ユーザーことができます、アプリの確認を自動的に書き込みます。 

## <a name="summary"></a>まとめ

この記事の内容について説明しました RequestReview メソッドその Apple iOS 10 と Xamarin.iOS に実装する方法に追加します。



## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
