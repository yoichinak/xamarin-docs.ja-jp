---
title: SkiaSharp の3D 回転
description: この記事では、非アフィン変換を使用して3D 空間で2D オブジェクトを回転させる方法について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 855fde62483dcbc6f8769e7a8eb66d84aadfe1da
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934980"
---
# <a name="3d-rotations-in-skiasharp"></a>SkiaSharp の3D 回転

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_非アフィン変換を使用して、3D 空間内の2D オブジェクトを回転します。_

非アフィン変換の一般的なアプリケーションの1つは、3D 空間における2D オブジェクトの回転をシミュレートすることです。

![3D 空間で回転したテキスト文字列](3d-rotation-images/3drotationsexample.png)

このジョブでは、3次元の回転を操作した後、これらの3D 回転を実行する非アフィン変換を派生させ `SKMatrix` ます。

この変換は、 `SKMatrix` 2 つのディメンション内でのみ動作するように開発するのは困難です。 この3×3行列が3D グラフィックスで使用される4×4行列から派生している場合、ジョブははるかに簡単になります。 SkiaSharp には [`SKMatrix44`](xref:SkiaSharp.SKMatrix44) この目的のクラスが含まれていますが、3d 回転と4×4変換行列を理解するために3d グラフィックスの背景がいくつか必要です。

3次元の座標系は、Z という3番目の軸を追加します。概念的には、Z 軸は画面に直角に位置しています。 3D 空間の座標点は、3つの数値 (x、y、z) で示されます。 この記事で使用されている3D 座標系では、X の値を大きくすると、2つの次元のように Y の値が大きくなります。 正の Z 値を大きくすると画面が表示されなくなります。 原点は、2D グラフィックスと同様、左上隅にあります。 この画面は、Z 軸がこの平面と直角にある XY 平面と考えることができます。

これは、左側の座標系と呼ばれます。 左側の人差し指でが正の X 座標 (右側) の方向に向かっていて、中心の指が Y 座標を上げる方向 (下) である場合は、Z 座標を大きくする方向 (下) で thumb をポイントします。画面から拡張します。

3D グラフィックスでは、変換は4×4の行列に基づいています。 4×4の id 行列は次のようになります。

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4×4のマトリックスを使用する場合、行と列の番号を含むセルを識別すると便利です。

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

ただし、SkiaSharp `Matrix44` クラスは少し異なります。 で個別のセル値を設定または取得する唯一の方法は、 `SKMatrix44` インデクサーを使用することです [`Item`](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32)) 。 行と列のインデックスは、1から始まるのではなく、ゼロから始まり、行と列はスワップされます。 上の図のセル M14 は、オブジェクトのインデクサーを使用してアクセスされ `[3, 0]` `SKMatrix44` ます。

3D グラフィックスシステムでは、3D ポイント (x、y、z) が4×4の変換行列によって乗算されるように、1×4の行列に変換されます。

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

3次元で行われる2D 変換に似ていますが、3D 変換は4つの次元で行われると見なされます。 4番目の次元は W と呼ばれ、3D 空間は、W 座標が1と等しい4D 空間内に存在すると見なされます。 変換式は次のとおりです。

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

変換式では、セルが `M11` `M22` `M33` x、y、および z 方向のスケールファクターであることがわかります。また、、、 `M41` `M42` および `M43` は、x、y、および z 方向の変換要因です。

これらの座標を、W が1に等しい3D 空間に戻すには、x '、y '、および z ' 座標がすべて w ' で除算されます。

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

W による除算では、3D 空間のパースペクティブが提供されます。 W ' が1の場合、パースペクティブは発生しません。

3D 空間での回転は非常に複雑になることがありますが、最も単純な回転は、X 軸、Y 軸、Z 軸を中心にしています。 X 軸を中心とした角度の回転は次のようにαます。

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X の値は、この変換を対象とする場合は変わりません。 Y 軸を中心に回転すると、Y の値は変更されません。

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z 軸を中心とする回転は、2D グラフィックスの場合と同じです。

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

