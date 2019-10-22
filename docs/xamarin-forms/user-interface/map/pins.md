---
title: Xamarin. フォームマップのピン
description: この記事では、Xamarin. Forms マップでピンを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/23/2019
ms.openlocfilehash: 76535f9c31a9dc138e132a3e582b986daf89bdb0
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697669"
---
# <a name="xamarinforms-map-pins"></a>Xamarin. フォームマップのピン

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin Forms `Maps` コントロールを使用すると、`Pin` オブジェクトでマークすることができます。 @No__t_0 は、クリックまたはタップしたときに情報ウィンドウを開くマップマーカーです。

@No__t_0 クラスには、次のプロパティがあります。

- `Type` は `PinType` 列挙値 (Generic、Place、SavedPin、または SearchResult) です。
- `Position` は、pin の緯度と経度を含む `Position` インスタンスです。
- `Label` は、通常、pin タイトルとして表示される `string` です。
- `Address` は、[情報] ウィンドウに表示される `string` です。 アドレスだけでなく、任意の `string` コンテンツを指定できます。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。つまり、`Pin` をデータバインディングのターゲットにすることができます。 詳細については、「[データバインディングを使用して pin を作成する](#create-pins-with-data-binding)」を参照してください。

## <a name="create-map-pins"></a>マップのピンを作成する

コードで `Pin` インスタンスを作成し、マップに追加することができます。

```csharp
Pin pin1 = new Pin
{
    Type = PinType.Place,
    Position = new Position(47.6368678, -122.137305),
    Label = "Example Pin 1",
    Address = "Example custom details..."
};
map.Pins.Add(pin1);
```

> [!NOTE]
> @No__t_0 値は、プラットフォームに応じて pin がどのようにレンダリングされるかに影響します。 Pin の外観をカスタマイズするには、カスタムレンダラーを作成する必要があります。 詳細については、「[マップのピン留めをカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)」を参照してください。

## <a name="create-pins-with-data-binding"></a>データバインドを使用したピンの作成

[@No__t_1](xref:Xamarin.Forms.Maps.Map)クラスは、次のプロパティを公開します。

- `ItemsSource` –表示する `IEnumerable` 項目のコレクションを指定します。
- `ItemTemplate` –表示されている項目のコレクション内の各項目に適用する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を指定します。
- `ItemTemplateSelector` –実行時に項目の[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択するために使用される[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を指定します。

> [!NOTE]
> @No__t_1 と `ItemTemplateSelector` の両方のプロパティが設定されている場合は、`ItemTemplate` プロパティが優先されます。

データバインディングを使用して、その `ItemsSource` プロパティを `IEnumerable` コレクションにバインドすることによって、 [`Map`](xref:Xamarin.Forms.Maps.Map)にデータを設定できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

@No__t_0 プロパティデータは、接続されたビューモデルの `Locations` プロパティにバインドされます。これにより、カスタム型の `Location` オブジェクトの `ObservableCollection` が返されます。 各 `Location` オブジェクトは、型 `string` の `Address` と `Description` プロパティ、[および `Position` 型](xref:Xamarin.Forms.Maps.Position)の `Position` プロパティを定義します。

@No__t_0 コレクション内の各項目の外観は、データを適切なプロパティにバインドする[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトを含む[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に `ItemTemplate` プロパティを設定することによって定義されます。

次のスクリーンショットは、データバインディングを使用して[`Pin`](xref:Xamarin.Forms.Maps.Pin)コレクションを表示する[`Map`](xref:Xamarin.Forms.Maps.Map)を示しています。

[![IOS と Android でのデータバインドされた pin を使用したマップのスクリーンショット](map-images/pins-itemssource.png "データバインドされた pin を使用したマップ")](map-images/pins-itemssource-large.png#lightbox "データバインドされた pin を使用したマップ")

### <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

@No__t_0 コレクション内の各項目の外観は、項目の値に基づいて実行時に選択できます。そのためには、`ItemTemplateSelector` プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に設定します。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

次の例は、`MapItemTemplateSelector` クラスを示しています。

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

@No__t_0 クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `DefaultTemplate` と `XamarinTemplate` を定義します。 @No__t_0 メソッドは、`XamarinTemplate` を返します。この場合、`Pin` がタップされたときに "Xamarin" というラベルが表示され、その項目に "サンフランシスコ" が含まれているアドレスが含まれています。 "サンフランシスコ" を含むアドレスが項目にない場合、`OnSelectTemplate` メソッドは `DefaultTemplate` を返します。

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="pin-events"></a>イベントのピン留め

@No__t_0 クラスには、次の2つのイベントがあります。

- pin がクリックまたはタップされると `MarkerClicked` が発生します。
- `InfoWindowClicked` は、情報ウィンドウがクリックまたはタップされたときに発生します。

イベントハンドラーは、コード内の pin にアタッチできます。

```csharp
public class PinEvents: ContentPage
{
    public PinEvents()
    {
        // ...

        pin1.MarkerClicked += OnMarkerClickedAsync;
        pin1.InfoWindowClicked += OnInfoWindowClickedAsync;
    }

    async void OnMarkerClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
    }

    async void OnInfoWindowClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
    }
}
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [カスタムレンダラーのマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
