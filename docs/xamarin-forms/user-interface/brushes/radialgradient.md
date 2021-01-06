---
title: 'Xamarin.Forms ブラシ: 放射状グラデーション'
description: Xamarin.FormsRadialGradientBrush クラスは、放射状グラデーションで領域を塗りつぶします。
ms.prod: xamarin
ms.assetid: 099BA530-3B38-4005-9B19-A0EB4D4DEEFC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5bba3fd223d5777036cb0e31030aad22d46ed864
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940045"
---
# <a name="no-locxamarinforms-brushes-radial-gradients"></a>Xamarin.Forms ブラシ: 放射状グラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`RadialGradientBrush`クラスはクラスから派生 `GradientBrush` し、放射状グラデーションで領域を塗りつぶします。これにより、円全体で2つ以上の色がブレンドされます。 `GradientStop` オブジェクトは、グラデーションの色とその位置を指定するために使用されます。 オブジェクトの詳細について `GradientStop` は、「 [ Xamarin.Forms ブラシ: グラデーション](gradient.md)」を参照してください。

`RadialGradientBrush` クラスでは、次のプロパティが定義されます。

- `Center`[`Point`](xref:Xamarin.Forms.Point)放射状グラデーションの円の中心点を表す型の。 このプロパティの既定値は (0.5, 0.5) です。
- `Radius``double`放射状グラデーションの円の半径を表す型の。 このプロパティの既定値は0.5 です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

`RadialGradientBrush`クラスには、ブラシに `IsEmpty` `bool` オブジェクトが割り当てられているかどうかを表すを返すメソッドもあり `GradientStop` ます。

> [!NOTE]
> また、CSS 関数を使用して放射状グラデーションを作成することもでき `radial-gradient()` ます。

## <a name="create-a-radialgradientbrush"></a>RadialGradientBrush を作成する

放射状グラデーションブラシのグラデーションの分岐点は、円で定義されたグラデーション軸に沿って配置されます。 グラデーション軸は、円の中心から円周までを放射状にします。 円の位置とサイズは、ブラシのプロパティとプロパティを使用して変更でき `Center` `Radius` ます。 円は、グラデーションの終点を定義します。 したがって、1.0 のグラデーションの分岐点は、円の円周の色を定義します。 0.0 のグラデーションの分岐点は、円の中心の色を定義します。

放射状グラデーションを作成するには、 `RadialGradientBrush` オブジェクトを作成し、その `Center` プロパティとプロパティを設定し `Radius` ます。 次に、2つ以上の `GradientStop` オブジェクトをコレクションに追加します。これにより、グラデーションの `RadialGradientBrush.GradientStops` 色とその位置が指定されます。

次の XAML の例は、のとして設定されたを示してい `RadialGradientBrush` `Background` [`Frame`](xref:Xamarin.Forms.Frame) ます。

```xaml    
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- Center defaults to (0.5,0.5)
             Radius defaults to (0.5) -->
        <RadialGradientBrush>
            <GradientStop Color="Red"
                          Offset="0.1" />
            <GradientStop Color="DarkBlue"
                          Offset="1.0" />
        </RadialGradientBrush>
    </Frame.Background>
</Frame>
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が、 `RadialGradientBrush` 赤から濃い青のを補間するで塗りつぶされています。 放射状グラデーションの中心がの中央に配置され `Frame` ます。

![中央 RadialGradientBrush で塗りつぶされるフレーム](radialgradient-images/center.png)

次の XAML の例では、放射状グラデーションの中心をの左上隅に移動し [`Frame`](xref:Xamarin.Forms.Frame) ます。

```xaml
<!-- Radius defaults to (0.5) -->
<RadialGradientBrush Center="0.0,0.0">
    <GradientStop Color="Red"
                  Offset="0.1" />
    <GradientStop Color="DarkBlue"
                  Offset="1.0" />
</RadialGradientBrush>
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が、 `RadialGradientBrush` 赤から濃い青のを補間するで塗りつぶされています。 放射状グラデーションの中心がの左上に配置され `Frame` ます。

![左上の RadialGradientBrush で塗りつぶされるフレーム](radialgradient-images/top-left.png)

次の XAML の例では、放射状グラデーションの中心をの右下隅に移動し [`Frame`](xref:Xamarin.Forms.Frame) ます。

```xaml
<!-- Radius defaults to (0.5) -->
<RadialGradientBrush Center="1.0,1.0">
    <GradientStop Color="Red"
                  Offset="0.1" />
    <GradientStop Color="DarkBlue"
                  Offset="1.0" />
</RadialGradientBrush>            
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が、 `RadialGradientBrush` 赤から濃い青のを補間するで塗りつぶされています。 放射状グラデーションの中心は、の右下に配置され `Frame` ます。

![右下の RadialGradientBrush で塗りつぶされるフレーム](radialgradient-images/bottom-right.png)

## <a name="related-links"></a>関連リンク

- [BrushesDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Xamarin.Forms ブラシ: グラデーション](gradient.md)
