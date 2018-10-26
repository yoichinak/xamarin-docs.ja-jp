---
title: トランザクションと Xamarin.iOS での検証
description: このドキュメントでは、Xamarin.iOS アプリで過去の購入の復元にできるようにする方法について説明します。 購入とサーバーによって提供される製品をセキュリティで保護する方法についても説明します。
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 83f5fd233c004271169a4d00d0a65e70aa925b95
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117656"
---
# <a name="transactions-and-verification-in-xamarinios"></a>トランザクションと Xamarin.iOS での検証

## <a name="restoring-past-transactions"></a>過去のトランザクションを復元します。

アプリケーションには、復元可能な製品の種類がサポートしている場合は、これらの購入を復元できるようにするいくつかのユーザー インターフェイス要素を含める必要があります。
この機能により、顧客またはクリーン ワイプした後、同じデバイスに製品を復元する、製品の追加のデバイスを追加または削除して、アプリを再インストールします。 次の製品の種類は復元です。

-  非コンシューマブル製品
-  自動更新可能なサブスクリプション
-  無料サブスクリプション

復元プロセスは、レコードを更新する必要があります、製品を満たすためにデバイス上に保持します。 お客様は、任意のデバイスで、いつでも復元を選択できます。 復元プロセスは、そのユーザーの以前のすべてのトランザクションを再送信します。アプリケーション コードでは、(デバイスで、その購入のレコードが既にある場合とそうでない購入のレコードを作成する場合にチェックとユーザーの製品を有効にするなど) は、その情報を取得するには、どのような操作を決定する必要があります。

### <a name="implementing-restore"></a>復元を実装します。

ユーザー インターフェイス**復元**ボタンで RestoreCompletedTransactions をトリガーする次のメソッドを呼び出し、`SKPaymentQueue`します。

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

Storekit や Apple のサーバーには、復元要求を非同期的に送信されます。   
   
`CustomPaymentObserver`が登録されている Apple のサーバーが応答すると、トランザクションのオブザーバーとしてメッセージを受け取ることができます。 応答は、このユーザーが (すべてのデバイス) でのこのアプリケーションで行われたすべてのトランザクションが含まれます。 トランザクションごとに、ループのコードを検出し、復元された状態の呼び出し、`UpdatedTransactions`次に示すように処理するメソッド。

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

ユーザーの復元可能な製品がない場合`UpdatedTransactions`は呼び出されません。   
   
サンプル内の特定のトランザクションを復元する最も簡単なコードは、購入の点を除いて、場所がいつと同じ操作、`OriginalTransaction`プロパティは、製品 ID へのアクセスに使用します。

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

その他の可能性がありますを確認するより高度な実装`transaction.OriginalTransaction`、元の日付と受信確認番号などのプロパティ。 この情報は、いくつか製品の種類 (サブスクリプション) などの便利になります。

#### <a name="restore-completion"></a>復元の完了

`CustomPaymentObserver` (正常にまたはエラーに)、復元プロセスが完了したときに、StoreKit によって呼び出される 2 つの追加のメソッドが次に示します。

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

例ではこれらのメソッド何もしないが、実際のアプリケーションにメッセージをユーザーまたはその他のいくつかの機能を実装することもできます。

## <a name="securing-purchases"></a>購入をセキュリティで保護します。

この 2 つの例の使用について説明`NSUserDefaults`購入を追跡するために。   
   
 **コンシューマブル**– クレジットの購入の '残高' は、単純な`NSUserDefaults`の購入ごとにインクリメントされる整数値。   
   
 **非コンシューマブル**– 写真フィルターのそれぞれの購入が、キーと値のペアとして格納されている`NSUserDefaults`します。

使用して`NSUserDefaults`がコードの例を単純なままで技術的に関心を持つユーザーは (支払い機構のバイパス) 設定を更新する可能性があるように、非常に安全なソリューションを提供しません。   
   
