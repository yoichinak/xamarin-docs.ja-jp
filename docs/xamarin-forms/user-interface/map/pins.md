---
title: Xamarin. フォームマップのピン
description: この記事では、Xamarin. Forms マップでピンを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: 3df78a7c8eaf12306ade182f134f8d294d203af5
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517585"
---
# <a name="xamarinforms-map-pins"></a>Xamarin. フォームマップのピン

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin. フォーム[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールでは、オブジェクトを使用し[`Pin`](xref:Xamarin.Forms.Maps.Pin)て場所をマークできます。 `Pin`は、タップしたときに情報ウィンドウを開くマップマーカーです。

[![IOS と Android のマップピンとその情報ウィンドウのスクリーンショット](pins-images/pin-and-information-window.png "Pin を情報ウィンドウにマップする")](pins-images/pin-and-information-window-large.png#lightbox "Pin を情報ウィンドウにマップする")

[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトが[`Map.Pins`](xref:Xamarin.Forms.Maps.Pin)コレクションに追加されると、ピンがマップに表示されます。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスには、次のプロパティがあります。

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)型`string`の。通常は、pin の場所のアドレスを表します。 ただし、アドレスだけでなく`string` 、任意のコンテンツを指定できます。
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)型`string`の。通常は pin のタイトルを表します。
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)ピンの緯度[`Position`](xref:Xamarin.Forms.Maps.Position)と経度を表す、型の。
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)ピンの種類[`PinType`](xref:Xamarin.Forms.Maps.PinType)を表す、型の。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支え`Pin`られています。つまり、はデータバインディングのターゲットにすることができます。 データバインディング`Pin`オブジェクトの詳細については、「 [Pin コレクションの表示](#display-a-pin-collection)」を参照してください。

さらに、クラス[`Pin`](xref:Xamarin.Forms.Maps.Pin)は、 `MarkerClicked`イベント`InfoWindowClicked`とイベントを定義します。 Pin `MarkerClicked`がタップされるとイベントが発生し、情報`InfoWindowClicked`ウィンドウがタップされるとイベントが発生します。 両方`PinClickedEventArgs`のイベントに付随するオブジェクトには`HideInfoWindow` 、型`bool`の1つのプロパティがあります。

## <a name="display-a-pin"></a>Pin を表示する

は[`Pin`](xref:Xamarin.Forms.Maps.Pin) 、XAML [`Map`](xref:Xamarin.Forms.Maps.Map)のに追加できます。

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

この XAML は、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)オブジェクトによって指定された領域を表示するオブジェクトを作成します。 `MapSpan`オブジェクトは、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトによって表される緯度と経度の中央にあり、0.01 緯度と経度の角度を超えています。 [`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクト[`Map.Pins`](xref:Xamarin.Forms.Maps.Pin)がコレクションに追加され、 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)プロパティによって`Map`指定された位置のに描画されます。 構造体の[`Position`](xref:Xamarin.Forms.Maps.Position)詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。 既定のコンストラクターを持たないオブジェクトに XAML の引数を渡す方法については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

該当の C# コードを次に示します。

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
> [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label)プロパティを設定しなかった場合は、 `ArgumentException` [`Pin`](xref:Xamarin.Forms.Maps.Pin)がに追加されると、が[`Map`](xref:Xamarin.Forms.Maps.Map)スローされます。

このコード例では、マップに1つの pin がレンダリングされます。

[![IOS と Android のマップピンのスクリーンショット](pins-images/pin-only.png "ピンのマップ")](pins-images/pin-only-large.png#lightbox "ピンのマップ")

## <a name="interact-with-a-pin"></a>Pin を操作する

既定では、 [`Pin`](xref:Xamarin.Forms.Maps.Pin)がタップされると、その情報ウィンドウが表示されます。

[![IOS と Android のマップピンとその情報ウィンドウのスクリーンショット](pins-images/pin-and-information-window.png "Pin を情報ウィンドウにマップする")](pins-images/pin-and-information-window-large.png#lightbox "Pin を情報ウィンドウにマップする")

マップ上の他の場所をタップすると、情報ウィンドウが閉じます。

クラス[`Pin`](xref:Xamarin.Forms.Maps.Pin)は、 `Pin`が`MarkerClicked`タップされたときに発生するイベントを定義します。 情報ウィンドウを表示するためにこのイベントを処理する必要はありません。 代わりに、特定の pin がタップされたことを通知する必要がある場合に、このイベントを処理する必要があります。

また[`Pin`](xref:Xamarin.Forms.Maps.Pin) 、クラスは、 `InfoWindowClicked`情報ウィンドウがタップされたときに発生するイベントも定義します。 このイベントは、特定の情報ウィンドウがタップされたことを通知する必要がある場合に処理する必要があります。

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

両方`PinClickedEventArgs`のイベントに付随するオブジェクトには`HideInfoWindow` 、型`bool`の1つのプロパティがあります。 イベントハンドラー内でこのプロパティ`true`がに設定されている場合、情報ウィンドウは非表示になります。

## <a name="pin-types"></a>Pin の種類

[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトには[`Type`](xref:Xamarin.Forms.Maps.Pin.Type) 、pin の種類[`PinType`](xref:Xamarin.Forms.Maps.PinType)を表す型のプロパティが含まれます。 `PinType` 列挙体を使って、次のメンバーを定義できます。

- `Generic`は汎用的な pin を表します。
- `Place`は、場所の pin を表します。
- `SavedPin`は、保存された場所の pin を表します。
- `SearchResult`は、検索結果の pin を表します。

ただし、 [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type)プロパティを任意[`PinType`](xref:Xamarin.Forms.Maps.PinType)のメンバーに設定しても、表示される pin の外観は変わりません。 代わりに、カスタムレンダラーを作成して、pin の外観をカスタマイズする必要があります。 詳細については、「[マップのピン留めをカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)」を参照してください。

## <a name="display-a-pin-collection"></a>Pin コレクションを表示する

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、次のプロパティを定義します。

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)表示する項目`IEnumerable`の`IEnumerable`コレクションを指定する型の。
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。これは、表示[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)されている項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`型[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)の。これは、実行[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)時に項目のを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)選択するために使用されるを指定します。

