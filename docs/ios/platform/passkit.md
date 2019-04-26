---
title: Xamarin.iOS で PassKit
description: ウォレット アプリにより、iOS ユーザーが自分のデバイスでデジタルのパスを格納します。 PassKit framework では、プログラムのパスを操作できます。
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2018
ms.openlocfilehash: d1c640bef41e875b3bb427d657c9c239e4c3e16d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61192679"
---
# <a name="passkit-in-xamarinios"></a>Xamarin.iOS で PassKit

IOS のウォレット アプリでは、自分のデバイスでデジタルのパスを格納することができます。
これらのパスは加盟店によって生成され、電子メール、Url、またはマーチャントの iOS アプリを使って、顧客に送信します。 これらのパスは、ムービーのチケットに搭乗券、ロイヤルティ カードをから、さまざまな項目を表すことができます。 PassKit framework では、プログラムのパスを操作できます。

このドキュメントでは、ウォレット、PassKit API を使用して Xamarin.iOS でが導入されています。

 [![](passkit-images/image1.png "Wallet を格納し、スマート フォン上のすべてのパスを整理")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>必要条件

このドキュメントで説明した PassKit 機能では、iOS 6 および Xamarin.iOS 6.0 と共に、Xcode 4.5 が必要です。

## <a name="introduction"></a>はじめに

PassKit を解決する重要な問題には、配布とバーコードの管理です。 バーコードの現在の使用方法のいくつかの実際の例は次のとおりです。

-   **映画チケット オンライン購入**–、チケットを表すバーコードが通常の顧客にメールで送信されます。 このバーコードが出力され、エントリのスキャン、映画を取得します。
-   **ロイヤルティ カード**– 顧客数、ウォレットまたは財布内でさまざまなストア固有のカードの実行、表示したり、スキャンするときに商品を購入します。
-   **クーポン**– クーポンは、電子メール、web ページの印刷可能なレター ボックスを通じて、新聞や雑誌のバーコードとしてとして配布されます。 顧客は、商品やサービスの割引が表示される代わりに、スキャンするためのストアに置きます。
-   **パスのボード**– と同様に、ムービーのチケットを購入します。

PassKit は、これらのシナリオごとの代替を提供します。

-   **邀**– 顧客、購入後 (電子メールまたは web サイトのリンク) を使って、イベントのチケット パスを追加します。 時間、ムービーの方法として、パスは、アラームとしてロック画面に自動的に表示されますおよび映画の到着時にパスは、簡単に取得され、スキャンのウォレットに表示されます。
-   **ロイヤルティ カード**– なく (またはに加えて) 物理カードを提供するには、ストアを発行できます (電子メールまたは web サイトのログイン後)、ストア カードのパス。 ストアは、プッシュ通知を使用して、パス上のアカウントの残高の更新などの追加機能を提供でき、地理的位置情報サービスを使用して、パスでした自動的に表示、ロック画面に、顧客がストアの場所の近くにある場合。
-   **クーポン**– クーポンのパスは簡単に固有の特性の追跡のヘルプを使用して生成された、電子メールまたは web サイト リンク経由で配布されます。 クーポンをダウンロードしたは、ユーザーが特定の場所の近くまたは特定の日付 (有効期限が近づいている場合) などの場合に自動的にロック画面に表示できます。 ユーザーの電話番号には、クーポンが格納されているため、便利なは常にあり、紛失しない操作を行います。 クーポンは App Store のリンクは、顧客とのエンゲージメントを増やすと、パスに組み込むことがあるため、コンパニオン アプリをダウンロードいただく場合があります。
-   **パスのボード**– オンラインのでは、チェックイン プロセスの後に、顧客が電子メールまたはリンクを使用して、搭乗を受け取るとします。 トランスポート プロバイダーによって提供されるコンパニオン アプリでは、チェックイン プロセスでは、し、顧客のシートまたは食事を選択するような追加機能を実行することもできます。 トランスポート プロバイダーでは、プッシュ通知を使用して、トランスポートの遅延またはキャンセルされた場合、パスを更新します。 乗車時間のアプローチとして、パスは、アラームとして、パスにすばやくアクセスを提供する、ロック画面に表示されます。

基本的には、PassKit 格納および iOS デバイスでバーコードを表示する簡単で便利な方法を提供します。 さらに時間と場所のロック画面の統合により、プッシュ通知とコンパニオン アプリケーション統合、非常に高度な販売の基盤を提供発券、およびサービスを課金します。

## <a name="passkit-ecosystem"></a>PassKit のエコシステム

PassKit は CocoaTouch 内 API だけではありませんではなくアプリ、データとサービスをセキュリティで保護された共有を容易にしてバーコードの管理、およびその他のデータの大規模なエコシステムの一部になります。 この概要図では、パスの作成とは別のエンティティを示しています。

 [![](passkit-images/image2.png "この高レベルの図は表示を作成すると、パスを使用して、エンティティを関連します。")](passkit-images/image2.png#lightbox)

エコシステムの各部分では、明確に定義されたロールがあります。

-   **ウォレット**– Apple の組み込みの iOS のアプリを格納し、パスが表示されます。 これは、実際に (つまり、バーコードが表示され、パス内のすべてのローカライズされたデータ) で使用するためのパスが表示される唯一の場所です。
-   **コンパニオン アプリ**– によってビルドされた iOS 6 アプリ、ストア カード、搭乗券またはその他のビジネス固有の関数でシートを変更する値を追加するなど、発行するパスの機能を拡張するプロバイダーを渡します。 コンパニオン アプリを使用するパスの必要はありません。
-   **サーバー** – セキュリティで保護されたサーバー パスの生成し、配布の署名をことができます。 コンパニオン アプリは、新しいパスを生成または既存のパスに更新を要求するようにサーバーに接続できます。 必要に応じて、web サービス API を更新するウォレットを呼び出すを渡しますを実装することができます。
-   **APNS サーバー** – サーバーに APNS を使用して指定されたデバイス上のパスに更新プログラムのウォレットに通知します。 ウォレット、変更の詳細については、サーバーに接続し、これに通知をプッシュします。 コンパニオン アプリは、この機能の APNS を実装する必要はありません (をリッスンすることができます、 `PKPassLibraryDidChangeNotification` )。
-   **コンジット アプリ**– を直接操作しないアプリケーションが成功 (など、コンパニオン アプリは、次の操作) しますが、パスを認識して、ウォレットに追加することによって、ユーティリティ品質を向上させます。 添付ファイルまたはパスへのリンク、メール クライアント、ソーシャル ネットワーク ブラウザー、およびその他のデータ集計のアプリのすべてが発生します。

エコシステム全体を検索できるようにいくつかのコンポーネントは省略可能、PassKit のずっと単純な実装が可能です。 注目すべきなります複雑で、します。

## <a name="what-is-a-pass"></a>パスとは何ですか。

パスは、チケット、クーポンまたはカードを表すデータのコレクションです。 個人が 1 回の使用を想定している可能性があります (およびしたがってフライトの数と接続クライアントの割り当てなどの詳細が含まれます) または複数の使用トークンのユーザー (割引クーポン) などの任意の数で共有できることがあります。 詳細な説明については、apple の[渡すファイル](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)ドキュメント。

### <a name="types"></a>型

現在、ウォレット アプリ内のパスの上端のレイアウトとによって識別できる種類は、5 つサポートされています。

-  **イベントのチケット**– 小規模半円の素材です。
-   **オンボード パス**-(例: ノッチ側、トランスポートに固有のアイコンを指定することができます バス、電車、飛行機)。
-   **カードを格納**– 丸みのある上、クレジット_カードまたはデビット カードのようにします。
-  **クーポン**上部裂ける – します。
-  **ジェネリック**– ストア カード、角の丸い上と同じです。


5 つのパスの種類が、このスクリーン ショットに表示されます (順番: クーポン、汎用的で、ストア カード、搭乗およびイベントのチケット)。

 [![](passkit-images/image3.png "5 つのパスの種類は次のスクリーン ショットします。")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>ファイルの構造

パスのファイルは ZIP アーカイブを実際には、 **.pkpass**拡張機能をいくつか特定 JSON ファイル (必須)、各種のイメージを含むファイル (省略可能) やローカライズされた文字列 (も省略可能)。

-   **pass.json** – 必須。 パスのすべての情報が含まれています。
-   **manifest.json** – 必須。 SHA1 ハッシュ、署名ファイルを除く、パス内の各ファイルをこのファイル (manifest.json) が含まれています。
-   **署名**– 必須。 署名によって作成された、 `manifest.json` iOS プロビジョニング ポータルで生成された証明書を持つファイル。
-  **logo.png** – 省略可能。
-  **background.png** – 省略可能。
-  **icon.png** – 省略可能。
-  **ローカライズ可能な文字列ファイル**– 省略可能。

パスのファイルのディレクトリ構造を次に示します (これは、ZIP アーカイブの内容です)。

 [![](passkit-images/image4.png "パスのファイルのディレクトリ構造が次に示します")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.json

パスは通常、サーバーで作成されるため、JSON は、形式 – 生成コードがプラットフォームに依存しないサーバーにします。 各パス内の情報の 3 つの主な要素は次のとおりです。

-   **teamIdentifier** – これにより、App Store のアカウントを生成するすべてのパスがリンクされます。 この値は、iOS プロビジョニング ポータルに表示されます。
-   **passTypeIdentifier** – (1 つ以上の型を生成する) 場合に、グループにプロビジョニング ポータルで登録が一緒に渡します。 たとえば、コーヒー ショップが顧客ロイヤルティのクレジットを獲得するを許可するストア カード成功の種類を作成可能性がありますが、個別のクーポンを作成し、割引クーポンを配布する型を渡すことも。 その同じコーヒー ショップ可能性がありますもライブ音楽イベントを保持し、それらのイベントのチケットのパスを発行します。
-   **serialNumber** – この内で一意の文字列`passTypeidentifier`します。 値は、ウォレットに対して非透過的ですが、サーバーと通信するときに、特定のパスを追跡するため重要です。

これの例を以下に、各パス内の他の JSON キーの数が多いがあります。

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>バーコード

だけ 2D の形式がサポートされています。Pdf417 バーコード、アステカ、QR できます。 Apple では、1 次元バーコードがスキャン バックライト スマート フォンの画面に適していないことを要求します。

バーコードの下に表示される代替テキストは省略可能 – 一部の加盟店が読み取り/型を手動でにできるようにします。

ISO 8859-1 のエンコーディングは、パスを読み取るするスキャンのシステムで使用されるエンコーディング、最も一般的なチェックです。

### <a name="relevancy-lock-screen"></a>関連性 (画面のロック)

ロック画面に表示されるパスを引き起こす可能性のあるデータの 2 種類あります。

 **場所**

例: 顧客が頻繁にアクセスすると、ストアまたは映画や空港の場所のパスでは、最大で 10 個の場所を指定できます。 顧客は、コンパニオン アプリを使用してこれらの場所を設定できます。 またはプロバイダーでしたを特定して使用状況データから (顧客のアクセス許可を持つ収集) 場合。

パスは、ロック画面が表示されたら、フェンスは、パスがロック画面から非表示、ユーザーが、領域を離れるように計算されます。 不正使用を防ぐためにスタイルを渡すには、半径関連付けられます。

 **日付と時刻**

パスでは、1 つだけの日付/時刻を指定できます。 日付と時刻は、搭乗券とイベントのチケットのロック画面通知をトリガーするために便利ですが。

または更新できますプッシュを使用して、PassKit の API を使用して (シアターまたはスポーツ複合のシーズン チケット) などの複数の用途のチケットの場合、日付/時刻を更新できる可能性がありますようにします。

### <a name="localization"></a>ローカリゼーション

IOS アプリケーションのローカライズに似ています、パスを複数の言語に翻訳する – で特定のディレクトリを作成するには、言語、`.lproj`拡張機能内のローカライズされた要素を配置します。 テキスト翻訳を入力する必要があります、`pass.strings`ファイル、ローカライズされたイメージは、パスのルートに置き換え、イメージと同じ名前を持ちます。

## <a name="security"></a>セキュリティ

パスは、iOS プロビジョニング ポータルで生成したプライベート証明書で署名されます。 パスに署名する手順は次のとおりです。

1.  パスのディレクトリ内の各ファイルの SHA1 ハッシュを計算 (含めないでください、`manifest.json`または`signature`ファイル、とにかくこの段階で存在する必要がありますが)。
1.  書き込み`manifest.json`としてそのハッシュを使用して各ファイル名の JSON のキー/値の一覧。
1.  署名証明書を使用して、`manifest.json`ファイルを開き、結果をという名前のファイルに書き込む`signature`します。
1.  すべてを zip 圧縮し、生成されたファイル、`.pkpass`ファイル拡張子。


パスのアクセスを 秘密キーが必要なため、このプロセスを制御するセキュリティで保護されたサーバーでのみ実行する必要があります。 アプリケーションでパスを生成して、キーを配布しないでください。

 
## <a name="configuration-and-setup"></a>構成とセットアップ

このセクションでは、セットアップ、プロビジョニングの詳細や、最初のパスを作成するための手順を説明します。

### <a name="provisioning-passkit"></a>PassKit のプロビジョニング

App Store を入力するパスの順番は、開発者アカウントにリンクする必要があります。 これには、2 つの手順が必要です。

1.  渡す型 ID と呼ばれる、一意の識別子を使用して、パスを登録する必要があります。
1.  開発者のデジタル署名でパスを署名する有効な証明書を生成する必要があります。

次のパス タイプ ID do を作成します。

#### <a name="create-a-pass-type-id"></a>パスの種類の ID を作成します。

それぞれのパスの型 ID を設定するには、まず_型_のサポートされるパス。 渡す ID (または渡す型識別子)、パスの一意の識別子を作成します。 証明書を使用して、開発者アカウントと、パスをリンクするのにこの ID を使用します。

1. [IOS プロビジョニング ポータルの証明書、識別子、およびプロファイル セクション](https://developer.apple.com/account/overview.action)に移動します**識別子**選択**型 Id を渡す**します。 選択し、 **+** 新しい成功の種類を作成するボタン。[![](passkit-images/passid.png "新しいパスの種類を作成します。")](passkit-images/passid.png#lightbox)

2.   提供、**説明**(名) と**識別子**(一意の文字列) のパス。 型 Id を渡すすべてが文字列で始まる必要がありますので注意`pass.`使用してこの例では`pass.com.xamarin.coupon.banana`:[![](passkit-images/register.png "説明と識別子を指定します。")](passkit-images/register.png#lightbox)


3.   キーを押して渡す ID の確認、**登録**ボタンをクリックします。

#### <a name="generate-a-certificate"></a>証明書を生成します。

このパスの型 ID の新しい証明書を作成するには、次の操作を行います。

1.  一覧から、新しく作成された渡す ID を選択し、をクリックして**編集**:[![](passkit-images/pass-done.png "一覧から新しいパスの ID を選択します。")](passkit-images/pass-done.png#lightbox)

    次に、選択**証明書を作成しています.** :

    [![](passkit-images/cert-dist.png "証明書を作成する を選択")](passkit-images/cert-dist.png#lightbox)


2.  証明書署名要求 (CSR) を作成する手順に従います。
  
3. キーを押して、**続行**開発者ポータルでボタンをクリックし、証明書を生成する CSR をアップロードします。

4. 証明書をダウンロードし、ダブルクリックして、キーチェーンにインストールします。


このパスの型 ID の証明書を作成しました、次のセクションには、パスを手動で構築する方法について説明します。

ウォレットのプロビジョニングの詳細についてを参照してください、[機能の使用](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md)ガイド。

### <a name="create-a-pass-manually"></a>パスを手動で作成します。

渡す型を作成しましたので、シミュレーターまたはデバイスでテストするパスを手動で作成できます。 パスを作成する手順は次のとおりです。

-  パスのファイルを格納するディレクトリを作成します。
-  必要なすべてのデータを含む pass.json ファイルを作成します。
-  (必要な場合) フォルダーにイメージが含まれます。
-  フォルダー内のすべてのファイルの SHA1 ハッシュを計算し、manifest.json に書き込みます。
-  ダウンロードした証明書の .p12 ファイルを manifest.json に署名します。
-  ディレクトリの内容を zip 圧縮し、.pkpass 拡張子の名前を変更します。


いくつかのソース ファイルがある、[サンプル コード](https://developer.xamarin.com/samples/monotouch/PassKit/)今回は、パスを生成するために使用できます。 内のファイルを使用して、 `CouponBanana.raw` CreateAPassManually ディレクトリのディレクトリ。 次のファイルが存在します。

 [![](passkit-images/image18.png "これらのファイルが存在します。")](passkit-images/image18.png#lightbox)

Pass.json を開き、JSON を編集します。 少なくともを更新する必要があります、`passTypeIdentifier`と`teamIdentifer`Apple 開発者アカウントを一致するようにします。

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

各ファイルのハッシュを計算し、作成、`manifest.json`ファイル。 完了すると次のように表示されます。

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

次にこのパス タイプ ID が生成された証明書 (.p12 ファイル) を使用してこのファイルの署名を生成する必要があります。

#### <a name="signing-on-a-mac"></a>Mac 署名

ダウンロード、**ウォレット シード サポート資料**から、 [Apple ダウンロード](https://developer.apple.com/downloads/index.action?name=Passbook)サイト。 使用して、 `signpass` (これはも計算、SHA1 ハッシュを計算し、.pkpass ファイルに出力を ZIP) パスに、フォルダーを有効にするツール。

#### <a name="testing"></a>テスト

(ファイル名を .zip に設定し、それを開く) でこれらのツールの出力を確認する場合は、次のファイルが表示される (の追加に注意してください、`manifest.json`と`signature`ファイル)。

 [![](passkit-images/image19.png "これらのツールの出力を検証")](passkit-images/image19.png#lightbox)

署名済み、zip 形式し (例: ファイルの名前を変更したら `BananaCoupon.pkpass`) をテストするには、シミュレーターにドラッグしますまたは、メールを自分で実際のデバイスで取得することができます。 画面を表示する必要があります**追加**次のように、パス。

 [![](passkit-images/image20.png "パスの画面を追加します。")](passkit-images/image20.png#lightbox)

通常そのプロセスを自動化して、サーバーでパスの手動作成がバック エンド サーバーのサポートを必要としないクーポンを作成するだけの小規模企業の選択肢になります。

## <a name="wallet"></a>ウォレット

ウォレットは、PassKit エコシステムの主要な部分です。 このスクリーン ショットは、空のウォレット、およびパスの一覧と個々 のパスを検索する方法を示しています。

 [![](passkit-images/image21.png "このスクリーン ショットは、空のウォレット、およびパスの一覧と個々 のパスを検索する方法を示しています。")](passkit-images/image21.png#lightbox)

ウォレットの機能は次のとおりです。

-  パスは、バーコードをスキャンするために表示される唯一の場所になります。
-  ユーザーは、更新プログラムの設定を変更できます。 有効な場合、プッシュ通知は、パス内のデータの更新をトリガーできます。
-  ユーザーでは、有効にしたり、ロック画面の統合を無効にすることができます。 有効な場合、これにより、パスに埋め込まれている時刻と場所の関連するデータに基づいて、ロック画面に自動的に表示されるパスです。
-  パスの裏面は、web サーバー URL が JSON パスで指定した場合にプルして更新をサポートします。
-  コンパニオン アプリを開く (したりダウンロード)、アプリの ID が JSON パスで指定した場合。
-  パスは、(かわいらしい細分化アニメーション) を削除できます。

## <a name="adding-passes-into-wallet"></a>ウォレットにパスを追加します。

パスは、次の方法でウォレットに追加できます。

* **コンジット アプリ**– パスを直接操作しないこれらを単にファイルのパスをロードし、ウォレットに追加するオプションをユーザーに表示します。 

* **コンパニオン アプリ**– これらは、パスを配布しを参照するか、編集して追加の機能を提供するプロバイダーによって書き込まれます。 Xamarin.iOS アプリケーションを作成し、パスを操作するには、PassKit API に完全にアクセスがあります。 パスを追加することができますし、ウォレットを使用する、`PKAddPassesViewController`します。 このプロセスがで詳しく説明されている、**コンパニオン アプリケーション**このドキュメントの「します。

### <a name="conduit-applications"></a>コンジット アプリケーション

コンジット アプリケーションには、中間のアプリ、ユーザーに代わってパスが表示される可能性があり、そのコンテンツの種類を認識し、ウォレットに追加する機能を提供するようにプログラミングする必要がありますですが。 コンジット アプリの例は次のとおりです。

-   **メール**– 添付ファイル、パスとして認識します。
-   **Safari** – パスの URL リンクがクリックされたときに、パスのコンテンツの種類を認識します。
-   **その他のカスタム アプリ**– 添付ファイルを受信または (クライアントのソーシャル メディア、電子メール リーダーなど) へのリンクを開いているすべてのアプリ。


このスクリーン ショットは方法**メール**6 がパスの添付ファイルを認識する iOS (タッチ) する場合とは、提供**追加**ウォレットに。

 [![](passkit-images/image22.png "このスクリーン ショットは、iOS 6 でメールがパスの添付ファイルを認識する方法を示しています。")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "このスクリーン ショットは、ウォレットにパスの添付ファイルを追加するメールを提供する方法を示しています。")](passkit-images/image23.png#lightbox)

パスの可能性があるアプリを構築する場合で認識されないことができます。

-  **ファイル拡張子**-.pkpass
-  **MIME の種類**-application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


パスのファイルを取得し、呼び出す PassKit のコンジット アプリケーションの基本的な操作は、`PKAddPassesViewController`ウォレットにパスを追加するオプションをユーザーに提供します。 このビュー コント ローラーの実装については、次のセクションで**コンパニオン アプリケーション**します。

コンジット アプリケーションは、特定のパスの種類 ID コンパニオン アプリケーションを実行するのと同じ方法でプロビジョニングする必要はありません。

## <a name="companion-applications"></a>コンパニオン アプリケーション

コンパニオン アプリケーションは、パスを作成するパスに関連付けられている情報を更新して、それ以外の場合、アプリケーションに関連付けられたパスを管理するなど、パスを操作するための追加の機能を提供します。

コンパニオン アプリケーションしようとしないでくださいウォレットの機能が重複しています。 スキャンするためのパスを表示するのにはしていません。

このセクションの残りの部分では、基本的なコンパニオン アプリと対話する PassKit で構築する方法について説明します。

### <a name="provisioning"></a>プロビジョニング

ウォレットは、ストア テクノロジであるため、アプリケーションが個別にプロビジョニングする必要があるし、チームのプロビジョニング プロファイルまたはワイルドカード アプリ ID を使用することはできません。 参照してください、[機能の使用](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md)ウォレット アプリケーションの一意のアプリ ID とプロビジョニング プロファイルを作成するガイドです。

### <a name="entitlements"></a>権利

**Entitlements.plist**ファイルは、最近使ったすべての Xamarin.iOS プロジェクトに含める必要があります。 次の手順では、新しい Entitlements.plist ファイルを追加する、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)ガイド。

設定する権利は、次を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

ダブルクリックして、 **Entitlements.plist** Entitlements.plist エディターを開く Solution Pad でファイル。

![](passkit-images/image31.png "Entitlements.plst エディター")

[Wallet] セクションでは、選択、 **Wallet を有効にする**オプション

![](passkit-images/image32.png "ウォレットの権利を有効にします。")


既定のオプションでは、アプリがすべてがパスの種類を許可します。 ただし、アプリを制限し、チームのパスの種類のサブセットのみを許可することができます。 この選択を有効にする、**チームのサブセットを許可する型を渡す**のサブセットを許可する pass 型の識別子を入力します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

ダブルクリックして、 **Entitlements.plist**ファイル、XML ソース ファイルを開きます。

ウォレット エンタイトルメントを追加するには、設定、**プロパティ**に`Passbook Identifiers`のドロップダウンで、これは自動的に設定、**型**`Array`します。 文字列を設定し、**値**に`$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "ウォレットの権利を有効にします。")

これにより、アプリはすべてのパスの種類を許可できるようになります。 アプリを制限し、チームのパスの種類のサブセットのみを許可する文字列値を設定します。

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

場所`pass.$(CFBundleIdentifier)`が作成されてパス ID[上](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>デバッグ

アプリケーションのデプロイに問題がある場合、正しいは使用することを確認**プロビジョニング プロファイル**ことと、`Entitlements.plist`として選択されて、**カスタム権利**ファイル**iPhone バンドルの署名**オプション。

展開する場合: このエラーが発生した場合

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

次に、`pass-type-identifiers`権利配列が正しくありません (または一致しない場合、**プロビジョニング プロファイル**)。 型 Id を渡すと、チーム ID が正しいことを確認します。

## <a name="classes"></a>クラス

次の PassKit クラスは、パスにアクセスするアプリに対して使用できます。

-  **PKPass** – パスのインスタンス。
-  **PKPassLibrary** – デバイスのパスにアクセスする API を提供します。
-  **PKAddPassesViewController** – ユーザーが、ウォレットに保存するためのパスを表示するために使用します。
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS の開発者

## <a name="example"></a>例

PassLibrary プロジェクトを参照してください、[サンプル コード](https://developer.xamarin.com/samples/monotouch/PassKit/)この記事でします。 これには、ウォレット コンパニオン アプリケーションで処理するために必要な次の一般的な関数を示しています。

### <a name="check-that-wallet-is-available"></a>ウォレットが利用可能であることを確認します。

ウォレットは、iPad で利用可能なはないため、アプリケーションが PassKit の機能にアクセスする前に確認する必要があります。

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>ライブラリ パス インスタンスを作成します。

PassKit ライブラリはシングルトンではありませんをアプリケーションを作成および格納し、PassKit API にアクセスするインスタンスします。

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>パスの一覧を取得します。

アプリケーションでは、ライブラリからのパスの一覧を要求できます。 この一覧は自動的に、PassKit でフィルター処理、権利でパスを使用するチーム ID が作成されていて、リストされていますだけ見えるようにします。

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

シミュレーターをフィルター処理しない一覧が返されたパスのこのメソッドは、実際のデバイスで常にテストする必要がありますに注意してください。 この一覧は、UITableView で表示できます。 [サンプル アプリ](https://developer.xamarin.com/samples/monotouch/PassKit/)のように 2 つのクーポンを追加した後。

 [![](passkit-images/image29.png "サンプル アプリ ファイルの場所などのこの 2 つのクーポンを追加した後")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>パスを表示します。

限られた情報は、コンパニオン アプリ内でパスの描画に使用できます。

コード例は、パスの一覧を表示する標準的なプロパティには、このセットから選択します。

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

この文字列は、アラートとして表示されます、[サンプル](https://developer.xamarin.com/samples/monotouch/PassKit/):

 [![](passkit-images/image30.png "サンプルのクーポンが選択されているアラート")](passkit-images/image30.png#lightbox)

使用することも、`LocalizedValueForFieldKey()`設計したパス内のフィールドからデータを取得するメソッド (がわかっているのでフィールドが存在する)。 コード例ではこの表示はされません。

### <a name="loading-a-pass-from-a-file"></a>ファイルからのパスの読み込み

パスをウォレットに追加するには、ユーザーのアクセス許可を持つ、ためビュー コント ローラーを決定できるように表示する必要があります。 このコードで使用、**追加**ボタンの例では、(に署名するものに取り替えてください)、アプリに埋め込まれている作成済みパスを読み込めません。

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

パスが表示され**追加**と**キャンセル**オプション。

 [![](passkit-images/image20.png "追加し、[キャンセル] のオプションが表示パス")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>既存のパスを置き換える

既存のパスを置き換える必要はありません、ユーザーのアクセス許可が、パスが存在しない場合は失敗します。

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>パスの編集

PKPass は不変ではないため、コード内のパス オブジェクトを更新することはできません。 パス内のデータを変更するのには、パスのレコードを保持したり、アプリケーションがダウンロードできる更新された値を持つ新しいパス ファイルを生成する web サーバーへのアクセスがアプリケーションに必要です。

パスのファイルの作成には、パスは、プライベートかつ安全に保持する必要がある証明書で署名する必要がありますので、サーバーにする必要があります。

更新されたパス、ファイルが生成されると、使用、`Replace`デバイス上の古いデータを上書きする方法。

### <a name="display-a-pass-for-scanning"></a>スキャンのパスを表示します。

上記のよう、だけでウォレットをスキャンするためのパスを表示できます。 使用して、パスを表示できます、`OpenUrl`メソッド。

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>変更の通知を受け取る

アプリケーションがリッスンできる渡すライブラリに加えられる変更を使用して、`PKPassLibraryDidChangeNotification`します。 バック グラウンドで更新をトリガーするため、アプリでリッスンすることをお勧めの通知では、変更が考えられます。

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

ライブラリのインスタンスを渡す PKPassLibrary はシングルトンではないために、通知を登録するときに重要です。

## <a name="server-processing"></a>サーバーの処理

PassKit をサポートするためにサーバー アプリケーションの構築の詳細については、この入門記事で取り上げませんです。

参照してください[dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook)オープン ソースC#サーバー側コード。

## <a name="push-notifications"></a>プッシュ通知

詳細については、プッシュ通知を使用してパスを更新するのでは、この入門記事で取り上げませんです。

更新プログラムが必要な場合は、ウォレットから web 要求に応答する Apple によって定義されている残りの部分のような API を実装する必要があります。

Apple を参照してください。[パスを更新](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1)詳細ガイド。

## <a name="summary"></a>まとめ

この記事では、PassKit の導入、いくつかの項目が便利な理由を説明し、PassKit の完全なソリューションを実装する必要があるさまざまな部分を説明します。 パスは、のようにするプロセス、パスも、手動で Xamarin.iOS アプリケーションから、PassKit Api にアクセスする方法を作成する Apple Developer アカウントを構成するための手順を説明しました。

## <a name="related-links"></a>関連リンク

- [開発者向けウォレット](https://developer.apple.com/wallet/)
- [PassKit のサンプル](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [ウォレットの開発者ガイド](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [フレームワーク: Apple Pay、およびウォレット (WWDC ビデオ)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [PassKit Framework リファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Web サービス参照の passbook](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [パスのファイルについて](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook)、iOS ウォレット パッケージを生成するため、オープン ソース ライブラリ
- [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)
