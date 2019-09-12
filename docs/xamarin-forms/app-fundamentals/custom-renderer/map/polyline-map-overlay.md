---
title: マップ上でのルートの強調表示
description: この記事では、マップにポリラインのオーバーレイを追加する方法について説明します。 ポリラインのオーバーレイは一連の接続された線分であり、通常はマップ上のルートを示すため、または必要な任意の図形を形成するために使用されます。
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: af6e491725ac3dad843238c1adb547ac76a9c9ea
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771866"
---
# <a name="highlighting-a-route-on-a-map"></a>マップ上でのルートの強調表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-polyline)

"_この記事では、マップにポリラインのオーバーレイを追加する方法について説明します。ポリラインのオーバーレイは一連の接続された線分であり、通常はマップ上のルートを示すため、または必要な任意の図形を形成するために使用されます。_"

## <a name="overview"></a>概要

オーバーレイは、マップ上に重ねて配置されるグラフィックです。 オーバーレイでは、マップが拡大縮小されたときに、マップと一緒に拡大縮小されるグラフィカル コンテンツの描画がサポートされます。 次のスクリーン ショットに、マップにポリラインのオーバーレイを追加した結果を示します。

![](polyline-map-overlay-images/screenshots.png)

Xamarin.Forms アプリケーションによって [`Map`](xref:Xamarin.Forms.Maps.Map) コントロールが レンダリングされると、iOS では `MapRenderer` クラスがインスタンス化され、それによってネイティブの `MKMapView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`MapRenderer` クラスによってネイティブの `MapView` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`MapRenderer` クラスによってネイティブの `MapControl` がインスタンス化されます。 レンダリング プロセスを活用して各プラットフォーム上で `Map` 用のカスタム レンダラーを作成することで、プラットフォーム固有のマップのカスタマイズを実装できます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms カスタム マップを[作成](#Creating_the_Custom_Map)します。
1. Xamarin.Forms からカスタム マップを[使用](#Consuming_the_Custom_Map)します。
1. 各プラットフォーム上でマップ用のカスタム レンダラーを作成することで、マップを[カスタマイズ](#Customizing_the_Map)します。

> [!NOTE]
> 使用する前に [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) を初期化して構成する必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法については、「[Customizing a Map Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)」(マップ ピンのカスタマイズ) を参照してください。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップの作成

`RouteCoordinates` プロパティを追加する、[`Map`](xref:Xamarin.Forms.Maps.Map) のサブクラスを作成します。

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

`RouteCoordinates` プロパティには、強調表示されるルートを定義する座標のコレクションが格納されます。

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

この初期化によって、強調表示されるマップのルートを定義する、一連の緯度と経度の座標が指定されます。 次に、[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) メソッドを使用してマップのビューが位置付けられ、[`Position`](xref:Xamarin.Forms.Maps.Position) と [`Distance`](xref:Xamarin.Forms.Maps.Distance) から [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) を作成することでマップの位置とズーム レベルが変更されます。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップのカスタマイズ

次に、マップにポリラインのオーバーレイを追加するために、各アプリケーション プロジェクトにカスタム レンダラーを追加する必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドをオーバーライドして、ポリラインのオーバーレイを追加します。

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

このメソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、次の構成が実行されます。

- `MKMapView.OverlayRenderer` プロパティが対応するデリゲートに設定されます。
- `CustomMap.RouteCoordinates` プロパティから緯度と経度の座標のコレクションが取得され、`CLLocationCoordinate2D` インスタンスの配列として格納されます。
- 静的な `MKPolyline.FromCoordinates` メソッドを呼び出すことでポリラインが作成され、各ポイントの経度と緯度が指定されます。
- `MKMapView.AddOverlay` メソッドを呼び出すことで、ポリラインがマップに追加されます。

次に、`GetOverlayRenderer` メソッドを実行して、オーバーレイのレンダリングをカスタマイズします。

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドと `OnMapReady` メソッドをオーバーライドして、ポリラインのオーバーレイを追加します。

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

`OnElementChanged` メソッドにより、`CustomMap.RouteCoordinates` プロパティから緯度と経度の座標のコレクションが取得され、それらがメンバー変数に格納されます。 次に、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、`MapView.GetMapAsync` メソッドが呼び出され、ビューに関連付けられている基の `GoogleMap` が取得されます。 `GoogleMap` インスタンスが使用可能になると、`OnMapReady` メソッドが呼び出され、各ポイントの緯度と経度を指定する `PolylineOptions` をインスタンス化することで、ポリラインが作成されます。 `NativeMap.AddPolyline` メソッドを呼び出すことで、ポリラインがマップに追加されます。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォーム上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドをオーバーライドして、ポリラインのオーバーレイを追加します。

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

このメソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、次の操作が実行されます。

- `CustomMap.RouteCoordinates` プロパティから緯度と経度の座標のコレクションが取得され、`BasicGeoposition` 座標の `List` に変換されます。
- `MapPolyline` オブジェクトをインスタンス化することで、ポリラインが作成されます。 `MapPolygon` クラスを使用して、その `Path` プロパティを線の座標が含まれる `Geopath` オブジェクトに設定することで、マップ上に線が表示されます。
- `MapControl.MapElements` コレクションに追加することで、ポリラインがマップ上にレンダリングされます。

## <a name="summary"></a>まとめ

この記事では、ポリラインのオーバーレイをマップに追加して、ルートやその他の必要な形状をマップ上に表示する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [ポリラインのマップのオーバーレイ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-polyline)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
