---
title: StoreKit 概要と Xamarin.iOS で製品情報を取得します。
description: このドキュメントでは、StoreKit の概要を説明します。 StoreKit、StoreKit 相互作用をテスト、製品売上を表示する、処理、無効な製品および表示するローカライズされた価格で使用されるクラスがについて説明します。
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 964b97e82db8e79cb32598d0c955fac3ab122314
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787225"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>StoreKit 概要と Xamarin.iOS で製品情報を取得します。

アプリ内購入のユーザー インターフェイスは、次のスクリーン ショットに表示されます。
任意のトランザクションの実行前にアプリケーションは、製品の価格と表示の説明を取得する必要があります。 ユーザーが押したときに、**購入**アプリケーションが要求を送信、確認のダイアログ ボックスと Apple ID でのログインを管理する StoreKit です。 トランザクションは、成功すると、StoreKit 通知、アプリケーション コードと仮定するとトランザクションの結果を格納の購入へのアクセスをユーザーに提供し、する必要があります。   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit 通知、アプリケーション コードは、トランザクションの結果を格納しの購入へのアクセスをユーザーに提供する必要があります。")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>クラス

アプリ内購入を実装するには、StoreKit フレームワークから次のクラスが必要です。   
   
 **SKProductsRequest** – (App Store) を販売するために承認されている製品の StoreKit を要求します。 製品 Id の数で構成できます。

-   **SKProductsRequestDelegate** – 製品要求と応答を処理するメソッドを宣言します。 
-   **SKProductsResponse** – StoreKit (App Store) から代理人に送信します。 Id が要求と共に送信される製品に一致する SKProducts が含まれています。 
-   **SKProduct** – (つまり iTunes Connect で構成した) StoreKit から製品を取得します。 など、製品 ID、タイトル、説明、価格、製品に関する情報が含まれています。 
-   **SKPayment** – 製品 ID を作成され、注文書を実行する支払キューに追加します。 
-   **SKPaymentQueue** – Apple に送信する支払要求をキューに登録済みです。 通知が各支払いを処理中の結果として実行されます。 
-   **SKPaymentTransaction** : 完了したトランザクション (App Store で処理および StoreKit 経由でのアプリケーションに送信されている発注要求) を表します。 トランザクションは、Purchased、復元された、または失敗した可能性があります。 
-   **SKPaymentTransactionObserver** – StoreKit 支払キューによって生成されたイベントに応答するカスタム サブクラスです。 
-   **StoreKit 操作は非同期**-、SKProductRequest が開始されてか、SKPayment が、キューに追加されるコードに制御が返されます。 StoreKit は Apple のサーバーからデータを受け取ると、SKProductsRequestDelegate または SKPaymentTransactionObserver サブクラスでメソッドが呼び出すされます。 


次の図は、(アプリケーションでは抽象クラスを実装する必要があります)、さまざまな StoreKit クラス間の関係を示しています。   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "アプリでさまざまな StoreKit クラスの抽象クラス間の関係を実装する必要があります。")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 これらのクラスは、このドキュメントの後半で詳しく説明します。

## <a name="testing"></a>テスト中

ほとんどの StoreKit 操作では、テストの実際のデバイスが必要です。 (Ie 製品の情報の取得。 価格&amp;説明) は、シミュレーターが購入で動作し、復元操作には、エラーが返されます (FailedTransaction コードなど 5002 = 不明なエラーが発生しました)。

注: StoreKit は、iOS シミュレーターでは動作しません。 IOS シミュレーターでアプリケーションを実行して、支払キューを取得しようとすると、アプリケーション、StoreKit は警告を記録します。 ストアのテストは、実際のデバイスで行う必要があります。   
   
   
   
 重要: 署名しない設定 でテスト アカウントを使用します。 既存の Apple ID アカウントがサインイン設定アプリケーションを使用することができますし、入力を求めるに待機する必要があります*アプリ内購入シーケンス内で*Apple id テストを使用してログイン。   
   
   
   

テスト アカウントを使用して実際のストアにサインインしようとすると、自動的に変換されます、実際の Apple ID を そのアカウントは、テストに使用できるされなくなります。

StoreKit コードをテストするには、は、正規 iTunes テスト アカウントとログインのログアウトをテストのストアにリンクされている特殊なテスト アカウント (iTunes Connect で作成される) が必要です。 現在のアカウントのアクセスからサインアウト**設定 > iTunes と App Store**次のように。

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "現在のアカウントのアクセス設定 iTunes と App Store からサインアウトするには")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
テスト用のアカウントでサインインし*StoreKit によって要求されると、アプリ内で*:



