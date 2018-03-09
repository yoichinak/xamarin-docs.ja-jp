---
title: "3D 回転"
description: "非アフィン変換を使用して、3 D 空間に 2 D のオブジェクトを回転します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 1341cde32778358fbeb7b65045616d5d81623d37
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="3d-rotations"></a>3D 回転

_非アフィン変換を使用して、3 D 空間に 2 D のオブジェクトを回転します。_

非アフィン変換の 1 つの一般的なアプリケーションでは、3D スペースで 2D オブジェクトの回転をシミュレートします。

![](3d-rotation-images/3drotationsexample.png "3D スペースで回転したテキスト文字列")

このジョブは、3 次元の回転で作業し、派生し、非アフィン`SKMatrix`これらの 3D 回転を実行する変換です。

これを開発することは困難`SKMatrix`トランス フォームの 2 つのディメンション内でのみ動作します。 この 3-3 でマトリックスは、3 D グラフィックスで使用される 4-4 でマトリックスから派生した場合、ジョブがはるかに簡単になります。 SkiaSharp が含まれています、 [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/)が、この目的で、3 D グラフィックスの背景を少しのクラスは 3D 回転と 4-4 での変換行列を理解するために必要です。

3 次元座標系が概念的には z までと呼ばれる 3 番目の軸を追加し、Z 軸は直角の画面に、します。 3D スペースで座標の点は 3 つの数値で示されます。 (x、y、z)。 3D で x 軸の値を増やすと、この記事で使用される座標系は右側があり、Y の増加する値が 2 つのディメンションと同様、移動します。 正の値の増加する Z 値は、画面から復帰します。 原点は、左上隅の 2 次元グラフィックと同様です。 この平面に直角に Z 軸を持つ、XY 平面として画面の考えることができます。

これは、左側の座標系と呼ばれます。 ポイントする場合 (右) に正の X の方向で、左側の人差し指を調整し、増加する Y の方向に中央の指の座標 (ダウン)、Z 座標 &#x2014; を増加させる方向で、thumb ポイントし、画面からを拡張します。

3D グラフィックで変換は、4-4 でマトリックスに基づいています。 4-4 を単位行列を次に示します。

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4-4 でマトリックスを使用して、その行および列番号を含むセルを識別すると便利です。

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

ただし、SkiaSharp`Matrix44`クラスが若干異なります。 個々 のセル値を取得または設定する唯一の方法`SKMatrix44`を使用して、 [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/)インデクサーです。 行と列のインデックスは 0 から始まるではなく 1 から始まるし、行と列をスワップします。 セル M14 上の図は、インデクサーを使用してアクセス`[3, 0]`で、`SKMatrix44`オブジェクト。

システムでは、3 D グラフィックス、4-4 での変換行列を乗算することの 1 ~ 4 でマトリックスを (x、y、z)、3 D のポイントが変換されます。

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

2 D に類似は、3 つのディメンションでその行わを変換、3D 変換 4 つのディメンションで行われると見なされます。 4 番目の次元し、呼ばれ W、3 D 空間が W 座標が 1 に等しい 4 D 領域内に存在すると見なされます。 変換の数式は次のとおりです。

x' M11·x + M21·y + M31·z + M41 を =

y' M12·x + M22·y + M32·z + M42 を =

z' M13·x + M23·y + M33·z + M43 を =

w' M14·x + M24·y + M34·z + M44 を =

変換数式から明らかであることをセル`M11`、 `M22`、 `M33` X、Y、Z の方向にスケール係数と`M41`、 `M42`、および`M43`X、Y、Z の平行移動要素方向。

これらの座標を W が 1、x を等しい 3 D 空間に変換する '、y'、z' 座標は、すべて割った w'。

x"= x'/w '

y" = y' / w'

z"z ='/w '

w" = w' / w' = 1

その除算 w' 3D スペースでパースペクティブを提供します。 場合 w' が 1 と等しい、パースペクティブは行われません。

最も単純な回転は、X、Y、Z 軸の周りは 3D 空間での回転を非常に複雑なことができます。 角度 α X 軸の周りの回転は、このマトリックスを示します。

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X 軸の値は、この変換の実行対象と同じになります。 Y 軸の周りの回転のまま変更せず Y の値。

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z 軸の周りの回転は、2 D グラフィックスの場合と同様です。

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

