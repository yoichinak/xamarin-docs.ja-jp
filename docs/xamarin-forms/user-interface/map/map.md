---
title: Xamarin. フォームマップコントロール
description: マップコントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 3861200446ea9c0e368aa251f3e7ec3f992c7152
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517524"
---
# <a name="xamarinforms-map-control"></a>Xamarin. フォームマップコントロール

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。

[![IOS と Android でのマップコントロールのスクリーンショット](map-images/map-default.png "マップ コントロール")](map-images/map-default-large.png#lightbox "マップ コントロール")

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、マップの外観と動作を制御する次のプロパティを定義します。

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)型`bool`のは、マップにユーザーの現在の場所が表示されているかどうかを示します。
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)表示する項目`IEnumerable`の`IEnumerable`コレクションを指定する型の。
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。これは、表示[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)されている項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`型[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)の。これは、実行[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)時に項目のを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)選択するために使用されるを指定します。
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)型`bool`ので、マップのスクロールが許可されているかどうかを判断します。
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)型`bool`のは、マップのズームを許可するかどうかを決定します。
- `MapElements`型`IList<MapElement>`のは、多角形やポリラインなど、マップ上の要素のリストを表します。
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)型[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)のは、マップの表示スタイルを示します。
- `MoveToLastRegionOnLayoutChange`型`bool`の。レイアウトの変更が発生したときに、表示されているマップ領域を現在の領域から前の領域に移動するかどうかを制御します。
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)型`IList<Pin>`のは、マップ上のピンのリストを表します。
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)型[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)のは、現在表示されているマップの領域を返します。

これらの`MapElements`プロパティは、、 `Pins`、および`VisibleRegion`の各プロパティを除き、オブジェクトに[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)よって支えられています。これは、データバインディングのターゲットになる可能性があることを意味します。

また[`Map`](xref:Xamarin.Forms.Maps.Map) 、クラスは、 `MapClicked`マップがタップされたときに発生するイベントも定義します。 イベント`MapClickedEventArgs`に付随するオブジェクトには、型`Position` [`Position`](xref:Xamarin.Forms.Maps.Position)のという名前のプロパティが1つあります。 イベントが発生すると、 `Position`プロパティは、タップされたマップの場所に設定されます。 構造体の[`Position`](xref:Xamarin.Forms.Maps.Position)詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)、 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)、および`ItemTemplateSelector`の各プロパティの詳細については、「 [pin コレクションの表示](pins.md#display-a-pin-collection)」を参照してください。

## <a name="display-a-map"></a>マップを表示する

は[`Map`](xref:Xamarin.Forms.Maps.Map) 、レイアウトまたはページに追加することによって表示できます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> 追加`xmlns`の名前空間定義は、Xamarin. Forms. マップコントロールを参照するために必要です。 前の例では`Xamarin.Forms.Maps` 、キーワードに`maps`よって名前空間が参照されています。

該当の C# コードを次に示します。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Maps;

namespace WorkingWithMaps
{
    public class MapTypesPageCode : ContentPage
    {
        public MapTypesPageCode()
        {
            Map map = new Map();
            Content = map;
        }
    }
}
```

この例では、 [`Map`](xref:Xamarin.Forms.Maps.Map)既定のコンストラクターを呼び出します。これにより、ローマでマップが中心になります。

