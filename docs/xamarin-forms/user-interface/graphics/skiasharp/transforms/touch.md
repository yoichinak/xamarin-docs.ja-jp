---
title: タッチ操作
description: この記事では、行列変換を使用して、ドラッグのタッチ、ピンチ、および回転を実装する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: davidbritch
ms.author: dabritch
ms.date: 09/14/2018
ms.openlocfilehash: 407fe78618c5e5fcd8732d9ff3cea50561ca78f3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655543"
---
# <a name="touch-manipulations"></a>タッチ操作

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_タッチをドラッグする、ピンチ、および回転を実装するために使用して行列の変換します。_

多くの場合、ユーザーはモバイル デバイスなど、マルチタッチ環境で、画面上のオブジェクトを操作するのに指を使用します。 1 本の指のドラッグと 2 本の指によるピンチなどの一般的なジェスチャでは、移動、物体、およびまたはも回転したりできます。 これらのジェスチャは、変換行列を使用して実装されている一般に、この記事では、その方法を示します。

![](touch-images/touchmanipulationsexample.png "平行移動、拡大縮小、および回転を受けたビットマップ")

ここに示すサンプルがすべての記事で紹介した Xamarin.Forms タッチ追跡効果を使用する[**効果からイベントを呼び出す**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)します。

## <a name="dragging-and-translation"></a>ドラッグと翻訳

行列変換の最も重要なアプリケーションの 1 つは、タッチ処理です。 1 つ[ `SKMatrix` ](xref:SkiaSharp.SKMatrix)値は、一連のタッチ操作を統合できます。 

1 本指をドラッグする、`SKMatrix`値変換を実行します。 これは、方法については、**ビットマップ ドラッグ**ページ。 XAML ファイルのインスタンスを作成、 `SKCanvasView` 、Xamarin.Forms で`Grid`します。 A`TouchEffect`オブジェクトが追加されました、`Effects`そのコレクション`Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.BitmapDraggingPage"
             Title="Bitmap Dragging">
    
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

理論的には、`TouchEffect`オブジェクトに直接追加できる、`Effects`のコレクション、`SKCanvasView`が、そのすべてのプラットフォームでは機能しません。 `SKCanvasView`と同じサイズには、`Grid`この構成でアタッチして、`Grid`うまく機能します。

分離コード ファイルは、コンス トラクターでビットマップ リソースを読み込むときとで表示、`PaintSurface`ハンドラー。

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    // Bitmap and matrix for display
    SKBitmap bitmap;
    SKMatrix matrix = SKMatrix.MakeIdentity();
    ···

    public BitmapDraggingPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, new SKPoint());
    }
}
```

さらにコードをしなくても、`SKMatrix`値は常に、識別マトリックスで、ビットマップの表示には効果はありません。 目標、`OnTouchEffectAction`ハンドラーを XAML ファイルで設定は、タッチ操作を反映するように、マトリックスの値を変更します。

