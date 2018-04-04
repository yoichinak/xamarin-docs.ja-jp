---
title: PassKit
description: ウォレットを格納し、バーコードと '現実の世界' でそれらの電話に顧客のトランザクションをリンクするその他の情報を表示するシステムの iOS アプリです。
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: f1c8ac92c5ff7eed5116587ed13755ddee74a877
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="passkit"></a>PassKit

_ウォレットを格納し、バーコードと '現実の世界' でそれらの電話に顧客のトランザクションをリンクするその他の情報を表示するシステムの iOS アプリです。_

ウォレットの Iphone アプリは、し、iOS 6 と iPod が触れます。 格納し、バーコードと '現実の世界' でそれらの電話に顧客のトランザクションをリンクするその他の情報が表示されます。 パスはオンライン ショップによって生成され、電子メールで Url または内から、顧客に送信ショップの iOS アプリ。 ウォレットは、格納し、スマート フォン上のすべてのパスを編成し、日付/時刻またはデバイスの位置によってロック画面でパス アラームを表示します。

このドキュメントでは、ウォレット、Xamarin.iOS で渡すキット API の使用を紹介し、サーバー上のパスを実装する方法について説明します。

 [![](passkit-images/image1.png "スマート フォン上のすべてのパスを分類して格納、ウォレット")](passkit-images/image1.png#lightbox)


## <a name="requirements"></a>要件

このドキュメントで説明するストア キット機能は、iOS 6 および Xamarin.iOS 6.0 と共に、Xcode 4.5 が必要です。


## <a name="introduction"></a>はじめに

渡すキットを解決するキーの問題は、配布とバーコードの管理です。 現在のバーコードが使用する方法の実際の例をいくつかは次のとおりです。

-   **ムービーのチケットをオンラインで購入**: 顧客が、チケットを表すバーコード通常電子メールで送信します。 このバーコードを出力し、エントリをスキャンする映画を取得します。
-   **ロイヤルティ カード**– お客様のウォレットか財布にさまざまなストアに固有のカードの数の実行、表示したり、スキャン時に商品を購入します。
-   **クーポン**– クーポンは、電子メールで web ページの印刷可能な letterboxes と新聞や雑誌のバーコードとしてとして配布します。 お客様は、スキャン、または受信する商品、services 割引戻り値のストアに置きます。
-   **パスをボード**– と同様に、ムービーのチケットを購入します。


パスのキットには、これらの各シナリオの代替が提供しています。

-   **ムービー チケット**–、購入後、顧客が (電子メールまたは web サイトのリンク) 経由でのイベント チケット パスを追加します。 ムービーのアプローチの時間、としてなおに、ロック画面上のパスを自動的に表示されます、映画到着するまでのパスは、簡単に取得され、スキャンのウォレットに表示されます。
-   **ロイヤルティ カード**– なく (またはそれらに加えて)、物理カードを提供するには、ストアを発行できます (電子メールまたは web サイトのログイン後) ストア カード パス。 ストアは、プッシュ通知を使用して、パス上のアカウントの残高の更新などの追加機能を提供できる場合は、地理的位置情報サービスを使用して、パスでした自動的に表示ロック画面で、顧客がストアの場所に近いです。
-   **クーポン**– クーポンのパスは簡単に追跡、ヘルプに固有の特性を使用して生成や電子メールまたは web サイト リンク経由で配布します。 ダウンロードしたクーポンはことができます、ユーザーが特定の場所の近くまたは特定の日付 (有効期限が近づいている場合) などのロック画面に自動的に表示します。 クーポンは、ユーザーの電話に保存されているため便利なは常にし、紛失はありません。 クーポンは、アプリ ストアへのリンクは、顧客との連携を増やすと、パスに組み込むことがあるために、コンパニオン アプリをダウンロードしていただく可能性があります。
-   **パスをボード**– オンラインのチェックイン プロセスの後に、お客様は、電子メールまたはリンクを使用して、オンボーディング パスを受信します。 トランスポート プロバイダーによって提供されるコンパニオン アプリ チェックイン プロセスを指定し、接続クライアント ライセンスまたは料理を選択するように追加の機能を実行する顧客を許可することも。 トランスポート プロバイダーは、プッシュ通知を使用して、トランスポートが遅延されるか、取り消された場合、パスを更新することができます。 オンボーディングの時刻として、パスのアプローチは、アラームとは、パスにすばやくアクセスを提供するロック画面に表示されます。


基本的は、渡すキットは、保存して iPhone または iPod touch デバイスでバーコードを表示する簡単で便利な方法を提供します。 余分な時間と場所のロック画面の統合では、プッシュ通知およびコンパニオン アプリケーション統合は非常に高度な sales 用、foundation チケット発行やサービスの課金します。


## <a name="pass-kit-ecosystem"></a>キット エコシステムを渡す

パス キットが CocoaTouch 内の API だけではないアプリ、データとサービスをセキュリティで保護された共有を容易にしてバーコードの管理およびその他のデータの大規模なエコシステムの一部ではなく、 この高レベルの図は、作成すると、パスを使用して、複数のエンティティ関係していることができますを示しています。

 [![](passkit-images/image2.png "この高レベルの図は、エンティティ関係を作成すると、パスを使用して")](passkit-images/image2.png#lightbox)

それぞれのエコシステムでは、明確に定義されたロールがあります。

-   **ウォレット**– Apple の iOS 組み込みアプリ (iPhone と iPod touch) の格納し、パスを表示します。 これは、(つまり、バーコードが表示されます、パス内のすべてのローカライズされたデータと共に)、実際の使用のパスが表示される唯一の場所です。
-   **アプリのコンパニオン**– iOS 6 アプリ ストアのカード、オンボーディング合格したか、他のビジネス固有の関数に、いすの変更に値を追加するなど、発行するパスの機能を拡張するパス プロバイダーによって構築します。 コンパニオン アプリは、パスが役に立つに必要ではありません。
-   **サーバー** : セキュリティで保護されたサーバーのパスを生成および配布するために署名できます。 コンパニオン アプリは、新しいパスを生成または既存のパスに更新を要求するようにサーバーに接続できます。 必要に応じてパスを更新するウォレットを呼び出す web サービス API を実装することができます。
-   **APNS サーバー** –、サーバーは、APNS を使用して特定のデバイス上のパスに更新プログラムのウォレットを通知する権限を持ちます。 定期券サイズ変更の詳細については、サーバーに接続し、これには、通知をプッシュします。 コンパニオン アプリは、この機能の APNS を実装する必要はありません (をリッスンすることができます、 `PKPassLibraryDidChangeNotification` )。
-   **アプリのパイプ**– アプリケーション (ようコンパニオン アプリ) のパスを直接操作しないが、パスを認識およびウォレットに追加することによって、ユーティリティを向上することができます。 メール クライアント、ソーシャル ネットワーク ブラウザーおよび他のデータ集計アプリ事項がありますすべての添付ファイルまたはパスへのリンク。


エコシステム全体は検索もいくつかのコンポーネントは省略可能とはるかに簡単を渡すキット実装があることに注目すべきであるため、複雑なします。

## <a name="what-is-a-pass"></a>パスとは何ですか。

パスは、チケット、クーポンまたはカードを表すデータのコレクションです。 個人によって単一の使用を想定している可能性があります (およびので、航空券数と接続クライアント ライセンスの割り当てなどの詳細が含まれます) または複数の使用トークン割引クーポン) などのユーザーの任意の数で共有できることがあります。 詳細な説明については、Apple ので[渡すファイル](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)ドキュメント。


### <a name="types"></a>種類

現在ウォレット アプリ内のパスの上端のレイアウトとによって識別できる種類は、5 つサポートされています。

-  **イベントのチケット**– 小規模の半素材。
-   **オンボーディング パス**– ノッチ側、トランスポートに固有のアイコンを指定できます。 bus、トレーニング、航空機)。
-   **カードを格納**– 丸みのある上、クレジット_カードやデビット カードのようにします。
-  **クーポン**– 通気上部に配置します。
-  **ジェネリック**– ストア カード、角の丸い上と同じです。


