---
title: アプリ内購入の基本と Xamarin.iOS での構成
description: このドキュメントでは、ルール、構成、および iTunes Connect に関する関連情報について説明する Xamarin.iOS でのアプリ内購入について説明します。
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 267dac5b6aec263f1d8b69d81f34f732118c1802
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61406894"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>アプリ内購入の基本と Xamarin.iOS での構成

アプリ内購入を実装するには、デバイスで storekit や API を利用するアプリケーションが必要です。 Storekit や Apple の iTunes のサーバー製品の情報を取得し、トランザクションを実行するとすべての通信を管理します。 アプリ内購入のプロビジョニング プロファイルを構成する必要があり、iTunes Connect で製品情報を入力する必要があります。

 [![](in-app-purchase-basics-and-configuration-images/image1.png "Storekit や Apple のすべての通信はこのグラフに示すように、管理します。")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

App Store を使用して、アプリ内購入を提供するには、次のセットアップと構成が必要です。

-  **iTunes Connect** : 販売する製品を構成し、購入をテストするサンド ボックスのユーザー アカウントをセットアップします。 必要がありますも提供されている、銀行と税の情報を Apple にので自動的に収集された資金を送金します。
-   **iOS プロビジョニング ポータル**– バンドル Id を作成して、アプリの App Store へのアクセスを有効にします。
-  **ストア キット**– 製品を表示、製品の購入、およびトランザクションを復元するため、アプリにコードを追加します。
-  **カスタム コード**– を顧客による購入を追跡し、製品や購入したサービスを提供します。 お使いの製品は、書籍や雑誌) などのサーバーからダウンロードしたコンテンツで構成されている場合、配信確認メッセージを検証するサーバー側プロセスを実装する必要がありますもことがあります。


2 つストア キット「サーバーの環境」があります。

-  **実稼働**– 実際のコストとトランザクション。 送信および Apple によって承認されたアプリケーションを使用してのみアクセスできます。 アプリ内購入製品のレビューし、で、運用環境で使用する前に承認もする必要があります。
-  **サンド ボックス**-発生テスト場所。 製品は、ここでする (承認プロセスは運用環境にのみ適用されます) の作成後すぐに使用できます。 サンド ボックス内のトランザクションでは、テスト ユーザー (Apple Id を実際のない) トランザクションを実行する必要があります。

## <a name="in-app-purchase-rules"></a>アプリ内購入の規則

アプリ内で他の形式のデジタル製品やサービスの支払いをそのまま使用および伝えしたり、アプリ内からユーザーを参照してくださいことはできません。 つまり、最も適切な支払いメカニズムは、アプリ内購入時にクレジット_カードや PayPal を受け入れることはできません。 アプリの外部のデジタル製品の購入、特殊なケースがありますの使用など、特定の"login"に関連付けられている web サイトにブックを購入し、アプリでは、"login"ユーザー アクセスできるようにすることを使用して、アプリで購入したブックの「します。
このように動作するアプリケーションは言うまでもまたは外部の購入機能へのリンクは許可されていません: 開発者が (おそらく電子メール マーケティングまたはその他のいくつかの直接チャネル) を介して他の方法で、ユーザーには、この機能を通信する必要があります。

ただし、使用できないためアプリ内購入の物理的な商品は、許可されている場合 (例: 代替の支払いメカニズムを使用することで クレジット_カードや paypal など) から、アプリ内で。

Apple は、上と販売 – 名前、説明になったし、'product' のスクリーン ショットは、確認のために必要な前に、すべての製品を承認する必要があります。 製品のレビュー時間は、アプリケーションのレビューと同じです。

製品の任意の価格を選択することはできません: Apple をサポートするそれぞれの国/通貨で特定の値を持つ '価格レベル' を選択することがあります。 さまざまな市場の価格レベルが異なることはできません。

## <a name="configuration"></a>構成

