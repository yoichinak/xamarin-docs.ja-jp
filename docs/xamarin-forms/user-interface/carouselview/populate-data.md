---
title: CarouselView データ
description: CarouselView には、ItemsSource プロパティを、IEnumerable を実装する任意のコレクションに設定することによってデータが設定されます。
ms.prod: xamarin
ms.assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
ms.openlocfilehash: 8ec66a8d39f373b624e3a597e62014e3b1c72f56
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480557"
---
# <a name="xamarinforms-carouselview-data"></a>CarouselView データ

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)には、表示するデータとその外観を定義する次のプロパティが含まれています。

- `IEnumerable`型の[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)は、表示される項目のコレクションを指定します。既定値は `null`です。
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)は、表示される項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は、新しい項目が追加されたときの `CarouselView` のスクロール動作を表す `ItemsUpdatingScrollMode` プロパティを定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](scrolling.md#control-scroll-position-when-new-items-are-added)」を参照してください。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、ユーザーがスクロールするときにデータを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](#load-data-incrementally)」を参照してください。

## <a name="populate-a-carouselview-with-data"></a>CarouselView にデータを設定する

[`CarouselView`](xref:Xamarin.Forms.CarouselView)には、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを `IEnumerable`を実装する任意のコレクションに設定することによってデータが設定されます。 項目は、文字列の配列から `ItemsSource` プロパティを初期化することによって、XAML で追加できます。

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

同等の C# コードを次に示します。

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
> 基になるコレクションで項目が追加、削除、または変更されたときに、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)を更新する必要がある場合、基になるコレクションは、`ObservableCollection`などのプロパティ変更通知を送信する `IEnumerable` コレクションである必要があります。

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)項目は水平方向に表示されます。 次のスクリーンショットは、iOS と Android で異なる文字列項目を表示する `CarouselView` を示しています。

