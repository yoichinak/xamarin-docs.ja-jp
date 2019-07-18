---
title: StoreKit の概要と Xamarin.iOS で製品情報を取得します。
description: このドキュメントでは、StoreKit の概要を示します。 Storekit や、storekit や相互作用をテスト、販売製品を表示する、無効な製品は、処理、およびローカライズされた価格を表示すると共に使用するクラスがについて説明します。
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 0dcda2e4fd1ca7773668a0a6fdf46e01f2f0841d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61366965"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>StoreKit の概要と Xamarin.iOS で製品情報を取得します。

次のスクリーン ショットは、アプリ内購入物のユーザー インターフェイスに表示されます。
任意のトランザクションの実行前に、アプリケーションは、製品の価格と表示の説明を取得する必要があります。 ユーザーが押したときに、**購入**アプリケーション、確認のダイアログ ボックスと Apple ID でのログインを管理する StoreKit への要求。 Storekit や、アプリケーション コードに通知し、トランザクションが成功したと仮定するとトランザクションの結果を格納の購入へのアクセスをユーザーに提供し、する必要があります。   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "Storekit や通知、アプリケーション コードは、トランザクションの結果を格納しの購入へのアクセスをユーザーに提供する必要があります。")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>クラス

アプリ内購入を実装するには、storekit やフレームワークから次のクラスが必要です。   
   
 **SKProductsRequest** – StoreKit へ (App Store) を販売する承認済みの商品の要求。 製品 Id の数が構成できます。

-   **SKProductsRequestDelegate** – 製品要求と応答を処理するメソッドを宣言します。 
-   **SKProductsResponse** – StoreKit (App Store) から代理人に送信します。 Id が要求と共に送信される製品に一致する SKProducts が含まれています。 
-   **SKProduct** – (を iTunes Connect で構成した) StoreKit から製品を取得します。 製品 ID、タイトル、説明、価格など、製品についての情報が含まれています。 
-   **SKPayment** – 製品 id を作成し、購入を実行する支払キューに追加します。 
-   **SKPaymentQueue** – Apple に送信される支払い要求をキューに登録済みです。 支払いが処理されている各結果は、通知がトリガーされます。 
-   **SKPaymentTransaction** – 完了したトランザクション (App Store によって処理および storekit や経由でのアプリケーションに送信された発注要求) を表します。 トランザクションは、Purchased、復元済みまたは失敗した可能性があります。 
-   **SKPaymentTransactionObserver** – storekit や支払いのキューによって生成されたイベントに応答するカスタム サブクラスです。 
-   **StoreKit の操作は非同期**-、SKProductRequest が開始または、SKPayment がキューに追加されて、コードに制御が返されます。 StoreKit が Apple のサーバーからデータを受信したときは、SKProductsRequestDelegate または SKPaymentTransactionObserver サブクラスでメソッドを呼び出します。 


次の図は、(抽象クラスは、アプリケーションで実装する必要があります)、さまざまな storekit やクラス間の関係を示しています。   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "アプリでさまざまな storekit やクラスの抽象クラス間の関係を実装する必要があります。")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 これらのクラスは、このドキュメントの後半で詳しく説明します。

## <a name="testing"></a>テスト

StoreKit のほとんどの操作では、テストの実際のデバイスが必要です。 (つまり製品の情報の取得 価格&amp;説明) は、シミュレーターが発注機能および復元操作はエラーを返します (FailedTransaction コードなど 5002 を = 不明なエラーが発生しました)。

メモ:StoreKit は、iOS シミュレーターでは動作しません。 IOS シミュレーターでアプリケーションを実行して、支払キューを取得しようとすると、アプリケーション、StoreKit は警告を記録します。 ストアのテストは、実際のデバイスで行う必要があります。   
   
   
   
 重要 : 設定 アプリケーションのテスト アカウントでサインインしない操作を行います。 すべての既存の Apple ID アカウントからの署名を設定アプリケーションを使用することができますし、ダイアログを表示するまで待機する必要があります*アプリ内購入するシーケンス内で*テスト Apple ID を使用してログインするには   
   
   
   

テスト アカウントを使用して実際のストアにサインインを試みると、自動的に変換されます実際の Apple id そのアカウントは、テストに使用できるされなくなります。

Storekit やコードをテストするには、テスト ストアにリンクされている (iTunes Connect で作成された) 特別なテスト アカウントで、通常の iTunes テスト アカウントとログインのログアウトがあります。 現在のアカウントのアクセスからサインアウト**設定 > iTunes アプリ ストア**次のように。

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "現在のアカウントのアクセス設定の iTunes とアプリ ストアからサインアウトするには")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
テスト アカウントでサインイン*storekit や、アプリ内での要求時*:



作成する iTunes Connect でのテスト ユーザーのクリックしてで**ユーザーおよびロール**のメイン ページ。

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "ITunes でテスト ユーザーを作成するには、接続 をクリックしてユーザーおよびロールのメイン ページで、")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

選択**サンド ボックス テスト担当者**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "サンド ボックス テスト担当者を選択します。")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

既存のユーザーの一覧が表示されます。 新しいユーザーを追加したり、既存のレコードを削除することができます。 ポータルはありません (現在) の表示または編集既存のテスト ユーザーをため (特に割り当てたパスワード) に作成される各テスト ユーザーの適切なレコードを保持することをお勧めすることができるようにします。 テスト ユーザーを削除すると、電子メール アドレスが別のテスト アカウントの再利用することはできません。  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "既存のユーザーの一覧が表示されます。")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 新しいテスト ユーザーでは、(名、パスワード、秘密の質問と回答) などの Apple ID を実際に類似する属性があります。 ここに入力したすべての詳細を記録します。 **ITunes ストアの選択**フィールドは通貨を判断し、言語、アプリ内購入には、ときに使用されますとしてそのユーザーでログインします。

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "ユーザーの通貨と言語のアプリ内購入を iTunes ストアの選択フィールドが決まります")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>製品情報の取得

アプリ内購入製品を販売する最初の手順が表示する: 表示のアプリ ストアから現在の価格と説明を取得します。   
   
   
   
 製品の種類のアプリを販売して (使用できる、非コンシューマブルまたはサブスクリプションの種類) に関係なく、表示するための製品情報を取得するプロセスは同じです。 この記事に付属する InAppPurchaseSample コードには、という名前のプロジェクトが含まれています。*消耗品*表示に関する運用情報を取得する方法を示します。 表示する方法。

-  実装を作成`SKProductsRequestDelegate`を実装し、`ReceivedResponse`抽象メソッド。 コード例はこれを呼び出す、`InAppPurchaseManager`クラス。 
-  Storekit や支払いを許可するかどうかを確認すると確認 (を使用して`SKPaymentQueue.CanMakePayments`)。 
-  インスタンスを作成、 `SKProductsRequest` iTunes Connect で定義されている製品 Id を持つ。 これを行う例の`InAppPurchaseManager.RequestProductData`メソッド。 
-  Start メソッドを呼び出す、`SKProductsRequest`します。 これは、アプリ ストア サーバーへの非同期呼び出しをトリガーします。 デリゲート ( `InAppPurchaseManager` ) と呼ばれるバックを結果になります。 
-  デリゲートの ( `InAppPurchaseManager` )`ReceivedResponse`メソッドは、(製品の価格と説明については、または無効な製品についてのメッセージ) は、アプリ ストアから返されたデータで UI を更新します。 

次のような全体的な相互作用 ( **StoreKit**が iOS に組み込まれて、 **App Store** Apple のサーバーを表します)。

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "グラフの製品情報を取得します。")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>製品情報の例を表示します。

[InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *消耗品*サンプル コードでは、製品情報を取得する方法を示しています。 サンプルのメイン画面には、アプリ ストアから取得した 2 つの製品情報が表示されます。   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "メイン画面には、アプリ ストアから取得した情報製品が表示されます。")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 取得し、製品情報を表示するには、サンプル コードについては、以下で詳しく説明します。


#### <a name="viewcontroller-methods"></a>ViewController メソッド

`ConsumableViewController`クラスは、製品 Id がクラスにハードコーディングされた 2 つの製品の価格の表示を管理します。

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

セットアップに使用されるレベルもあります、NSObject を宣言するクラスを`NSNotificationCenter`オブザーバー。

```csharp
NSObject priceObserver;
```

ViewWillAppear メソッドでは、オブザーバーが作成され、既定の通知センターを使用して割り当てられています。

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 最後に、`ViewWillAppear`メソッドを呼び出し、 `RequestProductData` storekit や要求を開始するメソッド。 この要求が確立されると、StoreKit は非同期的に、情報を取得し、アプリケーションにフィード、Apple のサーバーに問い合わせてください。 これによって、`SKProductsRequestDelegate`サブクラス ( `InAppPurchaseManager`) [次へ] のセクションで説明します。

```csharp
iap.RequestProductData(products);
```

価格と説明を表示するコードは、単に、SKProduct から情報が取得され、UIKit コントロールに割り当てられます (表示通知、`LocalizedTitle`と`LocalizedDescription`– storekit や、正しいテキストとに基づいて価格を自動的に解決します。ユーザーのアカウント設定)。 次のコードは、上記で作成した通知が属しています。

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