回転の方向は、座標系のきき手によって暗黙的に示されます。 これは左手座標系です。そのため、特定の軸の回転を左右するように左側のつまみをポイントし、Y 軸を中心に回転する場合は下向き、Z 軸を中心に回転する場合は、正の角度の回転の方向を示すことになります。

`SKMatrix44`には、 [`CreateRotation`](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single)) [`CreateRotationDegrees`](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single)) 回転の中心となる軸を指定できる一般化された静的メソッドとメソッドがあります。

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X 軸を中心に回転する場合は、最初の3つの引数を1、0、0に設定します。 Y 軸を中心に回転する場合は、それらを0、1、0に設定し、Z 軸を中心とした回転を行うには、0、0、1に設定します。

4×4の4列目はパースペクティブを対象としています。 には、 `SKMatrix44` パースペクティブ変換を作成するためのメソッドはありませんが、次のコードを使用して独自に作成することができます。

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

引数名の理由は、 `depth` すぐに明らかになります。 このコードによってマトリックスが作成されます。

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

変換式では、次のように w ' が計算されます。

`w' = –z / depth + 1`

これにより、Z の値が0未満の場合 (概念的には XY 平面の背後)、x 座標と Y 座標が Z の正の値になるようになります。Z 座標がと等しい場合 `depth` 、w ' は0になり、座標は無限になります。 3次元グラフィックスシステムは、カメラの比喩を中心に構築されてい `depth` ます。この値は、座標系の原点からのカメラの距離を表します。 原点からの単位である Z 座標がグラフィックオブジェクトにある場合、そのオブジェクトは、 `depth` 概念的にカメラのレンズに接し、無限に大きくなります。

この値は、回転行列と組み合わせて使用することをお勧めし `perspectiveMatrix` ます。 回転されているグラフィックスオブジェクトの X 座標と Y 座標の値がを超えている場合、 `depth` 3d 空間でのこのオブジェクトの回転には、より大きい Z 座標が関係している可能性があり `depth` ます。 これは避ける必要があります。 を作成するときに、 `perspectiveMatrix` `depth` グラフィックスオブジェクトのすべての座標に対して、回転方法に関係なく、すべての座標に対して十分に大きい値を設定します。 これにより、ゼロによる除算が行われることはありません。

3D 回転と奥行を組み合わせるには、4×4の行列を乗算する必要があります。 このために、は `SKMatrix44` 連結メソッドを定義します。 `A`と `B` がオブジェクトの場合 `SKMatrix44` 、次のコードでは、を× B と同じに設定します。

```csharp
A.PostConcat(B);
```

4×4の変換行列が2D グラフィックスシステムで使用されている場合は、2D オブジェクトに適用されます。 これらのオブジェクトはフラットであり、Z 座標が0であると見なされます。 変換乗算は、前に示した変換よりも少し単純です。

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Z の値が0の場合、マトリックスの3番目の行のセルを含まない変換式が生成されます。

x ' = M11 · x + M21 · y + M41

y ' = M12 · x + M22 · y + M42

z ' = M13 · x + M23 · y + M43

w ' = M14 · x + M24 · y + M44

さらに、z 座標もここでは意味がありません。 3D オブジェクトが2D グラフィックスシステムに表示される場合、Z 座標の値を無視することで、3D オブジェクトが2次元のオブジェクトに折りたたまれます。 変換式は、実際には次の2つにすぎません。

`x" = x' / w'`

`y" = y' / w'`

これは、4×4行列の3番目の行*と*3 番目の列を無視できることを意味します。

ただし、そのような場合は、最初に4×4のマトリックスが必要なのはなぜですか。

4×4の3番目の行と3番目の列は2次元の変換には関係あり*ませ*んが、3番目の行と列は、さまざまな値が乗算される前にロールを果たし `SKMatrix44` ます。 たとえば、Y 軸を中心とした回転を透視線変換で乗算するとします。

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

製品では、セルに `M14` perspective 値が含まれるようになりました。 このマトリックスを2D オブジェクトに適用する場合、3番目の行列に変換するために3番目の行と列が削除されます。

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

