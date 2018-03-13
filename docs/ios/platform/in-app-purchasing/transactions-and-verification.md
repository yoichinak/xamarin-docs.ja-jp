---
title: "トランザクションと検証"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 21f8c6738c00d5738c02962ee95b415e3855d740
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="transactions-and-verification"></a>トランザクションと検証

## <a name="restoring-past-transactions"></a>過去のトランザクションを復元します。

アプリケーションでは、復元可能な製品の種類をサポートする場合は、これらの購入を復元するユーザーを許可するユーザー インターフェイス要素の一部を含める必要があります。
この機能により、顧客、製品の追加のデバイスを追加または消去されている後に、同じデバイスに製品を復元または削除して、アプリを再インストールします。 次の製品の種類は、復元可能ながあります。

-  非が使用できる製品
-  自動更新可能なサブスクリプション
-  無料サブスクリプション


復元プロセスは、レコードを更新する必要があります、製品を満たすためにデバイス上に保持します。 お客様は、任意のデバイスにいつでも復元を選択できます。 復元プロセスは、そのユーザーの以前のすべてのトランザクションを再送信します。アプリケーション コードは必要があります (たとえば、チェック場合は、注文書のレコードを作成しないこと、およびデバイスで、その注文書のレコードが既にある場合や、ユーザーのために、製品の有効化) は、その情報を取得するには、どのような操作を確認します。

### <a name="implementing-restore"></a>復元の実装

ユーザー インターフェイス**復元**ボタンが RestoreCompletedTransactions のトリガーが、次のメソッドを呼び出して、`SKPaymentQueue`です。

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit は、Apple のサーバーには、復元要求を非同期的に送信されます。   
   
   
   
 `CustomPaymentObserver`が登録されている Apple のサーバーは、応答時に、トランザクションのオブザーバーとしてメッセージが表示にされます。 応答は、このユーザーが (すべてのデバイス) の間でのこのアプリケーションで行われたすべてのトランザクションが含まれます。 復元された状態と呼び出し、コードをトランザクションごとに、ループが検出された、`UpdatedTransactions`次に示すように処理するメソッド。

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

ユーザーの復元可能な製品が存在しない場合`UpdatedTransactions`は呼び出されません。   
   
   
   
 サンプル内の特定のトランザクションを復元する最も簡単なコードは、購入が行われる、点を除いてを参照する時期と同じ操作、`OriginalTransaction`製品 ID にアクセスするプロパティを使用します。

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

その他の可能性がありますを確認するより高度な実装`transaction.OriginalTransaction`、元の日付と受信確認番号などのプロパティです。 この情報は、一部の製品の種類 (サブスクリプション) などに便利ですがあります。

#### <a name="restore-completion"></a>復元の完了

`CustomPaymentObserver` (正常またはエラーのため、)、復元処理が完了したら、StoreKit によって呼び出される 2 つの追加方法がありますの下に表示します。

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

例ではこれらのメソッド何もしないが、実際のアプリケーションにメッセージをユーザーまたは他の機能を実装することをします。

## <a name="securing-purchases"></a>購入をセキュリティで保護します。

この 2 つの例の使用について説明`NSUserDefaults`購入を追跡するためにします。   
   
 **消耗**– クレジットの購入の '残高' は、単純な`NSUserDefaults`各購入を増加する整数値。   
   
 **非消耗**– 各写真フィルターの注文書は、キーと値のペアとして格納`NSUserDefaults`です。

使用して`NSUserDefaults`コード例を単純に保たれますが、技術的に関心を持つユーザー (支払機構のバイパス) 設定を更新する可能性がある可能性がありますと、非常に安全なソリューションは提供しません。   
   
注意: 実際のアプリケーションは、購入したユーザーが改ざんされる可能性がないコンテンツが格納するための安全なメカニズムを採用する必要があります。 これは、暗号化やリモート サーバーの認証を含むその他の手法にする必要があります。   
   
 メカニズムは、iOS、iTunes および iCloud の組み込みのバックアップと回復の機能を活用するためにも設計する必要があります。 これにより、ユーザーは、バックアップを復元した後、以前に購入されるすぐに使用します。   
   
   
 Apple のセキュリティで保護されたコーディングのガイドを参照 iOS に固有の詳細なガイドラインです。

## <a name="receipt-verification-and-server-delivered-products"></a>受信確認とサーバーによって提供される製品

