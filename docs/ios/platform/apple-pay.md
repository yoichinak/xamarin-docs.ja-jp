---
title: Xamarin. iOS の Apple Pay
description: このガイドでは、Apple Pay と共に使用して、食品、エンターテイメント、メンバーシップなどの物理的な商品をアプリで支払うための Xamarin の iOS 環境の設定について説明します。 これには、必要な識別子、証明書、および権利に関する情報が含まれます。
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: f264f210a9228fd213f0c041abb5b26023c796f4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753267"
---
# <a name="apple-pay-in-xamarinios"></a>Xamarin. iOS の Apple Pay

_このガイドでは、Apple Pay と共に使用して、食品、エンターテイメント、メンバーシップなどの物理的な商品をアプリで支払うための Xamarin の iOS 環境の設定について説明します。これには、必要な識別子、証明書、および権利に関する情報が含まれます。_

Apple Pay は、iOS 8 と共に導入され、ユーザーは iOS デバイスを介して食品、エンターテイメント、メンバーシップなどの物理的な商品を支払うことができます。 IPhone 6 および iPhone 6 Plus で利用できます。また、ストア内購入の Apple Watch と組み合わせて使用することもできます。 IPhone で使用する場合、ユーザーのクレジットカードまたはデビットカードに対するトランザクションを確認して承認する手段として、Touch ID を使用します。

## <a name="requirements"></a>必要条件

Apple Pay は、iOS 8 以降でのみ使用できます。したがって、少なくとも Xcode 6 が必要です。

Apple Pay をアプリに統合するには、次の項目も必要です。

- 支払いプロセッサのプラットフォーム
- マーチャント識別子
- Apple Pay 証明書
- Apple Pay の権利

このドキュメントでは、これらの項目について詳しく説明します。

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay と IAP の違い

Apple Pay と*アプリ内購入*(iap) の主な違いは、販売する製品に関連しています。 *物理的*な商品は Apple Pay を通じて販売されます。食べ物、宿泊施設、および物理的なエンターテイメント (映画チケットなど) はすべて、この例の一例です。 これに対し、IAP は、premium や追加のコンテンツ、サブスクリプションなどの*仮想*商品を販売しています。これは、ストリーミングサービスの追加の月やゲームの追加の生活を考えます。

使用されるフレームワークも重要な違いです。Apple Pay には[Pass kit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)が使用されますが、 [storekit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)には iap 用のフレームワーク API が用意されています。

Apple Pay では、 [Apple は](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf)、"[はユーザー、商人、または開発者は支払いに Apple Pay を使用しません" ということを示しています。 これに対して、IAP ではトランザクションごとに 30% の料金が請求されます。 さらに、Apple Pay では、トランザクションが Apple を経由することはありません。代わりに、支払いプラットフォームが使用されます。

## <a name="using-a-payment-processor-platform"></a>支払いプロセッサプラットフォームの使用

Apple Pay の基本的な部分の1つは、支払い処理です。 この操作は自分で行うことができますが、暗号化に関するかなりの知識が必要です。
- 詳細については、「Apple の[支払い処理ガイド](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)」を参照してください。
一方、支払い処理プラットフォームでは、これらの操作を処理して、アプリの構築に専念することができます。

次の2つのオプションがあります。

