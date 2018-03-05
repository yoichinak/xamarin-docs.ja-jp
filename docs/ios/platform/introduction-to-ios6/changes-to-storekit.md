---
title: "StoreKit への変更"
description: "iOS 6 のストア キット API に 2 つの変更が導入されています: iTunes (と App Store/iBookstore) を表示する機能、アプリと、新規のアプリ内から製品は、Apple が、ダウンロード可能なファイルをホストするオプションを購入します。 このドキュメントでは、Xamarin.iOS とそれらの機能を実装する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: cbaa389e4a115be2face2b72db6108c836676dc7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-storekit"></a>StoreKit への変更

_iOS 6 のストア キット API に 2 つの変更が導入されています: iTunes (と App Store/iBookstore) を表示する機能、アプリと、新規のアプリ内から製品は、Apple が、ダウンロード可能なファイルをホストするオプションを購入します。このドキュメントでは、Xamarin.iOS とそれらの機能を実装する方法について説明します。_

IOS6 ストア キットの主な変更は、これら 2 つの新機能です。

-   **アプリ内のコンテンツの表示および購入**: ユーザーを購入でき、アプリを離れることがなくコンテンツのダウンロードのアプリ、音楽、ブックおよびその他の iTunes です。 購入を昇格または同様のレビューと評価をお勧めする独自のアプリをリンクすることもできます。 
-   **アプリ内購入ホストされているコンテンツ**– Apple が格納され、コンテンツ提供、製品のアプリ内購入に関連付けられている必要がある、ファイルをホストする別のサーバー、自動的にバック グラウンドでダウンロードをサポートしすることができます以下のコードを記述します。 


このドキュメントは、既存の Xamarin.iOS と共にで読み取ることをお勧め[アプリ内購入](~/ios/platform/in-app-purchasing/index.md)ドキュメント。

## <a name="requirements"></a>必要条件

このドキュメントで説明するストア キット機能は、iOS 6 および Xamarin.iOS 6.0 と共に、Xcode 4.5 が必要です。


## <a name="in-app-content-display--purchasing"></a>アプリ内のコンテンツの表示と購入

アプリ内購入の新機能で iOS では、製品情報を表示し、購入またはアプリ内から製品をダウンロードすることができます。
以前のアプリケーションは、iTunes、App Store、または、ユーザーが元のアプリケーションのままになります iBookstore を起動する必要があります。 この新しい機能は、操作が完了自動的にユーザーとアプリに返します。

 [ ![](changes-to-storekit-images/image1.png "購入後、アプリに自動的に返す")](changes-to-storekit-images/image1.png)

さまざまな場所があります、シナリオがあります (これらに限定されません) が含まれます。

-   **ユーザーにアプリを評価する推奨**– ユーザーが評価してそれを離れることがなく、アプリをレビューできるように、アプリ ストアのページを開くことができます。 
-   **アプリの間の昇格**– 購入/ダウンロードをすぐにする機能を公開する他のアプリの表示をユーザーに許可します。 
-   **ユーザーを支援を検索してコンテンツをダウンロード**– ユーザーがアプリを検索、管理または (を集計するコンテンツを購入できます。 音楽に関連するアプリでした曲の再生リストを示し、アプリ内から購入するには、各曲を許可する) します。 


1 回、`SKStoreProductViewController`が表示された iTunes、App Store、または、iBookstore にいるかのように、ユーザーが製品情報と対話できます。 バインディングには、以下の項目が含まれます。

-  ()、アプリのスクリーン ショットを表示します。
-  サンプリングの曲またはビデオ (音楽、テレビ番組や映画)
-  読み取り (および書き込み) は、次のレビュー、
-  購入 (&) をダウンロードする、動作は、ビュー コント ローラーとストア キット内で完全です。 アプリケーションは、これを行うためのコードを提供する必要はありません。 


内で一部のオプションはことに注意してください、`SKStoreProductViewController`などをクリックすると、関連するストア アプリを開き、アプリのままにするユーザーも強制は**関連製品**またはアプリの**サポート**リンクします。

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

すべてのアプリ内製品を表示する API は、非常に単純な: を作成し、表示のみが必要です、`SKStoreProductViewController`です。 次の手順を作成し、製品を表示します。

