---
title: タッチ操作
description: この記事では、マトリックス変換を使用してタッチドラッグ、ピンチ、および回転を実装する方法について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: davidbritch
ms.author: dabritch
ms.date: 09/14/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ee69ca1e95f7dcffa60387579e89c3a2d3e985da
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374291"
---
# <a name="touch-manipulations"></a>タッチ操作

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_マトリックス変換を使用してタッチドラッグ、ピンチ、および回転を実装する_

モバイルデバイス上のようなマルチタッチ環境では、ユーザーが指を使用して画面上のオブジェクトを操作することがよくあります。 ワンフィンガードラッグや2つの指によるピンチ操作などの一般的なジェスチャでは、オブジェクトの移動や拡大、または回転が可能です。 これらのジェスチャは、一般に変換行列を使用して実装されます。この記事では、その方法について説明します。

![平行移動、拡大縮小、および回転に影響を受けるビットマップ](touch-images/touchmanipulationsexample.png)

ここに示されているすべてのサンプルでは、 Xamarin.Forms 「 [**効果からのイベントの呼び出し**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)」に示されているタッチトラッキング効果を使用します。

## <a name="dragging-and-translation"></a>ドラッグアンド平行移動

マトリックス変換の最も重要なアプリケーションの1つは、タッチ処理です。 1つの値で、 [`SKMatrix`](xref:SkiaSharp.SKMatrix) 一連のタッチ操作を統合できます。 

1本の指をドラッグした場合、 `SKMatrix` 値は変換を実行します。 これについては、「 **ビットマップのドラッグ** 」ページで説明されています。 XAML ファイルは、内のをインスタンス化し `SKCanvasView` Xamarin.Forms `Grid` ます。 `TouchEffect`オブジェクトが、のコレクションに追加されました `Effects` `Grid` 。

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

理論的には、 `TouchEffect` オブジェクトはのコレクションに直接追加でき `Effects` `SKCanvasView` ますが、すべてのプラットフォームでは機能しません。 は、 `SKCanvasView` この構成のと同じサイズであるため、 `Grid` へのアタッチも同様に `Grid` 機能します。

分離コードファイルは、コンストラクター内のビットマップリソースに読み込まれ、ハンドラーに表示され `PaintSurface` ます。

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

コードを追加しない場合、 `SKMatrix` 値は常に識別行列になり、ビットマップの表示には影響しません。 `OnTouchEffectAction`XAML ファイルで設定されたハンドラーの目的は、タッチ操作を反映するようにマトリックスの値を変更することです。

この `OnTouchEffectAction` ハンドラーは、値を Xamarin.Forms SkiaSharp 値に変換することによって開始され `Point` `SKPoint` ます。 これは、 `Width` `Height` のプロパティとプロパティ `SKCanvasView` (デバイスに依存しない単位) とプロパティ (ピクセル単位) に基づいて、簡単にスケーリングでき `CanvasSize` ます。

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

指が画面に触れると、型のイベント `TouchActionType.Pressed` が発生します。 最初のタスクは、指がビットマップに接しているかどうかを判断することです。 このようなタスクは、しばしば _ヒットテスト_ と呼ばれます。 この場合、ビットマップに対応する値を作成し、を `SKRect` 使用してマトリックス変換を適用した `MapRect` 後、タッチポイントが変換された四角形内にあるかどうかを判断することで、ヒットテストを行うことができます。

その場合は、 `touchId` フィールドがタッチ ID に設定され、指位置が保存されます。

イベントの場合 `TouchActionType.Moved` 、値の変換係数 `SKMatrix` は、指の現在位置と指の新しい位置に基づいて調整されます。 その新しい位置は、次のときに保存され、は `SKCanvasView` 無効になります。

このプログラムを試してみると、ビットマップが表示されている領域に指が触れるときにのみ、ビットマップをドラッグできることに注意してください。 この制限は、このプログラムにとって非常に重要ではありませんが、複数のビットマップを操作するときに重要になります。

## <a name="pinching-and-scaling"></a>ピンチとスケーリング

