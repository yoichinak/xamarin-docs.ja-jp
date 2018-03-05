---
title: Apple Pay
description: "このガイドでは、Apple の食品、エンターテイメントなど、アプリを使用してメンバーシップなどの物理的な商品の支払いに支払いで使用する Xamarin.iOS 環境のセットアップについて説明します。 必要な識別子、証明書と資格情報が含まれています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 0ac2a19e9020113df273897a8ec2c86ee1763ec2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="apple-pay"></a>Apple Pay

_このガイドでは、Apple の食品、エンターテイメントなど、アプリを使用してメンバーシップなどの物理的な商品の支払いに支払いで使用する Xamarin.iOS 環境のセットアップについて説明します。必要な識別子、証明書と資格情報が含まれています。_


Apple Pay 導入された iOS 8 と共に食品、エンターテイメントなど、iOS デバイスを使用してメンバーシップなどの物理的な商品の支払いにユーザーを有効にします。 IPhone 6 および iPhone 6 では利用プラス、ストア内での購入の Apple Watch とも組で使用できるとします。 IPhone で使用すると、ことを確認し、ユーザーのクレジット_カードまたはデビット カードへのトランザクションを承認する方法として Touch ID を使用します。


## <a name="requirements"></a>必要条件

Apple Pay のみ上、および iOS 8 内で使用可能なしたがって Xcode 6 の最低限必要です。

次の項目も Apple Pay アプリに統合に必要です。

 - 支払プロセッサ プラットフォーム
 - パートナー識別子
 - Apple Pay の証明書
 - Apple Pay 権利

このドキュメントはこれらの項目をさらに詳しく説明します。

## <a name="differences-between-apple-pay-and-iap"></a>Apple 支払と IAP の相違点

Apple Pay の主な違いと*アプリ内購入*(IAP) は、販売する製品に関連します。 *物理*Apple Pay 経由で販売済み商品; 食品、宿泊施設/物理エンターテイメント (映画チケット) などはすべて、これの例です。 これに対し、IAP 販売*仮想*premium または他のコンテンツおよびストリーミング サービスの場合、またはゲームの余分な生活がどれほどの追加の月数をサブスクリプション – 待ち時間などの商品します。

