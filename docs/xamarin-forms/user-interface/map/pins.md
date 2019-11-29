---
title: Xamarin. フォームマップのピン
description: この記事では、Xamarin. Forms マップでピンを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: 197c48a7a3486d7161d351a6b06101daaa389256
ms.sourcegitcommit: 2cc0796902123df137611b855a55b754ca3c6d73
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2019
ms.locfileid: "74556158"
---
# <a name="xamarinforms-map-pins"></a>Xamarin. フォームマップのピン

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin Forms [`Map`](xref:Xamarin.Forms.Maps.Map)コントロールを使用すると、 [`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトでマークすることができます。 `Pin` は、タップしたときに情報ウィンドウを開くマップマーカーです。

[![IOS と Android のマップピンとその情報ウィンドウのスクリーンショット](pins-images/pin-and-information-window.png "Pin を情報ウィンドウにマップする")](pins-images/pin-and-information-window-large.png#lightbox "Pin を情報ウィンドウにマップする")

[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトが[`Map.Pins`](xref:Xamarin.Forms.Maps.Pin)コレクションに追加されると、ピンがマップに表示されます。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスには、次のプロパティがあります。

- `string`型の[`Address`](xref:Xamarin.Forms.Maps.Pin.Address)。通常は、pin の場所のアドレスを表します。 ただし、アドレスだけでなく、任意の `string` コンテンツを指定することもできます。
- `string`型の[`Label`](xref:Xamarin.Forms.Maps.Pin.Label)。通常は pin のタイトルを表します。
- pin の緯度と経度を表す[`Position`](xref:Xamarin.Forms.Maps.Position)型の[`Position`](xref:Xamarin.Forms.Maps.Pin.Position)。
- pin の種類を表す[`PinType`](xref:Xamarin.Forms.Maps.PinType)型の[`Type`](xref:Xamarin.Forms.Maps.Pin.Type)。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。つまり、`Pin` をデータバインディングのターゲットにすることができます。 データバインディング `Pin` オブジェクトの詳細については、「 [pin コレクションの表示](#display-a-pin-collection)」を参照してください。

さらに、 [`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスは `MarkerClicked` イベントと `InfoWindowClicked` イベントを定義します。 `MarkerClicked` イベントは、pin がタップされると発生し、[情報] ウィンドウがタップされると、`InfoWindowClicked` イベントが発生します。 両方のイベントに付随する `PinClickedEventArgs` オブジェクトには、`bool`型の単一の `HideInfoWindow` プロパティがあります。

## <a name="display-a-pin"></a>Pin を表示する

[`Pin`](xref:Xamarin.Forms.Maps.Pin)は、XAML の[`Map`](xref:Xamarin.Forms.Maps.Map)に追加できます。

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

この XAML は、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)オブジェクトによって指定された領域を示す[`Map`](xref:Xamarin.Forms.Maps.Map)オブジェクトを作成します。 `MapSpan` オブジェクトは、 [`Position`](xref:Xamarin.Forms.Maps.Position)のオブジェクトによって表される緯度と経度の中央にあり、0.01 の緯度と経度の角度を超えています。 [`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトが[`Map.Pins`](xref:Xamarin.Forms.Maps.Pin)コレクションに追加され、 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)プロパティで指定された位置に `Map` に描画されます。 [`Position`](xref:Xamarin.Forms.Maps.Position)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。 既定のコンストラクターを持たないオブジェクトに XAML の引数を渡す方法については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

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
> [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label)プロパティを設定しなかった場合は、 [`Pin`](xref:Xamarin.Forms.Maps.Pin)が[`Map`](xref:Xamarin.Forms.Maps.Map)に追加されると、`ArgumentException` がスローされます。

このコード例では、マップに1つの pin がレンダリングされます。

[![IOS と Android のマップピンのスクリーンショット](pins-images/pin-only.png "ピンのマップ")](pins-images/pin-only-large.png#lightbox "ピンのマップ")

## <a name="interact-with-a-pin"></a>Pin を操作する

既定では、 [`Pin`](xref:Xamarin.Forms.Maps.Pin)がタップされると、その情報ウィンドウが表示されます。

[![IOS と Android のマップピンとその情報ウィンドウのスクリーンショット](pins-images/pin-and-information-window.png "Pin を情報ウィンドウにマップする")](pins-images/pin-and-information-window-large.png#lightbox "Pin を情報ウィンドウにマップする")

マップ上の他の場所をタップすると、情報ウィンドウが閉じます。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスは、`Pin` がタップされたときに発生する `MarkerClicked` イベントを定義します。 情報ウィンドウを表示するためにこのイベントを処理する必要はありません。 代わりに、特定の pin がタップされたことを通知する必要がある場合に、このイベントを処理する必要があります。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスは、情報ウィンドウがタップされたときに発生する `InfoWindowClicked` イベントも定義します。 このイベントは、特定の情報ウィンドウがタップされたことを通知する必要がある場合に処理する必要があります。

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

両方のイベントに付随する `PinClickedEventArgs` オブジェクトには、`bool`型の単一の `HideInfoWindow` プロパティがあります。 このプロパティをイベントハンドラー内で `true` に設定すると、情報ウィンドウが非表示になります。

## <a name="pin-types"></a>Pin の種類

[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトには、pin の種類を表す[`PinType`](xref:Xamarin.Forms.Maps.PinType)型の[`Type`](xref:Xamarin.Forms.Maps.Pin.Type)プロパティが含まれています。 `PinType` 列挙体を使って、次のメンバーを定義できます。

- `Generic`は、汎用の pin を表します。
- `Place`は、場所の pin を表します。
- `SavedPin`は、保存された場所の pin を表します。
- `SearchResult`は、検索結果の pin を表します。

ただし、 [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type)プロパティを任意の[`PinType`](xref:Xamarin.Forms.Maps.PinType)メンバーに設定しても、表示される pin の外観は変わりません。 代わりに、カスタムレンダラーを作成して、pin の外観をカスタマイズする必要があります。 詳細については、「[マップのピン留めをカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)」を参照してください。

## <a name="display-a-pin-collection"></a>Pin コレクションを表示する

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、次のプロパティを定義します。

- `IEnumerable`型の[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)。表示される `IEnumerable` 項目のコレクションを指定します。
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の[`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)。表示されている項目のコレクション内の各項目に適用する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を指定します。
- [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)型の `ItemTemplateSelector`。実行時に項目の[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択するために使用される[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を指定します。

> [!IMPORTANT]
> `ItemTemplate` と `ItemTemplateSelector` の両方のプロパティが設定されている場合は、 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)プロパティが優先されます。

データバインディングを使用して[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)プロパティを `IEnumerable` コレクションにバインドすることによって、 [`Map`](xref:Xamarin.Forms.Maps.Map)にピンを設定できます。

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

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)プロパティデータは、接続されたビューモデルの `Locations` プロパティにバインドされます。これにより、カスタム型の `Location` オブジェクトの `ObservableCollection` が返されます。 各 `Location` オブジェクトは、型 `string`の `Address` と `Description` プロパティ、[および `Position` 型](xref:Xamarin.Forms.Maps.Position)の`Position`プロパティを定義します。

