---
title: Xamarin で watchOS 上の Apple Pay
description: この記事が、拡張機能では Apple は watchOS 3 と Apple Watch の Xamarin.iOS でそれらを実装する方法で Apple Pay に行ったが。
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 354e03ee1e07ba99fcdeb05617bc65ed89f0e8c2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364175"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Xamarin で watchOS 上の Apple Pay

Apple は、アプリでの支払いのサポートを追加する watchOS 3 で Apple Pay にいくつかの機能強化を行ったが。 これにより、ユーザーを安全に支払いを提供し、Apple Watch から直接、物理的な商品およびサービスの支払いに問い合わせができます。


## <a name="about-apple-pay-enhancements"></a>Apple Pay 機能強化について

上記 Stated、として Apple Apple Pay にいくつかの機能強化がセキュリティで保護された支払を許可して、連絡先情報を Apple Watch から直接、物理的な商品およびサービスの支払いに watchOS 3 で行われました。 これらの拡張機能は、PassKit framework の変更によって提供されます。

IOS 10 と watchOS 3 の場合は、いくつかの新しい Api が追加されました iOS と動的な支払いネットワークと新しいサンド ボックス テスト環境をサポートする watchOS の両方を操作します。

## <a name="passkit-framework-enhancements"></a>PassKit Framework の機能強化

外部の Apple Pay をサポートするために、iOS 10 で、PassKit framework が拡張されています`UIKit`とそれぞれのアプリ内から、カードを提示するカードの発行元を許可するようにします。 

### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit の外部で Apple Pay のサポート

使用して[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)、アプリによって提供されるのと同じ機能をサポートできる[PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit を使用せずします。 この新しい API は、Apple Watch 上 (とも、特定の目的で)、Apple Pay をサポートするために必要な既存のアプリ) などの他の状況では省略可能です。 ただし、Apple は、1 つのコード ベースを持つすべての開発者のアプリ全体で、Apple Pay の広範なサポートを提供する、できるだけ早く、新しい API への移行をお勧めします。 インテントの詳細については、Siri の統合を参照してください、 [SiriKit の概要](~/ios/platform/sirikit/index.md)ドキュメント。

### <a name="presenting-issuer-cards-from-within-apps"></a>アプリ内から発行者のカードを表示します。

IOS 10 と watchOS 3 の場合は、独自のアプリ内からのペイメント カードを提示するカードの発行元を許可する PassKit framework の新機能が追加されました。 追加する、 `PKPaymentButtonTypeInStore` UIButton カード用の Apple Pay のボタンを表示するアプリのユーザー インターフェイスにします。

`PresentPaymentPass`のメソッド、 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary)クラスがプログラムでカードを表示することもできます。

## <a name="new-payment-network-support"></a>新しい支払ネットワークのサポート

新しい iOS 10 watchOS 3 をアプリに自動的にネットワークをサポートできます新しい支払い、開発者の変更、アプリを再コンパイルし、アプリ ストアに再送信することがなく利用可能になったときにします。

新しい[AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks)のメソッド、`PKPaymentNetwork`クラスは、実行時に、ユーザーのデバイスで使用可能なネットワークの検出にアプリを使用できます。 さらに、 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks)を引数としての支払いプロバイダーの名前を取得するプロパティが拡張されています。 これらのメソッドを使用して、アプリはできる支払いプロバイダーをサポートするネットワークをサポートに自動的に。

詳細についてを参照してください、 [Apple 支払い構成](~/ios/platform/apple-pay.md)と Apple の[Apple のガイドで支払う](https://developer.apple.com/apple-pay/)します。

## <a name="new-testing-environment"></a>新しいテスト環境

IOS 10 と watchOS 3 の場合は、Apple はにより、開発者は iOS デバイスで直接テスト ペイメント カードをプロビジョニングする新しいテスト環境を導入しました。 この新しいテスト環境は、暗号化されたテストのお支払いデータをアプリに戻ります。

新しいテスト環境を有効にするには、次の操作を行います。

1. ITunes Connect で新しいテストの iCloud アカウントを作成します。
2. 新しいテスト アカウントを使用して iOS デバイスにログインします。
3. アプリをテストする目的のリージョンを設定します。
4. テストのペイメント カードのいずれかを使用して、 [Apple のガイドで支払う](https://developer.apple.com/apple-pay/)に料金を支払います。

> [!NOTE]
> ICloud アカウントを切り替えることで、デバイスは、新しいテスト環境に自動的に切り替わります。 ただし、Apple がまだ**必要があります**iTunes App Store に提出する前に、運用環境で実際にテスト対象のアプリがカードします。

## <a name="summary"></a>まとめ

この記事では、拡張機能をカバーされて Apple は watchOS 3 と Xamarin.iOS でそれらを実装する方法で Apple Pay に行ったが。
