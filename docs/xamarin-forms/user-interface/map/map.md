---
title: Xamarin.Formsマップ コントロール
description: マップコントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1aee81b6988e1f3a7099c2722b6f336f071ad8c0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946365"
---
# <a name="xamarinforms-map-control"></a>Xamarin.Formsマップ コントロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

コントロールは、 [`Map`](xref:Xamarin.Forms.Maps.Map) マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。

[![IOS と Android でのマップコントロールのスクリーンショット](map-images/map-default.png "マップ コントロール")](map-images/map-default-large.png#lightbox "マップ コントロール")

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) マップの外観と動作を制御する次のプロパティを定義します。

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)型のは、 `bool` マップにユーザーの現在の場所が表示されているかどうかを示します。
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)`IEnumerable`表示する項目のコレクションを指定する型の。 `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)型の。 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) これは、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 表示されている項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`型の。 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) これは、 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 実行時に項目のを選択するために使用されるを指定します。
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)型ので、 `bool` マップのスクロールが許可されているかどうかを判断します。
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)型のは、 `bool` マップのズームを許可するかどうかを決定します。
- `MapElements`型のは、 `IList<MapElement>` 多角形やポリラインなど、マップ上の要素のリストを表します。
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)型のは、 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) マップの表示スタイルを示します。
- `MoveToLastRegionOnLayoutChange`型の `bool` 。レイアウトの変更が発生したときに、表示されているマップ領域を現在の領域から前の領域に移動するかどうかを制御します。
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)型のは、 `IList<Pin>` マップ上のピンのリストを表します。
- `TrafficEnabled`型のは、 `bool` トラフィックデータをマップに重ねて表示するかどうかを示します。
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)型のは、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 現在表示されているマップの領域を返します。

これらのプロパティは `MapElements` 、、、およびの各プロパティを除き、 `Pins` `VisibleRegion` オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットになる可能性があることを意味します。

[`Map`](xref:Xamarin.Forms.Maps.Map)また、クラスは、 `MapClicked` マップがタップされたときに発生するイベントも定義します。 `MapClickedEventArgs`イベントに付随するオブジェクトには、型のという名前のプロパティが1つあり `Position` [`Position`](xref:Xamarin.Forms.Maps.Position) ます。 イベントが発生すると、 `Position` プロパティは、タップされたマップの場所に設定されます。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「[マップの位置と距離](position-distance.md)」を参照してください。