`OnTouchEffectAction`ハンドラーは、Xamarin.Forms を変換することによって開始`Point`値を SkiaSharp に`SKPoint`値。 これは単純に基づくスケーリングの問題、`Width`と`Height`プロパティの`SKCanvasView`(これは、デバイスに依存しない単位) と`CanvasSize`ピクセル単位でのプロパティ。

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    ···
    // Touch information
    long touchId = -1;
    SKPoint previousPoint;
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point = 
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point))
                {
                    touchId = args.Id;
                    previousPoint = point;
                }
                break;

            case TouchActionType.Moved:
                if (touchId == args.Id)
                {
                    // Adjust the matrix for the new position
                    matrix.TransX += point.X - previousPoint.X;
                    matrix.TransY += point.Y - previousPoint.Y;
                    previousPoint = point;
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = -1;
                break;
        }
    }
    ···
}
```

画面では、型のイベントが最初にタッチ指と`TouchActionType.Pressed`が発生します。 最初のタスクでは、指がビットマップの手を加えることがかどうかを決定します。 このようなタスクとも呼ばれます_ヒット テスト_します。 この場合は、ヒット テストによって実現できますを作成する、`SKRect`に行列変換を適用するビットマップに対応する値`MapRect`、タッチ ポイントを変換四角形の内側はかどうかを決定するとします。

その場合は、`touchId`フィールドは、タッチ ID に設定され、本の指の位置が保存されます。

`TouchActionType.Moved`イベントは、の翻訳要因、`SKMatrix`値は、本の指による、現在の位置と指の新しい位置に基づいて調整されます。 新しい位置が、次回の保存されている、`SKCanvasView`は無効になります。

このプログラムで実験するときは、指ビットマップを表示する領域に触れたときに、のみ、ビットマップをドラッグすることをメモしてをおきます。 このプログラムの非常に重要な制限は、ことが重要複数ビットマップを操作するときにします。

## <a name="pinching-and-scaling"></a>ピンチとスケーリング

2 本の指タッチのビットマップのときに発生するしますか。 2 本の指を並列で移動する可能性がある場合は、本の指と共に移動するビットマップ。 2 本の指では、わずかなを実行したり、操作を拡張、(次のセクションで説明) に回転またはスケールするビットマップをする可能性があります。 ビットマップをスケーリングするときに最も適した、ビットマップの基準としたのと同じ位置に保持する 2 本の指をそれに応じてスケールするビットマップ。

複雑で、2 本の指を一度に処理するように見えますが、留意する、`TouchAction`ハンドラーは、時刻に 1 本の指の情報のみを受け取ります。 2 本の指がビットマップを操作する場合、各イベントの 1 本の指の位置が変更されたが、他の方法が変更されていません。 **ビットマップ スケーリング**次のページ コード位置が変更されていない指が呼び出されます、 _pivot_変換は、その時点の基準としたためをポイントします。

このプログラムは、前のプログラムには 1 つの違いは、その複数のタッチ Id を保存する必要があります。 ディクショナリは、touch ID が辞書のキー、ディクショナリの値は、その指の現在の位置は、この目的に使用されます。

```csharp
public partial class BitmapScalingPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point) && !touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Add(args.Id, point);
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger scale and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index of non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points involved in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Scaling factors are ratios of those
                        float scaleX = newVector.X / oldVector.X;
                        float scaleY = newVector.Y / oldVector.Y;

                        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
                            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
                        {
                            // If something bad hasn't happened, calculate a scale and translation matrix
                            SKMatrix scaleMatrix = 
                                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);

                            SKMatrix.PostConcat(ref matrix, scaleMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }
    ···
}
```

処理、`Pressed`アクションがディクショナリに追加されて、前の点を除いてプログラム、ID と接点と同じがほとんどです。 `Released`と`Cancelled`アクションは、辞書のエントリを削除します。

処理、`Moved`アクション、ただしより複雑な。 存在する場合のみ 1 本の指、関連する処理は非常にすることは前のプログラムと同じです。 2 つ以上の指のプログラムは情報を移行しない指に関連するディクショナリからも取得する必要があります。 これは、配列にディクショナリのキーをコピーおよび移動中、本の指の ID を持つ最初のキーと比較してによって実行します。 これにより、移行しない指に対応するピボット ポイントを取得するプログラムです。

次に、プログラムは、ピボット ポイントの基準とした新しい本の指の位置とピボット ポイントの基準とした古い本の指の位置の 2 つのベクトルを計算します。 これらのベクターの比率ではスケール係数。 0 による除算が可能性のための無限値または NaN (非数) 値これらチェックする必要があります。 スケーリングの変換を連結する場合は、`SKMatrix`フィールドとして保存される値。

このページで実験するとき、ビットマップに 1 つまたは 2 つの指をドラッグまたは 2 本の指でのスケールできることがわかります。 スケーリングは_異方性_をスケーリングできることを意味水平および垂直方向で異なります。 これは、縦横比が変形がミラー イメージを作成するビットマップを反転することもできます。 消えますを 0 個のディメンションにビットマップを圧縮することができますをも検出可能性があります。 運用環境のコードでは、これに対抗するします。

## <a name="two-finger-rotation"></a>2 本の指による回転

**ビットマップを回転**ページでは、回転やスケーリングのアイソトロ ピックのいずれかの 2 本の指を使用することができます。 ビットマップは、常に、正しい縦横比を保持します。 2 本の指の回転とスケーリングの異方性は使用できません非常にうまくため、本の指の動きが両方の作業とよく似ています。

このプログラムでは最初の大きな違いは、ヒット テストのロジックです。 以前に使用されるプログラム、`Contains`メソッドの`SKRect`ビットマップに対応する変換された四角形が、タッチ ポイントを判断します。 ように、ユーザーは、ビットマップを操作する、ビットマップがありますが、回転、および`SKRect`正しく回転した四角形を表すことはできません。 ヒット テストのロジックがその場合ではなく複雑な分析のジオメトリを実装する必要があることを心配可能性があります。

ただし、ショートカットを使用できます。変換された四角形の境界内に点があるかどうかを判断することは、逆方向に変換された点が着想四角形の境界内にあるかどうかを判断することと同じです。 くらい簡単に計算され、ロジックを引き続き使用できます、便利な`Contains`メソッド。

```csharp
public partial class BitmapRotationPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!touchDictionary.ContainsKey(args.Id))
                {
                    // Invert the matrix
                    if (matrix.TryInvert(out SKMatrix inverseMatrix))
                    {
                        // Transform the point using the inverted matrix
                        SKPoint transformedPoint = inverseMatrix.MapPoint(point);

                        // Check if it's in the untransformed bitmap rectangle
                        SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);

                        if (rect.Contains(transformedPoint))
                        {
                            touchDictionary.Add(args.Id, point);
                        }
                    }
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger rotate, scale, and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Find angles from pivot point to touch points
                        float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                        float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                        // Calculate rotation matrix
                        float angle = newAngle - oldAngle;
                        SKMatrix touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                        // Effectively rotate the old vector
                        float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                        oldVector.X = magnitudeRatio * newVector.X;
                        oldVector.Y = magnitudeRatio * newVector.Y;

                        // Isotropic scaling!
                        float scale = Magnitude(newVector) / Magnitude(oldVector);

                        if (!float.IsNaN(scale) && !float.IsInfinity(scale))
                        {
                            SKMatrix.PostConcat(ref touchMatrix,
                                SKMatrix.MakeScale(scale, scale, pivotPoint.X, pivotPoint.Y));

                            SKMatrix.PostConcat(ref matrix, touchMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }

    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
    ···
}
```

ロジックを`Moved`は前のプログラムのようにイベントが開始します。 という名前の 2 つのベクトル`oldVector`と`newVector`前と移動の指の現在の点と動かない指のピボット ポイントに基づいて計算されます。 これらのベクターの角度を特定し、いて、違いは、回転の角度。

スケーリングがも関係している、ため、古いベクターは回転の角度に基づいて回転されます。 2 つのベクトルの相対的な大きさは、スケール ファクターではようになりました。 注意して同じ`scale`水平方向の値を使用して、アイソトロ ピックがスケーリングされるように、垂直方向のスケーリングします。 `matrix`回転行列とスケールのマトリックスの両方でフィールドを調整します。

アプリケーションがタッチを実装する必要がある場合、単一のビットマップ (またはその他のオブジェクト) の処理を調整できます 3 つのサンプルからコード、独自のアプリケーション。 タッチが複数ビットマップの処理を実装する必要がある場合がありますがおそらくこれらをカプセル化する他のクラスでの操作にタッチします。

## <a name="encapsulating-the-touch-operations"></a>タッチ操作をカプセル化します。

**タッチ操作**ページは、上記のロジックの多くをカプセル化する他のいくつかのファイルを使用して 1 つのビットマップのタッチ操作を示します。 これらのファイルの 1 つ目は、 [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs)いただけるコードで実装されるタッチ操作の種類を示す列挙体。

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` 翻訳に実装されている 1 本の指のドラッグが。 後続のすべてのオプションはまたパンが含まれますが、2 本の指を伴う:`IsotropicScale`結果オブジェクトを水平および垂直方向に均等に拡大縮小、ピンチ操作です。 `AnisotropicScale` 等しくない拡張を実現できます。

