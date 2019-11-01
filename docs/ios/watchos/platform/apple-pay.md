---
title: Xamarin での watchOS の Apple Pay
description: この記事では、watchOS 3 での Apple の Apple Pay に加えられた機能強化と、Xamarin での Apple Watch の実装方法について説明します。
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 372b034b7e14f3cfaadde8fe5a5370e368f161db
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030125"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Xamarin での watchOS の Apple Pay

Apple では、アプリ内支払いのサポートを追加する watchOS 3 の Apple Pay に対していくつかの機能強化が行われています。 これにより、ユーザーは、Apple Watch から直接、物理的な商品やサービスに対して支払いを行うために、支払いと連絡先情報を安全に提供できます。

## <a name="about-apple-pay-enhancements"></a>Apple Pay の拡張機能について

前述のように、Apple は watchOS 3 の Apple Pay に対していくつかの機能強化を行っています。これにより、セキュリティで保護された支払いと連絡先情報を、Apple Watch から直接、物理的な商品やサービスに対して支払うことができます。 これらの機能強化は、Pass Kit フレームワークに対する変更によって提供されます。

IOS 10 と watchOS 3 では、iOS と watchOS の両方を使用して、動的な支払いネットワークと新しいサンドボックステスト環境をサポートする新しい Api がいくつか追加されています。

## <a name="passkit-framework-enhancements"></a>Pass Kit フレームワークの機能強化

IOS 10 では、Pass Kit フレームワークが拡張され、`UIKit` の外部 Apple Pay がサポートされるようになり、カードの発行者がアプリ内からカードを提示できるようになりました。 

### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit 外での Apple Pay のサポート

[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)を使用すると、アプリは、uikit を使用せずに[PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller)が提供するものと同じ機能をサポートできます。 この新しい API は、Apple Watch で Apple Pay をサポートするために必要ですが (具体的には、特定の目的でも)、他の状況 (既存のアプリなど) では省略可能です。 ただし、Apple では、できるだけ早く新しい API に移行することをお勧めします。これにより、1つのコードベースを持つすべての開発者のアプリで幅広い Apple Pay サポートを提供できるようになります。 インテントと Siri の統合の詳細については、 [SiriKit の概要に](~/ios/platform/sirikit/index.md)関するドキュメントを参照してください。

### <a name="presenting-issuer-cards-from-within-apps"></a>アプリ内での発行者カードの提示

IOS 10 と watchOS 3 では、カードの発行者が独自のアプリ内から支払いカードを提示できるようにする新しい機能が、Pass Kit フレームワークに追加されました。 開発者は、カードの Apple Pay ボタンを表示するアプリのユーザーインターフェイスに `PKPaymentButtonTypeInStore` UIButton を追加できます。

[Pkpass library](https://developer.apple.com/reference/passkit/pkpasslibrary)クラスの `PresentPaymentPass` メソッドを使用して、カードをプログラムで表示することもできます。

## <a name="new-payment-network-support"></a>新しい支払いネットワークのサポート

IOS 10 と watchOS 3 を初めて使用する場合、アプリは、開発者がアプリケーションを変更して再コンパイルし、アプリストアに再送信することなく、新しい支払いネットワークが利用可能になったときに自動的にサポートできます。

`PKPaymentNetwork` クラスの新しい[AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks)メソッドを使用すると、アプリは実行時にユーザーのデバイスで利用可能なネットワークを検出できます。 さらに、 [Supportednetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks)プロパティが拡張され、支払プロバイダーの名前が引数として使用されるようになりました。 これらの方法を使用すると、アプリは、支払いプロバイダーがサポートする任意のネットワークを自動的にサポートできます。

詳細については、 [Apple Pay の構成](~/ios/platform/apple-pay.md)と Apple の[Apple Pay ガイド](https://developer.apple.com/apple-pay/)を参照してください。

## <a name="new-testing-environment"></a>新しいテスト環境

IOS 10 と watchOS 3 では、Apple は新しいテスト環境を導入しました。これにより、開発者は、iOS デバイスでテスト用の支払いカードを直接プロビジョニングできます。 この新しいテスト環境は、暗号化されたテストの支払いデータをアプリに返します。

新しいテスト環境を有効にするには、次の手順を実行します。

1. ITunes Connect で、新しいテスト用 iCloud アカウントを作成します。
2. 新しいテストアカウントを使用して iOS デバイスにログインします。
3. アプリをテストする目的のリージョンを設定します。
4. [Apple Pay ガイド](https://developer.apple.com/apple-pay/)のいずれかのテスト支払カードを使用して、支払いを行います。

> [!NOTE]
> ICloud アカウントを切り替えると、デバイスは自動的に新しいテスト環境に切り替わります。 ただし、Apple では、iTunes App Store に送信する前に、実稼働環境で実際のカードを使用してアプリをテストする**必要があり**ます。

## <a name="summary"></a>まとめ

この記事では、watchOS 3 での Apple の Apple Pay に加えられた拡張機能と、それらを Xamarin. iOS で実装する方法について説明しました。
