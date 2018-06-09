---
title: 多角形やパラメーターの式
description: ここでは、任意の行を表示するために使用する SkiaSharp を定義する方法をパラメーター型用いた、について説明しのサンプル コードを示します。
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9539a21b7dbc91da63795639610886233ed705be
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245310"
---
# <a name="polylines-and-parametric-equations"></a>多角形やパラメーターの式

_パラメーター式で指定できますいずれかの行を表示するために使用する SkiaSharp_

このガイドの後半で、さまざまな方法が表示されますを`SKPath`曲線の特定の種類を表示するために定義します。 ただし、によって直接サポートされていない曲線を描画する必要があります`SKPath`です。 このような場合は、数学的に定義できるどのような曲線を描画する多角形 (接続されている行のコレクション) を使用できます。 行は、大きすぎてし場合、結果が多数不十分な曲線のようになります。 このなるは、実際に 3,600 の小さな行を示します。

![](polylines-images/spiralexample.png "なります。")

通常、パラメーター型の式のペアの観点から曲線を定義することをお勧めします。 これらは、X 座標と Y 座標を用の数式とも呼ばれる 3 番目の変数に依存`t`時間。 たとえば、次のパラメーター型の式はの中心点 (0, 0) が 1 の半径の円を定義*t* 0 ~ 1。

 x = cos(2πt) y = sin(2πt)

 Radius を 1 より大きい、する場合は、その radius で正弦と余弦の値を乗算し、センターを別の場所に移動する必要がある場合は、それらの値を追加します。

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

楕円を水平方向および垂直の軸並列での 2 つの半径が関係しています。

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

さまざまなポイントを計算し、パスに追加するループでし、同等の SkiaSharp コードを配置できます。 次の SkiaSharp コードを作成、`SKPath`表示画面が塗りつぶしますされる楕円のオブジェクト。 ループは、直接 360 度を循環します。 中央半分の幅と、ディスプレイ画面の高さは、次の 2 つの半径は。

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

これは、結果、360 の小さな行によって定義される楕円を描画します。 レンダリングされると、滑らかな表示されます。

ポリラインを使用しているため、楕円を作成する必要はありませんもちろん、`SKPath`が含まれています、`AddOval`を行うメソッドです。 によって提供されていないビジュアル オブジェクトを描画することができますが、`SKPath`です。

**Archimedean なる**ページには、コードのような重要な相違点が、楕円のコードにします。 これでループ処理、円の 360 度 10 回半径を継続的に調整します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

結果とも呼ばれますが、*算術なる*各ループの間のオフセットは定数ため。

[![](polylines-images/archimedeanspiral-small.png "Archimedean なるページのスクリーン ショットをトリプル")](polylines-images/archimedeanspiral-large.png#lightbox "Archimedean なるページのトリプル スクリーン ショット")

注意して、`SKPath`で作成された、`using`ブロックします。 これは、`SKPath`より多くのメモリを消費、`SKPath`されることを意味の以前のプログラム内のオブジェクト、`using`ブロックは、アンマネージ リソースを破棄するより適切な。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
