---
title: マップ上の円形の領域を強調表示
description: この記事では、円形のマップの領域を強調表示する、マップに循環オーバーレイを追加する方法について説明します。 IOS および Android、Api が循環のオーバーレイをマップに追加するため、UWP のでは、オーバーレイが多角形としてレンダリングされます。
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 3064296d4c78a3342fb27afc971c37a029987e5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998558"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>マップ上の円形の領域を強調表示

_この記事では、円形のマップの領域を強調表示する、マップに循環オーバーレイを追加する方法について説明します。_

## <a name="overview"></a>概要

オーバーレイは、マップ上の複数層の図です。 オーバーレイに拡大されると、マップを使用して拡大/縮小するグラフィカル コンテンツを描画をサポートします。 次のスクリーン ショットは、マップへの循環のオーバーレイの追加の結果を表示します。

![](circle-map-overlay-images/screenshots.png)

ときに、 [ `Map` ](xref:Xamarin.Forms.Maps.Map) iOS での Xamarin.Forms アプリケーションでコントロールが表示される、`MapRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`MKMapView`コントロール。 Android のプラットフォームで、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapControl`します。 レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のマップのカスタマイズを実装する`Map`各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms のカスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [カスタマイズ](#Customizing_the_Map)各プラットフォームで、マップのカスタム レンダラーを作成してマップします。

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 初期化して使用する前に構成されている必要があります。 詳細については、次を参照してください。 [`Maps Control`](~/xamarin-forms/user-interface/map.md)

カスタム レンダラーを使用してマップをカスタマイズする方法については、次を参照してください。[マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)します。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップを作成します。

作成、`CustomCircle`を持つクラス`Position`と`Radius`プロパティ。

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

サブクラスを作成し、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)型のプロパティを追加するには、クラス`CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>カスタム マップの使用

使用、 `CustomMap` XAML ページのインスタンスでそのインスタンスを宣言することで制御します。

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

または、使用、 `CustomMap` c# ページ インスタンスでそのインスタンスを宣言することによってコントロール。

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

初期化、`CustomMap`必要に応じて制御します。

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

この初期化を追加[ `Pin` ](xref:Xamarin.Forms.Maps.Pin)と`CustomCircle`カスタム マップにインスタンスし、のマップのビューを配置、 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドは、位置とズームを変更します。作成して、マップのレベルを[ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan)から、 [ `Position` ](xref:Xamarin.Forms.Maps.Position)と[ `Distance`](xref:Xamarin.Forms.Maps.Distance)します。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップのカスタマイズ

カスタム レンダラーは、マップに循環のオーバーレイを追加するには、各アプリケーション プロジェクトにようになりました追加する必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`循環オーバーレイを追加する方法。

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

このメソッドは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることに、次の構成を実行します。

- `MKMapView.OverlayRenderer`プロパティが対応するデリゲートに設定します。
- 静的なを設定して円が作成される`MKCircle`メートル単位で、円の中心と円の半径を指定するオブジェクト。
- 円は、呼び出すことによって、マップに追加されます、`MKMapView.AddOverlay`メソッド。

次に、実装、`GetOverlayRenderer`オーバーレイの表示をカスタマイズする方法。

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`と`OnMapReady`循環オーバーレイを追加する方法。

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

`OnElementChanged`メソッドの呼び出し、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること、ビューに関連付けられているです。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`円がインスタンス化して作成されている、メソッドが呼び出されます、`CircleOptions`メートル単位で、円の中心と円の半径を指定するオブジェクト。 円が呼び出すことによって、マップに追加し、`NativeMap.AddCircle`メソッド。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームでのカスタム レンダラーの作成

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`循環オーバーレイを追加する方法。

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

このメソッドは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることに、次の操作を実行します。

- 円の位置と半径がから取得した、`CustomMap.Circle`プロパティに渡されると、`GenerateCircleCoordinates`緯度と経度を生成するメソッドは、円の境界の座標。 このヘルパー メソッドのコードは、以下に示します。
- 円の境界の座標に変換する`List`の`BasicGeoposition`座標。
- 円がインスタンス化によって作成された、`MapPolygon`オブジェクト。 `MapPolygon`マルチポイント図形を設定して、マップに表示するクラスが使用されるその`Path`プロパティを`Geopath`図形の座標を格納しているオブジェクト。
- 多角形は、マップ上に追加することによって表示される、`MapControl.MapElements`コレクション。


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

この記事では、円形のマップの領域を強調表示する、マップに循環オーバーレイを追加する方法について説明します。


## <a name="related-links"></a>関連リンク

- [円形のマップ Ovlerlay (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
