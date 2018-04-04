---
title: マップ上のルートを強調表示
description: この記事では、マップに多角形のオーバーレイを追加する方法について説明します。 ポリライン オーバーレイは、通常は、マップ上のルートの表示を切り替えるために必要な任意の図形をフォームに使用した接続されている直線セグメントの系列です。
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: f781a472a63d97c8859aff36b28e0fd4fa0c7756
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="highlighting-a-route-on-a-map"></a>マップ上のルートを強調表示

_この記事では、マップに多角形のオーバーレイを追加する方法について説明します。ポリライン オーバーレイは、通常は、マップ上のルートの表示を切り替えるために必要な任意の図形をフォームに使用した接続されている直線セグメントの系列です。_

## <a name="overview"></a>概要

オーバーレイは、マップ上の複数層グラフィックです。 オーバーレイに拡大されると、マップされた拡大縮小描画のグラフィカルなコンテンツをサポートします。 次のスクリーン ショットは、マップに多角形のオーバーレイを追加した結果を表示します。

![](polyline-map-overlay-images/screenshots.png)

ときに、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)コントロールが iOS での Xamarin.Forms アプリケーションによって表示される、`MapRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`MKMapView`コントロール。 Android のプラットフォームでは、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapControl`します。 表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のマップのカスタマイズ設定を実装する、`Map`プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms カスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [カスタマイズ](#Customizing_the_Map)各プラットフォームで、マップのカスタム レンダラーを作成することでマップします。

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) 初期化して使用する前に構成されている必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法の詳細については、次を参照してください。[マップ暗証番号 (pin) をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)です。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップを作成します。

サブクラスを作成、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)を追加するには、クラス、`RouteCoordinates`プロパティ。

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

`RouteCoordinates`プロパティが強調表示するルートを定義する座標のコレクションを格納します。

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

この初期化は、一連の強調表示するマップのルートを定義する、緯度と経度座標を指定します。 マップのビューを配置し、 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/)を作成することで位置と、マップのズーム レベルを変更するメソッドを[ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/)から、 [ `Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)と[ `Distance`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)です。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップをカスタマイズします。

カスタム レンダラーをマップに多角形のオーバーレイを追加するには、各アプリケーション プロジェクトに追加されます必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`ポリライン オーバーレイを追加する方法。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

このメソッドは、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされている、次の構成を実行します。

- `MKMapView.OverlayRenderer`プロパティが対応するデリゲートに設定します。
- 緯度と経度座標のコレクションから取得、`CustomMap.RouteCoordinates`プロパティの配列として格納されていると`CLLocationCoordinate2D`インスタンス。
- ポリラインからは、静的なを呼び出すことによって作成`MKPolyline.FromCoordinates`メソッドでの緯度と経度の各ポイントを指定します。
- ポリラインからは呼び出すことによって、マップに追加、`MKMapView.AddOverlay`メソッドです。

次に、実装、`GetOverlayRenderer`オーバーレイの表示をカスタマイズする方法。

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`と`OnMapReady`ポリライン オーバーレイを追加する方法。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

`OnElementChanged`メソッドからの緯度と経度座標のコレクションを取得する、`CustomMap.RouteCoordinates`プロパティとメンバー変数に格納します。 呼び出して、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`が関連付けられているビューには、カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされています。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`をインスタンス化して、多角形を作成する場所、メソッドが呼び出されます、`PolylineOptions`緯度と経度の各ポイントを指定するオブジェクト。 ポリラインからは呼び出すことによって追加、マップにし、`NativeMap.AddPolyline`メソッドです。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームには、カスタム レンダラーを作成します。

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`ポリライン オーバーレイを追加する方法。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
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

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

このメソッドは、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされている、次の操作を実行します。

- 緯度と経度座標のコレクションから取得、`CustomMap.RouteCoordinates`プロパティおよびに変換された、`List`の`BasicGeoposition`座標。
- ポリラインからは、インスタンス化によって作成された、`MapPolyline`オブジェクト。 `MapPolygon`を設定して、マップに線を表示するクラスが使用されるその`Path`プロパティを`Geopath`線座標を格納しているオブジェクト。
- ポリラインからは、マップ上に追加することによって表示される、`MapControl.MapElements`コレクション。

## <a name="summary"></a>まとめ

この記事は、マップ上のルートの表示を切り替えるために必要な任意の図形を形成する、マップに多角形のオーバーレイを追加する方法について説明します。


## <a name="related-links"></a>関連リンク

- [ポリライン マップ Ovlerlay (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
