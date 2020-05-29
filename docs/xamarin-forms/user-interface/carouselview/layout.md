---
title: Xamarin.FormsCarouselView レイアウト
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 44df710df0272afe3c6f6911381af1a88c8cf923
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140284"
---
# <a name="xamarinforms-carouselview-layout"></a>Xamarin.FormsCarouselView レイアウト

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)レイアウトを制御する次のプロパティを定義します。

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)型ので、 `LinearItemsLayout` 使用するレイアウトを指定します。
- `PeekAreaInsets`型の。は、 [`Thickness`](xref:Xamarin.Forms.Thickness) 隣接する項目を部分的に表示する方法を指定します。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

既定では、の [`CarouselView`](xref:Xamarin.Forms.CarouselView) 項目は水平方向に表示されます。 1つの項目が画面に表示されます。スワイプジェスチャを使用すると、項目のコレクションの前後に移動します。 ただし、垂直方向も可能です。 これは、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) プロパティが型であり `LinearItemsLayout` 、クラスから継承されるためです [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 。 `ItemsLayout`クラスは、次のプロパティを定義します。

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)型の。は、を [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) [`CarouselView`](xref:Xamarin.Forms.CarouselView) 項目として追加する方向を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)(型) では、 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) スナップポイントを項目と整列する方法を指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)型のは、 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) スクロール時のスナップポイントの動作を指定します。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。 スナップポイントの詳細については、「 [ Xamarin.Forms CollectionView スクロール](scrolling.md)ガイド」の「[スナップポイント](scrolling.md#snap-points)」を参照してください。

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙体は、次のメンバーを定義します。

- `Vertical`項目が追加されると、が垂直方向に展開されることを示し [`CarouselView`](xref:Xamarin.Forms.CarouselView) ます。
- `Horizontal`項目が追加されると、が水平方向に展開されることを示し [`CarouselView`](xref:Xamarin.Forms.CarouselView) ます。

クラスは、 `LinearItemsLayout` クラスから継承され、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) `ItemSpacing` 各項目の周囲の空白を表す型のプロパティを定義し `double` ます。 このプロパティの既定値は0で、値は常に0以上である必要があります。 クラスは、 `LinearItemsLayout` 静的 `Vertical` メンバーとメンバーも定義し `Horizontal` ます。 これらのメンバーを使用すると、縦または横のリストをそれぞれ作成できます。 または、 `LinearItemsLayout` オブジェクトを作成し、 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 列挙型のメンバーを引数として指定することもできます。

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)ネイティブレイアウトエンジンを使用してレイアウトを実行します。

## <a name="horizontal-layout"></a>水平方向のレイアウト

既定で [`CarouselView`](xref:Xamarin.Forms.CarouselView) は、アイテムは水平方向に表示されます。 このため、 [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) このレイアウトを使用するようにプロパティを設定する必要はありません。

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

または、プロパティを [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) オブジェクトに設定し `LinearItemsLayout` 、 `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 列挙型のメンバーをプロパティ値として指定することで、このレイアウトを実現することもでき `Orientation` ます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

同等の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

この結果、新しい項目が追加されるにつれて、レイアウトが横方向に拡大します。

[![IOS と Android での CarouselView 横レイアウトのスクリーンショット](layout-images/horizontal.png "CarouselView 横レイアウト")](layout-images/horizontal-large.png#lightbox "CarouselView 横レイアウト")

## <a name="vertical-layout"></a>縦方向のレイアウト

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、プロパティをオブジェクトに設定して、プロパティ [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) `LinearItemsLayout` `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 値として列挙メンバーを指定することで、項目を垂直方向に表示でき `Orientation` ます。

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

同等の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

この結果、新しい項目が追加されるにつれて、レイアウトが垂直方向に拡大します。

[![IOS と Android での CarouselView 縦レイアウトのスクリーンショット](layout-images/vertical.png "CarouselView 縦レイアウト")](layout-images/vertical-large.png#lightbox "CarouselView 縦レイアウト")

## <a name="partially-visible-adjacent-items"></a>部分的に表示される隣接項目

既定では、は [`CarouselView`](xref:Xamarin.Forms.CarouselView) すべての項目を一度に表示します。 ただし、この動作は、 `PeekAreaInsets` `Thickness` によって、隣接する項目を部分的に表示する方法を指定する値にプロパティを設定することによって変更できます。 これは、表示する追加項目があることをユーザーに示す場合に便利です。 次の XAML は、このプロパティを設定する例を示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    ...
</CarouselView>
```

同等の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    PeekAreaInsets = new Thickness(100)
};
```

結果として、隣接する項目が画面に部分的に公開されます。

[![IOS と Android で、部分的に表示されている隣接する項目を含む CollectionView のスクリーンショット](layout-images/peek-items.png "CarouselView ピーク領域のインセット")](layout-images/peek-items-large.png#lightbox "CarouselView ピーク領域のインセット")

## <a name="item-spacing"></a>項目の間隔

既定では、内の各項目の間にスペースはありません [`CarouselView`](xref:Xamarin.Forms.CarouselView) 。 この動作は `ItemSpacing` 、によって使用される項目のレイアウトのプロパティを設定することによって変更でき `CarouselView` ます。

が [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) プロパティをオブジェクトに設定すると、 `LinearItemsLayout` プロパティは、 `LinearItemsLayout.ItemSpacing` `double` 項目間のスペースを表す値に設定できます。

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
> プロパティには、 `LinearItemsLayout.ItemSpacing` プロパティの値が常に0以上であることを保証する検証コールバックセットがあります。

同等の C# コードを次に示します。

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

このコードを実行すると、項目間に20の間隔がある縦方向のレイアウトが生成されます。

## <a name="dynamic-resizing-of-items"></a>項目の動的なサイズ変更

内の項目は [`CarouselView`](xref:Xamarin.Forms.CarouselView) 、内の要素のレイアウトに関連するプロパティを変更することで、実行時に動的に変更でき [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 たとえば、次のコード例では、 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) オブジェクトのプロパティとプロパティ、 [`Image`](xref:Xamarin.Forms.Image) および `HeightRequest` その親のプロパティを変更し [`Frame`](xref:Xamarin.Forms.Frame) ます。

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

`OnImageTapped`イベントハンドラーは、タップされるオブジェクトに応答して実行され [`Image`](xref:Xamarin.Forms.Image) 、イメージ (およびその親) のサイズを変更して `Frame` 、より簡単に表示できるようにします。

[![IOS と Android での動的な項目サイズ設定を使用した CarouselView のスクリーンショット](layout-images/runtime-resizing.png "CarouselView 動的な項目のサイズ変更")](layout-images/runtime-resizing-large.png#lightbox "CarouselView 動的な項目のサイズ変更")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、プロパティをに設定することによって、右から左へのフロー方向にコンテンツをレイアウトでき [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) ます。 ただし、 `FlowDirection` プロパティはページまたはルートレイアウトで設定することをお勧めします。これにより、ページ内のすべての要素、またはルートレイアウトがフロー方向に応答するようになります。

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

親を [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 持つ要素の既定値は [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) です。 したがって、は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) `FlowDirection` からプロパティ値を継承し [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.FormsCarouselView スクロール](scrolling.md)
