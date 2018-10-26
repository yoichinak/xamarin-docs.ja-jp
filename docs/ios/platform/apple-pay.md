---
title: Xamarin.iOS で Apple Pay
description: このガイドでは、食品、エンターテイメント、アプリを使用してメンバーシップなどの物理的な商品の支払いの Apple Pay を使用するための Xamarin.iOS の環境のセットアップについて説明します。 識別子が必要な証明書および権利に関する情報が含まれています。
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: b971029ff3b2b1e8f5e63233d1d754c44b0e3309
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110096"
---
# <a name="apple-pay-in-xamarinios"></a>Xamarin.iOS で Apple Pay

_このガイドでは、食品、エンターテイメント、アプリを使用してメンバーシップなどの物理的な商品の支払いの Apple Pay を使用するための Xamarin.iOS の環境のセットアップについて説明します。識別子が必要な証明書および権利に関する情報が含まれています。_

Apple Pay が導入されましたと共に iOS 8、食品、エンターテイメント、自分の iOS デバイスを使用してメンバーシップなどの物理的な商品の支払いにユーザーを有効にします。 IPhone 6 および iPhone 6 では利用また、ストア内での購入の Apple Watch とペアにもなりますが。 IPhone で使用すると、確認し、ユーザーのクレジット_カードまたはデビット カードにトランザクションを承認する方法として Touch ID を使用します。

## <a name="requirements"></a>必要条件

Apple Pay、のみ内での iOS 8 以降を使用し、したがって Xcode 6 の最小値が必要です。

次の項目も、アプリに Apple Pay を統合するために必要。

 - 支払いのプロセッサ プラットフォーム
 - マーチャント識別子
 - Apple Pay の証明書
 - Apple Pay のエンタイトルメント

このドキュメントはこれらの項目について詳しく説明します。

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay と IAP の違い

Apple Pay の主な違いと*アプリ内購入*(IAP) は、販売する製品に関連します。 *物理*Apple Pay を使用して商品が販売されている; 食品、宿泊施設/(映画チケット) などの物理的なエンターテイメントはすべて、これの例です。 これに対し、IAP を販売して*仮想*premium または他のコンテンツ、およびストリーミング サービスでは、ゲームで余分な生活がどれほどの追加の月をサブスクリプション – 待ち時間などの商品です。

