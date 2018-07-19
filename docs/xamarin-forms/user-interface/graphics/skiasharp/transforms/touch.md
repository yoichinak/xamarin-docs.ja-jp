---
title: タッチ操作
description: この記事では、行列変換を使用して、ドラッグのタッチ、ピンチ、および回転を実装する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: 2de5b9a3a6bf0d36330212a52ba5c7278b970efc
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130908"
---
# <a name="touch-manipulations"></a>タッチ操作

_タッチをドラッグする、ピンチ、および回転を実装するために使用して行列の変換します。_

多くの場合、ユーザーはモバイル デバイスなど、マルチタッチ環境で、画面上のオブジェクトを操作するのに指を使用します。 1 本の指のドラッグと 2 本の指によるピンチなどの一般的なジェスチャでは、移動、物体、およびまたはも回転したりできます。 これらのジェスチャは、変換行列を使用して実装されている一般に、この記事では、その方法を示します。

![](touch-images/touchmanipulationsexample.png "平行移動、拡大縮小、および回転を受けたビットマップ")

## <a name="manipulating-one-bitmap"></a>1 つのビットマップを操作します。

**タッチ操作**ページは、1 つのビットマップのタッチ操作を示します。
このサンプルの記事で紹介したタッチ追跡効果の利用[効果からイベントを呼び出す](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)します。

その他のいくつかのファイルのサポートを提供する、**タッチ操作**ページ。 1 つは、 [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs)いただけるコードで実装されるタッチ操作の種類を示す列挙体。

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

`ScaleRotate`オプションは 2 本の指が拡大縮小および回転用です。 スケーリングは、アイソトロ ピックです。 アニソトロ ピック スケーリングと 2 本の指による回転を実装する、問題のあるは、本の指の動きは基本的に同じためです。

`ScaleDualRotate`オプションが 1 本指による回転を追加します。 1 本の指がオブジェクトをドラッグしたとき、オブジェクトの中心のドラッグのベクターで行をドラッグしたオブジェクトがの中心最初回転します。

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml)ファイルが含まれています、`Picker`のメンバーと共に、`TouchManipulationMode`列挙体。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
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
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
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
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
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

`HitTest`メソッドを返します。`true`ユーザーが、ビットマップの境界内のポイント時に画面をタッチする場合。 ユーザーは、ビットマップを操作する、またはビットマップ、回転する場合がありますも (異方性の拡大縮小および回転の組み合わせ)、平行四辺形の形にします。 心配することがあります、`HitTest`メソッドは、その場合ではなく複雑な分析のジオメトリを実装する必要があります。

ただし、ショートカットがあります。

変換された四角形の境界内に点がある場合を判断すると、逆の変換されたポイントが 未変換四角形の境界内に存在するかどうかと同じです。 くらい簡単に計算があるし、便利な使用`Contains`メソッドによって定義された`SKRect`:

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

これは重要です。、`Manipulate`メソッドでは、1 つだけの指の動きを処理していると想定できます。 この呼び出しの時点で、その他の本の指のいずれも、移動は、本当に移動している (と可能性があります)、それらの動きが以降の呼び出しで処理されます`Manipulate`します。

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

これは、方法については、 **1 つの本の指上隅にあるスケール**ページ。 このサンプルはことで実装されている若干異なるタイプのスケールを使用するため、`TouchManipulationManager`クラス、そのクラスを使用しない、または`TouchManipulationBitmap`クラス。 代わりに、すべてのタッチ ロジックは、分離コード ファイルでは。 これは、一度に、のみ 1 本の指を追跡し、セカンダリの指が画面に触れることがありますを無視するだけのために通常よりもやや単純なロジックです。

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

小規模またはしっぽビットマップでは、楕円の内部では、ビットマップの大部分を占めるし、ビットマップをスケーリングする角を非常に小さな領域のままに可能性があります。 ある程度別のアプローチを好む場合があります、その場合は、その全体を置き換えることができます`if`設定ブロック`isScaling`に`true`次のコードで。

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

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [効果からのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
