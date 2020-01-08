---
title: IndicatorView
description: IndicatorView は、CarouselView 内の項目数と現在位置を表すインジケーターを表示するコントロールです。
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
ms.openlocfilehash: 6b7845011470d83d8f2187e0227950c23e46d52d
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490518"
---
# <a name="xamarinforms-indicatorview"></a>IndicatorView

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView` は、`CarouselView`内の項目数と現在位置を表すインジケーターを表示するコントロールです。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](indicatorview-images/circles.png "IndicatorView の円")](indicatorview-images/circles-large.png#lightbox "IndicatorView の円")

`IndicatorView` は、iOS および Android プラットフォームの Xamarin. Forms 4.4 で使用できます。 ただし、現在は実験的で、`Forms.Init`を呼び出す前に、次のコード行を iOS の `AppDelegate` クラス、または Android の `MainActivity` クラスに追加することによってのみ使用できます。

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` 次のプロパティを定義します。

- インジケーターの数 `int`型の `Count`。
- 型 `bool`の `HideSingle`は、1つだけ存在する場合にインジケーターを非表示にするかどうかを示します。 既定値は `true`です。
- インジケーターの色を `Color`型の `IndicatorColor`。
- インジケーターのサイズ `double`型の `IndicatorSize`。 既定値は6.0 です。
- `Layout<View>`型の `IndicatorLayout`は、`IndicatorView`を表示するために使用するレイアウトクラスを定義します。 このプロパティは、Xamarin. Forms によって設定されます。通常、開発者が設定する必要はありません。
- 各インジケーターの外観を定義するテンプレート `DataTemplate`型の `IndicatorTemplate`。
- 各インジケーターの形状 `IndicatorShape`型の `IndicatorsShape`。
- `IEnumerable`型の `ItemsSource`、インジケーターが表示されるコレクションです。 このプロパティは、`ItemsSourceBy` プロパティが設定されている場合に自動的に設定されます。
- インジケーターを表示する `CarouselView` オブジェクト `VisualElement`型の `ItemsSourceBy`。
- `int`型の `MaximumVisible`、表示されるインジケーターの最大数。 既定値は `int.MaxValue`です。
- 現在選択されているインジケーターインデックスの型 `int`の `Position`。 このプロパティは、`TwoWay` バインドを使用します。 このプロパティは、`ItemsSourceBy` プロパティが設定されている場合に自動的に設定されます。
- `CarouselView`内の現在の項目を表すインジケーターの色 `Color`型の `SelectedIndicatorColor`。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

## <a name="create-an-indicatorview"></a>IndicatorView を作成する

次の例は、XAML で `IndicatorView` をインスタンス化する方法を示しています。

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

この例では、`IndicatorView` が `CarouselView`の下にレンダリングされ、`CarouselView`内の各項目のインジケーターが表示されます。 `IndicatorView` には、`ItemsSourceBy` プロパティを `CarouselView` オブジェクトに設定することによってデータが設定されます。 各インジケーターは薄い灰色の円であり、`CarouselView` の現在の項目を表すインジケーターは濃い灰色です。

> [!IMPORTANT]
> `ItemsSourceBy` プロパティを設定すると、`Position` プロパティのバインドが `CarouselView.Position` プロパティになり、`ItemsSource` プロパティが `CarouselView.ItemsSource` プロパティにバインドされます。

## <a name="change-indicator-shape"></a>インジケーター形状の変更

`IndicatorView` クラスには、インジケーターの形状を示す `IndicatorsShape` プロパティがあります。 このプロパティは、`IndicatorShape` 列挙型のメンバーのいずれかに設定できます。

- `Circle` は、インジケーター図形を円形にすることを指定します。 これは、`IndicatorView.IndicatorsShape` プロパティの既定値です。
- `Square` は、インジケーター図形が正方形であることを示します。

次の例は、正方形のインジケーターを使用するように構成された `IndicatorView` を示しています。

```xaml
<IndicatorView IndicatorsShape="Square"
               ItemsSourceBy="carouselView"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="define-indicator-appearance"></a>インジケーターの外観を定義する

各インジケーターの外観は、`IndicatorView.IndicatorTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義できます。

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
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

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で指定された要素は、各インジケーターの外観を定義します。 この例では、各インジケーターは、`FontImage` マークアップ拡張機能を使用してフォントアイコンを表示する[`Image`](xref:Xamarin.Forms.Image)です。

次のスクリーンショットは、フォントアイコンを使用して表示されるインジケーターを示しています。

[![IOS と Android のテンプレート化された IndicatorView のスクリーンショット](indicatorview-images/templated.png "テンプレート化 IndicatorView")](indicatorview-images/templated-large.png#lightbox "テンプレート化 IndicatorView")

`FontImage` マークアップ拡張機能の詳細については、「 [FontImage markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)」を参照してください。

## <a name="related-links"></a>関連リンク

- [IndicatorView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage のマークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