`ScaleRotate`オプションは 2 本の指が拡大縮小および回転用です。 スケーリングは、アイソトロ ピックです。 前述のように、異方性のスケーリングと 2 本の指による回転を実装するが問題のある本の指の動きは基本的に同じです。

`ScaleDualRotate`オプションが 1 本指による回転を追加します。 1 本の指がオブジェクトをドラッグしたとき、オブジェクトの中心のドラッグのベクターで行をドラッグしたオブジェクトがの中心最初回転します。

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml)ファイルが含まれています、`Picker`のメンバーと共に、`TouchManipulationMode`列挙体。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos.Transforms"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:TouchManipulationMode}">
                    <x:Static Member="local:TouchManipulationMode.None" />
                    <x:Static Member="local:TouchManipulationMode.PanOnly" />
                    <x:Static Member="local:TouchManipulationMode.IsotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.AnisotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.ScaleRotate" />
                    <x:Static Member="local:TouchManipulationMode.ScaleDualRotate" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>
        
        <Grid BackgroundColor="White"
              Grid.Row="1">
            
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

後の方には、`SKCanvasView`と`TouchEffect`1 つのセルに接続されている`Grid`を囲んだ外です。

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs)分離コード ファイルが、`bitmap`型のフィールドはありません`SKBitmap`します。 種類は`TouchManipulationBitmap`(後ほどクラス)。

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

