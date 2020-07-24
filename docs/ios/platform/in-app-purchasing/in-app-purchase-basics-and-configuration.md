---
title: Xamarin でのアプリ内購入の基本と構成
description: このドキュメントでは、Xamarin. iOS でのアプリ内購入について説明し、ルール、構成、iTunes Connect に関する関連情報について説明します。
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: fb63568adee9e89d08a0fc64168c865eeb271f10
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928888"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>Xamarin でのアプリ内購入の基本と構成

アプリ内購入を実装するには、アプリケーションがデバイスで StoreKit API を利用する必要があります。 StoreKit は、Apple の iTunes サーバーとのすべての通信を管理して、製品情報を取得し、トランザクションを実行します。 プロビジョニングプロファイルは、アプリ内購入用に構成されている必要があります。また、製品情報を iTunes Connect に入力する必要があります。

 [![このグラフに示すように、StoreKit は Apple とのすべての通信を管理します。](in-app-purchase-basics-and-configuration-images/image1.png)](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

アプリストアを使用してアプリ内購入を提供するには、次の設定と構成が必要です。

- **ITunes Connect** –購入テスト用にサンドボックスユーザーアカウントを設定するための製品を構成します。 また、お客様に代わって収集された資金を送金できるように、Apple に銀行と税金の情報を提供する必要があります。
- **IOS プロビジョニングポータル**–バンドル id を作成し、アプリに対する app Store アクセスを有効にします。
- **ストアキット**–製品を表示し、製品を購入し、トランザクションを復元するためのコードをアプリに追加します。
- **カスタムコード**–顧客によって行われた購入を追跡し、購入した製品またはサービスを提供します。 サーバーからダウンロードされたコンテンツで製品が構成されている場合は、サーバー側のプロセスを実装して確認メッセージを検証することも必要になる場合があります (書籍や雑誌の問題など)。

次の2つのストアキットの "サーバー環境" があります。

- **実稼働**–実際のコストを持つトランザクションです。 Apple によって送信および承認されたアプリケーションでのみアクセスできます。 アプリ内購入製品は、運用環境で使用できるようになる前に確認および承認する必要もあります。
- **サンドボックス**–テストが行われます。 製品は、作成後すぐに使用できます (承認プロセスは運用環境にのみ適用されます)。 サンドボックス内のトランザクションには、トランザクションを実行するためのテストユーザー (実際の Apple Id ではありません) が必要です。

## <a name="in-app-purchase-rules"></a>アプリ内購入のルール

アプリ内のデジタル製品またはサービスに対する他の形式の支払いを受け入れることはできません。また、アプリ内からユーザーを言及したり、ユーザーを参照したりすることはできません。 つまり、アプリ内購入が最適な支払い方法である場合、クレジットカードや PayPal を受け入れることはできません。 アプリの外部でデジタル製品を購入する場合、アプリで使用する特殊なケースがあります。たとえば、特定の "ログイン" に関連付けられている web サイトでブックを購入し、アプリでその "ログイン" を使用すると、ユーザーは購入したブックにアクセスできます。
この方法で動作するアプリケーションは、外部の購入機能への言及やリンクは許可されていません。開発者は、この機能を他の方法でユーザーに通知する必要があります (おそらく、電子メールマーケティングや他の直接チャネルを使用して)。

ただし、物理的な商品に対してアプリ内購入を使用することはできません。その場合は、別の支払い方法を使用できます (例: クレジットカード、PayPal) をアプリ内から。

Apple は、販売を開始する前にすべての製品を承認する必要があります。「製品」の名前、説明、およびスクリーンショットを確認する必要があります。 製品のレビュー時間は、アプリケーションのレビューと同じです。

製品に対して価格を選択することはできません。 Apple がサポートしている国/通貨ごとに特定の値を持つ ' 価格レベル ' のみを選択できます。 市場によって異なる価格レベルを持つことはできません。

## <a name="configuration"></a>構成

アプリ内購入コードを記述する前に、iTunes Connect ( [itunesconnect.apple.com](https://itunesconnect.apple.com)) と IOS プロビジョニングポータル ( [developer.apple.com/iOS](https://developer.apple.com/iOS)) でいくつかのセットアップ作業を行う必要があります。

コードを記述する前に、次の3つの手順を完了する必要があります。

- **Apple の開発者アカウント**–お客様の銀行および課税情報を apple に送信します。
- **IOS プロビジョニングポータル**–アプリに有効なアプリ ID があることを確認します (ワイルドカードではなくアスタリスク * を含む)。アプリの購入が有効になっていることを確認します。
- **ITunes Connect アプリケーション管理**–アプリケーションに製品を追加します。

### <a name="apple-developer-account"></a>Apple 開発者アカウント

無料のアプリを構築して配布するには、 [ITunes Connect](https://itunesconnect.apple.com)の構成がほとんど必要ありません。ただし、有料アプリやアプリ内購入を販売するには、Apple に銀行および課税情報を提供する必要があります。 次に示すメインメニューから、[**契約]、[税金と銀行**] の順にクリックします。

 [![メインメニューの [契約]、[税金と銀行] をクリックします。](in-app-purchase-basics-and-configuration-images/image2.png)](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

このスクリーンショットに示すように、開発者アカウントには**IOS 有料アプリケーション**コントラクトが有効になっている必要があります。

 [![開発者アカウントには、iOS 有料アプリケーションコントラクトが有効になっている必要があります。](in-app-purchase-basics-and-configuration-images/image3.png)](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

**IOS の有料アプリケーション**契約を取得するまで、storekit の機能をテストすることはできません。コード内の storekit の呼び出しは、Apple が**契約、税金、銀行**の情報を処理するまで失敗します。

### <a name="ios-provisioning-portal"></a>iOS プロビジョニング ポータル

新しいアプリケーションは、 **IOS プロビジョニングポータル**の [**アプリ id** ] セクションで設定します。 新しいアプリ ID を作成するには、 [Ios プロビジョニングポータルのメンバーセンター](https://developer.apple.com/membercenter/index.action)に移動し、ポータルの [**証明書]、[識別子]、[プロファイル**] セクションに移動して、[ *Ios アプリ*] の [**識別子**] をクリックします。 次に、右上にある [+] をクリックして、新しいアプリ ID を生成します。

新しい**アプリ id**を作成するためのフォーム

 次のようになります。

 [![新しいアプリ Id を作成するためのフォーム](in-app-purchase-basics-and-configuration-images/image4.png)](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

*説明*に適切なものを入力して、リスト内でこのアプリ ID を簡単に識別できるようにします。 [*アプリ Id プレフィックス*] で、チーム id を選択します。

#### <a name="bundle-identifierapp-id-suffix-format"></a>バンドル識別子/アプリ ID サフィックス形式

**バンドル識別子**には任意の文字列を使用できます (アカウント内で一意である必要があります)。ただし、Apple は、任意の文字列を使用するのではなく、逆引き DNS 形式に従うことをお勧めします。 この記事に付属しているサンプルアプリケーションでは、バンドル識別子に対して com. storekit. テストを使用しますが、my_store_example のような識別子 (Apple では推奨されません) を使用することもできます。

> [!IMPORTANT]
> また、Apple では、1つのアプリ ID を複数のアプリケーションに使用できるように、ワイルドカードのアスタリスクを**バンドル識別子**の末尾に追加することもできます。ただし、_ワイルドカードアプリ Id を apppurchase に使用することはできません_。 たとえば、ワイルドカードバンドル識別子の例としては、「xamarin. *」などがあります。

#### <a name="enabling-app-services"></a>App Services の有効化

**アプリ内購入**は、サービスの一覧で自動的に有効になります。

 [![アプリ内購入は、サービスの一覧で自動的に有効になります](in-app-purchase-basics-and-configuration-images/image5.png)](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>プロビジョニング プロファイル

開発と運用のプロビジョニングプロファイルを通常どおり作成します。アプリ内購入用に設定したアプリ ID を選択します。 詳細については、「 [IOS デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」ガイドを参照してください。

## <a name="itunes-connect"></a>iTunes Connect

[ITunes Connect の**マイアプリ**] をクリックして、iOS アプリケーションエントリを作成または編集します。 アプリケーションの [概要] ページを次に示します。

 [![アプリケーションの [概要] ページ](in-app-purchase-basics-and-configuration-images/image6.png)](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

[**アプリ内購入**] をクリックして、販売する製品を作成または編集します。 このスクリーンショットは、いくつかの製品が既に追加されているサンプルアプリを示しています。

 [![いくつかの製品が既に追加されているサンプルアプリ](in-app-purchase-basics-and-configuration-images/image7.png)](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

新しい製品を追加するプロセスには、次の2つの手順があります。

1. 製品の種類を選択する: [ ![ 製品の種類を選択する](in-app-purchase-basics-and-configuration-images/image8.png)](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2. 製品 Id、価格レベル、およびローカライズされた説明を含む製品の属性を入力します。製品[ ![ 属性の入力](in-app-purchase-basics-and-configuration-images/image9.png)](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

アプリ内購入製品ごとに必要なフィールドを次に示します。

### <a name="reference-name"></a>参照名

参照名はユーザーに表示されません。これは内部使用用であり、iTunes Connect でのみ表示されます。

### <a name="product-id-format"></a>製品 ID の形式

製品識別子に含めることができるのは、英数字 (A-z、a-z、0 ~ 9)、アンダースコア (_)、およびピリオド (.) のみです。 識別子には任意の文字列を使用できますが、Apple では逆引き DNS 形式を使用することをお勧めします。 たとえば、サンプルアプリケーションでは、次のバンドル識別子を使用します。

 `com.xamarin.storekit.testing`

そのため、アプリ内購入製品を識別する規則は次のようになります。

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

この名前付け規則は適用されません。単に製品の管理に役立つ推奨事項です。 さらに、同じ逆引き DNS 規則に従っても、製品識別子はバンドル識別子に*関連付け*られず、同じ文字列で始まる必要はありません。 ただし、photo_product_greyscale のような識別子を使用することもできます (Apple では推奨されません)。

製品 ID はユーザーに表示されませんが、アプリケーションコードで製品を参照するために使用されます。

### <a name="product-type"></a>Product Type (製品の種類)

アプリ内購入製品には、次の5種類が用意されています。

1. 使用**可能–プレーヤー**が使用できるゲーム内通貨など、"使用済み" です。 ユーザーがバックアップ/復元を実行した場合、またはその他の方法でデバイスを更新した場合は、使用できるトランザクションも復元されません (これにより、プレーヤーは実質的に同じ特典を再び有効にすることになります)。 アプリケーションコードでは、トランザクションが完了するとすぐに ' 使用可能な項目 ' を提供する必要があります。
1. **非**利用-デジタル雑誌の問題やゲームレベルなど、ユーザーが購入した製品を所有している製品。
1. **自動更新サブスクリプション**–実際の雑誌サブスクリプションと同様に、サブスクリプション期間の終了時に、Apple は顧客に自動的に料金を請求し、無期限に、または顧客が明示的にキャンセルするまでサブスクリプション期間を延長します。 これは、Newsstand アプリのお支払い方法として推奨されています (実際には、アプリはこの支払い方法をサポートして Newsstand 配布を承認する必要があります)。
1. **無料サブスクリプション**: Newsstand 対応アプリでのみ提供でき、顧客はすべてのデバイスでサブスクリプションコンテンツにアクセスできます。 無料サブスクリプションは期限切れになりません。
1. **非更新サブスクリプション**-1 か月の写真アーカイブへのアクセスなど、静的なコンテンツへの時間制限付きアクセスを販売するために使用する必要があります。

 *このドキュメントでは、現在、最初の2つの製品の種類 (利用できると非消費) のみについて説明します。*

 <a name="Price_Tiers"></a>

### <a name="price-tiers"></a>価格レベル

アプリストアでは、製品に対して任意の価格を選択することはできません。 Apple には、選択できる固定価格レベルが用意されています。 価格は通貨ごとに固定されています。 Apple は、相対的な価格を調整する権利を留保します (たとえば、特定の通貨と米国ドルとの相対的な海外換算レートが変化した後など)。

Apple では、希望する通貨/価格に適したレベルを選択するための価格マトリックスが提供されています。 価格マトリックス (2012 年8月) の抜粋を次に示します。

 [![2012年8月の価格マトリックスの抜粋](in-app-purchase-basics-and-configuration-images/image10.png)](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

書き込み時点 (2013 年6月) では、87のレベルは0.99 から USD 999.99 になります。 価格マトリックスには、お客様が支払う料金、および Apple から受け取る金額が示されています。これは、30% の料金と、収集する必要がある現地の税金です (米国およびカナダ販売者が 99 c 製品の場合は70c を受け取りますが、オーストラリアの販売元は、 &amp; 販売価格で徴収される商品サービス税に起因する

製品の価格は、将来の日付に適用されるスケジュールされた価格変更を含め、いつでも更新できます。 このスクリーンショットは、将来の価格の変更がどのように追加されるかを示しています。価格は、9月の月について、階層1から階層3に一時的に変更されています。

 [![価格は、9月の月について、階層1から階層3に一時的に変更されている将来の価格変更です。](in-app-purchase-basics-and-configuration-images/image11.png)](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>サポートされていない無料製品

Apple は Newsstand アプリ向けに特別な無料サブスクリプションオプションを提供していますが、他のアプリ内購入の種類に対してゼロ (無料) 価格を設定することはできません。 販売促進の価格を編集することはできますが、iTunes Connect を使用してアプリ内購入を行うことはできません。

### <a name="localization"></a>ローカリゼーション

ITunes Connect では、サポートされている任意の数の言語に対して、異なる名前と説明のテキストを入力できます。 ポップアップを使用して、各言語をで追加または編集できます。

 [![ポップアップを使用して、各言語をで追加または編集できます。](in-app-purchase-basics-and-configuration-images/image12.png)](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   

アプリに製品情報を表示すると、ローカライズされたテキストを StoreKit を使用して表示できるようになります。 通貨表示は、正しい記号と10進数の書式を表示するようにローカライズする必要もあります。この書式設定については、ドキュメントの後半で説明します。

### <a name="app-store-review"></a>App Store のレビュー

アプリと同じ–各製品は、販売が開始される前に、Apple によってレビューされます。 製品名または説明に不適切な内容が含まれていると、製品が拒否されることがあります。または、Apple が間違った製品の種類を選択したと判断する可能性があります (例: 本または雑誌の問題が作成されましたが、使用できる製品の種類が使用されています)。 製品レビューは、アプリのレビュー期間中に限り得ることができます。

アプリの購入が有効になっているアプリが初めて送信されたとき (新しいアプリであるか、既存のアプリに機能が追加されているかにかかわらず)、送信する一部の製品も選択する必要があります。 ITunes Connect ポータルでは、次のスクリーンショットに示すように、これを行うように求められます。

 [![ITunes Connect ポータルでも、一部の製品を送信するように求められます。](in-app-purchase-basics-and-configuration-images/image13.png)](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   

アプリケーションとアプリ内購入は一緒にレビューされるので、すべてが一度に承認されるようになります (アプリが承認された製品を使用せずにストアに移動することはありません)。

アプリ内購入機能を使用した最初のバージョンが承認されたら、さらに製品を追加して、いつでもレビュー用に送信することができます。 また、プロンプトに示されているように、[**バージョンの詳細**] ページを使用して、特定のアプリ内購入製品と共に新しいバージョンを送信することもできます。

詳細については、 [App Store のレビューに関するガイドライン](https://developer.apple.com/appstore/guidelines.html)を参照してください。

 [パート 2-ストアキットの概要と製品情報の取得](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