> [!IMPORTANT]
> プロパティ[`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) `ItemTemplate`と`ItemTemplateSelector`プロパティの両方が設定されている場合は、プロパティが優先されます。

データ[`Map`](xref:Xamarin.Forms.Maps.Map)バインディングを使用して、 [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)プロパティを`IEnumerable`コレクションにバインドすることにより、にピンを設定できます。

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

プロパティ[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)データは、接続さ`Locations`れたビューモデルのプロパティにバインドさ`ObservableCollection`れ`Location`ます。このプロパティは、カスタム型のオブジェクトのを返します。 各`Location`オブジェクトは`Address` 、 `Description`型のプロパティと`string`プロパティ`Position` 、および型[`Position`](xref:Xamarin.Forms.Maps.Position)のプロパティを定義します。

コレクション内の各項目の外観を定義するには、 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)適切なプロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)にデータを[`Pin`](xref:Xamarin.Forms.Maps.Pin)バインドするオブジェクトを含むにプロパティを設定します。 `IEnumerable`

次のスクリーンショットは[`Map`](xref:Xamarin.Forms.Maps.Map) 、データ[`Pin`](xref:Xamarin.Forms.Maps.Pin)バインディングを使用してコレクションを表示する方法を示しています。

[![IOS と Android でのデータバインドされた pin を使用したマップのスクリーンショット](pins-images/pins-itemsource.png "データバインドされた pin を使用したマップ")](pins-images/pins-itemsource-large.png#lightbox "データバインドされた pin を使用したマップ")

### <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

`IEnumerable`コレクション内の各項目の外観は、 `ItemTemplateSelector`プロパティをに設定することにより、項目の値に基づいて実行時に選択でき[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)ます。

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

クラスの`MapItemTemplateSelector`例を次に示します。

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

クラス`MapItemTemplateSelector`は、 `DefaultTemplate`さまざま`XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)なデータテンプレートに設定されるプロパティとプロパティを定義します。 メソッド`OnSelectTemplate`はを返し`XamarinTemplate`ます。このメソッドは、 `Pin`がタップされたときに "Xamarin" をラベルとして表示し、項目に "サンフランシスコ" を含むアドレスがある場合はそれを表示します。 "サンフランシスコ" を含むアドレスが項目に含まれていない`OnSelectTemplate`場合、メソッド`DefaultTemplate`はを返します。

> [!NOTE]
> この機能のユースケースは、サブ型に基づいて[`Pin`](xref:Xamarin.Forms.Maps.Pin) `Pin` 、サブ分類されたオブジェクトのプロパティを別のプロパティにバインドすることです。

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [カスタムレンダラーのマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [渡す (引数を XAML で)](~/xamarin-forms/xaml/passing-arguments.md)
- [Xamarin.Forms DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