使用するフレームワークも、主な違いです。[PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)は Apple Pay の使用、 [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP のフレームワーク API を提供します。

Apple Pay、Apple と[状態](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf)こと、"[] 料金は発生しません支払に Apple 支払を使用するには、ユーザー、オンライン ショップや開発者"です。 比較、トランザクションごとに 30% 充電に IAP があります。 さらに、Apple Pay でトランザクションを経由しない Apple まったく、代わりに、通過支払いプラットフォームです。


## <a name="using-a-payment-processor-platform"></a>支払プロセッサ プラットフォームを使用します。

Apple Pay の根本的な部分の 1 つは、支払いの処理です。 自分で行うことはできますが、暗号化の重要なナレッジが必要です。
- Apple の詳しく説明したよう[支払い処理のガイド](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)です。
支払い処理プラットフォームは、その一方で、アプリの構築に集中することができます、このような操作を処理します。

2 つのオプションは次のとおりです。

- **ストライプ ・** -でサインアップ[Stripe.com](https://stripe.com/) Api にアクセスします。

- **JudoPay** -チェック アウト、 [github での Xamarin のサンプル コード](https://github.com/Judopay/Xamarin-Sample-App)、登録と[JudoPay.com](https://www.judopay.com/)です。


## <a name="provisioning-for-apple-pay"></a>Apple 支払のプロビジョニング

Apple Pay を使用するアプリを構成するには、Apple 開発者ポータルには、アプリのセットアップが必要です。 いくつかの手順を Apple 支払用のアプリを正常にプロビジョニングするために従う必要があります。

1. パートナーの ID を作成します。
    - 手順に従います[ここ](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. 料金を支払う適用機能を持つアプリ ID を作成し、販売者を追加します。
    - 手順に従います[ここ](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. 販売者 ID を証明書を生成します。
    - 手順に従います[ここ](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. プロビジョニング プロファイルを新しく作成されたアプリ id を生成します。
    - 手順に従います[ここ](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay 権利を追加します。
    - 詳しく説明されている Apple 支払権利を選択して[ここ](~/ios/deploy-test/provisioning/entitlements.md)、元のファイルにキー/値ペアを手動で追加または[ここ](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Apple 支払の操作

Apple がに対していくつかの機能強化 Apple Pay の web サイトから Siri とマップとの対話によっても、セキュリティで保護された支払にユーザーに許可する ios 10 です。

10、iOS でいくつかの新しい Api が追加されました iOS プラットフォームと watchOS 動的支払ネットワークと新しいサンド ボックスのテスト環境をサポートするために両方を操作します。


### <a name="apple-pay-website-integration"></a>Apple 支払 web サイトの統合

新しい 10、iOS を開発者に組み込むことが Apple Pay 直接その web サイトを使用して**ApplePay JS**です。 IOS または macOS Safari で web サイトを閲覧するユーザーは、その iPhone または Apple Watch でのトランザクションの検証で Apple Pay の支払を作成できます。 詳細については、Apple を参照してください[ApplePay JP フレームワーク参照](https://developer.apple.com/reference/applepayjs)です。

### <a name="passkit-framework-enhancements"></a>PassKit フレームワークの機能強化

外部の Apple Pay をサポートするために 10、iOS で PassKit フレームワークが拡張されて`UIKit`カード発行者、アプリの中から自分のカードを提示するようにします。


#### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit の外部で Apple 支払をサポートします。

使用して[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)、アプリによって提供されるのと同じ機能をサポートできる[PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit を使用せずします。 この新しい API では、Apple Watch で (または特定の目的にも)、Apple Pay をサポートするために必要なは、(既存のアプリ) などその他の状況では省略可能です。 ただし、Apple は提案をサポートする広範な Apple Pay 全体での開発者向けのアプリをすべて 1 つのコード ベースをできるだけ早く新しい API を移動します。 インテントの詳細については、Siri 統合を参照してください、 [SiriKit の概要](~/ios/platform/sirikit/index.md)ドキュメント。

#### <a name="presenting-issuer-cards-from-within-apps"></a>アプリ内の発行者のカードを表示します。

10、iOS で独自のアプリ内から、カードを提示するカードの発行元を許可する PassKit framework の新機能が追加されました。 追加する、 `PKPaymentButtonTypeInStore` UIButton カード用の Apple Pay のボタンを表示するアプリのユーザー インターフェイスにします。

`PresentPaymentPass`のメソッド、 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary)クラスを使用してプログラムでカードを表示することもできます。

### <a name="new-payment-network-support"></a>新しい支払ネットワークのサポート

新しい 10、iOS にアプリに自動的にネットワークをサポートできます新しい支払、開発者が、アプリを再コンパイルをクリックし、アプリ ストアに再送信することがなく利用可能になったときにします。

新しい[AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks)のメソッド、`PKPaymentNetwork`クラスは、実行時に、ユーザーのデバイスで利用可能なネットワークの検出にアプリを使用できます。 さらに、 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks)支払いプロバイダーの名前を引数として受け取るにプロパティが拡張されています。 これらのメソッドを使用して、アプリに自動的がサポートできる支払いプロバイダーをサポートするネットワーク。

詳細についてを参照してください、 [Apple 料金を支払う構成](~/ios/platform/apple-pay.md)と Apple の[Apple のガイドで料金を支払う](https://developer.apple.com/apple-pay/)です。

### <a name="new-testing-environment"></a>新しいテスト環境

10、iOS では、Apple には、開発者は、iOS デバイスで直接テスト支払いカードのプロビジョニングを許可されている新しいテスト環境が導入されました。 この新しいテスト環境は、暗号化されたテスト支払いのデータをアプリに返します。

新しいテスト環境を有効にするには、次の操作を行います。

1. ITunes Connect で新しいテスト iCloud アカウントを作成します。
2. 新しいテスト アカウントを使用して iOS デバイスにログインします。
3. アプリをテストする目的の地域を設定します。
4. テストの支払いカードからの 1 つを使用して、 [Apple のガイドで料金を支払う](https://developer.apple.com/apple-pay/)の支払いを行うにします。

> [!IMPORTANT]
>  **注:** iCloud アカウントを切り替えることによってデバイスは自動的に新しいテスト環境に切り替わります。 ただし、Apple がまだ**必要があります**アプリでテストするのには、iTunes App Store に送信する前に、実稼働環境でカードです。

## <a name="summary"></a>まとめ

この記事では、アプリ内で Apple Pay を使用するために必要なさまざまなアイテムについて説明します。 内での使用方法と、店舗 ID を作成する方法について説明しました、 **Entitlements.plist**、手動で変更する必要があります。


## <a name="related-links"></a>関連リンク

- [アプリ内購入](~/ios/platform/in-app-purchasing/index.md)
- [PassKit の概要](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [店 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