2本の指がビットマップに触れるときの動作 2本の指が並列で移動する場合は、ビットマップを指と一緒に移動させることをお勧めします。 2本の指がピンチ操作またはストレッチ演算を実行する場合は、ビットマップを回転させる (次のセクションで説明します) か、または拡大縮小することができます。 ビットマップをスケーリングする場合、2本の指はビットマップに対して相対的に同じ位置に保持され、ビットマップはそれに応じて拡大縮小されます。

2本の指を一度に処理するのは複雑ですが、ハンドラーは一度 `TouchAction` に1本の指に関する情報のみを受け取ることに注意してください。 2本の指がビットマップを操作している場合、各イベントについて、ある指が位置を変更したが、もう一方は変更されていません。 次の **ビットマップスケーリング** ページコードでは、位置が変更されていない指が _ピボット_ ポイントと呼ばれます。これは、変換がその点に対して相対的であるためです。

このプログラムと前のプログラムの違いの1つは、複数のタッチ Id を保存する必要があることです。 この目的では、ディクショナリが使用されます。ここで、touch ID はディクショナリキー、ディクショナリ値はその指の現在位置です。

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

アクションの処理 `Pressed` は、前のプログラムとほぼ同じですが、ID とタッチポイントがディクショナリに追加される点が異なります。 `Released`And アクションは、 `Cancelled` 辞書のエントリを削除します。

ただし、アクションの処理 `Moved` はより複雑です。 指が1つしか含まれていない場合、処理は前のプログラムとほぼ同じです。 2つ以上の指の場合、プログラムは、移動していない指を含むディクショナリから情報を取得する必要もあります。 これを行うには、ディクショナリキーを配列にコピーし、最初のキーと移動する指の ID を比較します。 これにより、プログラムは、移動していない指に対応するピボットポイントを取得できます。

次に、ピボットポイントを基準とする新しい指位置の2つのベクトルと、ピボットポイントを基準とした古い指位置を計算します。 これらのベクトルの比率は、スケールファクターです。 0による除算は可能性があるので、無限値または NaN (非数) の値をチェックする必要があります。 すべてが適切な場合は、スケーリング変換が `SKMatrix` フィールドとして保存された値と連結されます。

このページを試してみると、ビットマップを1つまたは2本の指でドラッグしたり、2本の指で拡大したりできることがわかります。 スケーリングは _異方_ です。これは、水平方向と垂直方向でスケーリングが異なる場合があることを意味します。 これにより、縦横比が歪曲されますが、ビットマップを反転してミラーイメージを作成することもできます。 ビットマップを0次元に縮小しても、表示されなくなることがあります。 運用環境のコードでは、このことを防ぐ必要があります。

## <a name="two-finger-rotation"></a>2本指による回転

[ **ビットマップの回転** ] ページでは、回転または等幅のスケーリングに2本の指を使用できます。 ビットマップでは、常に正しい縦横比が維持されます。 2本の指を回転と異方に使用しても、指の動きは両方のタスクで非常によく似ているため、非常にうまく機能しません。

このプログラムの最初の大きな違いは、ヒットテストロジックです。 前のプログラムでは、のメソッドを使用して、 `Contains` `SKRect` タッチポイントが、ビットマップに対応する変換された四角形内にあるかどうかを確認していました。 ただし、ユーザーがビットマップを操作すると、ビットマップが回転し、 `SKRect` 回転した四角形を正しく表すことができなくなる可能性があります。 ヒットテストロジックでは、複雑な分析ジオメトリを実装する必要があると心配する場合があります。

ただし、ショートカットを使用できます。変換された四角形の境界内にポイントがあるかどうかを判断することは、逆方向に変換された点が着想四角形の境界内にあるかどうかを判断することと同じです。 これは非常に簡単な計算であり、ロジックは引き続き便利な方法を使用でき `Contains` ます。

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

イベントのロジックは、 `Moved` 前のプログラムと同じように開始されます。 とという名前の2つのベクトルは、移動する指 `oldVector` `newVector` の前の点と現在のポイント、および移動していない指のピボットポイントに基づいて計算されます。 ただし、これらのベクトルの角度は決定され、差は回転角度になります。

