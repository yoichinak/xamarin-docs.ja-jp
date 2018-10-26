---
title: TvOS 10 の概要
description: この記事では、Xamarin.tvOS 開発者向けのすべての新規および変更した Api と tvOS 10 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 260d01d6aa8344dd3cf107f1ffc34167c457a491
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120984"
---
# <a name="introduction-to-tvos-10"></a>TvOS 10 の概要

_この記事では、Xamarin.tvOS 開発者向けのすべての新規および変更した Api と tvOS 10 で使用できる機能を紹介します。_

新しい tvOS では新しい Api とサービス アプリおよび機能の新しいカテゴリを作成する開発者を 10 SDK Apple が含まれます。 

TvOS 10 の詳細については、Apple を参照してください[tvOS アプリ +](https://developer.apple.com/tvos/)ドキュメント。

## <a name="whats-new-in-tvos-10"></a>TvOS 10 を新します。

Apple には、多くの機能強化を含め、既存の機能と共に tvOS 10 でいくつかの新しい Api やサービスが追加します。

## <a name="new-user-interface-styles"></a>新しいユーザー インターフェイスのスタイル

tvOS 10 に、サポートを自動的に制御するすべてのビルドの UIKit ダークおよびライト ユーザー インターフェイスの両方のテーマの適応ここでは、ユーザーの設定に基づいています。

作成すると、新しいカスタムの UI コントロールを実装する、開発者が使用する必要があります、 [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection)ユーザーの選択したテーマに適応するクラス。

詳細についてを参照してください、[新しいユーザー インターフェイスのスタイル](~/ios/tvos/platform/user-interface-styles.md)ドキュメント。

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple は、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立つ tvOS 10 のプライバシーとセキュリティの両方にいくつかの機能強化をしました。

その結果、アプリで watchOS 3 (以降) で実行されている必要がありますで 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能やユーザー情報にアクセスしようとすると、静的に宣言、`Info.plist`ファイルをユーザー、アプリがアクセスしようとした理由を説明します。

TvOS 10 では、iOS 10 とこれらの変更を共有するため、iOS 10 を参照してください[セキュリティおよびプライバシーの強化機能](~/ios/app-fundamentals/security-privacy.md)詳細情報をガイドします。

## <a name="video-subscriber-account"></a>ビデオのサブスクライバー アカウント

新しい tvOS 10 のビデオ サブスクライバー アカウント フレームワークでアプリその認証のストリーミング サポートまたはビデオ オンデマンドで、ケーブル会社または衛星テレビ放送、エンドユーザーは、単一のサインイン エクスペリエンスを使用して認証します。

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>色

tvOS 10 では、拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属製および AVFoundation などのフレームワークを含めて、システム全体で全体の色域スペースのサポートを拡張します。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

さらに、`UIKit`が変更されている、新しい機能を拡張**sRGB**大幅なパフォーマンス失わずワイド色域にカラーの混合をやすくするための色空間。

Apple は、さまざまな色を使用する場合、次のベスト プラクティスを提供します。

 - `UIColor` これは、sRGB 色空間となることはありませんクランプする値、`0.0`に`1.0`範囲。 アプリは、以前のクランプ動作に依存する場合は、tvOS 10 に変更する必要があります。
 - アプリでのカスタム レンダリングを実行する場合`UIImages`、使用して、新しい[UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer)拡張範囲または範囲の標準形式の使用を指定するクラス。
 - コア グラフィックスや金属などの低レベルの API を使用して、イメージの処理を提供する、アプリは、16 ビット浮動小数点値をサポートする拡張範囲の色領域とピクセル形式を使用する必要があります。 必要に応じて、アプリが色コンポーネントの値を手動でクランプする必要があります。
 - コア グラフィックス、Core イメージおよび金属パフォーマンス シェーダーは、2 つのカラー スペース間で変換するための新しいメソッドを提供します。

詳細については、次を参照してください、[色の概要](~/ios/platform/wide-color.md)ガイド。

## <a name="newly-available-existing-frameworks"></a>新しく使用可能な既存のフレームワーク

IOS (および tvOS ではない) で使用可能になったいくつかのフレームワークが加えられました tvOS 10 の使用など。

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - 写真
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

だけでなく、主要なフレームワークの変更と上記に一覧表示されます、Apple は tvOS 10 の多くの他の軽微なフレームワーク変更を加えたが。

詳細については、次を参照してください、 [Framework 変更](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md)ガイド。

## <a name="deprecated-apis"></a>非推奨の API

Api またはフレームワーク非推奨とされたなしで tvOS 10。 Apple を参照してください。 [tvOS 10 API の相違点](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html)API の変更の完全な一覧についてはドキュメントです。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