作成するテスト ユーザー iTunes Connect でをクリックして**ユーザーおよびロール**メイン ページで、します。

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "ITunes でテスト ユーザーを作成するには、接続をクリックしてユーザーおよびロールのメイン ページで、")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

選択**サンド ボックスのテスト担当者**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "サンド ボックスのテスト担当者を選択します。")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

既存のユーザーの一覧が表示されます。 新しいユーザーを追加したり、既存のレコードを削除することができます。 ポータルがしません (現在) を表示または編集既存ため (特に、パスワードを割り当てる) を作成した各テスト ユーザーの適切なレコードを保持することをお勧めのテスト ユーザーを使用します。 テスト ユーザーを削除すると、電子メール アドレスが別のテスト アカウント用に再利用することはできません。  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "既存のユーザーの一覧が表示されます。")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 テスト用の新しいユーザーには、実際の Apple ID (名前、パスワード、秘密の質問および答え) などのような属性を持ちます。 ここに入力したすべての詳細のレコードを保持します。 **ITunes ストアの選択**フィールドは通貨を決定し、アプリ内購入言語には、ときに使用されますそのユーザーとしてのログに記録します。

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "ユーザーの通貨とアプリ内購入金額の言語、iTunes ストアの選択フィールドが決定されます。")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>製品情報の取得

アプリ内購入製品を販売している最初の手順で表示する: 表示するため、アプリ ストアから現在の価格と説明を取得します。   
   
   
   
 製品の種類のアプリを販売して (使用できる、サブスクリプションの種類、または非が使用できる) に関係なく、表示の製品情報を取得するプロセスは同じです。 この記事に付属している InAppPurchaseSample コードには、という名前のプロジェクトが含まれています。*消耗*を表示するための運用情報を取得する方法を示しています。 表示する方法。

-  実装を作成する`SKProductsRequestDelegate`を実装し、`ReceivedResponse`抽象メソッドです。 コード例では、これを呼んで、`InAppPurchaseManager`クラスです。 
-  支払いが許可されているかどうかを表示する StoreKit に確認 (を使用して`SKPaymentQueue.CanMakePayments`)。 
-  インスタンスを作成、 `SKProductsRequest` iTunes Connect で定義されている製品の Id を使用します。 これは、例の`InAppPurchaseManager.RequestProductData`メソッドです。 
-  Start メソッドを呼び出して、`SKProductsRequest`です。 これは、App Store サーバーへの非同期呼び出しをトリガーします。 デリゲート ( `InAppPurchaseManager` ) 結果と呼ばれるバックになります。 
-  デリゲートの ( `InAppPurchaseManager` )`ReceivedResponse`メソッドが (製品の価格と説明については、または無効な製品に関するメッセージ) のアプリ ストアから返されたデータで UI を更新します。 

全体の相互作用が次のよう ( **StoreKit**は iOS に組み込まれて、 **App Store** Apple のサーバーを表します)。

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "グラフの製品情報を取得します。")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>製品の情報の例を表示します。

[InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *消耗*サンプル コードでは、製品情報を取得する方法を示しています。 サンプルのメイン画面には、アプリ ストアから取得される 2 つの製品情報が表示されます。   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "メイン画面には、アプリ ストアから取得した情報製品が表示されます。")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 取得し、製品情報を表示するには、サンプル コードについてで詳しく説明します。


#### <a name="viewcontroller-methods"></a>ViewController メソッド

`ConsumableViewController`クラスは、製品 Id がハードコードされたクラスの 2 つの製品の価格の表示を管理します。

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

セットアップに使用されるレベルもあります、NSObject を宣言するクラス、`NSNotificationCenter`オブザーバー。

```csharp
NSObject priceObserver;
```

ViewWillAppear メソッドで、オブザーバーが作成され、既定の通知センターを使用して割り当てます。

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 最後に、`ViewWillAppear`メソッドを呼び出し、 `RequestProductData` StoreKit 要求を開始するメソッド。 この要求が確立されると、StoreKit は非同期的に、情報を取得し、アプリにフィード Apple のサーバーに問い合わせてください。 これを実現、`SKProductsRequestDelegate`サブクラス ( `InAppPurchaseManager`)」を次のセクションで説明されています。

```csharp
iap.RequestProductData(products);
```

価格と説明を表示するコードは、単に、SKProduct から情報を取得し、UIKit コントロールに割り当てられます (表示通知、`LocalizedTitle`と`LocalizedDescription`– StoreKit 正しいテキストとに基づいて料金を自動的に解決します。ユーザーのアカウントの設定)。 次のコードは、上で作成した通知に属します。

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

