---
title: Xamarin. フォームマップの多角形とポリライン
description: この記事では、Xamarin. Forms map インスタンスで多角形とポリラインを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/20/2019
ms.openlocfilehash: b45a7af917e9147f519056ee5a9e5d06da51113a
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697663"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin. フォームマップの多角形とポリライン

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[iOS と Android での "多角形とポリラインのデモ" を ![](polygons-images/polygon-app-cropped.png)](polygons-images/polygon-app.png#lightbox)

`Polygon` 要素と `Polyline` 要素を使用すると、マップ上の特定の領域を強調表示できます。 @No__t_0 は、ストロークと塗りつぶしの色を持つ、完全に囲まれた図形です。 @No__t_0 は、領域を完全に囲む線ではありません。

> [!NOTE]
> @No__t_0 と `Polyline` の例については、サンプルプロジェクトの**PolygonsPage**を参照してください。

@No__t_0 クラスと `Polyline` クラスは `MapElement` から派生し、次の[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)プロパティを公開します。

- `StrokeColor` は、線の色を決定する `Color` のプロパティです。
- `StrokeWidth` は、線の幅を決定する `float` のプロパティです。
- `Geopath` は `Polygon` と `Polyline` の両方で定義され、図形のポイントを指定する[`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトの一覧です。

@No__t_0 クラスは、追加のプロパティを定義します。

- `FillColor` は、多角形の背景色を決定する `Color` プロパティです。

> [!NOTE]
> @No__t_0 プロパティが指定されていない場合、ストロークは既定で黒に設定されます。 @No__t_0 プロパティが指定されていない場合、塗りつぶしは既定で透過的になります。 したがって、どちらのプロパティも指定されていない場合、図形は塗りつぶされていない黒いアウトラインを持ちます。

## <a name="create-a-polygon"></a>多角形を作成する

@No__t_0 オブジェクトをインスタンス化し、マップの `MapElements` コレクションに追加することによって、マップに追加できます。

```csharp
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
}

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

XAML では、`Polygon` を作成することもできます。

```xaml
<maps:Map x:Name="map">
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
```

多角形のアウトラインをカスタマイズするには、`StrokeColor` プロパティと `StrokeWidth` プロパティを指定します。 @No__t_0 プロパティ値は `StrokeColor` プロパティ値と一致しますが、透明にするために指定されたアルファ値を持っています。これにより、基になるマップを図形で表示できるようになります。 @No__t_0 プロパティには、多角形のポイントの地理的座標を定義する `Position` オブジェクトの一覧が含まれています。 @No__t_0 オブジェクトは、`Map` の `MapElements` コレクションに追加された後、マップに表示されます。

> [!NOTE]
> @No__t_0 は、完全に囲まれた図形です。 最初と最後のポイントが一致しない場合は、自動的に接続されます。

## <a name="create-a-polyline"></a>ポリラインを作成する

@No__t_0 オブジェクトをインスタンス化し、マップの `MapElements` コレクションに追加することによって、マップに追加できます。

```csharp
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

XAML では、`Polyline` を作成することもできます。

```xaml
<maps:Map x:Name="map">
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
```

線をカスタマイズするには、`StrokeColor` と `StrokeWidth` のプロパティを指定します。 @No__t_0 プロパティには、ポリラインポイントの地理的座標を定義する `Position` オブジェクトの一覧が含まれています。 @No__t_0 オブジェクトは、`Map` の `MapElements` コレクションに追加された後、マップに表示されます。

## <a name="related-links"></a>関連リンク

- [サンプルマッププロジェクト](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin. Forms マップ](~/xamarin-forms/user-interface/map/index.md)
