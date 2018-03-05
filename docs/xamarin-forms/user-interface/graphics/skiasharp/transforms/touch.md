---
title: "タッチ操作"
description: "タッチ ドラッグ、はさむ、および回転の実装を使用して行列による変換します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: b418e0179c95a424c88d5f5063a09f984bb13ec0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="touch-manipulations"></a>タッチ操作

_タッチ ドラッグ、はさむ、および回転の実装を使用して行列による変換します。_

多くの場合、ユーザーはモバイル デバイス上のものなど、マルチタッチ環境で、画面上のオブジェクトを操作するのに、指を使用します。 1 本指のドラッグと 2 本の指ピンチなどの一般的なジェスチャでは、移動、オブジェクトを拡張およびまたは回転したりもできます。 これらのジェスチャの変換行列を使用したは一般に実装され、この記事を実行する方法を示します。

![](touch-images/touchmanipulationsexample.png "対象となる平行移動、拡大/縮小、および回転ビットマップ")

## <a name="manipulating-one-bitmap"></a>1 つのビットマップを操作します。

**タッチ操作**ページを 1 つのビットマップでタッチ操作に示します。
このサンプルでは、使用、記事で紹介したタッチ追跡効果[効果からイベントを呼び出す](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)。

その他のいくつかのファイルのサポートを提供する、**タッチ操作**ページ。 1 つは、 [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs)決まりましたコードで実装されるタッチ操作のさまざまな種類を示す列挙体。

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

`PanOnly` 変換が実装されている 1 本指ドラッグします。 後続のすべてのオプションも含めるパンは、2 本の指を伴う:`IsotropicScale`均等に水平方向および垂直方向にスケーリングされた結果となるピンチ操作です。 `AnisotropicScale` 等しくない拡張を実現できます。

`ScaleRotate`オプションは、2 本の指のスケールおよび回転します。 スケール、アイソトロ ピックです。 本の指の動きが基本的に同じなので、異方性のスケーリングと 2 本の指の回転を実装することは問題です。

`ScaleDualRotate`オプションが 1 本指の回転を追加します。 1 本の指ドラッグすると、オブジェクトには、オブジェクトの中心がドラッグ ベクターと並ぶようにドラッグされたオブジェクトはの中心回転最初。

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml)ファイルが含まれる、`Picker`のメンバーと、`TouchManipulationMode`列挙型。

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

下部、`SKCanvasView`と`TouchEffect`1 つのセルにアタッチされている`Grid`を囲むことです。

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs)分離コード ファイルが、`bitmap`フィールドがない型の`SKBitmap`します。 種類は`TouchManipulationBitmap`(後ほどクラス)。

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

コンス トラクターをインスタンス化、`TouchManipulationBitmap`コンス トラクターに渡して、オブジェクト、`SKBitmap`埋め込みリソースから取得します。 コンス トラクターが終了を設定して、`Mode`のプロパティ、`TouchManager`のプロパティ、`TouchManipulationBitmap`オブジェクトのメンバーを`TouchManipulationMode`列挙します。

`SelectedIndexChanged`のハンドラーは、`Picker`も設定`Mode`プロパティ。

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

`TouchAction`のハンドラー、 `TouchEffect` 2 つの方法で XAML ファイルの呼び出しでインスタンス化される`TouchManipulationBitmap`という`HitTest`と`ProcessTouchEvent`:

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

場合、`HitTest`メソッドを返します。 `true` & #x 2014; 指がビットマップ & #x 2014; によって占有される領域内で画面をタッチし、に touch ID を追加することの意味、`TouchIds`コレクション。 この ID は、指を画面から解除されるまでに、その指タッチ イベントのシーケンスを表します。 複数の指タッチ、ビットマップの場合、`touchIds`コレクションには各本指タッチ ID が含まれています。

`TouchAction`ハンドラーも呼び出します、`ProcessTouchEvent`クラス`TouchManipulationBitmap`です。 ここでは一部 (すべてではない) 実際のタッチの処理が行われます。

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs)クラスのラッパー クラスは、`SKBitmap`ビットマップを表示し、タッチ イベントを処理するコードを含むです。 詳細と組み合わせて動作内のコードを汎用化、 `TouchManipulationManager` (クラスは間もなく表示されます)。

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

これは、`Matrix`プロパティは、タッチ アクティビティのすべての結果として得られるまでの累積変換します。 連結された、マトリックスに各タッチ イベントを解決するように表示されます、`SKMatrix`によって格納される値、`Matrix`プロパティです。

