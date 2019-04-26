---
title: SkiaSharp の 3D 回転
description: この記事では、非アフィン変換を使用して 3D 空間を 2D オブジェクトを回転する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
ms.openlocfilehash: 7ac9ec458f16357ef50e23c459a9b0e1f79bdd97
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61349034"
---
# <a name="3d-rotations-in-skiasharp"></a>SkiaSharp の 3D 回転

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_非アフィン変換を使用して、3 D 空間で 2D オブジェクトを回転させます。_

非アフィン変換の 1 つの一般的なアプリケーションが 3D 空間で 2D オブジェクトの回転をシミュレートします。

![](3d-rotation-images/3drotationsexample.png "3 次元空間で回転したテキスト文字列")

このジョブは、3 次元の回転を使用し、派生し、非アフィン`SKMatrix`これらの 3D 回転を実行する変換。

これを開発するが難しい`SKMatrix`変換の 2 つのディメンション内でのみ動作します。 この 3-3 で行列は 3D グラフィックスで使用される 4-4 で行列から派生したときに、ジョブははるかに簡単になります。 SkiaSharp が含まれています、 [ `SKMatrix44` ](xref:SkiaSharp.SKMatrix44) 、この目的は 3D グラフィックスでいくつかのバック グラウンドのクラスは 3D 回転と 4-4 での変換行列を理解するために必要です。

Z 軸の目盛りが画面に直角に交わっての 3 次元座標系に概念的には z までという 3 つ目の軸が追加されます。 3 次元空間で座標の点が 3 つの数値で示されます。 (x, y, z)。 X の値を増やすと、この記事で使用される座標系は 3D の右側にし、Y の値が、2 つのディメンションと同じように移動します。 画面から出て正の Z 値が増加します。 原点は、左上隅の 2D グラフィックと同様です。 画面は、この平面に直角に交わって Z 軸を XY 平面として考えることができます。

これは、左手座標系と呼ばれます。 X 座標 (右側) に正の方向で、左側の人差し指をポイントして、中指を Y を増加させる方向を調整 (ダウン) 場合、に、Z 座標を増加させる方向にし、つまみポイント-からを拡張します。画面。

3D グラフィックは、変換は、4-4 でのマトリックスに基づいています。 4-4 が恒等行列を次に示します。

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4-4 でマトリックスの操作、その行と列番号を持つセルを識別するために便利です。

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

ただし、SkiaSharp`Matrix44`クラスは少し異なります。 個々 のセルの値を取得または設定する唯一の方法`SKMatrix44`を使用して、 [ `Item` ](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32))インデクサーです。 行と列のインデックスは 0 から始まるではなく 1 から始まるし、行と列がスワップされます。 上の図では、セル M14 にインデクサーを使用してアクセス`[3, 0]`で、`SKMatrix44`オブジェクト。

3D グラフィックス システムで 3D の点 (x, y, z) は、4-4 での変換行列を掛けることの 1 ~ 4 をマトリックスに変換されます。

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

2D に類似の 3 つのディメンションがその実行を変換、3 D 変換 4 次元で行われると見なされます。 4 番目の次元は、W キーと呼ばれます、3 D 空間は W 座標が 1 の 4d 空間内に存在すると見なされます。 変換式は次のとおりです。

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

変換式からも明らかをセル`M11`、 `M22`、 `M33` X、Y、および Z の方向にスケーリングが要因と`M41`、`M42`と`M43`X、Y、および Z の翻訳要素方向。

これらの座標を 3D に W が 1、x と等しい領域に変換する '、y'、z' 座標はすべて割った w'。

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

その除算 w' 3D 空間でパースペクティブを提供します。 場合 w' が 1 と等しい、パースペクティブは行われません。

3D 空間での回転は非常に複雑にすることができますが、最も簡単な回転は X、Y、および Z 軸を中心。 角度 α X 軸の周りの回転角度は、このマトリックスを示します。

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X の値は、この変換の対象と同じになります。 変更なし Y の値を Y 軸の周りの回転になります。

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z 軸の周りの回転は 2D グラフィックスと同じです。

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

