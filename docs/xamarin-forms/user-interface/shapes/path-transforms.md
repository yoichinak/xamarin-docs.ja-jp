---
title: 'Xamarin.Forms図形: パスの変換'
description: 変換は、 Xamarin.Forms パスオブジェクトをある座標空間から別の座標空間に変換する方法を定義します。
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 41de95c452212dce77d6365265e4813170c9b9b9
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853042"
---
# <a name="xamarinforms-shapes-path-transforms"></a>Xamarin.Forms図形: パスの変換

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

は、ある `Transform` `Path` 座標空間から別の座標空間にオブジェクトを変換する方法を定義します。 変換がオブジェクトに適用されると `Path` 、オブジェクトが UI でどのようにレンダリングされるかが変更されます。

変換は、回転、拡大縮小、傾斜、および平行移動という4つの一般的な分類に分類できます。 Xamarin.Formsは、次の各変換分類のクラスを定義します。

- `RotateTransform``Path`。指定したでを回転させ `Angle` ます。
- `ScaleTransform``Path`。指定された量だけオブジェクトをスケーリングし `ScaleX` `ScaleY` ます。
- `SkewTransform``Path`。指定された量だけオブジェクトを傾斜させ `AngleX` `AngleY` ます。
- `TranslateTransform``Path`。指定された量だけオブジェクトを移動し `X` `Y` ます。

Xamarin.Formsには、より複雑な変換を作成するための次のクラスも用意されています。

- `TransformGroup`。複数の変換オブジェクトで構成される複合変換を表します。
- `CompositeTransform`。オブジェクトに複数の変換操作を適用 `Path` します。
- `MatrixTransform`。他の変換クラスによって提供されないカスタム変換を作成します。