1.  作成、`StoreProductParameters`ビュー コント ローラーにパラメーターを渡すオブジェクトを含む、`productId`コンス トラクターでします。 
1.  インスタンスを作成、`SKProductViewController`です。 クラス レベルのフィールドに割り当てます。 
1.  ビュー コント ローラーにハンドラーを割り当てる`Finished`イベントで、ビュー コント ローラーを消去する必要があります。 ユーザーがキャンセル をクリックするときにこのイベントが呼び出されますまたは、それ以外の場合、ビューのコント ローラー内のトランザクションを終了します。 
1.  呼び出す、`LoadProduct`メソッドに渡して、`StoreProductParameters`し、完了ハンドラー。 製品要求が正常にしたことをチェックして存在する場合は、完了ハンドラー、`SKProductViewController`モーダルでします。 製品を取得できない場合に、適切なエラー処理を追加する必要があります。 

### <a name="example"></a>例

*ProductView*プロジェクトで、 *StoreKit*この記事のサンプル コードを実装して、`Buy`を任意の製品を受け取るメソッドの Apple ID と表示、`SKStoreProductViewController`です。 次のコードには、任意指定の Apple ID の製品情報が表示されます。

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

実行する – ダウンロードまたは購入内に完全が発生したときに次のように、アプリの外観、 `SKStoreProductViewController`:

 [ ![](changes-to-storekit-images/image2.png "実行時に次のように、アプリを外観します。")](changes-to-storekit-images/image2.png)

### <a name="supporting-older-operating-systems"></a>以前のオペレーティング システムをサポートします。

サンプル アプリケーションには、以前のバージョンの iOS のアプリ ストア、iTunes または、iBookstore を開く方法を示すコードが含まれています。 使用して、`OpenUrl`を開き、正常に作成された**itunes.com** URL。

実行するには次のようにコードを判断するバージョンのチェックを実装できます。

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>エラー

使用する Apple ID が有効でない場合は、何らかのネットワークまたは認証の問題を意味こともある混乱を招くので、次のエラーが発生します。

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Objective C のドキュメントの読み取り

