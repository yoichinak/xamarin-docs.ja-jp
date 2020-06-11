---
title: "SkiaSharp でのフィンガーペイント" の説明: "この記事では、指を使用してアプリケーションの SkiaSharp canvas で描画する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B author: davidbritch dabritch: ms. date: 04/05/2017 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="finger-painting-in-skiasharp"></a>SkiaSharp での指描画

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_指を使用して、キャンバス上に描画します。_

`SKPath`オブジェクトは継続的に更新および表示できます。 この機能を使用すると、指描画プログラムなどの対話型描画にパスを使用できます。

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

のタッチサポートで Xamarin.Forms は、画面上の個々の指を追跡することはできません。そのため、タッチ Xamarin.Forms 追跡効果は、追加のタッチサポートを提供するために開発されています。 この効果については、「[**効果からイベントを呼び出す**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)」を参照してください。 サンプルプログラムの[**タッチ追跡効果のデモ**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)には、SkiaSharp を使用する2つのページが含まれています (指描画プログラムを含む)。

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)ソリューションには、このタッチ追跡イベントが含まれています。 .NET Standard ライブラリプロジェクトには、 `TouchEffect` クラス、 `TouchActionType` 列挙体、デリゲート、およびクラスが含まれてい `TouchActionEventHandler` `TouchActionEventArgs` ます。 各プラットフォームプロジェクトには、そのプラットフォームのクラスが含まれています `TouchEffect` 。 iOS プロジェクトには、クラスも含まれてい `TouchRecognizer` ます。

**SkiaSharpFormsDemos**の**指描画**ページは、指描画の実装を簡略化したものです。 色またはストロークの幅を選択することはできません。キャンバスをクリアする方法はありません。もちろん、アートワークを保存することはできません。

[**FingerPaintPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml)ファイルは、を `SKCanvasView` 1 つのセルに格納し、を `Grid` そのにアタッチします `TouchEffect` `Grid` 。

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

`TouchEffect`をに直接アタッチする `SKCanvasView` ことは、すべてのプラットフォームでは機能しません。

[**FingerPaintPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml.cs)分離コードファイルは、オブジェクトを格納するための2つのコレクション `SKPath` と、 `SKPaint` これらのパスを表示するためのオブジェクトを定義します。

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

名前が示すように、 `inProgressPaths` ディクショナリは、現在1つ以上の指で描画されているパスを格納します。 Dictionary キーは、タッチイベントに付随する touch ID です。 `completedPaths`フィールドは、パスを画面からリフトした指が終了したときに完了したパスのコレクションです。

`TouchAction`このハンドラーは、これらの2つのコレクションを管理します。 指が画面に触れると、 `SKPath` に新しいが追加され `inProgressPaths` ます。 指が移動すると、パスに追加のポイントが追加されます。 指が離されると、パスがコレクションに転送され `completedPaths` ます。 複数の指で同時に塗りつぶすことができます。 パスまたはコレクションのいずれかに変更を行うと、が `SKCanvasView` 無効になります。

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

タッチトラッキングイベントに付随する点は、 Xamarin.Forms 座標です。これらは、ピクセルである SkiaSharp 座標に変換する必要があります。 これがメソッドの目的 `ConvertToPixel` です。

ハンドラーは、 `PaintSurface` 両方のパスのコレクションを単にレンダリングします。 以前に完了したパスは、進行中のパスの下に表示されます。

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

指描いは、自分の才能によってのみ制限されています。

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

ここでは、直線を描画し、パラメーター式を使用して曲線を定義する方法を説明しました。 [**SkiaSharp の曲線とパス**](../curves/index.md)の後のセクションでは、でサポートされるさまざまな種類の曲線について説明し `SKPath` ます。 しかし、前提条件として、 [**SkiaSharp 変換**](../transforms/index.md)の探索が役立ちます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [タッチトラッキング効果のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [エフェクトからのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