`TouchManipulationBitmap`オブジェクトを描画自体その`Paint`メソッドです。 引数が、`SKCanvas`オブジェクト。 これは、`SKCanvas`適用されている変換が既にあるため、`Paint`メソッドを連結、`Matrix`プロパティが既存の変換では、ビットマップに関連付けられているし、キャンバスを復元が完了すると。

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

`HitTest`メソッドを返します。`true`場合は、ユーザーが、ビットマップの境界内にある時点で画面に触れるです。 ように、ユーザーは、ビットマップを操作する、ビットマップ、回転する可能性があります。 またはでも (異方性スケールおよび回転の組み合わせ) は平行四辺形の図形にします。 懸念する可能性があります、`HitTest`メソッドは、その場合ではなく複雑な分析ジオメトリを実装する必要があります。

ただし、ショートカットがあります。

変換された四角形の境界内に点がある場合を判断すると、逆の変換された点がされた未変形四角形の境界内にあるかどうかと同じです。 多くの計算が容易であるし、便利なを使用できる`Contains`メソッドによって定義された`SKRect`:

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

2 番目のパブリック メソッドで`TouchManipulationBitmap`は`ProcessTouchEvent`します。 このメソッドが呼び出されると、それが既に確立されてタッチ イベントがこの特定のビットマップに属していること。 メソッドのディクショナリを保持[ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs)オブジェクトで、前のポイントでは単純にし、各本の指の新しい点には。

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

ここでは、ディクショナリ、および`ProcessTouchEvent`メソッド自体。

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

`Moved`と`Released`イベント、メソッド呼び出し`Manipulate`です。 次の日時、 `touchDictionary` 1 つ以上含む`TouchManipulationInfo`オブジェクト。 場合、 `touchDictionary` 1 つ可能性があること、アイテムを`PreviousPoint`と`NewPoint`値が等しくないと、本の指の移動を表します。 ビットマップは、複数の指を変更することと、辞書には、複数の項目が含まれていますが、これらの項目の 1 つだけが異なる`PreviousPoint`と`NewPoint`値。 他のすべてが等しい`PreviousPoint`と`NewPoint`値。

これは重要:`Manipulate`メソッドがのみ 1 本の指の移動を処理していることを想定することができます。 この呼び出しの時点での他の指は移動、されず、実際に移行して (現状可能性があります)、それらの動きが将来の呼び出しで処理されます`Manipulate`です。

`Manipulate`メソッドはまず便宜上配列にディクショナリにコピーします。 最初の 2 つのエントリ以外は無視されます。 複数の 2 本の指をビットマップを操作しようとしている、他は無視されます。 `Manipulate` 最後のメンバーである`TouchManipulationBitmap`:

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

1 本の指が、ビットマップを操作する場合`Manipulate`呼び出し、`OneFingerManipulate`のメソッド、`TouchManipulationManager`オブジェクト。 2 本の指呼び出し`TwoFingerManipulate`です。 これらのメソッド引数は、同じ:`prevPoint`と`newPoint`引数は、移行する指を表します。 `pivotPoint`引数は、2 つの呼び出しに異なります。

1 本指の操作のため、`pivotPoint`ビットマップのセンターです。 これは、1 本指の回転を許可します。 2 本の指の操作のイベントがのみ 1 本の指の動きを示すように、`pivotPoint`移行しない指がします。

どちらの場合も、`TouchManipulationManager`を返します、`SKMatrix`メソッドを連結し、現在の値`Matrix`プロパティを`TouchManipulationPage`ビットマップを表示するために使用します。