このスクリーン ショットに 5 つのパスの種類を示します (順番: クーポン、ジェネリック、カード、オンボーディング パスおよび保存イベント チケット)。

 [![](passkit-images/image3.png "このスクリーン ショットで 5 つのパスの種類が表示されます。")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>ファイルの構造

パスのファイルが ZIP アーカイブを実際には、 **.pkpass**拡張子、ファイルを含むいくつか特定 JSON (必須)、各種のイメージ ファイル (省略可能) およびローカライズされた文字列 (も省略可能)。

-   **pass.json** -必須です。 パスのすべての情報が含まれています。
-   **manifest.json** -必須です。 シグネチャ ファイルを除く、パス内の各ファイルとファイル (manifest.json) の SHA1 ハッシュが含まれています。
-   **署名**-必須です。 署名によって作成された、 `manifest.json` iOS プロビジョニング ポータルで生成された証明書を持つファイルです。
-  **logo.png** – optional.
-  **background.png** -省略可能です。
-  **icon.png** – optional.
-  **ローカライズ可能な文字列ファイル**-省略可能です。


パスのファイルのディレクトリ構造を次に示します (これは、ZIP アーカイブの内容です)。

 [![](passkit-images/image4.png "パスのファイルのディレクトリ構造を示します")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.json

JSON では、パスは通常、サーバーで作成されるため、形式 – 生成コードがプラットフォームに依存しないサーバーにします。 次の 3 つの重要な情報すべてのパスでは次のとおりです。

-   **teamIdentifier** – これがアプリ ストア アカウントを生成するすべてのパスをリンクします。 この値は、iOS プロビジョニング ポータルに表示されます。
-   **passTypeIdentifier** – (1 つ以上の型を生成する) 場合にグループにプロビジョニング ポータルで登録が一緒に渡します。 たとえば、コーヒー ショップでは、型は、ストア カード渡すロイヤルティ クレジットを獲得する顧客を許可するがも作成し、割引クーポンを配布する別のクーポン渡す型を作成可能性があります。 その同じコーヒー ショップ可能性がありますもライブ音楽のイベントを保持し、それらのイベントのチケットのパスを発行します。
-   **serialNumber** – 内で一意の文字列`passTypeidentifier`です。 値は、ウォレットに対して非透過的ですが、サーバーと通信するときに、特定のパスを追跡するため重要です。


これの例を次に、各パス内の他の JSON キーの数が多いです。

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

2D のみの形式がサポートされています: pdf417 バーコード、アステカ、QR です。 Apple では、1 次元バーコードにバックライト電話の画面にスキャンするのに適したがないことを要求します。

バーコードの下に表示される代替テキストは省略可能 – ショップの一部が読み取り/型を手動でにできるようにします。

ISO 8859-1 のエンコーディングは、スキャンのシステム パスを読み取ることによって使用されるエンコーディングの最も一般的なチェックです。

### <a name="relevancy-lock-screen"></a>妥当性 (ロック画面)

ロック画面に表示されるパスを引き起こす可能性のあるデータの 2 つの種類があります。

 **場所**

たとえば、顧客が頻繁にアクセスすると、ストアまたは映画や空港の場所のパスでは、最大で 10 個の場所を指定できます。 顧客でした設定コンパニオン アプリ経由でこれらの場所またはプロバイダーでした決定に使用状況データから (お客様のアクセス許可を持つ収集) 場合。

ロック画面で、パスが表示されたら、ロック画面のパスを非表示、ユーザーが、領域を離れるとようにフェンスが計算されます。 不正使用を防ぐためにスタイルを渡すには、半径は関連付けられています。

 **日付と時刻**

パスでは、1 つだけの日付/時刻を指定できます。 日付と時刻は、オンボーディング パスおよびイベント チケットに対してロック画面通知をトリガーするために役立ちます。

更新できるプッシュまたは PassKit API を使用してできるように、(シアターまたはスポーツの複合型で繁忙期のチケット) などの複数の用途をチケットの場合、日付/時刻を更新できませんできます。

### <a name="localization"></a>ローカリゼーション

複数の言語にパスの変換は iOS アプリケーションのローカライズに似ています-言語で特定のディレクトリを作成、`.lproj`拡張機能内のローカライズされた要素を配置します。 テキストの翻訳を入力する必要があります、`pass.strings`ファイル中、ローカライズされたイメージは、イメージのパスのルートに置き換わるものと同じ名前を持つ必要があります。

## <a name="security"></a>セキュリティ

パスは、iOS プロビジョニング ポータルで生成したプライベート証明書で署名されます。 パスに署名する手順は次のとおりです。

1.  パスのディレクトリ内の各ファイルの SHA1 ハッシュを計算 (を含めないでください、`manifest.json`または`signature`ファイルをこの段階でか存在するのもする必要があります)。
1.  書き込む`manifest.json`としてそのハッシュを使用して各ファイル名の JSON キー/値の一覧です。
1.  署名する証明書を使用して、`manifest.json`ファイルおよび結果をという名前のファイルに書き込み`signature`です。
1.  すべてのものを zip 圧縮し、結果として得られるファイルに付けます、`.pkpass`ファイル拡張子。


秘密キーが必要なため、パスの署名に、このプロセスはユーザーが制御するセキュリティで保護されたサーバーでのみ行ってください。 アプリケーションでパスを生成して、キーを配布しないでください。

 
## <a name="configuration-and-setup"></a>構成とセットアップ

このセクションでは、プロビジョニングの詳細をセットアップし、最初のパスを作成するために手順を説明します。

### <a name="provisioning-passkit"></a>プロビジョニング PassKit

App Store を入力するパスの順序では、開発者アカウントをリンクする必要があります。 これには、2 つの手順が必要です。

1.  渡す型 ID と呼ばれる、一意の識別子を使用して、パスを登録する必要があります。
1.  開発者のデジタル署名を持つパスの署名に有効な証明書を生成する必要があります。

次のタイプ ID を渡す操作を作成します。


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>パスの種類 ID を作成します。

それぞれに渡す型 ID を設定するには、まず_型_のサポートされるパス。 ID を渡す (または型を渡す識別子) は、パスの一意の識別子を作成します。 この ID は証明書を使用して、開発者アカウントでパスをリンクを使用します。

1. [IOS プロビジョニング ポータルの「証明書識別子、およびプロファイル](https://developer.apple.com/account/overview.action)、に移動**識別子**選択**型 Id を渡す**です。 選択し、 **+**パスの種類を作成するにはボタン: [ ![ ](passkit-images/passid.png "新しいパスの種類を作成します。")](passkit-images/passid.png#lightbox)

2.   提供、**説明**(名) と**識別子**(一意の文字列) のパス。 型 Id を渡すすべてが文字列で始まる必要がありますを`pass.`使用して、この例では`pass.com.xamarin.coupon.banana`: [ ![ ](passkit-images/register.png "説明と識別子を指定")](passkit-images/register.png#lightbox)


3.   キーを押して渡す ID の確認、**登録**ボタンをクリックします。


<a name="generate" />

#### <a name="generate-a-certificate"></a>証明書の生成します。

この渡す型 ID の新しい証明書を作成するには、次の操作を行います。

1.  リストから、新しく作成された渡す ID を選択し、クリックして**編集**: [ ![ ](passkit-images/pass-done.png "渡す新しい ID を一覧から選択")](passkit-images/pass-done.png#lightbox)

    次に、選択**証明書を作成しています.** :

    [![](passkit-images/cert-dist.png "証明書を作成する を選択")](passkit-images/cert-dist.png#lightbox)


2.  証明書署名要求 (CSR) を作成する手順を実行します。
  
3. キーを押して、**続行**開発者ポータルでボタンをクリックし、証明書を生成する CSR をアップロードします。

4. 証明書をダウンロードしをダブルクリックして、キーチェーンにインストールします。


この渡す型 ID を証明書を作成したが、これで、次のセクションは、パスを手動で作成する方法を説明します。

ウォレットのプロビジョニングの詳細についてを参照してください、[機能を備えた作業](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md)ガイドです。

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>パスを手動で作成します。

これで、シミュレーターまたはデバイスでテストするパスを手動で作成できますお渡す型を作成しました。 パスを作成する手順は次のとおりです。

-  パスのファイルを格納するディレクトリを作成します。
-  必要なすべてのデータを含む pass.json ファイルを作成します。
-  (必要な場合) フォルダーにイメージが含まれます。
-  フォルダーで、すべてのファイルの SHA1 ハッシュを計算し、manifest.json を記述します。
-  ダウンロードした証明書の .p12 ファイルで manifest.json を署名します。
-  ディレクトリの内容を zip 圧縮し、.pkpass 拡張子を持つ名前を変更します。


パスを生成するために使用するこの記事のサンプル コードには、一部のソース ファイルがあります。 内のファイルを使用して、 `CouponBanana.raw` CreateAPassManually ディレクトリのディレクトリ。 次のファイルが存在します。

 [![](passkit-images/image18.png "これらのファイルがあります。")](passkit-images/image18.png#lightbox)

Pass.json を開き、JSON を編集します。 更新する必要がありますには、少なくとも、`passTypeIdentifier`と`teamIdentifer`Apple の開発者アカウントを一致するようにします。

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

各ファイルのハッシュを計算し、作成、`manifest.json`ファイル。 次のようになは、完了すると、表示されます。

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

次にこのパスの種類 ID に対して生成された証明書 (.p12 ファイル) を使用してこのファイルの署名を生成する必要があります。

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>Mac 上の署名

ダウンロード、**ウォレット シードのサポート資料**から、 [Apple ダウンロード](https://developer.apple.com/downloads/index.action?name=Passbook)サイトです。 使用して、 `signpass` (これはまた、SHA1 ハッシュと計算 .pkpass ファイルに出力を zip 圧縮) パスに、フォルダーを有効にするツールです。

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>PC の署名

このサンプルが、この記事のコードはというプロジェクト`signpassnet`Windows で .NET で実行されています。 これは、はるかに少ない検証コードが含まれていますが、Apple のツールを模倣するために試行します。

 <a name="Testing" />


#### <a name="testing"></a>テスト中

(.Zip ファイル名を設定し、開くことして) これらのツールの出力を検査する場合は、次のファイルの表示と同じ (の追加に注意してください、`manifest.json`と`signature`ファイル)。

 [![](passkit-images/image19.png "これらのツールの出力を確認します。")](passkit-images/image19.png#lightbox)

署名済み、zip 形式し (ファイルの名前を変更したら、 `BananaCoupon.pkpass`) をテストするには、シミュレーターにドラッグしたり、メールを自分自身を実際のデバイスで取得します。 画面が表示されます**追加**パスには、次のように、します。

 [![](passkit-images/image20.png "パスの画面を追加します。")](passkit-images/image20.png#lightbox)

通常サーバーでは、パスのただし手動作成がバックエンド サーバーのサポートを必要としないクーポンを作成するだけの小規模企業のオプションを選択できます。 そのプロセスを自動化するとします。

 <a name="Wallet" />

## <a name="wallet"></a>ウォレット

サーバーの全体を渡すキット エコシステムのウォレットです。 このスクリーン ショットは、空のウォレットとパスの一覧と個々 のパスを検索する方法を示します。

 [![](passkit-images/image21.png "このスクリーン ショットは、空のウォレットとパスの一覧と個々 のパスを検索する方法を示しています。")](passkit-images/image21.png#lightbox)

ウォレットの機能は次のとおりです。

-  パスをスキャンするためには、そのバーコードをレンダリングする際、唯一の場所であります。
-  ユーザーは、更新プログラムの設定を変更できます。 有効な場合、プッシュ通知は、パス内のデータへの更新をトリガーできます。
-  ユーザーでは、有効にしたり、ロック画面の統合を無効にすることができます。 有効な場合、これにより、ロック画面に自動的に表示すると、パスに基づいて、パスに埋め込まれている関連の時間と場所のデータ。
-  パスの裏面は、渡す JSON に web サーバー URL を指定した場合に、更新するプルをサポートします。
-  コンパニオン アプリを開く (したりダウンロード) を渡す JSON でアプリの ID が指定した場合。
-  (かわいらしいシュレッダ アニメーションの場合) のパスを削除できます。


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>ウォレットにパスを追加します。

パスは、次の方法でウォレットに追加できます。

* **アプリのパイプ**– パスを直接操作しないこれらは、単に読み込むファイルのパスを財布に追加のオプションをユーザーに表示します。 

* **アプリのコンパニオン**– これらはパスを配布しを参照するか、編集して追加の機能を提供するプロバイダーによって書き込まれます。 Xamarin.iOS アプリケーションでは、完全な API へのアクセスを渡すキットを作成し、パスの操作があります。 パスを追加することができますし、ウォレットを使用する、`PKAddPassesViewController`です。 このプロセスがで詳しく説明されている、**コンパニオン アプリケーション**このドキュメントの「します。

### <a name="conduit-applications"></a>コンジット アプリケーション

コンジット アプリケーションは、中級者向けのアプリをユーザーに代わってパスが表示される可能性があり、そのコンテンツの種類を認識し、ウォレットに追加する機能を提供するようにプログラミングする必要があります。 コンジット アプリの例については、次のとおりです。

-   **メール**– パスとして添付ファイルを認識します。
-   **Safari** – 渡す URL リンクがクリックされたときに渡すコンテンツの種類を認識します。
-   **その他のカスタム アプリ**– (ソーシャル メディア クライアント、メール リーダーなど) へのリンクを開いたりする添付ファイルを受信するすべてのアプリです。


このスクリーン ショットに示す方法**メール**iOS 6 を認識してパスの添付ファイル (タッチ) する場合とに提供する**追加**ウォレットにします。

 [![](passkit-images/image22.png "このスクリーン ショットは、6、iOS でのメールがパスの添付ファイルを認識する方法を示しています。")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "このスクリーン ショットは、ウォレットの添付ファイルのパスを追加するメールを提供する方法を示しています。")](passkit-images/image23.png#lightbox)

パスのコンジットの可能性があるアプリを構築する場合は、によって認識されることができます。

-  **ファイル拡張子**-.pkpass
-  **MIME の種類**-application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


コンジット アプリケーションの基本的な操作は、パスのファイルを取得および渡すキットの呼び出しを`PKAddPassesViewController`ウォレットにパスを追加するオプションをユーザーに提供します。 このビューのコント ローラーの実装については、次のセクションで**コンパニオン アプリケーション**です。

コンジット アプリケーションは、コンパニオン アプリケーションと同じ方法で特定の型を渡す ID 用にプロビジョニングする必要はありません。

## <a name="companion-applications"></a>コンパニオン アプリケーション

コンパニオン アプリケーションでは、アプリケーションに関連付けられているパスを作成するパスに関連付けられている情報を更新する、パスを管理するそれ以外の場合など、パスを操作するための追加の機能を提供します。

コンパニオン アプリケーションは、ウォレットの機能を複製するのにはできません。 スキャンするためのパスを表示するものではありません。

このセクションの残りの部分では、基本的なコンパニオン アプリを連携する渡すキットを作成する方法について説明します。

### <a name="provisioning"></a>プロビジョニング

ウォレットがストア テクノロジであるため、アプリケーションとは別にプロビジョニングする必要があり、チーム プロビジョニング プロファイルまたはワイルドカード アプリ ID を使用できません。 参照してください、[機能を持つ作業](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md)ウォレット アプリケーションの一意のアプリ ID とプロビジョニング プロファイルを作成するをガイドします。

### <a name="entitlements"></a>権利

**Entitlements.plist** 最近使ったすべて Xamarin.iOS プロジェクトにファイルを含める必要があります。 新しい Entitlements.plist ファイルを追加する手順を[権利に関する作業](~/ios/deploy-test/provisioning/entitlements.md)ガイドです。

設定する権利は、次の操作します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

ダブルクリックして、 **Entitlements.plist**ソリューション パッドを開くには、Entitlements.plist エディター内のファイル。

![](passkit-images/image31.png "Entitlements.plst editor")

[定期券サイズ] セクションでは、選択、**を有効にするウォレット**オプション

![](passkit-images/image32.png "ウォレットの権利を有効にします。")


既定のオプションでは、アプリの種類を渡すすべてを許可するためです。 ただし、アプリを制限し、チームのパスの型のサブセットのみを許可することができます。 この選択を有効にする、**チームのサブセットを許可する型を渡す**しできるようにする、サブセットのパスの型識別子を入力します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ダブルクリックして、 **Entitlements.plist** XML ソース ファイルを開くファイル。

ウォレットの権利を追加するには、設定、**プロパティ**に`Passbook Identifiers`のドロップダウンで、これは自動的に設定、**型**`Array`です。 文字列を設定し、**値**に`$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "ウォレットの権利を有効にします。")

これにより、アプリはすべてのパスの種類を許可できるようになります。 アプリを制限し、チームのパスの型のサブセットのみを許可する、文字列値設定します。

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

ここで`pass.$(CFBundleIdentifier)`渡す id が作成されたです[上](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>デバッグ

アプリケーションの配置の問題があれば、確認が使用されている正しい**プロビジョニング プロファイル**ことと、`Entitlements.plist`として選択されて、**カスタム権利**ファイル**バンドル署名 * iPhone**オプション。

展開するときにします。 このエラーが発生した場合

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

続いて、`pass-type-identifiers`権利配列が正しくありません (または一致しない場合、**プロビジョニング プロファイル**)。 型 Id を渡すと、チーム ID が正しいことを確認します。

 <a name="Classes" />

## <a name="classes"></a>クラス

次の渡すキット クラスは、パスにアクセスするアプリに対して使用できます。

-  **PKPass** – パスのインスタンス。
-  **PKPassLibrary** – デバイスのパスにアクセスする API を提供します。
-  **PKAddPassesViewController** –、ウォレットの保存先をユーザーのパスを表示するために使用します。
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS 開発者


## <a name="example"></a>例

この記事のサンプル コードで PassLibrary プロジェクトを参照してください。 ウォレット コンパニオン アプリケーションで必要となる次の一般的な関数を示しています。

### <a name="check-that-wallet-is-available"></a>ウォレットが利用可能であることを確認します。

定期券サイズは、iPad で利用可能なはないため、アプリケーションが渡すキットの機能にアクセスする前に確認する必要があります。

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>ライブラリ パス インスタンスを作成します。

アプリケーションを作成および保存し、渡すキット API にアクセスするインスタンスで、渡すキット ライブラリは、シングルトンではありません。

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>パスの一覧を取得します。

アプリケーションは、ライブラリからのパスの一覧を要求できます。 この一覧は自動的にフィルター選択されたによって渡すキットにチーム ID で作成され、表示されるパスは、権利でのみ認識できるようにします。

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

シミュレーターをフィルター処理しない返される、パスの一覧のため、このメソッドは、実際のデバイスで常にテストする必要がありますに注意してください。 この一覧は、2 つの割引券を追加した後、UITableView、次のようにサンプル アプリの外観で表示できます。

 [![](passkit-images/image29.png "サンプル アプリの外観次のように 2 つの割引券を追加した後")](passkit-images/image29.png#lightbox)


### <a name="displaying-passes"></a>パスを表示します。

限られた情報はコンパニオン アプリ内でパスのレンダリングに使用できます。

コード例は、パスの一覧を表示するこの一連の標準的なプロパティから選択します。

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

この文字列は、サンプルのアラートとして表示されます。

 [![](passkit-images/image30.png "このサンプルでクーポンが選択されているアラート")](passkit-images/image30.png#lightbox)

使用することも、`LocalizedValueForFieldKey()`設計したパスのフィールドからデータを取得する方法 (がわかっているためにフィールドが存在する)。 このコード例は表示されません。

### <a name="loading-a-pass-from-a-file"></a>ファイルからのパスの読み込み

パスを財布に追加するには、ユーザーのアクセス許可を持つ、ためビュー コント ローラーが決定できるように表示する必要があります。 このコードで使用、**追加**読み込めません (を置き換えてくださいこの署名している) アプリに埋め込まれているビルド済みパスの例でのボタン。

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

パスが表示され、**追加**と**キャンセル**オプション。

 [![](passkit-images/image20.png "追加し、[キャンセル] のオプションの表示パス")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>既存のパスを置き換えます

既存のパスを置き換えることは不要、ユーザーのアクセス許可が、パスが既に存在しない場合は失敗です。

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>パスの編集

コードにパス オブジェクトを更新することはできませんので PKPass は変更不可ではありません。 アプリケーションのパスでデータを変更するには、パスのレコードを保持したり、アプリケーションをダウンロードする更新された値を持つ新しいパス ファイルを生成できる web サーバーへのアクセスが必要です。

パスは、プライベートとセキュリティで保護された保持する必要がある証明書で署名する必要がありますので、サーバーでパスのファイルの作成を行う必要があります。

更新されたパスのファイルが生成されるを使用して、`Replace`デバイス上の古いデータを上書きする方法です。

### <a name="display-a-pass-for-scanning"></a>スキャンのパスを表示します。

上記のよう、専用ウォレットをスキャンするため、パスを表示できます。 使用してパスを表示することができます、`OpenUrl`ように、メソッド。

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>変更の通知の受信

アプリケーションがリッスンできる渡すライブラリに加えられる変更を使用して、`PKPassLibraryDidChangeNotification`です。 バック グラウンドで更新をトリガーするため、それらのアプリでリッスンすることをお勧めの通知では、変更が考えられます。

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

ライブラリのインスタンスを渡す PKPassLibrary がシングルトンではないために、通知に登録するときに重要です。

## <a name="server-processing"></a>サーバーの処理

渡すキットをサポートするためにサーバー アプリケーションのビルドの詳細については、この記事は入門編の範囲を超えてはします。

提供される .NET コード、 *signpassnet*サンプルは、パスを生成するサーバー側メソッドの基礎として使用可能性があります。

ビュー [WWDC ビデオ: Introducing Passbook、パート 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309)詳細については 27:00 分からです。

### <a name="other-resources"></a>その他のリソース

参照してください[dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook)ソース c# サーバー側コードを開きます。

## <a name="push-notifications"></a>プッシュ通知

プッシュ通知を使用してパスを更新するの詳細については、この記事は入門編の範囲を超えてはします。

更新プログラムが必要な場合は、ウォレットから web 要求に応答する Apple によって定義された REST のような API を実装する必要があります。 提供される .NET コード、 *signpassnet*サンプルは、それらの要求の結果として新しいパスを生成するための基礎として使用可能性があります。

ビュー [WWDC ビデオ: Introducing Passbook、パート 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309)詳細については 27:00 分からです。

## <a name="summary"></a>まとめ

この記事は、渡すキットの導入し、利点理由のいくつかの概要を示す、渡すキットの完全なソリューションを実装する必要がさまざまな部分を説明します。 パス、ようにする処理のパスと手動で Xamarin.iOS アプリケーションから渡すキット Api にアクセスする方法を作成するには、Apple 開発者アカウントを構成するための手順を説明します。

## <a name="related-links"></a>関連リンク

- [CreateAPassManually (サンプル)](https://developer.xamarin.com/samples/PassKit/)
- [PassKit サンプル](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)
- [Passbook プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [開発者向けの passbook](https://developer.apple.com/passbook/)
- [パスのファイルについて](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [キット フレームワーク参照を渡す](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [キット フレームワーク参照を渡す](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook Web サービス参照の追加](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