回転方向は、座標系のき手によって暗黙的に指定します。 左ききシステムでは、これは、左側にある特定の軸 &#x2014; の値を増やすに向かってのつまみをポイントするためX 軸の周りの回転の右側に Z 軸 &#x2014; 周りの回転角度の Y 軸を中心と手前の回転の下矢印その他の指の曲線は、回転の角度に正の値の方向を示します。

`SKMatrix44` 静的が汎用された[ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/)と[ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/)を対象となる、回転が発生した軸を指定できるようにするメソッド。

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X 軸の周りの回転の最初の 3 つの引数を 1, 0, 0 に設定します。 Y 軸の周りの回転の 0、1、0 に設定し、Z 軸の周りの回転の 0, 0, 1 に設定します。

4、4 の 4 列目は、パースペクティブです。 `SKMatrix44`が存在せず、パースペクティブ変換を作成するメソッドを次のコードを使用して自分で 1 つを作成することができます。

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

引数の名前の理由`depth`間もなく明らかになります。 そのコードは、マトリックスを作成します。

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

変換の数式の結果 w の次の計算に '。

w' – z =/深さは + 1

これは、X 座標と Y 座標の削減 Z の値が 0 未満 (概念的には、XY 平面) の後ろにし、Z の正の値 X と Y 座標を向上させるのには機能します。Z 座標と等しい`depth`w、' 0 の場合は、座標が無限になります。 3 次元グラフィック システムは、カメラの比喩、に基づいて構築されると、`depth`ここに値が、座標系の原点からカメラの距離を表します。 グラフィカルなオブジェクトが調整されている Z を持つかどうか`depth`ユニット、原点からカメラのレンズを変更することは、概念的が無限に大きくなるとします。

使用することがおそらくこのことに留意してください`perspectiveMatrix`回転行列と組み合わせて値。 回転されているグラフィック オブジェクトの X または Y の座標よりも大きい場合`depth`を 3D 空間でこのオブジェクトの回転角度は Z の座標よりも大きいが関与すること、`depth`です。 これを回避する必要があります。 作成するときに`perspectiveMatrix`を設定する`depth`回転方法に関係なく、グラフィックス オブジェクト内のすべての座標に十分な大きさの値にします。 これにより、0 による除算はありません。

3D 回転とパースペクティブの組み合わせには、4-4、行列の乗算が必要です。 このため、`SKMatrix44`連結メソッドを定義します。 場合`A`と`B`は`SKMatrix44`オブジェクトを次のコードが × b: を等しいかどうかを設定し、

```csharp
A.PostConcat(B);
```

2 D グラフィックス システムで 4-4 での変換行列を使用するときに、2 D のオブジェクトに適用されます。 これらのオブジェクトは、フラットな 0 の Z 座標にあると見なされます。 トランス フォーム乗算では、少し前に示した変換より簡素化です。

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

マトリックスの 3 番目の行のセルがない変換の数式で z 結果を得るには、0 の値がその:

x' M11·x + M21·y M41 を =

y' M12·x + M22·y M42 を =

z' M13·x + M23·y M43 を =

w' M14·x + M24·y M44 を =

さらに、z' 座標は使用されませんこちらもします。 2 D グラフィックス システムでは、3 D のオブジェクトが表示されたら、Z 座標値を無視することで、2 次元のオブジェクトに折りたたまれています。 変換式が実際にはこれらの 2 つあります。

x"= x'/w '

y" = y' / w'

つまり、3 番目の行*と*4-4 でマトリックスの 3 番目の列を無視することができます。

かどうかは、理由が、最初の場所でも必要な 4-4 でマトリックスですか。

3 行目と 4 で 4 の 3 番目の列が関連する 2 次元の変換は、3 番目の行と列ではありませんが*は*時にする前に役割を果たすさまざまな`SKMatrix44`値が乗算されます。 たとえば、パースペクティブ変換を使用して、Y 軸の周りの回転を乗算するとします。

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