これまで、このドキュメントの例が App Store サーバーと直接通信するアプリケーションが機能または機能をアプリに既にコード化されたロックを解除する購入トランザクションを実行するだけの主なものです。   
   
   
   
 Apple では、注文書のセキュリティのレベルを上げるため、購入の一部としてデジタル コンテンツを配信する前に、要求を検証する役に立ちます別のサーバーによって個別に検証する注文書確認メッセージを許可する (デジタル書籍など、またはマガジン)。   
   
   
   
 **組み込みの製品**: アプリケーションに付属機能としてこのドキュメントの例についてと同様に購入される製品が存在します。 アプリ内購入を使用して、機能にアクセスできます。
製品 Id は、ハードコードされたです。   
   
 **製品のサーバーによって提供される**: 製品は、成功したトランザクションが原因で、コンテンツをダウンロードするまで、リモート サーバーに格納されているダウンロード可能なコンテンツで構成されます。
例としては、書籍や雑誌にあります。 通常の製品 Id をソース (ここで、製品のコンテンツはホストにも) 外部のサーバーはします。 アプリケーションでは、コンテンツのダウンロードが失敗した場合にできるように再実行しようとしたユーザーの混乱を招くことがなくトランザクションが完了するを記録するための信頼性の高い方法を実装する必要があります。

### <a name="server-delivered-products"></a>サーバーによって提供される製品

ブックと雑誌 (またはゲームのレベルでも)、発注プロセス中に、リモート サーバーからダウンロードする必要があります。 など、いくつかの製品のコンテンツ、です。 これは、追加のサーバーを保存し、購入後に、製品のコンテンツを配信するために必要なことを意味します。

#### <a name="getting-prices-for-server-delivered-products"></a>サーバーによって提供される製品の価格を取得します。