コンス トラクターをインスタンス化、`TouchManipulationBitmap`コンス トラクターに渡して、オブジェクト、`SKBitmap`埋め込みリソースから取得します。 コンス トラクターは、最後に設定して、`Mode`のプロパティ、`TouchManager`のプロパティ、`TouchManipulationBitmap`オブジェクトのメンバーを`TouchManipulationMode`列挙体。

`SelectedIndexChanged`のハンドラー、`Picker`もこれを設定`Mode`プロパティ。

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            bitmap.TouchManager.Mode = (TouchManipulationMode)picker.SelectedItem;
        }
    }
    ...
}
```

`TouchAction`のハンドラー、 `TouchEffect` XAML ファイルの呼び出しで 2 つのメソッドをインスタンス化`TouchManipulationBitmap`という`HitTest`と`ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

場合、`HitTest`メソッドを返します。 `true` &mdash;指がビットマップで占有される領域内で画面をタッチされたことを意味&mdash;に touch ID を追加し、`TouchIds`コレクション。 この ID は、画面から指を離したまで、その指のタッチ イベントのシーケンスを表します。 複数の指タッチ、ビットマップの場合、`touchIds`コレクションには、それぞれの指のタッチ ID が含まれています。

`TouchAction`ハンドラーを呼び出すことも、`ProcessTouchEvent`クラス`TouchManipulationBitmap`します。 ここではいくつか (ただしすべて) の実際のタッチ処理が行われます。

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs)クラスのラッパー クラスは、`SKBitmap`をビットマップをレンダリングし、タッチ イベントを処理するコードを格納しています。 詳細と組み合わせて動作内のコードを汎用化されて、`TouchManipulationManager`クラス (これは、後述します)。

`TouchManipulationBitmap`コンス トラクターの保存、`SKBitmap`し、2 つのプロパティをインスタンス化、`TouchManager`型のプロパティ`TouchManipulationManager`と`Matrix`型のプロパティ`SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

これは、`Matrix`プロパティが累積して変換をすべてのタッチ操作に起因します。 各タッチ イベントは、行列と連結された後に解決わかる、`SKMatrix`に格納される値、`Matrix`プロパティ。

`TouchManipulationBitmap`オブジェクト描画の`Paint`メソッド。 引数が、`SKCanvas`オブジェクト。 これは、 `SKCanvas` 、適用される変換を既に持っているため、`Paint`メソッドを連結、`Matrix`プロパティに既存の変換では、ビットマップに関連付けられているし、キャンバスを復元が完了したら。

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest`メソッドを返します。`true`ユーザーが、ビットマップの境界内のポイント時に画面をタッチする場合。 これは、使用の前に示したロジック、**ビットマップの回転**ページ。

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

2 番目のパブリック メソッドで`TouchManipulationBitmap`は`ProcessTouchEvent`します。 このメソッドが呼び出されると、それが既に確立されて、タッチ イベントは、この特定のビットマップに属していること。 メソッドのディクショナリを保持[ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs)オブジェクトで、前のポイントだけですが、それぞれの指の新しいポイント。

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

ディクショナリをここでは、`ProcessTouchEvent`メソッド自体。

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

`Moved`と`Released`イベント、メソッド呼び出し`Manipulate`します。 次の日時、 `touchDictionary` 1 つ以上含む`TouchManipulationInfo`オブジェクト。 場合、 `touchDictionary` 1 つ含まれる可能性は、項目を`PreviousPoint`と`NewPoint`値を本の指の動きを表す値が等しくないです。 複数の指がビットマップ触れることは、1 つ以上の項目がディクショナリに含まれていますが、これらの項目の 1 つだけが異なる`PreviousPoint`と`NewPoint`値。 残りの部分すべてと等しい`PreviousPoint`と`NewPoint`値。

これは重要なことです。メソッド`Manipulate`は、1本の指の動きのみを処理していると想定できます。 この呼び出しの時点で、その他の本の指のいずれも、移動は、本当に移動している (と可能性があります)、それらの動きが以降の呼び出しで処理されます`Manipulate`します。