また、拡大縮小が必要になる場合もあるため、回転角度に基づいて古いベクターが回転します。 2つのベクトルの相対絶対値がスケールファクターになります。 `scale`スケーリングが等値になるように、水平方向と垂直方向のスケーリングに同じ値が使用されていることに注意してください。 この `matrix` フィールドは、回転行列とスケール行列の両方によって調整されます。

アプリケーションで1つのビットマップ (またはその他のオブジェクト) のタッチ処理を実装する必要がある場合は、これら3つのサンプルのコードを独自のアプリケーションに適合させることができます。 しかし、複数のビットマップに対してタッチ処理を実装する必要がある場合は、これらのタッチ操作を他のクラスにカプセル化することをお勧めします。

## <a name="encapsulating-the-touch-operations"></a>タッチ操作をカプセル化する

**タッチ操作** ページは、1つのビットマップのタッチ操作を示しますが、上記のロジックの多くをカプセル化した他のいくつかのファイルを使用します。 最初のファイルは列挙型であり [`TouchManipulationMode`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) 、表示するコードによって実装されるさまざまな種類のタッチ操作を示します。

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

`PanOnly` は、平行移動によって実装されるワンフィンガードラッグです。 後続のすべてのオプションには、パンも含まれますが、2本の指が含まれています。 `IsotropicScale` は、水平方向および垂直方向に均等にオブジェクトをスケーリングするピンチ演算です。 `AnisotropicScale` 等しくないスケーリングを可能にします。

この `ScaleRotate` オプションは、2本の指によるスケーリングと回転を行う場合に使用します。 スケーリングは等幅です。 前述のように、2本の指による回転を実装することは、指の動きが基本的に同じであるため問題になります。

オプションは、 `ScaleDualRotate` 1 つの指の回転を追加します。 1本の指でオブジェクトをドラッグすると、ドラッグされたオブジェクトは最初に中心を中心に回転し、オブジェクトの中心がドラッグベクターで上になります。

[**TouchManipulationPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml)ファイルには、 `Picker` 列挙体のメンバーを含むが含まれてい `TouchManipulationMode` ます。

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

下端に向かっては、とは、 `SKCanvasView` `TouchEffect` それを囲む1つのセルに結び付けられてい `Grid` ます。

[**TouchManipulationPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs)分離コードファイルにはフィールドがありますが、 `bitmap` 型ではありません `SKBitmap` 。 型は `TouchManipulationBitmap` (すぐに表示されるクラス) です。

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

コンストラクターは、 `TouchManipulationBitmap` `SKBitmap` 埋め込まれたリソースから取得したコンストラクターに渡して、オブジェクトをインスタンス化します。 コンストラクターは、 `Mode` オブジェクトのプロパティのプロパティを `TouchManager` `TouchManipulationBitmap` 列挙体のメンバーに設定することによって終了し `TouchManipulationMode` ます。

の `SelectedIndexChanged` ハンドラーは、 `Picker` このプロパティも設定し `Mode` ます。

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

`TouchAction`XAML ファイルでインスタンス化されたのハンドラーは、 `TouchEffect` `TouchManipulationBitmap` とという名前の2つのメソッドを呼び出し `HitTest` `ProcessTouchEvent` ます。

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

この `HitTest` メソッドがを返す場合、 `true` &mdash; タッチ ID がビットマップによって占有されている領域内の画面に触れると、 &mdash; タッチ ID がコレクションに追加され `TouchIds` ます。 この ID は、指が画面から離した後の指のタッチイベントのシーケンスを表します。 複数の指がビットマップに触れる場合、 `touchIds` コレクションには各指のタッチ ID が含まれます。

ハンドラーは、 `TouchAction` でも `ProcessTouchEvent` クラスを呼び出し `TouchManipulationBitmap` ます。 ここで、実際のタッチ処理の一部 (ただしすべてではありません) が発生します。

[`TouchManipulationBitmap`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs)クラスは、 `SKBitmap` ビットマップおよびプロセスタッチイベントをレンダリングするコードを含むのラッパークラスです。 クラスの一般化されたコードと組み合わせて動作 `TouchManipulationManager` します (これについては後ほど説明します)。

