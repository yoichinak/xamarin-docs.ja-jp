---
title: 'Xamarin.Forms ブラシ: グラデーション'
description: Gradientbrush でクラスは、グラデーションの分岐 Xamarin.Forms 点で構成されるグラデーションを記述する抽象クラスです。
ms.prod: xamarin
ms.assetid: 24763E56-74EC-4082-897B-E4EAACCADFEE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08a423830ee3db55cb0ec7facfa5630c8832885b
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562939"
---
# <a name="no-locxamarinforms-brushes-gradients"></a>Xamarin.Forms ブラシ: グラデーション

![プレビュー API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`GradientBrush`クラスはクラスから派生し、グラデーションの分岐点で `Brush` 構成されるグラデーションを記述する抽象クラスです。 グラデーション ブラシは、軸に沿って互いに溶け込む複数の色で領域を塗りつぶします。 から派生するクラスは、 `GradientBrush` グラデーションの分岐点を解釈するさまざまな方法を記述し、 Xamarin.Forms 次のグラデーションブラシを提供します。

- `LinearGradientBrush`。線状グラデーションで領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 線状グラデーション](lineargradient.md)」を参照してください。
- `RadialGradientBrush`。放射状グラデーションで領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 放射状グラデーション](radialgradient.md)」を参照してください。

クラスは、ブラシのグラデーションの分岐点を `GradientBrush` `GradientStops` 表す型のプロパティを定義します。各グラデーションの分岐点は、ブラシ `GradientStopsCollection` のグラデーション軸に沿って色とオフセットを指定します。 `GradientStopsCollection`は、 `ObservableCollection` オブジェクトのです `GradientStop` 。 `GradientStops`プロパティはオブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> `GradientStops`プロパティは `ContentProperty` クラスのである `GradientBrush` ため、XAML から明示的に設定する必要はありません。

## <a name="gradient-stops"></a>グラデーション境界

グラデーションの分岐点はグラデーションブラシの構成要素であり、グラデーションの色とグラデーション軸に沿った位置を指定します。 グラデーションの分岐点は、オブジェクトを使用して指定し `GradientStop` ます。

`GradientStop` クラスでは、次のプロパティが定義されます。

- `Color`[`Color`](xref:Xamarin.Forms.Color)グラデーションの分岐点の色を表す、型の。 このプロパティの既定値は `Color.Default` です。
- `Offset``float`グラデーションベクター内のグラデーション境界の位置を表す型の。 このプロパティの既定値は0で、有効な値は 0.0 ~ 1.0 の範囲内です。 この値が0に近いほど、色がグラデーションの始点に近づきます。 同様に、この値が1に近いほど、色がグラデーションの端に近づきます。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

> [!IMPORTANT]
> グラデーションで使用される座標系は、出力領域の境界ボックスに対して相対的です。 0 は境界ボックスの 0% を示し、1 は境界ボックスの 100% を示します。 したがって、(0.5, 0.5) は境界ボックスの中央にある点を表し、(1, 1) は境界ボックスの右下にある点を表します。

次の XAML の例では、4色の対角線が作成され `LinearGradientBrush` ます。

```xaml
<LinearGradientBrush StartPoint="0,0"
                     EndPoint="1,1">
    <GradientStop Color="Yellow"
                  Offset="0.0" />
    <GradientStop Color="Red"
                  Offset="0.25" />
    <GradientStop Color="Blue"
                  Offset="0.75" />             
    <GradientStop Color="LimeGreen"
                  Offset="1.0" />
</LinearGradientBrush>                                                       
```

グラデーションの分岐点の間の各ポイントの色は、2つの境界グラデーションの分岐点によって指定された色の組み合わせとして補間されます。 次の図は、前の例のグラデーションの分岐点を示しています。

![斜線 System.windows.media.lineargradientbrush> で塗りつぶされるフレーム](gradient-images/gradient-stops.png)

この図では、円はグラデーションの分岐点の位置をマークし、破線はグラデーション軸を示しています。 最初のグラデーションの分岐点では、オフセット0.0 の色が黄色で指定されます。 2番目のグラデーションの分岐点は、オフセット0.25 の赤の色を示します。 グラデーション軸に沿って左から右に移動すると、これら2つのグラデーション境界の間の点は、黄色から赤に徐々に変化します。 3番目のグラデーションの分岐点では、オフセット0.75 の青色が指定されています。 2 番目と 3 番目のグラデーション境界の間の点は、赤から青に徐々に変化します。 4番目のグラデーションの分岐点は、1.0 のオフセットで緑の色を指定します。 3 番目と 4 番目のグラデーション境界の間の点は、青から緑に徐々に変化します。

## <a name="related-links"></a>関連リンク

- [BrushesDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Xamarin.Forms ブラシ: 線状グラデーション](lineargradient.md)
- [Xamarin.Forms ブラシ: 放射状グラデーション](radialgradient.md)