アプリ内購入コードを記述する前に、iTunes Connect では、いくつかセットアップ作業を行う必要があります ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) および iOS プロビジョニング ポータル ( [developer.apple.com/iOS](https://developer.apple.com/iOS))。

これら 3 つの手順は、コードを記述する前に完了する必要があります。

-  **Apple 開発者アカウント**– Apple に銀行業務および課税情報を送信します。
-  **iOS プロビジョニング ポータル**– アプリが有効なアプリ ID を確認します (ワイルドカードにアスタリスクが付いていない * が) がアプリの購入で有効になります。
-  **iTunes Connect アプリケーション管理**– 製品、アプリケーションを追加します。


### <a name="apple-developer-account"></a>Apple 開発者アカウント

無料アプリの配布のビルドとでほとんどの構成が必要です[iTunes Connect](https://itunesconnect.apple.com)、有料を販売するアプリまたはアプリ内購入する必要があります銀行業務および課税情報を Apple に提供します。 をクリックして**契約、税金、銀行**ここで示すように、メイン メニューから。

 [![](in-app-purchase-basics-and-configuration-images/image2.png "契約、税金、銀行のメイン メニューからをクリックします。")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

開発者アカウントが必要、 **iOS 有料アプリケーション**実際には、このスクリーン ショットで示すようにコントラクトします。

 [![](in-app-purchase-basics-and-configuration-images/image3.png "有料アプリケーション コントラクトの有効な iOS を開発者アカウントが必要")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

なるまで、storekit や機能をテストすることはできません、 **iOS 有料アプリケーション**コントラクト – storekit や呼び出しコードでは失敗 Apple が処理されるまで、**契約、税金、および銀行**情報。

### <a name="ios-provisioning-portal"></a>iOS プロビジョニング ポータル

新しいアプリケーションが設定されて、**アプリ Id**のセクション、 **iOS Provisioning Portal**します。 新しいアプリ ID を作成するには、 [iOS プロビジョニング ポータルの Member Center](https://developer.apple.com/membercenter/index.action)に移動します**証明書、識別子、およびプロファイル**クリックして、ポータルのセクション**識別子**  *iOS アプリ*します。 新しいアプリ ID を生成する権利を上にある「+」をクリックし、


新規作成フォーム**アプリ Id**

 次に示します。

 [![](in-app-purchase-basics-and-configuration-images/image4.png "新しいアプリ Id を作成するためのフォーム")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

適切なものを入力、*説明*リスト内のこのアプリ ID を簡単に識別できるようにします。 *アプリ ID プレフィックス*のチーム ID を選択します。

#### <a name="bundle-identifierapp-id-suffix-format"></a>バンドル識別子/アプリ ID のサフィックスの形式

ような任意の文字列を使用することができます、**バンドル識別子**である限り、アカウント内で一意である)、逆引き DNS 形式ではなくする任意の文字列を使用して、Apple がお勧めしますが、します。 この記事に付属するサンプル アプリケーションは、(Apple では推奨していない) 場合でも、my_store_example のような識別子を使用するも同じく有効なりますが、バンドル Id の com.xamarin.storekit.testing を使用します。

> [!IMPORTANT]
> Apple ことができますの末尾に追加するワイルドカードのアスタリスクを**バンドル識別子**1 つのアプリ ID をただし、複数のアプリケーションを使用できるように_AppPurchaseのワイルドカードアプリIdを使用することはできません_. ワイルド カード バンドル識別子が com.xamarin.* あります例

#### <a name="enabling-app-services"></a>App Services を有効にします。

なお**アプリ内購入**サービスの一覧に自動的に有効にします。

 [![](in-app-purchase-basics-and-configuration-images/image5.png "サービスの一覧で、アプリ内購入を自動的に有効になります")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>プロビジョニング プロファイル

アプリ内購入を設定したアプリ ID 選択は、通常とは、開発と実稼働プロビジョニング プロファイルを作成します。 参照してください、 [iOS デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[App Store に公開](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)詳細情報をガイドします。

## <a name="itunes-connect"></a>iTunes Connect

をクリックして**マイ アプリ**で iTunes Connect iOS アプリケーションのエントリを作成または更新します。 アプリケーションの概要 ページを次に示します。

 [![](in-app-purchase-basics-and-configuration-images/image6.png "アプリケーションの概要 ページ")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

クリックして**アプリ内購入**を作成または販売、製品を編集します。 このスクリーン ショットでは、既にいくつかの製品とサンプル アプリを示しています。

 [![](in-app-purchase-basics-and-configuration-images/image7.png "既にいくつかの製品を使ってサンプル アプリ")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

新しい製品を追加するプロセスでは、2 つの手順があります。

1.   製品の種類を選択します。[![](in-app-purchase-basics-and-configuration-images/image8.png "製品の種類を選択します。")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   製品の属性を製品 Id を含む、価格レベルとローカライズされた説明を入力します。[![](in-app-purchase-basics-and-configuration-images/image9.png "製品の属性を入力")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

各アプリ内購入製品に必要なフィールドを次に示します。


### <a name="reference-name"></a>参照名

参照名は、ユーザーに表示されません。内部使用し、iTunes Connect でのみ表示されます。

### <a name="product-id-format"></a>製品 ID の形式

製品識別子は、英数字 (A ~ Z、a ~ z、0-9) を含めることができますのみアンダー スコア (_)、およびピリオド (.) 文字。 任意の文字列は、識別子を使用できますが、Apple は、逆引き DNS 形式をお勧めします。 たとえば、サンプル アプリケーションは、このバンドル識別子を使用します。

 `com.xamarin.storekit.testing`

そのため、アプリ内購入製品を識別するために、規則であるよう。

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

この名前付け規則は適用されませんが、製品の管理に役立つ推奨事項だけです。 さらに、同じの逆引き DNS の規則に従うに関係なく、製品の Id は*関連しない*バンドル識別子とは必要ありません、同じ文字列で開始します。 (Apple では推奨していない) 場合でも、photo_product_greyscale などの識別子を使用する有効なままできます。

製品 ID は、ユーザーに表示されませんが、アプリケーション コードで製品を参照に使用されます。

### <a name="product-type"></a>製品の種類

アプリ内購入製品を提供することができますの 5 つの種類があります。

1.  **消耗**– べき点が 'を '、使用、プレーヤーが費やすことができます、ゲーム内の通貨など。 ユーザーはバックアップ/復元、またはそれ以外の場合、デバイスが更新、使用できるトランザクションは取得は復元されませんも (となる効果的にプレーヤー何度も同じ特典)。 アプリケーション コードは、トランザクションが完了するとすぐに、'消耗品' を提供することを確認する必要があります。
1.  **非コンシューマブル**– デジタル マガジンの問題やゲーム レベルなど、1 回購入製品 'ユーザー 'が所有します。
1.  **自動更新可能なサブスクリプション**だけ雑誌の講読、現実世界のように、サブスクリプション期間の最後に Apple に自動的にもう一度顧客に請求し、サブスクリプション期間を明示的に状態のままか、顧客まで拡張これをキャンセルします。 Newsstand のアプリに支払い方法では (実際には、サポートする Newsstand 配布用に承認するには、この支払い方法)。
1.  **無料サブスクリプション**– Newsstand 対応のアプリのみが提供することができ、すべてのデバイスにより、お客様はサブスクリプション コンテンツにアクセスします。 無料のサブスクリプションの有効期限はありません。
1.  **サブスクリプションの非更新**– 写真アーカイブへの 1 か月のアクセスなどの静的コンテンツに時間制限アクセスを販売するために使用する必要があります。


 *現在、このドキュメントには、(使用できると非コンシューマブル) のみ最初 2 つの製品タイプがについて説明します。*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>価格レベル

App Store では、製品の任意の価格を選択することはできません-Apple から選択できる固定価格レベルを提供します。 価格は、通貨ごとに固定し、Apple は (たとえば、特定の通貨と米ドルの間の相対外国換算レートの持続的な変更) 後に相対の価格を調整する権利を留保します。

Apple では、する通貨/価格の適切なレベルを選択するための価格のマトリックスを提供します。 価格のマトリックス (2012 年 8 月) の抜粋を次に示します。

 [![](in-app-purchase-basics-and-configuration-images/image10.png "2012 年 8 月の価格のマトリックスの抜粋")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

(2013 年 6 月) の書き込み時に、米国ドルから 87 階層 USD 999.99 0.99 です。 価格のマトリックスには、価格が表示されます、お客様がお支払いとも Apple – から届きます量これは、以下の 30% の料金でありことも、現地の税金 (例では米国とカナダの販売者が 99 c p の 70 c を受信する通知を収集するために必要製品、オーストラリアの販売者により 63 c のみを受信中に '商品&amp;サービス税' 販売価格を徴収)。

製品の価格は、将来の日付で有効にするためのスケジュールされた料金の変更を含め、いつでも更新できます。 このスクリーン ショットは、将来の付の価格の変更を追加する方法を示しています: 価格が一時的に変更する階層 1 から階層 3 に 9 月のみを示します。

 [![](in-app-purchase-basics-and-configuration-images/image11.png "場所、価格一時的に変更する階層 1 から階層 3 に 9 月のみの将来の付価格変更")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>無料の製品がサポートされていません

Apple には、Newsstand アプリ用の特別な無料のサブスクリプション オプションが用意されていますが、他の種類のアプリ内購入のゼロ (無料) 価格を設定することはできません。 編集することができます (つまり。 下位) 販売プロモーションの価格は、iTunes Connect を使用して"無償版"アプリ内購入を行うことはできません。

### <a name="localization"></a>ローカリゼーション

ITunes Connect では、任意の数のサポートされている言語の別の名前と説明テキストを入力できます。 各言語は、追加/編集で、ポップアップを使用して指定できます。

 [![](in-app-purchase-basics-and-configuration-images/image12.png "各言語を追加/編集できますでポップアップを使用して")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 アプリで製品情報を表示する場合は、StoreKit を使用して表示するためのローカライズされたテキストがあります。 通貨の表示は、この書式設定は、ドキュメントの後半で説明、正しいシンボルと – 10 進数の書式設定を表示するもローカライズする必要があります。

### <a name="app-store-review"></a>App Store のレビュー

アプリと同じ各製品をレビューする Apple によって販売上を移動する許可。 名前または説明、不適切なコンテンツの製品を拒否する可能性があります。 または Apple が正しくない製品の種類を (例: 選択した場合があります。 した書籍や雑誌の問題の作成が消耗製品の種類を使用)。 製品のレビューには、アプリのレビュー程度かかることができます。

初めてのアプリ内購入 (新しいアプリ、または既存のサブスクリプションへの機能が追加されました) かどうかを有効になっていると、アプリが送信される一部の製品を送信するも選択する必要があります。 ITunes Connect ポータルには、このスクリーン ショットに示すように、これを行うを求められます。

 [![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect ポータルには、一部の製品もを送信するように求められます")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 アプリケーションとアプリ内購入いただきます、(したがって、そのアプリに移動しないストア承認済み製品なしに!) を一度に承認すべてようにします。

アプリ内購入機能により、最初のバージョンが承認されると、さらに製品を追加し、それらをいつでも確認用に送信できます。 選択することできますも、特定のアプリ内購入製品と、新しいバージョンを送信するを使用して、**バージョン詳細**が示すように、プロンプトのページします。

参照してください、 [App Store レビューに関するガイドライン](https://developer.apple.com/appstore/guidelines.html)詳細についてはします。

 [パート 2 - ストア キットの概要と製品情報の取得](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