次に、2D ポイントを変換するために使用できるようになりました。

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

変換式は次のとおりです。

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

ここで、すべてを z ' で除算します。

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

Y 軸を中心に正の角度を付けて2D オブジェクトを回転させると、正の x 値は recede の背景になり、負の X 値は前景になります。 X 値は、y 軸から遠い方の座標が (余弦値によって制御される) Y 軸の近くに移動したように見えます。これは、ビューアーからさらに上に移動したり、ビューアーに近づけたりすると、y 軸の座標が小さくなるか大きくなります。

を使用する場合は `SKMatrix44` 、さまざまな値を乗算することによって、すべての3d 回転操作とパースペクティブ演算を実行し `SKMatrix44` ます。 次に、クラスのプロパティを使用して、4×4行列から2次元の3×3行列を抽出でき [`Matrix`](xref:SkiaSharp.SKMatrix44.Matrix) `SKMatrix44` ます。 このプロパティは、なじみのある値を返し `SKMatrix` ます。

**回転 3d**ページでは、3d 回転を試すことができます。 [**Rotation3DPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml)ファイルは、X 軸、Y 軸、Z 軸の周りの回転を設定し、深さの値を設定する4つのスライダーをインスタンス化します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`depthSlider`が250の値で初期化されていることに注意して `Minimum` ください。 これは、ここで回転される2D オブジェクトの X 座標と Y 座標が、原点を中心として250ピクセル半径で定義された円に制限されていることを意味します。 3D 空間でこのオブジェクトを回転すると、常に250より小さい座標値になります。

[**Rotation3DPage.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs)分離コードファイルは300ピクセルのビットマップで読み込まれます。

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

3D 変換がこのビットマップの中央に配置されている場合、X 座標と Y 座標の範囲は– 150 ~ 150 です。一方、角は中央から212ピクセルであるため、すべてが250ピクセル半径内にあります。

`PaintSurface`ハンドラーは、 `SKMatrix44` スライダーに基づいてオブジェクトを作成し、を使用してそれらを乗算し `PostConcat` ます。 `SKMatrix`最後のオブジェクトから抽出 `SKMatrix44` された値は、画面の中央に回転を中心とする平行移動変換によって囲まれています。

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

4番目のスライダーを使用して実験すると、異なる深度設定によってオブジェクトがビューアーからさらに移動されるのではなく、パースペクティブ効果の範囲が変更されることがわかります。

[![回転3D ページのトリプルスクリーンショット](3d-rotation-images/rotation3d-small.png)](3d-rotation-images/rotation3d-large.png#lightbox "回転3D ページのトリプルスクリーンショット")

**アニメーションの回転 3d**では、を使用して、 `SKMatrix44` 3d 空間のテキスト文字列をアニメーション化することもできます。 `textPaint`フィールドとして設定されたオブジェクトは、テキストの境界を決定するためにコンストラクターで使用されます。

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing`オーバーライドは Xamarin.Forms `Animation` `xRotationDegrees` 、、 `yRotationDegrees` 、およびの `zRotationDegrees` 各フィールドを異なるレートでアニメーション化するために、3つのオブジェクトを定義します。 これらのアニメーションの期間は、素数 (5 秒、7秒、および11秒) に設定されているため、全体的な組み合わせは385秒ごと、または10分以上繰り返されます。

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

前のプログラムと同様に、 `PaintCanvas` ハンドラーは `SKMatrix44` 回転とパースペクティブの値を作成し、それらを乗算します。

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

この3D 回転は、回転の中心を画面の中央に移動し、テキスト文字列のサイズを拡大して画面と同じ幅にするために、いくつかの2D 変換で囲まれています。

[![アニメーション化された回転3D ページのトリプルスクリーンショット](3d-rotation-images/animatedrotation3d-small.png)](3d-rotation-images/animatedrotation3d-large.png#lightbox "アニメーション化された回転3D ページのトリプルスクリーンショット")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
