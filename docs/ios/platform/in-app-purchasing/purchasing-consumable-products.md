---
title: Xamarin での消費製品の購入
description: このドキュメントでは、Xamarin. iOS の利用できる製品について説明します。 使用できる製品は、ゲーム内の通貨などの単一の用途の機能です。
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d6cc17d4a3a77487689357641999525a539b442f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435529"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Xamarin での消費製品の購入

"復元" の要件はないため、利用できる製品は最も簡単に実装できます。 これらは、ゲーム内の通貨や単一の用途の機能などの製品に役立ちます。 ユーザーは、使用可能な製品をさらに再購入できます。

## <a name="built-in-product-delivery"></a>組み込みの製品配信

このドキュメントに付属するサンプルコードは、組み込み製品を示しています。製品 Id は、支払い後に機能を "ロック解除" するコードに密に結合されているため、アプリケーションにハードコーディングされています。 購入プロセスは次のように視覚化できます。   
   
[![購入プロセスの視覚化](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 基本的なワークフローは次のとおりです。   
   
 1. アプリによってがキューに追加され `SKPayment` ます。 必要に応じて、ユーザーは Apple ID の入力を求められ、支払いの確認を求められます。   
   
 2. StoreKit は、要求をサーバーに送信して処理します。   
   
 3. トランザクションが完了すると、サーバーはトランザクションの確認で応答します。   
   
 4. サブクラスは、受信 `SKPaymentTransactionObserver` 確認を受信して処理します。   
   
 5. アプリケーションによって (またはその他のメカニズムを更新して) 製品が有効になり、 `NSUserDefaults` StoreKit のが呼び出さ `FinishTransaction` れます。

別の種類のワークフローとして、 *サーバーによって提供* される製品があります。これについては、ドキュメントで後ほど説明します (「確認」および「 *サーバー配信製品*」を参照してください)。

## <a name="consumable-products-example"></a>使用できる製品の例

[InAppPurchaseSample コード](/samples/xamarin/ios-samples/storekit)には、基本的な ' ゲーム内通貨 ' ("サルクレジット" と呼ばれます) を実装する*消耗品*と呼ばれるプロジェクトが含まれています。 このサンプルでは、2つのアプリ内購入製品を実装し、ユーザーが希望どおりに "サルのクレジット" を購入できるようにする方法を示します。実際のアプリケーションでは、これを使用する方法もあります。   

アプリケーションは次のスクリーンショットに示されています。購入するたびに、ユーザーの残高に "サルクレジット" が追加されます。   

 [![購入ごとに、ユーザーの残高に対してより多くのサルクレジットが追加されます](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   

カスタムクラス、StoreKit、アプリストア間の相互作用は次のようになります。   

 [![カスタムクラス、StoreKit、およびアプリストア間の相互作用](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

### <a name="viewcontroller-methods"></a>ViewController メソッド

ビューコントローラーでは、製品情報を取得するために必要なプロパティとメソッドに加えて、購入関連の通知をリッスンするために追加の通知オブザーバーが必要になります。 これらは `NSObjects` 、それぞれで登録と削除が行われるだけです `ViewWillAppear` `ViewWillDisappear` 。

```csharp
NSObject succeededObserver, failedObserver;
```

また、コンストラクターは `SKProductsRequestDelegate` サブクラス () を作成 `InAppPurchaseManager` し、() を作成して登録し `SKPaymentTransactionObserver` `CustomPaymentObserver` ます。   

アプリ内購入トランザクションの処理の最初の部分は、次のサンプルアプリケーションのコードに示すように、ユーザーが何かを購入するときにボタンの押下を処理することです。

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

ユーザーインターフェイスの2番目の部分では、表示されたバランスを更新することで、トランザクションが成功したという通知を処理します。

```csharp
succeededObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

なんらかの理由でトランザクションがキャンセルされた場合、ユーザーインターフェイスの最後の部分でメッセージが表示されます。 コード例では、メッセージは単に出力ウィンドウに書き込まれます。

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

ビューコントローラーでこれらのメソッドに加えて、消費製品の購入トランザクションでも、とのコードが必要になり `SKProductsRequestDelegate` `SKPaymentTransactionObserver` ます。

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager メソッド

このサンプルコードでは、 `PurchaseProduct` インスタンスを作成 `SKPayment` し、それを処理のためにキューに追加するメソッドなど、InAppPurchaseManager クラスでいくつかの購入関連のメソッドを実装しています。

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

キューへの支払いの追加は、非同期操作です。 StoreKit によってトランザクションが処理され、Apple のサーバーに送信される間、アプリケーションは制御を取り戻すことができます。 この時点で、iOS はユーザーがアプリストアにログインしていることを確認し、必要に応じて Apple ID とパスワードの入力を求めます。   

ユーザーが App Store での認証に成功し、トランザクションに同意した場合、は `SKPaymentTransactionObserver` StoreKit の応答を受け取り、次のメソッドを呼び出してトランザクションを実行して完了します。

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

最後の手順では、次のようにを呼び出して、トランザクションが正常に完了したことを StoreKit に通知し `FinishTransaction` ます。

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

製品が配信されたら、を `SKPaymentQueue.DefaultQueue.FinishTransaction` 呼び出して、支払キューからトランザクションを削除する必要があります。

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver) メソッド

StoreKit は、 `UpdatedTransactions` Apple のサーバーから応答を受信したときにメソッドを呼び出し、 `SKPaymentTransaction` コードで検査するオブジェクトの配列を渡します。 メソッドは、各トランザクションをループし、次に示すように、トランザクションの状態に基づいて異なる関数を実行します。

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

`CompleteTransaction`このセクションの前半で説明したメソッドは、購入の詳細をに保存し `NSUserDefaults` 、storekit を使用してトランザクションを終了し、最後に UI に更新を通知します。

### <a name="purchasing-multiple-products"></a>複数の製品を購入する

アプリケーションで複数の製品を購入するのが理にかなっている場合は、クラスを使用 `SKMutablePayment` して Quantity フィールドを設定します。

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

完成したトランザクションを処理するコードでは、次のように Quantity プロパティを照会して、適切に購入する必要もあります。

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

ユーザーが複数の数量を購入すると、StoreKit 確認アラートには、次のスクリーンショットに示すように、請求される数量、単価、および料金の合計が反映されます。

[![購入の確認](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>ネットワーク障害の処理

アプリ内購入では、StoreKit が Apple のサーバーと通信するために、正常なネットワーク接続が必要です。 ネットワーク接続が利用できない場合は、アプリ内購入は使用できません。

### <a name="product-requests"></a>製品要求

を作成しているときにネットワークが使用できない場合は、 `SKProductRequest` `RequestFailed` `SKProductsRequestDelegate` 次に示すように、サブクラス () のメソッド `InAppPurchaseManager` が呼び出されます。

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

次に、ViewController が通知をリッスンし、[購入] ボタンにメッセージを表示します。

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

ネットワーク接続はモバイルデバイスでは一時的なものであるため、アプリケーションは SystemConfiguration フレームワークを使用してネットワークの状態を監視し、ネットワーク接続が利用可能になったときに再試行することができます。 Apple のまたはそれを使用するを参照してください。

### <a name="purchase-transactions"></a>トランザクションの購入

StoreKit の支払キューは、可能な場合は購入要求を格納して転送します。そのため、ネットワークの停止の影響は、購入プロセス中にネットワークが失敗した場合によって異なります。   

トランザクションの実行中にエラーが発生した場合、 `SKPaymentTransactionObserver` サブクラス () によって `CustomPaymentObserver` メソッドが呼び出され、 `UpdatedTransactions` クラスは "失敗" の `SKPaymentTransaction` 状態になります。

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

メソッドは、 `FailedTransaction` 次に示すように、ユーザーによるキャンセルによってエラーが発生したかどうかを検出します。

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

トランザクションが失敗した場合でも、 `FinishTransaction` 支払キューからトランザクションを削除するには、メソッドを呼び出す必要があります。

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

このコード例では、ViewController がメッセージを表示できるように通知を送信します。 ユーザーがトランザクションをキャンセルした場合、アプリケーションは追加のメッセージを表示しません。 次のようなエラーコードが発生する可能性があります。

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>処理の制限

IOS の **設定 > 全般 > 制限** 機能を使用すると、ユーザーは自分のデバイスの特定の機能をロックできます。   

ユーザーがメソッドを使用してアプリ内購入を行うことを許可されているかどうかをクエリでき `SKPaymentQueue.CanMakePayments` ます。 False が返された場合、ユーザーはアプリ内購入にアクセスできません。 購入が試行された場合、StoreKit はユーザーにエラーメッセージを自動的に表示します。 この値を確認することで、アプリケーションで購入ボタンを非表示にしたり、他の操作を行ってユーザーを支援したりすることができます。   

このファイルでは、 `InAppPurchaseManager.cs` メソッドは次の `CanMakePayments` ように storekit 関数をラップします。

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

このメソッドをテストするには、iOS の **制限** 機能を使用して、 **アプリ内購入**を無効にします。   

 [![IOS の制限機能を使用してアプリ内購入を無効にする](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   

このコード例で `ConsumableViewController` `CanMakePayments` は、無効になっているボタンに **Appstore の無効** なテキストを表示することによって、false が返されるようになりました。

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

**アプリ内購入**機能が制限されている場合、アプリケーションは次のようになります。 [購入] ボタンは無効になっています。   

 [![アプリ内購入機能が制限されている場合、アプリケーションは次のようになります。購入ボタンは無効になります。](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   

が false の場合でも製品情報を要求でき `CanMakePayments` ます。そのため、アプリでは引き続き価格を取得して表示できます。 つまり、コードからチェックを削除した場合で `CanMakePayments` も、購入ボタンは引き続きアクティブですが、購入時には、 **アプリ内購入は許可** されていないというメッセージがユーザーに表示されます (これは、支払いキューにアクセスしたときに storekit によって生成されます)。   

 [![アプリ内購入は使用できません](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   

実際のアプリケーションでは、ボタンを完全に非表示にしたり、StoreKit によって自動的に表示されるアラートよりも詳細なメッセージを提供するなど、さまざまな方法で制限を処理することがあります。