回転の方向がの座標システムの処理によって暗黙的に指定します。 左手のシステムでは、これはつまみの左側にある特定の軸の値の増加方向をポイントする場合、X 軸を中心とする回転の右側に Z 軸の周りの回転の回転を Y 軸を中心とするダウン-の曲線では、yoその他の指では、正の角度の回転の方向を示します。

`SKMatrix44` 静的に一般化が[ `CreateRotation` ](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single))と[ `CreateRotationDegrees` ](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single))となる、回転が行われる軸を指定するためのメソッド。

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X 軸の周りの回転の最初の 3 つの引数を 1, 0, 0 に設定します。 Y 軸の周りの回転、0、1、0 に設定し、Z 軸の周りの回転、0, 0, 1 に設定します。

第 4 列 4、4 は、パースペクティブによってです。 `SKMatrix44`パースペクティブの変換を作成するためのメソッドがありませんが、次のコードを使用して自分で 1 つを作成することができます。

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

引数名の理由`depth`すぐ明らかになります。 そのコードは、マトリックスを作成します。

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

W の次の計算で発生する変換式は、'。

`w' = –z / depth + 1`

これは、Z の値が 0 未満 (概念的にはバック XY 平面) ときに、X および Y 座標を削減し、Z の正の値の座標 X と Y 座標を増やすには機能します。Z 座標と等しい`depth`、w、' 0 の場合は、座標が無限になります。 3 次元のグラフィックス システムは、カメラのメタファに基づいて作成されます、`depth`値は、ここでは、カメラの距離を表す、座標系の原点から。 かどうか、グラフィカル オブジェクトは調整されている Z`depth`ユニット、配信元からの カメラのレンズは概念的に触れることと無制限に大きくなり、します。

注意をおそらく使用するこの`perspectiveMatrix`回転行列と組み合わせて値。 回転されているグラフィック オブジェクトに X または Y 座標のより大きい場合`depth`、3 D 空間では、このオブジェクトの回転により大きい Z 座標が関与することは`depth`します。 これを回避する必要があります。 作成するときに`perspectiveMatrix`を設定する`depth`回転方法に関係なく、グラフィックス オブジェクト内のすべての座標に十分な大きさの値にします。 これにより、ゼロによる除算はありません。

パースペクティブの 3D 回転を組み合わせることでは、まとめて 4-4 で行列を乗算することが必要です。 このため、`SKMatrix44`連結メソッドを定義します。 場合`A`と`B`は`SKMatrix44`オブジェクトの場合、次のコードは、等号を × b: に設定し、

```csharp
A.PostConcat(B);
```

4-4 での変換行列を使用すると、2 D グラフィックス システムでは、2 D のオブジェクトに適用されます。 これらのオブジェクトは、フラット 0 の Z 座標と見なされます。 変換の乗算は前に示した変換よりも簡単です。

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

0 値の変換式を行列の 3 番目の行内のどのセルを含まないで z 結果をします。

x' = M11・x + M21・y + M41

y' = M12・x + M22・y + M42

z' = M13・x + M23・y + M43

w' = M14・x + M24・y + M44

さらに、z、' 座標は関係ありませんもします。 2D グラフィックス システムで 3D オブジェクトが表示される場合は、2 次元のオブジェクトに、Z 座標の値を無視することで折りたたまれています。 変換式は、実際にはこれらの 2 つです。

`x" = x' / w'`

`y" = y' / w'`

つまり、3 番目の行*と*4-4 でマトリックスの 3 番目の列は無視できます。

かどうかですが、なぜ必要さえ 4-4 でマトリックスを最初にですか。

第 3 行、第 3 列 4、4 は 2 次元の変換、3 番目の行と列の関連する*は*時にする前に役割を果たすさまざまな`SKMatrix44`値が乗算されます。 たとえば、透視変換を Y 軸の周りの回転を乗算するとします。

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

