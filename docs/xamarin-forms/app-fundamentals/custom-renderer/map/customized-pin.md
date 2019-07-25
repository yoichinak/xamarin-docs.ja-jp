---
title: マップ ピンのカスタマイズ
description: この記事では、各プラットフォーム上でカスタマイズされたピンとピン データのカスタマイズされたビューを含むネイティブ マップを表示する、マップ コントロール用のカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 8e80016e33e8bebba715be4f02060e76086884fc
ms.sourcegitcommit: 4b6e832d1db5616b657dc8540da67c509b28dc1d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386198"
---
# <a name="customizing-a-map-pin"></a>マップ ピンのカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/Map/Pin/)

_この記事では、各プラットフォーム上でカスタマイズされたピンとピン データのカスタマイズされたビューを含むネイティブ マップを表示する、マップ コントロール用のカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms ビューに、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 iOS で Xamarin.Forms アプリケーションによって [`Map`](xref:Xamarin.Forms.Maps.Map) がレンダリングされると、`MapRenderer` クラスがインスタンス化され、次に、ネイティブの `MKMapView` コントロールがインスタンス化されます。 Android プラットフォーム上では、`MapRenderer` クラスによってネイティブの `MapView` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`MapRenderer` クラスによってネイティブの `MapControl` がインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスとネイティブ コントロール) を参照してください。

次の図は、[`Map`](xref:Xamarin.Forms.Maps.Map) と、それを実装する、対応するネイティブ コントロールの関係を示しています。

![](customized-pin-images/map-classes.png "マップ コントロールと実装するネイティブ コントロールの関係")

レンダリング プロセスを使用して、各プラットフォーム上で [`Map`](xref:Xamarin.Forms.Maps.Map) 用のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装することができます。 これを行うプロセスは次のとおりです。

