---
title: マップ上の円形の領域を強調表示
description: この記事では、円形のマップの領域を強調表示する、マップに循環オーバーレイを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 1bec7a318bebc40c050104a51408473d89f483a5
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846524"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>マップ上の円形の領域を強調表示

_この記事では、円形のマップの領域を強調表示する、マップに循環オーバーレイを追加する方法について説明します。_

## <a name="overview"></a>概要

オーバーレイは、マップ上の複数層グラフィックです。 オーバーレイに拡大されると、マップされた拡大縮小描画のグラフィカルなコンテンツをサポートします。 次のスクリーン ショットは、マップに循環オーバーレイを追加する結果を表示します。

![](circle-map-overlay-images/screenshots.png)

ときに、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)コントロールが iOS での Xamarin.Forms アプリケーションによって表示される、`MapRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`MKMapView`コントロール。 Android のプラットフォームでは、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapControl`します。 表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のマップのカスタマイズ設定を実装する、`Map`プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms カスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [カスタマイズ](#Customizing_the_Map)各プラットフォームで、マップのカスタム レンダラーを作成することでマップします。

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") 初期化して使用する前に構成されている必要があります。 詳細については、次を参照してください。 [`Maps Control`](~/xamarin-forms/user-interface/map.md)

カスタム レンダラーを使用してマップをカスタマイズする方法の詳細については、次を参照してください。[マップ暗証番号 (pin) をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)です。

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

次のサブクラスを作成、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)型のプロパティを追加するクラス`CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>カスタムのマップの使用

使用する、 `CustomMap` XAML ページ インスタンスでそのインスタンスを宣言することによって制御します。

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

または、使用、 `CustomMap` c# ページ インスタンスでそのインスタンスを宣言することによって制御します。

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

初期化、`CustomMap`必要に応じてを制御します。

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

この初期化を追加[ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/)と`CustomCircle`カスタムのマップをインスタンス化されでマップのビューが置か、 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/)位置とズームを変更する方法作成することで、マップのレベル、 [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/)から、 [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)と[ `Distance`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)です。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップをカスタマイズします。

カスタム レンダラーをマップに循環オーバーレイを追加するには、各アプリケーション プロジェクトに追加されます必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

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

このメソッドは、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされている、次の構成を実行します。

- `MKMapView.OverlayRenderer`プロパティが対応するデリゲートに設定します。
- 円が静的なを設定して作成した`MKCircle`メートル単位で、円の中心と、円の半径を指定するオブジェクト。
- 円が呼び出しによって、マップに追加される、`MKMapView.AddOverlay`メソッドです。

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

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

`OnElementChanged`メソッドの呼び出し、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`が関連付けられているビューには、カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされています。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`円がインスタンス化して作成される場所、メソッドが呼び出されます、`CircleOptions`メートル単位で、円の中心と、円の半径を指定するオブジェクト。 円が呼び出すことによって、マップに追加し、`NativeMap.AddCircle`メソッドです。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームには、カスタム レンダラーを作成します。

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

このメソッドは、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされている、次の操作を実行します。

- 円を位置および radius がから取得、`CustomMap.Circle`プロパティに渡されると、`GenerateCircleCoordinates`緯度と経度を生成するメソッドの円の境界の座標。 このヘルパー メソッドのコードは、以下に示します。
- 円の境界の座標に変換する`List`の`BasicGeoposition`座標。
- 円がインスタンス化して作成された、`MapPolygon`オブジェクト。 `MapPolygon`マルチポイント図形を設定して、マップに表示するクラスが使用されるその`Path`プロパティを`Geopath`図形の座標を格納しているオブジェクト。
- 追加することによって、マップの多角形が表示される、`MapControl.MapElements`コレクション。


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
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