製品にセル`M14`観点の値が含まれています。 2D オブジェクトにその行列を適用する場合は、3 番目の行と列が 3-3 でのマトリックスに変換する除外されます。

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

これで 2D ポイントの変換を使用できます。

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

変換式は次のとおりです。

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

これですべてを z' で除算 。

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

X の値が負の値の中にバック グラウンドに遠ざかります 2D オブジェクトが Y 軸を中心、正の値は正の角度で回転したときに、前景色を取得する X 値。 X の値は、遠い Y 軸の座標として (これは、コサイン値によって制御されます)、Y 軸に近づけるが小さいまたはビューアーから移動すると拡大になったり、ビューアーに近いようです。

使用する場合`SKMatrix44`、さまざまな乗算することによって、すべての 3D 回転とパースペクティブの操作を実行する`SKMatrix44`値。 4、4 から 3-3、2 次元行列を抽出することができますし、マトリックスを使用して、 [ `Matrix` ](xref:SkiaSharp.SKMatrix44.Matrix)のプロパティ、`SKMatrix44`クラス。 このプロパティは、使い慣れたを返します`SKMatrix`値。

**回転 3D**ページの 3D 回転を試すことができます。 [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml)ファイルには、X、Y、および Z 軸の周りの回転を設定して、深さの値を設定する 4 つのスライダーがインスタンス化します。

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

注意、`depthSlider`を使用して初期化、 `Minimum` 250 の値。 これは、ため、ここで回転されている 2D オブジェクトに、X および Y 座標原点の周囲の 250 ピクセルの半径によって定義される円に制限にはが含まれています。 3 次元空間では、このオブジェクトの回転は、座標の値が 250 よりも小さいか常に得られます。

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs)ビットマップが 300 ピクセルの正方形で分離コード ファイルが読み込まれます。

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

3D 変換がこのビットマップの中央に配置する場合、X および Y 座標 –150 ~ 150、範囲 250 ピクセル半径内にあるため、すべてのもの、コーナーは、中心から 212 ピクセル。

`PaintSurface`ハンドラーを作成します`SKMatrix44`オブジェクトは、スライダーに基づいており、乗算を使用して`PostConcat`します。 `SKMatrix`最後から抽出された値`SKMatrix44`オブジェクトの周囲で翻訳画面の中央に回転の中心の変換。

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

4 番目のスライダーをテストするときに、別の設定は、ビューアーから離れた場所にさらにオブジェクトを移動しないが、パースペクティブの効果の程度を代わりに alter のことを確認します。

[![](3d-rotation-images/rotation3d-small.png "回転の 3D ページのスクリーン ショットをトリプル")](3d-rotation-images/rotation3d-large.png#lightbox "回転 3D ページの 3 倍になるスクリーン ショット")

**アニメーション回転 3D**では`SKMatrix44`3 D 空間内のテキスト文字列をアニメーション化します。 `textPaint`オブジェクト コンス トラクターで、テキストの範囲の決定に使用されるためにフィールドを設定します。

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

`OnAppearing`オーバーライドは、次の 3 つの Xamarin.Forms を定義します。`Animation`アニメーション化するオブジェクト、 `xRotationDegrees`、 `yRotationDegrees`、および`zRotationDegrees`異なるレートでフィールド。 通知に事前通知するこれらのアニメーションの期間が設定されているため、全体的な組み合わせは、すべて 385 の秒数または 10 分以上にのみ繰り返されます (5 秒、7 秒、および 11 秒) の数値します。

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

前のプログラムでは、ように、`PaintCanvas`ハンドラーを作成します`SKMatrix44`の回転と見ると、値し、を乗算しています。

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

この 3D 回転は、いくつかの 2D 変換を回転の中心を画面の中央に移動し、画面と同じ幅にあるテキスト文字列のサイズを調整するで囲まれます。

[![](3d-rotation-images/animatedrotation3d-small.png "アニメーションの回転の 3D ページのスクリーン ショットをトリプル")](3d-rotation-images/animatedrotation3d-large.png#lightbox "アニメーション回転 3D ページの 3 倍になるスクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
