---
title: 多角形やパラメーターの式
description: この記事では、SkiaSharp を使用して、パラメーター式を使用して定義できる任意の線を表示する方法について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 75f6f9b3e7a493121b8f4dacb87749d4a9616dee
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557102"
---
# <a name="polylines-and-parametric-equations"></a>多角形やパラメーターの式

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して、パラメーター式を使用して定義できる任意の線を表示します。_

このガイドの「 [**SkiaSharp の曲線とパス**](../curves/index.md) 」セクションでは、 [`SKPath`](xref:SkiaSharp.SKPath) 特定の種類の曲線をレンダリングするためにが定義されているさまざまなメソッドについて説明します。 ただし、によって直接サポートされない型の曲線を描画することが必要になる場合があり `SKPath` ます。 このような場合は、ポリライン (接続された直線のコレクション) を使用して、数学的に定義できる曲線を描画できます。 行を十分に小さくして十分な数にすると、結果は曲線のようになります。 このらせんは実際には、次のようにわずかに3600です。

![らせん](polylines-images/spiralexample.png)

一般に、パラメーター式のペアの観点から曲線を定義することをお勧めします。 これらは、3番目の変数に依存する X 座標と Y 座標の式であり、時間と呼ばれることもあり `t` ます。 たとえば、次のパラメーター式は、0 ~ 1 の *t* のポイント (0, 0) の中心が1である円を定義しています。

`x = cos(2πt)`

`y = sin(2πt)`

 半径を1より大きくする場合は、その半径で正弦と余弦の値を乗算するだけで、中央を別の場所に移動する必要がある場合は、これらの値を追加します。

`x = xCenter + radius·cos(2πt)`

`y = yCenter + radius·sin(2πt)`

水平方向と垂直方向の軸を持つ楕円の場合は、次の2つの半径が関係します。

`x = xCenter + xRadius·cos(2πt)`

`y = yCenter + yRadius·sin(2πt)`

次に、同等の SkiaSharp コードをループに配置して、さまざまな点を計算し、それらをパスに追加できます。 次の SkiaSharp コードは、 `SKPath` 表示サーフェイスを塗りつぶす楕円のオブジェクトを作成します。 ループは、360°を直接通過します。 中央は画面の幅と高さの半分であるため、2つの半径があります。

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

これにより、360の小さな行で定義された楕円が生成されます。 レンダリングされると、スムーズに表示されます。

もちろん、ポリラインを使用して楕円を作成する必要はありません `SKPath` 。には、そのようなメソッドが含まれているため `AddOval` です。 ただし、で提供されていないビジュアルオブジェクトを描画することもでき `SKPath` ます。

アーカイブのための **らせん** のページには、楕円コードに似たコードがありますが、大きな違いがあります。 次のように、円の360°を10回ループし、半径を継続的に調整します。

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

各ループ間のオフセットは定数であるため、結果は *算術らせん* とも呼ばれます。

[![アーカイブの三重のらせんページのトリプルスクリーンショット](polylines-images/archimedeanspiral-small.png)](polylines-images/archimedeanspiral-large.png#lightbox "アーカイブの三重のらせんページのトリプルスクリーンショット")

`SKPath`がブロック内に作成されていることに注意して `using` ください。 これは、 `SKPath` 前のプログラムのオブジェクトよりも多くのメモリを消費します `SKPath` 。これは、 `using` アンマネージリソースを破棄するためにブロックがより適切であることを示しています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)