- [Stripe.com](https://stripe.com/)にサインアップし**て、api**にアクセスします。

- **JudoPay** - [github で Xamarin サンプルコード](https://github.com/Judopay/Xamarin-Sample-App)を確認し、 [JudoPay.com](https://www.judopay.com/)に登録します。

## <a name="provisioning-for-apple-pay"></a>Apple Pay のプロビジョニング

Apple Pay を使用するようにアプリを構成するには、Apple Developer ポータルおよびアプリ内でのセットアップが必要です。 Apple の支払い用にアプリを正常にプロビジョニングするには、次のようないくつかの手順を実行する必要があります。

1. マーチャント ID を作成します。
    - こちらの手順に従って[ください](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. 支払い機能を適用してアプリ ID を作成し、それにマーチャントを追加します。
    - こちらの手順に従って[ください](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. マーチャント ID の証明書を生成します。
    - こちらの手順に従って[ください](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. 新しく作成されたアプリ ID でプロビジョニングプロファイルを生成します。
    - こちらの手順に従って[ください](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay の権利を追加します。
    - [ここ](~/ios/deploy-test/provisioning/entitlements.md)で詳しく説明されている Apple の支払い資格を選択するか、キーと値のペアを[ここ](~/ios/deploy-test/provisioning/entitlements.md)からファイルに手動で追加します。

## <a name="working-with-apple-pay"></a>Apple Pay の操作

Apple は iOS 10 の Apple Pay に対していくつかの機能強化を行いました。これにより、ユーザーは、web サイトからのセキュリティで保護された支払いを行うことができ、Siri および Maps との対話が可能に

IOS 10 では、iOS と watchOS の両方を使用して、動的な支払いネットワークと新しいサンドボックステスト環境をサポートする新しい Api がいくつか追加されています。

### <a name="apple-pay-website-integration"></a>Apple Pay Web サイトの統合

IOS 10 を初めて使用する場合、開発者は**APPLEPAY JS**を使用して web サイトに直接 Apple Pay を組み込むことができます。 IOS または macOS で Safari を使用して web サイトを参照しているユーザーは、iPhone または Apple Watch でトランザクションを検証することによって、Apple Pay で支払いを行うことができます。 詳細については、Apple の[APPLEPAY JP Framework リファレンス](https://developer.apple.com/reference/applepayjs)を参照してください。

### <a name="passkit-framework-enhancements"></a>Pass Kit フレームワークの機能強化

IOS 10 では、またはの`UIKit`外部で Apple Pay をサポートするように、pass kit フレームワークが拡張されており、カードの発行者がアプリ内から独自のカードを提示できるようになりました。

#### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit 外での Apple Pay のサポート

[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)を使用すると、アプリは、uikit を使用せずに[PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller)が提供するものと同じ機能をサポートできます。 この新しい API は、Apple Watch で Apple Pay をサポートするために必要ですが (具体的には、特定の目的でも)、他の状況 (既存のアプリなど) では省略可能です。 ただし、Apple では、できるだけ早く新しい API に移行することをお勧めします。これにより、1つのコードベースを持つすべての開発者のアプリで幅広い Apple Pay サポートを提供できるようになります。 インテントと Siri の統合の詳細については、 [SiriKit の概要に](~/ios/platform/sirikit/index.md)関するドキュメントを参照してください。

#### <a name="presenting-issuer-cards-from-within-apps"></a>アプリ内での発行者カードの提示

IOS 10 では、カードの発行者が独自のアプリ内からカードを提示できるようにする新しい機能が、Pass Kit フレームワークに追加されています。 開発者は、カード`PKPaymentButtonTypeInStore`の Apple Pay ボタンを表示する uibutton をアプリのユーザーインターフェイスに追加できます。

[Pkpass library](https://developer.apple.com/reference/passkit/pkpasslibrary)クラスのメソッドを使用して、カードをプログラムで表示することもできます。`PresentPaymentPass`

### <a name="new-payment-network-support"></a>新しい支払いネットワークのサポート

IOS 10 を初めて使用する場合、アプリは、開発者がアプリケーションを変更して再コンパイルし、アプリストアに再送信することなく、新しい支払いネットワークが利用可能になったときに自動的にサポートできます。

`PKPaymentNetwork`クラスの new [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks)メソッドを使用すると、アプリは実行時にユーザーのデバイスで利用可能なネットワークを検出できます。 さらに、 [Supportednetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks)プロパティが拡張され、支払プロバイダーの名前が引数として使用されるようになりました。 これらの方法を使用すると、アプリは、支払いプロバイダーがサポートする任意のネットワークを自動的にサポートできます。

詳細については、 [Apple Pay の構成](~/ios/platform/apple-pay.md)と Apple の[Apple Pay ガイド](https://developer.apple.com/apple-pay/)を参照してください。

### <a name="new-testing-environment"></a>新しいテスト環境

IOS 10 では、開発者が iOS デバイスで直接テスト用の支払いカードをプロビジョニングできる新しいテスト環境が導入されました。 この新しいテスト環境は、暗号化されたテストの支払いデータをアプリに返します。

新しいテスト環境を有効にするには、次の手順を実行します。

1. ITunes Connect で、新しいテスト用 iCloud アカウントを作成します。
2. 新しいテストアカウントを使用して iOS デバイスにログインします。
3. アプリをテストする目的のリージョンを設定します。
4. [Apple Pay ガイド](https://developer.apple.com/apple-pay/)のいずれかのテスト支払カードを使用して、支払いを行います。

> [!IMPORTANT]
> ICloud アカウントを切り替えると、デバイスは自動的に新しいテスト環境に切り替わります。 ただし、Apple では、iTunes App Store に送信する前に、実稼働環境で実際のカードを使用してアプリをテストする**必要があり**ます。

## <a name="summary"></a>Summary

この記事では、アプリ内で Apple Pay を使用するために必要なさまざまな項目について説明します。 ここでは、マーチャント ID を作成する方法と、手動で変更する必要がある**権利を plist**で使用する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [アプリ内購入](~/ios/platform/in-app-purchasing/index.md)
- [Pass Kit の入門](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-emporium)
