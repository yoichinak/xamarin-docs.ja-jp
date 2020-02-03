---
title: iOS 6 の StoreKit の変更点
description: iOS 6 では、ストアキット API に2つの変更が導入されています。アプリ内から iTunes (および App Store/iBookstore) 製品を表示する機能と、Apple がダウンロード可能なファイルをホストする新しいアプリ内購入オプションです。 このドキュメントでは、これらの機能を Xamarin iOS で実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 7cf18934c70acf59213a697ab57b6c5e308e7b2a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725222"
---
# <a name="changes-to-storekit-in-ios-6"></a>iOS 6 の StoreKit の変更点

_iOS 6 では、ストアキット API に2つの変更が導入されました。アプリ内から iTunes (および App Store/iBookstore) 製品を表示する機能と、Apple がダウンロード可能なファイルをホストする新しいアプリ内購入オプションが導入されました。このドキュメントでは、これらの機能を Xamarin iOS で実装する方法について説明します。_

IOS6 のストアキットに対する主な変更は、次の2つの新機能です。

- アプリ**内コンテンツ表示 & 購入**-ユーザーはアプリを離れることなく、アプリ、音楽、ブック、およびその他の iTunes コンテンツを購入してダウンロードできます。 また、独自のアプリにリンクして購入を促進したり、レビューや評価を促進したりすることもできます。
- **アプリ内購入ホストコンテンツ**-Apple は、アプリ内購入製品に関連付けられているコンテンツを格納して配信します。これにより、ファイルをホストするための個別のサーバーが不要になり、バックグラウンドダウンロードが自動的にサポートされ、コードの記述が少なくなります。

StoreKit Api の詳細については、[アプリ内購入](~/ios/platform/in-app-purchasing/index.md)ガイドを参照してください。

## <a name="requirements"></a>必要条件

このドキュメントで説明されているストアキットの機能には、iOS 6 と Xcode 6.0 4.5 が必要です。

## <a name="in-app-content-display--purchasing"></a>アプリ内コンテンツ表示 & 購入

IOS のアプリ内購入の新機能により、ユーザーはアプリ内から製品情報を表示し、製品を購入またはダウンロードすることができます。
以前のアプリケーションでは、iTunes、App Store、または iBookstore をトリガーする必要がありました。これにより、ユーザーは元のアプリケーションを離れることになります。 この新しい機能は、完了時に自動的にユーザーをアプリに返します。

