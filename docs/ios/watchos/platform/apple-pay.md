---
title: "Apple watchOS に料金を支払う"
description: "拡張機能を取り上げて Apple がに対して Apple Pay watchOS 3 および Apple Watch の Xamarin.iOS でそれらを実装する方法です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e10a34bc5de16c19f48fa1b869daca9670f37804
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="apple-pay-on-watchos"></a>Apple watchOS に料金を支払う

Apple の拡張機能を Apple のお支払いに向けた watchOS 3 アプリ内の支払いのサポートを追加します。 これにより、ユーザーを安全に支払いを提供し、Apple Watch から直接、物理的な商品およびサービスの支払いに情報を問い合わせてください。


## <a name="about-apple-pay-enhancements"></a>Apple 給与の機能強化について

Stated 上、として Apple がに対していくつかの機能強化 Apple Pay watchOS 3 を可能にするセキュリティで保護された支払いの連絡先情報を Apple Watch から直接、物理的な商品およびサービスの支払いにします。 PassKit フレームワークへの変更によって、これらの機能強化が提供されます。

IOS 10 と watchOS 3 では、いくつかの新しい Api が追加されました iOS プラットフォームと watchOS 動的支払ネットワークと新しいサンド ボックスのテスト環境をサポートするために両方を操作します。

## <a name="passkit-framework-enhancements"></a>PassKit フレームワークの機能強化

Apple の外部の支払いをサポートするために 10、iOS で PassKit フレームワークが拡張されて`UIKit`とそれぞれのアプリ内から、カードを提示するカードの発行元を許可します。 

### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit の外部で Apple 支払をサポートします。

使用して[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)、アプリによって提供されるのと同じ機能をサポートできる[PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit を使用せずします。 この新しい API では、Apple Watch で (または特定の目的にも)、Apple Pay をサポートするために必要なは、(既存のアプリ) などその他の状況では省略可能です。 ただし、Apple は提案をサポートする広範な Apple Pay 全体での開発者向けのアプリをすべて 1 つのコード ベースをできるだけ早く新しい API を移動します。 インテントの詳細については、Siri 統合を参照してください、 [SiriKit の概要](/~/ios/platform/sirikit/index.md)ドキュメント。

### <a name="presenting-issuer-cards-from-within-apps"></a>アプリ内の発行者のカードを表示します。

IOS 10 と watchOS 3 では、独自のアプリ内から、支払いカードを提示するカードの発行元を許可する PassKit framework の新機能が追加されました。 追加する、 `PKPaymentButtonTypeInStore` UIButton カード用の Apple Pay のボタンを表示するアプリのユーザー インターフェイスにします。

`PresentPaymentPass`のメソッド、 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary)クラスを使用してプログラムでカードを表示することもできます。

## <a name="new-payment-network-support"></a>新しい支払ネットワークのサポート

新しい iOS 10 watchOS 3 をアプリに自動的にネットワークをサポートできます新しい支払、開発者が、アプリを再コンパイルをクリックし、アプリ ストアに再送信することがなく利用可能になったときにします。

新しい[AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks)のメソッド、`PKPaymentNetwork`クラスは、実行時に、ユーザーのデバイスで利用可能なネットワークの検出にアプリを使用できます。 さらに、 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks)支払いプロバイダーの名前を引数として受け取るにプロパティが拡張されています。 これらのメソッドを使用して、アプリに自動的がサポートできる支払いプロバイダーをサポートするネットワーク。

詳細についてを参照してください、 [Apple 料金を支払う構成](~/ios/platform/apple-pay.md)と Apple の[Apple のガイドで料金を支払う](https://developer.apple.com/apple-pay/)です。

## <a name="new-testing-environment"></a>新しいテスト環境

IOS 10、watchOS 3 では、Apple には、開発者は、iOS デバイスで直接テスト支払いカードのプロビジョニングを許可されている新しいテスト環境が導入されました。 この新しいテスト環境は、暗号化されたテスト支払いのデータをアプリに返します。

新しいテスト環境を有効にするには、次の操作を行います。

1. ITunes Connect で新しいテスト iCloud アカウントを作成します。
2. 新しいテスト アカウントを使用して iOS デバイスにログインします。
3. アプリをテストする目的の地域を設定します。
4. テストの支払いカードからの 1 つを使用して、 [Apple のガイドで料金を支払う](https://developer.apple.com/apple-pay/)の支払いを行うにします。

> ⚠️ **注:** iCloud アカウントを切り替えることによってデバイスは自動的に新しいテスト環境に切り替わります。 ただし、Apple がまだ**必要があります**アプリでテストするのには、iTunes App Store に送信する前に、実稼働環境でカードです。

## <a name="summary"></a>まとめ

この記事は、拡張機能をカバーされて Apple がに対して Apple Pay watchOS 3 と Xamarin.iOS でそれらを実装する方法です。
