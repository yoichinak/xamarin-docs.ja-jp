---
title: SkiaSharp の指による描画
description: この記事では、指を使用して、Xamarin.Forms アプリケーションで SkiaSharp キャンバスに描画する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
ms.openlocfilehash: 571ddae0757691cd7fee301076f0b1310749531d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657454"
---
# <a name="finger-painting-in-skiasharp"></a>SkiaSharp の指による描画

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_指を使用して、キャンバスに描画します。_

`SKPath`オブジェクトは継続的に更新および表示されることができます。 この機能は、フィンガーペインティング プログラムなど、対話型の描画に使用するパスを使用できます。

![](finger-paint-images/fingerpaintsample.png "指による描画の課題")

Xamarin.Forms でのタッチ サポートでは、Xamarin.Forms のタッチ追跡効果は、追加のタッチ サポートを提供する開発が完了するために、画面で、個々 の本の指を追跡することはできません。 この効果は、情報の記事に記載されて[**効果からイベントを呼び出す**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)します。 サンプル プログラム[**タッチ追跡効果デモ**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) SkiaSharp、フィンガーペインティング プログラムなどを使用する 2 つのページが含まれています。

[ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)ソリューションには、このタッチ追跡イベントが含まれています。 .NET Standard ライブラリ プロジェクトが含まれています、`TouchEffect`クラス、`TouchActionType`列挙型で、`TouchActionEventHandler`デリゲートと`TouchActionEventArgs`クラス。 各プラットフォーム プロジェクトが含まれています、`TouchEffect`そのプラットフォームのクラスは、iOS プロジェクトにも含まれています、`TouchRecognizer`クラス。

**指ペイント**ページ**SkiaSharpFormsDemos**指による描画の簡略化された実装です。 色の選択を許可したり、ストロークの幅しない、キャンバスをクリアする方法がない、および、もちろん、アートワークを保存することはできません。

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml)ファイルの配置、 `SKCanvasView` 1 つのセルで`Grid`アタッチし、`TouchEffect`に`Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

アタッチ、`TouchEffect`に直接、`SKCanvasView`はすべてのプラットフォームでは動作しません。

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs)分離コード ファイルを格納するための 2 つのコレクションを定義する、`SKPath`オブジェクト、および`SKPaint`これらのパスを表示するためのオブジェクト。

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

名前のとおり、`inProgressPaths`が 1 つ以上の指で描画される現在のパスを格納するディクショナリ。 ディクショナリのキーは、タッチ イベントに付属しているタッチ ID です。 `completedPaths`フィールドは、パスの描画が指が画面から放されたときに終了したパスのコレクション。

`TouchAction`ハンドラーは、これら 2 つのコレクションを管理します。 まず、画面に触れると、新しい`SKPath`に追加されます`inProgressPaths`します。 その指を移動すると、追加の点がパスに追加されます。 パスを転送、本の指がリリースされたときに、`completedPaths`コレクション。 同時に複数の指で描画できます。 パスまたはコレクションのいずれかに変更するたびに、`SKCanvasView`は無効になります。

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

タッチの追跡イベントに付属するポイントは Xamarin.Forms の座標です。これらは、ピクセルである SkiaSharp 座標に変換する必要があります。 目的は、`ConvertToPixel`メソッド。

`PaintSurface`しハンドラー単に、パスの両方のコレクションを表示します。 進行中のパスの下にある完了した前のパスが表示されます。

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

指、絵画はあなたの才能によってのみ制限されます。

[![](finger-paint-images/fingerpaint-small.png "本の指ペイント ページのスクリーン ショットをトリプル")](finger-paint-images/fingerpaint-large.png#lightbox "指ペイント ページの 3 倍になるスクリーン ショット")

これで線を描画して、パラメーターの式を使用して曲線を定義する方法を説明しました。 以降のセクション[ **SkiaSharp の曲線とパス**](../curves/index.md)曲線のさまざまな種類について説明する`SKPath`をサポートしています。 便利な前提条件は、探索の[ **SkiaSharp の変換**](../transforms/index.md)します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [タッチ追跡効果デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [効果からのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