[![IOS と Android での既定の場所を使用したマップコントロールのスクリーンショット](map-images/map-default.png "既定の場所でのマップコントロール")](map-images/map-default-large.png#lightbox "既定の場所でのマップコントロール")

または、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)引数を[`Map`](xref:Xamarin.Forms.Maps.Map)コンストラクターに渡して、マップの読み込み時にマップの中心点とズームレベルを設定することもできます。 詳細については、「[マップに特定の場所を表示する](#display-a-specific-location-on-a-map)」を参照してください。

## <a name="map-types"></a>マップの種類

[`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティを[`MapType`](xref:Xamarin.Forms.Maps.MapType)列挙メンバーに設定すると、マップの表示スタイルを定義できます。 `MapType` 列挙体を使って、次のメンバーを定義できます。

- `Street`道路地図が表示されることを指定します。
- `Satellite`サテライト画像を含むマップが表示されることを指定します。
- `Hybrid`道路と衛星のデータを組み合わせたマップが表示されることを指定します。

既定では、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティが定義されていない場合、はストリートマップを表示します。 また、 `MapType`プロパティを[`MapType`](xref:Xamarin.Forms.Maps.MapType)列挙型のメンバーの1つに設定することもできます。

```xaml
<maps:Map MapType="Satellite" />
```

該当の C# コードを次に示します。

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

次のスクリーンショットは[`Map`](xref:Xamarin.Forms.Maps.Map) 、プロパティ[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)がに`Street`設定されている場合を示しています。

[![IOS と Android でのマップコントロールのスクリーンショット (ストリートマップの種類)](map-images/maptype-street.png "ストリート maptype によるマップコントロール")](map-images/maptype-street-large.png#lightbox "ストリートマップの種類によるマップコントロール")

次のスクリーンショットは[`Map`](xref:Xamarin.Forms.Maps.Map) 、プロパティ[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)がに`Satellite`設定されている場合を示しています。

[![IOS と Android での、衛星マップの種類を使用したマップコントロールのスクリーンショット](map-images/maptype-satellite.png "サテライト maptype を使用したマップコントロール")](map-images/maptype-satellite-large.png#lightbox "衛星マップの種類を使用したマップコントロール")

次のスクリーンショットは[`Map`](xref:Xamarin.Forms.Maps.Map) 、プロパティ[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)がに`Hybrid`設定されている場合を示しています。

[![IOS と Android での、ハイブリッドマップの種類を使用したマップコントロールのスクリーンショット](map-images/maptype-hybrid.png "ハイブリッド maptype によるマップコントロール")](map-images/maptype-hybrid-large.png#lightbox "ハイブリッドマップの種類によるマップコントロール")

## <a name="display-a-specific-location-on-a-map"></a>マップ上の特定の場所を表示する

マップが読み込まれたときに表示するマップの領域は、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) [`Map`](xref:Xamarin.Forms.Maps.Map)コンストラクターに引数を渡すことによって設定できます。

```xaml
<maps:Map>
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
</maps:Map>
```

該当の C# コードを次に示します。

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

この例では[`Map`](xref:Xamarin.Forms.Maps.Map) 、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)オブジェクトによって指定された領域を示すオブジェクトを作成します。 `MapSpan`オブジェクトは、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトによって表される緯度と経度の中央にあり、0.01 緯度と0.01 経度の角度にまたがります。 構造体の[`Position`](xref:Xamarin.Forms.Maps.Position)詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。 XAML で引数を渡す方法の詳細については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

結果として、マップが表示されると、特定の位置に中央に配置され、緯度と経度の角度が特定の数にまたがります。

[![指定された場所のマップコントロールのスクリーンショット (iOS と Android)](map-images/map-region.png "指定した位置にコントロールをマップする")](map-images/map-region-large.png#lightbox "指定した位置にコントロールをマップする")

## <a name="create-a-mapspan-object"></a>MapSpan オブジェクトを作成する

オブジェクトを作成[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)するには、いくつかの方法があります。 一般的な方法では、 `MapSpan`コンストラクターに必須の引数を指定します。 これらは、 [`Position`](xref:Xamarin.Forms.Maps.Position) `MapSpan`オブジェクトによって表される緯度と経度`double`で、によっての緯度と経度の角度を表す値です。 構造体の[`Position`](xref:Xamarin.Forms.Maps.Position)詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

また、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)クラスには、新しい`MapSpan`オブジェクトを返すメソッドが3つあります。

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)メソッドの`MapSpan`クラスインスタンスと`LongitudeDegrees`同じを持つを返し、引数`north`および`south`引数によって定義された半径を返します。
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)引数`MapSpan` [`Position`](xref:Xamarin.Forms.Maps.Position)および[`Distance`](xref:Xamarin.Forms.Maps.Distance)引数によって定義されたを返します。
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)メソッドの`MapSpan`クラスインスタンスと同じ中心を持つを返します。ただし、radius の`double`引数を乗算しています。

構造体の[`Distance`](xref:Xamarin.Forms.Maps.Distance)詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)が作成されると、次のプロパティにアクセスしてデータを取得できます。

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)。の地理的な[`Position`](xref:Xamarin.Forms.Maps.Position)中央のを表します。 `MapSpan`
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)`MapSpan`。によってスパンされる緯度の角度を表します。
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)`MapSpan`。によってスパンされる経度の角度を表します。
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)。 `MapSpan`半径を表します。

## <a name="move-the-map"></a>マップを移動する

マップ[`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)の位置とズームレベルを変更するには、メソッドを呼び出すことができます。 このメソッドは、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)表示するマップの領域とそのズームレベルを定義する引数を受け取ります。

