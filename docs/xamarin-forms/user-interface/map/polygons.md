---
title: Xamarin.Forms 多角形、ポリライン、および円のマップ
description: この記事では、マップインスタンスで多角形、ポリライン、および円を作成する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d4b82cc9c04dd711cc99b558475c2e003e557252
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91559780"
---
# <a name="no-locxamarinforms-map-polygons-and-polylines"></a>Xamarin.Forms 多角形とポリラインのマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

`Polygon`、、およびの各要素を使用すると、 `Polyline` `Circle` マップ上の特定の領域を強調表示できます。 は、 `Polygon` ストロークと塗りつぶしの色を持つことができる、完全に囲まれた形状です。 は、 `Polyline` 領域を完全に囲む線ではありません。 は、 `Circle` マップの円形の領域を強調表示します。

[!["IOS および Android でのマップの多角形とポリラインのスクリーンショット"](polygons-images/polygon-polyline.png "マップ上の多角形とポリライン")](polygons-images/polygon-polyline-large.png#lightbox "マップ上の多角形とポリライン") 
[ !["IOS および Android でのマップの円のスクリーンショット"](polygons-images/circle.png "マップ上の円")](polygons-images/circle-large.png#lightbox "マップ上の円")

`Polygon`、 `Polyline` 、およびの `Circle` 各クラスは、 `MapElement` 次のバインド可能なプロパティを公開するクラスから派生します。

- `StrokeColor``Color`線の色を決定するオブジェクトです。
- `StrokeWidth``float`線の幅を決定するオブジェクトです。

クラスは、 `Polygon` 追加のバインド可能なプロパティを定義します。

- `FillColor``Color`多角形の背景色を決定するオブジェクトです。

また、 `Polygon` `Polyline` クラスとクラスはどちらも `GeoPath` プロパティを定義します。これは、 [`Position`](xref:Xamarin.Forms.Maps.Position) 図形の点を指定するオブジェクトのリストです。

クラスは、 `Circle` 次のバインド可能なプロパティを定義します。

- `Center`[`Position`](xref:Xamarin.Forms.Maps.Position)緯度と経度の円の中心を定義するオブジェクトです。
- `Radius` は、 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 円の半径をメートル、キロメートル、またはマイル単位で定義するオブジェクトです。
- `FillColor` は、 `Color` 円の境界内の色を決定するプロパティです。

> [!NOTE]
> `StrokeColor`プロパティが指定されていない場合、ストロークは既定で黒に設定されます。 `FillColor`プロパティが指定されていない場合、塗りつぶしは既定で透過的になります。 したがって、どちらのプロパティも指定されていない場合、図形は塗りつぶされていない黒いアウトラインを持ちます。

## <a name="create-a-polygon"></a>多角形を作成する

`Polygon`オブジェクトをインスタンス化してマップのコレクションに追加することで、そのオブジェクトをマップに追加でき `MapElements` ます。 XAML では次のようにしてこれを実現できます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polygon StrokeColor="#FF9900"
                           StrokeWidth="8"
                           FillColor="#88FF9900">
                 <maps:Polygon.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6368678</x:Double>
                             <x:Double>-122.137305</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polygon.Geopath>
             </maps:Polygon>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};

// instantiate a polygon
Polygon polygon = new Polygon
{
    StrokeWidth = 8,
    StrokeColor = Color.FromHex("#1BA1E2"),
    FillColor = Color.FromHex("#881BA1E2"),
    Geopath =
    {
        new Position(47.6368678, -122.137305),
        new Position(47.6368894, -122.134655),
        new Position(47.6359424, -122.134655),
        new Position(47.6359496, -122.1325521),
        new Position(47.6424124, -122.1325199),
        new Position(47.642463,  -122.1338932),
        new Position(47.6406414, -122.1344833),
        new Position(47.6384943, -122.1361248),
        new Position(47.6372943, -122.1376912)
    }
};

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

`StrokeColor`プロパティと `StrokeWidth` プロパティは、多角形のアウトラインをカスタマイズするために指定されます。 `FillColor`プロパティ値はプロパティ値と一致します `StrokeColor` が、透明にするために指定されたアルファ値を持っています。これにより、基になるマップを図形で表示できるようになります。 プロパティには、 `GeoPath` 多角形の `Position` ポイントの地理的座標を定義するオブジェクトの一覧が含まれています。 `Polygon`オブジェクトは、のコレクションに追加されると、マップに表示され `MapElements` `Map` ます。

> [!NOTE]
> は、 `Polygon` 完全に囲まれた図形です。 最初と最後のポイントが一致しない場合は、自動的に接続されます。

## <a name="create-a-polyline"></a>ポリラインを作成する

`Polyline`オブジェクトをインスタンス化してマップのコレクションに追加することで、そのオブジェクトをマップに追加でき `MapElements` ます。 XAML では次のようにしてこれを実現できます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polyline StrokeColor="Blue"
                            StrokeWidth="12">
                 <maps:Polyline.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6381401</x:Double>
                             <x:Double>-122.1317367</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polyline.Geopath>
             </maps:Polyline>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
// instantiate a polyline
Polyline polyline = new Polyline
{
    StrokeColor = Color.Blue,
    StrokeWidth = 12,
    Geopath =
    {
        new Position(47.6381401, -122.1317367),
        new Position(47.6381473, -122.1350841),
        new Position(47.6382847, -122.1353094),
        new Position(47.6384582, -122.1354703),
        new Position(47.6401136, -122.1360819),
        new Position(47.6403883, -122.1364681),
        new Position(47.6407426, -122.1377019),
        new Position(47.6412558, -122.1404056),
        new Position(47.6414148, -122.1418647),
        new Position(47.6414654, -122.1432702)
    }
};

// add the polyline to the map's MapElements collection
map.MapElements.Add(polyline);
```

`StrokeColor`および `StrokeWidth` プロパティは、線をカスタマイズするために指定されます。 プロパティには、 `GeoPath` `Position` ポリラインポイントの地理的座標を定義するオブジェクトの一覧が含まれています。 `Polyline`オブジェクトは、のコレクションに追加されると、マップに表示され `MapElements` `Map` ます。

## <a name="create-a-circle"></a>円を作成する

`Circle`オブジェクトをインスタンス化してマップのコレクションに追加することで、そのオブジェクトをマップに追加でき `MapElements` ます。 XAML では次のようにしてこれを実現できます。

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
      <maps:Map>
          <maps:Map.MapElements>
              <maps:Circle StrokeColor="#88FF0000"
                           StrokeWidth="8"
                           FillColor="#88FFC0CB">
                  <maps:Circle.Center>
                      <maps:Position>
                          <x:Arguments>
                              <x:Double>37.79752</x:Double>
                              <x:Double>-122.40183</x:Double>
                          </x:Arguments>
                      </maps:Position>
                  </maps:Circle.Center>
                  <maps:Circle.Radius>
                      <maps:Distance>
                          <x:Arguments>
                              <x:Double>250</x:Double>
                          </x:Arguments>
                      </maps:Distance>
                  </maps:Circle.Radius>
              </maps:Circle>             
          </maps:Map.MapElements>
          ...
      </maps:Map>
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map();

// Instantiate a Circle
Circle circle = new Circle
{
    Center = new Position(37.79752, -122.40183),
    Radius = new Distance(250),
    StrokeColor = Color.FromHex("#88FF0000"),
    StrokeWidth = 8,
    FillColor = Color.FromHex("#88FFC0CB")
};

// Add the Circle to the map's MapElements collection
map.MapElements.Add(circle);
```

マップ上のの場所 `Circle` は、プロパティとプロパティの値によって決まり `Center` `Radius` ます。 プロパティは、 `Center` 円の中心 (緯度と経度) を定義し、 `Radius` プロパティは円の半径をメートル単位で定義します。 `StrokeColor`および `StrokeWidth` プロパティは、円の輪郭をカスタマイズするために指定されます。 `FillColor`プロパティ値は、円の境界内の色を指定します。 どちらの色の値も、アルファチャネルを指定します。これにより、基になるマップを円で見ることができます。 `Circle`オブジェクトは、のコレクションに追加されると、マップに表示され `MapElements` `Map` ます。

> [!NOTE]
> `GeographyUtils`クラスには、 `ToCircumferencePositions` `Circle` オブジェクト (およびプロパティの値を定義する `Center` ) を、 `Radius` `Position` 円の境界の緯度と経度の座標を構成するオブジェクトのリストに変換する拡張メソッドがあります。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](/samples/xamarin/xamarin-forms-samples/workingwithmaps)