Apple の開発者ポータルでのストア キットに関する読み取り開発者にプロトコル – 表示[SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – この新機能に関連して前に説明します。 デリゲートのプロトコルとして公開されている 1 つだけメソッド – productViewControllerDidFinish –、`Finished`でイベントを`SKStoreProductViewController`Xamarin.iOS にします。


## <a name="determining-apple-ids"></a>Apple Id を決定します。

必要なの Apple ID、`SKStoreProductViewController`は、*数*(混同しないようにバンドル Id を持つ"com.xamarin.mwc2012"など)。 いくつか異なる方法でする製品を表示するには、以下の Apple ID を調べることができます。

### <a name="itunesconnect"></a>iTunesConnect

パブリッシュするアプリケーションでは、簡単に見つけ、 **Apple ID** iTunes Connect で。

 [ ![](changes-to-storekit-images/image3.png "ITunes Connect で Apple ID を検索します。")](changes-to-storekit-images/image3.png)

 <a name="Search_API" />


### <a name="search-api"></a>検索 API

Apple には、アプリ ストア、iTunes および、iBookstore 内のすべての製品を照会する動的な検索の API が用意されています。 検索 API にアクセスする方法の詳細については含まれて[Apple の関連リソース](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)(関連会社が登録されていない) すべてのユーザーに、API が公開されているが、します。 生成された JSON を解析して、検出、`trackId`で使用する Apple ID は`SKStoreProductViewController`します。

結果は、情報の表示と、アプリ内製品を表示するために使用できるアートワークの Url を含むその他のメタデータも含まれます。

次にいくつかの例を示します。

-   **iBooks app*- [http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **ドットと持っている iBook*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;エンティティ = 電子&amp;国 = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>エンタープライズ パートナー フィード

Apple では、ダウンロード可能なデータベースの準備完了のフラット ファイルの形式で、すべての製品の完全なデータ ダンプを認定パートナーを提供します。 アクセスを修飾する場合、[エンタープライズ パートナー フィード](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html)し、そのデータセットに、製品の Apple ID を確認できます。

エンタープライズ パートナー フィードの多くのユーザーのメンバーであることに注意してください、[関連プログラム](http://www.apple.com/itunes/affiliates)歩合商品の売上を獲得することができます。 `SKStoreProductViewController` 関連の Id をサポートしません (書き込み時にこの可能性があります将来追加される Apple によって)。

### <a name="direct-product-links"></a>直接的な製品のリンク

製品の Apple ID は、その iTunes プレビュー URL リンクから推論することができます。
始まる URL の一部を検索する (アプリ、音楽、またはブック) の製品のリンク、iTunes で`id`に続く数値を使用するとします。

IBooks への直接リンクは、たとえば、します。

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

Apple ID は**364709193**です。 同様に、MWC2012 アプリの直接のリンクは、

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

Apple ID は**496963922**です。

## <a name="in-app-purchase-hosted-content"></a>アプリ内購入には、コンテンツがホストされています。

ダウンロード可能なコンテンツ (ブックまたはその他のメディア、ゲームのレベル アートと構成、またはその他の大きなファイル) などの場合は、アプリ内購入、web サーバーでホストされるようにこれらのファイルを使用して、アプリを安全にダウンロードした後にそれらのコードを組み込む必要があります。購入します。 Ios 6 Apple が導入がサーバー上のファイルをホストするオプションを別のサーバーにすること。 機能は、非が使用できる製品 (使用できるまたはサブスクリプション) に利用できるのみです。 Apple のホスティング サービスを使用する利点は次のとおりです。

-  ホストと帯域幅のコストを削減します。
-  おそらくよりも高い拡張性を使用している現在どのようなサーバー ホストです。 
-  コードは任意のサーバー側の処理をビルドする必要はありませんので、書き込みを少なくします。 
-  バック グラウンドでダウンロードが実装します。


注: テスト ホストされるため、シミュレーターはサポートされていません、iOS でアプリ内購入コンテンツ実際のデバイスでテストする必要があります。

### <a name="hosted-content-basics"></a>ホストされているコンテンツの基本事項

IOS 6、前に、製品を提供する 2 つの方法が (で詳しく説明されている[Xamarin のアプリ内購入](~/ios/platform/in-app-purchasing/index.md)ドキュメント)。

-   **組み込みの製品**– を 'ロックが解除されて'、購入することで (どちらもコード、または埋め込みリソースとして) アプリケーションに組み込まれている機能です。 組み込みの製品には、ロックされていないフォト フィルターまたはゲーム内の power ups などがあります。 
-   **製品のサーバーによって提供される**–、購入後、アプリケーションは、運用しているサーバーからコンテンツをダウンロードする必要があります。 このコンテンツは、購入時にダウンロードおよびデバイスに格納されている、その製品の一部として表示されます。 例には、書籍、雑誌やバック グラウンド アートおよび構成ファイルで構成されるゲームのレベルが含まれます。 


6 Apple ios には、サーバーによって提供される製品のバリエーションが提供しています。 それぞれのサーバーでコンテンツ ファイルをホストします。 これにより、別のサーバーを操作する必要はありませんし、ストア キットが以前に配置を自分で作成したバック グラウンドでダウンロードの機能を提供するために、サーバーによって提供される製品をビルドするとはるかにします。 活用するために Apple のホスト、新しい製品のアプリ内購入をホストするコンテンツを有効にし、活用するためにストア キット コードを変更します。 製品のコンテンツ ファイルは Xcode を使用して作成しを確認して、Apple のサーバーにアップロードします。

 [ ![](changes-to-storekit-images/image4.png "ビルドおよび配信のプロセス")](changes-to-storekit-images/image4.png)

アプリ内購入を提供する、アプリ ストアを使用して*でコンテンツをホスト*次のセットアップと構成が必要です。

-   **iTunes Connect** – する*必要があります*情報を指定銀行および税を Apple に資金を自動的に収集されたを依頼するようにします。 販売、および購入をテストするサンド ボックスのユーザー アカウントを設定するように製品を構成することができます。  *ホストされているコンテンツも構成してあります**Apple でホストするこれらの非が使用できる製品**です。*  
-   **iOS プロビジョニング ポータル**– バンドル Id を作成し、アプリ内購入をサポートするアプリケーションの場合と同様に、アプリのアプリ ストアへのアクセスを有効にします。 
-   **キットを格納**– 製品を表示する、製品の購入、およびトランザクションを復元するため、アプリにコードを追加します。  *Ios 6 ストア キットも管理、コンテンツのダウンロード、製品、バック グラウンドで進行状況の更新プログラムにします。* 
-   **カスタム コード**– を顧客による購入を追跡し、製品や購入したサービスを提供します。 ような新しい iOS 6 ストア キット クラスを利用して`SKDownload`Apple によってホストされているコンテンツを取得します。 


次のセクションでは、作成して、注文書を管理するパッケージをアップロードしてから、ホストされているコンテンツを実装して、この記事のサンプル コードを使用して、プロセスをダウンロードする方法を説明します。


### <a name="sample-code"></a>サンプル コード

サンプル プロジェクト*HostedNonConsumables* (StoreKitiOS6.zip) でアプリを使用してホストされているコンテンツを構築する方法を示します。 アプリでは、次の 2 つ「書籍の」販売、対象のコンテンツを Apple のサーバーでホストを提供します。 実際のアプリケーションでより複雑なコンテンツを使用する可能性がありますが、テキスト ファイルとイメージのコンテンツで構成されます。

アプリは、前に、中、および、購入後に、このようになります。

 [ ![](changes-to-storekit-images/image5.png "前に、中、および、購入後に次のように、アプリの外観します。")](changes-to-storekit-images/image5.png)

テキスト ファイルとイメージがダウンロードされ、アプリケーションの Documents ディレクトリにコピーします。 参照してください、[ファイル システムのドキュメントを扱う](~/ios/app-fundamentals/file-system.md)アプリケーション記憶域の使用可能な別のディレクトリについての詳細。

## <a name="itunes-connect"></a>iTunes Connect

Apple で使用する新しい製品を作成するのコンテンツをホストしている場合を選択することを確認して、**以外が使用できる**製品の種類。 その他の製品の種類では、コンテンツをホストすることはできません。 また、許可しないでのコンテンツをホストしている*既存*製品を販売する; 新製品のコンテンツをホストにのみ、オンにします。

 [ ![](changes-to-storekit-images/image6.png "非が使用できる製品の種類を選択します。")](changes-to-storekit-images/image6.png)

入力、**プロダクト ID**です。 これに必要になる後でこの製品のコンテンツを作成します。

 [ ![](changes-to-storekit-images/image7.png "製品 ID を入力してください。")](changes-to-storekit-images/image7.png)

コンテンツ ホストは、[詳細] セクションで設定されます。 運用開始アプリ内購入する前に、(テスト内容をアップロードした) 場合でもをキャンセルする場合、"ホストのコンテンツを Apple"チェック ボックスをオフします。 ただしコンテンツをホストしている後に削除できません、アプリ内購入をライブになりました。

 [ ![](changes-to-storekit-images/image8.png "Apple でコンテンツをホストしています。")](changes-to-storekit-images/image8.png)

コンテンツをホストしている、製品入力**のアップロードを待機している**状態と、このメッセージを表示。

 [ ![](changes-to-storekit-images/image9.png "製品はアップロードのステータスの待機を入力して、このメッセージを表示")](changes-to-storekit-images/image9.png)

コンテンツを使用して、必要があります、今すぐ Xcode で作成され、アーカイブ ツールを使用してアップロードします。 コンテンツのパッケージを作成する方法については、次のセクションで示されます**作成します。パッケージ ファイル**です。

## <a name="creating-pkg-files"></a>作成します。パッケージ ファイル

Apple にアップロードしたコンテンツのファイルには、次の制限を満たす必要があります。

-  サイズ 2 GB を超えることはできません。
-  実行可能コード (またはコンテンツ外を指しているシンボリック リンク) を含めることはできません。 
-  正しくフォーマットされているあります (形式の .plist ファイルを含む) および .pkg ファイル拡張子が付いています。 これに自動的に Xcode を使ってこれらの手順を実行する場合です。 


これらの制限を満たしている限り、多くの別のファイルと、ファイルの種類を追加することができます。 コンテンツがアプリケーションに配信される前に圧縮し、コードでアクセスする前に、ストア キットで解凍します。

コンテンツのパッケージをアップロードした後、新しいコンテンツに置き換えられますことができます。 新しいコンテンツのアップロードし、通常のプロセスを使用してレビュー/承認のために送信する必要があります。 インクリメントする必要があります、`ContentVersion`フィールド更新されたコンテンツのパッケージにします。

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode アプリ内購入コンテンツ プロジェクト

現在アプリ内購入製品のコンテンツのパッケージを作成するには、Xcode が必要です。 NO OBJECTIVE C のコーディングを必ず指定します。Xcode では、新しいプロジェクトの種類のこれらのパッケージだけにファイルとを含む、plist があります。

このサンプル アプリケーションが書籍の販売 – 各章コンテンツのパッケージが含まれます。

-  テキスト ファイル、および
-  この章を表すイメージ。


選択して開始**ファイル > 新しいプロジェクト**、メニューから選択して**アプリ内購入コンテンツ**:

 [ ![](changes-to-storekit-images/image10.png "コンテンツをアプリ内購入を選択します。")](changes-to-storekit-images/image10.png)

入力、**製品名**と**会社 Id**になるよう、**バンドル Id**と一致する、**プロダクト ID** iTunes で入力しました。この製品に接続します。

 [ ![](changes-to-storekit-images/image11.png "識別子と名前を入力してください。")](changes-to-storekit-images/image11.png)

これは空白では**アプリ内購入コンテンツ**プロジェクト。 右クリックし、**ファイルを追加しています.** ドラッグまたは、**プロジェクト ナビゲーター**です。 いることを確認、 **ContentVersion** (にする必要があります 1.0 では、開始が、後で、コンテンツの更新を選択する場合はそれをインクリメントしてください) が正しくです。

このスクリーン ショットは、Xcode をプロジェクトとメイン ウィンドウに表示されている plist エントリに含まれるコンテンツ ファイルを示します。

 [ ![](changes-to-storekit-images/image12.png "このスクリーン ショットは、プロジェクトとメイン ウィンドウに表示されている plist エントリに含まれるコンテンツ ファイルで Xcode を示しています")](changes-to-storekit-images/image12.png)

すべてのコンテンツ ファイルを追加するとこのプロジェクトを保存し、後でもう一度編集したり、アップロード プロセスを開始できます。

## <a name="uploading-pkg-files"></a>アップロードしています。パッケージ ファイル

コンテンツのパッケージをアップロードする最も簡単な方法は、 **Xcode アーカイブ ツール**です。 選択**製品 > アーカイブ**開始するには、メニューから。

 ![](changes-to-storekit-images/image13.png "Archiven を選択します。")

コンテンツのパッケージは、次に示すように、アーカイブに表示されます。 アーカイブの種類とアイコンを表示するこれは、通知、**アプリ内購入コンテンツ アーカイブ**です。 をクリックして**を検証しています.** 実際には、アップロードを preforming せず、コンテンツのパッケージのエラーを確認します。

 [ ![](changes-to-storekit-images/image14.png "パッケージを検証します。")](changes-to-storekit-images/image14.png)

ITunes、接続の資格情報でログインします。

 [ ![](changes-to-storekit-images/image15.png "接続の資格情報、iTunes を持つログイン")](changes-to-storekit-images/image15.png)

このコンテンツを関連付けるには、正しいアプリケーションとアプリ内購入を選択します。

 [ ![](changes-to-storekit-images/image16.png "このコンテンツを関連付けるには、正しいアプリケーションとアプリ内購入を選択します。")](changes-to-storekit-images/image16.png)

このようなメッセージが表示されます。

 ![](changes-to-storekit-images/image17.png "例の問題のメッセージが見つかりません")

これで同様のプロセスをクリックすると**配布しています.** 実際には、コンテンツをアップロードします。

 [ ![](changes-to-storekit-images/image18.png "アプリを配布します。")](changes-to-storekit-images/image18.png)

コンテンツをアップロードする、最初のオプションを選択します。

 ![](changes-to-storekit-images/image19.png "コンテンツをアップロードします。")

もう一度ログイン:

 [ ![](changes-to-storekit-images/image15.png "ログイン")](changes-to-storekit-images/image15.png)

コンテンツをアップロードすると、正しいアプリケーションやアプリ内購入レコードを選択します。

 [ ![](changes-to-storekit-images/image20.png "アプリケーションとアプリ内の注文書レコードを選択します。")](changes-to-storekit-images/image20.png)

待ちを設定すると、ファイルがアップロードされます。

 [ ![](changes-to-storekit-images/image21.png "コンテンツのアップロード ダイアログ")](changes-to-storekit-images/image21.png)

アップロードが完了したら、コンテンツがアプリ ストアに送信されたことを通知するメッセージが表示されます。

 [ ![](changes-to-storekit-images/image22.png "アップロードの成功のメッセージの例")](changes-to-storekit-images/image22.png)

行った後に戻ったら、製品ページ iTunes Connect パッケージの詳細を表示し、であることに**送信準備ができて**状態です。 製品は、この状態では、サンド ボックス環境でテストを開始できます。 サンド ボックスにテストのための製品を送信する必要はありません。

 [ ![](changes-to-storekit-images/image23.png "iTunes Connect パッケージ詳細を表示し、送信の状態を準備するには")](changes-to-storekit-images/image23.png)

時間がかかるいくつか。 数分後に) 間、アーカイブと iTunes Connect ステータスの更新中にアップロードします。 レビューの製品を個別に、送信したり、アプリケーションのバイナリ ファイルと共に送信することができます。 Apple が正式にコンテンツを承認された後にのみ、されます App Store アプリ内購入を実稼働環境で使用できます。

### <a name="pkg-file-format"></a>パッケージ ファイルの形式

Xcode およびアーカイブ ツールを使用して作成してホストされているコンテンツのパッケージをアップロードする、パッケージ自体の内容は表示されないことを意味します。 ファイルとサンプル アプリを作成したパッケージのディレクトリは、次のように、`plist`ルートと製品内のファイル内のファイル、`Contents`サブディレクトリ。

 [ ![](changes-to-storekit-images/image24.png "ルートとその内容のサブディレクトリ内の製品ファイル形式の .plist ファイル")](changes-to-storekit-images/image24.png)

パッケージのディレクトリ構造に注意してください (内のファイルの場所では特に、`Contents`サブディレクトリ) ため、デバイス上のパッケージからファイルを抽出するには、この情報を理解する必要があります。

### <a name="updating-package-content"></a>パッケージのコンテンツの更新

承認された後にコンテンツを更新するための手順:

-  Xcode でのアプリ内購入コンテンツのプロジェクトを編集します。
-  バージョン番号に上げるです。
-  ITunes Connect にもう一度アップロードします。 後続の購入者は、最新バージョンを自動的に取得しますが、古いバージョンを既に持っているユーザーには、任意の通知は受け取りません。 
-  アプリは、ユーザーに通知し、新しいバージョンのコンテンツを取得するを担当します。 アプリでは、新しいバージョンをダウンロードする関数もビルドする必要があります。 これを行う必要がありますストア キットの復元機能を使用します。 
-  新しいバージョンが存在するかどうかを判断するには、ことができますの機能をビルド SKProducts (をフェッチするアプリに 製品の価格を取得するために使用されるプロセスと同じ) と ContentVersion プロパティを比較します。 

## <a name="purchasing-overview"></a>購入の概要

このセクションの内容を読み取る前に確認既存[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)です。

ホストされているコンテンツを持つ製品ときに発生するイベントのシーケンスを購入し、ダウンロードがこの図に示します。

 [ ![](changes-to-storekit-images/image25.png "ホストされているコンテンツを持つ製品ときに発生するイベントのシーケンスを購入し、ダウンロード")](changes-to-storekit-images/image25.png)

1.  新しい製品は、iTunes を使用してホストするコンテンツの有効な接続で作成できます。 実際のコンテンツが (フォルダーに単に、ドラッグ ファイル) として Xcode で個別に構築された後、アーカイブ、iTunes (コーディングは必要ありません) にアップロードします。 各製品を使用可能になったら発注の承認を得るのため、送信されます。 サンプル コードでこれらの製品 Id は、ハードコードされたが、新しい製品と iTunes Connect にコンテンツを送信するときに更新できるように、リモート サーバーで使用可能な製品の一覧を格納する場合より柔軟なは Apple でコンテンツをホストしています。 
1.  ユーザーが製品を購入するとトランザクションは支払い処理キューに配置されます。 
1.  ストア キットは、iTunes サーバーで処理する注文書の要求を転送します。 
1.  トランザクションが完了の iTunes サーバー上 (リフレッシュ レート。 顧客が請求されます)、製品情報がダウンロードできるかどうかを含む添付、アプリに受信確認が返されます (と、そのファイルのサイズと他のメタデータ)。 
1.  コードが製品を downloadble 場合をチェックして、その場合は、支払キューにも配置されているコンテンツのダウンロード要求を行います。 ストア キットは、iTunes サーバーにこの要求を送信します。 
1.  サーバーは、コンテンツ ファイルをダウンロードの進行状況と、コードに推定残り時間を返すコールバックを提供するストア キットを返します。 
1.  完了するは通知を取得してキャッシュ フォルダーにファイルの場所を渡されます。 
1.  コードは必要がありますファイルをコピーし、製品を購入したことに注意する必要のある任意の状態保存、それらを確認してください。 新しいファイルに対して正しくバックアップ フラグを設定するには、この機会を利用して (ヒント: サーバーから取得し、ユーザーは編集しない場合する可能性がありますをスキップするバックアップ、ユーザーが常に取得ために Apple のサーバーから後で)。 
1.  FinishTransaction を呼び出します。 これは、トランザクションを支払いキューから削除されるので、重要です。 呼び出すことはありませんまで FinishTransaction キャッシュ ディレクトリからコンテンツをコピーした後に重要です。 FinishTransaction を呼び出すと、キャッシュされたファイルは簡単に消去する可能性があります。 


## <a name="implementing-hosted-content-purchase"></a>ホストされているコンテンツの購入を実装します。

次の情報は、完全なと共にお読みください[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)です。 このドキュメントの情報は、ホストされているコンテンツと以前の実装の違いについて説明します。


### <a name="classes"></a>クラス

次のクラスを追加または iOS 6 にホストされているサポートのコンテンツに変更されました。

-   **SKDownload** – 進行中のダウンロードを表す新しいクラスです。 1 つ以上の製品、1 つだけが最初にただしが実装されて、API を使用します。 
-   **SKProduct** – 新しいプロパティの追加: `Downloadable` 、 `ContentVersion` 、`ContentLengths`配列。 
-   **SKPaymentTransaction** – 新しいプロパティの追加:`Downloads`のコレクションを含む`SKDownload`オブジェクトをこの製品がダウンロード可能なコンテンツをホストされているかどうか。 
-   **SKPaymentQueue** – 新しいメソッドを追加:`StartDownloads`です。 このメソッドを呼び出します`SKDownload`オブジェクトをホストされているコンテンツを取得します。 ダウンロードはバック グラウンドで発生します。 
-   **SKPaymentTransactionObserver** – 新しいメソッド:`UpdateDownloads`です。 ストア キットは、現在ダウンロード操作の進行状況の情報には、このメソッドを呼び出します。 


新しい詳細`SKDownload`クラス。

-   **進行状況**– インジケーターを表示する、完了した割合をユーザーに使用できる 0 ~ 1 の間の値。 進行状況を使用しないでください、ダウンロードが完了するかどうかを検出、状態をチェックする場合は 1 を = = = = 完了します。 
-   **TimeRemaining** – ダウンロードの残り時間を秒単位での推定値です。 -1 は、推定値が計算していますを意味します。 
-   **状態**– アクティブな場合、待機中、完了、失敗した、一時停止している、取り消されました。 
-   **ContentURL** – ファイルのコンテンツが配置される、ディスク上の場所、`Cache`ディレクトリ。 ダウンロードが完了後にのみ設定されます。 
-   **エラー** – 状態が失敗した場合、このプロパティを確認します。 


(ホストされているコンテンツの購入に固有のコードは緑で表示) このダイアグラムは、サンプル コードでクラスの間のやり取りを示したものです。

 [ ![](changes-to-storekit-images/image26.png "この図で緑でホストされているコンテンツの購入記録が表示されます。")](changes-to-storekit-images/image26.png)

これらのクラスが使用されているサンプル コードは、このセクションの残りの部分で示されます。

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

既存の変更`UpdatedTransactions`ダウンロード可能なコンテンツ、および呼び出しを確認する上書き`StartDownloads`必要な場合。

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

オーバーライド対象メソッドで新しい`UpdatedDownloads`を次に示します。 ストア キットは後にこのメソッドを呼び出して`StartDownloads`でトリガーされる`UpdatedTransactions`です。 このメソッドは*複数回*でダウンロードの進行状況を提供する中間の間隔でし、もう一度ダウンロードが完了するとします。 通知、メソッドの配列を受け取る`SKDownload`オブジェクト、キュー内の複数のダウンロードの状態で各メソッド呼び出しを提供できるようにします。 示すように、すべての時間と適切なアクション、ダウンロードの状態は、次の実装がチェックされます。

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Cancelled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

このクラスには、新しいメソッドが含まれています。`SaveDownload`各ダウンロードが正常に完了した後に呼び出されます。

ホストされているコンテンツを正常にダウンロードしに解凍した、`Cache`ディレクトリ。 構造、します。パッケージ ファイルがすべてのファイルを保存する必要があります、`Contents`サブディレクトリに、次のコード内からファイルを抽出するように、`Contents`サブディレクトリです。

コードは、コンテンツのパッケージ内のすべてのファイルを反復処理しにコピー、`Documents`のという名前のサブフォルダーに、ディレクトリ、`ProductIdentifier`です。 最後`CompleteTransaction`、どの呼び出し`FinishTransaction`支払キューからトランザクションを削除します。

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

ときに`FinishTransaction`、ダウンロードしたと呼ばれるファイルの保証が不要になったためで、`Cache`ディレクトリ。 すべてのファイルを呼び出す前にコピーする必要があります`FinishTransaction`です。


## <a name="other-considerations"></a>その他の注意事項

上記のコード例では、ホストされているコンテンツの購入の非常に単純な実装を示します。 いくつか追加の点を考慮する必要があります。

### <a name="detecting-updated-content"></a>更新されたコンテンツを検出します。

ホストされているコンテンツのパッケージを更新することはできますが、ストア キットは既にダウンロードして、製品を購入しているユーザーをこれらの更新プログラムをプッシュする任意のメカニズムが提供されません。 この機能を実装するコード、可能性があります確認し、新しい`SKProduct.ContentVersion`プロパティ (場合、`SKProduct`は`Downloadable`) 定期的に、検出のかどうか、値がインクリメントされます。 また、プッシュ通知システムを作成します。

### <a name="installing-updated-content-versions"></a>更新されたコンテンツのバージョンをインストールします。

ファイルが既に存在する場合に、上記のサンプル コードは、ファイルのコピーをスキップします。 コンテンツのダウンロード中の新しいバージョンをサポートする場合でもこれはお勧めではありません。

代わりにバージョンについては、という名前のフォルダーにコンテンツをコピーし、これは、現在のバージョン (の追跡を指定します。 `NSUserDefaults`または完了した注文書レコードを格納する任意の場所)。

### <a name="restoring-transactions"></a>トランザクションを復元します。

ときに`SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions`が呼び出されると、ストア キットは、ユーザーの以前のすべてのトランザクションを返します。 、項目の数が多いが購入した場合、または各購入に非常に大きなコンテンツのパッケージがある場合は、復元結果、大量のネットワーク トラフィックすべて取得キューとしてダウンロードを一度にします。

関連付けられているコンテンツのパッケージの実際のダウンロードから製品を個別に購入したかどうかの追跡することを検討してください。

### <a name="pausing-restarting-and-canceling-downloads"></a>一時停止、再起動してダウンロードのキャンセル

サンプル コードでは、この機能は示しませんは一時停止してホストされているコンテンツのダウンロードを再開することです。 `SKPaymentQueue.DefaultQueue`の方法があります`PauseDownloads`、`ResumeDownloads`と`CancelDownloads`です。

コードを呼び出す場合`FinishTransaction`ダウンロードされる前に支払キューで`Finished`し、そのダウンロードが自動的に取り消されました。

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>ダウンロードされたコンテンツにスキップ バックアップ フラグを設定

非ユーザーのコンテンツがサーバーから簡単に復元される必要がある Apple の iCloud のバックアップ ガイドラインを提案*いない*するバックアップ (ため、不必要を iCloud 記憶域を使用します)。 参照してください、 [、ファイル システムで作業](~/ios/app-fundamentals/file-system.md)バックアップ属性を設定する方法の詳細についてはドキュメントです。

## <a name="summary"></a>まとめ

この記事には、iOS6 でストア キットの 2 つの新しい機能が導入されました。 iTunes や、アプリ内の他のコンテンツを購入し、独自のアプリ内購入をホストする Apple のサーバーを利用します。 この概要は、既存と共にお読みください[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)ストア キット機能の実装の完全なカバレッジ。

## <a name="related-links"></a>関連リンク

- [StoreKit (サンプル)](https://developer.xamarin.com/samples/StoreKit/)
- [アプリ内購入](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit Framework リファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController クラスのリファレンス](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes 検索 API リファレンス](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [ストア キットを使用して製品を販売している WWDC ビデオ。](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
