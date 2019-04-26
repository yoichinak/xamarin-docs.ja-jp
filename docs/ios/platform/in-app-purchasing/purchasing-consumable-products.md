---
title: Xamarin.iOS でコンシューマブル製品の購入
description: このドキュメントでは、Xamarin.iOS でコンシューマブル製品について説明します。 コンシューマブル製品は、使い捨てなゲーム内の通貨などの機能です。
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: b55465a700974e0ce5ceb8893d96311d920e04ae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61366718"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Xamarin.iOS でコンシューマブル製品の購入

コンシューマブル製品は 'restore' 要件がないため、実装する最も簡単です。 これらは、ゲーム内の通貨や、使い捨ての機能などの製品に役立ちます。 ユーザーがもう一度コンシューマブル製品 over-over 購入再ことができます。

## <a name="built-in-product-delivery"></a>組み込みの製品の出荷

このドキュメントに付属するサンプル コードは、組み込みの製品を示します: 'ロックを解除する ' 機能は、支払い後のコードに密結合されているために、製品 Id は、アプリケーションにハードコーディングしています。 購入プロセスを視覚化するには、次のようにします。   
   
[![購入プロセスの視覚化](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 基本的なワークフローは次のとおりです。   
   
 1. アプリを追加、`SKPayment`をキューにします。 ユーザーが自分の Apple ID の入力を求めし、支払いの確認を求めに必要な場合。   
   
 2. StoreKit では、サーバー処理のために、要求を送信します。   
   
 3. トランザクションが完了したら、サーバーは、トランザクション受信で応答します。   
   
 4. `SKPaymentTransactionObserver`サブクラスですが、受信確認を受信し、それを処理します。   
   
 5. アプリケーションにより、製品 (更新することで`NSUserDefaults`またはその他のメカニズム)、号餧ェヒェマル StoreKit の`FinishTransaction`します。

– ワークフローの別の型がある*Server-Delivered 製品*– つまり、ドキュメントの後半で説明されている (を参照してください*受信確認と Server-Delivered 製品*)。

## <a name="consumable-products-example"></a>使用できる製品の例

[InAppPurchaseSample コード](https://developer.xamarin.com/samples/monotouch/StoreKit/)という名前のプロジェクトを含む*消耗品*basic 'ゲームに通貨' (「クレジット モンキー」と呼ばれます) を実装します。 サンプルでは、多くの「monkey クレジット」– 必要実際のアプリケーションにもそれらの支出の何らかの方法として購入するユーザーを許可する 2 つのアプリ内購入製品を実装する方法を示します。   
   
   
   
 アプリケーションがこれらのスクリーン ショットに示すように – のそれぞれの購入は、ユーザーの残高をさらに「monkey クレジット」を追加します。   
   
   
   
 [![それぞれの購入ユーザー残高にさらに monkey クレジットを追加します](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 StoreKit とアプリ ストアのカスタムのクラス間の相互作用は、次のようになります。   
   
   
   
 [![StoreKit とアプリ ストアのカスタムのクラス間の相互作用](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController メソッド

だけでなく、プロパティと製品情報を取得するために必要なメソッドは、ビュー コント ローラーには、購入に関連する通知をリッスンするオブザーバーを追加の通知が必要です。 これらはだけ`NSObjects`登録されているし、で削除する`ViewWillAppear`と`ViewWillDisappear`それぞれします。

```csharp
NSObject succeededObserver, failedObserver;
```

また、コンス トラクターを作成、`SKProductsRequestDelegate`サブクラス ( `InAppPurchaseManager`) をさらに作成して登録します、 `SKPaymentTransactionObserver` ( `CustomPaymentObserver`)。   
   
   
   
 アプリ内購入、トランザクションの処理の最初の部分は、サンプル アプリケーションから次のコードに示すように、何か購入するユーザーが希望する場合、ボタン押下を処理するためには。

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 ユーザー インターフェイスの 2 番目の部分は、通知の処理、トランザクションが成功したこと、ここで表示される残高を更新することで。

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

何らかの理由により、トランザクションが取り消された場合、ユーザー インターフェイスの最後の部分は、メッセージに表示されています。 コード例は、出力ウィンドウに単純にメッセージが書き込まれます。

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

ビュー コント ローラーでこれらのメソッドだけでなく消耗品の購入取引もコードが必要で、 `SKProductsRequestDelegate` 、`SKPaymentTransactionObserver`します。

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager メソッド

いくつかのサンプル コードの実装は購入 InAppPurchaseManager クラスに関連するメソッドを含む、`PurchaseProduct`を作成するメソッド、`SKPayment`インスタンスし、処理のキューに追加します。

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

非同期操作には、支払いをキューに追加します。 アプリケーションは、コントロールを再度獲得 storekit やトランザクションを処理し、Apple のサーバーに送信します。 この時点では、その iOS は、ユーザーが App Store にログインし、必要な場合に、Apple ID とパスワードのプロンプト彼女を確認します。   
   
   
   
 App Store で認証し、トランザクションにすることに同意が正常にユーザーと仮定した場合、 `SKPaymentTransactionObserver` StoreKit の応答を受信して、トランザクションを実行し、完成させるのには、次のメソッドを呼び出します。

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

最後の手順が呼び出すことによって、トランザクションが正常に満たされたことを StoreKit に通知することを確認するには`FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

製品を配信すると`SKPaymentQueue.DefaultQueue.FinishTransaction`支払キューからトランザクションを削除するのには呼び出す必要があります。

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver) メソッド

StoreKit の呼び出し、`UpdatedTransactions`メソッドの配列を渡し、Apple のサーバーから応答を受信する場合`SKPaymentTransaction`コードを検査するオブジェクト。 メソッドは、各トランザクションをループ処理し、(ここで示すように) とトランザクションの状態に基づいて別の関数を実行します。

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`CompleteTransaction`メソッドは、このセクションで先ほど説明した – 購入の詳細を保存します`NSUserDefaults`StoreKit を使用してトランザクションを終了し、最後に更新する UI に通知します。

### <a name="purchasing-multiple-products"></a>複数の製品を購入します。

アプリケーション内に複数の製品を購入する意味を提示した場合に使用して、`SKMutablePayment`クラスし、Quantity フィールドを設定します。

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

完了したトランザクションを処理するコードは、購入を正しく満たすために数量プロパティをクエリもする必要があります。

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

ユーザーは、複数の数量を購入、storekit や確認のアラートが反映されます、数量、ユニットの料金および課金されますが、合計価格の次のスクリーン ショット。

[![購入の確認](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>ネットワーク障害の処理

アプリ内購入には、Apple のサーバーと通信する StoreKit の作業のネットワーク接続が必要です。 ネットワーク接続が利用できない場合、アプリ内購入は使用できません。

### <a name="product-requests"></a>製品要求

ネットワークが作成中に使用可能でない場合、 `SKProductRequest`、`RequestFailed`のメソッド、`SKProductsRequestDelegate`サブクラスです ( `InAppPurchaseManager`) 次に示すというされます。

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

ViewController は次の通知をリッスンし、購入ボタンにメッセージを表示します。

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

モバイル デバイスで一時的なネットワーク接続を使用できる、ので、アプリケーションは SystemConfiguration framework を使用してネットワークの状態を監視して、ネットワーク接続があるときにもう一度お試し可能性があります。 それを使用する、または Apple の参照します。

### <a name="purchase-transactions"></a>購入のトランザクション

Storekit や支払いのキューを格納し、購入手続きの際に、ネットワークが失敗したときに、ネットワークの停止の影響をに応じて異なりますので、前方の購入が可能であればに要求します。   
   
   
   
 トランザクション中にエラーが発生した場合、`SKPaymentTransactionObserver`サブクラスです ( `CustomPaymentObserver`) 必要があります、`UpdatedTransactions`メソッドと呼ばれる、`SKPaymentTransaction`クラスは、失敗状態になります。

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction`メソッドは次のように、エラーがユーザーによるキャンセルの場合、原因かどうかを検出します。

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

トランザクションが失敗した場合でも、`FinishTransaction`支払いキューからトランザクションを削除するメソッドを呼び出す必要があります。

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

コード例は、ViewController がメッセージを表示できるようにし、通知を送信します。 アプリケーションは、追加表示されませんが、ユーザーには、トランザクションが取り消された場合のメッセージします。 発生する可能性があるその他のエラー コードは次のとおりです。

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>処理の制約

**設定 > [全般] > 制限**iOS の機能により、ユーザーが自分のデバイスの特定の機能をロックします。   
   
   
   
 使用してアプリ内購入のために、ユーザーが許可されているかどうかを照会することができます、`SKPaymentQueue.CanMakePayments`メソッド。 これは、false を返す場合、ユーザーは、アプリ内購入でアクセスできません。 購入しようとすると StoreKit はユーザーに自動的にエラー メッセージを表示します。 この値をチェックして、アプリケーションが代わりに購入ボタンを非表示にするか、ユーザーを支援するその他の操作できます。   
   
   
   
 `InAppPurchaseManager.cs`ファイル、`CanMakePayments`メソッドは、このような storekit や関数をラップします。

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

このメソッドをテストするには、使用、**制限**機能を無効にする iOS の**アプリ内購入**:   
   
   
   
 [![IOS の制限機能を使用して、アプリ内購入を無効にするには](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 次のコード例`ConsumableViewController`に反応`CanMakePayments`false を返すことを表示して**AppStore で無効になっている**無効なボタンのテキスト。

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

アプリケーションの場合に、このよう、**アプリ内購入**機能は制限付き – 購入ボタンが無効になっています。   
   
   
   
 [![機能は、アプリ内購入には、ボタンが無効になっている購入が制限されていると、アプリケーションがこのような検索します。](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

製品情報ができます。 要求されたときに`CanMakePayments`、アプリが取得でも、および価格を表示できるように値が false の場合。 つまり、削除しました、`CanMakePayments`チェックも購入ボタン コードからアクティブである購入しようとすると、ユーザーにメッセージが表示される**アプリ内購入が許可されていません**(StoreKit によって生成されました。支払キューがアクセスされるとき)。   
   
   
   
 [![アプリ内購入が許可されていません](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 実際のアプリケーションでは、完全にボタンを非表示や、おそらく StoreKit が自動的に表示されるアラートよりも詳細なメッセージを提供するなど、制限を処理する別のアプローチをかかる場合があります。

