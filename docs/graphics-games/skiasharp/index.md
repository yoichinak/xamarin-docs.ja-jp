---
title: SkiaSharp を 2D の描画
description: このドキュメントでは、描画 SkiaSharp クロスプラット フォームの 2D の概要を示します。 SkiaSharp を記述するさまざまなガイドやそのさまざまな Api にリンクします。
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 962fe657f25976f9b5069f2d434e92f816d249ca
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783293"
---
# <a name="2d-drawing-with-skiasharp"></a>SkiaSharp を 2D の描画

SkiaSharp は、2 次元グラフィックスを行うための強力な c# API を提供します。 電源が入って[Google の Skia ライブラリ](http://skia.org)、Google Chrome、Firefox および Android のグラフィック スタックの電源を同じライブラリです。

[![](images/ide-sml.png "SkiaSharp 2D グラフィックスを行うための強力な c# API を提供します。")](images/ide.png#lightbox)

SkiaSharp は、ポータブル ライブラリ、されとして簡単に付属しています、[クロスプラット フォームの NuGet パッケージ](https://www.nuget.org/packages/SkiaSharp)、すぐに、次のプラットフォームをサポートして: macOS、Xamarin.Android、Xamarin.iOS、および Windows デスクトップです。

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[SkiaSharp の概要](~/graphics-games/skiasharp/introduction.md)

SkiaSharp とサンプルの主要な概念の概要については、画像、テキスト、ビットマップを表示するためにコードし、イメージ フィルターを使用します。

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Xamarin.Forms の SkiaSharp チュートリアル](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

クロス プラットフォーム グラフィックス Xamarin.Forms のレンダリングを使用する方法を学習します。

- [描画の基本](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [単純な円を描画します。](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Xamarin.Forms との統合](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [デバイス非依存単位、ピクセル](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [基本的なアニメーション](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [テキストとグラフィックスを統合します。](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [ビットマップの基礎](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [行とパス](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [行とストローク キャップ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [パスの基礎](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [パスの塗りつぶしの種類](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [多角形やパラメーターの式](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [ドットとダッシュ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [指による描画](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [平行移動の変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [スケールの変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [回転変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [傾斜変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [行列による変換します。](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [タッチ操作](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [非アフィン変換します。](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D 回転](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [曲線とパス](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [円弧を描画する 3 つの方法](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [3 つの種類のベジエ曲線](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG Path Data](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [パスおよび領域でクリッピング](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [パスの効果](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [パスとテキスト](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [パス情報と列挙](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[プラットフォーム固有の注意事項](~/graphics-games/skiasharp/platform.md)

このページは、iOS、Android、macOS、および Windows を含むさまざまなプラットフォームの SkiaSharp のセットアップ手順を説明します。

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API のドキュメント](https://developer.xamarin.com/api/namespace/SkiaSharp/)

参照することができます、 [API ドキュメント](https://developer.xamarin.com/api/namespace/SkiaSharp/)SkiaSharp については、web サイトにします。

## <a name="work-in-progress"></a>進行中の作業

SkiaSharp は、コミュニティと共有している処理中です。 Skia API の重要な部分にバインドしましたが、中に実行する多くの作業が残っています。 Skia、側に表示される安定した C API を使用して、原因となった Api を完全に保護する Skia の C バインドに作業を続行することを計画します。

バインディング労力をガイド、くださいのままにコメントや提案問題として GitHub リポジトリにご協力[ http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp)です。