製品、セルに`M14`パースペクティブ値が含まれています。 2D オブジェクトにその行列を適用する場合は、3 番目の行と列が 3-3 でマトリックスに変換する除外されます。

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

今すぐ 2D ポイントの変換を使用できます。

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

変換の数式は次のとおりです。

x' = cos (α) ·x

y' = y

z' = = (sin (α)/深度) ·x + 1

今すぐ除算すべて z'。

x" = cos(α)·x / ((sin(α)/depth)·x + 1)

y" = y / ((sin(α)/depth)·x + 1)

X の値が負の値の中にバック グラウンド遠ざかります 2D オブジェクトは、Y 軸の周り、正の値は正の角度に回転したときに、前景色を取得する X 値。 X 値に見える遠い Y 軸座標と Y 軸 (を余弦の値によって制御されます) に近い場所に移動するより小さなまたはビューアーから移動するには大きなまたはビューアーに近い方になります。

使用する場合`SKMatrix44`、さまざまなを乗算することによって、すべての 3D 回転とパースペクティブの操作を実行`SKMatrix44`値。 4、4 から 3-3 で 2 次元行列を抽出することができますし、マトリックスを使用して、 [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/)のプロパティ、`SKMatrix44`クラスです。 このプロパティを返します使い慣れた`SKMatrix`値。

**回転の 3D**ページの 3D 回転を試すことができます。 [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml)ファイルには、X、Y、および Z 軸の周りの回転を設定して、深さの値を設定する 4 つのスライダーがインスタンス化します。

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

注意して、`depthSlider`による初期化を行う、 `Minimum` 250 の値。 これは、ここで回転されている 2D オブジェクトに原点の周囲の 250 ピクセルの半径によって定義される円に限定される X と Y 座標が含まれているを意味します。 3D スペースでは、このオブジェクトの回転は、250 未満の座標値に常になります。

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) 300 ピクセルの四角形をビットマップに分離コード ファイルが読み込まれます。

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

3D 変換で、このビットマップの中心は場合、X および Y 座標 –150 ~ 150、範囲角ため 250 ピクセル radius ではすべて、センターから 212 ピクセルされているときにします。

`PaintSurface`ハンドラーを作成`SKMatrix44`オブジェクトは、スライダーに基づいておりを使用して乗算`PostConcat`です。 `SKMatrix`最後から抽出された値`SKMatrix44`オブジェクトの周囲で変換するため、画面の中央の回転の中心の変換。

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

4 番目のスライダーに試行するときにわかります深さが異なる設定が、ビューアーから離れた場所でさらに、オブジェクトを移動しないが、パースペクティブの影響の程度を代わりに alter:

[![](3d-rotation-images/rotation3d-small.png "回転の 3D ページのスクリーン ショットをトリプル")](3d-rotation-images/rotation3d-large.png "回転の 3D ページのトリプル スクリーン ショット")

**アニメーションの回転の 3D**も使用`SKMatrix44`3 D 空間内のテキスト文字列をアニメーション化します。 `textPaint`オブジェクト セットのフィールドは、テキストの範囲を決定するコンス トラクターで使用されるものとします。

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

`OnAppearing`上書き定義の 3 つの Xamarin.Forms`Animation`アニメーション化するオブジェクト、 `xRotationDegrees`、 `yRotationDegrees`、および`zRotationDegrees`異なるレートでフィールドです。 これらのアニメーションの期間が含まれる素数 &#x2014; に設定されていることを確認します。5 秒、7 秒、および 11 秒 & #x 2014 です。全体的な組み合わせは、すべて 385 秒、または 10 分以上にのみ繰り返されます。

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

前のプログラムと同様に、`PaintCanvas`ハンドラーを作成`SKMatrix44`回転および観点の値し、を乗算しています。

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

この 3D 回転は、いくつかの 2D 変換を回転の中心を画面の中央に移動し、画面と同じ幅になるように、テキスト文字列のサイズを拡大または縮小をで囲まれます。

[![](3d-rotation-images/animatedrotation3d-small.png "アニメーションの回転の 3D ページのスクリーン ショットをトリプル")](3d-rotation-images/animatedrotation3d-large.png "アニメーションの回転の 3D ページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
