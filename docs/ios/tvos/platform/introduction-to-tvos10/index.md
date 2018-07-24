---
title: TvOS 10 の概要
description: この記事では、Xamarin.tvOS 開発者向けのすべての新しいまたは変更された Api と tvOS 10 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 642a851cbfc0450ee8f5f5c4d798c40778e3d3dd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30779756"
---
# <a name="introduction-to-tvos-10"></a>TvOS 10 の概要

_この記事では、Xamarin.tvOS 開発者向けのすべての新しいまたは変更された Api と tvOS 10 で使用できる機能を紹介します。_

新しい tvOS では、10 の SDK Apple にが新しい Api と、開発者はアプリおよび機能の新しいカテゴリの作成を有効にするサービスに含まれます。 

TvOS 10 の詳細については、Apple を参照してください[tvOS + アプリ](https://developer.apple.com/tvos/)ドキュメント。

## <a name="whats-new-in-tvos-10"></a>新機能 tvOS 10

Apple には、多くの機能強化を含め、既存の機能にと共に tvOS 10 でいくつかの新しい Api とサービスが追加します。

## <a name="new-user-interface-styles"></a>新しいユーザー インターフェイスのスタイル

tvOS 10 サポート Dark とライト ユーザー インターフェイスの両方のテーマを自動的にコントロールのすべてのビルドで UIKit を合わせて、これは、ユーザーの設定に基づいています。

開発者が使用する必要がありますを作成し、新しいカスタムの UI コントロールを実装するときに、 [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection)ユーザーの選択したテーマに適応するクラス。

詳細についてを参照してください、[新しいユーザー インターフェイスのスタイル](~/ios/tvos/platform/user-interface-styles.md)ドキュメント。

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple では、両方のセキュリティとプライバシー、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立つ tvOS 10 にいくつかの機能強化が行わします。

その結果、watchOS 3 (以降) で実行されている必要があります静的に宣言の 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能、またはユーザー情報にアクセスしようとすると、その`Info.plist`ファイルをアプリがアクセスしようとした理由をユーザーに説明します。

TvOS 10 では、iOS 10 でこれらの変更を共有するため、iOS 10 を参照してください[セキュリティとプライバシーの機能強化](~/ios/app-fundamentals/security-privacy.md)についてガイドします。

## <a name="video-subscriber-account"></a>ビデオのサブスクライバーのアカウント

新しい tvOS 10 のビデオ サブスクライバー アカウント フレームワーク アプリがその認証のストリーミング サポートまたはそのケーブルまたは衛星放送テレビ プロバイダーと、エンドユーザーの単一のサインイン エクスペリエンスを使用して認証するビデオ オンデマンドします。

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>広色

tvOS 10 では、拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属および AVFoundation などのフレームワークを含む、システム全体で全体の色域スペースのサポートを拡張します。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、グラフィック スタック全体におけるこの動作を提供することにより容易になります。

さらに、`UIKit`が変更された新しい機能を拡張**sRGB**大幅なパフォーマンスを失うことがなくワイド色域内の色を混在させるをやすくするための色空間です。

幅の色を扱うときに、Apple は次のベスト プラクティスを提供します。

 - `UIColor` これで、使用、sRGB 領域の色およびしなくなります固定値を`0.0`に`1.0`範囲です。 アプリは、以前のクランプ動作に依存する場合は、tvOS 10 に変更する必要があります。
 - アプリのカスタムの表示を実行する場合`UIImages`、使用して、新しい[UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer)拡張範囲または標準範囲の形式の使用を指定するクラス。
 - コア グラフィックや金属などの低レベルの API を使用して、イメージ処理を提供する、アプリは 16 ビット浮動小数点値をサポートする拡張範囲色領域とのピクセル形式を使用する必要があります。 必要に応じて、アプリがコンポーネントの色の値を手動で固定する必要があります。
 - コア グラフィックス、Core のイメージとメタル パフォーマンス シェーダーは、2 つのカラー スペース間で変換するための新しいメソッドを提供します。

詳細については、次を参照してください、[広色の概要](~/ios/platform/wide-color.md)ガイドです。

## <a name="newly-available-existing-frameworks"></a>新しく利用できる既存のフレームワーク

IOS (およびいない tvOS) で使用可能であったいくつかのフレームワークが使用可能な tvOS 10 のなど。

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - 写真
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

に加えて、主要なフレームワークの変更と追加機能の上に示した Apple 追加マイナー フレームワークの多くの変更に向けた tvOS 10 です。

詳細については、次を参照してください、 [Framework の変更の追加](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md)ガイドです。

## <a name="deprecated-apis"></a>非推奨 API

Api、またはフレームワークを非推奨となったありません tvOS 10 でします。 Apple を参照してください[tvOS 10 API の相違点](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html)API の変更の完全な一覧についてはドキュメントです。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
