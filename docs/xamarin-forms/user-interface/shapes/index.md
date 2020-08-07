---
title: Xamarin.Forms図形
description: Xamarin.Forms図形は、画面に図形を描画できるようにするビューの種類です。
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6a0771ac0dbbbc89301aeca3812c3b49e14655a2
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918464"
---
# <a name="no-locxamarinforms-shapes"></a>Xamarin.Forms図形

![プレリリース API](~/media/shared/preview.png)

は、 `Shape` [`View`](xref:Xamarin.Forms.View) 画面に図形を描画できるようにするの型です。 `Shape`オブジェクトは、クラスがクラスから派生するため、レイアウトクラスやほとんどのコントロール内で使用でき `Shape` `View` ます。

Xamarin.Forms図形は、 `Xamarin.Forms.Shapes` iOS、Android、macOS、ユニバーサル Windows プラットフォーム (UWP)、および Windows Presentation Foundation (WPF) の名前空間で使用できます。

> [!IMPORTANT]
> Xamarin.Forms図形は現在試験段階であり、フラグを設定することによってのみ使用でき `Shapes_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

`Shape` は次の特性を定義します。

- `Aspect`型のは、 `Stretch` 割り当てられた領域を形状がどのように塗りつぶすかを記述します。 このプロパティの既定値は `Stretch.None` です。
- `Fill`型のは、 `Brush` 図形の内部を描画するために使用されるブラシを示します。
- `Stroke`型のは、 `Brush` 図形の輪郭の描画に使用するブラシを示します。
- `StrokeDashArray`型の。 `DoubleCollection` これ `double` は、形状のアウトラインに使用される破線のパターンを示す値のコレクションを表します。
- `StrokeDashOffset`型のは、ダッシュ `double` を開始する破線パターン内の距離を指定します。 このプロパティの既定値は0.0 です。
- `StrokeLineCap`型のは、 `PenLineCap` 直線またはセグメントの開始位置と終了位置にある図形を記述します。 このプロパティの既定値は `PenLineCap.Flat` です。
- `StrokeLineJoin`型のは、 `PenLineJoin` 図形の頂点で使用される結合の種類を指定します。 このプロパティの既定値は `PenLineJoin.Miter` です。
- `StrokeMiterLimit`型のは、 `double` 図形の半分に対するマイタの長さの比率の制限を指定し `StrokeThickness` ます。 このプロパティの既定値は10.0 です。
- `StrokeThickness`型のは、 `double` 図形のアウトラインの幅を示します。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

Xamarin.Formsクラスから派生するオブジェクトの数を定義 `Shape` します。 これらは、、、、、 `Ellipse` `Line` `Path` `Polygon` `Polyline` 、および `Rectangle` です。

## <a name="paint-shapes"></a>描画図形

`Brush`オブジェクトは、図形のおよびを描画するために使用され `Stroke` `Fill` ます。

```xaml
<Ellipse Fill="DarkBlue"
         Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

この例では、のストロークと塗りつぶし `Ellipse` が指定されています。

![描画図形](images/ellipse.png "描画図形")

> [!IMPORTANT]
> `Brush`オブジェクトは、プロパティに対して値を指定できるようにする型コンバーターを使用し [`Color`](xref:Xamarin.Forms.Color) `Stroke` ます。

にオブジェクトを指定しない場合、 `Brush` `Stroke` またはを0に設定した場合、 `StrokeThickness` 図形の周りの境界線は描画されません。