、、およびの各プロパティの詳細については [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) `ItemTemplateSelector` 、「 [pin コレクションの表示](pins.md#display-a-pin-collection)」を参照してください。

## <a name="display-a-map"></a>マップを表示する

は、 [`Map`](xref:Xamarin.Forms.Maps.Map) レイアウトまたはページに追加することによって表示できます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> を `xmlns` 参照するには、追加の名前空間定義が必要です Xamarin.Forms 。コントロールをマップします。 前の例では、 `Xamarin.Forms.Maps` キーワードによって名前空間が参照されてい `maps` ます。

これに相当する C# コードを次に示します。

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

この例では、既定のコンストラクターを呼び出します。これにより、 [`Map`](xref:Xamarin.Forms.Maps.Map) ローマでマップが中心になります。

[![IOS と Android での既定の場所を使用したマップコントロールのスクリーンショット](map-images/map-default.png "既定の場所でのマップコントロール")](map-images/map-default-large.png#lightbox "既定の場所でのマップコントロール")

または、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 引数をコンストラクターに渡し [`Map`](xref:Xamarin.Forms.Maps.Map) て、マップの読み込み時にマップの中心点とズームレベルを設定することもできます。 詳細については、「[マップに特定の場所を表示する](#display-a-specific-location-on-a-map)」を参照してください。

## <a name="map-types"></a>マップの種類

[`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティを列挙メンバーに設定すると、 [`MapType`](xref:Xamarin.Forms.Maps.MapType) マップの表示スタイルを定義できます。 `MapType` 列挙体を使って、次のメンバーを定義できます。

- `Street`道路地図が表示されることを指定します。
- `Satellite`サテライト画像を含むマップが表示されることを指定します。
- `Hybrid`道路と衛星のデータを組み合わせたマップが表示されることを指定します。

既定では、 [`Map`](xref:Xamarin.Forms.Maps.Map) プロパティが定義されていない場合、はストリートマップを表示し [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) ます。 また、 `MapType` プロパティを列挙型のメンバーの1つに設定することもでき [`MapType`](xref:Xamarin.Forms.Maps.MapType) ます。

```xaml
<maps:Map MapType="Satellite" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

次のスクリーンショットは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) プロパティがに設定されている場合を示してい `Street` ます。

[![IOS と Android でのマップコントロールのスクリーンショット (ストリートマップの種類)](map-images/maptype-street.png "ストリート maptype によるマップコントロール")](map-images/maptype-street-large.png#lightbox "ストリートマップの種類によるマップコントロール")

次のスクリーンショットは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) プロパティがに設定されている場合を示してい `Satellite` ます。

[![IOS と Android での、衛星マップの種類を使用したマップコントロールのスクリーンショット](map-images/maptype-satellite.png "サテライト maptype を使用したマップコントロール")](map-images/maptype-satellite-large.png#lightbox "衛星マップの種類を使用したマップコントロール")

次のスクリーンショットは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) プロパティがに設定されている場合を示してい `Hybrid` ます。

[![IOS と Android での、ハイブリッドマップの種類を使用したマップコントロールのスクリーンショット](map-images/maptype-hybrid.png "ハイブリッド maptype によるマップコントロール")](map-images/maptype-hybrid-large.png#lightbox "ハイブリッドマップの種類によるマップコントロール")

## <a name="display-a-specific-location-on-a-map"></a>マップ上の特定の場所を表示する

マップが読み込まれたときに表示するマップの領域は、コンストラクターに引数を渡すことによって設定でき [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) [`Map`](xref:Xamarin.Forms.Maps.Map) ます。

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

これに相当する C# コードを次に示します。

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

この例 [`Map`](xref:Xamarin.Forms.Maps.Map) では、オブジェクトによって指定された領域を示すオブジェクトを作成し [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) ます。 オブジェクトは、 `MapSpan` オブジェクトによって表される緯度と経度の中央に [`Position`](xref:Xamarin.Forms.Maps.Position) あり、0.01 緯度と0.01 経度の角度にまたがります。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「[マップの位置と距離](position-distance.md)」を参照してください。 XAML で引数を渡す方法の詳細については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

結果として、マップが表示されると、特定の位置に中央に配置され、緯度と経度の角度が特定の数にまたがります。

[![指定された場所のマップコントロールのスクリーンショット (iOS と Android)](map-images/map-region.png "指定した位置にコントロールをマップする")](map-images/map-region-large.png#lightbox "指定した位置にコントロールをマップする")

## <a name="create-a-mapspan-object"></a>MapSpan オブジェクトを作成する

オブジェクトを作成するには、いくつかの方法があり [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) ます。 一般的な方法では、コンストラクターに必須の引数を指定し `MapSpan` ます。 これらは、オブジェクトによって表される緯度と経度で、によっての [`Position`](xref:Xamarin.Forms.Maps.Position) `double` 緯度と経度の角度を表す値です `MapSpan` 。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「[マップの位置と距離](position-distance.md)」を参照してください。

また、クラスには、新しいオブジェクトを返すメソッドが3つあり [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) `MapSpan` ます。

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)`MapSpan` `LongitudeDegrees` メソッドのクラスインスタンスと同じを持つを返し、引数および引数によって定義された半径を返し `north` `south` ます。
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)`MapSpan`引数および引数によって定義されたを返し [`Position`](xref:Xamarin.Forms.Maps.Position) [`Distance`](xref:Xamarin.Forms.Maps.Distance) ます。
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)`MapSpan`メソッドのクラスインスタンスと同じ中心を持つを返します。ただし、radius の引数を乗算 `double` しています。

構造体の詳細については [`Distance`](xref:Xamarin.Forms.Maps.Distance) 、「[マップの位置と距離](position-distance.md)」を参照してください。

[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)が作成されると、次のプロパティにアクセスしてデータを取得できます。

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)[`Position`](xref:Xamarin.Forms.Maps.Position)。の地理的な中央のを表し `MapSpan` ます。
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)。によってスパンされる緯度の角度を表し `MapSpan` ます。
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)。によってスパンされる経度の角度を表し `MapSpan` ます。
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)。半径を表し `MapSpan` ます。

## <a name="move-the-map"></a>マップを移動する

