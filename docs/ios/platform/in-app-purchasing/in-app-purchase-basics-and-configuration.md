---
title: アプリ内購入の基本と Xamarin.iOS 内の構成
description: このドキュメントでは、ルール、構成、および iTunes Connect に関する関連情報について説明する Xamarin.iOS でのアプリ内購入について説明します。
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9ded160ad4b31346c400e63d739a3dc21f6304d3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787244"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>アプリ内購入の基本と Xamarin.iOS 内の構成

アプリ内購入を実装するには、デバイスで StoreKit API を利用するアプリケーションが必要です。 StoreKit では、Apple の iTunes サーバー製品の情報を取得し、トランザクションを実行するとすべての通信を管理します。 アプリ内購入するため、プロビジョニング プロファイルを構成する必要があり、iTunes Connect では、製品情報を入力する必要があります。

 [![](in-app-purchase-basics-and-configuration-images/image1.png "この表に示すように、StoreKit と Apple のすべての通信を管理します。")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

アプリ ストアを使用して、アプリ内購入を提供するには、次のセットアップと構成が必要です。

-  **iTunes Connect** – 販売するために製品を構成し、購入をテストするサンド ボックスのユーザー アカウントをセットアップします。 必要がありますも情報を指定、銀行および税を Apple に資金を自動的に収集されたを依頼するようにします。
-   **iOS プロビジョニング ポータル**– バンドル Id を作成し、アプリのアプリ ストアへのアクセスを有効にします。
-  **キットを格納**– 製品を表示する、製品の購入、およびトランザクションを復元するため、アプリにコードを追加します。
-  **カスタム コード**– を顧客による購入を追跡し、製品や購入したサービスを提供します。 実装することがも必要があります (書籍や雑誌) などのサーバーからダウンロードしたコンテンツの場合は、製品、配信確認メッセージを検証するサーバー側のプロセスです。


2 つストア キット「サーバーの環境」があります。

-  **実稼働**– 実際のコストとトランザクションです。 送信され、Apple によって承認されているアプリケーションからのみアクセスできます。 アプリ内購入製品のレビューし、は、実稼働環境で使用する前に承認もする必要があります。
-  **サンド ボックス**: テストの動作をします。 製品は、次にする (承認プロセスは、実稼働環境にのみ適用されます) の作成後すぐに使用できます。 サンド ボックス内のトランザクションでは、トランザクションを実行するテスト ユーザー (いない実際の Apple Id) が必要です。

## <a name="in-app-purchase-rules"></a>アプリ内購入ルール

アプリ内でデジタル製品やサービスの支払い額の他のフォームを受け入れ、言及もやアプリ内から、ユーザーを参照してくださいを行うことはできません。 つまり、最も適切な支払メカニズムは、アプリ内購入時にクレジット_カードや PayPal に同意することはできません。 アプリ外部デジタルの製品を購入するための特殊なケースがあるの使用など、特定の"login"に関連付けられている web サイトにブックを購入して、アプリでは、"login"ににより、ユーザーのアクセスを使用して、アプリ内購入済みのブック。
このように動作するアプリケーションが言及または外部の購入のフィーチャーにリンクすることはできません: 開発者が (おそらく電子メール マーケティングまたは) 経由で直接その他のいくつかのチャネルには、他の方法でユーザーにこの機能を通信する必要があります。

ただし、使用することはできませんのでアプリ内購入物理の商品が許可されている場合、代替の支払メカニズム (を使用します。 クレジット_カードや PayPal) から、アプリ内で。

販売 – 名、説明に移動し、'product' のスクリーン ショットは、レビューに必要な前に、Apple はすべての製品を承認する必要があります。 製品のレビュー時間は、アプリケーションのレビューと同じです。

製品の任意の価格を選択することはできません: 特定の値を持つ通貨ごとに国/Apple をサポートする '価格レベル' のみを選択した可能性があります。 別の市場で別の価格レベルを持つことはできません。

## <a name="configuration"></a>構成

アプリ内購入コードを記述する前に iTunes Connect では、いくつかのセットアップ作業を行う必要があります ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) および iOS プロビジョニング ポータル ( [developer.apple.com/iOS](http://developer.apple.com/iOS))。

これら 3 つの手順は、任意のコードを記述する前に完了する必要があります。

-  **Apple の開発者アカウント**– apple 銀行および課税情報を送信します。
-  **iOS プロビジョニング ポータル**–、アプリが有効なアプリ ID を確認してください (アスタリスク ワイルドカードされません * 内) が有効になったアプリを購入にします。
-  **iTunes Connect アプリケーション管理**– 製品をアプリケーションに追加します。


### <a name="apple-developer-account"></a>Apple 開発者アカウント

ビルドと無料のアプリを配布するのにはほとんど構成は必要[iTunes Connect](https://itunesconnect.apple.com)、有料を販売するアプリまたはアプリ内購入する必要があります銀行および課税の情報を Apple に提供します。 をクリックして**契約、税およびバンキング**ここで示すように、メイン メニューから。

 [![](in-app-purchase-basics-and-configuration-images/image2.png "アグリーメント、税やオンライン バンキングのメイン メニューからをクリックします。")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

開発者アカウントである必要があります、 **iOS 有料アプリケーション**実際には、このスクリーン ショットに示すようにコントラクトします。

 [![](in-app-purchase-basics-and-configuration-images/image3.png "IOS アプリケーションの支払いが有効でコントラクトを開発者アカウントが必要")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

なるまで、StoreKit 機能をテストすることはできません、 **iOS 有料アプリケーション**コントラクト – StoreKit 呼び出し、コードでは失敗 Apple が処理されるまで、**コントラクト、税、およびバンキング**情報です。

### <a name="ios-provisioning-portal"></a>iOS プロビジョニング ポータル

新しいアプリケーション設定されて、**のアプリ Id**のセクションで、 **iOS プロビジョニング ポータル**です。 新しいアプリ ID を作成するには、 [iOS プロビジョニング ポータルの Member Center](https://developer.apple.com/membercenter/index.action)に移動**証明書、識別子、およびプロファイル**セクションをクリックして、ポータルの**識別子**  *iOS アプリの*します。 新しいアプリ ID を生成する、「+」上の右側をクリックします。


新規作成フォーム**のアプリ Id**

 次のようになります。

 [![](in-app-purchase-basics-and-configuration-images/image4.png "新しいアプリ Id を作成するためのフォーム")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

適切な名前を入力してください、*説明*リスト内のこのアプリ ID を簡単に識別できるようにします。 *アプリ ID のプレフィックス*、選択、チームの ID

#### <a name="bundle-identifierapp-id-suffix-format"></a>バンドルの識別子/アプリ ID のサフィックスの形式

使用する任意の文字列を使用することができます、**バンドル Id** (限り、アカウント内で一意である)、Apple では、逆引き DNS 形式に従うのではなくする任意の文字列を使用することをお勧めします。 この記事に付属するサンプル アプリケーションは、(Apple では推奨していない) 場合でも、my_store_example のような識別子を使用する有効な均等になりますが、バンドル Id の com.xamarin.storekit.testing を使用します。

> [!IMPORTANT]
> Apple では、ワイルドカードのアスタリスクの末尾に追加することもできます、**バンドル Id** 1 つのアプリ ID を複数のアプリケーションが使用できるようにする_AppPurchaseワイルドカードのアプリIdは使用できません_. ワイルドカードのバンドル Id が com.xamarin.* あります例

#### <a name="enabling-app-services"></a>アプリ サービスを有効にします。

なお**アプリ内購入**サービスの一覧で自動的に有効にします。

 [![](in-app-purchase-basics-and-configuration-images/image5.png "サービスの一覧でアプリ内購入は自動的に有効になって")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>プロビジョニング プロファイル

通常を選択して、アプリ内購入を設定したアプリ ID と、開発と実稼働のプロビジョニング プロファイルを作成します。 参照してください、 [iOS デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[アプリ ストアに公開](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)詳細情報をガイドします。

## <a name="itunes-connect"></a>iTunes Connect

をクリックして**My Apps** iTunes iOS アプリケーションのエントリを作成またはへの接続にします。 アプリケーションの概要 ページを次に示します。

 [![](in-app-purchase-basics-and-configuration-images/image6.png "アプリケーションの概要 ページ")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

をクリックして**アプリ内購入**を作成または販売製品を編集します。 このスクリーン ショットは、既にいくつかの製品とサンプル アプリを示しています。

 [![](in-app-purchase-basics-and-configuration-images/image7.png "既にいくつかの製品とサンプル アプリ")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

新しい製品を追加するプロセスには、2 つの手順があります。

1.   製品の種類を選択します[![](in-app-purchase-basics-and-configuration-images/image8.png "製品の種類の選択。")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   製品の属性、製品 Id を含む、価格レベルとローカライズされた説明を入力: [![](in-app-purchase-basics-and-configuration-images/image9.png "製品属性を入力します。")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

アプリ内購入製品ごとに必要なフィールドを以下に示します。


### <a name="reference-name"></a>参照名

参照名は、ユーザーに表示されません。内部使用して、iTunes Connect でのみ表示されます。

### <a name="product-id-format"></a>製品 ID の形式

製品識別子は、英数字 (A ~ Z、a ~ z、0 ~ 9) を含めることができますのみアンダー スコア (_)、およびピリオド (.) 文字です。 任意の文字列を使用するには、識別子の Apple 逆引き DNS 形式お勧めします。 たとえば、サンプル アプリケーションは、このバンドル Id を使用します。

 `com.xamarin.storekit.testing`

したがって、規則アプリ内購入製品を識別するようになりますとおり

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

この名前付け規則は適用されません、製品を管理するために推奨するだけです。 さらに、同一の逆引き DNS 規則に従うと、製品の Id は*関係のない*へは、バンドル Id しなければならないのと同じ文字列で開始します。 (Apple では推奨していない) 場合でも、photo_product_greyscale などの識別子を使用する有効なままできます。

製品 ID は、ユーザーに表示されませんが、アプリケーション コードで製品を参照に使用されます。

### <a name="product-type"></a>製品の種類

アプリ内購入製品が提供することができますの 5 つの種類があります。

1.  **消耗**– いるかもしれない 'を '、使用などゲームの通貨に換算するプレーヤーを費やすことができます。 場合は、ユーザーがバックアップ/復元を実行するか、または自分のデバイスが更新それ以外の場合、利用できるトランザクションは取得は復元されませんも (となる効果的にプレーヤーで再び同じ特典)。 アプリケーション コードでは、トランザクションが完了するとすぐには、'消耗 item' を提供する必要があります。
1.  **非が使用できる**– デジタル雑誌問題やゲームのレベルなど、1 回購入ユーザー 'が所有する' 製品です。
1.  **自動更新可能なサブスクリプション**– だけ実際の雑誌の講読と同様に、サブスクリプション期間の最後に Apple 自動的に顧客にもう一度請求し、サブスクリプション期間を明示的に無限、または顧客まで拡張これをキャンセルします。 これは、Newsstand アプリの優先支払方法 (実際には、サポート Newsstand 配布用に承認するには、この支払方法)。
1.  **サブスクリプションの無料**– Newsstand 対応アプリでのみ提供されていることができ、により、すべてのデバイスで、お客様はサブスクリプション コンテンツにアクセスします。 無料サブスクリプションの有効期限はありません。
1.  **サブスクリプションの非更新**– フォト アーカイブへの 1 か月間のアクセスなどの静的なコンテンツに時間制限付きアクセスを販売するために使用する必要があります。


 *現在、このドキュメントには、最初 2 つの製品タイプのみ (使用できると以外が使用できる) がについて説明します。*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>価格レベル

アプリ ストアでは、製品の任意の価格を選択することはできません-Apple から選択できる固定価格レベルを提供します。 価格は通貨ごとに固定し、Apple は (たとえば、特定の通貨と米国ドル間の相対外国換算レートの持続的な変更) 後に相対価格を調整する権利を留保します。

Apple では、簡単にする通貨/価格の適切な層を選択できる価格マトリックスを提供します。 価格マトリックス (2012 年 8 月) の抜粋を次に示します。

 [![](in-app-purchase-basics-and-configuration-images/image10.png "価格マトリックス 2012 年 8 月の抜粋")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

(2013 年 6 月) の書き込み時に、USD から 87 階層 USD 999.99 0.99 です。 料金のマトリックスで表示価格される、顧客が得られますとも量 Apple – を受け取ることはこれは、30% 充電以下およびもローカル税金が必要な (例では米国とカナダ sellers が 99 c p 70 c を受信する通知を収集するにはroduct、オーストラリア sellers 63 c だけの理由のための受信中に '商品&amp;Services 税' 徴収販売価格で)。

製品の価格は、将来の日付で有効にするスケジュールの価格変更も含め、いつでも更新できます。 このスクリーン ショットは、将来日の価格の変更を追加する方法を示しています。 – 価格は一時的にから変更されている第 1 層階層 3 を 9 月ののみ。

 [![](in-app-purchase-basics-and-configuration-images/image11.png "ここで価格が一時的にから変更されている第 1 層第 3 層を 9 月ののみ将来日の価格の変更")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>無料の製品がサポートされていません

Apple Newsstand アプリ用の特別な無料のサブスクリプション オプションが用意されていますが、その他のアプリ内購入型 0 (無料) 価格を設定することはできません。 編集できますが、(ie。 下限) 販売プロモーションの価格、'無料' iTunes Connect 経由でのアプリ内購入を行うことができません。

### <a name="localization"></a>ローカリゼーション

ITunes Connect では、任意の数のサポートされている言語の別の名前と説明テキストを入力できます。 各言語を追加/編集、ポップアップを使用して指定できます。

 [![](in-app-purchase-basics-and-configuration-images/image12.png "各言語が追加/編集、ポップアップを使用して指定できます。")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 アプリの製品情報を表示する場合は、StoreKit を使用して表示するためのローカライズされたテキストがあります。 通貨の表示は、この書式設定については、ドキュメントの後半で説明、適正なシンボルと – 10 進数の書式設定を表示するもローカライズする必要があります。

### <a name="app-store-review"></a>App Store のレビュー

アプリ – と同じ各製品をレビューする apple 販売上を移動する許可。 名前または説明、内の不適切なコンテンツの製品が拒否される可能性がまたは Apple が間違った製品の種類を (選択したことを決定することがあります。 した作成書籍や雑誌の問題が利用できる製品の種類を使用)。 製品のレビューには、アプリのレビュー程度かかることができます。

アプリ内購入 (はアプリを新規または既存の機能が追加されました) かどうかを有効になっているアプリが送信された最初の時刻は、また、と共に送信するためのいくつかの製品を選択する必要があります。 ITunes Connect ポータルには、このスクリーン ショットに示すようにするを求められます。

 [![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect ポータルには、いくつかの製品もを送信するように求められます")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 アプリケーションとアプリ内購入が確認、まとめて、それらはすべての承認を一度に (ため、そのアプリは承認されている製品せず、ストアに!) できるようにします。

アプリ内購入機能により、最初のバージョンを承認すると、さらに製品を追加し、送信するにはいつでも確認できます。 できますを特定のアプリ内購入製品と共に新しいバージョンの送信を使用して、**バージョン詳細**が示すように、プロンプトのページです。

参照してください、 [App Store レビューに関するガイドライン](https://developer.apple.com/appstore/guidelines.html)詳細についてはします。

 [パート 2 - ストア キットの概要と取得の製品情報](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
