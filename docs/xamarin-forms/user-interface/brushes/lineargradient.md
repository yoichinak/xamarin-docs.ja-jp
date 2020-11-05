---
title: 'Xamarin.Forms ブラシ: 線状グラデーション'
description: Xamarin.FormsSystem.windows.media.lineargradientbrush> クラスは、線状グラデーションで領域を塗りつぶします。
ms.prod: xamarin
ms.assetid: BEA2B3F5-96B0-4E39-88A6-0FAFE95C3DCD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01dba502f6a561aac122e9fa44d7b257c76c5b4b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374330"
---
# <a name="no-locxamarinforms-brushes-linear-gradients"></a>Xamarin.Forms ブラシ: 線状グラデーション

![プレビュー API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`LinearGradientBrush`クラスはクラスから派生 `GradientBrush` し、線状グラデーションで領域を描画します。これにより、グラデーション軸と呼ばれる線に沿って2つ以上の色がブレンドされます。 `GradientStop` オブジェクトは、グラデーションの色とその位置を指定するために使用されます。 オブジェクトの詳細について `GradientStop` は、「 [ Xamarin.Forms ブラシ: グラデーション](gradient.md)」を参照してください。

`LinearGradientBrush` クラスでは、次のプロパティが定義されます。

- `StartPoint`[`Point`](xref:Xamarin.Forms.Point)線形グラデーションの開始2次元座標を表す型の。 このプロパティの既定値は (0, 0) です。
- `EndPoint`[`Point`](xref:Xamarin.Forms.Point)線形グラデーションの終点の2次元座標を表す型の。 このプロパティの既定値は (1, 1) です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

クラスは、 `LinearGradientBrush` ブラシに `IsEmpty` `bool` オブジェクトが割り当てられているかどうかを表すを返すメソッドとも呼ばれ `GradientStop` ます。

> [!NOTE]
> また、CSS 関数を使用して線形グラデーションを作成することもでき `linear-gradient()` ます。

## <a name="create-a-lineargradientbrush"></a>System.windows.media.lineargradientbrush> を作成する

線状グラデーションブラシのグラデーションの分岐点は、グラデーション軸に沿って配置されます。 グラデーション軸の向きとサイズは、ブラシのプロパティとプロパティを使用して変更でき `StartPoint` `EndPoint` ます。 これらのプロパティを操作することにより、水平、垂直、斜線のグラデーションを作成したり、グラデーションの方向を逆にしたり、グラデーションの拡散を縮小したりすることができます。

`StartPoint`プロパティと `EndPoint` プロパティは、描画される領域に対する相対値です。 (0, 0) は、塗りつぶされる領域の左上隅を表し、(1, 1) は塗りつぶされる領域の右下隅を表します。 次の図は、斜線線形グラデーションブラシのグラデーション軸を示しています。

![斜線のグラデーション軸を持つフレーム](lineargradient-images/gradient-axis.png)

この図では、破線はグラデーション軸を示しています。グラデーション軸は、始点から終点までのグラデーションの補間パスを強調表示します。

### <a name="create-a-horizontal-linear-gradient"></a>水平方向の線状グラデーションを作成する

水平方向の線状グラデーションを作成するには、オブジェクトを作成 `LinearGradientBrush` し、 `StartPoint` を (0, 0) に設定し、 `EndPoint` を (1, 0) に設定します。 次に、2つ以上の `GradientStop` オブジェクトをコレクションに追加します。これにより、グラデーションの `LinearGradientBrush.GradientStops` 色とその位置が指定されます。

次の XAML の例は、のとして設定された水平を示してい `LinearGradientBrush` `Background` [`Frame`](xref:Xamarin.Forms.Frame) ます。

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0) -->
        <LinearGradientBrush EndPoint="1,0">
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>  
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が、 `LinearGradientBrush` 黄色から緑色に補間するで描画されます。

![水平 System.windows.media.lineargradientbrush> で塗りつぶされるフレーム](lineargradient-images/horizontal.png)

### <a name="create-a-vertical-linear-gradient"></a>垂直方向の線状グラデーションを作成する

垂直方向の線状グラデーションを作成するには、オブジェクトを作成 `LinearGradientBrush` し、 `StartPoint` を (0, 0) に設定し、 `EndPoint` を (0, 1) に設定します。 次に、2つ以上の `GradientStop` オブジェクトをコレクションに追加します。これにより、グラデーションの `LinearGradientBrush.GradientStops` 色とその位置が指定されます。

次の XAML の例は、のとして設定された垂直を示してい `LinearGradientBrush` `Background` [`Frame`](xref:Xamarin.Forms.Frame) ます。

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0) -->    
        <LinearGradientBrush EndPoint="0,1">
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が、 `LinearGradientBrush` 黄色から緑の垂直方向に補間するで塗りつぶされています。

![垂直 System.windows.media.lineargradientbrush> で塗りつぶされるフレーム](lineargradient-images/vertical.png)

### <a name="create-a-diagonal-linear-gradient"></a>対角線方向の線形グラデーションを作成する

対角線方向の線形グラデーションを作成するには、オブジェクトを作成 `LinearGradientBrush` し、 `StartPoint` を (0, 0) に設定し、 `EndPoint` を (1, 1) に設定します。 次に、2つ以上の `GradientStop` オブジェクトをコレクションに追加します。これにより、グラデーションの `LinearGradientBrush.GradientStops` 色とその位置が指定されます。

次の XAML の例は、のとして設定された対角線を示してい `LinearGradientBrush` `Background` [`Frame`](xref:Xamarin.Forms.Frame) ます。

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0)      
             Endpoint defaults to (1,1) -->
        <LinearGradientBrush>
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が、 `LinearGradientBrush` 黄色から緑色の対角線を補間するで塗りつぶされています。

![斜線 System.windows.media.lineargradientbrush> で塗りつぶされるフレーム](lineargradient-images/diagonal.png)

## <a name="related-links"></a>関連リンク

- [BrushesDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Xamarin.Forms ブラシ: グラデーション](gradient.md)