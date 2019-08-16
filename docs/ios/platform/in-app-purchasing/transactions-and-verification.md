---
title: Xamarin. iOS でのトランザクションと検証
description: このドキュメントでは、Xamarin iOS アプリでの過去の購入の復元を許可する方法について説明します。 また、購入およびサーバー配信製品をセキュリティで保護する方法についても説明します。
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 2a0d0e1ab7272094d55dff7fa083e61ee9c3286c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527593"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Xamarin. iOS でのトランザクションと検証

## <a name="restoring-past-transactions"></a>過去のトランザクションの復元

アプリケーションで復元可能な製品の種類がサポートされている場合は、ユーザーインターフェイスの要素を含めて、ユーザーがそれらの購入を復元できるようにする必要があります。
この機能により、顧客は、追加のデバイスに製品を追加したり、消去したり、アプリを削除して再インストールしたりした後に、同じデバイスに製品を復元することができます。 次の製品の種類は復元可能です。

- 非消費製品
- 自動更新サブスクリプション
- 無料のサブスクリプション

復元プロセスでは、デバイスで保持するレコードを更新して、製品を処理する必要があります。 お客様は、任意のデバイスでいつでも復元を選択できます。 復元プロセスでは、そのユーザーの以前のすべてのトランザクションが再送信されます。次に、アプリケーションコードは、その情報に対して実行するアクションを決定する必要があります (たとえば、デバイスに購入したレコードが既に存在するかどうかを確認し、存在しない場合は購入のレコードを作成して、ユーザーの製品を有効にします)。

### <a name="implementing-restore"></a>復元の実装

ユーザーインターフェイスの **[復元]** ボタンをクリックすると、で RestoreCompletedTransactions が`SKPaymentQueue`トリガーされ、次のメソッドが呼び出されます。

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit は、Apple のサーバーに復元要求を非同期的に送信します。   
   
`CustomPaymentObserver`はトランザクションオブザーバーとして登録されるため、Apple のサーバーが応答するときにメッセージを受信します。 応答には、このユーザーがこのアプリケーションに対して実行したすべてのトランザクション (すべてのデバイスで) が含まれます。 このコードは、各トランザクションをループ処理し、復元され`UpdatedTransactions`た状態を検出し、次のようにメソッドを呼び出して処理します。

