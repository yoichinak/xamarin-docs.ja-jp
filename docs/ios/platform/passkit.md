---
title: Xamarin. iOS の Pass Kit
description: ウォレットアプリを使用すると、iOS ユーザーは自分のデバイスにデジタルパスを保存できます。 Pass Kit フレームワークを使用すると、開発者はプログラムによってパスを操作できます。
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/13/2018
ms.openlocfilehash: 8039482175465a67867f3c70f17518dee8b9500b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70277869"
---
# <a name="passkit-in-xamarinios"></a>Xamarin. iOS の Pass Kit

IOS ウォレットアプリを使用すると、ユーザーは自分のデバイスにデジタルパスを保存できます。
これらのパスは、商人によって生成され、電子メール、Url、またはマーチャントの iOS アプリを通じて顧客に送信されます。 これらのパスは、ムービーチケットからロイヤルティカードまで、さまざまなものを表すことができます。 Pass Kit フレームワークを使用すると、開発者はプログラムによってパスを操作できます。

このドキュメントでは、ウォレットと、Xamarin. iOS での Pass Kit API の使用について説明します。

 [![](passkit-images/image1.png "ウォレットは、電話のすべてのパスを保存して整理します。")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>必要条件

このドキュメントで説明されている Pass Kit の機能には、iOS 6 と Xcode 4.5、および Xamarin. iOS 6.0 が必要です。

## <a name="introduction"></a>概要

Pass Kit が解決する主な問題は、バーコードの分布と管理です。 現在、バーコードを使用する実際の例を次に示します。

- **オンラインでのムービーチケットの購入**–顧客は通常、チケットを表すバーコードを電子メールで送信します。 このバーコードは印刷され、エントリのスキャン対象の映画に取り込まれます。
- **ロイヤルティカード**–お客様は、商品を購入したときに表示とスキャンを行うために、ウォレットやハンドバッグにさまざまな店舗固有のカードを持っています。
- **クーポン**–クーポンは、letterboxes を通じて、また新聞や雑誌のバーコードとして、電子メールで、印刷可能な web ページとして配信されます。 お客様は、スキャン、商品、サービス、または割引を取得するための店舗を利用できます。
- 採用**パス**–映画チケットを購入する場合と似ています。

Pass Kit は、次の各シナリオの代替手段を提供します。

- **ムービーチケット**–購入後、顧客は、(電子メールまたは web サイトリンクを使用して) イベントチケットパスを追加します。 ムービーの再生時間が近づくと、パスが自動的にロック画面に表示されます。また、映画に到着すると、パスが簡単に取得され、ウォレットに表示されてスキャンできるようになります。
- **ロイヤルティカード**は、物理的なカードの提供 (またはその他) ではなく、ストアカードによって (電子メールまたは web サイトのログイン後に) 発行されます。 ストアでは、プッシュ通知によるパスでのアカウントのバランスの更新などの追加機能を提供できます。また、位置情報サービスを使用すると、顧客が店舗の場所に近づいたときに、パスが自動的にロック画面に表示されることがあります。
- **クーポン**–クーポンパスは、追跡を容易にし、電子メールや web サイトリンク経由で配布するための一意の特性によって、簡単に生成できます。 ダウンロードしたクーポンは、ユーザーが特定の場所または特定の日付 (有効期限が近づいているときなど) に近づいたときに、ロック画面に自動的に表示されます。 クーポンはユーザーの電話に格納されるため、常に便利であり、誤った場所にあることはありません。 アプリストアのリンクを成功させることができ、顧客との契約が増えるため、クーポンによってコンパニオンアプリをダウンロードすることをお勧めします。
- 採用**パス**-オンラインチェックインプロセスの後、顧客は電子メールまたはリンクを使用して、その製品を受け取ることができます。 トランスポートプロバイダーによって提供されるコンパニオンアプリには、チェックインプロセスを含めることができます。また、顧客が座席や料理を選択するなどの追加機能を実行できるようにすることもできます。 トランスポートプロバイダーは、トランスポートが遅延またはキャンセルされた場合に、プッシュ通知を使用してパスを更新できます。 使用時間が近づくにつれて、パスがロック画面にリマインダーとして表示され、パスにすばやくアクセスできるようになります。

コアでは、使用している iOS デバイスにバーコードを格納して表示するための簡単で便利な方法が用意されています。 追加の時間と場所のロック画面の統合により、プッシュ通知とコンパニオンアプリケーション統合により、非常に高度な販売、チケット発行、および課金サービスの基盤が提供されます。

## <a name="passkit-ecosystem"></a>Pass Kit のエコシステム

Pass Kit は、CocoaTouch 内の API ではなく、アプリケーション、データ、およびサービスの大規模なエコシステムの一部であり、バーコードやその他のデータの安全な共有と管理を容易にします。 この図は、パスの作成と使用に関係するさまざまなエンティティを示しています。

 [![](passkit-images/image2.png "この概要図は、パスの作成と使用に関連するエンティティを示しています。")](passkit-images/image2.png#lightbox)

エコシステムの各部分には、明確に定義されたロールがあります。

- **ウォレット**–パスを保存および表示する Apple の組み込み iOS アプリです。 これは、実際の世界で使用するためにパスがレンダリングされる唯一の場所です (コードバーが表示され、パス内のすべてのローカライズされたデータが表示されます)。
- **コンパニオンアプリ**– pass プロバイダーによって構築された iOS 6 アプリは、店舗カードへの値の追加、ボードパスまたはその他のビジネス固有の機能の座席の変更など、問題が発生したパスの機能を拡張します。 パスを利用するには、コンパニオンアプリは必要ありません。
- **サーバー** : パスを生成し、配布用に署名できる、セキュリティで保護されたサーバー。 コンパニオンアプリはサーバーに接続して、新しいパスを生成したり、既存のパスへの更新を要求したりすることができます。 必要に応じて、ウォレットが更新パスを呼び出す web サービス API を実装することもできます。
- **Apns サーバー** –サーバーは、apns を使用して、特定のデバイスでのパスに対する更新をウォレットに通知する機能を備えています。 ウォレットに通知をプッシュします。これにより、変更の詳細がサーバーに接続されます。 コンパニオンアプリでは、 `PKPassLibraryDidChangeNotification`この機能に APNS を実装する必要はありません (をリッスンできます)。
- **コンジットアプリ**-パスを直接操作しないアプリケーション (コンパニオンアプリの場合など)。ただし、パスを認識してウォレットに追加できるようにすることで、ユーティリティを改善できます。 メールクライアント、ソーシャルネットワークブラウザー、およびその他のデータ集計アプリでは、すべての添付ファイルまたはパスへのリンクが発生する可能性があります。

エコシステム全体は複雑であるため、一部のコンポーネントはオプションであり、はるかに簡単な Pass Kit の実装が可能であることに注意してください。

## <a name="what-is-a-pass"></a>パスとは何ですか。

Pass は、チケット、クーポン、またはカードを表すデータのコレクションです。 個人による1回の使用を目的としており (したがって、フライト番号や座席割り当てなどの詳細が含まれます)、または任意の数のユーザーが共有できる複数の使用トークン (割引クーポンなど) を使用することができます。 詳細な説明については、Apple の[パスファイル](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)のドキュメントを参照してください。

### <a name="types"></a>型

現在、5種類のサポートされています。ウォレットアプリでは、パスのレイアウトと上端で区別できます。

- **イベントチケット**–小さな半円形カットアウト。
- [輸送**パス**] – [側]-[トランスポート固有] アイコンを指定できます (例: バス、電車、航空機)。
- **店舗カード**–クレジットカードまたはデビットカードのように、上に切り上げられます。
- **クーポン**–上部に沿って perforated。
- **Generic** –店舗カードと同じで、上に切り上げられます。


このスクリーンショットには、次の5つのパスの種類が示されています (注文: クーポン、汎用、店舗カード、採用パス、およびイベントチケット)。

 [![](passkit-images/image3.png "このスクリーンショットには、次の5つのパスの種類が表示されます。")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>ファイル構造

パスファイルは、実際には、特定の JSON ファイル (必須)、さまざまなイメージファイル (省略可能)、およびローカライズされた文字列 (省略可能) を含む、拡張子が**PKPASS** ZIP アーカイブです。

- **pass. json** –必須。 パスのすべての情報が含まれます。
- **manifest. json** –必須。 署名ファイルとこのファイル (manifest.xml) を除き、パス内の各ファイルの SHA1 ハッシュを格納します。
- **signature** –必須。 IOS プロビジョニングポータルで`manifest.json`生成された証明書を使用してファイルに署名することによって作成されます。
- **logo .png** –省略可能。
- **background .png** –省略可能。
- **icon .png** –省略可能。
- **ローカライズ可能な文字列ファイル**–省略可能。

パスファイルのディレクトリ構造を次に示します (これは ZIP アーカイブの内容です)。

 [![](passkit-images/image4.png "パスファイルのディレクトリ構造を次に示します。")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.json

パスは通常、サーバー上で作成されるため、JSON は形式です。これは、サーバー上で生成コードがプラットフォームに依存しないことを意味します。 すべてのパスの3つの重要な情報を次に示します。

- **Teamidentifier** –生成したすべてのパスを App Store アカウントにリンクします。 この値は、iOS プロビジョニングポータルに表示されます。
- **passTypeIdentifier** –プロビジョニングポータルに登録して、グループのパスをグループ化します (複数の種類を作成した場合)。 たとえば、コーヒーショップでは、顧客がロイヤルティクレジットを獲得するための店舗カードパスの種類を作成し、割引クーポンを作成および配布するために別のクーポンパスの種類を作成する場合があります。 同じコーヒーショップが、ライブミュージックイベントを保持し、それらのイベントチケットパスを発行する場合もあります。
- **シリアル**値: この`passTypeidentifier`内の一意の文字列。 値はウォレットに対して非透過的ですが、サーバーと通信するときに特定のパスを追跡するために重要です。

各パスには、次に示すような多数の JSON キーがあります。その例を次に示します。

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

### <a name="barcodes"></a>コード

サポートされているのは、2D 形式のみです。PDF417、アステカ、QR。 Apple は、1D バーコードがバックライトされた電話画面でのスキャンに unsuited ていることを要求します。

バーコードの下に表示される代替テキストは省略可能です。一部の加盟店では、手動で読み取り/種類を設定できます。

ISO-8859-1 エンコードが最も一般的であり、パスを読み取るスキャンシステムによって使用されるエンコーディングを確認します。

### <a name="relevancy-lock-screen"></a>関連性 (ロック画面)

ロック画面にパスが表示される原因となるデータには、次の2種類があります。

 **場所**

パスには最大10個の場所を指定できます。たとえば、顧客が頻繁にアクセスする店舗や、映画または空港の場所を指定できます。 顧客は、コンパニオンアプリを使用してこれらの場所を設定できます。また、プロバイダーは、使用状況データ (顧客のアクセス許可で収集された場合) からそれらを特定できます。

パスがロック画面に表示されたときにフェンスが計算され、ユーザーがその領域を離れると、そのパスはロック画面から非表示になります。 この半径は、誤用を防ぐためにパスに関連付けられています。

 **日付と時刻**

パスで指定できる日付/時刻は1つだけです。 日付と時刻は、パススルーパスとイベントチケットに対してロック画面のアラームをトリガーする場合に便利です。

Push または Pass Kit API を使用して更新することができます。これにより、複数の使用チケット (シアターやスポーツなど) の場合に日付/時刻を更新できるようになります。

### <a name="localization"></a>ローカリゼーション

パスを複数の言語に変換することは、iOS アプリケーションをローカライズすることと似て`.lproj`います。拡張機能を使用して言語固有のディレクトリを作成し、その中にローカライズされた要素を配置します。 テキスト変換は`pass.strings`ファイルに入力する必要がありますが、ローカライズされたイメージはパスルートで置き換えるイメージと同じ名前にする必要があります。

## <a name="security"></a>セキュリティ

パスは、iOS プロビジョニングポータルで生成したプライベート証明書で署名されます。 パスに署名する手順は次のとおりです。

1. パスディレクトリ内の各ファイルについて SHA1 ハッシュを計算します ( `manifest.json`また`signature`はファイルを含めないでください。この段階では、そのいずれも存在すべきではありません)。
1. 各`manifest.json`ファイル名の JSON キー/値リストとしてハッシュを使用して書き込みます。
1. 証明書を使用して`manifest.json`ファイルに署名し、その結果をと`signature`いう名前のファイルに書き込みます。
1. すべてを ZIP 形式にし、結果ファイルに`.pkpass`ファイル拡張子を付けます。


パスの署名には秘密キーが必要であるため、このプロセスは、ユーザーが制御するセキュリティで保護されたサーバー上でのみ実行する必要があります。 アプリケーションのパスを試して生成するためにキーを配布しないようにしてください。

 
## <a name="configuration-and-setup"></a>構成とセットアップ

このセクションでは、プロビジョニングの詳細を設定し、最初のパスを作成するための手順について説明します。

### <a name="provisioning-passkit"></a>プロビジョニングキット

パスをアプリストアに入力するには、開発者アカウントにリンクされている必要があります。 これには、次の2つの手順が必要です。

1. パスは、パスの種類 ID と呼ばれる一意の識別子を使用して登録する必要があります。
1. 開発者のデジタル署名でパスに署名するには、有効な証明書を生成する必要があります。

パスの種類 ID を作成するには、次の手順を実行します。

#### <a name="create-a-pass-type-id"></a>パスの種類 ID を作成する

最初の手順では、サポートされる異なる_種類_のパスごとにパスの種類 ID を設定します。 パス ID (またはパスの種類の識別子) によって、パスの一意の識別子が作成されます。 この ID を使用して、証明書を使用して開発者アカウントにパスを関連付けます。

1. [IOS プロビジョニングポータルの [証明書]、[識別子]、[プロファイル] セクション](https://developer.apple.com/account/overview.action)で、 **[識別子]** に移動して、 **[パスの種類 id]** を選択します。 次に、 **+** ボタンを選択して新しいパスの種類を作成します。[![](passkit-images/passid.png "新しいパスの種類を作成する")](passkit-images/passid.png#lightbox)

2. パスの**説明**(名前) と**識別子**(一意の文字列) を指定します。 すべてのパスの種類 id は、次の例`pass.` `pass.com.xamarin.coupon.banana`の文字列で始まる必要があることに注意してください。[![](passkit-images/register.png "説明と識別子を入力してください")](passkit-images/register.png#lightbox)


3. [Register] \ (**登録**\) ボタンを押して、パス ID を確認します。

#### <a name="generate-a-certificate"></a>証明書を生成する

このパスの種類 ID の新しい証明書を作成するには、次の手順を実行します。

1. 新しく作成したパス ID を一覧から選択し、 **[編集]** をクリックします。[![](passkit-images/pass-done.png "一覧から新しいパス ID を選択します")](passkit-images/pass-done.png#lightbox)

    次に、 **[証明書の作成]** を選択します。 :

    [![](passkit-images/cert-dist.png "証明書の作成の選択")](passkit-images/cert-dist.png#lightbox)


2. 証明書署名要求 (CSR) を作成する手順に従います。
  
3. 開発者ポータルの **[続行]** ボタンをクリックし、CSR をアップロードして証明書を生成します。

4. 証明書をダウンロードし、ダブルクリックしてキーチェーンにインストールします。


これで、このパスの種類 ID の証明書が作成されました。次のセクションでは、パスを手動で作成する方法について説明します。

ウォレットのプロビジョニングの詳細については、[機能](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md)の使用に関するガイドを参照してください。

### <a name="create-a-pass-manually"></a>パスを手動で作成する

パスの種類を作成したので、シミュレーターまたはデバイスでテストするためのパスを手動で作成できます。 パスを作成する手順は次のとおりです。

- パスファイルを格納するディレクトリを作成します。
- すべての必要なデータを含むパスの json ファイルを作成します。
- 必要に応じて、フォルダーに画像を含めます。
- フォルダー内のすべてのファイルについて SHA1 ハッシュを計算し、manifest に書き込みます。
- ダウンロードした証明書の p12 ファイルで、マニフェストに署名します。
- ディレクトリの内容を ZIP 形式にし、拡張子を pkpass して名前を変更します。


この記事の[サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)には、パスを生成するために使用できるソースファイルがいくつかあります。 Createapassmanually ディレクトリ`CouponBanana.raw`のディレクトリにあるファイルを使用します。 次のファイルが存在します。

 [![](passkit-images/image18.png "これらのファイルは存在します")](passkit-images/image18.png#lightbox)

[Pass. json] を開き、JSON を編集します。 Apple Developer アカウントと一致さ`passTypeIdentifier`せる`teamIdentifer`には、少なくともとを更新する必要があります。

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

次に、各ファイルのハッシュを計算して、 `manifest.json`ファイルを作成する必要があります。 完了すると、次のようになります。

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

次に、このパスの種類 ID に対して生成された証明書 (p12 ファイル) を使用して、このファイルに対して署名を生成する必要があります。

#### <a name="signing-on-a-mac"></a>Mac での署名

[Apple ダウンロード](https://developer.apple.com/downloads/index.action?name=Passbook)サイトから**ウォレットシードサポート資料**をダウンロードします。 `signpass`ツールを使用して、フォルダーをパスに変換します (これにより、SHA1 ハッシュも計算され、出力が pkpass ファイルに圧縮されます)。

#### <a name="testing"></a>テスト

これらのツールの出力を調べると (ファイル名を .zip に設定して開く)、次のファイルが表示されます (ファイル`manifest.json`と`signature`ファイルが追加されていることに注意してください)。

 [![](passkit-images/image19.png "これらのツールの出力を調べる")](passkit-images/image19.png#lightbox)

署名が完了し、ファイルの名前が変更された (例 に`BananaCoupon.pkpass`移動するには、シミュレーターにドラッグしてテストするか、自分自身に電子メールで送信して実際のデバイスで取得します。 次のように、パスを**追加**するための画面が表示されます。

 [![](passkit-images/image20.png "パス画面を追加する")](passkit-images/image20.png#lightbox)

通常、このプロセスはサーバー上で自動化されますが、手動パスの作成は、バックエンドサーバーのサポートを必要としないクーポンの作成のみを行う小規模企業にとっても可能です。

## <a name="wallet"></a>ウォレット

ウォレットは、Pass Kit エコシステムの中心的な部分です。 このスクリーンショットは、空のウォレットと、パスリストと個々のパスの外観を示しています。

 [![](passkit-images/image21.png "このスクリーンショットは、空のウォレットと、パスリストと個々のパスの外観を示しています。")](passkit-images/image21.png#lightbox)

ウォレットの機能は次のとおりです。

- これは、スキャンのためにバーコードを使用して渡される唯一の場所です。
- ユーザーは、更新プログラムの設定を変更できます。 有効にすると、プッシュ通知はパス内のデータの更新をトリガーできます。
- ユーザーは、ロック画面の統合を有効または無効にすることができます。 有効にすると、パスに埋め込まれた関連する時間と場所のデータに基づいて、パスがロック画面に自動的に表示されるようになります。
- Pass JSON で web サーバーの URL が指定されている場合、パスの逆側はプルから更新をサポートします。
- アプリの ID が pass JSON で指定されている場合は、コンパニオンアプリを開く (またはダウンロードする) ことができます。
- パスを削除することができます (かわいらしいシュレッダアニメーション)。

## <a name="adding-passes-into-wallet"></a>ウォレットへのパスの追加

次の方法で、ウォレットに渡すことができます。

- **コンジットアプリ**–これらはパスを直接操作しません。パスファイルを読み込み、ユーザーにウォレットに追加するオプションを提供するだけです。 

- **コンパニオンアプリ**–パスを配布するためのプロバイダーによって作成され、それらを参照または編集するための追加機能を提供します。 Xamarin iOS アプリケーションは、Pass Kit API に完全にアクセスして、パスを作成および操作します。 これにより、 `PKAddPassesViewController`を使用してウォレットにパスを追加できます。 このプロセスの詳細については、このドキュメントの「**コンパニオンアプリケーション**」セクションを参照してください。

### <a name="conduit-applications"></a>コンジットアプリケーション

コンジットアプリケーションは、ユーザーの代わりにパスを受け取る可能性がある中間アプリであり、そのコンテンツタイプを認識し、ウォレットに追加する機能を提供するようにプログラミングする必要があります。 コンジットアプリの例を次に示します。

- **Mail** –パスとして添付ファイルを認識します。
- **Safari** –パス URL リンクがクリックされたときに、パスのコンテンツの種類を認識します。
- **その他のカスタムアプリ**–添付ファイルを受信したり、リンクを開いたりするアプリ (ソーシャルメディアクライアント、メールリーダーなど)。


このスクリーンショットは、iOS 6 の**メール**がパスの添付ファイルを認識し、(操作された場合に) ウォレットに**追加**するように提供する方法を示しています。

 [![](passkit-images/image22.png "このスクリーンショットは、iOS 6 のメールがパスの添付ファイルを認識する方法を示しています。")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "このスクリーンショットは、ウォレットに pass 添付ファイルを追加する方法を示しています。")](passkit-images/image23.png#lightbox)

パスのパイプである可能性があるアプリを構築する場合、次の方法で認識できます。

- **ファイル拡張子**-pkpass
- **MIME の種類**-application/vnd. apple. pkpass
- **UTI** – com. pkpass


コンジットアプリケーションの基本的な操作では、pass ファイルを取得し、pass kit `PKAddPassesViewController`を呼び出して、ウォレットにパスを追加するオプションをユーザーに付与します。 このビューコントローラーの実装については、**コンパニオンアプリケーション**の次のセクションで説明します。

パイプアプリケーションは、コンパニオンアプリケーションと同じように、特定のパスの種類 ID に対してプロビジョニングする必要はありません。

## <a name="companion-applications"></a>コンパニオンアプリケーション

コンパニオンアプリケーションは、パスの作成、パスに関連付けられた情報の更新、アプリケーションに関連付けられているパスの管理など、パスを操作するための追加機能を提供します。

コンパニオンアプリケーションは、ウォレットの機能の複製を試行しないでください。 スキャン用のパスを表示するためのものではありません。

このセクションの残りの部分では、Pass Kit と対話する基本的なコンパニオンアプリを構築する方法について説明します。

### <a name="provisioning"></a>プロビジョニング

ウォレットは店舗テクノロジであるため、アプリケーションを個別にプロビジョニングする必要があり、チームプロビジョニングプロファイルやワイルドカードアプリ ID を使用することはできません。 ウォレットアプリケーションの一意のアプリ ID とプロビジョニングプロファイルを作成するには、[機能の使用](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md)に関するガイドを参照してください。

### <a name="entitlements"></a>権利

最新の Xamarin. iOS プロジェクトには、**権利の plist**ファイルが含まれている必要があります。 新しい権利の plist ファイルを追加するには、「[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)」ガイドの手順に従います。

権利を設定するには、次の手順を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Solution Pad の**権利の plist**ファイルをダブルクリックして、[権利] エディターを開きます。

![](passkit-images/image31.png "権利. plst エディター")

ウォレット セクションで、**ウォレットを有効にする** オプションを選択します。

![](passkit-images/image32.png "ウォレットの権利を有効にする")


既定のオプションでは、アプリはすべてのパスの種類を許可します。 ただし、アプリを制限し、チームのパスの種類のサブセットのみを許可することができます。 これを有効にするには、[**チームのパスの種類のサブセットを許可**する] を選択し、許可するサブセットのパスの種類の識別子を入力します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**権限の plist**ファイルをダブルクリックして、XML ソースファイルを開きます。

ウォレットの権利を追加するには 、ドロップ`Passbook Identifiers`ダウンでプロパティをに設定します。これにより、自動的に**型** `Array`が設定されます。 次に、文字列**値**をに`$(TeamIdentifierPrefix)*`設定します。

![](passkit-images/image33.png "ウォレットの権利を有効にする")

これにより、アプリはすべてのパスの種類を許可できるようになります。 アプリを制限し、チームパスの種類のサブセットのみを許可するには、文字列値をに設定します。

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

`pass.$(CFBundleIdentifier)` は、[上](~/ios/platform/passkit.md)で作成したパス ID です

-----

### <a name="debugging"></a>デバッグ

アプリケーションの展開で問題が発生した場合は、正しい**プロビジョニングプロファイル**を使用している`Entitlements.plist`こと、および**iPhone バンドル署名**オプションでが**カスタム権利**ファイルとして選択されていることを確認してください。

展開時にこのエラーが発生した場合は、次の手順を実行します。

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

その後`pass-type-identifiers` 、権利配列が正しくありません (または**プロビジョニングプロファイル**と一致しません)。 パスの種類 Id とチーム ID が正しいことを確認してください。

## <a name="classes"></a>クラス

アプリがパスにアクセスするには、次の Pass Kit クラスを使用できます。

- **Pkpass** –パスのインスタンス。
- **Pkpass library** –デバイスのパスにアクセスするための API を提供します。
- **PKAddPassesViewController** –ユーザーがウォレットに保存するためのパスを表示するために使用されます。
- **PKAddPassesViewControllerDelegate** – Xamarin 開発者向け

## <a name="example"></a>例

この記事の[サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)では、pass library プロジェクトを参照してください。 ウォレット・コンパニオンアプリケーションで必要となる次の一般的な関数について説明します。

### <a name="check-that-wallet-is-available"></a>ウォレットが使用可能であることを確認します

ウォレットは iPad では利用できないため、アプリケーションは、Pass Kit の機能にアクセスする前に確認する必要があります。

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>パスライブラリインスタンスの作成

Pass Kit ライブラリはシングルトンではありません。アプリケーションでは、およびインスタンスを作成して保存し、を使用して、Pass Kit API にアクセスする必要があります。

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>パスの一覧を取得する

アプリケーションは、ライブラリからパスの一覧を要求できます。 この一覧は、成功キットによって自動的にフィルター処理されるので、自分の権利に記載されている、チーム ID で作成されたパスのみを表示できます。

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

シミュレーターは返されるパスの一覧をフィルター処理しないため、この方法は常に実際のデバイスでテストする必要があります。 この一覧は、UITableView に表示できます。 [サンプルアプリ](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)は、2つのクーポンが追加された後、次のようになります。

 [![](passkit-images/image29.png "サンプルアプリは、2つのクーポンが追加された後、次のようになります。")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>表示 (パスを)

コンパニオンアプリ内のパスを表示するために、限定された情報のセットを利用できます。

コード例に示すように、この一連の標準プロパティから選択してパスの一覧を表示します。

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

この文字列は、[サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)ではアラートとして表示されます。

 [![](passkit-images/image30.png "サンプルで選択されているクーポンのアラート")](passkit-images/image30.png#lightbox)

また、 `LocalizedValueForFieldKey()`メソッドを使用して、デザインしたパス内のフィールドからデータを取得することもできます (どのフィールドが存在する必要があるかがわかります)。 コード例では、これは示されていません。

### <a name="loading-a-pass-from-a-file"></a>ファイルからのパスの読み込み

パスは、ユーザーのアクセス許可を持つウォレットにのみ追加できるので、ビューコントローラーを提示して決定する必要があります。 このコードは、この例の **[追加]** ボタンで、アプリに埋め込まれている既成のパスを読み込むために使用されます (これは、署名したものと置き換える必要があります)。

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

パスには、**追加**オプションと**キャンセル**オプションが表示されます。

 [![](passkit-images/image20.png "追加とキャンセルのオプションで示されるパス")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>既存のパスを置き換える

既存のパスを置き換える場合は、ユーザーのアクセス許可は必要ありませんが、パスがまだ存在しない場合は失敗します。

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>パスの編集

PKPass は変更できないため、コード内の pass オブジェクトを更新することはできません。 パスのデータを変更するには、アプリケーションがパスの記録を保持し、アプリケーションがダウンロードできる更新された値を持つ新しいパスファイルを生成できる web サーバーへのアクセスが必要です。

パスは、プライベートで安全な状態に保つ必要がある証明書を使用して署名する必要があるため、サーバー上でパスファイルの作成を行う必要があります。

更新されたパスファイルが生成されたら`Replace` 、メソッドを使用して、デバイス上の古いデータを上書きします。

### <a name="display-a-pass-for-scanning"></a>スキャンのためのパスを表示する

前述のように、スキャンのパスを表示できるのはウォレットだけです。 次に示すように、 `OpenUrl`メソッドを使用してパスを表示できます。

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>変更通知の受信

アプリケーションは、を使用して、 `PKPassLibraryDidChangeNotification`パスライブラリに対して行われた変更をリッスンできます。 変更は、通知によってバックグラウンドで更新がトリガーされることによって発生する可能性があるため、アプリでリッスンすることをお勧めします。

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

Pkpass Library はシングルトンではないため、通知の登録時にライブラリインスタンスを渡すことが重要です。

## <a name="server-processing"></a>サーバー処理

この入門記事では、サーバーアプリケーションを構築して、Pass Kit をサポートする方法について詳しく説明します。

「 [Dotnet-](https://github.com/tomasmcguinness/dotnet-passbook)オープンソースC#のサーバー側コード」を参照してください。

## <a name="push-notifications"></a>プッシュ通知

プッシュ通知を使用してパスを更新する方法の詳細については、この入門記事では扱いません。

更新が必要な場合、ウォレットからの web 要求に応答するには、Apple によって定義されている REST に似た API を実装する必要があります。

詳細について[は、「パスガイドを更新する](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1)」を参照してください。

## <a name="summary"></a>Summary

この記事では、役に立ついくつかの理由と、完全な Pass Kit ソリューションに実装する必要があるさまざまな部分について説明しました。 この記事では、パスを作成するために Apple Developer アカウントを構成するために必要な手順、手動でパスを作成するプロセス、および Xamarin. iOS アプリケーションからの Pass Kit Api へのアクセス方法について説明しています。

## <a name="related-links"></a>関連リンク

- [開発者向けウォレット](https://developer.apple.com/wallet/)
- [Pass Kit サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)
- [ウォレット開発者ガイド](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [フレームワーク– Apple Pay およびウォレット (WWDC ビデオ)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [Pass Kit フレームワークリファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Pass book Web サービスリファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [パスファイルについて](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [dotnet](https://github.com/tomasmcguinness/dotnet-passbook)(iOS ウォレットパッケージを生成するためのオープンソースライブラリ)
- [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)