注: 実際のアプリケーションは、ユーザーが改ざんされる可能性がないコンテンツが購入を格納するためのセキュリティで保護されたメカニズムを採用する必要があります。 これは、暗号化やその他の手法をリモート サーバーの認証を含む必要があります。   
   
 メカニズムは、iOS、iTunes および iCloud の組み込みのバックアップと回復機能を活用するために設計することも必要があります。 これにより、ユーザーは、バックアップを復元した後、以前に購入はすぐに使用できるが保証されます。   
   
Apple のセキュリティで保護されたコーディング ガイドをご覧ください iOS 固有の詳細なガイドラインです。

## <a name="receipt-verification-and-server-delivered-products"></a>受信確認とサーバーによって提供される製品

これまでに、このドキュメントの例が、アプリ ストアのサーバーと直接通信するアプリケーションが購入のトランザクションは、ロック解除機能または既にアプリにコード化された機能を実行するだけの行いました。   
   
Apple では、購入の一部として、デジタル コンテンツを配信する前に、要求が検証に役に立ちます別のサーバーによって個別に確認する発注確認メッセージを許可することで購入のセキュリティのレベルを提供します (デジタル書籍など、または。マガジン)。   
   
 **組み込み製品**– アプリケーションに付属の機能として、このドキュメントの例に示すよう購入される製品が存在します。 アプリ内購入は、機能にアクセスできます。
製品 Id は、ハードコーディングしています。   
   
 **サーバーによって提供される製品**– 製品は、成功のトランザクションが原因で、コンテンツをダウンロードするまで、リモート サーバーに格納されているダウンロード可能なコンテンツで構成されています。
例については、書籍や雑誌などがあります。 (場所、製品のコンテンツがホストも) 外部のサーバー製品の Id に基づいている通常します。 アプリケーションでは、コンテンツのダウンロードが失敗した場合にできるように再実行しようとしたユーザーの混乱を招くことがなくトランザクションが完了するの記録のための優れた方法を実装する必要があります。

### <a name="server-delivered-products"></a>サーバーによって提供される製品

ブックと雑誌 (またはゲームのレベルでも) 購入手続きの際、リモート サーバーからダウンロードする必要があります。 など、いくつかの製品のコンテンツ。 つまり、格納、および購入後に製品のコンテンツを配信する追加のサーバーが必要です。

#### <a name="getting-prices-for-server-delivered-products"></a>サーバーによって提供される製品の価格の取得

製品がリモートで配信されるため、時間の経過と共に (アプリ コードの更新)、なし書籍や雑誌の新しい問題の詳細を追加するなどその他の製品を追加することがもします。 ように、アプリケーションでは、ニュースのこれらの製品を検出したり、ユーザーに表示することが、追加のサーバーは格納し、この情報を提供する必要があります。   
   