`Manipulate`メソッドはまず、利便性のための配列にディクショナリをコピーします。 最初の 2 つのエントリ以外は無視されます。 複数の 2 本の指をビットマップを操作しようとしている、他は無視されます。 `Manipulate` 最後のメンバーは、 `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

1 本の指がビットマップを操作する場合`Manipulate`呼び出し、`OneFingerManipulate`のメソッド、`TouchManipulationManager`オブジェクト。 2 本の指呼び出し`TwoFingerManipulate`します。 これらのメソッド引数は同じ:`prevPoint`と`newPoint`引数が移行する本の指を表します。 `pivotPoint`引数が 2 つの呼び出しに異なります。

1 本の指の操作のため、`pivotPoint`ビットマップの中央です。 これは、1 本指による回転を許可します。 2 本の指の操作のイベントがのみ 1 本の指の動きを示すように、`pivotPoint`が指を移動できません。

どちらの場合も、`TouchManipulationManager`を返します、`SKMatrix`値は、現在のメソッドを連結する`Matrix`プロパティを`TouchManipulationPage`を使用して、ビットマップを描画します。

`TouchManipulationManager` 汎用化されておよびを除くその他のファイルを使用していない`TouchManipulationMode`します。 独自のアプリケーションで変更なしには、このクラスを使用することがあります。 型の 1 つのプロパティを定義`TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


ただし、おそらくたいを避けるために、`AnisotropicScale`オプション。 0 になるようにスケーリング要因の 1 つにビットマップを操作するには、このオプションでは非常に簡単です。 これにより、返さないの姿を消しますビットマップです。 実際に行う必要がある場合異方性スケーリング、望ましくない結果を避けるためのロジックを強化するためにします。

`TouchManipulationManager` 使用して、ベクターがであるためありません`SKVector`SkiaSharp、構造`SKPoint`代わりに使用されます。 `SKPoint` ベクターとして処理できる、減算演算子と、結果をサポートします。 追加するために必要なだけベクトルに固有のロジックは、`Magnitude`計算。

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

回転を選択すると、たびに 1 本の指と 2 本の指の操作方法を最初の回転角度に処理します。 回転が検出されると、回転の成分は削除効果的にします。 残ったものは、パンしてスケーリングとして解釈されます。

ここでは、`OneFingerManipulate`メソッド。 1 本指による回転が有効になっていないかどうかは、ロジックが単純な&mdash;だけを使用して、以前の状態と新しいポイントをという名前のベクターを構築`delta`翻訳に正確に対応します。 1 本指による回転が有効になっている、メソッドを使用して角度 (ビットマップのセンター) のピボット ポイントから、以前の状態と新しいポイントを回転行列を構築します。

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

`TwoFingerManipulate`メソッド、ピボット ポイントは、この特定のタッチ イベントで移行しない指の位置。 回転を行いますが、1 本指による回転とよく似ていますが、ベクターが指定`oldVector`(以前の時点に基づく)、回転に合わせて調整します。 残りの移動は、スケーリングと解釈されます。

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

このメソッドで明示的な変換がないことがわかります。 ただし、両方の`MakeRotation`と`MakeScale`メソッドは、ピボット ポイントに基づいており、暗黙的な変換が含まれます。 ビットマップと同じ方向にドラッグすることで、2 本の指を使用している場合`TouchManipulation`一連の交互の 2 本の指のタッチ イベントを取得します。 他の拡大縮小や回転結果、中心とそれぞれの指に移動しますが、他の指を動かして、符号が反転され、結果は変換。

