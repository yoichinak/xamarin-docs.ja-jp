---
title: CarouselView データ
description: CarouselView には、ItemsSource プロパティを、IEnumerable を実装する任意のコレクションに設定することによってデータが設定されます。
ms.prod: xamarin
ms.assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/29/2020
ms.openlocfilehash: cdd77d333ead9b4ff4d2cf29b1e36ee2f287dd22
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517372"
---
# <a name="xamarinforms-carouselview-data"></a>CarouselView データ

![](~/media/shared/preview.png "This API is currently pre-release")

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)には、表示するデータとその外観を定義する次のプロパティが含まれています。

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)型`IEnumerable`のは、表示される項目のコレクションを指定します。の`null`既定値はです。
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のは、表示される項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)新しい項目`ItemsUpdatingScrollMode`が追加され`CarouselView`たときののスクロール動作を表すプロパティを定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](scrolling.md#control-scroll-position-when-new-items-are-added)」を参照してください。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)ユーザーがスクロールするときにデータの増分仮想化をサポートします。 詳細については、「[データの増分読み込み](#load-data-incrementally)」を参照してください。

## <a name="populate-a-carouselview-with-data"></a>CarouselView にデータを設定する

に[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティをを実装`IEnumerable`する任意のコレクションに設定することにより、データが設定されます。 項目は、文字列の配列からプロパティを`ItemsSource`初期化することによって、XAML で追加できます。

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> `x:Array` 要素には、配列内の項目の型を示す `Type` 属性が必要です。

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> 基に[`CarouselView`](xref:Xamarin.Forms.CarouselView)なるコレクションで項目が追加、削除、または変更されたときにを更新する必要がある場合`IEnumerable` 、基になるコレクションは、など`ObservableCollection`のプロパティ変更通知を送信するコレクションである必要があります。

既定では[`CarouselView`](xref:Xamarin.Forms.CarouselView) 、アイテムは水平方向に表示されます。 次のスクリーンショットは`CarouselView` 、IOS と Android でのさまざまな文字列項目の表示を示しています。

[![IOS と Android のテキスト項目を含む CarouselView のスクリーンショット](populate-data-images/text.png "CarouselView のテキスト項目")](populate-data-images/text-large.png#lightbox "CarouselView のテキスト項目")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)向きを変更する方法の詳細については、「 [Xamarin CarouselView Layout](layout.md)」を参照してください。 の各項目の外観を定義する方法の詳細につい`CarouselView`ては、「[項目の外観を定義](#define-item-appearance)する」を参照してください。

### <a name="data-binding"></a>データ バインディング

[`CarouselView`](xref:Xamarin.Forms.CarouselView)データバインディングを使用してデータを設定し、その[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを`IEnumerable`コレクションにバインドすることができます。 XAML では、これはマークアップ`Binding`拡張機能を使用して実現されます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例では、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティデータは接続さ`Monkeys`れたビューモデルのプロパティにバインドされます。

> [!NOTE]
> コンパイル済みのバインドを有効にすると、Xamarin. Forms アプリケーションでのデータバインディングのパフォーマンスを向上させることができます。 詳しくは、「[コンパイル済みのバインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義する

の[`CarouselView`](xref:Xamarin.Forms.CarouselView)各項目の外観は、 [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティをに設定することによって[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)定義できます。

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

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

「」で指定さ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)れた要素は、 `CarouselView`内の各項目の外観を定義します。 この例では、内の`DataTemplate`レイアウトはによっ[`StackLayout`](xref:Xamarin.Forms.StackLayout)て管理され、データは[`Image`](xref:Xamarin.Forms.Image)オブジェクトと3つの[`Label`](xref:Xamarin.Forms.Label)オブジェクトで表示されます。これらは`Monkey`すべて、クラスのプロパティにバインドされます。

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

次のスクリーンショットは、各項目のテンプレートの結果を示しています。

[![IOS と Android で、各項目がテンプレート化されている CarouselView のスクリーンショット](populate-data-images/datatemplate.png "CarouselView のテンプレート項目")](populate-data-images/datatemplate-large.png#lightbox "CarouselView のテンプレート項目")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

の[`CarouselView`](xref:Xamarin.Forms.CarouselView)各項目の外観は、項目の値に基づいて実行時に選択できます。これは[`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 、プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定することによって行います。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

プロパティ[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)が`MonkeyDataTemplateSelector`オブジェクトに設定されています。 クラスの`MonkeyDataTemplateSelector`例を次に示します。

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

クラス`MonkeyDataTemplateSelector`は、 `AmericanMonkey`さまざま`OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)なデータテンプレートに設定されるプロパティとプロパティを定義します。 この`OnSelectTemplate`オーバーライドは、 `AmericanMonkey`サル名に "America" が含まれている場合にテンプレートを返します。 サル名に "America" が含まれてい`OnSelectTemplate`ない場合、 `OtherMonkey`上書きによってテンプレートが返され、そのデータがグレーで表示されます。

[![IOS と Android での CarouselView runtime item テンプレートの選択のスクリーンショット](populate-data-images/datatemplateselector.png "CarouselView でのランタイム項目テンプレートの選択")](populate-data-images/datatemplateselector-large.png#lightbox "CarouselView でのランタイム項目テンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

> [!IMPORTANT]
> を使用[`CarouselView`](xref:Xamarin.Forms.CarouselView)する場合は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトのルート要素をに設定`ViewCell`しないでください。 これにより、にセルの概念が`CarouselView`ないため、例外がスローされます。

## <a name="display-indicators"></a>インジケーターの表示

の項目と現在位置`CarouselView`を表すインジケーターは、の`CarouselView`横に表示できます。 これは、コントロールを使用`IndicatorView`して実現できます。

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

この例では、 `IndicatorView`の下`CarouselView`にがレンダリングされ、内の各項目の`CarouselView`インジケーターが表示されます。 に`IndicatorView`は、 `CarouselView.IndicatorView`プロパティを`IndicatorView`オブジェクトに設定することによって、データが設定されます。 各インジケーターは薄い灰色の円であり、の`CarouselView`現在の項目を表すインジケーターは濃い灰色です。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](populate-data-images/indicators.png "IndicatorView の円")](populate-data-images/indicators-large.png#lightbox "IndicatorView の円")

> [!IMPORTANT]
> `CarouselView.IndicatorView` `CarouselView.Position` `IndicatorView.ItemsSource` `CarouselView.ItemsSource`プロパティを設定すると、プロパティがプロパティにバインドされ、プロパティがプロパティにバインドされます。 `IndicatorView.Position`

インジケーターの詳細については、「 [IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)」を参照してください。

## <a name="context-menus"></a>コンテキスト メニュー

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、を使用し`SwipeView`てデータ項目のコンテキストメニューをサポートしています。これにより、スワイプジェスチャを使用してコンテキストメニューが示されます。 `SwipeView`は、コンテンツの項目をラップし、そのコンテンツ項目のコンテキストメニュー項目を提供するコンテナーコントロールです。 そのため、 `CarouselView` `SwipeView` `SwipeView`ラップするコンテンツを定義するを作成し、スワイプジェスチャによって公開されるコンテキストメニュー項目を作成することによって、に対してコンテキストメニューを実装します。 これを実現するには`SwipeView` 、の[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)データの各項目の外観を定義するをに`CarouselView`追加します。

```xaml
<CarouselView x:Name="carouselView"
              ItemsSource="{Binding Monkeys}">
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
                        <SwipeView>
                            <SwipeView.TopItems>
                                <SwipeItems>
                                    <SwipeItem Text="Favorite"
                                               IconImageSource="favorite.png"
                                               BackgroundColor="LightGreen"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.FavoriteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.TopItems>
                            <SwipeView.BottomItems>
                                <SwipeItems>
                                    <SwipeItem Text="Delete"
                                               IconImageSource="delete.png"
                                               BackgroundColor="LightPink"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.DeleteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.BottomItems>
                            <StackLayout>
                                <!-- Define item appearance -->
                            </StackLayout>
                        </SwipeView>
                    </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    StackLayout stackLayout = new StackLayout();
    Frame frame = new Frame { ... };

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: carouselView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: carouselView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.TopItems = new SwipeItems { favoriteSwipeItem };
    swipeView.BottomItems = new SwipeItems { deleteSwipeItem };

    StackLayout swipeViewStackLayout = new StackLayout { ... };
    swipeView.Content = swipeViewStackLayout;
    frame.Content = swipeView;
    stackLayout.Children.Add(frame);

    return stackLayout;
});
```

この例では、 `SwipeView`コンテンツは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Frame`](xref:Xamarin.Forms.Frame)内ので囲まれた各項目の外観を定義するです[`CarouselView`](xref:Xamarin.Forms.CarouselView)。 スワイプ項目は、 `SwipeView`コンテンツに対する操作を実行するために使用されます。また、コントロールが一番下からスワイプされたときに明らかになります。

[![Screenshot of CarouselView bottom context menu item, on iOS and Android](populate-data-images/swipeview-bottom.png "CarouselView with bottom SwipeView コンテキストメニュー項目")](populate-data-images/swipeview-bottom-large.png#lightbox "CarouselView with bottom SwipeView コンテキストメニュー項目")Ios と android の
[![CarouselView トップメニュー項目](populate-data-images/swipeview-top.png "Top SwipeView コンテキストメニュー項目を含む CarouselView")の ios と android の CarouselView 下部コンテキストメニュー項目のスクリーンショット](populate-data-images/swipeview-top-large.png#lightbox "Top SwipeView コンテキストメニュー項目を含む CarouselView")

`SwipeView`4つの異なるスワイプ方向をサポートします。スワイプ方向は、 `SwipeItems` `SwipeItems`オブジェクトが追加される方向のコレクションによって定義されます。 既定では、スワイプ項目はユーザーがタップしたときに実行されます。 また、スワイプ項目が実行されると、スワイプ項目が非表示になり`SwipeView` 、コンテンツが再度表示されます。 ただし、これらの動作は変更できます。

コントロールの`SwipeView`詳細については、「 [SwipeView](~/xamarin-forms/user-interface/swipeview.md)」を参照してください。

## <a name="pull-to-refresh"></a>引っ張って更新

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、を使用した`RefreshView`プルへのプルの機能がサポートされています。これにより、表示されているデータを項目の上にプルダウンして更新できます。 は`RefreshView` 、子がスクロール可能なコンテンツをサポートしている場合に、その子に対してプルを行う機能を提供するコンテナーコントロールです。 そのため、の子として設定`CarouselView`することにより、に対する pull `RefreshView`to refresh が実装されます。

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

該当の C# コードを次に示します。

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

ユーザーが更新を開始すると、 `ICommand` `Command`プロパティによって定義されたが実行されます。これにより、表示されている項目が更新されます。 更新が行われている間に、更新の視覚化が表示されます。これは、アニメーションの進行状況の円で構成されます。

[![IOS と Android での CarouselView のプルから更新のスクリーンショット](populate-data-images/pull-to-refresh.png "CarouselView のプルから更新")](populate-data-images/pull-to-refresh-large.png#lightbox "CarouselView のプルから更新")

`RefreshView.IsRefreshing`プロパティの値は、 `RefreshView`の現在の状態を示します。 ユーザーによって更新がトリガーされると、このプロパティは自動的`true`にに移行します。 更新が完了したら、プロパティをに`false`リセットする必要があります。

の詳細につい`RefreshView`ては、「 [Xamarin. フォーム refreshview](~/xamarin-forms/user-interface/refreshview.md)」を参照してください。

## <a name="load-data-incrementally"></a>データを増分読み込み

[`CarouselView`](xref:Xamarin.Forms.CarouselView)ユーザーがスクロールするときにデータの増分仮想化をサポートします。 これにより、ユーザーがスクロールするときに web サービスからデータページを非同期的に読み込むなどのシナリオが可能になります。 さらに、より多くのデータが読み込まれるポイントは、ユーザーが空白の領域を表示しないように、またはスクロールから停止するように構成できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、次のプロパティを定義して、データの増分読み込みを制御します。

- `RemainingItemsThreshold`型`int`の、 `RemainingItemsThresholdReached`イベントが発生するリストにまだ表示されていない項目のしきい値。
- `RemainingItemsThresholdReachedCommand`に到達し`ICommand`たとき`RemainingItemsThreshold`に実行される、型の。
- `RemainingItemsThresholdReachedCommandParameter`: `object` 型、`RemainingItemsThresholdReachedCommand`に渡されるパラメーターです。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)また、は`RemainingItemsThresholdReached` 、 `RemainingItemsThreshold`項目が表示され`CarouselView`ていない大きさまでスクロールしたときに発生するイベントも定義します。 このイベントを処理して、さらに多くの項目を読み込むことができます。 さらに、 `RemainingItemsThresholdReached`イベントが発生すると、 `RemainingItemsThresholdReachedCommand`が実行され、増分データの読み込みがビューモデルで行われるようになります。

`RemainingItemsThreshold`プロパティの既定値は-1 です。これは、 `RemainingItemsThresholdReached`イベントが発生しないことを示します。 プロパティ値が0の場合、の`RemainingItemsThresholdReached` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)最後の項目が表示されるときにイベントが発生します。 0より大きい値の場合、 `RemainingItemsThresholdReached`にまだスクロールされて`ItemsSource`いない項目の数が含まれていると、イベントが発生します。

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)プロパティを`RemainingItemsThreshold`検証して、その値が常に-1 以上であることを確認します。

次の XAML の例は[`CarouselView`](xref:Xamarin.Forms.CarouselView) 、データを増分読み込みするを示しています。

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

このコード例では、 `RemainingItemsThresholdReached`まだスクロールされていない項目が2つある場合にイベントが発生`OnCollectionViewRemainingItemsThresholdReached`し、応答ではイベントハンドラーが実行されます。

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> をビューモデルの`RemainingItemsThresholdReachedCommand` `ICommand`実装にバインドすることによって、データを徐々に読み込むこともできます。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)
- [Xamarin. フォーム RefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