1. Xamarin.Forms カスタム マップを[作成](#Creating_the_Custom_Map)します。
1. Xamarin.Forms からカスタム マップを[使用](#Consuming_the_Custom_Map)します。
1. 各プラットフォーム上でマップのカスタム レンダラーを[作成](#Creating_the_Custom_Renderer_on_each_Platform)します。

各プラットフォームでカスタマイズされたピンとピン データのカスタマイズされたビューを含むネイティブ マップを表示する `CustomMap` レンダラーを実装するために、ここでは項目ごとに順番に説明します。

> [!NOTE]
> 使用する前に [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) を初期化して構成する必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>カスタム マップの作成

カスタム マップ コントロールは、次のコード例のように、[`Map`](xref:Xamarin.Forms.Maps.Map) クラスをサブクラス化することで作成できます。

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` コントロールは、.NET 標準ライブラリ プロジェクトで作成されます。このコントロールではカスタム マップの API を定義します。 カスタム マップでは、各プラットフォーム上にネイティブ マップ コントロールによってレンダリングされる、`CustomPin` オブジェクトのコレクションを表す `CustomPins` プロパティが公開されます。 `CustomPin` クラスを次のコード例に示します。

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

このクラスでは、[`Pin`](xref:Xamarin.Forms.Maps.Pin) クラスのプロパティを継承するものとして、`Url` プロパティを追加することで、`CustomPin` を定義します。

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>カスタム マップの使用

`CustomMap` コントロールは、その場所の名前空間を宣言し、カスタム マップ コントロール上で名前空間プレフィックスを使用することで、.NET 標準ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページでどのように `CustomMap` コントロールを使用できるかを示しています。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

`local` 名前空間プレフィックスには任意の名前を付けることができます。 しかし、`clr-namespace` と `assembly` の値は、カスタム マップの詳細と一致する必要があります。 名前空間が宣言されたら、カスタム マップを参照するためにプレフィックスが使用されます。

次のコード例は、C# ページでどのように `CustomMap` コントロールを使用できるかを示しています。

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

`CustomMap` インスタンスは、各プラットフォーム上でネイティブ マップを表示するために使用されます。 その [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) プロパティでは、[`MapType`](xref:Xamarin.Forms.Maps.MapType) 列挙型で定義されている使用可能な値で、[`Map`](xref:Xamarin.Forms.Maps.Map) の表示スタイルが設定されます。 iOS と Android の場合、マップの幅と高さは、プラットフォーム固有のプロジェクトで初期化される `App` クラスのプロパティを使用して設定されます。

マップの場所、およびそれに含まれるピンは、次のコード例に示すように初期化されます。

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    MarkerId = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

この初期化ではカスタム ピンが追加され、[`Position`](xref:Xamarin.Forms.Maps.Position) と [`Distance`](xref:Xamarin.Forms.Maps.Distance) から [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) を作成することでマップの位置とズーム レベルを変更する、[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) メソッドを使用してマップのビューが位置付けられます。

これで、カスタム レンダラーを各アプリケーション プロジェクトに追加して、ネイティブ マップ コントロールをカスタマイズできるようになります。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム マップをレンダリングする `MapRenderer` クラスのサブクラスを作成します。
1. カスタム マップをレンダリングする `OnElementChanged` メソッドをオーバーライドし、ロジックを書き込んでカスタマイズします。 対応する Xamarin.Forms カスタム マップの作成時に、このメソッドが呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加し、Xamarin.Forms カスタム マップのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラス用の既定のレンダラーが使用されます。

次の図に、サンプル アプリケーション内の各プロジェクトの役割と、それらの関係を示します。

![](customized-pin-images/solution-structure.png "CustomMap カスタム レンダラーのプロジェクトの役割")

`CustomMap` コントロールはプラットフォーム固有のレンダラー クラスによってレンダリングされます。このクラスは各プラットフォームの `MapRenderer` クラスから派生しています。 この結果、次のスクリーンショットに示すように、プラットフォーム固有のコントロールを使用して、それぞれの `CustomMap` コントロールがレンダリングされます。

![](customized-pin-images/screenshots.png "各プラットフォームの CustomMap")

`MapRenderer` クラスでは `OnElementChanged` メソッドを公開します。このメソッドは、対応するネイティブ コントロールをレンダリングするために、Xamarin.Forms カスタム マップの作成時に呼び出されます。 このメソッドでは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーが接続して*いた* Xamarin.Forms 要素と、レンダラーが現在接続して*いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `CustomMap` インスタンスへの参照が含まれます。

各プラットフォーム固有のレンダラー クラス内の、オーバーライドされたバージョンの `OnElementChanged` メソッドは、ネイティブ コントロールのカスタマイズを行う場所です。 プラットフォーム上で使用されているネイティブ コントロールへの型指定された参照には、`Control` プロパティを使用してアクセスすることができます。 さらに、レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

次のコード例に示すように、`OnElementChanged` メソッドでイベント ハンドラーをサブスクライブする場合は注意が必要です。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.View> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされているときにのみ、ネイティブ コントロールを構成し、イベント ハンドラーをサブスクライブする必要があります。 同様に、レンダラーがアタッチされている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーのサブスクライブをすべて解除する必要があります。 この手法を採用することは、メモリ リークが発生しないカスタム レンダラーの作成に役立ちます。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms カスタム コントロールの種類名と、カスタム レンダラーの種類名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

次のセクションで、各プラットフォーム固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

次のスクリーンショットは、カスタマイズの前と後のマップを示しています。

![](customized-pin-images/map-layout-ios.png "カスタマイズの前と後のマップ コントロール")

iOS では、ピンは*注釈*と呼ばれ、カスタム イメージまたはさまざまな色のシステム定義のピンにすることができます。 注釈では、必要に応じて、注釈を選択するユーザーへの応答として表示される、*吹き出し*を示すことができます。 吹き出しでは、`Pin` インスタンスの `Label` および `Address` プロパティが表示され、オプションの左側と右側のアクセサリ ビューが示されます。 上記のスクリーンショットでは、左側のアクセサリ ビューはサルのイメージで、右側のアクセサリ ビューは*情報* ボタンとなっています。

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

`OnElementChanged` メソッドでは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、次の [`MKMapView`](xref:MapKit.MKMapView) インスタンスの構成が実行されます。

- [`GetViewForAnnotation`](xref:MapKit.MKMapView.GetViewForAnnotation*) プロパティは `GetViewForAnnotation` メソッドに設定されます。 このメソッドは、[注釈の場所がマップで表示されるようになった](#Displaying_the_Annotation)ときに呼び出され、表示する前に注釈をカスタマイズするために使用されます。
- `CalloutAccessoryControlTapped`、`DidSelectAnnotationView`、および `DidDeselectAnnotationView` イベントのイベント ハンドラーが登録されます。 これらのイベントは、ユーザーが[吹き出しの右側のアクセサリをタップ](#Tapping_on_the_Right_Callout_Accessory_View)したとき、およびユーザーが注釈を[選択](#Selecting_the_Annotation)および[選択解除](#Deselecting_the_Annotation)したときに、それぞれ発生します。 レンダラーがアタッチされているイベントが変わった場合にのみ、イベントのサブスクライブが解除されます。

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>注釈の表示

`GetViewForAnnotation` メソッドは、注釈の場所がマップで表示されるようになったときに呼び出され、表示する前に注釈をカスタマイズするために使用されます。 注釈には次の 2 つの部分があります。

- `MkAnnotation` – 注釈のタイトル、サブタイトル、および場所が含まれます。
- `MkAnnotationView` – 注釈を表すイメージが含まれます。また、必要に応じて、ユーザーが注釈をタップしたときに表示される吹き出しが含まれます。

`GetViewForAnnotation` メソッドでは、注釈のデータを含む `IMKAnnotation` が受け入れられ、マップに表示される `MKAnnotationView` が返され、以下のコード例のように示されます。

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.MarkerId.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.MarkerId.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).MarkerId = customPin.MarkerId.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

このメソッドにより、注釈はシステム定義のピンではなく、カスタム イメージとして確実に表示されるようになります。また、注釈がタップされたときに、注釈のタイトルとアドレスの左側と右側に追加のコンテンツを含む吹き出しが確実に表示されるようになります。 これは次のように行われます。

1. 注釈のカスタム ピン データを返すために、`GetCustomPin` メソッドが呼び出されます。
1. メモリを節約するため、[`DequeueReusableAnnotation`](xref:MapKit.MKMapView.DequeueReusableAnnotation*) の呼び出しで再利用できるように注釈のビューがプーリングされます。
1. `CustomMKAnnotationView` クラスでは、`CustomPin` インスタンスの同じプロパティに対応する `MarkerId` および `Url` プロパティを使用して、`MKAnnotationView` クラスが拡張されます。 注釈が `null` の場合、`CustomMKAnnotationView` の新しいインスタンスが作成されます。
    - `CustomMKAnnotationView.Image` プロパティは、マップ上の注釈を表すイメージに設定されます。
    - `CustomMKAnnotationView.CalloutOffset` プロパティは `CGPoint` に設定されます。これにより、吹き出しが注釈の上の中央に表示されることが指定されます。
    - `CustomMKAnnotationView.LeftCalloutAccessoryView` プロパティは、注釈のタイトルとアドレスの左側に表示されるサルのイメージに設定されます。
    - `CustomMKAnnotationView.RightCalloutAccessoryView` プロパティは、注釈のタイトルとアドレスの右側に表示される*情報* ボタンに設定されます。
    - `CustomMKAnnotationView.MarkerId` プロパティは、`GetCustomPin` メソッドによって返される `CustomPin.MarkerId` プロパティに設定されます。 これにより、注釈を識別でき、必要に応じて、その[吹き出しをさらにカスタマイズできる](#Selecting_the_Annotation)ようになります。
    - `CustomMKAnnotationView.Url` プロパティは、`GetCustomPin` メソッドによって返される `CustomPin.Url` プロパティに設定されます。 ユーザーが[右側の吹き出しのアクセサリ ビューに表示されるボタンをタップ](#Tapping_on_the_Right_Callout_Accessory_View)したときに、URL にナビゲートされます。
1. [`MKAnnotationView.CanShowCallout`](xref:MapKit.MKAnnotationView.CanShowCallout*) プロパティは `true` に設定され、注釈がタップされたときに吹き出しが表示されるようになります。
1. マップに表示される注釈が返されます。

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>注釈の選択

ユーザーが注釈をタップしたときに、`DidSelectAnnotationView` イベントが発生し、次に、`OnDidSelectAnnotationView` メソッドが実行されます。

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.MarkerId == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

選択された注釈の `MarkerId` プロパティが `Xamarin` に設定されている場合、このメソッドでは、Xamarin ロゴのイメージを含む既存の吹き出しに `UIView` インスタンスを追加することで、その吹き出し (左側と右側のアクセサリ ビューを含む) を拡張します。 これにより、異なる注釈に対して異なる吹き出しを表示できるシナリオが可能になります。 `UIView` インスタンスは、既存の吹き出しの上の中央に表示されます。

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>右側の吹き出しのアクセサリ ビューのタップ

ユーザーが右側の吹き出しのアクセサリ ビューにある*情報* ボタンをタップすると、`CalloutAccessoryControlTapped` イベントが発生し、次に、`OnCalloutAccessoryControlTapped` メソッドが実行されます。

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

このメソッドでは、Web ブラウザーを開き、`CustomMKAnnotationView.Url` プロパティに格納されているアドレスに移動します。 .NET 標準ライブラリ プロジェクトでの `CustomPin` コレクションの作成時に、アドレスが定義されていることに注意してください。

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>注釈の選択解除

注釈が表示され、ユーザーがマップをタップしたときに、`DidDeselectAnnotationView` イベントが発生し、次に、`OnDidDeselectAnnotationView` メソッドが実行されます。

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

このメソッドにより、既存の吹き出しが選択されていない場合は、吹き出しの拡張された部分 (Xamarin ロゴのイメージ) も確実に表示されなくなり、そのリソースが解放されるようになります。

`MKMapView` インスタンスのカスタマイズの詳細については、[iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)に関するページを参照してください。

### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

次のスクリーンショットは、カスタマイズの前と後のマップを示しています。

![](customized-pin-images/map-layout-android.png "カスタマイズの前と後のマップ コントロール")

Android では、ピンは*マーカー*と呼ばれ、カスタム イメージまたはさまざまな色のシステム定義のマーカーにすることができます。 マーカーでは*情報ウィンドウ*を表示することができます。このウィンドウは、マーカーをタップしたユーザーへの応答として表示されます。 情報ウィンドウには `Pin` インスタンスの `Label` および `Address` プロパティが表示されます。このウィンドウをカスタマイズして、他のコンテンツを含めることができます。 しかし、情報ウィンドウを表示できるのは一度に 1 つのみとなります。

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、`OnElementChanged` メソッドで `MapView.GetMapAsync` メソッドが呼び出され、ビューに関連付けられている基になる `GoogleMap` が取得されます。 `GoogleMap` インスタンスが使用できるようになったら、`OnMapReady` オーバーライドが呼び出されます。 このメソッドでは `InfoWindowClick` イベントのイベント ハンドラーが登録され、イベントは[情報ウィンドウがクリックされた](#Clicking_on_the_Info_Window)ときに発生し、レンダラーがアタッチされている要素が変わったときにのみ、サブスクライブが解除されます。 また、`OnMapReady` オーバーライドでは `SetInfoWindowAdapter` メソッドを呼び出し、`CustomMapRenderer` クラス インスタンスで、情報ウィンドウをカスタマイズするためのメソッドが提供されるように指定します。

`CustomMapRenderer` クラスでは、[情報ウィンドウをカスタマイズする](#Customizing_the_Info_Window)ための `GoogleMap.IInfoWindowAdapter` インターフェイスが実装されます。 このインターフェイスで、次のメソッドを実装する必要があることを指定します。

- `public Android.Views.View GetInfoWindow(Marker marker)` – このメソッドは、マーカーのカスタム情報ウィンドウを返すために呼び出されます。 `null` が返された場合、既定のウィンドウのレンダリングが使用されます。 `View` が返された場合、その `View` が情報ウィンドウのフレーム内に配置されます。
- `public Android.Views.View GetInfoContents(Marker marker)` – このメソッドは、情報ウィンドウのコンテンツを含む `View` を返すために呼び出され、`GetInfoWindow` メソッドで `null` が返された場合にのみ呼び出されます。 `null` が返された場合、情報ウィンドウのコンテンツの既定のレンダリングが使用されます。

サンプル アプリケーションでは、情報ウィンドウのコンテンツのみがカスタマイズされるため、これを有効にするために `GetInfoWindow` メソッドで `null` が返されます。

#### <a name="customizing-the-marker"></a>マーカーのカスタマイズ

マーカーを表すために使用されるアイコンは、`MarkerOptions.SetIcon` メソッドを呼び出すことでカスタマイズできます。 これは、マップに追加されている `Pin` ごとに呼び出される、`CreateMarker` メソッドをオーバーライドすることで行うことができます。

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

このメソッドでは、`Pin` インスタンスごとに新しい `MarkerOption` を作成します。 マーカーの位置、ラベル、およびアドレスを設定した後、そのアイコンが `SetIcon` メソッドを使用して設定されます。 このメソッドでは、`BitmapDescriptor` の作成を簡略化するためのヘルパー メソッドを提供する `BitmapDescriptor` クラスを使用して、アイコンをレンダリングするために必要なデータを含む `BitmapDescriptorFactory` オブジェクトが取得されます。 マーカーをカスタマイズするための `BitmapDescriptorFactory` クラスの使用の詳細については、[マーカーのカスタマイズ](~/android/platform/maps-and-location/maps/maps-api.md)に関するページを参照してください。

> [!NOTE]
> 必要に応じて、マップ レンダラーで `GetMarkerForPin` メソッドを呼び出し、`Pin` から `Marker` を取得することができます。

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>情報ウィンドウのカスタマイズ

`GetInfoWindow` メソッドで `null` が返される場合、ユーザーがマーカーをタップすると、`GetInfoContents` メソッドが実行されます。 次のコード例は、`GetInfoContents` メソッドを示しています。

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.MarkerId.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

このメソッドでは、情報ウィンドウのコンテンツを含む `View` が返されます。 これは次のように行われます。

- `LayoutInflater` インスタンスが取得されます。 これは、レイアウト XML ファイルをそれに対応する `View` にインスタンス化するために使用されます。
- 情報ウィンドウのカスタム ピン データを返すために、`GetCustomPin` メソッドが呼び出されます。
- `CustomPin.MarkerId` が `Xamarin` と等しい場合、`XamarinMapInfoWindow` レイアウトが拡張されます。 それ以外の場合は、`MapInfoWindow` レイアウトが拡張されます。 これにより、異なるマーカーに対して異なる情報ウィンドウ レイアウトを表示できるシナリオが可能になります。
- `InfoWindowTitle` および `InfoWindowSubtitle` リソースは拡張されたレイアウトから取得されます。リソースが `null` でない場合、それらの `Text` プロパティは `Marker` インスタンスの対応するデータに設定されます。
- マップに表示される `View` インスタンスが返されます。

> [!NOTE]
> 情報ウィンドウはライブ `View` ではありません。 代わりに、Android で `View` が静的なビットマップに変換され、イメージとして表示されます。 これは、情報ウィンドウではクリック イベントに応答できますが、タッチ イベントやジェスチャには応答できず、また、情報ウィンドウ内の個々のコントロールではそれ自体のクリック イベントに応答できないことを意味します。

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>情報ウィンドウのクリック

ユーザーが情報ウィンドウをクリックしたときに、`InfoWindowClick` イベントが発生し、次に、`OnInfoWindowClick` メソッドが実行されます。

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

このメソッドでは、Web ブラウザーを開き、`Marker` の取得された `CustomPin` インスタンスの `Url` プロパティに格納されているアドレスに移動します。 .NET 標準ライブラリ プロジェクトでの `CustomPin` コレクションの作成時に、アドレスが定義されていることに注意してください。

`MapView` インスタンスのカスタマイズの詳細については、[マップ API](~/android/platform/maps-and-location/maps/maps-api.md)に関するページを参照してください。

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォーム上でのカスタム レンダラーの作成

次のスクリーンショットは、カスタマイズの前と後のマップを示しています。

![](customized-pin-images/map-layout-uwp.png "カスタマイズの前と後のマップ コントロール")

UWP では、ピンは*マップ アイコン*と呼ばれ、カスタム イメージまたはシステム定義の既定のイメージにすることができます。 マップ アイコンでは、そのマップ アイコンをタップしたユーザーへの応答として表示される、`UserControl` を示すことができます。 `UserControl` では、`Pin` インスタンスの `Label` および `Address` プロパティを含む、すべてのコンテンツを表示できます。

次のコード例は、UWP のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、`OnElementChanged` メソッドでは次の操作が行われます。

- `MapControl.Children` コレクションがクリアされ、`MapElementClick` イベントのイベント ハンドラーを登録する前に、マップから既存のユーザー インターフェイス要素が削除されます。 このイベントは、ユーザーが `MapControl` で `MapElement` をタップまたはクリックしたときに発生し、レンダラーがアタッチされている要素が変わったときにのみ、サブスクライブが解除されます。
- `customPins` コレクション内の各ピンは、以下のように、マップ上の正しい地理的な場所に表示されます。
  - ピンの場所は、`Geopoint` インスタンスとして作成されます。
  - ピンを表すために `MapIcon` インスタンスが作成されます。
  - `MapIcon` を表すために使用されるイメージは、`MapIcon.Image` プロパティを設定することで指定されます。 しかし、マップ アイコンのイメージが常に表示されるとは限りません。これは、マップ上の他の要素で見えなくなる場合があるためです。 したがって、マップ アイコンの `CollisionBehaviorDesired` プロパティは `MapElementCollisionBehavior.RemainVisible` に設定されます。これにより、確実に表示されたままになります。
  - `MapIcon` の場所は、`MapIcon.Location` プロパティを設定することで指定されます。
  - `MapIcon.NormalizedAnchorPoint` プロパティは、イメージ上のポインターのおおよその場所に設定されます。 このプロパティでイメージの左上隅を表す既定値 (0,0) が保持されている場合、マップのズーム レベルの変更により、イメージで別の場所を指すようになる可能性があります。
  - `MapIcon` インスタンスは `MapControl.MapElements` コレクションに追加されます。 これで、マップ アイコンが `MapControl` に表示されるようになります。

> [!NOTE]
> 複数のマップ アイコンに対して同じイメージを使用する場合は、最適なパフォーマンスを得るために、`RandomAccessStreamReference` インスタンスをページまたはアプリケーション レベルで宣言する必要があります。

#### <a name="displaying-the-usercontrol"></a>UserControl の表示

ユーザーがマップ アイコンをタップすると、`OnMapElementClick` メソッドが実行されます。 以下のコード例はこのメソッドを示しています。

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.MarkerId.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

このメソッドでは、ピンに関する情報を表示する `UserControl` インスタンスが作成されます。 これは次のように行われます。

- `MapIcon` インスタンスが取得されます。
- 表示されるカスタム ピン データを返すために、`GetCustomPin` メソッドが呼び出されます。
- カスタム ピン データを表示するために、`XamarinMapOverlay` インスタンスが作成されます。 このクラスはユーザー コントロールです。
- `MapControl` に `XamarinMapOverlay` インスタンスを表示する地理的な場所は、`Geopoint` インスタンスとして作成されます。
- `XamarinMapOverlay` インスタンスは `MapControl.Children` コレクションに追加されます。 このコレクションには、マップに表示される XAML ユーザー インターフェイス要素が含まれています。
- マップ上の `XamarinMapOverlay` インスタンスの地理的な場所は、`SetLocation` メソッドを呼び出すことで設定されます。
- 指定された場所に対応する、`XamarinMapOverlay` インスタンスの相対的な場所は、`SetNormalizedAnchorPoint` メソッドを呼び出すことで設定されます。 これにより、マップのズーム レベルが変更されたときに、`XamarinMapOverlay` インスタンスが常に正しい場所に確実に表示されるようになります。

また、ピンに関する情報がマップに既に表示されている場合は、マップをタップすると、`MapControl.Children` コレクションから `XamarinMapOverlay` インスタンスが削除されます。

#### <a name="tapping-on-the-information-button"></a>情報ボタンのタップ

ユーザーが `XamarinMapOverlay` ユーザー コントロールにある*情報* ボタンをタップすると、`Tapped` イベントが発生し、次に `OnInfoButtonTapped` メソッドが実行されます。

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

このメソッドでは、Web ブラウザーを開き、`CustomPin` インスタンスの `Url` プロパティに格納されているアドレスに移動します。 .NET 標準ライブラリ プロジェクトでの `CustomPin` コレクションの作成時に、アドレスが定義されていることに注意してください。

`MapControl` インスタンスのカスタマイズの詳細については、MSDN の「[地図と位置情報の概要](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx)」を参照してください。

## <a name="related-links"></a>関連リンク

- [マップ コントロール](~/xamarin-forms/user-interface/map.md)
- [iOS のマップ](~/ios/user-interface/controls/ios-maps/index.md)
- [Maps API](~/android/platform/maps-and-location/maps/maps-api.md)
- [カスタマイズされたピン (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/Map/Pin/)
