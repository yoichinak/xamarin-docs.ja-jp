---
title: CarouselView レイアウト
description: 既定では、CarouselView はその項目を水平方向に表示します。 ただし、垂直方向も可能です。
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: 051bbc1732dc1b074d27080f74621a57a80aaaa4
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749821"
---
# <a name="xamarinforms-carouselview-layout"></a>CarouselView レイアウト

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、レイアウトを制御する次のプロパティを定義します。

- `LinearItemsLayout` 型の[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)は、使用するレイアウトを指定します。
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)型の[`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)は、使用する項目メジャーの方法を指定します。
- `int` 型の `NumberOfSideItems`、現在の項目に隣接して表示される項目の数。 既定値は 0 です。
- [`Thickness`](xref:Xamarin.Forms.Thickness)型の `PeekAreaInsets` は、で隣接する項目を部分的に表示する量を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)の項目は水平方向に表示されます。 1つの項目が画面に表示されます。スワイプジェスチャを使用すると、項目のコレクションの前後に移動します。 ただし、垂直方向も可能です。 これは、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)プロパティの型が `LinearItemsLayout` であり、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承されているためです。 @No__t_0 クラスは、次のプロパティを定義します。

- [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)型の[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)は、項目が追加されたときに[`CarouselView`](xref:Xamarin.Forms.CarouselView)を拡張する方向を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)型の[`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)は、スナップポイントを項目にどのように整列させるかを指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)型の[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)は、スクロール時のスナップポイントの動作を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。 スナップポイントの詳細については、「 [CollectionView スクロール](scrolling.md)ガイド」の「[スナップポイント](scrolling.md#snap-points)」を参照してください。

[@No__t_1](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙体は、次のメンバーを定義します。

- `Vertical` は、項目が追加されると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)が垂直方向に拡張されることを示します。
- `Horizontal` は、項目が追加されると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)が横方向に拡張されることを示します。

@No__t_0 クラスは[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承され、各項目の周囲の空白を表す `double` 型の `ItemSpacing` プロパティを定義します。 このプロパティの既定値は0で、値は常に0以上である必要があります。 @No__t_0 クラスは、静的 `Vertical` と `Horizontal` メンバーも定義します。 これらのメンバーを使用すると、縦または横のリストをそれぞれ作成できます。 または、`LinearItemsLayout` オブジェクトを作成し、 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙型のメンバーを引数として指定することもできます。

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は、ネイティブレイアウトエンジンを使用してレイアウトを実行します。

## <a name="horizontal-layout"></a>水平レイアウト

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)の項目は水平方向に表示されます。 このため、このレイアウトを使用するように[`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを設定する必要はありません。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

また、このレイアウトを実現するには、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを `LinearItemsLayout` オブジェクトに設定し、`Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙メンバーを `Orientation` プロパティ値として指定します。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

これにより、新しい項目が追加されたときに水平方向に拡大するレイアウトになります。

## <a name="vertical-layout"></a>縦方向のレイアウト

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを `LinearItemsLayout` オブジェクトに設定して、`Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙メンバーを `Orientation` プロパティ値として指定することで、項目を垂直方向に表示できます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CarouselView.ItemsLayout>
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

この結果、新しい項目が追加されるにつれて、レイアウトが垂直方向に拡大します。

## <a name="partially-visible-adjacent-items"></a>部分的に表示される隣接項目

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)すべての項目が一度に表示されます。 ただし、この動作は、`PeekAreaInsets` プロパティを、によって部分的に表示される隣接する項目の数を指定する `Thickness` 値に設定することによって変更できます。 これは、表示する追加項目があることをユーザーに示す場合に便利です。 次の XAML は、このプロパティを設定する例を示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    PeekAreaInsets = new Thickness(100)
};
```

結果として、隣接する項目が画面に部分的に公開されます。

## <a name="fully-visible-adjacent-items"></a>完全に表示されている隣接項目

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)は一度に1つの項目を表示します。 ただし、この動作は、`NumberOfSideItems` プロパティを、現在の項目に隣接して表示する項目の数を表す整数に設定することによって変更できます。 次の XAML は、このプロパティを設定する例を示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              NumberOfSideItems="1">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    NumberOfSideItems = 1
};
```

この例では、現在の項目の各辺に1つの隣接する項目が表示されます。

> [!NOTE]
> @No__t_0 プロパティを設定しても、`PeekAreaInsets` の値は適用されます。

## <a name="item-spacing"></a>項目の間隔

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)内の各項目には、その周囲に空の領域がありません。 この動作は、`CarouselView` によって使用される項目のレイアウトのプロパティを設定することによって変更できます。

[@No__t_1](xref:Xamarin.Forms.CarouselView)が[`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを `LinearItemsLayout` オブジェクトに設定する場合、`LinearItemsLayout.ItemSpacing` プロパティは、各項目の周囲の空白を表す `double` 値に設定できます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

> [!NOTE]
> @No__t_0 プロパティには検証コールバックセットがあります。これにより、プロパティの値が常に0以上になるようにします。

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

このコードを実行すると、各項目の周りに20の間隔を持つ縦方向のレイアウトが生成されます。

## <a name="dynamic-resizing-of-items"></a>項目の動的なサイズ変更

[@No__t_1](xref:Xamarin.Forms.CarouselView)内の項目は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)内の要素のレイアウトに関連するプロパティを変更することによって、実行時に動的にサイズを変更できます。 たとえば、次のコード例では、 [`Image`](xref:Xamarin.Forms.Image)オブジェクトの[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)と[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)プロパティ、およびその親[`Frame`](xref:Xamarin.Forms.Frame)の `HeightRequest` プロパティを変更します。

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

@No__t_0 イベントハンドラーは、タップされる[`Image`](xref:Xamarin.Forms.Image)オブジェクトに応答して実行され、イメージ (およびその親フレーム) のサイズを変更して、より簡単に表示できるようにします。

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティを[`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)に設定することによって、右から左へのフロー方向にコンテンツをレイアウトできます。 ただし、`FlowDirection` プロパティは、ページまたはルートレイアウトで設定することをお勧めします。これにより、ページ内のすべての要素またはルートレイアウトがフロー方向に応答します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CarouselViewDemos.Views.HorizontalTemplateLayoutRTLPage"
             Title="Horizontal layout (RTL FlowDirection)"
             FlowDirection="RightToLeft">    
    <CarouselView ItemsSource="{Binding Monkeys}">
        ...
    </CarouselView>
</ContentPage>
```

親を持つ要素の既定の[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)は[`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)です。 したがって、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)は[`ContentPage`](xref:Xamarin.Forms.ContentPage)から `FlowDirection` プロパティ値を継承します。

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [CarouselView のスクロール](scrolling.md)
