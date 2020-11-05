---
title: Xamarin.Forms ピンのマップ
description: この記事では、マップにピンを作成する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 033b4edd6eea8a4904364c906a6f9b30da6267ef
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375435"
---
# <a name="no-locxamarinforms-map-pins"></a>Xamarin.Forms ピンのマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/workingwithmaps)

コントロールを使用 Xamarin.Forms [`Map`](xref:Xamarin.Forms.Maps.Map) すると、位置をオブジェクトでマークでき [`Pin`](xref:Xamarin.Forms.Maps.Pin) ます。 は、 `Pin` タップしたときに情報ウィンドウを開くマップマーカーです。

[![IOS と Android のマップピンとその情報ウィンドウのスクリーンショット](pins-images/pin-and-information-window.png "Pin を情報ウィンドウにマップする")](pins-images/pin-and-information-window-large.png#lightbox "Pin を情報ウィンドウにマップする")

[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトがコレクションに追加されると、 [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) ピンがマップに表示されます。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスには、次のプロパティがあります。

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)型の `string` 。通常は、pin の場所のアドレスを表します。 ただし、アドレスだけでなく、任意のコンテンツを指定でき `string` ます。
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)型の `string` 。通常は pin のタイトルを表します。
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)[`Position`](xref:Xamarin.Forms.Maps.Position)ピンの緯度と経度を表す、型の。
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)[`PinType`](xref:Xamarin.Forms.Maps.PinType)ピンの種類を表す、型の。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、は `Pin` データバインディングのターゲットにすることができます。 データバインディングオブジェクトの詳細については `Pin` 、「 [pin コレクションの表示](#display-a-pin-collection)」を参照してください。

さらに、 [`Pin`](xref:Xamarin.Forms.Maps.Pin) クラスは、 `MarkerClicked` イベントとイベントを定義し `InfoWindowClicked` ます。 `MarkerClicked`Pin がタップされるとイベントが発生し、 `InfoWindowClicked` 情報ウィンドウがタップされるとイベントが発生します。 `PinClickedEventArgs`両方のイベントに付随するオブジェクトには `HideInfoWindow` 、型の1つのプロパティがあり `bool` ます。

## <a name="display-a-pin"></a>Pin を表示する

は、 [`Pin`](xref:Xamarin.Forms.Maps.Pin) XAML のに追加でき [`Map`](xref:Xamarin.Forms.Maps.Map) ます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False">
         <x:Arguments>
             <maps:MapSpan>
                 <x:Arguments>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     <x:Double>0.01</x:Double>
                     <x:Double>0.01</x:Double>
                 </x:Arguments>
             </maps:MapSpan>
         </x:Arguments>
         <maps:Map.Pins>
             <maps:Pin Label="Santa Cruz"
                       Address="The city with a boardwalk"
                       Type="Place">
                 <maps:Pin.Position>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                 </maps:Pin.Position>
             </maps:Pin>
         </maps:Map.Pins>
     </maps:Map>
</ContentPage>
```

この XAML は、 [`Map`](xref:Xamarin.Forms.Maps.Map) オブジェクトによって指定された領域を表示するオブジェクトを作成し [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) ます。 オブジェクトは、 `MapSpan` オブジェクトによって表される緯度と経度の中央に [`Position`](xref:Xamarin.Forms.Maps.Position) あり、0.01 緯度と経度の角度を超えています。 [`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトがコレクションに追加され、 [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) `Map` プロパティによって指定された位置のに描画され [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) ます。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「 [マップの位置と距離](position-distance.md)」を参照してください。 既定のコンストラクターを持たないオブジェクトに XAML の引数を渡す方法については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

これに相当する C# コードを次に示します。

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
Pin pin = new Pin
{
  Label = "Santa Cruz",
  Address = "The city with a boardwalk",
  Type = PinType.Place,
  Position = new Position(36.9628066, -122.0194722)
};
map.Pins.Add(pin);
```

> [!WARNING]
> プロパティを設定しなかっ [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) た場合は、 `ArgumentException` がに追加されると、がスローされ [`Pin`](xref:Xamarin.Forms.Maps.Pin) [`Map`](xref:Xamarin.Forms.Maps.Map) ます。

このコード例では、マップに1つの pin がレンダリングされます。

[![IOS と Android のマップピンのスクリーンショット](pins-images/pin-only.png "ピンのマップ")](pins-images/pin-only-large.png#lightbox "ピンのマップ")

## <a name="interact-with-a-pin"></a>Pin を操作する

既定では、 [`Pin`](xref:Xamarin.Forms.Maps.Pin) がタップされると、その情報ウィンドウが表示されます。

[![IOS と Android のマップピンとその情報ウィンドウのスクリーンショット](pins-images/pin-and-information-window.png "Pin を情報ウィンドウにマップする")](pins-images/pin-and-information-window-large.png#lightbox "Pin を情報ウィンドウにマップする")

マップ上の他の場所をタップすると、情報ウィンドウが閉じます。

クラスは、 [`Pin`](xref:Xamarin.Forms.Maps.Pin) `MarkerClicked` がタップされたときに発生するイベントを定義し `Pin` ます。 情報ウィンドウを表示するためにこのイベントを処理する必要はありません。 代わりに、特定の pin がタップされたことを通知する必要がある場合に、このイベントを処理する必要があります。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)また、クラスは、 `InfoWindowClicked` 情報ウィンドウがタップされたときに発生するイベントも定義します。 このイベントは、特定の情報ウィンドウがタップされたことを通知する必要がある場合に処理する必要があります。

次のコードは、これらのイベントを処理する例を示しています。

```csharp
using Xamarin.Forms.Maps;
// ...
Pin boardwalkPin = new Pin
{
    Position = new Position(36.9641949, -122.0177232),
    Label = "Boardwalk",
    Address = "Santa Cruz",
    Type = PinType.Place
};
boardwalkPin.MarkerClicked += async (s, args) =>
{
    args.HideInfoWindow = true;
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
};

Pin wharfPin = new Pin
{
    Position = new Position(36.9571571, -122.0173544),
    Label = "Wharf",
    Address = "Santa Cruz",
    Type = PinType.Place
};
wharfPin.InfoWindowClicked += async (s, args) =>
{
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
};
```

`PinClickedEventArgs`両方のイベントに付随するオブジェクトには `HideInfoWindow` 、型の1つのプロパティがあり `bool` ます。 イベントハンドラー内でこのプロパティがに設定さ `true` れている場合、情報ウィンドウは非表示になります。

## <a name="pin-types"></a>Pin の種類

[`Pin`](xref:Xamarin.Forms.Maps.Pin) オブジェクトには、 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) pin の種類を表す型のプロパティが含ま [`PinType`](xref:Xamarin.Forms.Maps.PinType) れます。 `PinType` 列挙体を使って、次のメンバーを定義できます。

- `Generic`は汎用的な pin を表します。
- `Place`は、場所の pin を表します。
- `SavedPin`は、保存された場所の pin を表します。
- `SearchResult`は、検索結果の pin を表します。

ただし、プロパティを [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) 任意のメンバーに設定しても、 [`PinType`](xref:Xamarin.Forms.Maps.PinType) 表示される pin の外観は変わりません。 代わりに、カスタムレンダラーを作成して、pin の外観をカスタマイズする必要があります。 詳細については、「 [マップのピン留めをカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)」を参照してください。

## <a name="display-a-pin-collection"></a>Pin コレクションを表示する

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、次のプロパティを定義します。

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)`IEnumerable`表示する項目のコレクションを指定する型の。 `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)型の。 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) これは、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 表示されている項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`型の。 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) これは、 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 実行時に項目のを選択するために使用されるを指定します。

> [!IMPORTANT]
> [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)プロパティ `ItemTemplate` とプロパティの両方が設定されている場合は、プロパティが優先 `ItemTemplateSelector` されます。

データバインディングを使用して、 [`Map`](xref:Xamarin.Forms.Maps.Map) プロパティをコレクションにバインドすることにより、にピンを設定でき [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) `IEnumerable` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
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

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)プロパティデータは、 `Locations` 接続されたビューモデルのプロパティにバインドされます。このプロパティ `ObservableCollection` は、カスタム型のオブジェクトのを返し `Location` ます。 各オブジェクトは、型の `Location` プロパティとプロパティ、および型のプロパティを定義し `Address` `Description` `string` `Position` [`Position`](xref:Xamarin.Forms.Maps.Position) ます。

コレクション内の各項目の外観を `IEnumerable` 定義するには、 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`Pin`](xref:Xamarin.Forms.Maps.Pin) 適切なプロパティにデータをバインドするオブジェクトを含むにプロパティを設定します。

次のスクリーンショットは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`Pin`](xref:Xamarin.Forms.Maps.Pin) データバインディングを使用してコレクションを表示する方法を示しています。

[![IOS と Android でのデータバインドされた pin を使用したマップのスクリーンショット](pins-images/pins-itemsource.png "データバインドされた pin を使用したマップ")](pins-images/pins-itemsource-large.png#lightbox "データバインドされた pin を使用したマップ")

### <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

コレクション内の各項目の外観は、プロパティをに `IEnumerable` 設定することにより、項目の値に基づいて実行時に選択でき `ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) ます。

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
                    <!-- Change the property values, or the properties that are bound to. -->
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

クラスの例を次に示し `MapItemTemplateSelector` ます。

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

`MapItemTemplateSelector`クラスは、 `DefaultTemplate` `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) さまざまなデータテンプレートに設定されるプロパティとプロパティを定義します。 メソッドはを返します。このメソッドは、 `OnSelectTemplate` `XamarinTemplate` がタップされたときに "Xamarin" をラベルとして表示し、 `Pin` 項目に "サンフランシスコ" を含むアドレスがある場合はそれを表示します。 "サンフランシスコ" を含むアドレスが項目に含まれていない場合、メソッドはを `OnSelectTemplate` 返し `DefaultTemplate` ます。

> [!NOTE]
> この機能のユースケースは、サブ型に基づいて、サブ分類されたオブジェクトのプロパティ [`Pin`](xref:Xamarin.Forms.Maps.Pin) を別のプロパティにバインドすることです `Pin` 。

データテンプレートセレクターの詳細については、「 [ Xamarin.Forms DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [カスタムレンダラーのマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
- [DataTemplateSelector の作成 Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)