---
title: マップ上の領域を強調表示
description: この記事では、マップ上の領域を強調表示する、マップに多角形のオーバーレイを追加する方法について説明します。 多角形は閉じた図形と、その内部に情報を入力します。
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: e0ffa1948bb7dd0996dd21793237df550a32aa70
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848331"
---
# <a name="highlighting-a-region-on-a-map"></a>マップ上の領域を強調表示

_この記事では、マップ上の領域を強調表示する、マップに多角形のオーバーレイを追加する方法について説明します。多角形は閉じた図形と、その内部に情報を入力します。_

## <a name="overview"></a>概要

オーバーレイは、マップ上の複数層グラフィックです。 オーバーレイに拡大されると、マップされた拡大縮小描画のグラフィカルなコンテンツをサポートします。 次のスクリーン ショットは、マップに多角形のオーバーレイを追加した結果を表示します。

![](polygon-map-overlay-images/screenshots.png)

ときに、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)コントロールが iOS での Xamarin.Forms アプリケーションによって表示される、`MapRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`MKMapView`コントロール。 Android のプラットフォームでは、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapControl`します。 表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のマップのカスタマイズ設定を実装する、`Map`プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms カスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [カスタマイズ](#Customizing_the_Map)各プラットフォームで、マップのカスタム レンダラーを作成することでマップします。

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) 初期化して使用する前に構成されている必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

カスタム レンダラーを使用してマップをカスタマイズする方法の詳細については、次を参照してください。[マップ暗証番号 (pin) をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)です。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>カスタム マップを作成します。

サブクラスを作成、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)を追加するには、クラス、`ShapeCoordinates`プロパティ。

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

`ShapeCoordinates`プロパティは、領域を強調表示を定義する座標のコレクションを格納します。

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

この初期化は、一連の強調表示するマップの領域を定義する、緯度と経度座標を指定します。 マップのビューを配置し、 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/)を作成することで位置と、マップのズーム レベルを変更するメソッドを[ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/)から、 [ `Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)と[ `Distance`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)です。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>マップをカスタマイズします。

カスタム レンダラーをマップに多角形のオーバーレイを追加するには、各アプリケーション プロジェクトに追加されます必要があります。

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

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

このメソッドは、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされている、次の構成を実行します。

- `MKMapView.OverlayRenderer`プロパティが対応するデリゲートに設定します。
- 緯度と経度座標のコレクションから取得、`CustomMap.ShapeCoordinates`プロパティの配列として格納されていると`CLLocationCoordinate2D`インスタンス。
- 多角形が、静的なを呼び出すことによって作成された`MKPolygon.FromCoordinates`メソッドでの緯度と経度の各ポイントを指定します。
- 多角形が呼び出しによって、マップに追加される、`MKMapView.AddOverlay`メソッドです。 このメソッドは、最初と最後の点を結ぶ線を描画して、多角形を自動的に閉じます。

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

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

`OnElementChanged`メソッドからの緯度と経度座標のコレクションを取得する、`CustomMap.ShapeCoordinates`プロパティとメンバー変数に格納します。 呼び出して、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`が関連付けられているビューには、カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされています。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`をインスタンス化して、多角形を作成する場所、メソッドが呼び出されます、`PolygonOptions`緯度と経度の各ポイントを指定するオブジェクト。 多角形が呼び出すことによって、マップに追加し、`NativeMap.AddPolygon`メソッドです。 このメソッドは、最初と最後の点を結ぶ線を描画して、多角形を自動的に閉じます。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームには、カスタム レンダラーを作成します。

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

このメソッドは、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされている、次の操作を実行します。

- 緯度と経度座標のコレクションから取得、`CustomMap.ShapeCoordinates`プロパティおよびに変換された、`List`の`BasicGeoposition`座標。
- 多角形がインスタンス化して作成された、`MapPolygon`オブジェクト。 `MapPolygon`マルチポイント図形を設定して、マップに表示するクラスが使用されるその`Path`プロパティを`Geopath`図形の座標を格納しているオブジェクト。
- 追加することによって、マップの多角形が表示される、`MapControl.MapElements`コレクション。 最初と最後の点を結ぶ線を描画して、多角形が自動的に閉じられることに注意してください。

## <a name="summary"></a>まとめ

この記事では、マップの領域を強調表示する、マップに多角形のオーバーレイを追加する方法について説明します。 多角形は閉じた図形と、その内部に情報を入力します。


## <a name="related-links"></a>関連リンク

- [多角形マップ オーバーレイ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [マップ ピンのカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
