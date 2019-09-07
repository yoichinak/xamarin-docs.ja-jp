---
title: tvOS 10 の概要
description: この記事では、tvOS 10 for Xamarin. tvOS 開発者向けの新しい Api と変更された Api と機能をすべて紹介します。
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 8c338f8a5b2f1d41b1ea0f61778a1c14eb84ce08
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769155"
---
# <a name="introduction-to-tvos-10"></a>tvOS 10 の概要

_この記事では、tvOS 10 for Xamarin. tvOS 開発者向けの新しい Api と変更された Api と機能をすべて紹介します。_

新しい tvOS 10 SDK Apple には、開発者がアプリと機能の新しいカテゴリを作成するための新しい Api とサービスが含まれています。 

TvOS 10 の詳細については、Apple の[tvOS + Apps](https://developer.apple.com/tvos/)のドキュメントを参照してください。

## <a name="whats-new-in-tvos-10"></a>TvOS 10 の新機能

Apple では、tvOS 10 に新しい Api とサービスがいくつか追加され、既存の機能に対する多くの機能強化が加えられています。

## <a name="new-user-interface-styles"></a>新しいユーザーインターフェイスのスタイル

tvOS 10 では、ユーザーの設定に基づいて、すべての組み込み UIKit コントロールが自動的に対応するように、ダークおよびライトの両方のユーザーインターフェイステーマをサポートするようになりました。

新しいカスタム UI コントロールを作成して実装する場合、開発者は、 [Uitraitcollection](https://developer.apple.com/reference/uikit/uitraitcollection)クラスを使用して、ユーザーが選択したテーマに適合させる必要があります。

詳細については、[新しいユーザーインターフェイススタイル](~/ios/tvos/platform/user-interface-styles.md)のドキュメントを参照してください。

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの強化

Apple は tvOS 10 のセキュリティとプライバシーの両方に対していくつかの機能強化を行っており、開発者はアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保することができます。

結果として、watchOS 3 (またはそれ以降) で実行されるアプリは、特定の機能またはユーザー情報にアクセスする目的を静的に宣言`Info.plist`する必要があります。そのためには、アプリがアクセスする必要がある理由を説明する1つまたは複数のプライバシー固有のキーをファイルに入力します。

TvOS 10 はこれらの変更を iOS 10 と共有しているため、詳細については、iOS 10 の[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するガイドを参照してください。

## <a name="video-subscriber-account"></a>ビデオ購読者アカウント

TvOS 10 の新機能であるビデオサブスクライバーアカウントフレームワークを使用すると、認証されたストリーミングまたはビデオオンデマンドをサポートするアプリで、エンドユーザーにシングルサインインエクスペリエンスを使用して、ケーブルまたは衛星放送会社による認証を行うことができます。

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>広色域

tvOS 10 は、コアグラフィックス、コアイメージ、メタル、AVFoundation などのフレームワークを含む、システム全体で拡張範囲のピクセル形式と広い範囲の色空間のサポートを拡張します。 グラフィックススタック全体でこの動作を提供することにより、さまざまな色で表示されるデバイスのサポートがさらに緩和さます。

また、 `UIKit`は新しい extended **sRGB** colorspace で動作するように変更されています。これにより、パフォーマンスが大幅に低下することなく、広い色域で色を簡単に混在させることができます。

Apple では、広範囲にわたる色を使用するときに、次のベストプラクティスを提供しています。

- `UIColor`では、sRGB 色空間が使用されるようになり、 `0.0`値`1.0`が to 範囲にクランプされなくなりました。 アプリが以前のクランプ動作に依存している場合は、tvOS 10 に対して変更する必要があります。
- アプリでの`UIImages`カスタムレンダリングを実行する場合は、新しい[UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer)クラスを使用して、拡張範囲または標準の範囲形式の使用を指定します。
- コアグラフィックスや金属などの低レベルの API を使用してイメージ処理を行う場合、アプリでは16ビット浮動小数点値をサポートする拡張範囲の色空間とピクセル形式を使用する必要があります。 必要に応じて、アプリで色コンポーネントの値を手動で固定する必要があります。
- コアグラフィックス、コアイメージ、および金属パフォーマンスシェーダーはすべて、2つの色空間間で変換を行うための新しいメソッドを提供します。

詳細については、「 [Wide カラー](~/ios/platform/wide-color.md)ガイドの概要」を参照してください。

## <a name="newly-available-existing-frameworks"></a>新しく利用可能な既存のフレームワーク

IOS で利用可能であった (tvOS ではない) いくつかのフレームワークは、次のような tvOS 10 で使用できます。

- ExternalAccessory
- HomeKit
- MultipeerConnectivity
- フォト
- ReplayKit
- UserNotification

## <a name="additional-framework-changes"></a>追加のフレームワークの変更

Apple では、上に示した主要なフレームワークの変更と追加に加えて、tvOS 10 で多くのマイナーフレームワーク変更が加えられています。

詳細については、追加の[フレームワーク変更](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md)ガイドを参照してください。

## <a name="deprecated-apis"></a>非推奨の API

TvOS 10 で非推奨とされた Api またはフレームワークはありません。 API の変更の完全な一覧については、Apple の[tvOS 10 api の相違点](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html)に関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
