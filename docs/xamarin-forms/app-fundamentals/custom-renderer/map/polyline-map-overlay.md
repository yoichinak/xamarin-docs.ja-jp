---
title: マップ上のルートを強調表示
description: この記事では、マップにポリライン オーバーレイを追加する方法について説明します。 ポリライン オーバーレイでは、一連のマップ上のルートを表示または必要な任意の図形をフォームに通常使用される接続された線分です。
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 786f050495d4682b719178f2723c482929544678
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998722"
---
# <a name="highlighting-a-route-on-a-map"></a>マップ上のルートを強調表示

_この記事では、マップにポリライン オーバーレイを追加する方法について説明します。ポリライン オーバーレイでは、一連のマップ上のルートを表示または必要な任意の図形をフォームに通常使用される接続された線分です。_

## <a name="overview"></a>概要

オーバーレイは、マップ上の複数層の図です。 オーバーレイに拡大されると、マップを使用して拡大/縮小するグラフィカル コンテンツを描画をサポートします。 次のスクリーン ショットは、ポリライン オーバーレイをマップに追加した結果を表示します。

![](polyline-map-overlay-images/screenshots.png)

ときに、 [ `Map` ](xref:Xamarin.Forms.Maps.Map) iOS での Xamarin.Forms アプリケーションでコントロールが表示される、`MapRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`MKMapView`コントロール。 Android のプラットフォームで、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapControl`します。 レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のマップのカスタマイズを実装する`Map`各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms のカスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [カスタマイズ](#Customizing_the_Map)各プラットフォームで、マップのカスタム レンダラーを作成してマップします。

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 初期化して使用する前に構成されている必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法については、次を参照してください。[マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)します。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップを作成します。

サブクラスを作成、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)を追加するには、クラス、`RouteCoordinates`プロパティ。

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

この初期化は、一連の強調表示するマップのルートを定義する、緯度と経度の座標を指定します。 マップのビューを配置し、 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドを作成して、位置と、マップのズーム レベルを変更する、 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan)から、 [ `Position`](xref:Xamarin.Forms.Maps.Position)と[ `Distance`](xref:Xamarin.Forms.Maps.Distance)します。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップのカスタマイズ

カスタム レンダラーは、マップにポリライン オーバーレイを追加するには、各アプリケーション プロジェクトにようになりました追加する必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

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

このメソッドは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることに、次の構成を実行します。

- `MKMapView.OverlayRenderer`プロパティが対応するデリゲートに設定します。
- 緯度と経度の座標のコレクションから取得、`CustomMap.RouteCoordinates`プロパティの配列として格納されていると`CLLocationCoordinate2D`インスタンス。
- ポリラインが静的なを呼び出すことによって作成された`MKPolyline.FromCoordinates`メソッドで、各ポイントの経度と緯度を指定します。
- ポリラインからは呼び出すことによって、マップに追加されます、`MKMapView.AddOverlay`メソッド。

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

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

`OnElementChanged`メソッドからの緯度と経度の座標のコレクションを取得する、`CustomMap.RouteCoordinates`プロパティとメンバー変数に保存します。 呼び出して、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること、ビューに関連付けられているです。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`ポリラインがインスタンス化して作成されている、メソッドが呼び出されます、`PolylineOptions`緯度と経度の各ポイントを指定するオブジェクト。 ポリラインが呼び出すことによってマップに追加し、`NativeMap.AddPolyline`メソッド。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームでのカスタム レンダラーの作成

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

このメソッドは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることに、次の操作を実行します。

- 緯度と経度の座標のコレクションから取得、`CustomMap.RouteCoordinates`プロパティに変換し、`List`の`BasicGeoposition`座標。
- ポリラインがインスタンス化によって作成された、`MapPolyline`オブジェクト。 `MapPolygon`を設定して、マップの線を表示するクラスが使用されるその`Path`プロパティを`Geopath`直線の座標を格納しているオブジェクト。
- ポリラインからは、マップ上に追加することによって表示される、`MapControl.MapElements`コレクション。

## <a name="summary"></a>まとめ

この記事では、マップ上のルートを表示するために必要な任意の図形を形成したり、マップにポリライン オーバーレイを追加する方法について説明します。


## <a name="related-links"></a>関連リンク

- [ポリライン マップ Ovlerlay (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