製品がリモートで配信されるため、書籍や雑誌の新しい懸案事項の詳細の追加など、(コードを更新せずアプリケーション)、時間の経過と共に他の製品を追加することはもです。 アプリケーションは、これらニュースの製品を検出したり、ユーザーに表示できる、ように追加のサーバーを格納し、この情報を提供します。   
   
   
   
 [![](transactions-and-verification-images/image38.png "サーバーによって提供される製品の価格を取得します。")](transactions-and-verification-images/image38.png#lightbox)   
   
   
   
 1. 複数の場所で製品情報を保存する必要があります: サーバー上と iTunes Connect でします。 さらに、各製品には、関連付けられているコンテンツ ファイルがあります。 これらのファイルは、成功、購入後に配信されます。   
   
 2. ユーザーが製品を購入しようとすると、アプリケーションがどのような製品を決定する必要があります。 この情報は、キャッシュされる可能性は、製品のマスター リストが格納されているリモート サーバーから配信する必要があります。   
   
 3. サーバーでは、製品 Id を解析するためにアプリケーションの一覧を返します。   
   
 4. その後、アプリケーションでは、価格と説明を取得する StoreKit に送信するこれらの製品 Id のどれを決定します。   
   
 5. StoreKit は、Apple のサーバーに製品 Id の一覧を送信します。   
   
 6. ITunes サーバーは、有効な製品の情報 (説明と現在の価格) で応答します。   
   
 7. アプリケーションの`SKProductsRequestDelegate`表示の製品情報をユーザーに渡されます。

#### <a name="purchasing-server-delivered-products"></a>サーバーによって提供される製品の購入

リモート サーバーが何らかの方法がコンテンツ要求が有効なことを検証する必要があるため (ie。 のために支払わ)、認証用に受信確認情報が渡されます。 リモート サーバーでは、検証を iTunes にそのデータを転送し、成功した場合、アプリケーションへの応答で製品のコンテンツが含まれています。   
   
 [![](transactions-and-verification-images/image39.png "サーバーによって提供される製品の購入")](transactions-and-verification-images/image39.png#lightbox)   
   
 1. アプリを追加、`SKPayment`をキューにします。 ユーザーの Apple ID の入力を求めし、する、支払いの確認を求めに必要な場合です。   
   
 2. StoreKit は、サーバー上で処理する要求を送信します。   
   
 3. トランザクションが完了したら、サーバーはトランザクションの確認メッセージで応答します。   
   
 4. `SKPaymentTransactionObserver`サブクラスは、配信確認メッセージを受信し、それを処理します。 製品は、サーバーからダウンロードする必要があります、ために、アプリケーションは、リモート サーバーへのネットワーク要求を開始します。   
   
 5. リモート サーバーでは、コンテンツにアクセスする権限を検証できるように、受信データによってダウンロード要求が伴います。 アプリケーションのネットワーク クライアントは、この要求に対する応答を待機します。   
   
 6. サーバーは、コンテンツの要求を受信したときに、受信データが解析し、配信確認メッセージを確認するには、iTunes サーバーに直接要求は有効なトランザクションを送信します。 サーバーは、実稼働またはサンド ボックスの URL に要求を送信するのにかどうかを決定するのにいくつかのロジックを使用する必要があります。 Apple 提案常に実稼働環境の URL を使用して場合サンド ボックスに切り替えステータス: 21007 (サンド ボックス receipt 実稼働サーバーに送信)-を参照してください[テクニカル ノート 2259 FAQ #16](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)です。   
   
 7. iTunes を配信確認メッセージを確認し、有効な場合に 0 の状態を返します。   
   
 8. サーバーは、iTunes の応答を待機します。 有効な応答を受信した場合、コードは、アプリケーションへの応答に含める関連付けられている製品のコンテンツ ファイルを見つける必要があります。   
   
 9. アプリケーションが受信して、デバイスのファイル システムに製品コンテンツを保存して、応答を解析します。   
   
 10. アプリケーションの製品を有効にしてから StoreKit の`FinishTransaction`します。 アプリケーションは、購入済みコンテンツ (たとえば、最初のページ表示購入した書籍や雑誌の問題の) を必要に応じて表示し、可能性があります。

単に、トランザクションをすばやく完了するように、トランザクションの配信確認メッセージを手順 9 に格納、および実際の製品のコンテンツをダウンロードするユーザーのユーザー インターフェイスを提供する非常に大規模な製品のコンテンツ ファイルの代替実装を含めることが後でします。 以降のダウンロード要求は、必要な製品のコンテンツ ファイルにアクセスするストアド確認メッセージを送信再ことができます。

### <a name="writing-server-side-receipt-verification-code"></a>サーバー側の受信確認コードの記述

サーバー側コードで受信確認を検証するは、単純な HTTP POST 要求/応答 #5. ~ 8 ワークフロー図での手順を含むで行うことができます。   
   
   
   
 抽出、`SKPaymentTansaction.TransactionReceipt`アプリでのプロパティです。 これは、iTunes の検証 (手順 5) に送信する必要があるデータです。

(手順 5 または 6 のいずれかの) トランザクションの受信データを Base64 エンコードします。

次のように単純な JSON ペイロードを作成します。

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST を JSON [https://buy.itunes.apple.com/verifyReceipt](https://buy.itunes.apple.com/verifyReceipt)運用環境または[https://sandbox.itunes.apple.com/verifyReceipt](https://sandbox.itunes.apple.com/verifyReceipt)テストします。   
   
 JSON 応答には、次のキーが含まれます。

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

0 の状態では、有効な受信確認を示します。 サーバーは、購入した製品のコンテンツを満たすために続行できます。 受信確認キーにはと同じプロパティを使用して JSON ディクショナリが含まれています、`SKPaymentTransaction`サーバー コードは、購入の数量と product_id などの情報を取得するには、このディクショナリにクエリを実行するために、アプリケーションによって受信されたオブジェクト。

Apple を参照してください[ストア領収書の検証](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StoreKitGuide/VerifyingStoreReceipts/VerifyingStoreReceipts.html#//apple_ref/doc/uid/TP40008267-CH104-SW1)のドキュメントに追加情報。

#### <a name="using-aspnet"></a>ASP.NET を使用してください。

C# 開発者と呼ばれる便利オープン ソース プロジェクト[APNS シャープ](https://github.com/Redth/APNS-Sharp)ASP.NET で動作する受信確認コードが含まれます。 具体的には、`Receipt.cs`と`ReceiptVerification.cs`内のファイル、 [ `Jdsoft.Apple.AppStore` ](https://github.com/Redth/APNS-Sharp/tree/master/JdSoft.Apple.AppStore) Server-Delivered 製品ワークフロー図から手順 6. 8 ~ # を簡単に実装する .NET web サイトにディレクトリを追加することができます。   
   
   
   
 ITunes サーバーとの通信は、これは c# で処理する簡単な JSON を使用します。   
   
   
   
 Apple のアプリ内購入を参照してください[受信検証](https://developer.apple.com/library/ios/#releasenotes/StoreKit/IAP_ReceiptValidation/_index.html)領収書が有効であることを確認する方法についての詳細なドキュメントです。

### <a name="3rd-party-receipt-verification"></a>サード パーティの受信確認

受信確認 (およびなど)、独自のサーバーを構成する代わりに使用できるプラットフォームを提供している会社があります。 Xamarin はこれらのサービスを裏書きしません参照は、ここでは、説明単にしています。

#### <a name="urban-airship"></a>都市飛行船

都市飛行船は、受信確認とプッシュ通知を含む iOS アプリ用の別のバックエンド サービスの数を提供します。   
   
 [http://urbanairship.com/products/in-app-purchase/](http://urbanairship.com/products/in-app-purchase/)