オブジェクトの詳細について `Brush` は、「 [ Xamarin.Forms ブラシ](~/xamarin-forms/user-interface/brushes/index.md)」を参照してください。 有効な値の詳細について [`Color`](xref:Xamarin.Forms.Color) は、「[の Xamarin.Forms 色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="stretch-shapes"></a>拡張図形

`Shape`オブジェクトには `Aspect` 、型のプロパティがあり `Stretch` ます。 このプロパティは、オブジェクトの `Shape` レイアウト空間を埋めるために、オブジェクトの内容をどのように拡大するかを決定し `Shape` ます。 `Shape`オブジェクトのレイアウト空間は、が `Shape` レイアウトシステムによって割り当てられる領域の量です Xamarin.Forms 。これは、明示的な `WidthRequest` 設定と設定、 `HeightRequest` またはとの設定が原因 `HorizontalOptions` `VerticalOptions` であるためです。

`Stretch` 列挙体を使って、次のメンバーを定義できます。

- `None`。コンテンツの元のサイズが保持されることを示します。 これは、`Shape.Aspect` プロパティの既定値です。
- `Fill`。コピー先のディメンションを埋めるようにコンテンツのサイズが変更されることを示します。 縦横比は維持されません。
- `Uniform`。縦横比を維持したまま、コピー先のディメンションに合わせてコンテンツのサイズが変更されることを示します。
- `UniformToFill`は、縦横比を維持したまま、コピー先のディメンションを埋めるようにコンテンツのサイズを変更することを示します。 ソース コンテンツの縦横比が対象の四角形の縦横比と異なる場合は、ソース コンテンツが対象の四角形に収まるように切り取られます。

次の XAML は、プロパティを設定する方法を示してい `Aspect` ます。

```xaml
<Path Aspect="Uniform"
      Stroke="Yellow"
      StrokeThickness="1"
      Fill="Red"
      BackgroundColor="LightGray"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <!-- Path data goes here -->
    </Path.Data>  
</Path>      
```

この例では、 `Path` オブジェクトはハートを描画します。 `Path`オブジェクトの `WidthRequest` プロパティとプロパティは、 `HeightRequest` デバイスに依存しない100に設定され、その `Aspect` プロパティはに設定され `Uniform` ます。 その結果、オブジェクトの内容は、縦横比を維持したまま、変換先のディメンションに合わせてサイズが変更されます。

![拡張図形](images/aspect.png "拡張図形")

## <a name="draw-dashed-shapes"></a>破線の図形を描画する

`Shape`オブジェクトには `StrokeDashArray` 、型のプロパティがあり `DoubleCollection` ます。 このプロパティは、図形の `double` アウトラインに使用される破線のパターンを示す値のコレクションを表します。 `DoubleCollection`は、 `ObservableCollection` 値のです `double` 。 `double`コレクション内の各は、ダッシュまたはギャップの長さを指定します。 コレクション内の最初の項目 (インデックス0の位置) は、ダッシュの長さを指定します。 コレクション内の2番目の項目 (インデックス 1) には、ギャップの長さを指定します。 そのため、偶数のインデックス値を持つオブジェクトは、ダッシュを指定しますが、奇数のインデックス値を持つオブジェクトは、ギャップを指定します。

`Shape`オブジェクトには、ダッシュを `StrokeDashOffset` `double` 開始する破線パターン内の距離を指定する型のプロパティもあります。 このプロパティを設定しなかった場合、には実線のアウトラインが表示され `Shape` ます。

破線の図形を描画するには、プロパティとプロパティの両方を設定し `StrokeDashArray` `StrokeDashOffset` ます。 プロパティは1つ以上の `StrokeDashArray` 値に設定する必要があり `double` ます。各ペアは、1つのコンマと1つ以上の空白で区切られます。 たとえば、"0.5 1.0" と "0.5, 1.0" は両方とも有効です。

次の XAML の例は、破線の四角形を描画する方法を示しています。

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           StrokeDashArray="1,1"
           StrokeDashOffset="6"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

この例では、塗りつぶされた四角形が破線ストロークで描画されます。

![破線の四角形](images/dashed-rectangle.png "破線")

## <a name="control-line-ends"></a>制御線の終了

直線には、開始キャップ、線本文、および終了キャップの3つの部分があります。 先頭と末尾のキャップは、直線またはセグメントの開始位置と終了位置にある図形を記述します。

`Shape`オブジェクトには、 `StrokeLineCap` `PenLineCap` 行またはセグメントの開始位置と終了位置にある図形を記述する型のプロパティがあります。 `PenLineCap` 列挙体を使って、次のメンバーを定義できます。

- `Flat`。行の最後の点を越えて拡張されないキャップを表します。 これは、行キャップなしに相当し、プロパティの既定値です `StrokeLineCap` 。
- `Square`。高さが線の太さに等しく、長さが線の太さの半分に等しい四角形を表します。
- `Round`。直径が線の太さに等しい半円を表します。

> [!IMPORTANT]
> `StrokeLineCap`開始点または終了点のない図形にプロパティを設定した場合、このプロパティは効果がありません。 たとえば、 `Ellipse` 、、またはに設定した場合、このプロパティは効果がありません `Rectangle` 。

次の XAML は、プロパティを設定する方法を示してい `StrokeLineCap` ます。

```xaml
<Line X1="0"
      Y1="20"
      X2="300"
      Y2="20"
      StrokeLineCap="Round"
      Stroke="Red"
      StrokeThickness="12" />
```

この例では、行の先頭と末尾で赤色の線が丸められます。

![ラインキャップ](images/linecap.png "ラインキャップ")

## <a name="control-line-joins"></a>制御線の結合

`Shape`オブジェクトには、 `StrokeLineJoin` `PenLineJoin` 図形の頂点で使用される結合の種類を指定する型のプロパティがあります。 `PenLineJoin` 列挙体を使って、次のメンバーを定義できます。

- `Miter`。通常の角度の頂点を表します。 これは、`StrokeLineJoin` プロパティの既定値です。
- `Bevel`。傾斜した頂点を表します。
- `Round`。丸い頂点を表します。

> [!NOTE]
> プロパティがに設定されている場合、プロパティをに設定して、 `StrokeLineJoin` `Miter` `StrokeMiterLimit` `double` 図形内の直線結合のマイタ長を制限できます。

次の XAML は、プロパティを設定する方法を示してい `StrokeLineJoin` ます。

```xaml
<Polyline Points="20 20,250 50,20 120"
          Stroke="DarkBlue"
          StrokeThickness="20"
          StrokeLineJoin="Round" />
```

この例では、ダークブルーポリラインが頂点で角を丸く丸めています。

![線の結合](images/linejoin.png "線の結合")

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Formsブラシ](~/xamarin-forms/user-interface/brushes/index.md)
- [色Xamarin.Forms](~/xamarin-forms/user-interface/colors.md)
