---
title: Xamarin. フォームマップコントロール
description: マップコントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 1cfda90360557af1160d421f18807f8b534967a8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426349"
---
# <a name="xamarinforms-map-control"></a>Xamarin. フォームマップコントロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。

[![IOS と Android でのマップコントロールのスクリーンショット](map-images/map-default.png "マップコントロール")](map-images/map-default-large.png#lightbox "マップコントロール")

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、マップの外観と動作を制御する次のプロパティを定義します。

- `bool`型の[`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)は、マップにユーザーの現在の場所が表示されているかどうかを示します。
- `IEnumerable`型の[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)。表示される `IEnumerable` 項目のコレクションを指定します。
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の[`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)。表示されている項目のコレクション内の各項目に適用する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を指定します。
- [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)型の `ItemTemplateSelector`。実行時に項目の[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択するために使用される[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を指定します。
- `bool`型の[`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)は、マップのスクロールが許可されているかどうかを判断します。
- `bool`型の[`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)は、マップのズームを許可するかどうかを決定します。
- `IList<MapElement>`型の `MapElements`は、多角形やポリラインなど、マップ上の要素のリストを表します。
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)型の[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)は、マップの表示スタイルを示します。
- `bool`型の `MoveToLastRegionOnLayoutChange`、レイアウトの変更が発生したときに、表示されているマップ領域を現在の領域から以前に設定した領域に移動するかどうかを制御します。
- `IList<Pin>`型の[`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)は、マップ上のピンの一覧を表します。
- [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)型の[`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)は、現在表示されているマップの領域を返します。

これらのプロパティは、`MapElements`、`Pins`、および `VisibleRegion` プロパティを除き、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットになる可能性があることを意味します。

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、マップがタップされたときに発生する `MapClicked` イベントも定義します。 イベントに付随する `MapClickedEventArgs` オブジェクトには、 [`Position`](xref:Xamarin.Forms.Maps.Position)型の `Position`という名前のプロパティが1つあります。 イベントが発生すると、`Position` プロパティが、タップされたマップの場所に設定されます。 [`Position`](xref:Xamarin.Forms.Maps.Position)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)、 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)、および `ItemTemplateSelector` プロパティの詳細については、「 [pin コレクションの表示](pins.md#display-a-pin-collection)」を参照してください。

## <a name="display-a-map"></a>マップを表示する

[`Map`](xref:Xamarin.Forms.Maps.Map)は、レイアウトまたはページに追加することによって表示できます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> 追加の `xmlns` 名前空間の定義は、Xamarin. Forms. マップコントロールを参照するために必要です。 前の例では、`Xamarin.Forms.Maps` 名前空間が `maps` キーワードを通じて参照されています。

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

この例では、既定の[`Map`](xref:Xamarin.Forms.Maps.Map)コンストラクターを呼び出します。これにより、ローマでマップが中心になります。

[![IOS と Android での既定の場所を使用したマップコントロールのスクリーンショット](map-images/map-default.png "既定の場所でのマップコントロール")](map-images/map-default-large.png#lightbox "既定の場所でのマップコントロール")

または、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)引数を[`Map`](xref:Xamarin.Forms.Maps.Map)コンストラクターに渡して、マップが読み込まれたときの中心点とズームレベルを設定することもできます。 詳細については、「[マップに特定の場所を表示する](#display-a-specific-location-on-a-map)」を参照してください。

## <a name="map-types"></a>マップの種類

[`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティを[`MapType`](xref:Xamarin.Forms.Maps.MapType)列挙メンバーに設定すると、マップの表示スタイルを定義できます。 `MapType` 列挙体を使って、次のメンバーを定義できます。

- `Street` は、道路マップが表示されることを指定します。
- `Satellite` は、衛星画像を含むマップが表示されることを指定します。
- `Hybrid` は、道路と衛星のデータを組み合わせたマップが表示されることを指定します。

既定では、 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティが定義されていない場合、 [`Map`](xref:Xamarin.Forms.Maps.Map)にはストリートマップが表示されます。 または、`MapType` プロパティを[`MapType`](xref:Xamarin.Forms.Maps.MapType) 列挙型のメンバーのいずれかに設定することもできます。

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

次のスクリーンショットは、 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティが `Street`に設定されている場合の[`Map`](xref:Xamarin.Forms.Maps.Map)を示しています。

[![IOS と Android でのマップコントロールのスクリーンショット (ストリートマップの種類)](map-images/maptype-street.png "ストリート maptype によるマップコントロール")](map-images/maptype-street-large.png#lightbox "Map control with the street map type")

次のスクリーンショットは、 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティが `Satellite`に設定されている場合の[`Map`](xref:Xamarin.Forms.Maps.Map)を示しています。

[![IOS と Android での、衛星マップの種類を使用したマップコントロールのスクリーンショット](map-images/maptype-satellite.png "サテライト maptype を使用したマップコントロール")](map-images/maptype-satellite-large.png#lightbox "Map control with the satellite map type")

次のスクリーンショットは、 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)プロパティが `Hybrid`に設定されている場合の[`Map`](xref:Xamarin.Forms.Maps.Map)を示しています。

[![IOS と Android での、ハイブリッドマップの種類を使用したマップコントロールのスクリーンショット](map-images/maptype-hybrid.png "ハイブリッド maptype によるマップコントロール")](map-images/maptype-hybrid-large.png#lightbox "Map control with the hybrid map type")

## <a name="display-a-specific-location-on-a-map"></a>マップ上の特定の場所を表示する

マップの読み込み時に表示するマップの領域は、 [`Map`](xref:Xamarin.Forms.Maps.Map)コンストラクターに[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)引数を渡すことによって設定できます。

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

この例では、 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)オブジェクトによって指定された領域を示す[`Map`](xref:Xamarin.Forms.Maps.Map)オブジェクトを作成します。 `MapSpan` オブジェクトは、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトによって表される緯度と経度の中央にあり、0.01 緯度と0.01 経度の角度にまたがります。 [`Position`](xref:Xamarin.Forms.Maps.Position)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。 XAML で引数を渡す方法の詳細については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

結果として、マップが表示されると、特定の位置に中央に配置され、緯度と経度の角度が特定の数にまたがります。

[![指定された場所のマップコントロールのスクリーンショット (iOS と Android)](map-images/map-region.png "指定した位置にコントロールをマップする")](map-images/map-region-large.png#lightbox "指定した位置にコントロールをマップする")

## <a name="create-a-mapspan-object"></a>MapSpan オブジェクトを作成する

[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)オブジェクトを作成するには、いくつかの方法があります。 一般的な方法として、`MapSpan` コンストラクターに必要な引数を指定します。 これらは、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトによって表される緯度と経度で、`MapSpan`によっての緯度と経度の角度を表す値 `double` ます。 [`Position`](xref:Xamarin.Forms.Maps.Position)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

また、新しい `MapSpan` オブジェクトを返す[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)クラスには、次の3つのメソッドがあります。

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)は、メソッドのクラスインスタンスと同じ `LongitudeDegrees` を持つ `MapSpan` と、その `north` および `south` 引数によって定義された半径を返します。
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)は、 [`Position`](xref:Xamarin.Forms.Maps.Position)引数と[`Distance`](xref:Xamarin.Forms.Maps.Distance)引数によって定義された `MapSpan` を返します。
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)は、メソッドのクラスインスタンスと同じ中心を持つ `MapSpan` を返しますが、radius の `double` 引数を乗算します。

[`Distance`](xref:Xamarin.Forms.Maps.Distance)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)が作成されると、次のプロパティにアクセスしてデータを取得できます。

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)。 `MapSpan`の地理的な中央の[`Position`](xref:Xamarin.Forms.Maps.Position)を表します。
- `MapSpan`によってスパンされる緯度の角度を表す[`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)。
- `MapSpan`によってスパンされている経度の角度を表す[`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)。
- `MapSpan` radius を表す[`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)。

## <a name="move-the-map"></a>マップを移動する

マップの位置とズームレベルを変更するには、 [`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドを呼び出すことができます。 このメソッドは、表示するマップの領域とそのズームレベルを定義する[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)引数を受け取ります。

次のコードは、マップ上の表示されている領域を移動する例を示しています。

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>マップをズームする

[`Map`](xref:Xamarin.Forms.Maps.Map)のズームレベルは、その場所を変更することなく変更できます。 これは、マップ UI を使用するか、現在の場所を[`Position`](xref:Xamarin.Forms.Maps.Position)引数として使用する[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)引数を使用して[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドを呼び出すことによって実行できます。

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

この例では、 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドを、 [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)プロパティを使用してマップの現在の場所を指定する[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)引数と、緯度と経度の角度としてズームレベルを指定して呼び出されます。 全体的な結果として、マップのズームレベルは変更されますが、その場所は変更されません。 マップにズームを実装する別の方法として、 [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)メソッドを使用してズームファクターを制御する方法があります。

> [!IMPORTANT]
> マップをズームする (マップ UI またはプログラムによって) 必要がある場合は、 [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)プロパティが `true`である必要があります。 このプロパティの詳細については、「 [Zoom を無効にする](#disable-zoom)」を参照してください。

## <a name="customize-map-behavior"></a>マップ動作のカスタマイズ

[`Map`](xref:Xamarin.Forms.Maps.Map)の動作は、そのプロパティの一部を設定し、`MapClicked` イベントを処理することによってカスタマイズできます。

> [!NOTE]
> マップのカスタムレンダラーを作成することによって、追加のマップ動作のカスタマイズを実現できます。 詳細については、「 [Xamarin のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)」を参照してください。

### <a name="disable-scroll"></a>スクロールを無効にする

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、`bool`型の[`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)プロパティを定義します。 既定では、このプロパティは `true`であり、マップのスクロールが許可されていることを示します。 このプロパティが `false`に設定されている場合、マップはスクロールしません。 次の例は、このプロパティを設定する方法を示しています。

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

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、`bool`型の[`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)プロパティを定義します。 既定では、このプロパティは `true`です。これは、マップ上でズームを実行できることを示します。 このプロパティが `false`に設定されている場合、マップをズームすることはできません。 次の例は、このプロパティを設定する方法を示しています。

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

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、`bool`型の[`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)プロパティを定義します。 既定では、このプロパティは `false`です。これは、マップにユーザーの現在の場所が表示されないことを示します。 このプロパティが `true`に設定されている場合、マップにユーザーの現在の場所が表示されます。 次の例は、このプロパティを設定する方法を示しています。

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
> iOS、Android、およびユニバーサル Windows プラットフォームでは、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が付与されている必要があります。 詳細については、「[プラットフォームの構成](setup.md#platform-configuration)」を参照してください。

### <a name="maintain-map-region-on-layout-change"></a>レイアウトの変更時にマップ領域を維持する

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、`bool`型の `MoveToLastRegionOnLayoutChange` プロパティを定義します。 既定では、このプロパティは `true` です。これは、デバイスの回転など、レイアウトの変更が発生したときに、表示されているマップ領域が現在の領域から以前に設定された領域に移動することを示します。 このプロパティが `false` に設定されている場合、レイアウトの変更が発生しても、表示されているマップ領域は中央のままになります。 次の例は、このプロパティを設定する方法を示しています。

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

[`Map`](xref:Xamarin.Forms.Maps.Map)クラスは、マップがタップされたときに発生する `MapClicked` イベントを定義します。 イベントに付随する `MapClickedEventArgs` オブジェクトには、 [`Position`](xref:Xamarin.Forms.Maps.Position)型の `Position`という名前のプロパティが1つあります。 イベントが発生すると、`Position` プロパティが、タップされたマップの場所に設定されます。 [`Position`](xref:Xamarin.Forms.Maps.Position)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

次のコード例は、`MapClicked` イベントのイベントハンドラーを示しています。

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

この例では、`OnMapClicked` イベントハンドラーが、タップされたマップの場所を表す緯度と経度を出力します。 イベントハンドラーは、次のように `MapClicked` イベントに登録できます。

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
- [Xamarin.Forms マップのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