`TouchManipulationManager` 汎用化されており、その他のファイルを除くを使用していない`TouchManipulationMode`です。 独自のアプリケーションで変更せずには、このクラスを使用することができます。 型の 1 つのプロパティを定義`TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


ただしがおそらくを避けたい、`AnisotropicScale`オプション。 このオプションを 0 になるようにスケーリングの要因の 1 つにビットマップを操作する非常に簡単です。 返さないの姿を消しますビットマップが、です。 実際に行う必要がある場合異方性スケーリング、望ましくない結果を回避するためのロジックを強化するためにします。

`TouchManipulationManager` 利用ベクターがあるためありません`SKVector`SkiaSharp、構造体`SKPoint`代わりに使用されます。 `SKPoint` では、減算演算子と、結果は、ベクターとして扱うことができます。 追加するために必要なだけベクターに固有のロジックは、`Magnitude`計算。

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

回転が選択されているときに 1 本指と指の 2 つの操作メソッドの両方が最初に回転を処理します。 回転が検出されると、し、回転の成分効果的に削除されます。 新機能は、パンおよび拡大/縮小として解釈されます。

ここでは、`OneFingerManipulate`メソッドです。 1 本指の回転が有効になっていない場合、ロジックは単純な & #x 2014 です。単に使用して、前のポイントと新しいポイントという名前のベクターを構築するために`delta`翻訳に正確に対応します。 1 本指の回転が有効になっている、メソッドを使って角度ピボット ポイント (ビットマップの中央) から、以前の時点と新しいポイント回転行列を作成します。

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

`TwoFingerManipulate`メソッド、ピボット ポイントがこの特定のタッチ イベントで移行しない指の位置。 回転は、1 本指のローテーションによく似ています、ベクターが名前付き`oldVector`(前のポイントに基づく) は、回転を調整します。 残りの移動は、スケーリングと解釈されます。

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

このメソッドでは、明示的な変換はありませんがわかります。 ただし、両方、`MakeRotation`と`MakeScale`メソッドは、ピボット ポイントに基づいており、暗黙的な変換が含まれます。 ビットマップと、同じ方向にドラッグすることで、2 本の指を使用している場合`TouchManipulation`一連の 2 本の指の間で切り替えるタッチ イベントを取得します。 各本の指の移動、他のスケールまたは回転結果、基準としたが、他の指の動きを否定、結果は翻訳します。

唯一の残りの部分、**タッチ操作**ページは、`PaintSurface`ハンドラー、`TouchManipulationPage`分離コード ファイル。 呼び出し、`Paint`のメソッド、 `TouchManipulationBitmap`、蓄積されたタッチ アクティビティを表す行列を適用します。

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

[![](touch-images/touchmanipulation-small.png "タッチ操作ページのスクリーン ショットをトリプル")](touch-images/touchmanipulation-large.png "タッチ操作ページのトリプル スクリーン ショット")

## <a name="manipulating-multiple-bitmaps"></a>複数のビットマップを操作します。

などのクラスでのタッチ処理コードを分離する利点の 1 つ`TouchManipulationBitmap`と`TouchManipulationManager`により、ユーザーが複数のビットマップを操作するプログラムでこれらのクラスを再利用する機能です。

**ビットマップ散布図ビュー**  ページでは、これを行う方法を示しています。 型のフィールドを定義するのではなく`TouchManipulationBitmap`、 [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs)クラスを定義、`List`ビットマップ オブジェクト。

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
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
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

コンス トラクターは、すべてのビットマップの埋め込みリソースとして使用できるを読み込んでに追加、`bitmapCollection`です。 注意して、`Matrix`プロパティは、各初期化`TouchManipulationBitmap`オブジェクト、ビットマップごとの左上隅の x 100 ピクセル オフセットようにします。

`BitmapScatterView`ページも複数のビットマップのタッチ イベントを処理する必要があります。 定義するのではなく、`List`の Id の現在の操作のタッチ`TouchManipulationBitmap`オブジェクトをこのプログラムには、辞書が必要です。

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

通知方法、`Pressed`ロジックがループ、`bitmapCollection`逆の順序で。 ビットマップを使用して、他の多くの場合、重なっています。 後で、コレクション内のビットマップには、コレクションの前半のビットマップの上に視覚的にあります。 指を画面に押した下にある複数のビットマップがある場合は、最上位の 1 つは、その指によって操作される 1 つをする必要があります。

また、`Pressed`ロジックが視覚的に他のビットマップの積み上がりの先頭に移動するように、コレクションの末尾にそのビットマップを移動します。

`Moved`と`Released`、イベント、`TouchAction`ハンドラーの呼び出し、`ProcessingTouchEvent`メソッド`TouchManipulationBitmap`以前のバージョンのプログラムと同じようにします。

最後に、`PaintSurface`ハンドラーの呼び出し、`Paint`それぞれの`TouchManipulationBitmap`オブジェクト。

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

コードでは、コレクションをループし、コレクションの先頭から末尾までのビットマップの積み上がりを表示します。

[![](touch-images/bitmapscatterview-small.png "ビットマップの散布図の表示 ページのスクリーン ショットをトリプル")](touch-images/bitmapscatterview-large.png "ビットマップ散布図の表示 ページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [呼び出し元のイベントの効果を適用](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