[![IOS と Android のテキスト項目を含む CarouselView のスクリーンショット](populate-data-images/text.png "CarouselView のテキスト項目")](populate-data-images/text-large.png#lightbox "CarouselView のテキスト項目")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)の向きを変更する方法の詳細については、「 [Xamarin CarouselView Layout](layout.md)」を参照してください。 `CarouselView`内の各項目の外観を定義する方法の詳細については、「[項目の外観を定義](#define-item-appearance)する」を参照してください。

### <a name="data-binding"></a>データ バインディング

データバインディングを使用して、その[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを `IEnumerable` コレクションにバインドすることによって、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)にデータを設定できます。 XAML では、これは `Binding` マークアップ拡張機能を使用して実現されます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

同等の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例では、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティデータを接続されたビューモデルの `Monkeys` プロパティにバインドします。

> [!NOTE]
> Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させるために、コンパイル済みのバインドを有効にすることができます。 詳しくは、「[コンパイル済みのバインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」(Xamarin.Forms データ バインディング) をご覧ください。

## <a name="define-item-appearance"></a>項目の外観を定義する

[`CarouselView`](xref:Xamarin.Forms.CarouselView)内の各項目の外観は、 [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義できます。

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

同等の C# コードを次に示します。

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

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で指定された要素は、`CarouselView`内の各項目の外観を定義します。 この例では、`DataTemplate` 内のレイアウトは[`StackLayout`](xref:Xamarin.Forms.StackLayout)によって管理され、データには[`Image`](xref:Xamarin.Forms.Image)オブジェクトと3つの[`Label`](xref:Xamarin.Forms.Label)オブジェクトが表示されます。これらはすべて、`Monkey` クラスのプロパティにバインドされます。

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

[`CarouselView`](xref:Xamarin.Forms.CarouselView)内の各項目の外観は、項目の値に基づいて実行時に選択できます。そのためには、 [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定します。

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

同等の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティは `MonkeyDataTemplateSelector` オブジェクトに設定されます。 次の例は、`MonkeyDataTemplateSelector` クラスを示しています。

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

`MonkeyDataTemplateSelector` クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `AmericanMonkey` と `OtherMonkey` を定義します。 `OnSelectTemplate` override は、サル名に "America" が含まれている場合に、`AmericanMonkey` テンプレートを返します。 サル名に "America" が含まれていない場合、`OnSelectTemplate` のオーバーライドによって `OtherMonkey` テンプレートが返され、そのデータがグレーで表示されます。

[![IOS と Android での CarouselView runtime item テンプレートの選択のスクリーンショット](populate-data-images/datatemplateselector.png "CarouselView でのランタイム項目テンプレートの選択")](populate-data-images/datatemplateselector-large.png#lightbox "CarouselView でのランタイム項目テンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)を使用する場合は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトのルート要素を `ViewCell`に設定しないでください。 これにより、`CarouselView` にセルの概念がないため、例外がスローされます。

## <a name="display-indicators"></a>インジケーターの表示

`CarouselView`内の項目数と現在位置を表すインジケーターは、`CarouselView`の横に表示できます。 これは、`IndicatorView` コントロールを使用して実現できます。

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView IndicatorView.ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

この例では、`IndicatorView` が `CarouselView`の下にレンダリングされ、`CarouselView`内の各項目のインジケーターが表示されます。 `IndicatorView` には、`ItemsSourceBy` 添付プロパティを `CarouselView` オブジェクトに設定することによってデータが設定されます。 各インジケーターは薄い灰色の円であり、`CarouselView` の現在の項目を表すインジケーターは濃い灰色です。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](populate-data-images/indicators.png "IndicatorView の円")](populate-data-images/indicators-large.png#lightbox "IndicatorView の円")

> [!IMPORTANT]
> `IndicatorView.ItemsSourceBy` 添付プロパティを設定すると、`IndicatorView.Position` プロパティバインドが `CarouselView.Position` プロパティになり、`IndicatorView.ItemsSource` プロパティが `CarouselView.ItemsSource` プロパティにバインドされます。

インジケーターの詳細については、「 [IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)」を参照してください。

## <a name="pull-to-refresh"></a>プルして更新

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、`RefreshView`を通じてプルを更新する機能がサポートされています。これにより、表示されているデータを項目にプルダウンして更新できます。 `RefreshView` は、子がスクロール可能なコンテンツをサポートしていれば、その子に対してプルを行う機能を提供するコンテナーコントロールです。 そのため、`RefreshView`の子として設定することにより、`CarouselView` の pull to refresh が実装されます。

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

同等の C# コードを次に示します。

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

ユーザーが更新を開始すると、`Command` プロパティによって定義された `ICommand` が実行されます。これにより、表示される項目が更新されます。 更新が行われている間に、更新の視覚化が表示されます。これは、アニメーションの進行状況の円で構成されます。

[![IOS と Android での CarouselView のプルから更新のスクリーンショット](populate-data-images/pull-to-refresh.png "CarouselView のプルから更新")](populate-data-images/pull-to-refresh-large.png#lightbox "CarouselView のプルから更新")

`RefreshView.IsRefreshing` プロパティの値は、`RefreshView`の現在の状態を示します。 ユーザーによって更新がトリガーされると、このプロパティは自動的に `true`に移行します。 更新が完了したら、プロパティを `false`にリセットする必要があります。

`RefreshView`の詳細については、「 [Xamarin. フォーム RefreshView](~/xamarin-forms/user-interface/refreshview.md)」を参照してください。

## <a name="load-data-incrementally"></a>データを増分読み込み

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、ユーザーが項目をスクロールすると、データの読み込みを段階的にサポートします。 これにより、ユーザーがスクロールするときに web サービスからデータページを非同期的に読み込むなどのシナリオが可能になります。 さらに、より多くのデータが読み込まれるポイントは、ユーザーが空白の領域を表示しないように、またはスクロールから停止するように構成できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、次のプロパティを定義して、データの増分読み込みを制御します。

- `int`型の `RemainingItemsThreshold`、`RemainingItemsThresholdReached` イベントが発生するリストにまだ表示されていない項目のしきい値。
- `ICommand`型の `RemainingItemsThresholdReachedCommand`。 `RemainingItemsThreshold` に達したときに実行されます。
- `RemainingItemsThresholdReachedCommandParameter`: `object` 型、`RemainingItemsThresholdReachedCommand` に渡されるパラメーター。

また、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) `RemainingItemsThreshold` 項目が表示されていない場合に `CarouselView` がスクロールされたときに発生する `RemainingItemsThresholdReached` イベントも定義します。 このイベントを処理して、さらに多くの項目を読み込むことができます。 さらに、`RemainingItemsThresholdReached` イベントが発生すると、`RemainingItemsThresholdReachedCommand` が実行され、増分データの読み込みがビューモデルで行われるようになります。

`RemainingItemsThreshold` プロパティの既定値は-1 です。これは、`RemainingItemsThresholdReached` イベントが発生しないことを示します。 プロパティ値が0の場合、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)の最後の項目が表示されるときに `RemainingItemsThresholdReached` イベントが発生します。 0より大きい値の場合、`ItemsSource` にまだスクロールされていない項目の数が含まれていると、`RemainingItemsThresholdReached` イベントが発生します。

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は `RemainingItemsThreshold` プロパティを検証して、その値が常に-1 以上になるようにします。

次の XAML の例は、データを増分読み込みする[`CarouselView`](xref:Xamarin.Forms.CarouselView)を示しています。

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

同等の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

このコード例では、まだスクロールされていない2つの項目があり、応答で `OnCollectionViewRemainingItemsThresholdReached` イベントハンドラーを実行すると、`RemainingItemsThresholdReached` イベントが発生します。

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> データは、`RemainingItemsThresholdReachedCommand` をビューモデルの `ICommand` の実装にバインドすることによって、段階的に読み込むこともできます。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)
- [Xamarin. フォーム RefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
