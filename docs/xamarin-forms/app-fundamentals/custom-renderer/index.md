---
title: Xamarin.Forms のカスタム レンダラー
description: カスタム レンダラーには、Xamarin.Forms コントロールの動作と外観をカスタマイズする各プラットフォームでネイティブ コントロールのレンダリングをオーバーライドする開発者が使用できます。
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a88462052906e68fd85a07161e8b5bb63a61e69d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239891"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms のカスタム レンダラー

_Xamarin.Forms ユーザー インターフェイスでは、Xamarin.Forms アプリケーション プラットフォームごとに適切なルック アンド フィールを維持できるように、ターゲット プラットフォームのネイティブ コントロールを使用して表示されます。カスタム レンダラーでは、開発者の各プラットフォームで Xamarin.Forms コントロールの動作と外観をカスタマイズするには、この処理をオーバーライドすることができます。_

## <a name="introduction-to-custom-renderersintroductionmd"></a>[カスタム レンダラーの概要](introduction.md)

カスタムのレンダラーでは、Xamarin.Forms コントロールの動作と外観をカスタマイズするための強力な方法を提供します。 これらは、小規模のスタイル設定の変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズ使用できます。 この記事では、カスタムのレンダラーを紹介し、カスタム レンダラーを作成するプロセスの概要を示します。

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[レンダラーの基本クラスおよびネイティブ コントロール](renderers.md)

各 Xamarin.Forms コントロールには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 この記事では、レンダラーと各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するコントロールのネイティブ クラスを示します。

## <a name="customizing-an-entryentrymd"></a>[エントリのカスタマイズ](entry.md)

Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールが 1 行のテキストを編集することを許可します。 この記事は、用のカスタム レンダラーを作成する方法を示します、`Entry`コントロール、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドします。

## <a name="customizing-a-contentpagecontentpagemd"></a>[コンテンツ ページのカスタマイズ](contentpage.md)

A [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)は、1 つのビューを表示し、画面の大部分を占める視覚的要素。 この記事は、用のカスタム レンダラーを作成する方法を示します、 `ContentPage`  ページで、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドします。

## <a name="customizing-a-mapmapindexmd"></a>[マップのカスタマイズ](map/index.md)

Xamarin.Forms.Maps は、ユーザーを高速で使い慣れたマップを提供する各プラットフォームでの Api が発生するネイティブのマップを使用してマップを表示するため、クロスプラット フォームの抽象化を提供します。 このトピックのカスタム レンダラーを作成する方法を示します、`Map`コントロール、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドします。

## <a name="customizing-a-listviewlistviewmd"></a>[ListView のカスタマイズ](listview.md)

Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)垂直方向の一覧としてデータのコレクションを表示するビューです。 この記事では、カスタム レンダラーを作成のプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブのリスト コントロールのパフォーマンスをより細かく制御できるようにする方法を示します。

## <a name="customizing-a-viewcellviewcellmd"></a>[ViewCell のカスタマイズ](viewcell.md)

Xamarin.Forms [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)セルに追加できるは、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)または[ `TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)開発者が定義したビューが含まれています。 この記事は、用のカスタム レンダラーを作成する方法を示します、 `ViewCell` 、Xamarin.Forms 内でホストされている`ListView`コントロール。 これにより、停止、Xamarin.Forms レイアウトの計算中に繰り返し呼び出される`ListView`スクロールします。

## <a name="implementing-a-viewviewmd"></a>[ページの実装](view.md)

Xamarin.Forms カスタム ユーザー インターフェイス コントロールがから派生する必要があります、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラスは、レイアウトと、画面上のコントロールを配置するために使用します。 この記事では、デバイスのカメラからプレビュー ビデオ ストリームの表示に使用される Xamarin.Forms のカスタム コントロールのカスタム レンダラーを作成する方法を示します。

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[HybridWebView の実装](hybridwebview.md)

この記事は、用のカスタム レンダラーを作成する方法を示します、`HybridWebView`カスタム コントロールは、JavaScript から呼び出せるように c# コードを許可するプラットフォーム固有の web コントロールを強化する方法を示します。

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[ビデオ プレーヤーを実装します。](video-player/index.md)

この記事の内容を実装するカスタム レンダラーを作成する方法を示しています。 `VideoPlayer` web、アプリケーションのリソースとして埋め込まれているビデオまたはユーザーのデバイス上のビデオ ライブラリに格納されているビデオからビデオを再生できるコントロールです。 メソッドと読み取り専用のバインド可能なプロパティを実装するなど、いくつかの手法が示されています。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
- [カスタム レンダラー (Xamarin 大学ビデオ)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [サンプルのカスタム レンダラー (Xamarin 大学ビデオ)](http://bit.ly/xf-customrenderer)
