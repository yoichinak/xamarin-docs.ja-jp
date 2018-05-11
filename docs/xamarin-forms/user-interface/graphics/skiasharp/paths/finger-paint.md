---
title: 本の指の描画
description: 指を使用して、キャンバスに描画します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: 6180eb61e7850b7739c5514461796fb0aacbc4ff
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="finger-painting"></a>本の指の描画

_指を使用して、キャンバスに描画します。_

`SKPath`オブジェクトは継続的に更新して表示することができます。 この機能は、finger-painting プログラムでなどの対話型の描画に使用するパスを使用します。

![](finger-paint-images/fingerpaintsample.png "本の指の描画の課題")

Xamarin.Forms でタッチのサポートでは、Xamarin.Forms タッチ追跡効果は、追加のタッチ サポートを提供する開発が完了するために、画面で、個々 の本の指を追跡することはできません。 この効果は、資料に記載されて[**効果からイベントを呼び出す**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)です。 サンプル プログラム[**タッチ追跡効果デモ**](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) SkiaSharp、finger-painting プログラムなどを使用する 2 つのページが含まれています。

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)ソリューションには、このタッチ追跡イベントが含まれています。 .NET 標準のライブラリ プロジェクトに含まれる、`TouchEffect`クラス、`TouchActionType`列挙型、`TouchActionEventHandler`デリゲート、および`TouchActionEventArgs`クラスです。 プラットフォーム プロジェクトそれぞれを含める、`TouchEffect`そのプラットフォームのクラス以外の場合は、iOS プロジェクトにも含まれています、`TouchRecognizer`クラスです。

**指ペイント**ページに**SkiaSharpFormsDemos**本の指の描画のシンプルな実装です。 色を選択できるようにしたり、幅の境界線の描画しないキャンバスをクリアすることがない、および、もちろん、アートワークを保存することはできません。

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

アタッチする、`TouchEffect`に直接、`SKCanvasView`すべてのプラットフォームにおいては使用できません。

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs)分離コード ファイルを格納するための 2 つのコレクションを定義する、`SKPath`オブジェクトだけでなく、`SKPaint`これらのパスを表示するためのオブジェクト。

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

名前推奨として、`inProgressPaths`辞書が 1 つまたは複数の指で現在描画されているパスを格納します。 ディクショナリのキーは、タッチ イベントを伴うタッチ ID です。 `completedPaths`フィールドが完了した場合、指が画面からリフトのパスを描画するパスのコレクション。

`TouchAction`ハンドラーは、これら 2 つのコレクションを管理します。 まず、画面に触れると、新しい`SKPath`に追加`inProgressPaths`です。 その指を移動すると、追加の点は、パスに追加されます。 パスが転送された指がリリースされたときに、`completedPaths`コレクション。 同時に複数指で描くことができます。 パスまたはコレクションのいずれかに変更するたびに、`SKCanvasView`は無効になります。

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

タッチ追跡イベントに付属している点は、Xamarin.Forms 座標です。これらは、SkiaSharp 座標は、ピクセルに変換する必要があります。 目的は、`ConvertToPixel`メソッドです。

`PaintSurface`しハンドラーだけで、パスの両方のコレクションを表示します。 進行中のパスの下に完了した前のパスが表示されます。

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
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

指絵画は才能によってのみ制限されます。

[![](finger-paint-images/fingerpaint-small.png "指ペイント ページのスクリーン ショットをトリプル")](finger-paint-images/fingerpaint-large.png#lightbox "指ペイント ページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [タッチ追跡効果デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [呼び出し元のイベントの効果を適用](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