これらのクラスはすべて `Transform` 、型のプロパティを定義するクラスから派生 `Value` `Matrix` します。 このプロパティは、現在の変換をオブジェクトとして表し `Matrix` ます。 構造体の詳細については `Matrix` 、「 [Transform matrix](#transform-matrix)」を参照してください。

に変換を適用するには `Path` 、変換クラスを作成し、プロパティの値として設定し `Path.RenderTransform` ます。

## <a name="rotation-transform"></a>回転変換

回転変換は、 `Path` 2d x-y 座標系の指定した点について、オブジェクトを時計回りに回転させます。

クラス `RotateTransform` から派生したクラスは、 `Transform` 次のプロパティを定義します。

- `Angle`型のは、 `double` 時計回りの回転の角度 (度単位) を表します。 このプロパティの既定値は0.0 です。
- `CenterX`型のは、 `double` 回転中心点の x 座標を表します。 このプロパティの既定値は0.0 です。
- `CenterY`型のは、 `double` 回転の中心点の y 座標を表します。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

`CenterX`プロパティと `CenterY` プロパティは、オブジェクトを回転させるポイントを指定し `Path` ます。 この中心点は、変換されたオブジェクトの座標空間で表されます。 既定では、回転は (0, 0) に適用されます。これはオブジェクトの左上隅です `Path` 。

次の例は、オブジェクトを回転させる方法を示してい `Path` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <RotateTransform CenterX="0"
                         CenterY="0"
                         Angle="45" />
    </Path.RenderTransform>
</Path>
```

この例では、 `Path` オブジェクトが左上隅に45度回転しています。

## <a name="scale-transform"></a>スケール変換

スケール変換は、 `Path` 2d x-y 座標系のオブジェクトをスケーリングします。

クラス `ScaleTransform` から派生したクラスは、 `Transform` 次のプロパティを定義します。

- `ScaleX``double`x 軸のスケールファクターを表す型の。 このプロパティの既定値は1.0 です。
- `ScaleY``double`y 軸のスケールファクターを表す型の。 このプロパティの既定値は1.0 です。
- `CenterX``double`この変換の中心点の x 座標を表す型の。 このプロパティの既定値は0.0 です。
- `CenterY``double`この変換の中心点の y 座標を表す型の。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

との値は、 `ScaleX` `ScaleY` 結果のスケーリングに大きな影響を与えます。

- 0 ~ 1 の値は、スケーリングされたオブジェクトの幅と高さを減らします。
- 1より大きい値を指定すると、スケーリングされたオブジェクトの幅と高さが大きくなります。
- 値1は、オブジェクトがスケーリングされていないことを示します。
- 負の値を指定すると、スケールオブジェクトが水平方向および垂直方向に反転されます。
- 0から-1 の間の値は、scale オブジェクトを反転し、幅と高さを小さくします。
- -1 未満の値を指定すると、オブジェクトが反転され、幅と高さが大きくなります。
- -1 の値は、スケーリングされたオブジェクトを反転しますが、水平方向または垂直方向のサイズを変更しません。

`CenterX`プロパティと `CenterY` プロパティは、オブジェクトをスケーリングするポイントを指定し `Path` ます。 この中心点は、変換されたオブジェクトの座標空間で表されます。 既定では、スケーリングは、オブジェクトの左上隅である (0, 0) に適用され `Path` ます。 これには、オブジェクトを移動してより大きな表示にする効果があり `Path` ます。変換を適用すると、オブジェクトが存在する座標空間が変更されるため `Path` です。

次の例は、オブジェクトをスケーリングする方法を示してい `Path` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <ScaleTransform CenterX="0"
                        CenterY="0"
                        ScaleX="1.5"
                        ScaleY="1.5" />
    </Path.RenderTransform>
</Path>
```

この例では、 `Path` オブジェクトはサイズの1.5 倍にスケーリングされます。

## <a name="skew-transform"></a>傾斜変換

傾斜変換は、 `Path` 2d x-y 座標系のオブジェクトを傾斜させます。これは、2d オブジェクトで3d 奥行の錯覚を作成する場合に便利です。

クラス `SkewTransform` から派生したクラスは、 `Transform` 次のプロパティを定義します。

- `AngleX``double`y 軸から反時計回りに計測した x 軸の傾斜角度を表す、型のです。 このプロパティの既定値は0.0 です。
- `AngleY``double`y 軸の傾斜角度を表す、型の。 x 軸から反時計回りに計測します。 このプロパティの既定値は0.0 です。
- `CenterX``double`変換の中心の x 座標を表す型の。 このプロパティの既定値は0.0 です。
- `CenterY``double`変換の中心の y 座標を表す型の。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

傾斜変換の効果を予測する際は、`AngleX` によって元の座標系に対して x 軸の値が傾斜することを考慮します。 したがって、が `AngleX` 30 の場合、y 軸は原点を介して30°回転し、原点から30°回転して値を傾斜させます。 同様に、 `AngleY` 30 のは、オブジェクトの y 値を `Path` 原点から30°傾斜させます。

> [!NOTE]
> オブジェクトを適切に傾斜させるには、 `Path` `CenterX` `CenterY` オブジェクトの中心点にプロパティとプロパティを設定します。

次の例は、オブジェクトを傾斜させる方法を示してい `Path` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <SkewTransform CenterX="0"
                       CenterY="0"
                       AngleX="45"
                       AngleY="0" />
    </Path.RenderTransform>
</Path>
```

この例では、水平方向の傾斜が45°の `Path` 中心点 (0, 0) からオブジェクトに適用されています。

## <a name="translate-transform"></a>変換変換

変換変換は、2D x-y 座標系のオブジェクトを移動します。

クラス `TranslateTransform` から派生したクラスは、 `Transform` 次のプロパティを定義します。

- `X``double`x 軸に沿って移動する距離を表す型の。 このプロパティの既定値は0.0 です。
- `Y``double`y 軸に沿って移動する距離を表す型の。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

負 `X` の値はオブジェクトを左に移動し、正の値はオブジェクトを右に移動します。 負 `Y` の値はオブジェクトを上に移動し、正の値はオブジェクトを下へ移動します。

次の例は、オブジェクトを変換する方法を示してい `Path` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TranslateTransform X="50"
                            Y="50" />
    </Path.RenderTransform>
</Path>
```

この例では、 `Path` オブジェクトは、デバイスに依存しないユニット50を右に移動し、50デバイスに依存しないユニットを停止します。

## <a name="multiple-transforms"></a>複数の変換

Xamarin.Formsには、オブジェクトへの複数の変換の適用をサポートする2つのクラスがあり `Path` ます。 これらは `TransformGroup` 、、および `CompositeTransform` です。 は `TransformGroup` 任意の順序で変換を実行し、は `CompositeTransform` 特定の順序で変換を実行します。

### <a name="transform-groups"></a>変換グループ

変換グループは、複数のオブジェクトで構成される複合変換を表し `Transform` ます。

クラス `TransformGroup` から派生したクラスは、 `Transform` `Children` オブジェクトのコレクションを表す型のプロパティを定義し `TransformCollection` `Transform` ます。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

クラスを使用する複合変換では、変換の順序が重要になり `TransformGroup` ます。 たとえば、最初に回転した後、スケーリングしてから平行移動した場合、最初に変換してから回転してからスケールする場合とは異なる結果が得られます。 理由の1つは、回転やスケーリングなどの変換が、座標系の原点に対して実行されることです。 原点から中央に配置されているオブジェクトをスケーリングすると、元の位置から離れた位置にあるオブジェクトを拡張するための異なる結果が生成されます。 同様に、原点から中央に配置されているオブジェクトを回転すると、原点から離れた位置にあるオブジェクトを回転した場合とは異なる結果が生成されます。

次の例は、クラスを使用して複合変換を実行する方法を示してい `TransformGroup` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TransformGroup>
            <ScaleTransform ScaleX="1.5"
                            ScaleY="1.5" />
            <RotateTransform Angle="45" />
        </TransformGroup>
    </Path.RenderTransform>
</Path>
```

この例では、 `Path` オブジェクトはサイズの1.5 倍にスケーリングされ、その後、45度回転しています。

## <a name="composite-transforms"></a>複合変換

複合変換は、オブジェクトに複数の変換を適用します。

クラス `CompositeTransform` から派生したクラスは、 `Transform` 次のプロパティを定義します。

- `CenterX``double`この変換の中心点の x 座標を表す型の。 このプロパティの既定値は0.0 です。
- `CenterY``double`この変換の中心点の y 座標を表す型の。 このプロパティの既定値は0.0 です。
- `ScaleX``double`x 軸のスケールファクターを表す型の。 このプロパティの既定値は1.0 です。
- `ScaleY``double`y 軸のスケールファクターを表す型の。 このプロパティの既定値は1.0 です。
- `SkewX``double`y 軸から反時計回りに計測した x 軸の傾斜角度を表す、型のです。 このプロパティの既定値は0.0 です。
- `SkewY``double`y 軸の傾斜角度を表す、型の。 x 軸から反時計回りに計測します。 このプロパティの既定値は0.0 です。
- `Rotation`型のは、 `double` 時計回りの回転の角度 (度単位) を表します。 このプロパティの既定値は0.0 です。
- `TranslateX``double`x 軸に沿って移動する距離を表す型の。 このプロパティの既定値は0.0 です。
- `TranslateY``double`y 軸に沿って移動する距離を表す型の。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

は、次の `CompositeTransform` 順序で変換を適用します。

1. Scale ( `ScaleX` および `ScaleY` )。
1. 傾斜 ( `SkewX` と `SkewY` )。
1. Rotate ( `Rotation` )。
1. ( `TranslateX` ,) を変換 `TranslateY` します。

異なる順序でオブジェクトに複数の変換を適用する場合は、を作成し、目的の順序で変換を挿入する必要があり `TransformGroup` ます。

> [!IMPORTANT]
> では、 `CompositeTransform` すべての変換に同じ中心点とが使用さ `CenterX` `CenterY` れます。 変換ごとに異なる中心点を指定する場合は、を使用します。 `TransformGroup`

次の例は、クラスを使用して複合変換を実行する方法を示してい `CompositeTransform` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <CompositeTransform ScaleX="1.5"
                            ScaleY="1.5"
                            Rotation="45"
                            TranslateX="50"
                            TranslateY="50" />
    </Path.RenderTransform>
</Path>
```

この例では、 `Path` オブジェクトはサイズの1.5 倍にスケーリングされた後、45度回転し、デバイスに依存しない50単位で変換されます。

## <a name="transform-matrix"></a>変換行列

変換は、2D 空間で変換を実行する 3 x 3 アフィン変換行列の観点から記述できます。 この3番目の行列は、構造体によって表され `Matrix` ます。これは、3つの行と3つの値の列からなるコレクションです `double` 。

`Matrix`構造体は、次のプロパティを定義します。

- `Determinant``double`行列の行列式を取得する型の。
- `HasInverse`型の `bool` 。行列が反転できるかどうかを示します。
- `Identity`型の。 `Matrix` id 行列を取得します。
- `HasIdentity`型の。 `bool` これは、マトリックスが id 行列であるかどうかを示します。
- `M11``double`行列の最初の行と最初の列の値を表す、型の。
- `M12``double`行列の第1行、第2列の値を表す型の。
- `M21``double`行列の第2行、第1列の値を表す型の。
- `M22``double`行列の2番目の行と2番目の列の値を表す型の。
- `OffsetX``double`行列の第3行、第1列の値を表す型の。
- `OffsetY``double`行列の第3行、第2列の値を表す型の。

`OffsetX`プロパティと `OffsetY` プロパティは、それぞれ x 軸と y 軸に沿って座標空間を平行移動する量を指定するため、という名前になります。

さらに、 `Matrix` 構造体は、、、、など、マトリックスの値を操作するために使用できる一連のメソッドを公開し `Append` `Invert` `Multiply` `Prepend` ます。

次の表は、マトリックスの構造を示してい Xamarin.Forms ます。

| | | |
|---------|---------|-----|
| M11     | M12     | 0.0 |
| M21     | M22     | 0.0 |
| OffsetX | OffsetY | 1.0 |

> [!NOTE]
> アフィン変換行列の最終的な列は (0, 0, 1) と等しくなるため、最初の2つの列のメンバーのみを指定する必要があります。

マトリックスの値を操作することにより、オブジェクトを回転、拡大縮小、傾斜、および平行移動でき `Path` ます。 たとえば、値を100に変更した場合、 `OffsetX` `Path` x 軸に沿ってオブジェクト100のデバイスに依存しない単位を移動することができます。 値を3に変更した場合は、 `M22` `Path` オブジェクトを現在の高さの3倍に拡張するために使用できます。 両方の値を変更する場合は、 `Path` x 軸に沿ってオブジェクト100のデバイスに依存しない単位を移動し、その高さを3倍にします。 さらに、アフィン変換行列を乗算して、回転や傾斜などの任意の数の線形変換を形成し、その後に平行移動を行うことができます。

## <a name="custom-transforms"></a>カスタム変換

クラス `MatrixTransform` から派生したクラスは、 `Transform` `Matrix` 型のプロパティを定義します。このプロパティは `Matrix` 、変換を定義する行列を表します。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

、、、またはオブジェクトで記述できる変換は `TranslateTransform` `ScaleTransform` `RotateTransform` `SkewTransform` 、によって同じように記述でき `MatrixTransform` ます。 ただし、、 `TranslateTransform` 、 `ScaleTransform` `RotateTransform` 、およびの `SkewTransform` 各クラスは、でベクターコンポーネントを設定するよりも概念が簡単です `Matrix` 。 このため、 `MatrixTransform` クラスは、通常、、、 `RotateTransform` 、またはの各クラスによって提供されないカスタム変換を作成するために使用され `ScaleTransform` `SkewTransform` `TranslateTransform` ます。

次の例は `Path` 、を使用してオブジェクトを変換する方法を示してい `MatrixTransform` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform>
            <MatrixTransform.Matrix>
                <!-- M11 stretches, M12 skews -->
                <Matrix OffsetX="10"
                        OffsetY="100"
                        M11="1.5"
                        M12="1" />
            </MatrixTransform.Matrix>
        </MatrixTransform>
    </Path.RenderTransform>
</Path>
```

この例では、 `Path` オブジェクトは X と Y の両方の次元で拡大、傾斜、およびオフセットが使用されています。

また、これは、に組み込まれている型コンバーターを使用する簡略化された形式で記述でき Xamarin.Forms ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform Matrix="1.5,1,0,1,10,100" />
    </Path.RenderTransform>
</Path>
```

この例では、 `Matrix` プロパティは、、、、、の6つのメンバーから成るコンマ区切りの文字列として指定されます `M11` `M12` `M21` `M22` `OffsetX` `OffsetY` 。 この例ではメンバーはコンマで区切られていますが、1つまたは複数の空白で区切ることもできます。

また、前の例は、プロパティの値と同じ6つのメンバーを指定することでさらに簡略化できます `RenderTransform` 。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      RenderTransform="1.5 1 0 1 10 100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z" />
```

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