使用するフレームワークも主な相違点です。[PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)は Apple Pay の使用、 [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP のフレームワーク API を提供します。

Apple Pay、Apple と[状態](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf)こと、"[] は料金がかかりません支払に Apple Pay を使用するには、ユーザー、マーチャントまたは開発者"です。 比較、IAP トランザクションごとに 30% の料金があります。 さらに、Apple Pay でトランザクションを経由しない Apple まったく、支払いプラットフォーム経由になります、代わりにします。

## <a name="using-a-payment-processor-platform"></a>支払いのプロセッサ プラットフォームを使用してください。

Apple Pay の基本的な部分の 1 つは、支払いの処理です。 手動で行うことはできますが、暗号化のかなりの知識が必要があります。
- Apple ので説明されている[支払処理ガイド](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)します。
支払い処理のプラットフォームは、その一方で、アプリの構築に専念することが、これらの操作を処理します。

2 つのオプションは次のとおりです。

- **Stripe** -でサインアップ[Stripe.com](https://stripe.com/)その Api にアクセスします。

- **JudoPay** -チェック アウト、 [github の Xamarin サンプル コード](https://github.com/Judopay/Xamarin-Sample-App)、登録と[JudoPay.com](https://www.judopay.com/)します。

## <a name="provisioning-for-apple-pay"></a>Apple Pay のプロビジョニング

Apple Pay を使用するアプリを構成すると、アプリ内で、Apple Developer ポータルでセットアップする必要があります。 Apple pay のアプリを正常にプロビジョニングする必要がありますの手順を数多くあります。

1. マーチャント ID を作成します。
    - 手順に従います[ここ](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Apply Pay 機能をアプリ ID を作成し、マーチャントを追加します。
    - 手順に従います[ここ](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. マーチャント ID の証明書を生成します。
    - 手順に従います[ここ](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. 新しく作成したアプリ ID とプロビジョニング プロファイルを生成します。
    - 手順に従います[ここ](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay のエンタイトルメントを追加します。
    - Apple pay のエンタイトルメントを詳しく説明されている選択[ここ](~/ios/deploy-test/provisioning/entitlements.md)からファイルをキー/値ペアを手動で追加または[ここ](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Apple Pay の使用

Apple は、Siri とマップとの対話と web サイトから支払いのセキュリティで保護されたユーザーに許可する iOS 10 で、Apple Pay にいくつかの機能強化を行ったが。

IOS 10 では、いくつかの新しい Api が追加されました iOS と動的な支払いネットワークと新しいサンド ボックス テスト環境をサポートする watchOS の両方を操作します。

### <a name="apple-pay-website-integration"></a>Apple Pay の web サイトの統合

新しい ios 10 では、開発者組み込むことができます Apple Pay を使用して、web サイトに直接**ApplePay JS**します。 Safari で iOS または macOS での web サイトを閲覧するユーザーは、iPhone または Apple Watch でのトランザクションを検証することで Apple Pay の支払を行うことができます。 詳細については、Apple を参照してください[ApplePay JP フレームワーク参照](https://developer.apple.com/reference/applepayjs)します。

### <a name="passkit-framework-enhancements"></a>PassKit Framework の機能強化

外部の Apple Pay をサポートするために、iOS 10 で、PassKit framework が拡張されています`UIKit`と、アプリ内から独自のカードを提示するカードの発行元を許可するようにします。


#### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit の外部で Apple Pay のサポート

使用して[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)、アプリによって提供されるのと同じ機能をサポートできる[PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit を使用せずします。 この新しい API は、Apple Watch 上 (とも、特定の目的で)、Apple Pay をサポートするために必要な既存のアプリ) などの他の状況では省略可能です。 ただし、Apple は、1 つのコード ベースを持つすべての開発者のアプリ全体で、Apple Pay の広範なサポートを提供する、できるだけ早く、新しい API への移行をお勧めします。 インテントの詳細については、Siri の統合を参照してください、 [SiriKit の概要](~/ios/platform/sirikit/index.md)ドキュメント。

#### <a name="presenting-issuer-cards-from-within-apps"></a>アプリ内から発行者のカードを表示します。

10、iOS で独自のアプリ内から、カードを提示するカードの発行元を許可する PassKit framework の新機能が追加されました。 追加する、 `PKPaymentButtonTypeInStore` UIButton カード用の Apple Pay のボタンを表示するアプリのユーザー インターフェイスにします。

`PresentPaymentPass`のメソッド、 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary)クラスがプログラムでカードを表示することもできます。

### <a name="new-payment-network-support"></a>新しい支払ネットワークのサポート

新しい ios 10 では、アプリに自動的にネットワークをサポートできます新しい支払い、開発者の変更、アプリを再コンパイルし、アプリ ストアに再送信することがなく利用可能になったときにします。

新しい[AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks)のメソッド、`PKPaymentNetwork`クラスは、実行時に、ユーザーのデバイスで使用可能なネットワークの検出にアプリを使用できます。 さらに、 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks)を引数としての支払いプロバイダーの名前を取得するプロパティが拡張されています。 これらのメソッドを使用して、アプリはできる支払いプロバイダーをサポートするネットワークをサポートに自動的に。

詳細についてを参照してください、 [Apple 支払い構成](~/ios/platform/apple-pay.md)と Apple の[Apple のガイドで支払う](https://developer.apple.com/apple-pay/)します。

### <a name="new-testing-environment"></a>新しいテスト環境

IOS 10 では、Apple により、開発者は iOS デバイスで直接テスト ペイメント カードをプロビジョニングする新しいテスト環境を導入しました。 この新しいテスト環境は、暗号化されたテストのお支払いデータをアプリに戻ります。

新しいテスト環境を有効にするには、次の操作を行います。

1. ITunes Connect で新しいテストの iCloud アカウントを作成します。
2. 新しいテスト アカウントを使用して iOS デバイスにログインします。
3. アプリをテストする目的のリージョンを設定します。
4. テストのペイメント カードのいずれかを使用して、 [Apple のガイドで支払う](https://developer.apple.com/apple-pay/)に料金を支払います。

> [!IMPORTANT]
> ICloud アカウントを切り替えることで、デバイスは、新しいテスト環境に自動的に切り替わります。 ただし、Apple がまだ**必要があります**iTunes App Store に提出する前に、運用環境で実際にテスト対象のアプリがカードします。

## <a name="summary"></a>まとめ

この記事では、アプリ内で Apple Pay を使用するために必要なさまざまなアイテムを説明します。 マーチャント ID を作成する方法と内での使用方法を説明しました、 **Entitlements.plist**、手動で変更する必要があります。

## <a name="related-links"></a>関連リンク

- [アプリ内購入](~/ios/platform/in-app-purchasing/index.md)
- [PassKit の概要](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [店 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
