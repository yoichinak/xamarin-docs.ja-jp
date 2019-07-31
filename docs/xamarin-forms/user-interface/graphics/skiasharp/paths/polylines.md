---
title: 多角形やパラメーターの式
description: この記事では、方法をレンダリングする SkiaSharp を使用するいずれかの行パラメーターの式を定義でき、サンプル コードでこのことについて説明します。
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: f326a2ef449b7c807be150a002a4afc600d9908d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652705"
---
# <a name="polylines-and-parametric-equations"></a>多角形やパラメーターの式

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して、パラメーターの式を定義できる任意の行をレンダリングするには_

[ **SkiaSharp の曲線とパス**](../curves/index.md)セクションのこのガイドでは、さまざまな方法がわかりますを[ `SKPath` ](xref:SkiaSharp.SKPath)曲線の特定の種類を表示するために定義します。 ただしで直接サポートされていない曲線を描画する必要があります`SKPath`します。 このような場合は、数学的に定義できる任意の曲線を描画するためにポリライン (接続されている行のコレクション) を使用することができます。 行を十分に小さい行うし、結果が多数が不十分曲線のようになります。 場合、 このスパイラルでは、実際に 3,600 の短い行を示します。

![](polylines-images/spiralexample.png "スパイラル")

通常、パラメーターの式のペアの観点から曲線を定義することをお勧めします。 これは、X および Y 座標の方程式とも呼ばれる 3 番目の変数に依存して`t`時間。 たとえば、次のパラメーターの式が 1 の中心点 (0, 0) の半径の円を定義の*t* 0 ~ 1。

`x = cos(2πt)`

`y = sin(2πt)`

 Radius を 1 より大きい、する場合は、半径の正弦と余弦の値を乗算し、center を別の場所に移動する必要がある場合は、これらの値を追加します。

`x = xCenter + radius·cos(2πt)`

`y = yCenter + radius·sin(2πt)`

水平および垂直方向の軸平行に楕円の半径の 2 つが含まれます。

`x = xCenter + xRadius·cos(2πt)`

`y = yCenter + yRadius·sin(2πt)`

SkiaSharp の同等のコードを配置して、ループをさまざまなポイントを計算し、それらをパスに追加します。 SkiaSharp の次のコード作成、`SKPath`楕円を表示画面を読み込むためのオブジェクト。 ループは、直接 360 度を循環します。 中心が半分の幅と、表示画面の高さと 2 つの半径をそのためには。

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

これは、結果、360 の小さな行によって定義される楕円。 レンダリングされると、滑らかな表示されます。

もちろん、ポリラインを使用しているため、楕円を作成する必要はありません`SKPath`が含まれています、`AddOval`メソッドによって処理されるためです。 によって提供されていないビジュアル オブジェクトを描画することもできますが、`SKPath`します。

**Archimedean スパイラル**ページにそのようなコードは楕円のコードには、重要な相違点があります。 これは、ループ円の 360 度 10 回、半径を継続的に調整。

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

結果とも呼ばれますが、*算術スパイラル*each ループの間のオフセットは定数であるため。

[![](polylines-images/archimedeanspiral-small.png "Archimedean スパイラル ページのスクリーン ショットをトリプル")](polylines-images/archimedeanspiral-large.png#lightbox "Archimedean スパイラル ページの 3 倍になるスクリーン ショット")

注意、`SKPath`で作成、`using`ブロックします。 これは、`SKPath`より多くのメモリを消費、`SKPath`を提案しています、前のプログラム内のオブジェクトを`using`ブロックがすべてのアンマネージ リソースを破棄するより適しています。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