最後に、`ViewWillDisappear`メソッドでは、オブザーバーは削除を確認する必要があります。

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>SKProductRequestDelegate (InAppPurchaseManager) メソッド

`RequestProductData`メソッドが呼び出されてアプリケーションが製品の価格とその他の情報を取得しようとします。 正しいデータ型に製品 Id のコレクションを解析してそれを作成し、`SKProductsRequest`情報を使用します。 Start メソッドを呼び出すには、Apple のサーバーに対するネットワーク要求とが発生します。 要求は非同期に実行され、呼び出し、`ReceivedResponse`が正常に完了したときに、デリゲートのメソッドです。

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS が自動的に要求をルーティングする – アプリケーションが実行されているどのようなプロビジョニング プロファイルによって、アプリ ストアの 'サンド ボックス' または '運用' のバージョンにため、開発したり、アプリをテストするとき、要求はすべての製品へのアクセスiTunes Connect (でも送信または Apple によって承認されていないもの) で構成されます。 アプリケーションは、実稼働環境では場合、StoreKit 要求の情報を返しますのみ**Approved**製品です。   
   
   
   
 `ReceivedResponse` Apple のサーバーがデータに応答した後、オーバーライドされたメソッドが呼び出されます。 これが呼び出されるため、バック グラウンドで、コードは有効なデータを解析し、製品情報、ViewControllers 'をリッスンする' 通知を送信する通知を使用する必要があります。 有効な製品情報を収集し、通知を送信するコードを次に示します。

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

図に示されていませんが、 `RequestFailed` App Store サーバーが到達できない (またはその他のエラーが発生する) 場合に、ユーザーにフィードバックを提供できるように、メソッドをオーバーライドも必要があります。 コード例は、コンソール、単に書き込みますが、実際のアプリケーションは、クエリを実行することもできます`error.Code`(ユーザーに警告) などのプロパティと実装のカスタム動作。

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

このスクリーン ショットは、読み込み (製品情報を使用できない場合) の直後に、サンプル アプリケーションを示しています。

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "製品情報が利用できない場合の読み込み直後のサンプル アプリ")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>無効な製品

`SKProductsRequest`無効な製品 Id の一覧を返すも可能性があります。 通常、無効な製品は、次のいずれかが原因返されます。   
   
   
   
 **製品 ID の誤りがある**– 有効な製品 Id のみが受け入れられます。   
   
 **製品が承認されていない**– によって返されるすべての製品売上をオフになっているのテスト中に、 `SKProductsRequest`; が、実稼働環境では、承認済みの製品のみが返されます。   
   
 **アプリ ID は明示的な**– (アスタリスク) のワイルドカード アプリ Id はアプリ内購入を許可していません。   
   
 **プロビジョニング プロファイルが正しくない**– を再生成して、アプリをビルドする際に、適切なプロビジョニング プロファイルを使用してください (たとえば、アプリ内購入をする)、プロビジョニング ポータルでアプリケーション構成の変更を加えた場合。   
   
 **iOS アプリケーションの有料のコントラクトが設定されていない**– Apple 開発者アカウントの有効なコントラクトがない限り、StoreKit 機能はまったく動作しません。   
   
 **拒否済みの状態では、バイナリ**– 場合がありますが、以前に送信したバイナリ拒否済みの状態で (またはのいずれかでは、App Store チームの開発者によって) StoreKit 機能が機能しません。

`ReceivedResponse`のサンプル コードにメソッドがコンソールに無効な製品を出力します。

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>ローカライズされた価格を表示します。

価格レベルは、国際対応のすべてのアプリ ストア間の各製品の特定の価格を指定します。 価格は通貨ごとに正しく表示されるためには、次の拡張メソッドを使用して (で定義されている`SKProductExtension.cs`) それぞれの価格プロパティではなく`SKProduct`:

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

ボタンのタイトルを設定するコードは、次のように、拡張メソッドを使用します。

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

(アメリカ合衆国のストアの 1 つ)、日本語のストアのいずれかと次のスクリーン ショットでは 2 つの異なる iTunes テスト アカウントを使用するには。   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "2 つの異なる iTunes 言語の特定の結果を表示してアカウントをテストします。")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 ストアが製品の情報と価格の通貨の中に、デバイスの言語設定に影響を与えるラベルとその他のローカライズされたコンテンツが利用される言語に影響することを確認します。   
   
   
   
 テストする必要がありますアカウントが、別のストアを使用することに注意してください**サイン アウト**で、**設定 > iTunes と App Store**別のアカウントでサインインするアプリケーションを再起動しています。 移動する、デバイスの言語を変更する**設定 > [全般] > 国際 > 言語**します。

