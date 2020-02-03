---
title: SkiaSharp の指による描画
description: この記事では、指を使用して、Xamarin.Forms アプリケーションで SkiaSharp キャンバスに描画する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
ms.openlocfilehash: d5cf0927c64732d6d0a44204db9509fae77f0d1d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724074"
---
# <a name="finger-painting-in-skiasharp"></a>SkiaSharp の指による描画

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_指を使用して、キャンバス上に描画します。_

`SKPath` オブジェクトを継続的に更新して表示することができます。 この機能は、フィンガーペインティング プログラムなど、対話型の描画に使用するパスを使用できます。

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

Xamarin.Forms でのタッチ サポートでは、Xamarin.Forms のタッチ追跡効果は、追加のタッチ サポートを提供する開発が完了するために、画面で、個々 の本の指を追跡することはできません。 この効果については、「[**効果からイベントを呼び出す**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)」を参照してください。 サンプルプログラムの[**タッチ追跡効果のデモ**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)には、SkiaSharp を使用する2つのページが含まれています (指描画プログラムを含む)。

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)ソリューションには、このタッチ追跡イベントが含まれています。 .NET Standard ライブラリプロジェクトには、`TouchEffect` クラス、`TouchActionType` 列挙型、`TouchActionEventHandler` デリゲート、および `TouchActionEventArgs` クラスが含まれています。 各プラットフォームプロジェクトには、そのプラットフォームの `TouchEffect` クラスが含まれています。iOS プロジェクトには、`TouchRecognizer` クラスも含まれています。

**SkiaSharpFormsDemos**の**指描画**ページは、指描画の実装を簡略化したものです。 色の選択を許可したり、ストロークの幅しない、キャンバスをクリアする方法がない、および、もちろん、アートワークを保存することはできません。

[**FingerPaintPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml)ファイルは、`SKCanvasView` を1つのセル `Grid` に格納し、その `Grid`に `TouchEffect` をアタッチします。

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

`TouchEffect` を `SKCanvasView` に直接アタッチすることは、すべてのプラットフォームでは機能しません。

[**FingerPaintPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml.cs)分離コードファイルは、`SKPath` オブジェクトを格納するための2つのコレクションと、これらのパスを表示するための `SKPaint` オブジェクトを定義します。

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

名前が示すように、`inProgressPaths` ディクショナリは、現在1つ以上の指で描画されているパスを格納します。 ディクショナリのキーは、タッチ イベントに付属しているタッチ ID です。 `completedPaths` フィールドは、パスを画面から持ち上げた指が終了したときに完了したパスのコレクションです。

`TouchAction` ハンドラーは、これらの2つのコレクションを管理します。 指が画面に触れると、新しい `SKPath` が `inProgressPaths`に追加されます。 その指を移動すると、追加の点がパスに追加されます。 指が離されると、パスが `completedPaths` コレクションに転送されます。 同時に複数の指で描画できます。 パスまたはコレクションのいずれかを変更すると、`SKCanvasView` は無効になります。

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

タッチの追跡イベントに付属するポイントは Xamarin.Forms の座標です。これらは、ピクセルである SkiaSharp 座標に変換する必要があります。 これが `ConvertToPixel` メソッドの目的です。

`PaintSurface` ハンドラーは、両方のパスのコレクションを単にレンダリングします。 進行中のパスの下にある完了した前のパスが表示されます。

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

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

これで線を描画して、パラメーターの式を使用して曲線を定義する方法を説明しました。 [**SkiaSharp の曲線とパス**](../curves/index.md)の後のセクションでは、`SKPath` がサポートするさまざまな種類の曲線について説明します。 しかし、前提条件として、 [**SkiaSharp 変換**](../transforms/index.md)の探索が役立ちます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [タッチトラッキング効果のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [呼び出し (影響からイベントを)](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