[![](transactions-and-verification-images/image38.png "サーバーによって提供される製品の価格の取得")](transactions-and-verification-images/image38.png#lightbox)   
   
1. 複数の場所で製品情報を格納する必要があります: iTunes Connect で、サーバー上とします。 さらに、各製品には、関連付けられているコンテンツ ファイルがあります。 これらのファイルは、成功した購入後に配信されます。   
   
2. ユーザーが製品を購入しようとすると、アプリケーションがどのような製品がありますを決定する必要があります。 この情報は、キャッシュすることがありますが、製品のマスター リストが格納されているリモート サーバーから配信する必要があります。   
   
3. サーバーは、製品 Id を解析するアプリケーションの一覧を返します。   
   
4. 次に、アプリケーションでは、価格と説明を取得する StoreKit に送信するこれらの製品 Id の決定します。   
   
5. StoreKit では、Apple のサーバーの製品 Id の一覧を送信します。   
   
6. ITunes サーバーは、有効な製品情報 (説明と現在の価格) で応答します。   
   
7. アプリケーションの`SKProductsRequestDelegate`ユーザーに表示するための製品情報を渡されます。

#### <a name="purchasing-server-delivered-products"></a>サーバーによって提供される製品の購入

リモート サーバーが何らかの方法の検証のコンテンツの要求が有効である必要があるため (つまり。 のために支払わ)、認証用に受信確認情報が渡されます。 リモート サーバーでは、検証のために iTunes にそのデータを転送して、成功した場合、アプリケーションへの応答で製品のコンテンツが含まれています。   
   
 [![](transactions-and-verification-images/image39.png "サーバーによって提供される製品の購入")](transactions-and-verification-images/image39.png#lightbox)   
   
1. アプリを追加、`SKPayment`をキューにします。 ユーザーが自分の Apple ID の入力を求めし、支払いの確認を求めに必要な場合。   
   
2. StoreKit では、サーバー処理のために、要求を送信します。   
   
3. トランザクションが完了したら、サーバーは、トランザクション受信で応答します。   
   
4. `SKPaymentTransactionObserver`サブクラスですが、受信確認を受信し、それを処理します。 製品は、サーバーからダウンロードする必要があります、ため、アプリケーションは、リモート サーバーへのネットワーク要求を開始します。   
   
5. リモート サーバーがコンテンツにアクセスする権限が確認できるように、受信データによって、ダウンロードの要求が伴います。 アプリケーションのネットワーク クライアントは、この要求に対する応答を待機します。   
   
6. サーバーは、コンテンツの要求を受信したときに、受信データを解析し、有効なトランザクションは、確認メッセージを確認するには、iTunes サーバーに直接要求を送信します。 サーバーは、いくつかのロジックを使用して、実稼働またはサンド ボックスの URL に要求を送信するかどうかを判断する必要があります。 Apple は、常に運用環境の URL を使用して場合、サンド ボックスへの切り替えを提案して 21007 (サンド ボックスの受信が実稼働サーバーに送信) の状態を受信します。 Apple を参照してください[レシートの検証のプログラミング ガイド](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)の詳細。
   
7. iTunes では、受信確認がチェックされ、有効な場合にステータス 0 を返します。   
   
8. サーバーは、iTunes の応答を待ちます。 有効な応答を受信した場合、コードは、アプリケーションへの応答に含める、関連付けられている製品のコンテンツ ファイルを見つける必要があります。   
  
9. アプリケーションが受信して、デバイスのファイル システムに製品の内容を保存する、応答を解析します。   
   
10. アプリケーションの製品を有効にし呼び出して StoreKit の`FinishTransaction`します。 アプリケーションは、購入済みコンテンツ (を購入した本や雑誌の問題の最初の表示ページなど) を必要に応じて表示し、可能性があります。

単純にトランザクションを迅速に完了できるようにトランザクション受信確認を手順 9 に格納、および実際の製品のコンテンツをダウンロードするユーザーのユーザー インターフェイスを提供する非常に大きな製品のコンテンツ ファイルの代替実装を含めることができます。後でします。 以降のダウンロード要求は、必要な製品のコンテンツ ファイルにアクセスするストアド確認メッセージを送信再ことができます。

### <a name="writing-server-side-receipt-verification-code"></a>サーバー側の受信確認コードを記述します。

サーバー側コードでの受領書を検証は、単純な HTTP POST 要求/応答のワークフロー図 8 から 5 の手順を含むで行うことができます。   
   
抽出、`SKPaymentTansaction.TransactionReceipt`アプリでのプロパティ。 これは、iTunes の検証 (手順 5) に送信する必要があるデータです。

(手順 5 または 6 でいずれかの) トランザクションの受信データを Base64 エンコードします。

このような単純な JSON ペイロードを作成します。

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST を JSON [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt)運用環境または[ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt)をテストするためです。   
   
 JSON 応答には、次のキーが含まれます。

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

0 の状態では、有効な受信確認を示します。 サーバーは、購入した製品のコンテンツを満たすために続行できます。 受信確認のキーと同じプロパティを持つ JSON ディクショナリを格納して、`SKPaymentTransaction`サーバー コードでの購入の数量 product_id などの情報を取得するには、このディクショナリが照会できるように、アプリで受信したオブジェクト。

Apple を参照してください。[レシートの検証のプログラミング ガイド](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html)に関する追加情報。