`TouchManipulationBitmap`コンストラクターは、を保存し、 `SKBitmap` 2 つのプロパティ ( `TouchManager` 型のプロパティ `TouchManipulationManager` と `Matrix` 型のプロパティ) をインスタンス化し `SKMatrix` ます。

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

この `Matrix` プロパティは、すべてのタッチアクティビティの結果として得られる累積変換です。 ご覧のとおり、各タッチイベントはマトリックスに解決され、その後、 `SKMatrix` プロパティによって格納された値と連結され `Matrix` ます。

オブジェクトは、 `TouchManipulationBitmap` メソッドにそれ自体を描画し `Paint` ます。 引数は `SKCanvas` オブジェクトです。 これには `SKCanvas` 既に変換が適用されている場合があるため、 `Paint` メソッドは `Matrix` ビットマップに関連付けられているプロパティを既存の変換に連結し、完了したときにキャンバスを復元します。

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

`HitTest` `true` ユーザーがビットマップの境界内の点で画面に触れると、メソッドはを返します。 これは、前の「ビットマップの **回転** 」ページで示したロジックを使用します。

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

の2番目のパブリックメソッド `TouchManipulationBitmap` は `ProcessTouchEvent` です。 このメソッドが呼び出されると、タッチイベントがこの特定のビットマップに属していることが既に確立されています。 メソッドは、オブジェクトのディクショナリを保持します [`TouchManipulationInfo`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) 。これは、単に前の点と各指の新しいポイントです。

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

次に、ディクショナリと `ProcessTouchEvent` メソッド自体を示します。

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

`Moved`イベントおよびイベントでは、 `Released` メソッドはを呼び出し `Manipulate` ます。 これらの時刻には、に `touchDictionary` 1 つ以上のオブジェクトが含まれてい `TouchManipulationInfo` ます。 に `touchDictionary` 1 つの項目が含まれている場合は、との値が等しくないため、指の動きを表している可能性があり `PreviousPoint` `NewPoint` ます。 複数の指がビットマップに接している場合、ディクショナリには複数の項目が含まれますが、値が異なるのはこれらの項目のうちの1つだけです `PreviousPoint` `NewPoint` 。 残りのすべての `PreviousPoint` 値は、との値と同じ `NewPoint` です。

これは重要なことです。メソッドは、 `Manipulate` 1 本の指の動きだけを処理していると見なすことができます。 この呼び出しの時点では、他の指は移動していませんが、実際に移動している場合は、の後の呼び出しでこれらの移動が処理され `Manipulate` ます。

メソッドは、 `Manipulate` 便宜上、ディクショナリを配列にコピーします。 最初の2つのエントリ以外は無視されます。 2つ以上の指がビットマップを操作しようとしている場合、他の指は無視されます。 `Manipulate` は、の最後のメンバーです `TouchManipulationBitmap` 。

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

1本の指がビットマップを操作している場合は、に `Manipulate` `OneFingerManipulate` よってオブジェクトのメソッドが呼び出され `TouchManipulationManager` ます。 2本の指の場合、を呼び出し `TwoFingerManipulate` ます。 これらのメソッドの引数は同じです。引数 `prevPoint` と `newPoint` 引数は、移動する指を表します。 ただし、 `pivotPoint` 次の2つの呼び出しでは引数が異なります。

1本指で操作する場合、は `pivotPoint` ビットマップの中心です。 これは、1つの指による回転を可能にするためです。 2本指による操作の場合、イベントは1本の指の動きだけを示します。これにより、は `pivotPoint` 移動しない指になります。

どちらの場合も、は値を返します。この値は、が `TouchManipulationManager` `SKMatrix` `Matrix` `TouchManipulationPage` ビットマップのレンダリングに使用する現在のプロパティと連結します。

