---
title: マップ上での領域の強調表示
description: この記事では、多角形のオーバーレイをマップに追加して、マップ上のある領域を強調表示する方法について説明します。 多角形は閉じた図形であり、その内側が塗りつぶされます。
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 103d4f40a1c368f576276c4cdcbdc585d2a1536a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771887"
---
# <a name="highlighting-a-region-on-a-map"></a>マップ上での領域の強調表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-polygon)

"_この記事では、多角形のオーバーレイをマップに追加して、マップ上のある領域を強調表示する方法について説明します。多角形は閉じた図形であり、その内側が塗りつぶされます。_"

## <a name="overview"></a>概要

オーバーレイは、マップ上に重ねて配置されるグラフィックです。 オーバーレイでは、マップが拡大縮小されたときに、マップと一緒に拡大縮小されるグラフィカル コンテンツの描画がサポートされます。 次のスクリーン ショットに、マップに多角形のオーバーレイを追加した結果を示します。

![](polygon-map-overlay-images/screenshots.png)

Xamarin.Forms アプリケーションによって [`Map`](xref:Xamarin.Forms.Maps.Map) コントロールが レンダリングされると、iOS では `MapRenderer` クラスがインスタンス化され、それによってネイティブの `MKMapView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`MapRenderer` クラスによってネイティブの `MapView` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`MapRenderer` クラスによってネイティブの `MapControl` がインスタンス化されます。 レンダリング プロセスを活用して各プラットフォーム上で `Map` 用のカスタム レンダラーを作成することで、プラットフォーム固有のマップのカスタマイズを実装できます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms カスタム マップを[作成](#Creating_the_Custom_Map)します。
1. Xamarin.Forms からカスタム マップを[使用](#Consuming_the_Custom_Map)します。
1. 各プラットフォーム上でマップ用のカスタム レンダラーを作成することで、マップを[カスタマイズ](#Customizing_the_Map)します。

> [!NOTE]
> 使用する前に [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) を初期化して構成する必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法については、「[Customizing a Map Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)」(マップ ピンのカスタマイズ) を参照してください。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップの作成

`ShapeCoordinates` プロパティを追加する、[`Map`](xref:Xamarin.Forms.Maps.Map) のサブクラスを作成します。

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` プロパティには、強調表示される領域を定義する座標のコレクションが格納されます。

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

この初期化によって、強調表示されるマップの領域を定義する、一連の緯度と経度の座標が指定されます。 次に、[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) メソッドを使用してマップのビューが位置付けられ、[`Position`](xref:Xamarin.Forms.Maps.Position) と [`Distance`](xref:Xamarin.Forms.Maps.Distance) から [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) を作成することでマップの位置とズーム レベルが変更されます。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップのカスタマイズ

次に、マップに多角形のオーバーレイを追加するために、各アプリケーション プロジェクトにカスタム レンダラーを追加する必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドをオーバーライドして、多角形のオーバーレイを追加します。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

このメソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、次の構成が実行されます。

- `MKMapView.OverlayRenderer` プロパティが対応するデリゲートに設定されます。
- `CustomMap.ShapeCoordinates` プロパティから緯度と経度の座標のコレクションが取得され、`CLLocationCoordinate2D` インスタンスの配列として格納されます。
- 静的な `MKPolygon.FromCoordinates` メソッドを呼び出すことで多角形が作成され、各ポイントの経度と緯度が指定されます。
- `MKMapView.AddOverlay` メソッドを呼び出すことで、多角形がマップに追加されます。 このメソッドにより最初と最後のポイントを結ぶ線が描画されることで、多角形が自動的に閉じられます。

次に、`GetOverlayRenderer` メソッドを実行して、オーバーレイのレンダリングをカスタマイズします。

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドと `OnMapReady` メソッドをオーバーライドして、多角形のオーバーレイを追加します。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

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
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

`OnElementChanged` メソッドにより、`CustomMap.ShapeCoordinates` プロパティから緯度と経度の座標のコレクションが取得され、それらがメンバー変数に格納されます。 次に、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、`MapView.GetMapAsync` メソッドが呼び出され、ビューに関連付けられている基の `GoogleMap` が取得されます。 `GoogleMap` インスタンスが使用可能になると、`OnMapReady` メソッドが呼び出され、各ポイントの緯度と経度を指定する `PolygonOptions` をインスタンス化することで、多角形が作成されます。 次に、`NativeMap.AddPolygon` メソッドを呼び出すことで、多角形がマップに追加されます。 このメソッドにより最初と最後のポイントを結ぶ線が描画されることで、多角形が自動的に閉じられます。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォーム上でのカスタム レンダラーの作成

`MapRenderer` クラスのサブクラスを作成し、その `OnElementChanged` メソッドをオーバーライドして、多角形のオーバーレイを追加します。

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
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

このメソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることを条件として、次の操作が実行されます。

- `CustomMap.ShapeCoordinates` プロパティから緯度と経度の座標のコレクションが取得され、`BasicGeoposition` 座標の `List` に変換されます。
- `MapPolygon` オブジェクトをインスタンス化することで、多角形が作成されます。 `MapPolygon` クラスを使用して、その `Path` プロパティを図形の座標が含まれる `Geopath` オブジェクトに設定することで、マップ上にマルチポイント図形が表示されます。
- `MapControl.MapElements` コレクションに追加することで、多角形がマップ上にレンダリングされます。 最初と最後のポイントを結ぶ線を描画することで、多角形が自動的に閉じられることに注意してください。

## <a name="summary"></a>まとめ

この記事では、多角形のオーバーレイをマップに追加して、マップ上のある領域を強調表示する方法について説明しました。 多角形は閉じた図形であり、その内側が塗りつぶされます。

## <a name="related-links"></a>関連リンク

- [ポリゴン マップのオーバーレイ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-polygon)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