```csharp
// called when the transaction status is updated
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
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

ユーザーに対して復元可能な製品がない`UpdatedTransactions`場合、は呼び出されません。   
   
このサンプルでは、特定のトランザクションを復元するための最も簡単なコードは、商品 ID にアクセスするために`OriginalTransaction`プロパティが使用される点を除いて、購入時と同じアクションを実行します。

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

より高度な実装では、 `transaction.OriginalTransaction`元の日付や受信確認番号などの他のプロパティを確認できます。 この情報は、一部の製品の種類 (サブスクリプションなど) に役立ちます。

#### <a name="restore-completion"></a>復元の完了

に`CustomPaymentObserver`は、次に示すように、復元処理が完了した (正常に完了したか失敗した) 場合に、storekit によって呼び出される2つの追加のメソッドがあります。

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

この例では、これらのメソッドは何も行いませんが、実際のアプリケーションでは、ユーザーやその他の機能に対してメッセージを実装することができます。

## <a name="securing-purchases"></a>購入のセキュリティ保護

このドキュメントの2つの例`NSUserDefaults`では、を使用して購入を追跡します。   
   
 **消耗品**–クレジット購入の "残高" は、購入`NSUserDefaults`ごとにインクリメントされる単純な整数値です。   
   
 **非消耗品**–各写真フィルター購入は、の`NSUserDefaults`キーと値のペアとして格納されます。

を`NSUserDefaults`使用すると、コード例は単純に保持されますが、技術的には志向ユーザーが設定を更新できる可能性があるため、非常に安全なソリューションは提供されません (支払いメカニズムをバイパスする)。   
   
メモ:実際のアプリケーションでは、ユーザーの改ざんの対象とならない購入済みコンテンツを格納するための安全なメカニズムを採用する必要があります。 暗号化やその他の手法 (リモートサーバー認証など) が含まれる場合があります。   
   
 このメカニズムは、iOS、iTunes、iCloud の組み込みのバックアップと回復機能を利用するように設計する必要もあります。 これにより、ユーザーがバックアップを復元した後、前回の購入がすぐに使用できるようになります。   
   
IOS 固有のガイドラインの詳細については、Apple の安全なコーディングガイドを参照してください。

## <a name="receipt-verification-and-server-delivered-products"></a>確認メッセージとサーバー配信製品

ここまでの例では、アプリストアサーバーと直接通信するアプリケーションだけで、アプリにコード化された機能や機能のロックを解除するアプリケーションのみを購入しました。   
   
Apple では、購入確認を別のサーバーが個別に確認できるようにすることで、追加のレベルの購入セキュリティを提供しています。これは、デジタルブックの一部としてデジタルコンテンツを配信する前に、要求を検証するのに便利です。マガジン)。   
   
 **組み込み製品**: このドキュメントの例と同様に、購入した製品は、アプリケーションに付属する機能として存在します。 アプリ内購入では、ユーザーは機能にアクセスできます。
製品 Id はハードコードされています。   
   
 **サーバーによって提供**される製品: 製品は、正常なトランザクションによってコンテンツがダウンロードされるまで、リモートサーバーに格納されるダウンロード可能なコンテンツで構成されます。
例として、書籍や雑誌の問題があります。 製品 Id は、通常、外部のサーバー (製品のコンテンツもホストされている) から供給されます。 アプリケーションは、トランザクションが完了したことを記録するための堅牢な方法を実装する必要があります。これにより、コンテンツのダウンロードが失敗した場合に、ユーザーを混乱させることなく再実行できます。

### <a name="server-delivered-products"></a>サーバーによって提供される製品

製品のコンテンツ (書籍、雑誌 (またはゲームレベル) など) は、購入プロセス中にリモートサーバーからダウンロードする必要があります。 つまり、購入後に製品コンテンツを格納して配信するには、追加のサーバーが必要です。

#### <a name="getting-prices-for-server-delivered-products"></a>サーバーで提供される製品の価格を取得する

製品がリモートで配信されるため、書籍の追加や雑誌の新しい問題など、時間の経過と共に製品を追加することもできます (アプリコードを更新する必要はありません)。 アプリケーションがこれらのニュース製品を検出してユーザーに表示できるように、追加のサーバーはこの情報を保存して提供する必要があります。   
   
[![](transactions-and-verification-images/image38.png "サーバーで提供される製品の価格を取得する")](transactions-and-verification-images/image38.png#lightbox)   
   
1. 製品情報は、サーバーと iTunes Connect の複数の場所に保存する必要があります。 さらに、各製品にはコンテンツファイルが関連付けられています。 これらのファイルは、正常に購入した後に配信されます。   
   
2. ユーザーが製品を購入する場合、アプリケーションは使用可能な製品を特定する必要があります。 この情報はキャッシュされる場合がありますが、製品のマスターリストが保存されているリモートサーバーから配信する必要があります。   
   
3. サーバーは、解析するアプリケーションの製品 Id の一覧を返します。   
   
4. 次に、アプリケーションは、どの製品 Id を StoreKit に送信して価格と説明を取得するかを決定します。   
   
5. StoreKit は、製品 Id の一覧を Apple のサーバーに送信します。   
   
6. ITunes サーバーは、有効な製品情報 (説明と現在の価格) で応答します。   
   
7. アプリケーションの`SKProductsRequestDelegate`には、表示する製品情報がユーザーに渡されます。

#### <a name="purchasing-server-delivered-products"></a>サーバーで提供される製品の購入

リモートサーバーでは、コンテンツ要求が有効であることを検証する何らかの方法が必要であるため (つまり、に対して支払いが行われています)、受信情報が認証のために渡されます。 リモートサーバーは、検証のためにそのデータを iTunes に転送します。成功した場合、アプリケーションへの応答に製品のコンテンツが含まれます。   
   
 [![](transactions-and-verification-images/image39.png "サーバーで提供される製品の購入")](transactions-and-verification-images/image39.png#lightbox)   
   
1. アプリによって`SKPayment`がキューに追加されます。 必要に応じて、ユーザーは Apple ID の入力を求められ、支払いの確認を求められます。   
   
2. StoreKit は、要求をサーバーに送信して処理します。   
   
3. トランザクションが完了すると、サーバーはトランザクションの確認で応答します。   
   
4. サブ`SKPaymentTransactionObserver`クラスは、受信確認を受信して処理します。 製品はサーバーからダウンロードする必要があるため、アプリケーションはリモートサーバーへのネットワーク要求を開始します。   
   
5. ダウンロード要求には、受信データが付属しているので、リモートサーバーはコンテンツにアクセスする権限があることを確認できます。 アプリケーションのネットワーククライアントは、この要求に対する応答を待機します。   
   
6. サーバーは、コンテンツの要求を受信すると、受信データを解析し、iTunes サーバーに直接要求を送信して、有効なトランザクションがあるかどうかを確認します。 サーバーは、要求を実稼働 URL またはサンドボックスの URL に送信するかどうかを判断するために、何らかのロジックを使用する必要があります。 Apple では、常に運用 URL を使用し、受信状態が 21007 (サンドボックスの受信確認が運用サーバーに送信される) の場合にサンドボックスに切り替えることを提案しています。 詳細については、「Apple の[受信確認のプログラミングガイド」](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)を参照してください。
   
7. iTunes は、確認メッセージを確認し、有効な場合は0の状態を返します。   
   
8. サーバーは、iTunes の応答を待機します。 有効な応答を受け取った場合、コードは、アプリケーションへの応答に含める、関連付けられている製品のコンテンツファイルを見つける必要があります。   
  
9. アプリケーションは、応答を受信して解析し、製品の内容をデバイスのファイルシステムに保存します。   
   
10. アプリケーションによって製品が有効になり、StoreKit `FinishTransaction`のが呼び出されます。 アプリケーションでは、購入したコンテンツを必要に応じて表示することができます (たとえば、購入した本や雑誌の問題の最初のページを表示します)。

非常に大規模な製品コンテンツファイルの別の実装では、トランザクションを迅速に完了でき、実際の製品コンテンツをダウンロードするためのユーザーインターフェイスをユーザーに提供できるように、単に手順 #9 にトランザクション受信を格納するだけで済みます。後で実行します。 以降のダウンロード要求では、保存されている確認メッセージを再送信して、必要な製品コンテンツファイルにアクセスできます。

### <a name="writing-server-side-receipt-verification-code"></a>サーバー側の受信確認コードを書き込んでいます

サーバー側コードでの受信確認は、ワークフロー図の #8 を通じて #5 手順を含む単純な HTTP POST 要求/応答を使用して行うことができます。   
   
アプリの`SKPaymentTansaction.TransactionReceipt`プロパティを抽出します。 これは、検証のために iTunes に送信する必要があるデータです (手順 #5)。

トランザクション受信データを Base64 でエンコードします (手順 #5 または #6)。

次のような単純な JSON ペイロードを作成します。

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP は、運用また[https://buy.itunes.apple.com/verifyReceipt](https://buy.itunes.apple.com/verifyReceipt)は[https://sandbox.itunes.apple.com/verifyReceipt](https://sandbox.itunes.apple.com/verifyReceipt)テストのために JSON をにポストします。   
   
 JSON 応答には、次のキーが含まれます。

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

状態が0の場合は、有効な受信確認を示します。 サーバーは、購入した製品のコンテンツを満たすことができます。 受信キーには、アプリによって受信された`SKPaymentTransaction`オブジェクトと同じプロパティを持つ JSON ディクショナリが含まれているので、サーバーコードはこのディクショナリにクエリを実行して、購入の product_id や数量などの情報を取得できます。

詳細については、Apple の受信確認の[プログラミングガイド](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html)に関するドキュメントを参照してください。