`TouchManipulationManager` は一般化されており、以外の他のファイルは使用しません `TouchManipulationMode` 。 独自のアプリケーションを変更することなく、このクラスを使用できる場合があります。 `TouchManipulationMode` 型の単一プロパティが定義されます。

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```

ただし、このオプションは使用しないことをお勧めし `AnisotropicScale` ます。 このオプションを使用すると、スケールファクターの1つがゼロになるようにビットマップを操作することが非常に簡単になります。 これにより、ビットマップが表示されなくなり、返されなくなります。 異方性のスケーリングが本当に必要な場合は、望ましくない結果を避けるためにロジックを強化する必要があります。

`TouchManipulationManager` ベクターを使用しますが、 `SKVector` SkiaSharp に構造がないため、を代わりに `SKPoint` 使用します。 `SKPoint` 減算演算子をサポートし、結果をベクターとして扱うことができます。 追加する必要があるベクター固有のロジックは、計算だけです `Magnitude` 。

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

回転が選択されている場合は常に、1本指と2本指操作のメソッドが最初に回転を処理します。 回転が検出されると、回転コンポーネントは実質的に削除されます。 残りの部分は、パンおよびスケーリングとして解釈されます。

メソッドを次に示し `OneFingerManipulate` ます。 1本指による回転が有効になっていない場合、ロジックは単純に、 &mdash; 前の点と新しい点を使用して、 `delta` 平行移動に相当するという名前のベクターを作成するだけです。 1本指による回転が有効になっている場合、メソッドはピボットポイント (ビットマップの中心) から前の点までの角度を使用して、回転行列を構築します。

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

メソッドで `TwoFingerManipulate` は、ピボットポイントは、この特定のタッチイベントで移動していない指の位置です。 回転は1本の指の回転とよく似ており、 `oldVector` (前の点に基づく) という名前のベクターが回転のために調整されます。 残りの移動は、スケーリングとして解釈されます。

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

このメソッドには明示的な変換がないことがわかります。 ただし、 `MakeRotation` メソッドとメソッドはどちらも `MakeScale` ピボットポイントに基づいており、暗黙的な変換が含まれています。 ビットマップに2本の指を使用していて、同じ方向にドラッグしている場合、 `TouchManipulation` は、2本の指の間に一連のタッチイベントを交互に表示します。 各指はもう一方の点を基準に移動しますが、拡大縮小または回転の結果は逆になりますが、その他の指の動きによって否定され、結果は変換になります。

**タッチ操作** ページの残りの部分は、 `PaintSurface` 分離コードファイル内のハンドラーだけです `TouchManipulationPage` 。 これにより `Paint` 、のメソッドが呼び出され `TouchManipulationBitmap` 、累積したタッチアクティビティを表す行列が適用されます。

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

この `PaintSurface` ハンドラーは、累積し `MatrixDisplay` たタッチ行列を示すオブジェクトを表示して終了します。

[![タッチ操作ページのトリプルスクリーンショット](touch-images/touchmanipulation-small.png)](touch-images/touchmanipulation-large.png#lightbox "タッチ操作ページのトリプルスクリーンショット")

## <a name="manipulating-multiple-bitmaps"></a>複数のビットマップの操作

やなどのクラスでタッチ処理コードを分離する利点の 1 `TouchManipulationBitmap` つ `TouchManipulationManager` は、ユーザーが複数のビットマップを操作できるようにするプログラムでこれらのクラスを再利用できることです。

[ **ビットマップ散布図** ] ページには、その方法が示されています。 クラスは、型のフィールドを定義するのではなく、 `TouchManipulationBitmap` [`BitmapScatterPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) `List` ビットマップオブジェクトのを定義します。

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

このコンストラクターは、埋め込みリソースとして使用できるすべてのビットマップを読み込み、に追加し `bitmapCollection` ます。 各オブジェクトでプロパティが初期化されていることに注意して `Matrix` `TouchManipulationBitmap` ください。したがって、各ビットマップの左上隅は100ピクセルでオフセットされます。

このページでは、 `BitmapScatterView` 複数のビットマップのタッチイベントも処理する必要があります。 `List`現在操作中のオブジェクトのタッチ id のを定義するのではなく `TouchManipulationBitmap` 、このプログラムには辞書が必要です。

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

