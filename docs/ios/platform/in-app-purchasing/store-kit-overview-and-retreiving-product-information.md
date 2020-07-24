---
title: StoreKit の概要と Xamarin での製品情報の取得
description: このドキュメントでは、StoreKit の概要について説明します。 StoreKit で使用されるクラス、StoreKit の相互作用のテスト、販売の製品の表示、無効な製品の処理、およびローカライズされた価格の表示について説明します。
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: f05bc534b1220e6659f123a17dc57e02185fdea4
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996345"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>StoreKit の概要と Xamarin での製品情報の取得

アプリ内購入のユーザーインターフェイスを次のスクリーンショットに示します。
トランザクションが実行される前に、アプリケーションは、表示する製品の価格と説明を取得する必要があります。 その後、ユーザーが [**購入**] を押すと、アプリケーションは storekit に対して、確認ダイアログと Apple ID ログインを管理する要求を行います。 トランザクションが成功した場合、StoreKit はアプリケーションコードに通知します。このコードには、トランザクションの結果を格納し、ユーザーに購入へのアクセスを提供する必要があります。   

 [![StoreKit は、トランザクションの結果を格納し、ユーザーに購入へのアクセスを提供する必要があることをアプリケーションコードに通知します。](store-kit-overview-and-retreiving-product-information-images/image14.png)](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>クラス

アプリ内購入を実装するには、StoreKit フレームワークの次のクラスが必要です。   

 **Skproducts 要求**: 販売する承認済み製品の storekit への要求 (App Store)。 複数の製品 Id を使用して構成できます。

- **Sk$ Requestdelegate** –製品の要求と応答を処理するメソッドを宣言します。
- **Sk製品応答**– storekit (App Store) からデリゲートに返されます。 要求と共に送信される製品 Id に一致する SKProducts を含みます。
- **Skproduct** – Storekit (iTunes Connect で構成したもの) から取得された製品です。 製品 ID、タイトル、説明、価格などの製品に関する情報が含まれています。
- **Skpayment** –製品 ID で作成され、購入を行うために支払キューに追加されます。
- **SKPaymentQueue** – Apple に送信される、キューに入れられた支払い要求です。 通知は、各支払いの処理結果としてトリガーされます。
- **SKPaymentTransaction** –完了したトランザクション (アプリストアによって処理され、storekit を介してアプリケーションに送信される購入要求) を表します。 トランザクションを購入、復元、または失敗させることができます。
- **SKPaymentTransactionObserver** – storekit の支払キューによって生成されるイベントに応答するカスタムサブクラスです。
- **Storekit 操作は非同期です**。 SKProductRequest が開始された後、または sk支払いがキューに追加されると、コードに制御が返されます。 StoreKit は、Apple のサーバーからデータを受信するときに、SKProductsRequestDelegate または SKPaymentTransactionObserver サブクラスでメソッドを呼び出します。

次の図は、さまざまな StoreKit クラス間の関係を示しています (抽象クラスはアプリケーションで実装する必要があります)。   

 [![さまざまな StoreKit クラスの抽象クラス間のリレーションシップをアプリに実装する必要があります](store-kit-overview-and-retreiving-product-information-images/image15.png)](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   

これらのクラスの詳細については、このドキュメントの後半で説明します。

## <a name="testing"></a>テスト

ほとんどの StoreKit 操作には、テスト用の実際のデバイスが必要です。 製品情報を取得しています (ie 料金 &amp; の説明) はシミュレーターで動作しますが、購入および復元操作でエラーが返されます (例: 5002 Transaction Code = "不明なエラーが発生しました。" など)。

注: StoreKit は、iOS シミュレーターでは動作しません。 IOS シミュレーターでアプリケーションを実行する場合、アプリケーションで支払いキューを取得しようとすると、StoreKit によって警告がログに記録されます。 ストアのテストは、実際のデバイスで行う必要があります。   

重要: 設定アプリケーションでは、テストアカウントでサインインしないでください。 設定アプリケーションを使用して、既存の Apple ID アカウントからサインアウトすることができます。その後、*アプリ内購入シーケンス内で*、テスト Apple id を使用してログインするように求めるメッセージが表示されるまで待機する必要があります。   

テストアカウントを使用して実際のストアにサインインしようとすると、自動的に実際の Apple ID に変換されます。 このアカウントは、テストに使用できなくなります。

StoreKit コードをテストするには、通常の iTunes テストアカウントからログアウトし、テストストアにリンクされている特別なテストアカウント (iTunes Connect で作成) を使用してログインする必要があります。 現在のアカウントからサインアウトするには、次に示すように、 **iTunes と App Store > の設定**にアクセスします。

 [![現在のアカウントからサインアウトするには、[設定] [iTunes と App Store] を参照してください。](store-kit-overview-and-retreiving-product-information-images/image16.png)](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)

次に、*アプリ内で StoreKit によって要求されたとき*に、テストアカウントでサインインします。

ITunes でテストユーザーを作成するには、メインページで [**ユーザーとロール**] をクリックします。

 [![ITunes Connect でテストユーザーを作成するには、メインページで [ユーザーとロール] をクリックします。](store-kit-overview-and-retreiving-product-information-images/image17.png)](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

**サンドボックスのテスト担当**者の選択

 [![サンドボックスのテスト担当者の選択](store-kit-overview-and-retreiving-product-information-images/image18.png)](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

既存のユーザーの一覧が表示されます。 新しいユーザーを追加したり、既存のレコードを削除したりできます。 現在、ポータルでは、既存のテストユーザーを表示したり編集したりすることはできません。そのため、作成された各テストユーザー (特に、割り当てたパスワード) の適切な記録を保持することをお勧めします。 テストユーザーを削除すると、別のテストアカウントに電子メールアドレスを再利用することはできません。  

 [![既存のユーザーの一覧が表示されます。](store-kit-overview-and-retreiving-product-information-images/image19.png)](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   

 新しいテストユーザーは、実際の Apple ID (名前、パスワード、秘密の質問、回答など) に似た属性を持っています。 ここで入力したすべての詳細を記録しておきます。 **[ITunes ストアの選択**] フィールドでは、そのユーザーとしてログインしたときにアプリ内購入で使用する通貨と言語が決定されます。

 [![[ITunes ストアの選択] フィールドによって、アプリ内購入のユーザーの通貨と言語が決定されます。](store-kit-overview-and-retreiving-product-information-images/image20.png)](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>製品情報の取得

アプリ内購入製品を販売するための最初のステップでは、表示するために、アプリストアから現在の価格と説明を取得しています。   

アプリで販売されている製品の種類 (消費、非消費、または種類のサブスクリプション) に関係なく、表示する製品情報を取得するプロセスは同じです。 この記事に付属する InAppPurchaseSample コードには、表示のために運用情報を取得する方法を示す、*コンシューマ*という名前のプロジェクトが含まれています。 次の方法を示します。

- の実装を作成 `SKProductsRequestDelegate` し、抽象メソッドを実装し `ReceivedResponse` ます。 このコード例では、このクラスを呼び出し `InAppPurchaseManager` ます。
- StoreKit で、支払いが許可されているかどうかを確認します (を使用 `SKPaymentQueue.CanMakePayments` )。
- `SKProductsRequest`ITunes Connect で定義されている製品 id を使用して、をインスタンス化します。 これは、例のメソッドで行い `InAppPurchaseManager.RequestProductData` ます。
- で Start メソッドを呼び出し `SKProductsRequest` ます。 これにより、App Store サーバーへの非同期呼び出しがトリガーされます。 デリゲート () は、 `InAppPurchaseManager` 結果と共に呼び出されます。
- デリゲートの ( `InAppPurchaseManager` ) メソッドは、 `ReceivedResponse` アプリストアから返されたデータ (製品の価格 & の説明、または無効な製品に関するメッセージ) を使用して UI を更新します。

全体的な相互作用は次のようになります ( **Storekit**は iOS に組み込まれており、 **App Store**は Apple のサーバーを表します)。

 [![製品情報の取得のグラフ](store-kit-overview-and-retreiving-product-information-images/image21.png)](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>製品情報の表示例

[InAppPurchaseSample](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit)の*消耗品*のサンプルコードでは、製品情報を取得する方法を示しています。 サンプルのメイン画面には、App Store から取得した2つの製品の情報が表示されます。   

 [![メイン画面には、App Store から取得した情報が表示されます。](store-kit-overview-and-retreiving-product-information-images/image23.png)](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   

製品情報を取得して表示するサンプルコードについては、以下で詳しく説明します。

#### <a name="viewcontroller-methods"></a>ViewController メソッド

クラスは、 `ConsumableViewController` クラスで製品 id がハードコードされている2つの製品の価格の表示を管理します。

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

クラスレベルでは、オブザーバーの設定に使用される NSObject も宣言されている必要があり `NSNotificationCenter` ます。

```csharp
NSObject priceObserver;
```

ViewWillAppear メソッドで、既定の通知センターを使用してオブザーバーを作成し、割り当てます。

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

メソッドの最後で `ViewWillAppear` 、メソッドを呼び出して、 `RequestProductData` storekit 要求を開始します。 この要求が完了すると、StoreKit は Apple のサーバーに非同期に接続して情報を取得し、アプリに送り返します。 これは、次の `SKProductsRequestDelegate` セクションで説明するサブクラス () によって実現され `InAppPurchaseManager` ます。

```csharp
iap.RequestProductData(products);
```

価格と説明を表示するコードでは、SKProduct から情報を取得し、それを UIKit コントロールに割り当て `LocalizedTitle` ます (および `LocalizedDescription` – storekit によって、ユーザーのアカウント設定に基づいて正しいテキストと価格が自動的に解決されることに注意してください)。 次のコードは、上記で作成した通知に含まれています。

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

最後に、 `ViewWillDisappear` メソッドでオブザーバーが削除されていることを確認する必要があります。

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>SKProductRequestDelegate (InAppPurchaseManager) メソッド

`RequestProductData`メソッドは、アプリケーションが製品価格やその他の情報を取得する場合に呼び出されます。 製品 Id のコレクションを正しいデータ型に解析し、その情報を使用してを作成し `SKProductsRequest` ます。 Start メソッドを呼び出すと、Apple のサーバーに対してネットワーク要求が行われます。 要求は非同期に実行され、 `ReceivedResponse` 正常に完了したときにデリゲートのメソッドを呼び出します。

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

iOS は、アプリケーションが実行されているプロビジョニングプロファイルに応じて、要求を ' sandbox ' または ' production ' バージョンのアプリストアに自動的にルーティングします。したがって、アプリを開発またはテストするとき、要求は iTunes Connect で構成されたすべての製品 (まだ Apple によって送信または承認されていないものも含む) アプリケーションが運用環境にある場合、StoreKit の要求は、**承認**された製品に関する情報のみを返します。   

`ReceivedResponse`上書きされたメソッドは、Apple のサーバーがデータで応答した後に呼び出されます。 これはバックグラウンドで呼び出されるので、コードは有効なデータを解析し、通知を使用して、その通知の "待機中" の ViewControllers に製品情報を送信する必要があります。 有効な製品情報を収集して通知を送信するコードを次に示します。

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

図には示されていませんが、メソッドをオーバーライドして、 `RequestFailed` App Store サーバーに到達できない場合 (またはその他のエラーが発生した場合) にユーザーにフィードバックを提供できるようにする必要もあります。 このコード例は単にコンソールに書き込むだけですが、実際のアプリケーションでは、プロパティに対してクエリを実行し、 `error.Code` カスタム動作 (ユーザーへの警告など) を実装することができます。

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

このスクリーンショットは、読み込み直後のサンプルアプリケーションを示しています (製品情報が利用できない場合)。

 [![製品情報が使用できないときの読み込み直後のサンプルアプリ](store-kit-overview-and-retreiving-product-information-images/image24.png)](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>無効な製品

は、 `SKProductsRequest` 無効な製品 id の一覧を返す場合もあります。 次のいずれかの理由により、通常、無効な製品が返されます。   

**製品 id が**正しくありません–有効な製品 id のみが受け入れられます。   

 **製品は承認されていません**。テスト中、販売用にクリアされたすべての製品は、によって返される必要があります `SKProductsRequest` 。ただし、運用環境では、承認された製品のみが返されます。   

 **アプリ id が明示的ではありません**-ワイルドカードアプリ id (アスタリスク) は、アプリ内購入を許可しません。   

 **プロビジョニングプロファイルが正しくありません**-プロビジョニングポータルでアプリケーションの構成を変更する場合 (アプリ内購入を有効にする場合など) は、アプリをビルドするときに適切なプロビジョニングプロファイルを再生成して使用することを忘れないでください。   

 **iOS の有料アプリケーション契約が**適用されていない-ストアキットの機能は、Apple 開発者アカウントの有効な契約がない限り、まったく機能しません。   

 **バイナリが拒否状態に**なっています-以前に送信されたバイナリが拒否状態にある場合 (App Store チームまたは開発者によって)、storekit の機能は機能しません。

`ReceivedResponse`サンプルコードのメソッドは、無効な製品をコンソールに出力します。

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>ローカライズされた価格の表示

価格レベルでは、すべての国際対応アプリストアの製品ごとに特定の価格を指定します。 通貨ごとに価格が正しく表示されるようにするには、 `SKProductExtension.cs` それぞれの Price プロパティではなく、次の拡張メソッド (で定義) を使用し `SKProduct` ます。

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

ボタンのタイトルを設定するコードは、次のように拡張メソッドを使用します。

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

2つの異なる iTunes テストアカウント (米国のストア用と日本語ストア用) を使用すると、次のスクリーンショットが表示されます。   

 [![言語固有の結果を示す2つの異なる iTunes テストアカウント](store-kit-overview-and-retreiving-product-information-images/image25.png)](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   

ストアは製品情報と価格通貨に使用される言語に影響しますが、デバイスの言語設定はラベルやその他のローカライズされたコンテンツに影響します。   

別のストアテストアカウントを使用するには、 **iTunes と App store > 設定**で**サインアウト**し、別のアカウントでサインインするためにアプリケーションを再起動する必要があることを思い出してください。 デバイスの言語を変更するには、 **[設定] [全般 > インターナショナル > 言語] > [設定**] に移動します。
