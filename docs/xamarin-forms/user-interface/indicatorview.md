---
title: Xamarin.Forms IndicatorView
description: IndicatorView は、CarouselView 内の項目数と現在位置を表すインジケーターを表示するコントロールです。
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 95c4e247ca0be6e5f0f39a7bc95c41a73b3f590e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556790"
---
# <a name="no-locxamarinforms-indicatorview"></a>Xamarin.Forms IndicatorView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

は、 `IndicatorView` 内の項目数と現在位置を表すインジケーターを表示するコントロールです `CarouselView` 。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](indicatorview-images/circles.png "IndicatorView の円")](indicatorview-images/circles-large.png#lightbox "IndicatorView の円")

`IndicatorView` は次の特性を定義します。

- `Count`インジケーターの数を示す型の `int` 。
- `HideSingle`型のは、 `bool` 1 つだけ存在する場合にインジケーターを非表示にするかどうかを示します。 既定値は `true` です。
- `IndicatorColor``Color`インジケーターの色 (型)。
- `IndicatorSize``double`インジケーターのサイズ (型)。 既定値は6.0 です。
- `IndicatorLayout`型のは、の `Layout<View>` レンダリングに使用されるレイアウトクラスを定義し `IndicatorView` ます。 このプロパティはによって設定され Xamarin.Forms 、通常、開発者が設定する必要はありません。
- `IndicatorTemplate`型の `DataTemplate` 。各インジケーターの外観を定義するテンプレート。
- `IndicatorsShape`型の、 `IndicatorShape` 各インジケーターの形状。
- `ItemsSource`型の `IEnumerable` 。インジケーターが表示されるコレクション。 プロパティが設定されると、このプロパティは自動的に設定され `CarouselView.IndicatorView` ます。
- `MaximumVisible`型の、 `int` 表示されるインジケーターの最大数。 既定値は `int.MaxValue` です。
- `Position`型の、 `int` 現在選択されているインジケーターインデックス。 このプロパティは、 `TwoWay` バインディングを使用します。 プロパティが設定されると、このプロパティは自動的に設定され `CarouselView.IndicatorView` ます。
- `SelectedIndicatorColor`型の `Color` 。の現在の項目を表すインジケーターの色 `CarouselView` 。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

## <a name="create-an-indicatorview"></a>IndicatorView を作成する

次の例は、XAML でをインスタンス化する方法を示してい `IndicatorView` ます。

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

この例では、の下にが `IndicatorView` レンダリングされ、 `CarouselView` 内の各項目のインジケーターが表示され `CarouselView` ます。 には、 `IndicatorView` プロパティをオブジェクトに設定することによって、データが設定され `CarouselView.IndicatorView` `IndicatorView` ます。 各インジケーターは薄い灰色の円であり、の現在の項目を表すインジケーターは `CarouselView` 濃い灰色です。

> [!IMPORTANT]
> プロパティを設定すると、プロパティがプロパティにバインドされ、プロパティがプロパティ `CarouselView.IndicatorView` `IndicatorView.Position` にバインドされ `CarouselView.Position` `IndicatorView.ItemsSource` `CarouselView.ItemsSource` ます。

## <a name="change-indicator-shape"></a>インジケーター形状の変更

`IndicatorView`クラスには `IndicatorsShape` 、インジケーターの形状を決定するプロパティがあります。 このプロパティは、列挙体のメンバーの1つに設定でき `IndicatorShape` ます。

- `Circle` インジケーター図形を円形にすることを指定します。 これは、`IndicatorView.IndicatorsShape` プロパティの既定値です。
- `Square` インジケーターの形状が正方形であることを示します。

次の例は、 `IndicatorView` 正方形のインジケーターを使用するように構成されたを示しています。

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>インジケーターサイズの変更

`IndicatorView`クラスには `IndicatorSize` `double` 、デバイスに依存しない単位でインジケーターのサイズを決定する型のプロパティがあります。 このプロパティの既定値は6.0 です。

次の例は、 `IndicatorView` より大きなインジケーターを表示するように構成されたを示しています。

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>表示されるインジケーターの数を制限する

`IndicatorView`クラスには `MaximumVisible` 、表示される `int` インジケーターの最大数を決定する型のプロパティがあります。

次の例は、 `IndicatorView` 最大6つのインジケーターを表示するように構成されたを示しています。

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>インジケーターの外観を定義する

各インジケーターの外観は、プロパティをに設定することによって定義でき `IndicatorView.IndicatorTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

「」で指定された要素は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 各インジケーターの外観を定義します。 この例では、各インジケーターは、 [`Image`](xref:Xamarin.Forms.Image) `FontImage` マークアップ拡張機能を使用してフォントアイコンを表示するです。

次のスクリーンショットは、フォントアイコンを使用して表示されるインジケーターを示しています。

[![IOS と Android のテンプレート化された IndicatorView のスクリーンショット](indicatorview-images/templated.png "テンプレート化 IndicatorView")](indicatorview-images/templated-large.png#lightbox "テンプレート化 IndicatorView")

マークアップ拡張機能の詳細については `FontImage` 、「 [FontImage markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)」を参照してください。

## <a name="related-links"></a>関連リンク

- [IndicatorView (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage のマークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)