`Pressed`ロジックがを逆にループ処理する方法に注目して `bitmapCollection` ください。 多くの場合、ビットマップは互いに重なっています。 コレクション内の後のビットマップは、コレクションの前のビットマップの上に視覚的に配置されます。 画面上にある指の下に複数のビットマップがある場合は、一番上のビットマップが、その指によって操作されているものである必要があります。

また、ロジックがその `Pressed` ビットマップをコレクションの末尾に移動して、他のビットマップの積み重ねの一番上に視覚的に移動することにも注意してください。

`Moved`イベントおよびイベントでは `Released` 、 `TouchAction` ハンドラーは、 `ProcessingTouchEvent` 前のプログラムと同様に、のメソッドを呼び出し `TouchManipulationBitmap` ます。

最後に、 `PaintSurface` ハンドラーは `Paint` 各オブジェクトのメソッドを呼び出し `TouchManipulationBitmap` ます。

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

このコードはコレクションをループ処理し、コレクションの先頭から末尾までのビットマップの束を表示します。

[![ビットマップ散布図ビューページのトリプルスクリーンショット](touch-images/bitmapscatterview-small.png)](touch-images/bitmapscatterview-large.png#lightbox "ビットマップ散布図ビューページのトリプルスクリーンショット")

## <a name="single-finger-scaling"></a>Single-Finger スケーリング

通常、スケーリング操作には、2本の指を使用したピンチジェスチャが必要です。 ただし、指を使用してビットマップの角を動かすことで、1本の指でスケーリングを実装することができます。

これについては、 **単一の指コーナースケール** のページで説明されています。 このサンプルでは、クラスに実装されているものとは多少異なる種類のスケーリングを使用しているため `TouchManipulationManager` 、そのクラスまたはクラスは使用しません `TouchManipulationBitmap` 。 代わりに、すべてのタッチロジックが分離コードファイルに含まれています。 これは、一度に1本の指だけを追跡し、画面に接している可能性のあるすべてのタッチ指を無視するだけなので、通常よりもやや単純なロジックです。

[**SingleFingerCornerScale**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml)ページはクラスをインスタンス化 `SKCanvasView` し、 `TouchEffect` タッチイベントを追跡するためのオブジェクトを作成します。

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

[**SingleFingerCornerScalePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs)ファイルは、**メディア** ディレクトリからビットマップリソースを読み込み、 `SKMatrix` フィールドとして定義されたオブジェクトを使用して表示します。

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

この `SKMatrix` オブジェクトは、次に示すタッチロジックによって変更されます。

分離コードファイルの残りの部分は、 `TouchEffect` イベントハンドラーです。 まず、指の現在位置を値に変換し `SKPoint` ます。 アクションの種類については `Pressed` 、ハンドラーは、他の指が画面に触れていないこと、および指がビットマップの境界内にあることを確認します。

コードの重要な部分は、 `if` メソッドの2つの呼び出しを含むステートメントです `Math.Pow` 。 この数値演算では、指の位置が、ビットマップを埋める楕円の外側にあるかどうかを確認します。 その場合、スケーリング操作です。 指はビットマップの四隅の近くにあり、反対側のコーナーであることがピボットポイントによって決定されます。 指がこの楕円内にある場合は、通常のパン操作になります。

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

アクションの種類によって、 `Moved` タッチアクティビティに対応する行列が計算されます。この時間が経過すると、指が画面を上に押した時間から表示されます。 このマトリックスは、指が最初にビットマップを押したときに有効になっているマトリックスと連結されます。 スケーリング操作は、指がタッチした角と逆のコーナーに対して常に相対的です。

小さいビットマップまたはしっぽビットマップの場合、内部楕円はビットマップの大部分を占め、四隅に小さな領域を置いてビットマップを拡大することがあります。 少し異なる方法を使用することもできます。この場合、がに設定されているブロック全体を `if` `isScaling` `true` 次のコードに置き換えることができます。

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

このコードにより、ビットマップの領域が内部のひし形に分割され、四隅に4つの三角形が挿入されます。 これにより、角の大規模な領域でビットマップを取得し、拡大縮小することができます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [エフェクトからのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)