`IEnumerable` コレクション内の各項目の外観は、データを適切なプロパティにバインドする[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトを含む[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に[`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)プロパティを設定することによって定義されます。

次のスクリーンショットは、データバインディングを使用して[`Pin`](xref:Xamarin.Forms.Maps.Pin)コレクションを表示する[`Map`](xref:Xamarin.Forms.Maps.Map)を示しています。

[![IOS と Android でのデータバインドされた pin を使用したマップのスクリーンショット](pins-images/pins-itemsource.png "データバインドされた pin を使用したマップ")](pins-images/pins-itemsource-large.png#lightbox "データバインドされた pin を使用したマップ")

### <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

`IEnumerable` コレクション内の各項目の外観は、項目の値に基づいて実行時に選択できます。そのためには、`ItemTemplateSelector` プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に設定します。

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

`MapItemTemplateSelector` クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `DefaultTemplate` と `XamarinTemplate` を定義します。 `OnSelectTemplate` メソッドは、`XamarinTemplate`を返します。この場合、`Pin` がタップされたときに "Xamarin" というラベルが表示され、その項目に "サンフランシスコ" が含まれているアドレスが含まれています。 "サンフランシスコ" を含むアドレスが項目にない場合、`OnSelectTemplate` メソッドは `DefaultTemplate`を返します。

> [!NOTE]
> この機能のユースケースは、`Pin` サブ型に基づいて、サブ[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトのプロパティを異なるプロパティにバインドすることです。

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [カスタムレンダラーのマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [渡す (引数を XAML で)](~/xamarin-forms/xaml/passing-arguments.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
