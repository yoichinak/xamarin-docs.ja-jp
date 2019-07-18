---
title: IOS 6 で StoreKit の変更
description: 'iOS 6 には、2 つの変更をストア キットの API が導入されています: iTunes (と App Store/iBookstore) を表示する機能、アプリと、新しいアプリ内から製品は、Apple が、ダウンロード可能なファイルをホストするオプションを購入します。 このドキュメントでは、Xamarin.iOS でこれらの機能を実装する方法について説明します。'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 35bac91e54181753bd1f3fd8b4cf0b851bfa1882
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827494"
---
# <a name="changes-to-storekit-in-ios-6"></a>IOS 6 で StoreKit の変更

_iOS 6 ストア キット API への 2 つの変更の導入: iTunes (と App Store/iBookstore) を表示する機能、アプリと、新しいアプリ内から製品は、Apple が、ダウンロード可能なファイルをホストするオプションを購入します。このドキュメントでは、Xamarin.iOS でこれらの機能を実装する方法について説明します。_

ストア キット iOS6 での主な変更は、これら 2 つの新機能です。

- **アプリ内のコンテンツの表示 & Purchasing** – ユーザーが購入できるし、アプリを離れることがなくアプリのダウンロード、音楽、書籍、およびその他の iTunes のコンテンツします。 購入を宣伝やレビューと評価をお勧めだけのための独自のアプリにリンクすることもできます。
- **アプリ内購入でホストされているコンテンツ**– Apple の格納し、してファイルをホストする別のサーバーの必要性を削除します自動的に、バック グラウンド ダウンロードのサポートおよびすることができます、アプリ内購入製品に関連付けられているコンテンツを配信。少ないコードを記述します。

参照してください、[アプリ内購入](~/ios/platform/in-app-purchasing/index.md)storekit や Api の詳細なカバレッジのガイド。

## <a name="requirements"></a>必要条件

このドキュメントで説明したストア キット機能は、iOS 6 および Xamarin.iOS 6.0 と共に、Xcode 4.5 が必要です。

## <a name="in-app-content-display--purchasing"></a>アプリ内のコンテンツの表示 (&) の購入

IOS で新しいアプリ内購入機能では、製品情報を表示し、購入またはアプリ内から製品をダウンロードすることができます。
以前のアプリケーションは、iTunes、App Store、またはユーザーの元のアプリケーションのままになります iBookstore をトリガーする必要があります。 この新しい機能は、終わったときに自動的にユーザーとアプリに返します。

