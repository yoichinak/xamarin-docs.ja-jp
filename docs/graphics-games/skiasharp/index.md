---
title: SkiaSharp の 2D 描画
description: このドキュメントでは、SkiaSharp 描画クロスプラット フォーム 2D の概要を示します。 SkiaSharp を記述するさまざまなガイドとそのさまざまな Api にリンクします。
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 7207f33e56f566a5528d93f9957e2ff780a22a65
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615523"
---
# <a name="2d-drawing-with-skiasharp"></a>SkiaSharp の 2D 描画

SkiaSharp 2D グラフィックスを行うための強力な c# API を提供します。 機能により、 [Google の Skia ライブラリ](http://skia.org)、Google Chrome、Firefox、および Android のグラフィックのスタックが作動するのと同じライブラリです。

[![](images/ide-sml.png "SkiaSharp 2D グラフィックスを行うための強力な c# API を提供します")](images/ide.png#lightbox)

SkiaSharp のポータブル ライブラリし、として簡単に付属しています、[クロスプラット フォームの NuGet パッケージ](https://www.nuget.org/packages/SkiaSharp)、すぐに、次のプラットフォームをサポートしています: macOS、Xamarin.Android、Xamarin.iOS、Windows デスクトップ。

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[SkiaSharp の概要](~/graphics-games/skiasharp/introduction.md)

SkiaSharp とサンプルの主要な概念の概要については、グラフィックス、テキスト、ビットマップをレンダリングするコードし、イメージ フィルターを使用します。

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Xamarin.Forms 用 SkiaSharp チュートリアル](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

クロス プラットフォーム グラフィックス Xamarin.Forms でレンダリングを使用する方法について説明します。

- [描画の基礎](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [単純な円を描画](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Xamarin.Forms との統合](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [ピクセルとデバイスに依存しない単位](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [基本アニメーション](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [テキストとグラフィックスの統合](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [ビットマップの基礎](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [線とパス](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [線とストローク キャップ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [パスの基礎](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [パスの塗りつぶしの種類](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [多角形やパラメーターの式](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [ドットとダッシュ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [指による描画](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [平行移動変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [スケール変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [回転の変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [傾斜変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [行列変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [タッチ操作](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [非アフィン変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D 回転](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [曲線とパス](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [円弧を描画する 3 つの方法](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [3 つの種類のベジエ曲線](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG Path Data](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [パスおよび領域でクリッピング](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [パスの効果](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [パスとテキスト](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [パス情報と列挙](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [ビットマップ](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [ビットマップの表示](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [ビットマップの作成と描画](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [ビットマップのトリミング](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [ビットマップのセグメント化された表示](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [ビットマップのファイルへの保存](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [ビットマップのピクセル ビットへのアクセス](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [ビットマップのアニメーション化](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[プラットフォーム固有の注意事項](~/graphics-games/skiasharp/platform.md)

このページでは、iOS、Android、macOS、および Windows を含むさまざまなプラットフォームで SkiaSharp のセットアップ手順について説明します。

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API ドキュメント](https://developer.xamarin.com/api/namespace/SkiaSharp/)

参照することができます、 [API ドキュメント](https://developer.xamarin.com/api/namespace/SkiaSharp/)SkiaSharp、web サイト上の。

## <a name="work-in-progress"></a>進行中の作業

SkiaSharp のコミュニティと共有している作業中です。 Skia API の重要な部分にバインドしましたが、中に実行する多くの作業が残っています。 Skia、側に表示される安定した C API を使用しているし、Api に完全なカバレッジを提供する Skia の C バインドへの投稿を続行する予定です。

バインドの取り組みのガイド、後の問題としてコメントまたは提案を残してください GitHub リポジトリにご協力[ http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp)します。