唯一の残りの部分、**タッチ操作**ページは、`PaintSurface`ハンドラーで、`TouchManipulationPage`分離コード ファイル。 これには、`Paint`のメソッド、 `TouchManipulationBitmap`、蓄積されたタッチ アクティビティを表す行列を適用します。

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface`ハンドラーの終了を表示して、`MatrixDisplay`累積タッチ マトリックスを表示するオブジェクト。

[![](touch-images/touchmanipulation-small.png "タッチ操作ページのスクリーン ショットをトリプル")](touch-images/touchmanipulation-large.png#lightbox "タッチ操作ページの 3 倍になるスクリーン ショット")

## <a name="manipulating-multiple-bitmaps"></a>複数のビットマップを操作します。

などのクラスでのタッチ処理コードを分離する利点の 1 つ`TouchManipulationBitmap`と`TouchManipulationManager`により、ユーザーが複数ビットマップを操作するプログラムでこれらのクラスを再利用できることです。

**ビットマップ散布ビュー**  ページでは、これを行う方法を示します。 型のフィールドを定義するのではなく`TouchManipulationBitmap`、 [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs)クラスを定義、`List`ビットマップ オブジェクトの。

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

コンス トラクターがすべての埋め込みリソースとして使用可能なビットマップの読み込みに追加されます、`bitmapCollection`します。 注意して、`Matrix`で各プロパティが初期化`TouchManipulationBitmap`オブジェクト、各ビットマップの左上隅が 100 ピクセルのオフセットします。

`BitmapScatterView`ページも複数のビットマップのタッチ イベントを処理する必要があります。 定義するのではなく、`List`の Id の現在の操作のタッチ`TouchManipulationBitmap`オブジェクトの場合、このプログラムには、ディクショナリが必要です。

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

通知方法、`Pressed`をループ処理ロジック、`bitmapCollection`逆の順序で。 ビットマップを使用して、他の多くの場合、重複します。 後で、コレクション内のビットマップには、視覚的にコレクションで先ほどビットマップの上にあります。 画面に押した指の下で複数のビットマップがある場合は、最上位の 1 つはその指で操作が 1 つである必要があります。

また、`Pressed`ロジックが視覚的に他のビットマップの山の先頭に移動するように、コレクションの末尾にそのビットマップを移動します。

`Moved`と`Released`、イベント、`TouchAction`ハンドラーの呼び出し、`ProcessingTouchEvent`メソッド`TouchManipulationBitmap`以前のバージョンのプログラムと同様です。

最後に、`PaintSurface`ハンドラーの呼び出し、`Paint`メソッドをそれぞれ`TouchManipulationBitmap`オブジェクト。

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

コードでは、コレクションをループし、コレクションの先頭から末尾までのビットマップの山を表示します。

[![](touch-images/bitmapscatterview-small.png "ビットマップの散布図の表示 ページのスクリーン ショットをトリプル")](touch-images/bitmapscatterview-large.png#lightbox "ビットマップ散布図の表示 ページの 3 倍になるスクリーン ショット")

## <a name="single-finger-scaling"></a>1 本指によるスケーリング

一般に、スケーリング操作には、2 本の指によるピンチ ジェスチャが必要です。 ただし、1 本の指と指のビットマップの角にマウスを移動することでのスケーリングを実装することができます。

これは、方法については、 **1 つの本の指上隅にあるスケール**ページ。 実装するため、このサンプルのスケールよりも若干異なる型を使用して、`TouchManipulationManager`クラス、そのクラスを使用しない、または`TouchManipulationBitmap`クラス。 代わりに、すべてのタッチ ロジックは、分離コード ファイルでは。 これは、一度に、のみ 1 本の指を追跡し、セカンダリの指が画面に触れることがありますを無視するだけのために通常よりもやや単純なロジックです。

[ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml)ページをインスタンス化、`SKCanvasView`クラスし、作成、`TouchEffect`タッチ イベントを追跡するためのオブジェクト。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

[ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs)ファイルからビットマップ リソースが読み込まれる、**メディア**directory 使用して、表示、`SKMatrix`として定義されているオブジェクトをフィールド:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

これは、`SKMatrix`オブジェクトを次に示すタッチ ロジックによって変更します。

分離コード ファイルの残りの部分は、`TouchEffect`イベント ハンドラー。 開始の指を現在の場所を変換することで、`SKPoint`値。 `Pressed`アクションの種類、ハンドラーのチェックが他の指が画面を触れることないことがとビットマップの境界内で指であります。

コードの重要な部分は、 `if` 2 つの呼び出しに関連するステートメント、`Math.Pow`メソッド。 この数学は、ビットマップを格納する楕円の外部での本の指の場所を確認します。 そうである場合をスケーリング操作です。 指がビットマップの四隅のいずれかに近いと反対側の隅にあるピボット ポイントを決定します。 指がこの楕円内にある場合は、通常のパン操作。

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved`アクションの種類は、本の指がこの時点まで、画面を押されてから、タッチ アクティビティに対応するマトリックスを計算します。 その行列に、行列は、指が最初に、ビットマップを押された時に有効に連結します。 スケーリング操作指タッチ 1 番目の対照的上隅に対して相対的では常にします。

小規模またはしっぽビットマップでは、楕円の内部では、ビットマップの大部分を占めるし、ビットマップをスケーリングする角にある小さな領域のままに可能性があります。 ある程度別のアプローチを好む場合があります、その場合は、その全体を置き換えることができます`if`設定ブロック`isScaling`に`true`次のコードで。

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

このコードは、内部菱形にビットマップの領域との角にある 4 つの三角形に効果的に分割します。 これにより、多くの大きな領域を取得して、ビットマップのスケールの隅にあります。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [効果からのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