[![](changes-to-storekit-images/image1.png "購入後、アプリに自動的に返す")](changes-to-storekit-images/image1.png#lightbox)

でしたこの使用方法の例は次のとおりです。

- **ユーザーが、アプリを評価することを勧める**– ユーザーが評価され、それを離れることがなく、アプリを確認できるように、App Store のページを開くことができます。
- **アプリのクロス昇格**– 購入/ダウンロードをすぐにする機能を発行するその他のアプリケーションを表示するユーザーを許可します。
- **ユーザー支援するヘルプを検索してコンテンツをダウンロード**– コンテンツをアプリを検索、管理、または (例: 集計を購入するユーザーを支援 音楽に関連するアプリでした曲の再生リストを提供し、各曲から、アプリ内購入を許可する)。

1 回、`SKStoreProductViewController`が表示された iTunes、App Store、または、iBookstore に存在するものとして、製品情報と、ユーザーが対話します。 ユーザーは次のとおりです。

- (アプリの場合)、ビューのスクリーン ショット
- サンプルの曲またはビデオ (音楽、テレビ番組や映画) を
- (読み取りと書き込み) のレビュー
- 購入とダウンロードで、ビュー コント ローラーとストア キット内全体に行われます。

内でいくつかのオプション、`SKStoreProductViewController`などをクリックすると、関連するストア アプリを開き、アプリのままにするユーザーも強制は**関連製品**またはアプリの**サポート**リンク。

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

すべてのアプリ内の製品を表示する API が単純な: を作成し、表示のみが必要です、`SKStoreProductViewController`します。 作成し、製品を表示する次の手順に従います。

1. 作成、`StoreProductParameters`ビュー コント ローラーにパラメーターを渡すオブジェクトを含む、`productId`コンス トラクターでします。
1. インスタンスを作成、`SKProductViewController`します。 クラス レベルのフィールドに割り当てます。
1. ビュー コント ローラーにハンドラーを割り当てる`Finished`イベントで、ビュー コント ローラーを無視する必要があります。 ユーザーがキャンセル をクリックしたときにこのイベントが呼び出されますまたは、それ以外の場合、ビュー コント ローラー内のトランザクションを終了します。
1. 呼び出す、`LoadProduct`メソッドに渡して、`StoreProductParameters`と完了ハンドラー。 完了ハンドラーが製品要求が正常にしたことを確認し、存在する場合は、する必要があります、`SKProductViewController`モーダル形式。 製品を取得できない場合に、適切なエラー処理を追加する必要があります。

### <a name="example"></a>例

*ProductView*プロジェクト、 *StoreKit*この記事のサンプル コードを実装して、`Buy`を任意の製品を受け取るメソッドの Apple ID と表示されます、`SKStoreProductViewController`します。 次のコードでは、特定の Apple ID の製品情報が表示されます。

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

実行する – ダウンロードまたは購入内に完全が発生したときに以下のスクリーン ショットのように次のアプリ、 `SKStoreProductViewController`:

[![](changes-to-storekit-images/image2.png "実行時に次のように次のアプリ")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>以前のオペレーティング システムをサポートしています。

サンプル アプリケーションには、以前のバージョンの iOS アプリ ストア、iTunes、または、iBookstore を開く方法を示すコードが含まれています。 使用して、`OpenUrl`を開き、適切に作成された**itunes.com** URL。

バージョンのチェックを実行するコードを次に示すように実装することができます。

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

使用する Apple ID が有効でない場合は何らかのネットワークまたは認証の問題を意味するはわかりにくいため、次のエラーが発生します。

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Objective C のドキュメントを読む

Apple の開発者ポータルでのストア キットに関する詳細を読む開発者 – プロトコルが表示されます[SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – この新機能との関連で説明しました。 デリゲートのプロトコルとして公開されている 1 つだけメソッド – productViewControllerDidFinish –、`Finished`上のイベント、 `SKStoreProductViewController` Xamarin.iOS でします。

## <a name="determining-apple-ids"></a>Apple Id を決定します。

必要な Apple ID、`SKStoreProductViewController`は、*数*(混同しないようにバンドル Id を持つ"com.xamarin.mwc2012"など)。 以下で、表示しようとする製品の Apple ID を確認できるいくつかのさまざまな方法はあります。

### <a name="itunesconnect"></a>iTunesConnect

アプリケーションを発行するは、簡単に見つけ、 **Apple ID** iTunes Connect で。

[![](changes-to-storekit-images/image3.png "ITunes Connect で Apple ID の検索")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />

### <a name="search-api"></a>Search API

Apple では、アプリ ストア、iTunes、および、iBookstore のすべての商品を照会する動的な検索の API を提供します。 Search API にアクセスする方法に関する情報が記載されて[Apple の関連リソース](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)API はすべてのユーザー (登録されていない関連会社) に公開されます。 結果として得られる JSON を解析して、検出、`trackId`を使用した Apple ID は`SKStoreProductViewController`します。

結果は、その他のメタデータなどの情報を表示、アプリで製品を表示するために使用できるアートワーク Url も含まれます。

次にいくつかの例を示します。

- **iBooks アプリ**– [ http://itunes.apple.com/search?term=ibooks&amp; エンティティ ソフトウェア =&amp;国 = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us)
- **ドットと持っている iBook** – [ http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; エンティティ電子ブックを =&amp;国 = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us)

### <a name="enterprise-partner-feed"></a>エンタープライズ パートナー フィード

Apple では、ダウンロード可能なデータベースの準備完了のフラット ファイルの形式で、すべての製品の完全なデータ ダンプを承認済みのパートナーを提供します。 アクセスを修飾する場合、[エンタープライズ パートナー フィード](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html)、そのデータセットで、製品の Apple ID が見つかりません。

エンタープライズ パートナー フィードの多くのユーザーのメンバーである、[アフィリ エイト プログラム](http://www.apple.com/itunes/affiliates)商品の売上を獲得にコミッションをできるようにします。 `SKStoreProductViewController` (この記事の執筆時点) では、関連の Id をサポートしません。

### <a name="direct-product-links"></a>製品の直接リンク

製品の Apple ID は、その iTunes プレビュー URL リンクから推論することができます。
始まる URL の一部を検索する (アプリ、音楽、またはブック) の製品リンクの iTunes で`id`し、次の番号を使用します。

IBooks への直接リンクは、たとえば、します。

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

Apple ID が**364709193**します。 同様に、MWC2012 アプリの直接のリンクは

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

Apple ID が**496963922**します。

## <a name="in-app-purchase-hosted-content"></a>アプリ内購入には、コンテンツがホストされています。

これらのファイル、web サーバーでホストするために使用し、し、アプリを安全にダウンロード後にコードを組み込む必要がある (ブックまたはその他のメディア、ゲーム同位のアートと構成、またはその他の大きなファイル) などのダウンロード可能なコンテンツの場合は、アプリ内購入購入できます。 IOS 6 以降では、Apple をホストする、サーバー上のファイルを別のサーバーの必要がなくなります。 機能は、非コンシューマブル製品 (使用できるまたはサブスクリプション) の使用のみ。 Apple のホスティング サービスを使用する利点は次のとおりです。

- ホスティングと帯域幅のコストを削減します。
- おそらくよりもスケーラブルな server ホストが現在使用しています。 
- コードは、サーバー側の処理を構築する必要がないため、書き込みを少なくします。 
- バック グラウンド ダウンロードは自動的に実装します。

注: に ios Simulator がサポートされていません、アプリ内購入コンテンツ テスト ホストされている場合、実際のデバイスでテストする必要があります。

### <a name="hosted-content-basics"></a>ホストされているコンテンツの基礎

IOS 6、前に、製品を提供する 2 つの方法が (で詳しく説明されている[Xamarin のアプリ内購入](~/ios/platform/in-app-purchasing/index.md)ドキュメント)。

- **組み込み製品**– を 'ロックが解除されて'、購入することで (コード、または埋め込みリソースとしてか)、アプリケーションに組み込まれている機能です。 組み込みの製品の例には、ロックされていない写真フィルターまたはゲームのパワーアップが含まれます。
- **サーバーによって提供される製品**–、購入後、アプリケーションは、運用しているサーバーからコンテンツをダウンロードする必要があります。 このコンテンツは、購入時にダウンロードおよびデバイスに格納されている、製品の一部としてレンダリングされます。 例には、ブック、マガジンのバック ナンバー、またはバック グラウンドのアートと構成ファイルで構成されるゲームのレベルが含まれます。

IOS では、6 Apple は、サーバーによって提供される製品のバリエーションを提供しています: がサーバーにコンテンツ ファイルをホストします。 これにより、別のサーバーを操作する必要はありませんし、ストア キットを自分で記述していたバック グラウンド ダウンロード機能を提供するためにサーバーによって提供される製品を構築するはるかに簡単です。 活用するために Apple のホスト、アプリ内購入の新製品のコンテンツのホスティングを有効にしに活用するためにストア キット コードを変更します。 製品のコンテンツ ファイルは、Xcode を使用して構築し、審査およびリリースするための Apple のサーバーにアップロードします。

[![](changes-to-storekit-images/image4.png "ビルドおよび配信の処理")](changes-to-storekit-images/image4.png#lightbox)

App Store を使用して、アプリ内購入を提供する*でコンテンツをホスト*次のセットアップと構成が必要です。

- **iTunes Connect** – する*する必要があります*自動的に収集された資金を送金するようにするための Apple、銀行と税の情報が用意します。 製品を販売、および購入をテストするサンド ボックスのユーザー アカウントの設定を構成できます。  _構成することする必要がありますもホストするコンテンツを Apple をホストするこれらの非コンシューマブル製品の_します。
- **iOS プロビジョニング ポータル**– バンドル Id を作成して、アプリ内購入をサポートするアプリケーションの場合と同様に、アプリでは、App Store へのアクセスを有効にします。
- **ストア キット**– 製品を表示、製品を購入およびトランザクションを復元するため、アプリにコードを追加します。  _IOS 6 ストア キットも管理バック グラウンドで進行状況の更新で、製品コンテンツをダウンロードします。_
- **カスタム コード**– を顧客による購入を追跡し、製品や購入したサービスを提供します。 などの iOS 6 ストア キット クラスの新しい利用`SKDownload`Apple によってホストされているコンテンツを取得します。

次のセクションを作成し、購買を管理するパッケージをアップロードして、ホストするコンテンツを実装し、この記事のサンプル コードを使用して、プロセスをダウンロードする方法を説明します。

### <a name="sample-code"></a>サンプル コード

サンプル プロジェクト*HostedNonConsumables* (StoreKitiOS6.zip) の使用がコンテンツをホストします。 アプリでは、2 つ「書籍の」販売、対象のコンテンツを Apple のサーバーでホストは提供します。 複雑なコンテンツは、実際のアプリケーションで使用できますが、テキスト ファイルと、イメージのコンテンツで構成されます。

アプリは、前に、中に、および、購入後に、ようになります。

 [![](changes-to-storekit-images/image5.png "アプリは前に、最中、購入後にこのような検索します。")](changes-to-storekit-images/image5.png#lightbox)

テキスト ファイルとイメージがダウンロードして、アプリケーションの Documents ディレクトリにコピーします。 アプリケーション ストレージに使用できる別のディレクトリの詳細については、次を参照してください。、[ファイル システムのマニュアル](~/ios/app-fundamentals/file-system.md)します。

## <a name="itunes-connect"></a>iTunes Connect

Apple が使用する新しい製品を作成するのコンテンツをホストを必ず選択して、**非コンシューマブル**製品の種類。 その他の製品の種類では、コンテンツをホストすることはできません。 またが有効にしないのコンテンツをホストしている*既存*製品を販売する;、新製品のコンテンツをホストにのみ、有効にします。

 [![](changes-to-storekit-images/image6.png "非コンシューマブル製品の種類を選択します。")](changes-to-storekit-images/image6.png#lightbox)

入力、**製品 ID**します。 この ID はこの製品のコンテンツを作成するときに後では、必要になります。

 [![](changes-to-storekit-images/image7.png "製品 ID を入力します。")](changes-to-storekit-images/image7.png#lightbox)

コンテンツをホストするいると、[詳細] セクションで設定されます。 オフにして、アプリ内購入運用を開始する前に、 **Apple でコンテンツをホスト**(テスト内容をアップロードしている) 場合でもをキャンセルする場合にチェック ボックスをオン。 ただしコンテンツをホストしている後に削除できません、アプリ内購入が稼動します。

 [![](changes-to-storekit-images/image8.png "Apple とのコンテンツをホストしています。")](changes-to-storekit-images/image8.png#lightbox)

コンテンツのホストをオンにして、製品は入力**アップロードを待機している**状態と、このメッセージを表示。

 [![](changes-to-storekit-images/image9.png "製品はアップロードの状態の待機を入力し、このメッセージを表示")](changes-to-storekit-images/image9.png#lightbox)

コンテンツのパッケージは、Xcode で作成して、アーカイブ ツールを使用してアップロードします。 コンテンツのパッケージを作成する方法については、次のセクションで指定された**作成します。PKG ファイル**します。

## <a name="creating-pkg-files"></a>作成します。PKG ファイル

Apple にアップロードしたコンテンツのファイルには、次の制限を満たす必要があります。

- サイズ 2 GB を超えることはできません。
- 実行可能コード (または、コンテンツの外側を指すシンボリック リンク) を含めることはできません。
- 正しく書式設定する必要があります (など、 **.plist**ファイル) であり、 **.pkg**ファイル拡張子。 これは実行に自動的に Xcode を使用してこれらの手順に従う場合。

これらの制限を満たしている限り、多くのファイルが異なると、ファイルの種類を追加できます。 コンテンツがアプリケーションに配信する前に zip 形式であり、コードでアクセスする前に、ストア キットによって解凍されします。

コンテンツのパッケージをアップロードした後、新しいコンテンツを交換できます。 新しいコンテンツのアップロードされ、通常のプロセスを使用してレビュー/承認のために送信する必要があります。 インクリメント、`ContentVersion`フィールドに新しいことを示すためにコンテンツのパッケージを更新します。

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode アプリ内購入コンテンツ プロジェクト

現在の製品のアプリ内購入コンテンツのパッケージを作成するには、Xcode が必要です。 NO OBJECTIVE C コーディングは不要です。Xcode を新しいプロジェクトのみのファイルと plist を含む型をこれらのパッケージがあります。

サンプル アプリケーションが販売書籍の章 – 各」の章のコンテンツのパッケージが含まれます。

-  テキスト ファイル、および
-  」の章を表すイメージ。


選択して開始**ファイル > 新しいプロジェクト**、メニューから選択して**アプリ内購入コンテンツ**:

 [![](changes-to-storekit-images/image10.png "アプリ内購入コンテンツを選択します。")](changes-to-storekit-images/image10.png#lightbox)

入力、**製品名**と**会社識別子**ように、**バンドル識別子**と一致する、**製品 ID** iTunes で入力しました。この製品に接続します。

[![](changes-to-storekit-images/image11.png "名前と識別子を入力します。")](changes-to-storekit-images/image11.png#lightbox)

空白をする必要がありますが**アプリ内購入コンテンツ**プロジェクト。 右クリックし、**ファイルを追加しています.** ドラッグするか、**プロジェクト ナビゲーター**します。 いることを確認、 **ContentVersion**が正しいの (これが 1.0 では、開始は以降では、コンテンツの更新を選択する場合はそれをインクリメントしてください)。

このスクリーン ショットでは、プロジェクトとメイン ウィンドウに表示される plist エントリに含まれるコンテンツのファイルに Xcode を示しています。

[![](changes-to-storekit-images/image12.png "このスクリーン ショット、プロジェクトとメイン ウィンドウに表示される plist エントリに含まれるコンテンツのファイルに Xcode を示しています")](changes-to-storekit-images/image12.png#lightbox)

すべてのコンテンツ ファイルを追加した後とこのプロジェクトを保存に後でもう一度編集またはアップロード プロセスを開始します。

## <a name="uploading-pkg-files"></a>アップロードしています。PKG ファイル

コンテンツのパッケージをアップロードする最も簡単な方法は、 **Xcode のアーカイブ ツール**します。 選択**製品 > アーカイブ**開始するには、メニューから。

![](changes-to-storekit-images/image13.png "Archiven を選択します。")

コンテンツのパッケージは、次に示すように、アーカイブに表示されます。 アーカイブの種類とアイコンは、この行は、表示する、**アプリ内購入コンテンツ アーカイブ**します。 クリックして**を検証しています.** 実際には、アップロードを実行せず、コンテンツのパッケージのエラーを確認します。

[![](changes-to-storekit-images/image14.png "パッケージを検証します。")](changes-to-storekit-images/image14.png#lightbox)

ITunes Connect の資格情報でログインします。

[![](changes-to-storekit-images/image15.png "ITunes Connect の資格情報でログイン")](changes-to-storekit-images/image15.png#lightbox)

関連付けるには、このコンテンツは、適切なアプリケーションとアプリ内購入を選択します。

[![](changes-to-storekit-images/image16.png "関連付けるには、このコンテンツは、適切なアプリケーションとアプリ内購入を選択します。")](changes-to-storekit-images/image16.png#lightbox)

このスクリーン ショットのようなメッセージが表示されます。

![例の問題のメッセージなし](changes-to-storekit-images/image17.png "例の問題のメッセージはありません")

同様のプロセスを経由し、クリックしてもようになりました**配布しています.** コンテンツを実際にアップロードされます。

[![アプリ配布](changes-to-storekit-images/image18.png "アプリを配布します")](changes-to-storekit-images/image18.png#lightbox)

コンテンツをアップロードする、最初のオプションを選択します。

![コンテンツのアップロード](changes-to-storekit-images/image19.png "コンテンツのアップロード")

もう一度サインインします。

[![](changes-to-storekit-images/image15.png "ログイン")](changes-to-storekit-images/image15.png#lightbox)

コンテンツをアップロードするには、適切なアプリケーションとアプリ内購入のレコードを選択します。

[![](changes-to-storekit-images/image20.png "アプリケーションとアプリ内購入のレコードを選択します。")](changes-to-storekit-images/image20.png#lightbox)

ファイルがアップロードされるまで待ち。

[![](changes-to-storekit-images/image21.png "コンテンツのアップロード ダイアログ")](changes-to-storekit-images/image21.png#lightbox)

アップロードが完了したら、コンテンツが、App Store に送信されたことを通知するメッセージが表示されます。

[![](changes-to-storekit-images/image22.png "アップロードが成功したメッセージの例")](changes-to-storekit-images/image22.png#lightbox)

行われていますとに戻ると、製品のページでは、iTunes Connect パッケージの詳細を表示し、**送信準備ができて**状態。 製品は、この状態では、サンド ボックス環境でテストを開始できます。 サンド ボックスでのテスト用の製品を送信する必要はありません。

[![](changes-to-storekit-images/image23.png "iTunes Connect パッケージの詳細を表示し、準備完了 でステータスを送信します。")](changes-to-storekit-images/image23.png#lightbox)

時間が (例: かかることができます。 数分後に) 間、アーカイブ、iTunes Connect のステータスが更新中にアップロードします。 レビュー用の製品を個別に送信したり、アプリケーションのバイナリと組み合わせて送信できます。 Apple が正式にコンテンツを承認された後でのみリリースされる予定、実稼働環境で App Store アプリ内購入します。

### <a name="pkg-file-format"></a>PKG ファイルの形式

Xcode とアーカイブ ツールを使用して作成してホストされているコンテンツのパッケージをアップロードするには、パッケージ自体の内容を表示することはありませんを意味します。 ファイルとサンプル アプリについては、次のスクリーン ショットのように作成されたパッケージ内のディレクトリで、 **plist**ルートと製品内のファイル内のファイル、**内容**サブディレクトリ。

[![](changes-to-storekit-images/image24.png "ルートとその内容のサブディレクトリに、製品ファイルの plist ファイル")](changes-to-storekit-images/image24.png#lightbox)

パッケージのディレクトリ構造に注意してください (特に内のファイルの場所、`Contents`サブディレクトリ) ため、デバイス上のパッケージからファイルを抽出するには、この情報を理解する必要があります。

### <a name="updating-package-content"></a>パッケージ コンテンツの更新

承認された後にコンテンツを更新するための手順:

- Xcode でプロジェクトをアプリ内購入コンテンツを編集します。
- バージョン番号を増やします。
- ITunes Connect に再度アップロードします。 後続の購入者の最新バージョンを自動的に取得するが、古いバージョンを既に持っているユーザーに通知が送信されません。
- アプリがユーザーに通知し、それらのコンテンツの新しいバージョンを取得する責任を負います。 アプリのストア キットの復元機能を使用して、新しいバージョンをダウンロードする関数もビルドする必要があります。
- 新しいバージョンが存在するかを確認するのには、(例: SKProducts をフェッチするアプリに機能を構築できます。 製品の価格を取得するために使用する同じプロセス) と ContentVersion プロパティを比較します。

## <a name="purchasing-overview"></a>購入の概要

このセクションを読む前に確認既存[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)します。

ホストされるコンテンツ、製品のときに発生するイベントのシーケンスを購入して、ダウンロードは、この図に示します。

[![](changes-to-storekit-images/image25.png "ホストされるコンテンツ、製品のときに発生するイベントのシーケンスを購入およびダウンロード")](changes-to-storekit-images/image25.png#lightbox)

1. ITunes Connect でホストするコンテンツの有効では、新製品を作成できます。 実際のコンテンツが (としてフォルダーにファイルを単にドラッグ) Xcode で個別に構築されたし、アーカイブし、iTunes (コーディングは必要ありません) にアップロードします。 各製品はご購入いただけますがその後、承認のため送信されます。 サンプル コードでこれらの製品 Id は、ハードコーディングされたが、新製品と iTunes Connect へのコンテンツを送信するときに更新できるように、リモート サーバーで使用可能な製品の一覧を格納する場合より柔軟なは、Apple とのコンテンツをホストします。
1. ユーザーは、製品を購入、トランザクションが処理の支払キューに配置されます。
1. ストア キットは、iTunes サーバー処理のために購入要求を転送します。
1. トランザクションが完了 iTunes サーバー (例。 顧客は課金されます) を含むかどうかはダウンロード可能なアタッチされている製品情報、アプリに受信確認が返されます (そうである場合、ファイルのサイズとその他のメタデータ)。
1. コードは、製品は、ダウンロード可能な場合にチェックし、その場合は、支払いのキューにも配置されているコンテンツのダウンロード要求を行う必要があります。 ストア キットは、iTunes のサーバーにこの要求を送信します。
1. サーバーは、ダウンロードの進行状況と、コードに推定残り時間を返すコールバックを提供するストア キットをコンテンツ ファイルを返します。
1. 完了すると、通知を取得し、キャッシュ フォルダーにファイルの場所を渡されます。
1. コードは、ファイルをコピーし、製品が購入されていることを覚えておく必要がある任意の状態の保存、ことを確認する必要があります。 新しいファイルのバックアップのフラグが正しく設定するには、この機会を利用 (ヒント: サーバーからは、ユーザーを編集しないがおそらくをスキップするバックアップ、ユーザー常に取得できます Apple のサーバーから将来のため)。
1. FinishTransaction を呼び出します。 この手順は重要なトランザクション支払いキューから削除します。 呼び出すことはありませんまで FinishTransaction キャッシュ ディレクトリの外部コンテンツをコピーした後に重要です。 FinishTransaction を呼び出すと、キャッシュされたファイルはすぐに削除する可能性があります。

## <a name="implementing-hosted-content-purchase"></a>ホストされているコンテンツの購入を実装します。

完全なと組み合わせて、次の情報が読み取られる[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)します。 このドキュメントの情報は、ホストされるコンテンツと以前の実装の違いについて説明します。

### <a name="classes"></a>クラス

次のクラスを追加または iOS 6 のサポートがホストされている内容に変更されました。

- **SKDownload** – 進行中のダウンロードを表す新しいクラスです。 1 つ以上、製品ごと、1 つ最初のみが実装されて、API を使用します。
- **SKProduct** – 新しいプロパティの追加: `Downloadable`、 `ContentVersion`、`ContentLengths`配列。
- **SKPaymentTransaction** – 新しいプロパティの追加:`Downloads`のコレクションを含む`SKDownload`オブジェクトをこの製品でダウンロード可能なコンテンツがホストされているかどうか。
- **SKPaymentQueue** – 新しいメソッドの追加:`StartDownloads`します。 このメソッドを呼び出します`SKDownload`オブジェクトをホストするコンテンツをフェッチします。 ダウンロードはバック グラウンドで発生します。
- **SKPaymentTransactionObserver** – 新しいメソッド:`UpdateDownloads`します。 ストア キットは、現在ダウンロード操作の進行状況の情報には、このメソッドを呼び出します。

新しい詳細`SKDownload`クラス。

- **進行状況**– 完了率のインジケーターをユーザーに表示するのに使用できる 0 ~ 1 の間の値。 進行状況を使用しないでください、ダウンロードが完了するかどうかを検出、状態をチェックする 1 を = = Finished = =。
- **TimeRemaining** – ダウンロードの残り時間を秒単位での推定値です。 -1 は、見積もりの計算も意味します。
- **状態**– アクティブな待機中、完了、失敗した、一時停止している、取り消されました。
- **ContentURL** – ファイルの配置される場所、コンテンツがディスク上で、`Cache`ディレクトリ。 ダウンロードが完了した後にのみ設定されます。
- **エラー** – 状態が失敗した場合、このプロパティを確認します。

サンプル コードでクラス間の相互作用は (ホストされているコンテンツでの購入に固有のコードは緑で表示)、この図に示します。

[![](changes-to-storekit-images/image26.png "ホストされているコンテンツの購入がこの図では緑色で表示します。")](changes-to-storekit-images/image26.png#lightbox)

これらのクラスが使用されているサンプル コードは、このセクションの残りの部分で示されます。

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

既存の変更`UpdatedTransactions`、ダウンロード可能なコンテンツと呼び出しを確認する上書き`StartDownloads`必要な場合。

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

新しいメソッドをオーバーライド`UpdatedDownloads`を次に示します。 ストア キットは、後に、このメソッドを呼び出して`StartDownloads`でトリガーされる`UpdatedTransactions`します。 このメソッドが呼び出された*複数回*でダウンロードの進行状況を提供する中間の間隔で、ダウンロードが完了し、もう一度とします。 配列を受け取るメソッドに注目してください`SKDownload`オブジェクトの場合、各メソッドの呼び出しをキューに複数のダウンロードの状態を提供できるようにします。 ように、すべての時間と適切なアクション、ダウンロードの状態は、次の実装がチェックされます。

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
            Console.WriteLine ("Canceled"); // TODO: UI?
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

ホストされたコンテンツを正常にダウンロードし、解凍に、`Cache`ディレクトリ。 構造、します。PKG ファイルがすべてのファイルを保存する必要があります、`Contents`サブディレクトリは、次のコード内からファイルを抽出するため、`Contents`サブディレクトリ。

コードは、コンテンツのパッケージ内のすべてのファイルを反復処理し、それらにコピー、`Documents`のという名前のサブフォルダーでは、ディレクトリ、`ProductIdentifier`します。 最後`CompleteTransaction`、呼び出す`FinishTransaction`支払キューからトランザクションを削除します。

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

ときに`FinishTransaction`が呼び出されると、ダウンロードしたファイルがであるという保証がなくなります、`Cache`ディレクトリ。 すべてのファイルを呼び出す前にコピーする必要があります`FinishTransaction`します。


## <a name="other-considerations"></a>その他の注意事項

上記のコード例では、ホストされているコンテンツの購入の非常に単純な実装を示します。 いくつか追加の点を考慮する必要がありますがあります。

### <a name="detecting-updated-content"></a>更新されたコンテンツを検出します。

ホストされているコンテンツのパッケージを更新することはできますが、ストア キットは既にダウンロードして、製品を購入するユーザーにこれらの更新プログラムをプッシュするメカニズムが提供されません。 この機能を実装するコードは、新しいチェックことができます`SKProduct.ContentVersion`プロパティ (場合、`SKProduct`は`Downloadable`) 定期的に、検出のかどうか、値がインクリメントされます。 また、プッシュ通知システムを構築できます。

### <a name="installing-updated-content-versions"></a>更新されたコンテンツのバージョンをインストールします。

ファイルが既に存在する場合に、上記のサンプル コードは、ファイルのコピーをスキップします。 ダウンロードされるコンテンツの新しいバージョンをサポートする場合でもこれは良い考えではありません。

バージョンについては、という名前のフォルダーに内容をコピーし、これは、現在のバージョン (例: の追跡を別の方法があります。 `NSUserDefaults`または購入を完了したレコードを格納する任意の場所)。

### <a name="restoring-transactions"></a>トランザクションを復元します。

ときに`SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions`が呼び出されると、ストア キットは、ユーザーの以前のすべてのトランザクションを返します。 、項目の数が多いが購入した場合、または購入ごとにコンテンツの大きなパッケージがある場合は、復元結果、大量のネットワーク トラフィックすべて取得キューとしてダウンロードを一度にします。

製品関連のコンテンツのパッケージの実際のダウンロードから個別に購入したかどうかを追跡することを検討してください。

### <a name="pausing-restarting-and-canceling-downloads"></a>一時停止と再起動、ダウンロードのキャンセル

サンプル コードは、この機能を示していませんが、一時停止し、ホストされているコンテンツのダウンロードを再開すること、します。 `SKPaymentQueue.DefaultQueue`の方法があります`PauseDownloads`、`ResumeDownloads`と`CancelDownloads`します。

コードを呼び出す場合`FinishTransaction`ダウンロードされる前に支払キューで`Finished`し、そのダウンロードが自動的に取り消されました。

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>ダウンロードしたコンテンツにスキップ バックアップ フラグを設定

Apple の iCloud のバックアップ ガイドラインを提案する非ユーザー コンテンツ サーバーから簡単に復元される必要があります*いない*のバックアップ (iCloud 記憶域を使用する場合と不必要に) ためです。 バックアップの属性の設定の詳細については、次を参照してください。、[ファイル システム](~/ios/app-fundamentals/file-system.md)ドキュメント。

## <a name="summary"></a>まとめ

この記事には iOS6 でストア キットの 2 つの新しい機能が導入: iTunes とアプリ内の他のコンテンツを購入し、独自のアプリ内購入をホストする Apple のサーバーを利用します。 この概要は、既存と共にお読みください[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)のストア キットの機能を実装する完全にカバーします。

## <a name="related-links"></a>関連リンク

- [StoreKit (サンプル)](https://developer.xamarin.com/samples/monotouch/StoreKit/)
- [アプリ内購入](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit のフレームワーク参照](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController クラスのリファレンス](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes Search API リファレンス](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC ビデオ:ストア キットで製品を販売](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