[![](changes-to-storekit-images/image1.png "Automatically returning to an app after purchase")](changes-to-storekit-images/image1.png#lightbox)

これを使用する方法の例を次に示します。

- **アプリの評価をユーザーに促す**-アプリストアのページを開いて、ユーザーがアプリを離れることなく評価してレビューできるようにすることができます。
- **アプリの昇格**-ユーザーが発行した他のアプリを表示し、すぐに購入/ダウンロードできるようにします。
- **ユーザーがコンテンツを検索してダウンロード**できるようにする-アプリで検出、管理、または集計されたコンテンツをユーザーが購入できるようにします (例: 音楽関連のアプリでは、曲の再生リストを提供し、各曲をアプリ内から購入できるようにすることができます。

`SKStoreProductViewController` が表示されると、ユーザーは iTunes、App Store、または iBookstore の場合と同じように製品情報を操作できます。 ユーザーは次のことができます。

- スクリーンショットの表示 (アプリの場合)
- 曲またはビデオのサンプル (音楽、テレビ番組、映画)
- 読み取り (および書き込み) レビュー、
- 購入 & ダウンロード。これは、ビューコントローラーとストアキット内で完全に発生します。

`SKStoreProductViewController` 内の一部のオプションでは、ユーザーがアプリを終了し、**関連する製品**やアプリの**サポート**リンクをクリックするなど、関連するストアアプリを開くように強制することができます。

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

アプリ内で製品を表示するための API は単純です。必要なのは、`SKStoreProductViewController`を作成して表示することだけです。 製品を作成して表示するには、次の手順に従います。

1. `StoreProductParameters` オブジェクトを作成して、パラメーターをビューコントローラーに渡します。これには、コンストラクター内の `productId` も含まれます。
1. `SKProductViewController`をインスタンス化します。 クラスレベルフィールドに割り当てます。
1. ビューコントローラーの `Finished` イベントにハンドラーを割り当てます。これにより、ビューコントローラーが破棄されます。 このイベントは、ユーザーが [キャンセル] を押したときに呼び出されます。それ以外の場合は、ビューコントローラー内のトランザクションを終了します。
1. `StoreProductParameters` と完了ハンドラーを渡す `LoadProduct` メソッドを呼び出します。 完了ハンドラーは、製品要求が正常に行われたことを確認し、存在する場合は、`SKProductViewController` モーダルであることを示します。 製品を取得できない場合は、適切なエラー処理を追加する必要があります。

### <a name="example"></a>例

この記事の*Storekit*サンプルコードの*productview*プロジェクトは、任意の製品の Apple ID を受け取り、`SKStoreProductViewController`を表示する `Buy` メソッドを実装しています。 次のコードは、特定の Apple ID の製品情報を表示します。

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

アプリの実行時には、次のスクリーンショットのようになります。ダウンロードまたは購入は、`SKStoreProductViewController`内で完全に発生します。

[![](changes-to-storekit-images/image2.png "The app looks like this when running")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>以前のオペレーティングシステムのサポート

このサンプルアプリケーションには、以前のバージョンの iOS でアプリストア、iTunes、または iBookstore を開く方法を示すコードが含まれています。 `OpenUrl` メソッドを使用して、適切に細工された**Itunes.com** URL を開きます。

次に示すように、バージョンチェックを実装して、実行するコードを決定できます。

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

使用する Apple ID が有効でない場合、次のエラーが発生します。これは、何らかの種類のネットワークまたは認証の問題を意味するため、混乱する可能性があります。

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>目的 C のドキュメントを読む

Apple の開発者ポータルでストアキットについてお読みになる開発者は、この新機能に関して説明されている protocol- [Skstoreproductviewcontrollerdelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html)を参照してください。 デリゲートプロトコルには、productViewControllerDidFinish のメソッドが1つだけあります。これは、Xamarin. iOS の `SKStoreProductViewController` で `Finished` イベントとして公開されています。

## <a name="determining-apple-ids"></a>Apple Id の確認

`SKStoreProductViewController` に必要な Apple ID は*数値*です ("mwc2012" のようなバンドル id と混同しないでください)。 表示したい製品の Apple ID を確認するには、次のようないくつかの方法があります。

### <a name="itunesconnect"></a>iTunesConnect

発行するアプリケーションでは、iTunes Connect で**APPLE ID**を簡単に見つけることができます。

[![](changes-to-storekit-images/image3.png "Finding the Apple ID in iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />

### <a name="search-api"></a>Search API

Apple は、App Store、iTunes、および iBookstore 内のすべての製品に対してクエリを実行するための動的検索 API を提供しています。 Search API へのアクセス方法に関する情報は、Apple の関連リソースに記載されています。ただし、API は、登録された関連会社ではなくすべてのユーザーに公開されます。 結果の JSON を解析して、`SKStoreProductViewController`で使用する Apple ID である `trackId` を検出できます。

結果には、アプリで製品をレンダリングするために使用できる表示情報やアートワークの Url など、他のメタデータも含まれます。

次にいくつかの例を示します。

- **Ibooks アプリ**– [https://itunes.apple.com/search?term=ibooks&amp; entity = software&amp;country = us](https://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us)
- **ドットと Kangaroo iBook** – [https://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; entity = ebook&amp;country = us](https://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us)

### <a name="enterprise-partner-feed"></a>エンタープライズパートナーフィード

Apple は、承認されたパートナーに、すべての製品の完全なデータダンプを、ダウンロード可能なデータベース準備済みフラットファイルの形式で提供します。 エンタープライズパートナーフィードへのアクセスが必要な場合は、製品の Apple ID がそのデータセットに含まれています。

エンタープライズパートナーフィードの多くのユーザーは、製品売上に対して歩合を獲得できる[関連プログラム](https://www.apple.com/itunes/affiliates)のメンバーです。 `SKStoreProductViewController` は、(書き込み時に) 関連 Id をサポートしていません。

### <a name="direct-product-links"></a>製品の直接リンク

製品の Apple ID は、iTunes Preview URL リンクから推測できます。
任意の iTunes 製品リンク (アプリ、音楽、またはブック) で、`id` で始まる URL の部分を見つけて、次の番号を使用します。

たとえば、iBooks への直接リンクは次のようになります。

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

Apple ID は**364709193**です。 同様に、MWC2012 アプリでは、直接リンクは

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

Apple ID は**496963922**です。

## <a name="in-app-purchase-hosted-content"></a>アプリ内購入ホストコンテンツ

アプリ内購入が、ダウンロード可能なコンテンツ (書籍やその他のメディア、ゲームレベルのアートと構成、その他の大きなファイル) で構成されている場合、これらのファイルは web サーバーでホストされるため、アプリケーションは、ご. IOS 6 以降では、Apple はサーバー上でファイルをホストするため、個別のサーバーを必要としません。 この機能は、非消費製品 (使用不可またはサブスクリプションではない) でのみ利用できます。 Apple のホスティングサービスを使用する利点は次のとおりです。

- ホスティング & 帯域幅のコストを節約します。
- 現在使用している任意のサーバーホストよりも拡張性が高いと思います。
- サーバー側の処理を構築する必要がないため、記述するコードが少なくて済みます。
- バックグラウンドダウンロードが実装されています。

注: iOS シミュレーターでのホスト型アプリ内購入コンテンツのテストはサポートされていないため、実際のデバイスでテストする必要があります。

### <a name="hosted-content-basics"></a>ホストされたコンテンツの基本

IOS 6 より前の製品を提供するには、2つの方法がありました ( [Xamarin のアプリ内購入](~/ios/platform/in-app-purchasing/index.md)ドキュメントで詳細に説明されています)。

- **組み込みの製品**–購入によって "ロックが解除されている" 機能ですが、アプリケーションに組み込まれています (コードとして、または埋め込みリソースとして)。 組み込み製品の例としては、ロックされていない写真フィルターやゲーム中のパワーアップなどがあります。
- **サーバーによって提供**される製品–購入後、アプリケーションは、運用しているサーバーからコンテンツをダウンロードする必要があります。 このコンテンツは、購入時にダウンロードされ、デバイスに保存され、製品の提供の一部としてレンダリングされます。 例としては、書籍、雑誌の問題、またはバックグラウンドアートと構成ファイルで構成されるゲームレベルがあります。

IOS 6 では、サーバーによって配信される製品が用意されています。これらの製品は、サーバー上でコンテンツファイルをホストします。 これにより、サーバーによって提供される製品を簡単に作成できるようになります。これは、別のサーバーを操作する必要がなく、ストアキットによって、以前に作成したバックグラウンドダウンロード機能が提供されるためです。 Apple のホスティングを利用するには、新しいアプリ内購入製品のコンテンツホスティングを有効にし、ストアキットコードを変更して活用します。 製品コンテンツファイルは Xcode を使用してビルドされ、確認とリリースのために Apple のサーバーにアップロードされます。

[![](changes-to-storekit-images/image4.png "The build and deliver process")](changes-to-storekit-images/image4.png#lightbox)

アプリストアを使用して、ホストされ*たコンテンツを*使用してアプリ内購入を提供するには、次の設定と構成が必要です。

- **ITunes Connect** –お客様に代わって収集された資金を送金できるように、Apple に銀行と税金の情報を提供する*必要があり*ます。 その後、販売する製品を構成し、購入をテストするためのサンドボックスユーザーアカウントを設定できます。  _Apple でホストする非消費製品用にホストされたコンテンツを構成する必要もあり_ます。
- **IOS プロビジョニングポータル**–バンドル id を作成し、アプリへのアプリストアアクセスを有効にします。アプリ内購入をサポートするアプリケーションの場合と同様です。
- **ストアキット**–製品を表示し、製品を購入し、トランザクションを復元するためのコードをアプリに追加します。  _また、iOS 6 ストアキットでは、製品コンテンツのダウンロードをバックグラウンドで管理し、進行状況を更新することもできます。_
- **カスタムコード**–顧客によって行われた購入を追跡し、購入した製品またはサービスを提供します。 `SKDownload` などの新しい iOS 6 ストアキットクラスを使用して、Apple によってホストされているコンテンツを取得します。

以下のセクションでは、この記事のサンプルコードを使用して、パッケージの作成とアップロードから購入プロセスとダウンロードプロセスの管理まで、ホストされたコンテンツを実装する方法について説明します。

### <a name="sample-code"></a>サンプル コード

サンプルプロジェクト*HostedNonConsumables* (StoreKitiOS6 内) は、ホストされたコンテンツを使用します。 このアプリでは、2つの "書籍の章" を販売用に提供しています。これは、が Apple のサーバーでホストされているコンテンツです。 コンテンツはテキストファイルとイメージで構成されていますが、実際のアプリケーションでは、より複雑なコンテンツを使用することもできます。

アプリは次のようになります。購入の前後、購入後:

 [![](changes-to-storekit-images/image5.png "The app looks like this before, during and after a purchase")](changes-to-storekit-images/image5.png#lightbox)

テキストファイルとイメージがダウンロードされ、アプリケーションの Documents ディレクトリにコピーされます。 アプリケーションストレージで使用できるさまざまなディレクトリの詳細については、[ファイルシステムのドキュメント](~/ios/app-fundamentals/file-system.md)を参照してください。

## <a name="itunes-connect"></a>iTunes Connect

Apple のコンテンツホスティングを使用する新しい製品を作成する場合は、使用**不可**の製品の種類を選択してください。 その他の製品の種類では、コンテンツホスティングはサポートされていません。 また、販売する*既存*の製品のコンテンツホスティングを有効にしないでください。新しい製品のコンテンツホスティングを有効にするだけです。

 [![](changes-to-storekit-images/image6.png "Select the Non-Consumable product type")](changes-to-storekit-images/image6.png#lightbox)

**製品 ID**を入力してください。 この ID は、後でこの製品のコンテンツを作成するときに必要になります。

 [![](changes-to-storekit-images/image7.png "Enter a Product ID")](changes-to-storekit-images/image7.png#lightbox)

コンテンツホスティングは詳細セクションで設定されます。 アプリ内購入が有効になる前に、キャンセルする場合は **[Apple でホストコンテンツ]** チェックボックスをオフにします (テストコンテンツをアップロードした場合でも)。 ただし、アプリ内購入が有効になった後にコンテンツホスティングを削除することはできません。

 [![](changes-to-storekit-images/image8.png "Hosting content with Apple")](changes-to-storekit-images/image8.png#lightbox)

コンテンツのホスティングを有効にすると、製品はアップロード状態を待機して次**の**メッセージを表示します。

 [![](changes-to-storekit-images/image9.png "The product will enter Waiting for Upload status and show this message")](changes-to-storekit-images/image9.png#lightbox)

コンテンツパッケージは、Xcode で作成し、Archive ツールを使用してアップロードする必要があります。 コンテンツパッケージを作成する手順については、次の「作成」を参照**してください。PKG ファイル**。

## <a name="creating-pkg-files"></a>,.PKG ファイル

Apple にアップロードするコンテンツファイルは、次の制限を満たしている必要があります。

- サイズが 2 GB を超えることはできません。
- 実行可能コード (または、コンテンツの外部をポイントするシンボリックリンク) を含めることはできません。
- は、適切な形式 ( **plist**ファイルを含む) であり、 **.pkg**ファイル拡張子を持つ必要があります。 これは、Xcode を使用してこれらの手順に従うと、自動的に実行されます。

これらの制限が満たされていれば、さまざまなファイルやファイルの種類を追加できます。 コンテンツは、アプリケーションに配信される前に圧縮され、コードにアクセスする前にストアキットによって解凍されます。

コンテンツパッケージをアップロードすると、新しいコンテンツに置き換えることができます。 通常のプロセスを使用して、新しいコンテンツをアップロードし、レビューと承認のために送信する必要があります。 更新されたコンテンツパッケージの `ContentVersion` フィールドをインクリメントして、新しいものであることを示します。

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode のアプリ内購入コンテンツプロジェクト

アプリ内購入製品のコンテンツパッケージを作成するには、現在 Xcode が必要です。 目的 C のコーディングは必要ありません。Xcode には、ファイルと plist を含むパッケージの新しいプロジェクトの種類があります。

サンプルアプリケーションには販売に関する書籍の章があります。各章コンテンツパッケージには次のものが含まれます。

- テキストファイル
- チャプターを表すイメージ。

まず、メニューから **[ファイル > 新しいプロジェクト]** を選択し、 **[アプリ内購入コンテンツ]** を選択します。

 [![](changes-to-storekit-images/image10.png "Choose In-App Purchase Content")](changes-to-storekit-images/image10.png#lightbox)

**[製品名]** と **[会社識別子**] を入力します。これにより、**バンドル識別子**は [この製品の iTunes Connect] に入力した**製品 ID**と一致します。

[![](changes-to-storekit-images/image11.png "Enter the  Name and Identifier")](changes-to-storekit-images/image11.png#lightbox)

これで、**アプリ内購入コンテンツ**プロジェクトが空になります。 右クリックして [ファイル] を**追加**することができます。 または、**プロジェクトナビゲーター**にドラッグします。 **Contentversion**が正しいことを確認します (1.0 から開始する必要がありますが、後でコンテンツを更新することを選択した場合は、必ず増やしてください)。

このスクリーンショットは、プロジェクトに含まれているコンテンツファイルと、メインウィンドウに表示される plist エントリを含む Xcode を示しています。

[![](changes-to-storekit-images/image12.png "This screenshot shows Xcode with the content files included in the project and the plist entries visible in the main window")](changes-to-storekit-images/image12.png#lightbox)

すべてのコンテンツファイルを追加したら、このプロジェクトを保存して後でもう一度編集したり、アップロードプロセスを開始したりすることができます。

## <a name="uploading-pkg-files"></a>アップロード.PKG ファイル

コンテンツパッケージをアップロードする最も簡単な方法は、 **Xcode Archive ツール**を使用することです。 メニューから **[Product > Archive]** を選択して開始します。

![](changes-to-storekit-images/image13.png "Choose Archiven")

次に示すように、コンテンツパッケージはアーカイブに表示されます。
アーカイブの種類とアイコンこの行は、**アプリ内購入コンテンツアーカイブ**です。 **[検証]** をクリックします。 アップロードを実際に実行しなくても、コンテンツパッケージにエラーがないかどうかを確認します。

[![](changes-to-storekit-images/image14.png "Validate the package")](changes-to-storekit-images/image14.png#lightbox)

ITunes Connect 資格情報でログインします。

[![](changes-to-storekit-images/image15.png "Login with your iTunes Connect credentials")](changes-to-storekit-images/image15.png#lightbox)

適切なアプリケーションとアプリ内購入を選択して、このコンテンツを関連付けます。

[![](changes-to-storekit-images/image16.png "Choose the correct application and in-app purchase to associate this content with")](changes-to-storekit-images/image16.png#lightbox)

次のスクリーンショットのようなメッセージが表示できます。

![問題のないメッセージの例](changes-to-storekit-images/image17.png "問題のないメッセージの例")

次に同様のプロセスを実行しますが、 **[配布]** をクリックします。 は実際にコンテンツをアップロードします。

[![アプリを配布する](changes-to-storekit-images/image18.png "アプリを配布する")](changes-to-storekit-images/image18.png#lightbox)

コンテンツをアップロードするには、最初のオプションを選択します。

![コンテンツをアップロードする](changes-to-storekit-images/image19.png "コンテンツをアップロードする")

もう一度サインインします。

[![](changes-to-storekit-images/image15.png "Login in")](changes-to-storekit-images/image15.png#lightbox)

適切なアプリケーションとアプリ内購入レコードを選択して、コンテンツをアップロードします。

[![](changes-to-storekit-images/image20.png "Choose the application and in-app purchase record")](changes-to-storekit-images/image20.png#lightbox)

ファイルがアップロードされるまでお待ちください:

[![](changes-to-storekit-images/image21.png "The content upload dialog")](changes-to-storekit-images/image21.png#lightbox)

アップロードが完了すると、アプリストアにコンテンツが送信されたことを知らせるメッセージが表示されます。

[![](changes-to-storekit-images/image22.png "An example successful upload message")](changes-to-storekit-images/image22.png#lightbox)

この処理が完了すると、iTunes Connect の製品ページに戻ると、パッケージの詳細が表示され、状態を**送信する準備が整い**ます。 製品がこの状態である場合は、サンドボックス環境でテストを開始できます。 サンドボックスでテストのために製品を "送信" する必要はありません。

[![](changes-to-storekit-images/image23.png "iTunes Connect it will show the package details and be in Ready to Submit status")](changes-to-storekit-images/image23.png#lightbox)

しばらく時間がかかることがあります (例 数分)。アーカイブをアップロードしてから、iTunes Connect ステータスを更新します。 レビュー用に製品を個別に送信することも、アプリケーションバイナリと共に送信することもできます。 Apple が公式に承認されている場合にのみ、コンテンツはアプリで購入するために実稼働アプリストアで使用できるようになります。

### <a name="pkg-file-format"></a>PKG ファイル形式

Xcode と Archive ツールを使用してホストされたコンテンツパッケージを作成およびアップロードすると、パッケージ自体の内容が表示されることはありません。 サンプルアプリ用に作成されたパッケージ内のファイルとディレクトリは、次のスクリーンショットのようになります。 **plist**ファイルがルートにあり、product ファイルが**Contents**サブディレクトリにあります。

[![](changes-to-storekit-images/image24.png "The plist file in the root and the product files in a Contents subdirectory")](changes-to-storekit-images/image24.png#lightbox)

パッケージのディレクトリ構造 (特に、`Contents` サブディレクトリ内のファイルの場所) に注意してください。これは、デバイス上のパッケージからファイルを抽出するために、この情報を理解する必要があるためです。

### <a name="updating-package-content"></a>パッケージコンテンツを更新しています

承認後にコンテンツを更新する手順は次のとおりです。

- Xcode で、アプリ内購入コンテンツプロジェクトを編集します。
- バンプバージョン番号。
- ITunes にアップロードして再接続します。 後続の購入者は最新バージョンを自動的に取得しますが、以前のバージョンを既に所有しているユーザーには通知が送信されません。
- アプリは、ユーザーに通知し、新しいバージョンのコンテンツを取得するように促す役割を担います。 また、ストアキットの復元機能を使用して、新しいバージョンをダウンロードする関数を作成する必要もあります。
- 新しいバージョンが存在するかどうかを判断するには、アプリに機能をビルドして、SKProducts をフェッチします (例として、 製品の価格を取得するために使用したのと同じプロセスで、ContentVersion プロパティを比較します。

## <a name="purchasing-overview"></a>購入の概要

このセクションを読む前に、既存[のアプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)を確認してください。

次の図に、ホストされたコンテンツを含む製品を購入してダウンロードするときに発生する一連のイベントを示します。

[![](changes-to-storekit-images/image25.png "The sequence of events that occurs when a product with hosted content is purchased and download")](changes-to-storekit-images/image25.png#lightbox)

1. 新しい製品は、ホストされているコンテンツが有効になっている iTunes Connect で作成できます。 実際のコンテンツは、(ファイルをフォルダーにドラッグするだけで) Xcode で個別に作成され、アーカイブされて iTunes にアップロードされます (コーディングは必要ありません)。 各製品は承認のために送信され、その後、購入できるようになります。 このサンプルコードでは、これらの製品 Id はハードコーディングされていますが、新しい製品やコンテンツを iTunes Connect に送信するときに更新できるように、使用可能な製品リストをリモートサーバーに保存すると、Apple でコンテンツをホストすることがより柔軟になります。
1. ユーザーが製品を購入すると、トランザクションが処理のために支払キューに配置されます。
1. ストアキットは、処理のために購入要求を iTunes サーバーに転送します。
1. トランザクションは、iTunes サーバーで完了します (例 お客様には課金されます) と領収書がアプリに返されます。製品情報は、ダウンロード可能かどうか (その場合はファイルサイズやその他のメタデータ) が含まれます。
1. コードで製品がダウンロード可能かどうかを確認し、必要な場合は、コンテンツのダウンロード要求を行う必要があります。これは、支払いキューにも配置されます。 この要求は、ストアキットによって iTunes サーバーに送信されます。
1. サーバーからストアキットにコンテンツファイルが返されます。これにより、ダウンロードの進行状況と残り時間の見積もりをコードに返すコールバックが提供されます。
1. 完了すると、通知が表示され、キャッシュフォルダー内のファイルの場所が渡されます。
1. コードでファイルをコピーして確認し、製品が購入されたことを覚えておく必要がある状態を保存する必要があります。 この機会に、新しいファイルでバックアップフラグを正しく設定します (ヒント: サーバーからのもので、ユーザーによって編集されていない場合は、ユーザーが将来 Apple のサーバーから常にそれらを取得できるため、バックアップをスキップする必要があります)。
1. Finish トランザクションを呼び出します。 この手順は、支払いキューからトランザクションを削除するため重要です。 キャッシュディレクトリからコンテンツをコピーした後は、終了トランザクションを呼び出さないようにすることも重要です。 Finish Transaction を呼び出すと、キャッシュされたファイルがすぐに削除される可能性があります。

## <a name="implementing-hosted-content-purchase"></a>ホストされたコンテンツの購入の実装

次の情報は、[アプリ内購入ドキュメント](~/ios/platform/in-app-purchasing/index.md)と併せて読む必要があります。 このドキュメントの情報は、ホストされたコンテンツと以前の実装の違いに焦点を当てています。

### <a name="classes"></a>クラス

IOS 6 でホストされるコンテンツをサポートするために、次のクラスが追加または変更されています。

- **Skdownload** –進行中のダウンロードを表す新しいクラス。 API では、製品ごとに複数のを使用できますが、初期状態で実装されているのは1つだけです。
- **Skproduct** –追加された新しいプロパティ: `Downloadable`、`ContentVersion`、`ContentLengths` 配列。
- **SKPaymentTransaction** –新しいプロパティが追加されました: `Downloads`には、この製品でホストされているコンテンツがダウンロード可能な場合、`SKDownload` オブジェクトのコレクションが含まれています。
- **SKPaymentQueue** –新しいメソッドが追加されました: `StartDownloads`。 `SKDownload` オブジェクトを使用してこのメソッドを呼び出し、ホストされたコンテンツをフェッチします。 ダウンロードはバックグラウンドで実行できます。
- **SKPaymentTransactionObserver** –新しいメソッド: `UpdateDownloads`。 ストアキットは、現在のダウンロード操作に関する進行状況の情報を使用して、このメソッドを呼び出します。

新しい `SKDownload` クラスの詳細:

- **[進行状況]** –ユーザーにパーセント入力のインジケーターを表示するために使用できる0-1 の間の値。 ダウンロードが完了したかどうかを検出するために Progress = = 1 を使用しないでください。状態が [完了] になっていることを確認します。
- **TimeRemaining** –ダウンロード時間の推定値 (秒単位)。 -1 は、推定値を計算していることを意味します。
- **状態**–アクティブ、待機中、完了、失敗、一時停止、取り消し済み。
- **Contenturl** – `Cache` ディレクトリ内の、コンテンツがディスク上に配置されたファイルの場所です。 ダウンロードの完了後にのみ設定されます。
- **エラー** –状態が Failed の場合は、このプロパティをオンにします。

この図には、サンプルコードのクラス間の対話が示されています (ホストされているコンテンツの購入に固有のコードは緑で示されています)。

[![](changes-to-storekit-images/image26.png "Hosted content purchases is shown in green in this diagram")](changes-to-storekit-images/image26.png#lightbox)

これらのクラスが使用されているサンプルコードは、このセクションの残りの部分で示しています。

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

既存の `UpdatedTransactions` 上書きを変更して、ダウンロード可能なコンテンツを確認し、必要に応じて `StartDownloads` を呼び出します。

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

新しいオーバーライドされたメソッド `UpdatedDownloads` を次に示します。 `UpdatedTransactions`で `StartDownloads` がトリガーされた後、ストアキットはこのメソッドを呼び出します。 このメソッドは、ダウンロードの進行状況を提供し、ダウンロードが終了したときに再度実行するために、不規則な間隔で*複数回*呼び出されます。 メソッドは `SKDownload` オブジェクトの配列を受け取ることに注意してください。そのため、各メソッド呼び出しで、キュー内の複数のダウンロードの状態を提供できます。 以下の実装に示すように、ダウンロードの状態は毎回チェックされ、適切なアクションが実行されます。

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

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (Sk$ Requestdelegate)

このクラスには、各ダウンロードが正常に完了した後に呼び出される `SaveDownload` 新しいメソッドが含まれています。

ホストされているコンテンツが正常にダウンロードされ、`Cache` ディレクトリに解凍されました。 の構造体。PKG ファイルでは、すべてのファイルが `Contents` サブディレクトリに保存されている必要があるため、以下のコードでは `Contents` サブディレクトリ内からファイルを抽出します。

このコードは、コンテンツパッケージ内のすべてのファイルを反復処理し、`ProductIdentifier`のという名前のサブフォルダー内の `Documents` ディレクトリにコピーします。 最後に、`CompleteTransaction`を呼び出します。これにより `FinishTransaction` が呼び出され、支払いキューからトランザクションが削除されます。

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

`FinishTransaction` が呼び出されると、ダウンロードされたファイルは `Cache` ディレクトリに存在する保証がなくなります。 `FinishTransaction`を呼び出す前に、すべてのファイルをコピーする必要があります。

## <a name="other-considerations"></a>その他の考慮事項

上記のコード例は、ホストされたコンテンツを購入するための非常に単純な実装を示しています。 考慮する必要がある追加のポイントがいくつかあります。

### <a name="detecting-updated-content"></a>更新されたコンテンツの検出

ホストされているコンテンツパッケージを更新することは可能ですが、ストアキットには、製品を既にダウンロードして購入しているユーザーにこれらの更新をプッシュするメカニズムは用意されていません。 この機能を実装するには、コードで新しい `SKProduct.ContentVersion` プロパティ (`SKProduct` が `Downloadable`) を定期的にチェックし、値がインクリメントされているかどうかを検出します。 または、プッシュ通知システムを構築することもできます。

### <a name="installing-updated-content-versions"></a>更新されたコンテンツバージョンのインストール

上記のサンプルコードは、ファイルが既に存在する場合、ファイルのコピーをスキップします。 ダウンロードするコンテンツの新しいバージョンをサポートする場合は、この方法をお勧めしません。

別の方法として、コンテンツをバージョンのという名前のフォルダーにコピーし、現在のバージョンを追跡することができます (例として、 `NSUserDefaults` または完了した購入記録を格納している場所)。

### <a name="restoring-transactions"></a>トランザクションの復元

`SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` が呼び出されると、ストアキットによって、ユーザーの前のすべてのトランザクションが返されます。 多数のアイテムを購入した場合、または各購入に大きなコンテンツパッケージが含まれている場合は、すべてのファイルが一度にキューに入れられるため、復元によって大量のネットワークトラフィックが発生する可能性があります。

製品が、関連付けられているコンテンツパッケージの実際のダウンロードとは別に購入されているかどうかを追跡することを検討してください。

### <a name="pausing-restarting-and-canceling-downloads"></a>ダウンロードの一時停止、再開、キャンセル

このサンプルコードではこの機能については説明していませんが、ホストされているコンテンツのダウンロードを一時停止して再起動することができます。 `SKPaymentQueue.DefaultQueue` には、`PauseDownloads`、`ResumeDownloads`、および `CancelDownloads`のためのメソッドがあります。

ダウンロードが `Finished` される前に、コードが支払キューで `FinishTransaction` を呼び出すと、ダウンロードは自動的に取り消されます。

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>ダウンロードしたコンテンツに対してスキップバックアップフラグを設定する

Apple の iCloud バックアップのガイドラインでは、サーバーから簡単に復元できる非ユーザーコンテンツ (iCloud 記憶域を不必要に使用するため) をバックアップしないようにすることを推奨して*い*ます。 Backup 属性の設定の詳細については、[ファイルシステム](~/ios/app-fundamentals/file-system.md)のドキュメントを参照してください。

## <a name="summary"></a>まとめ

この記事では、iOS6 のストアキットの2つの新機能を紹介しました。アプリ内から iTunes とその他のコンテンツを購入し、Apple のサーバーを利用して独自のアプリ内購入をホストします。 この概要については、既存[のアプリ内購入](~/ios/platform/in-app-purchasing/index.md)に関するドキュメントと共に、ストアキットの機能の実装の詳細について説明します。

## <a name="related-links"></a>関連リンク

- [StoreKit (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit)
- [アプリ内購入](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit フレームワークリファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController クラスのリファレンス](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC ビデオ: 店舗キットを使用した製品の販売](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
