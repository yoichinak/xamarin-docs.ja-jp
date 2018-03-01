---
title: "利用できる製品の購入"
ms.topic: article
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7366a4ce5cbb6a3026a7445a03f03b45d89d9210
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="purchasing-consumable-products"></a>利用できる製品の購入

最も単純な 'restore' 要件が存在しないため、実装の消耗製品である場合です。 これらは、ゲーム内の通貨や機能の 1 つを使用して一部のような製品に役立ちます。 ユーザーが消耗製品 over-over をもう一度購入再ことができます。

## <a name="built-in-product-delivery"></a>組み込みの製品の配信

このドキュメントに付属するサンプル コードは、組み込みの製品を示します: 'ロックを解除する ' 機能は、支払いの後のコードに密に結合するため、製品 Id は、アプリケーションにハードコーディングします。 購入プロセスは、次のように視覚化できます。   
   
[ ![購買プロセスの視覚化](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png)     
   
 基本的なワークフローとは。   
   
   
   
 1. アプリを追加、`SKPayment`をキューにします。 ユーザーの Apple ID の入力を求めし、する、支払いの確認を求めに必要な場合です。   
   
 2. StoreKit は、サーバー上で処理する要求を送信します。   
   
 3. トランザクションが完了したら、サーバーはトランザクションの確認メッセージで応答します。   
   
 4. `SKPaymentTransactionObserver`サブクラスは、配信確認メッセージを受信し、それを処理します。   
   
 5. アプリケーションにより、製品 (更新することによって`NSUserDefaults`またはその他の機構)、し StoreKit を呼び出して`FinishTransaction`です。

– ワークフローの別の型がある*Server-Delivered 製品*– つまり、ドキュメントの後半で説明されている (を参照してください*受信確認と Server-Delivered 製品*)。

## <a name="consumable-products-example"></a>利用できる製品の例

[InAppPurchaseSample コード](https://developer.xamarin.com/samples/monotouch/StoreKit/)と呼ばれるプロジェクトを含む*消耗*basic 'ゲーム内 currency' (「クレジットをモンキー」と呼ばれます) を実装します。 サンプルは、多くの「サル クレジット」– 必要実際のアプリケーションではもあるので支出になんらかの方法を購入するユーザーを許可する 2 つのアプリ内購入製品を実装する方法を示します。   
   
   
   
 これらのスクリーン ショットでは、アプリケーションが示すように – 各注文書、ユーザーのバランスを複数の「サル クレジット」を追加します。   
   
   
   
 [ ![各注文書残数のユーザーに複数のサル クレジットを追加します。](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png)   
   
   
   
 StoreKit と、アプリ ストアのカスタムのクラス間の相互作用は、次のようになります。   
   
   
   
 [ ![StoreKit と、アプリ ストアのカスタムのクラス間の相互作用](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController メソッド

に加えてのプロパティおよび製品情報を取得するために必要なメソッドを使用して、ビュー コント ローラーには、発注に関連する通知をリッスンするオブザーバーをその他の通知が必要です。 これらはだけ`NSObjects`登録されているし、で削除する`ViewWillAppear`と`ViewWillDisappear`それぞれします。

```csharp
NSObject succeededObserver, failedObserver;
```

また、コンス トラクターを作成、`SKProductsRequestDelegate`サブクラス ( `InAppPurchaseManager`) さらに作成して登録を実行する、 `SKPaymentTransactionObserver` ( `CustomPaymentObserver`)。   
   
   
   
 アプリ内購入トランザクションの処理の最初の部分では、サンプル アプリケーションから次のコードに示すように、ユーザーが、何か購入するしようとするときに、ボタン押下を処理します。

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 ユーザー インターフェイスの 2 番目の部分は、通知を処理は、トランザクションが成功したこと、ここで表示される残高を更新することで。

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

ユーザー インターフェイスの最後の部分は、何らかの理由により、トランザクションが取り消された場合、メッセージを表示されています。 コード例では、出力ウィンドウにメッセージが単に書き込まれます。

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

ビュー コント ローラーでのこれらのメソッドだけでなく利用できる製品購入トランザクションもコードが必要です、`SKProductsRequestDelegate`と`SKPaymentTransactionObserver`です。

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager メソッド

いくつかのサンプル コードを実装購入 InAppPurchaseManager クラスに関連するメソッドを含む、`PurchaseProduct`を作成するメソッド、`SKPayment`インスタンスし、処理のキューに追加します。

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

非同期操作は、支払いをキューに追加します。 アプリケーションでは、StoreKit がトランザクションを処理し、Apple のサーバーに送信中に、コントロールが再度獲得します。 この時点では、ユーザーがアプリ ストアへログに記録されの入力を求める彼女に Apple ID とパスワードが必要な場合、その iOS が確認されます。   
   
   
   
 アプリ ストアとを認証し、トランザクションに同意するユーザーを正常と仮定した場合、`SKPaymentTransactionObserver`を StoreKit の応答を受信し、トランザクションを実行し、完成させるのには、次のメソッドを呼び出します。

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

最後に、ことを通知して StoreKit を呼び出して、トランザクションが正常に満たされたことを確認してください`FinishTransaction`:。

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

製品を配信すると、`SKPaymentQueue.DefaultQueue.FinishTransaction`支払キューからトランザクションを削除するのに呼び出せる必要があります。

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver) メソッド

StoreKit 呼び出し、`UpdatedTransactions`メソッド Apple のサーバーから応答を受信しの配列を渡します`SKPaymentTransaction`コードの検査するオブジェクト。 メソッドは、各トランザクションをループし、(に示すように、トランザクションの状態に基づいて、別の関数を実行します。

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

`CompleteTransaction`メソッドは、このセクションの前半カバーされた – に購入の詳細を保存`NSUserDefaults`StoreKit を使用してトランザクションを終了し、最後に更新するための UI に通知します。

### <a name="purchasing-multiple-products"></a>複数の製品を購入

アプリケーション内に複数の製品を購入する理にかなって場合を使用して、`SKMutablePayment`クラスし、Quantity フィールドを設定します。

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

完了したトランザクションの処理コードを正しく購入を満たすために数量プロパティのクエリもする必要があります。

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

ユーザーが複数の数量を購入すると StoreKit 確認のアラートが反映されます数量、単価、および合計金額が課金されます、次のスクリーン ショットに示すように。

[ ![購入のことを確認します。](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png)

## <a name="handling-network-outages"></a>処理ネットワークの停止

アプリ内購入は、Apple のサーバーと通信する StoreKit のネットワークに接続が必要です。 ネットワーク接続が使用できない場合、アプリ内購入はできなくなります。

### <a name="product-requests"></a>製品の要求

ネットワークが同時に、その使用できない場合、 `SKProductRequest`、`RequestFailed`のメソッド、`SKProductsRequestDelegate`サブクラス ( `InAppPurchaseManager`) が呼び出されます、次のように。

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

ViewController に、通知をリッスンしのボタンの注文書メッセージが表示されます。

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

ネットワークに接続できますが、モバイル デバイスの一時的なのでアプリケーション可能性があります SystemConfiguration フレームワークを使用してネットワークの状態を監視するし、再試行して、ネットワーク接続が利用可能な場合です。 Apple を参照するか、それを使用します。

### <a name="purchase-transactions"></a>トランザクションを購入します。

StoreKit 支払キューを保存し、前方の注文書を要求可能であれば、ネットワークの停止の影響によって異なります、発注プロセス中にネットワークが失敗したときに、します。   
   
   
   
 トランザクション中にエラーが発生した場合、`SKPaymentTransactionObserver`サブクラス ( `CustomPaymentObserver`) が、`UpdatedTransactions`呼び出されるメソッドと`SKPaymentTransaction`クラスは、失敗状態になります。

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

`FailedTransaction`メソッドは次のように、エラーがユーザーのキャンセル、原因かどうかを検出します。

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

トランザクションが失敗した場合でも、`FinishTransaction`支払キューからトランザクションを削除するメソッドを呼び出す必要があります。

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

ViewController がメッセージを表示できるように、コード例は、通知を送信します。 アプリケーションを追加する必要があります表示しないメッセージの場合は、トランザクションが取り消されました。 発生する可能性があるその他のエラー コードは次のとおりです。

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>処理の制約

**設定 > [全般] > 制限**iOS の機能により、ユーザーが自分のデバイスの特定の機能をロックします。   
   
   
   
 ユーザーが経由でのアプリ内購入できるかどうかをクエリすることができます、`SKPaymentQueue.CanMakePayments`メソッドです。 これは、false を返す場合、ユーザーは、アプリ内購入でアクセスできません。 購入が試行されると StoreKit はユーザーに自動的にエラー メッセージを表示します。 この値をチェックして、アプリケーションが代わりに購入ボタンを非表示にするか、ユーザーを支援するその他の操作ことができます。   
   
   
   
 `InAppPurchaseManager.cs`ファイル、`CanMakePayments`メソッドは、次のように StoreKit 関数をラップします。

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

このメソッドをテストするには、使用、**制限**機能を無効にする iOS の**アプリ内購入**:   
   
   
   
 [ ![IOS の制限機能を使用して、アプリ内購入を無効にします。](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png)   
   
   
   
 次の例のコード`ConsumableViewController`に反応`CanMakePayments`を表示する場合は false を返す**AppStore で無効になっている**無効なボタンのテキスト。

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

アプリケーションの場合に、このよう、**アプリ内購入**機能は制限付き – 注文書ボタンが無効になっています。   
   
   
   
 [ ![機能は、アプリ内購入には、ボタンが無効になっている注文書が制限されているときに、アプリケーションが次のよう](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png)   
   
   
   

製品情報については、引き続き時に要求された`CanMakePayments`は false で、アプリの取得および価格を表示できるまだようにします。 つまり、削除したかどうか、`CanMakePayments`が、メッセージを表示しようとすると、注文書注文書ボタンもコードからのチェックは、アクティブにする**アプリ内購入が許可されていません**(StoreKit によって生成されます。支払キューがアクセスされるとき)。   
   
   
   
 [ ![アプリ内購入が許可されていません](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png)   
   
   
   
 実際のアプリケーションは、別のアプローチをボタンを完全に非表示にして、おそらく StoreKit を自動的に表示するアラートよりも詳細なメッセージを提供するなど、制限の処理にかかる場合があります。