最後に、`ViewWillDisappear`メソッドでは、オブザーバーを削除を確認する必要があります。

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>SKProductRequestDelegate (InAppPurchaseManager) メソッド

`RequestProductData`メソッドは、アプリケーションが製品価格やその他の情報を取得する必要があるときに呼び出されます。 正しいデータ型に製品 Id のコレクションを解析します。 その後で作成、`SKProductsRequest`その情報を使用します。 Start メソッドを呼び出すには、Apple のサーバーに対するネットワーク要求とが発生します。 要求は非同期的に実行され、呼び出し、`ReceivedResponse`が正常に完了したときに、デリゲートのメソッド。

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

iOS は自動的に要求をルーティングする – アプリケーションが実行されているプロビジョニング プロファイルによって、アプリ ストアの 'サンド ボックス' または 'production' のバージョンに、要求がすべての製品にアクセスを持つアプリのテストまたは開発するとき、iTunes Connect (も送信または Apple によって承認されていないもの) で構成されます。 アプリケーションは、実稼働環境では、ときに storekit や要求がのみの情報を返す**Approved**製品です。   
   
   
   
 `ReceivedResponse`データを Apple のサーバーが応答した後、オーバーライドされたメソッドが呼び出されます。 これは、バック グラウンドで呼び出される、ためコードは有効なデータを解析し、製品情報をすべて ViewControllers 'をリッスンする ' 通知を送信する通知を使用する必要があります。 有効な製品情報を収集し、通知を送信するコードは、以下に示します。

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

図では、示されていませんが、`RequestFailed`メソッドは、App Store のサーバーに到達できない (またはその他のエラーが発生した) 場合は、ユーザーにフィードバックを提供できるようにもオーバーライドする必要があります。 コード例は、コンソール、単に書き込みますが、実際のアプリケーションは、クエリを実行することもできます`error.Code`(ユーザーに警告) などのプロパティと実装のカスタム動作。

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

このスクリーン ショットでは、(使用可能な製品情報がありませんが) 場合の読み込み直後にサンプル アプリケーションを示します。

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "製品情報が利用できない場合の読み込み直後に、サンプル アプリ")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>無効な製品

`SKProductsRequest`無効な製品 Id の一覧を返すも可能性があります。 無効な製品は通常、次のいずれかにより返されます。   
   
   
   
 **製品 ID の誤りがある**– 有効な製品 Id のみが受け入れられます。   
   
 **製品が承認されていない**– からテストの間の販売がオフになっているすべての製品が返される、`SKProductsRequest`が運用環境では、承認済みの製品のみが返されます。   
   
 **アプリ ID は明示的な**– (アスタリスク) をワイルドカード アプリ Id はアプリ内購入を許可していません。   
   
 **プロビジョニング プロファイルが正しくない**– を再生成し、正しいプロビジョニング プロファイルを使用して、アプリを構築するときに注意してください (などを有効にするアプリ内購入)、プロビジョニング ポータルで、アプリケーションの構成を変更する場合。   
   
 **iOS アプリケーションの有料のコントラクトが設定されていない**– Apple 開発者アカウントの有効な契約がない限り、storekit や機能はまったく動作しません。   
   
 **バイナリが拒否状態**– にある場合、以前に送信したバイナリ (または、App Store チームが開発者によって) 拒否状態 storekit や機能は機能しません。

`ReceivedResponse`サンプル コードでメソッドの出力をコンソールに無効な製品。

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

価格レベルは、国際対応のすべてのアプリ ストアの間で、各製品の特定の価格を指定します。 各通貨の価格が正しく表示させるには、次の拡張メソッドを使用して、(で定義されている`SKProductExtension.cs`) それぞれの価格のプロパティではなく`SKProduct`:

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

ボタンのタイトルを設定するコードは、このような拡張メソッドを使用します。

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

(アメリカ合衆国のストアの 1 つ)、日本語のストアのいずれかと次のスクリーン ショットでは 2 つの異なる iTunes テスト アカウントを使用するには。   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "2 つの異なる iTunes 言語の特定の結果を表示するアカウントをテストします。")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 ストアが製品の情報と価格の通貨、デバイスの言語設定は、ラベルとその他のローカライズされたコンテンツに影響します。 中に使用される言語に影響することを確認します。   
   
   
   
 アカウントにする必要がありますをテストするさまざまなストアを使用することを思い出してください**サインアウト**で、**設定 > iTunes アプリ ストア**して別のアカウントでサインインするアプリケーションを再起動します。 移動するデバイスの言語を変更する**設定 > [全般] > 国際 > 言語**します。

