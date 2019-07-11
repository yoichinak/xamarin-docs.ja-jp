---
title: Xamarin.iOS の Apple Pay 機能
description: アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。 このガイドでは、Apple Pay 機能に必要な設定について説明します。
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: b9a5b70b46447ab6eb7143322dd0d2e5dc55200d
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675141"
---
# <a name="apple-pay-capabilities-in-xamarinios"></a>Xamarin.iOS の Apple Pay 機能

_アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。このガイドでは、Apple Pay 機能に必要な設定について説明します。_

Apple Pay を使用すると、ユーザーは自分の iOS デバイスから物理的な商品の支払いができます。 このセクションでは、Apple Developer Center で Apple Pay に必要なすべての必須コンポーネントを作成する方法について説明します。

Developer Center から新しいアプリをプロビジョニングするときに、実行する必要がある 3 つの手順があります。

1.  マーチャント ID を作成します。
2.  Apply Pay 機能を使用して App ID を作成し、それにマーチャントを追加します。
3.  マーチャント ID の証明書を生成します。

次の手順で、上記の項目の作成手順を説明します。

<a name="merchantid" />

## <a name="create-merchant-id"></a>マーチャント ID の作成

マーチャント ID は、支払いを受け取れることを Apple Pay に知らせるために使用され、PassKit の `PaymentRequest` メソッドに渡され、Apple Pay のエンタイトルメントで使用されます。