[`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)マップの位置とズームレベルを変更するには、メソッドを呼び出すことができます。 このメソッドは、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 表示するマップの領域とそのズームレベルを定義する引数を受け取ります。

次のコードは、マップ上の表示されている領域を移動する例を示しています。

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>マップをズームする

のズームレベルは、 [`Map`](xref:Xamarin.Forms.Maps.Map) その場所を変更することなく変更できます。 これは、マップ UI を使用するか、 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 現在の場所を引数として使用する引数を指定してメソッドを呼び出すことによって実行できます [`Position`](xref:Xamarin.Forms.Maps.Position) 。

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

この例では、 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) プロパティを使用してマップの現在位置を指定する引数を使用してメソッドを呼び出し、 [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) 緯度と経度の角度としてズームレベルを指定します。 全体的な結果として、マップのズームレベルは変更されますが、その場所は変更されません。 マップにズームを実装する別の方法として、メソッドを使用してズームファクターを制御する方法が [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) あります。

> [!IMPORTANT]
> マップをズームする (マップ UI またはプログラムによって) 必要がある場合は、プロパティがである必要があり [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) `true` ます。 このプロパティの詳細については、「 [Zoom を無効にする](#disable-zoom)」を参照してください。

## <a name="customize-map-behavior"></a>マップ動作のカスタマイズ

の動作は、 [`Map`](xref:Xamarin.Forms.Maps.Map) そのプロパティの一部を設定し、イベントを処理することによってカスタマイズでき `MapClicked` ます。

> [!NOTE]
> マップのカスタムレンダラーを作成して、追加のマップ動作のカスタマイズを行うことができます。 詳細については、「 [ Xamarin.Forms マップのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)」を参照してください。

### <a name="show-traffic-data"></a>トラフィック データを表示する

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) `TrafficEnabled` 型のプロパティを定義 `bool` します。 既定では、このプロパティはです。これは `false` 、トラフィックデータがマップに重ねられないことを示します。 このプロパティがに設定されている場合 `true` 、トラフィックデータはマップに重ねられます。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map TrafficEnabled="true" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map
{
    TrafficEnabled = true
};
```

### <a name="disable-scroll"></a>スクロールを無効にする

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) 型のプロパティを定義 `bool` します。 既定では、このプロパティはです。これは `true` 、マップのスクロールが許可されていることを示します。 このプロパティがに設定されている場合 `false` 、マップはスクロールしません。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map HasScrollEnabled="false" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>ズームを無効にする

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 型のプロパティを定義 `bool` します。 既定では、このプロパティはです。これは、 `true` マップ上でズームを実行できることを示します。 このプロパティがに設定されている場合 `false` 、マップをズームすることはできません。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map HasZoomEnabled="false" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>ユーザーの所在地を表示する

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) 型のプロパティを定義 `bool` します。 既定では、このプロパティはです。これは、 `false` マップにユーザーの現在の場所が表示されないことを示します。 このプロパティがに設定されている場合 `true` 、マップにはユーザーの現在の場所が表示されます。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map IsShowingUser="true" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> IOS、Android、およびユニバーサル Windows プラットフォームでは、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が付与されている必要があります。 詳細については、「[プラットフォームの構成](setup.md#platform-configuration)」を参照してください。

### <a name="maintain-map-region-on-layout-change"></a>レイアウトの変更時にマップ領域を維持する

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) `MoveToLastRegionOnLayoutChange` 型のプロパティを定義 `bool` します。 既定では、このプロパティはです。これは `true` 、デバイスの回転など、レイアウトの変更が発生したときに、表示されているマップ領域が現在の領域から以前に設定された領域に移動することを示します。 このプロパティがに設定されている場合 `false` 、レイアウトの変更が発生しても、表示されているマップ領域は中央のままになります。 次の例は、このプロパティを設定する方法を示しています。

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>マップのクリック

クラスは、 [`Map`](xref:Xamarin.Forms.Maps.Map) `MapClicked` マップがタップされたときに発生するイベントを定義します。 `MapClickedEventArgs`イベントに付随するオブジェクトには、型のという名前のプロパティが1つあり `Position` [`Position`](xref:Xamarin.Forms.Maps.Position) ます。 イベントが発生すると、 `Position` プロパティは、タップされたマップの場所に設定されます。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「[マップの位置と距離](position-distance.md)」を参照してください。

次のコード例は、イベントのイベントハンドラーを示してい `MapClicked` ます。

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

この例では、イベントハンドラーは、タップされた `OnMapClicked` マップの場所を表す緯度と経度を出力します。 イベントハンドラーは、次のようにイベントに登録でき `MapClicked` ます。

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

これに相当する C# コードを次に示します。

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [マップの位置と距離](position-distance.md)
- [マップのカスタマイズ Xamarin.Forms](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
