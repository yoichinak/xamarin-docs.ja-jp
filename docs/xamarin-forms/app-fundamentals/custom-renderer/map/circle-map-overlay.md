---
title: マップ上での円形の領域の強調表示
description: この記事では、円形のオーバーレイをマップに追加して、マップの円形の領域を強調表示する方法について説明します。 iOS と Android ではマップにオーバーレイを追加するための API が用意されていますが、UWP ではオーバーレイは多角形としてレンダリングされます。
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 551dea5455ffd060d808aa11e8996c5984745fda
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771910"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>マップ上での円形の領域の強調表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-circle)

"_この記事では、円形のオーバーレイをマップに追加して、マップの円形の領域を強調表示する方法について説明します。_"

## <a name="overview"></a>概要

オーバーレイは、マップ上に重ねて配置されるグラフィックです。 オーバーレイでは、マップが拡大縮小されたときに、マップと一緒に拡大縮小されるグラフィカル コンテンツの描画がサポートされます。 次のスクリーン ショットに、マップに円形のオーバーレイを追加した結果を示します。

![](circle-map-overlay-images/screenshots.png)

Xamarin.Forms アプリケーションによって [`Map`](xref:Xamarin.Forms.Maps.Map) コントロールが レンダリングされると、iOS では `MapRenderer` クラスがインスタンス化され、それによってネイティブの `MKMapView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`MapRenderer` クラスによってネイティブの `MapView` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`MapRenderer` クラスによってネイティブの `MapControl` がインスタンス化されます。 レンダリング プロセスを活用して各プラットフォーム上で `Map` 用のカスタム レンダラーを作成することで、プラットフォーム固有のマップのカスタマイズを実装できます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms カスタム マップを[作成](#Creating_the_Custom_Map)します。
1. Xamarin.Forms からカスタム マップを[使用](#Consuming_the_Custom_Map)します。
1. 各プラットフォーム上でマップ用のカスタム レンダラーを作成することで、マップを[カスタマイズ](#Customizing_the_Map)します。

> [!NOTE]
> 使用する前に [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) を初期化して構成する必要があります。 詳細については、[`Maps Control`](~/xamarin-forms/user-interface/map.md)に関する記事を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法については、「[Customizing a Map Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)」(マップ ピンのカスタマイズ) を参照してください。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップの作成

`Position` プロパティと `Radius` プロパティがある `CustomCircle` クラスを作成します。

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

次に、`CustomCircle` 型のプロパティを追加する、[`Map`](xref:Xamarin.Forms.Maps.Map) のサブクラスを作成します。

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>カスタム マップの使用

XAML ページ インスタンス内にそのインスタンスを宣言することで、`CustomMap` コントロールを使用します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

または、C# ページ インスタンス内にそのインスタンスを宣言することで、`CustomMap` コントロールを使用します。

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

必要に応じて、`CustomMap` コントロールを初期化します。

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

この初期化では、[`Pin`](xref:Xamarin.Forms.Maps.Pin) インスタンスと `CustomCircle` インスタンスがカスタム マップに追加され、[`Position`](xref:Xamarin.Forms.Maps.Position) と [`Distance`](xref:Xamarin.Forms.Maps.Distance) から [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) を作成することでマップの位置とズーム レベルを変更する [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) メソッドを使用して、マップのビューが配置されます。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップのカスタマイズ

次に、マップに円形のオーバーレイを追加するために、各アプリケーション プロジェクトにカスタム レンダラーを追加する必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドをオーバーライドして、円形のオーバーレイを追加します。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

このメソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、次の構成が実行されます。

- `MKMapView.OverlayRenderer` プロパティが対応するデリゲートに設定されます。
- 円の中心と円の半径をメートル単位で指定する静的な `MKCircle` オブジェクトを設定することで、円が作成されます。
- `MKMapView.AddOverlay` メソッドを呼び出すことで、円がマップに追加されます。

次に、`GetOverlayRenderer` メソッドを実行して、オーバーレイのレンダリングをカスタマイズします。

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドと `OnMapReady` メソッドをオーバーライドして、円形のオーバーレイを追加します。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、`OnElementChanged` メソッドによって `MapView.GetMapAsync` メソッドが呼び出され、ビューに関連付けられている基になる `GoogleMap` が取得されます。 `GoogleMap` インスタンスが使用可能になると、`OnMapReady` メソッドが呼び出され、各ポイントの緯度と経度を指定する `CircleOptions` をインスタンス化することで、円形が作成されます。 `NativeMap.AddCircle` メソッドを呼び出すことで、円がマップに追加されます。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォーム上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドをオーバーライドして、円形のオーバーレイを追加します。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }
            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        // GenerateCircleCoordinates helper method (below)
    }
}
```

このメソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、次の操作が実行されます。

- `CustomMap.Circle` プロパティから円の位置と半径が 取得され、`GenerateCircleCoordinates` メソッドに渡され、円の境界の緯度と経度の座標が生成されます。 このヘルパー メソッドのコードを次に示します。
- 円の境界の座標が `BasicGeoposition` 座標の `List` に変換されます。
- `MapPolygon` オブジェクトをインスタンス化することで、円形が作成されます。 `MapPolygon` クラスを使用して、その `Path` プロパティを図形の座標が含まれる `Geopath` オブジェクトに設定することで、マップ上にマルチポイント図形が表示されます。
- `MapControl.MapElements` コレクションに追加することで、多角形がマップ上にレンダリングされます。

```
List<Position> GenerateCircleCoordinates(Position position, double radius)
{
    double latitude = position.Latitude.ToRadians();
    double longitude = position.Longitude.ToRadians();
    double distance = radius / EarthRadiusInMeteres;
    var positions = new List<Position>();

    for (int angle = 0; angle <=360; angle++)
    {
        double angleInRadians = ((double)angle).ToRadians();
        double latitudeInRadians = Math.Asin(Math.Sin(latitude) * Math.Cos(distance) + Math.Cos(latitude) * Math.Sin(distance) * Math.Cos(angleInRadians));
        double longitudeInRadians = longitude + Math.Atan2(Math.Sin(angleInRadians) * Math.Sin(distance) * Math.Cos(latitude), Math.Cos(distance) - Math.Sin(latitude) * Math.Sin(latitudeInRadians));

        var pos = new Position(latitudeInRadians.ToDegrees(), longitudeInRadians.ToDegrees());
        positions.Add(pos);
    }

    return positions;
}
```

## <a name="summary"></a>まとめ

この記事では、円形のオーバーレイをマップに追加し、マップの円形の領域を強調表示する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [円形のマップのオーバーレイ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-circle)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