次のコードは、マップ上の表示されている領域を移動する例を示しています。

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>マップをズームする

のズームレベルは、 [`Map`](xref:Xamarin.Forms.Maps.Map)その場所を変更することなく変更できます。 これは、マップ UI を使用するか、現在の場所を[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) [`Position`](xref:Xamarin.Forms.Maps.Position)引数とし[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)て使用する引数を指定してメソッドを呼び出すことによって実行できます。

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

この例では、 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)プロパティを使用して[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)マップの現在位置を指定する引数を使用してメソッドを呼び出し、緯度と経度の角度としてズームレベルを指定します。 全体的な結果として、マップのズームレベルは変更されますが、その場所は変更されません。 マップにズームを実装する別の方法として、 [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)メソッドを使用してズームファクターを制御する方法があります。

> [!IMPORTANT]
> マップをズームする (マップ UI またはプログラムによって) 必要[`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)がある`true`場合は、プロパティがである必要があります。 このプロパティの詳細については、「 [Zoom を無効にする](#disable-zoom)」を参照してください。

## <a name="customize-map-behavior"></a>マップ動作のカスタマイズ

の動作は、 [`Map`](xref:Xamarin.Forms.Maps.Map)そのプロパティの一部を設定し、 `MapClicked`イベントを処理することによってカスタマイズできます。

> [!NOTE]
> マップのカスタムレンダラーを作成して、追加のマップ動作のカスタマイズを行うことができます。 詳細については、「 [Xamarin のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)」を参照してください。

### <a name="disable-scroll"></a>スクロールを無効にする

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、型[`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) `bool`のプロパティを定義します。 既定では、この`true`プロパティはです。これは、マップのスクロールが許可されていることを示します。 このプロパティがに`false`設定されている場合、マップはスクロールしません。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map HasScrollEnabled="false" />
```

該当の C# コードを次に示します。

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>ズームを無効にする

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、型[`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) `bool`のプロパティを定義します。 既定では、この`true`プロパティはです。これは、マップ上でズームを実行できることを示します。 このプロパティがに`false`設定されている場合、マップをズームすることはできません。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map HasZoomEnabled="false" />
```

該当の C# コードを次に示します。

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>ユーザーの所在地を表示する

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、型[`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) `bool`のプロパティを定義します。 既定では、この`false`プロパティはです。これは、マップにユーザーの現在の場所が表示されないことを示します。 このプロパティがに`true`設定されている場合、マップにはユーザーの現在の場所が表示されます。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map IsShowingUser="true" />
```

該当の C# コードを次に示します。

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> IOS、Android、およびユニバーサル Windows プラットフォームでは、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が付与されている必要があります。 詳細については、「[プラットフォームの構成](setup.md#platform-configuration)」を参照してください。

### <a name="maintain-map-region-on-layout-change"></a>レイアウトの変更時にマップ領域を維持する

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、型`MoveToLastRegionOnLayoutChange` `bool`のプロパティを定義します。 既定では、この`true`プロパティはです。これは、デバイスの回転など、レイアウトの変更が発生したときに、表示されているマップ領域が現在の領域から以前に設定された領域に移動することを示します。 このプロパティがに`false`設定されている場合、レイアウトの変更が発生しても、表示されているマップ領域は中央のままになります。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

該当の C# コードを次に示します。

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>マップのクリック

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)は、マップ`MapClicked`がタップされたときに発生するイベントを定義します。 イベント`MapClickedEventArgs`に付随するオブジェクトには、型`Position` [`Position`](xref:Xamarin.Forms.Maps.Position)のという名前のプロパティが1つあります。 イベントが発生すると、 `Position`プロパティは、タップされたマップの場所に設定されます。 構造体の[`Position`](xref:Xamarin.Forms.Maps.Position)詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

次のコード例は、 `MapClicked`イベントのイベントハンドラーを示しています。

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

この例では、 `OnMapClicked`イベントハンドラーは、タップされたマップの場所を表す緯度と経度を出力します。 イベントハンドラーは、次のように`MapClicked`イベントに登録できます。

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

該当の C# コードを次に示します。

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [マップの位置と距離](position-distance.md)
- [Xamarin.Forms のマップのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [渡す (引数を XAML で)](~/xamarin-forms/xaml/passing-arguments.md)
