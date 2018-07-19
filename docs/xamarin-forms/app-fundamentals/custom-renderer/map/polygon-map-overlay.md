---
title: マップ上の領域を強調表示
description: この記事では、マップ上の領域を強調表示する、マップに多角形のオーバーレイを追加する方法について説明します。 多角形は、閉じた形状と、その内部を塗りつぶすに情報を入力します。
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998135"
---
# <a name="highlighting-a-region-on-a-map"></a>マップ上の領域を強調表示

_この記事では、マップ上の領域を強調表示する、マップに多角形のオーバーレイを追加する方法について説明します。多角形は、閉じた形状と、その内部を塗りつぶすに情報を入力します。_

## <a name="overview"></a>概要

オーバーレイは、マップ上の複数層の図です。 オーバーレイに拡大されると、マップを使用して拡大/縮小するグラフィカル コンテンツを描画をサポートします。 次のスクリーン ショットは、マップに多角形のオーバーレイを追加した結果を表示します。

![](polygon-map-overlay-images/screenshots.png)

ときに、 [ `Map` ](xref:Xamarin.Forms.Maps.Map) iOS での Xamarin.Forms アプリケーションでコントロールが表示される、`MapRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`MKMapView`コントロール。 Android のプラットフォームで、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapControl`します。 レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のマップのカスタマイズを実装する`Map`各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms のカスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [カスタマイズ](#Customizing_the_Map)各プラットフォームで、マップのカスタム レンダラーを作成してマップします。

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 初期化して使用する前に構成されている必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法については、次を参照してください。[マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)します。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップを作成します。

サブクラスを作成、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)を追加するには、クラス、`ShapeCoordinates`プロパティ。

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

`ShapeCoordinates`プロパティが強調表示する領域を定義する座標のコレクションを格納します。

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

この初期化は、一連の強調表示するマップの領域を定義する、緯度と経度の座標を指定します。 マップのビューを配置し、 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドを作成して、位置と、マップのズーム レベルを変更する、 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan)から、 [ `Position`](xref:Xamarin.Forms.Maps.Position)と[ `Distance`](xref:Xamarin.Forms.Maps.Distance)します。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップのカスタマイズ

カスタム レンダラーは、マップに多角形のオーバーレイを追加するには、各アプリケーション プロジェクトにようになりました追加する必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`多角形のオーバーレイを追加する方法。

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

このメソッドは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることに、次の構成を実行します。

- `MKMapView.OverlayRenderer`プロパティが対応するデリゲートに設定します。
- 緯度と経度の座標のコレクションから取得、`CustomMap.ShapeCoordinates`プロパティの配列として格納されていると`CLLocationCoordinate2D`インスタンス。
- 多角形が、静的なを呼び出すことによって作成された`MKPolygon.FromCoordinates`メソッドで、各ポイントの経度と緯度を指定します。
- 多角形は、呼び出すことによって、マップに追加されます、`MKMapView.AddOverlay`メソッド。 このメソッドは、最初と最後の点を結ぶ線を描画することで、多角形を自動的に閉じます。

次に、実装、`GetOverlayRenderer`オーバーレイの表示をカスタマイズする方法。

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`と`OnMapReady`多角形のオーバーレイを追加する方法。

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

`OnElementChanged`メソッドからの緯度と経度の座標のコレクションを取得する、`CustomMap.ShapeCoordinates`プロパティとメンバー変数に保存します。 呼び出して、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること、ビューに関連付けられているです。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`をインスタンス化して、多角形が作成されている、メソッドが呼び出されます、`PolygonOptions`緯度と経度の各ポイントを指定するオブジェクト。 多角形が呼び出すことによって、マップに追加し、`NativeMap.AddPolygon`メソッド。 このメソッドは、最初と最後の点を結ぶ線を描画することで、多角形を自動的に閉じます。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームでのカスタム レンダラーの作成

サブクラスを作成、`MapRenderer`クラスし、オーバーライドの`OnElementChanged`多角形のオーバーレイを追加する方法。

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

このメソッドは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていることに、次の操作を実行します。

- 緯度と経度の座標のコレクションから取得、`CustomMap.ShapeCoordinates`プロパティに変換し、`List`の`BasicGeoposition`座標。
- 多角形がインスタンス化によって作成された、`MapPolygon`オブジェクト。 `MapPolygon`マルチポイント図形を設定して、マップに表示するクラスが使用されるその`Path`プロパティを`Geopath`図形の座標を格納しているオブジェクト。
- 多角形は、マップ上に追加することによって表示される、`MapControl.MapElements`コレクション。 最初と最後の点を結ぶ線を描画することで、多角形が自動的に閉じられることに注意してください。

## <a name="summary"></a>まとめ

この記事では、マップの領域を強調表示する、マップに多角形のオーバーレイを追加する方法について説明します。 多角形は、閉じた形状と、その内部を塗りつぶすに情報を入力します。


## <a name="related-links"></a>関連リンク

- [マップの多角形は (サンプル)"オーバーレイ"](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