1.  [Apple Developer Center](https://developer.apple.com/account/) の [Certificate, Identifiers and Profiles]\(証明書、ID、およびプロファイル\) セクションを参照します。 
 
    ![Developer Center でのマーチャント ID の選択](apple-pay-capabilities-images/image57.png)

2.  **[Identifiers]\(ID\)** の下で **[Merchant IDs]\(マーチャント ID\)** を選択し、 **+** を選択して新しいコンテナーを作成します。  

3.  次に示すように、フォームに新しい説明と識別子を記入します。 説明は、ID を特定しやすいものにします。これは後で変更することができます。 識別子は、一意である必要があり、文字列  `merchant` で始まる必要があります。 Apple では、識別子を次の形式にすることを推奨しています。`merchant.com.[Your-App-Name]`:
   
    ![新しいマーチャント ID の詳細](apple-pay-capabilities-images/image58.png)

4.  詳細を確認し、ID を **登録** します。 
    
    ![マーチャント ID の確認](apple-pay-capabilities-images/image59.png)

<a name="appid" />

## <a name="create-an-app-id-with-the-apple-pay-capability-that-includes-the-merchant-id"></a>Apple Pay 機能を使用してマーチャント ID を含む App ID を作成する

1.  [Developer Center](https://developer.apple.com/account/) で、 **[Identifiers]\(ID\)** の下で **[Merchant IDs]\(マーチャント ID\)** をクリックします。 
    
    ![Developer Center での App ID の選択](apple-pay-capabilities-images/image6.png)

2.  **+** ボタンを選択し、新しい App ID を追加します。 
   
    ![新しい App ID の追加ボタン](apple-pay-capabilities-images/image27.png)

3.  App ID の [Name]\(名前\) を入力し、[Explicit App ID]\(明示的な App ID\) を指定します。    
   
    ![App ID の詳細画面](apple-pay-capabilities-images/image35.png)

4.  [App Services] の下で [Apple Pay] を選択します。    
  
    ![App Services の Apple Pay](apple-pay-capabilities-images/image36.png)

5.  **[Continue]\(続行\)** 、 **[Register]\(登録\)** の順に選択します。 確認画面で、Apple Pay が黄色のシンボルで [Configurable]\(構成可能\) が選択されていることを示していることに注目してください。 
   
    ![Apple Pay の確認画面](apple-pay-capabilities-images/image37.png)

6.  App ID のリストに戻り、先ほど作成したものを選択します。  
   
    ![App ID の編集](apple-pay-capabilities-images/image38.png)

7.  この拡張されたセクションの下部までスクロールし、 **[Edit]\(編集\)** をクリックします。
8.  Apple Pay までリストを下にスクロールして、 **[Edit]\(編集\)** ボタンをクリックします。  
    
    ![Apple Pay の App ID 詳細の編集](apple-pay-capabilities-images/image39.png)

9.  この App ID で使用するマーチャント ID を選択し、 **[Continue]\(続行\)** をクリックします。  
    
    ![App ID に使用するマーチャント ID の選択](apple-pay-capabilities-images/image40.png)

10. マーチャント ID の割り当てを確認し、 **[Assign]\(割り当て\)** を押します。  
    
    ![確認画面](apple-pay-capabilities-images/image41.png)

これでこのアプリ ID を使用して、新しいプロビジョニング プロファイルの生成または再生成ができます。手順については、[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)に関するガイドを参照してください。 

<a name="certificate" />

## <a name="create-a-certificate-for-your-merchant-id"></a>マーチャント ID の証明書の作成

トランザクションに関連する機密データを暗号化するため、Apple によって証明書が求められます。 作成されたマーチャント ID ごとに独自の証明書が必要です。 

証明書を作成するには、次の手順に従います。

1.  上記で作成したマーチャント ID を選択し、 **[Edit]\(編集\)** を押します。 
    
    ![マーチャント ID の編集ダイアログ](apple-pay-capabilities-images/image42.png)

2.  [iOS Merchant ID Settings]\(iOS マーチャント ID の設定\) 画面で、 **[Create Certificate]\(証明書の作成\)** をクリックします。 
   
    ![支払い処理の証明書の作成](apple-pay-capabilities-images/image43.png)

3.  次の質問に答えます。 

    ![支払いが中国でのみ処理される場合の対処](apple-pay-capabilities-images/image44.png)

4.  この時点で、_証明書の署名要求_ (CSR) の作成が求められます。 

    ![証明書の署名要求の作成](apple-pay-capabilities-images/image45.png)
    
    > [!IMPORTANT]
    > Apple Pay に JudoPay や Stripe などの支払いプロバイダーを使用している場合は、これらのプロバイダーから、ここで使用可能な適切な形式の CSR を提供してもらうことができます。 CSR の要求に関する情報は、[JudoPay](https://www.judopay.com/docs/version-52/apple-pay/getting-started/#create-an-apple-pay-certificate) や [Stripe](https://stripe.com/docs/apple-pay/apps#csr) のサイトにあります。 独自の CSR を作成するには、次の手順 5 - 8 に従います。 CSR を入手したら、手順 9 に進みます。

5.  Keychain Access アプリケーションを開き、 **[Keychain Access] > [Certificate Assistant]\(証明書アシスタント\) > [Request a Certificate from a Certificate Authority]\(証明機関から証明書を要求する\)** の順に移動します。 

     ![Mac でキーチェーンを使用して CSR を作成する](apple-pay-capabilities-images/image46.png)

6.  メール アドレスと秘密キーの名前を入力し、[CA Email Address]\(CA メール アドレス\) は空のままにして、 **[Save to Disk]\(ディスクに保存\)** オプションを選択し、 **[Let me specify key pair information]\(キーペア情報を指定する\)** を選択します。

     ![[Certificate Information]\(証明書情報\) ダイアログ](apple-pay-capabilities-images/image47.png)

7.  CSR を任意の場所に保存します。 

     ![CSR をローカル コンピューターに保存](apple-pay-capabilities-images/image48.png)

8.  [Key Pair information]\(キーペア情報\) 画面で、 **[Key Size]\(キー サイズ\)** を **[256 bits]\(256 ビット\)** 、 **[Algorithm]\(アルゴリズム\)** を **[ECC]** に設定し、 **[Continue]\(続行\)** をクリックします。

     ![[Key Pair information]\(キーペア情報\) ダイアログの入力](apple-pay-capabilities-images/image49.png)

9.  Developer Center で、 **[Continue]\(続行\)** をクリックして CSR をアップロードします。 

     ![CSR を Developer Center にアップロードする準備](apple-pay-capabilities-images/image50.png)

10. **[ファイルの選択…] をクリックして、** CSR を選択し、 **[Continue]\(続行\)** を押して Developer ポータルにアップロードします。 

     ![CSR を Developer Center にアップロード](apple-pay-capabilities-images/image51.png)

11. 証明書が生成されたら、それをダウンロードし、ダブルクリックしてキーチェーンにインストールします。

Apple Pay の使用に関する詳細は、次のガイドを参照してください。

*   [ Apple Pay の概要](~/ios/platform/apple-pay.md)

## <a name="next-steps"></a>次の手順
 
以下のリストでは、実行する必要がある可能性のある追加の手順について説明します。

* アプリでフレームワークの名前空間を使用します。
* アプリに必要な権利を追加します。 必要な権利とその追加方法については、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)に関するガイドを参照してください。
* アプリの  **[iOS バンドル署名]** で、 **[カスタムの権利]** が **Entitlements.plist** に確実に設定されているようにします。 これは、デバッグと iOS シミュレーターのビルドに対する既定の設定では _"ありません"_  。

App Services で問題が発生した場合は、メイン ガイドの[トラブルシューティング](~/ios/deploy-test/provisioning/capabilities/index.md)のセクションを参照してください。