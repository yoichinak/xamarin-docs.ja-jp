---
title: マップ ピンのカスタマイズ
description: この記事では、各プラットフォームでカスタマイズした暗証番号 (pin) と、暗証番号 (pin) のデータのカスタマイズされたビューを使用してネイティブ マップを表示する、マップ コントロールのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4fee67f08e86c40709aa226c40c0f7721dc26800
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998317"
---
# <a name="customizing-a-map-pin"></a>マップ ピンのカスタマイズ

_この記事では、各プラットフォームでカスタマイズした暗証番号 (pin) と、暗証番号 (pin) のデータのカスタマイズされたビューを使用してネイティブ マップを表示する、マップ コントロールのカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms のビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)で iOS、Xamarin.Forms アプリケーションによって表示される、`MapRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`MKMapView`コントロール。 Android のプラットフォームで、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`MapRenderer`クラスのインスタンスを作成、ネイティブ`MapControl`します。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)およびそれを実装するネイティブ コントロールの対応します。

![](customized-pin-images/map-classes.png "マップ コントロールと実装のネイティブ コントロール間のリレーションシップ")

レンダリング プロセスのカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装するために使用できます、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms のカスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームで、マップのカスタム レンダラーです。

各項目が実装するためにさらに、説明するようになりましたが、`CustomMap`レンダラーに各プラットフォームでカスタマイズした暗証番号 (pin) と、暗証番号 (pin) のデータのカスタマイズされたビューを使用してネイティブ マップを表示します。

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 初期化して使用する前に構成されている必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>カスタム マップを作成します。

カスタムのマップ コントロールをサブクラス化して作成できます、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)クラスに、次のコード例に示すようにします。

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap`コントロールは、.NET Standard ライブラリ プロジェクトが作成され、カスタム マップの API を定義します。 カスタム マップを公開、`CustomPins`プロパティのコレクションを表す`CustomPin`各プラットフォームでネイティブのマップ コントロールによって表示されるオブジェクト。 `CustomPin`クラスを次のコード例に示します。

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

このクラスを定義、`CustomPin`のプロパティを継承すると、 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin)クラス、および追加を`Url`プロパティ。

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>カスタム マップの使用

`CustomMap`コントロールを参照できます XAML で .NET Standard ライブラリ プロジェクトの場所の名前空間を宣言して、カスタムのマップ コントロールの名前空間プレフィックスを使用します。 次のコード例に示す方法、 `CustomMap` XAML ページでコントロールを使用できます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム マップの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム マップを参照します。

次のコード例に示す方法、`CustomMap`コントロールは、c# のページで使用できます。

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

`CustomMap`インスタンスは、各プラットフォームでネイティブのマップを表示する使用されます。 [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType)プロパティの表示スタイルを設定する、 [ `Map`](xref:Xamarin.Forms.Maps.Map)で定義されている値を含む、 [ `MapType` ](xref:Xamarin.Forms.Maps.MapType)列挙体。 IOS と Android では、幅と高さマップの設定のプロパティを使用、`App`はプラットフォーム固有のプロジェクトで初期化されるクラス。

マップ、および、ピンの場所が含まれていますの次のコード例に示すように初期化されます。

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

この初期化は、独自の pin を追加しで、マップのビューを配置、 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)メソッドを作成して、位置と、マップのズーム レベルを変更する、 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) から[`Position` ](xref:Xamarin.Forms.Maps.Position)と[ `Distance`](xref:Xamarin.Forms.Maps.Distance)します。

カスタム レンダラーは、ネイティブのマップ コントロールをカスタマイズするには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`MapRenderer`カスタム マップを表示するクラス。
1. 上書き、`OnElementChanged`書き込みロジックをカスタマイズする、カスタムのマップを表示するメソッド。 このメソッドは、対応する Xamarin.Forms のカスタム マップの作成時に呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラスは、Xamarin.Forms カスタム マップを表示するために使用するように指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> 各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能になります。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。

次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](customized-pin-images/solution-structure.png "CustomMap カスタム レンダラーのプロジェクトの責任")

`CustomMap`から派生したプラットフォーム固有のレンダラー クラスによってコントロールが表示される、`MapRenderer`各プラットフォームのクラス。 これは、結果、各`CustomMap`の次のスクリーン ショットに示すようにプラットフォーム固有のコントロールに表示されるを制御します。

![](customized-pin-images/screenshots.png "各プラットフォームの CustomMap")

`MapRenderer`クラスでは、`OnElementChanged`メソッドで、対応するネイティブ コントロールを表示するために、Xamarin.Forms カスタム マップが作成されたときに呼び出されます。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティ。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラー*が*に接続されていると Xamarin.Forms 要素をレンダラー*は*に、それぞれに接続されています。 サンプル アプリケーションでは、`OldElement`プロパティになります`null`と`NewElement`プロパティへの参照には、`CustomMap`インスタンス。

オーバーライドされたバージョン、`OnElementChanged`各プラットフォームに固有のレンダラー クラスのメソッドはネイティブ コントロールのカスタマイズを実行する場所です。 を介してアクセスできる、プラットフォームで使用されているネイティブ コントロールへの参照を型指定された、`Control`プロパティ。 さらでレンダリングされている Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティ。

内のイベント ハンドラーをサブスクライブしているときは注意する必要がある、`OnElementChanged`メソッドは、次のコード例で示した。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
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

ネイティブ コントロールを構成する必要があり、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられている場合にのみにイベント ハンドラーをサブスクライブします。 同様に、サブスクライブされたイベント ハンドラーがサブスクリプションを解除するだけに、レンダラーが関連付けられている要素が変更されたとき。 このアプローチを採用すると、メモリ リークが発生しませんが、カスタム レンダラーを作成するのに役立ちます。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms カスタム コントロールの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

次のスクリーン ショットは、カスタマイズの前後に、マップを表示します。

![](customized-pin-images/map-layout-ios.png "マップ コントロールの前に、と後のカスタマイズ")

IOS、暗証番号 (pin) が呼び出された、*注釈*、カスタム イメージまたはさまざまな色のシステム定義の pin のいずれかを指定できます。 注釈を表示することができます必要に応じて、*コールアウト*注釈を選択すると、ユーザーへの応答に表示されます。 引き出し線の表示、`Label`と`Address`のプロパティ、`Pin`の省略可能な左と右アクセサリ ビューのインスタンス。 左側のアクセサリ ビューが右アクセサリのビューが、monkey のイメージは上記のスクリーン ショットで、*情報*ボタンをクリックします。

次のコード例では、iOS プラットフォーム用のカスタム レンダラーを示します。

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

`OnElementChanged`メソッドは、次の構成の実行、 [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/)インスタンス、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること。

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/)プロパティに設定されて、`GetViewForAnnotation`メソッド。 このメソッドが呼び出されます、[注釈の位置が、マップに表示される](#Displaying_the_Annotation)、表示する注釈の前にカスタマイズするために使用します。
- イベント ハンドラー、 `CalloutAccessoryControlTapped`、 `DidSelectAnnotationView`、および`DidDeselectAnnotationView`イベントが登録されます。 これらのイベントを発生させるときにユーザー[引き出し線の右側のアクセサリをタップ](#Tapping_on_the_Right_Callout_Accessory_View)とタイミング ユーザー[選択](#Selecting_the_Annotation)と[の選択を解除](#Deselecting_the_Annotation)注釈それぞれ。 イベントは、レンダラーでは、要素が変更に関連付けられている場合にのみのサブスクライブが解除されます。

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>注釈を表示します。

`GetViewForAnnotation`注釈の位置が、マップに表示されると表示する注釈の前にカスタマイズするために使用メソッドが呼び出されます。 注釈には、2 つの部分があります。

- `MkAnnotation` – タイトル、サブタイトル、および注釈の場所が含まれています。
- `MkAnnotationView` – 注釈、および必要に応じて、ユーザーが、注釈をタップしたときに表示される引き出し線を表すイメージが含まれています。

`GetViewForAnnotation`メソッドは、`IMKAnnotation`を注釈のデータを返す、`MKAnnotationView`マップ上に表示して、次のコード例に示した。

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

このメソッドにより、カスタム イメージとして注釈が表示されますではなくシステム定義の pin として注釈がタップされたコールアウトが表示されます、注釈のタイトルとアドレスの右側と左側に追加のコンテンツが含まれる. これは、次のように実現されます。

1. `GetCustomPin`注釈のピン留めするカスタム データを返すメソッドが呼び出されます。
1. 注釈の表示がへの呼び出しで再利用するためにプール メモリを節約するために[ `DequeueReusableAnnotation`](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/)します。
1. `CustomMKAnnotationView`クラスを拡張、`MKAnnotationView`クラス`Id`と`Url`のと同じプロパティに対応するプロパティ、`CustomPin`インスタンス。 新しいインスタンス、`CustomMKAnnotationView`が作成、注釈が`null`:
  - `CustomMKAnnotationView.Image`プロパティがマップに注釈を表すイメージに設定します。
  - `CustomMKAnnotationView.CalloutOffset`プロパティに設定されて、`CGPoint`引き出し線は、注釈の上中央に配置することを指定します。
  - `CustomMKAnnotationView.LeftCalloutAccessoryView`プロパティ注釈のタイトルとアドレスの左側に表示される monkey のイメージに設定されます。
  - `CustomMKAnnotationView.RightCalloutAccessoryView`プロパティに設定されて、*情報*注釈タイトルとアドレスの右側に表示されるボタンをクリックします。
  - `CustomMKAnnotationView.Id`プロパティに設定されて、`CustomPin.Id`プロパティによって返される、`GetCustomPin`メソッド。 これによりが識別できる注釈[吹き出しをさらにカスタマイズできます](#Selecting_the_Annotation)必要な場合、します。
  - `CustomMKAnnotationView.Url`プロパティに設定されて、`CustomPin.Url`プロパティによって返される、`GetCustomPin`メソッド。 URL をナビゲートするときに、ユーザー[正しいコールアウトのアクセサリ ビューで表示されるボタンのタップ](#Tapping_on_the_Right_Callout_Accessory_View)。
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/)プロパティに設定されて`true`注釈がタップされたときに、吹き出しが表示されます。
1. マップの表示の注釈が返されます。

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>注釈の選択

ユーザーが、注釈をタップしたときに、`DidSelectAnnotationView`が実行されるイベントの起動、`OnDidSelectAnnotationView`メソッド。

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

このメソッドを追加して (左と右アクセサリのビューを含みます) を既存の引き出し線の拡張、`UIView`選択したコメントを持っていれば、Xamarin のロゴのイメージを含むインスタンスにその`Id`に設定するプロパティ`Xamarin`. これにより、シナリオ別の注釈のさまざまな吹き出しを表示できます。 `UIView`既存の引き出し線の上に中央揃えのインスタンスが表示されます。

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>右側の吹き出しアクセサリ ビューをタップします。

ユーザーがタップしたときに、*情報*正しいコールアウトのアクセサリ ビューでは、ボタン、`CalloutAccessoryControlTapped`が実行されるイベントの起動、`OnCalloutAccessoryControlTapped`メソッド。

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

このメソッドが web ブラウザーを開きに格納されているアドレスに移動、`CustomMKAnnotationView.Url`プロパティ。 作成するときに、アドレスが定義されたに注意してください、 `CustomPin` .NET Standard ライブラリ プロジェクト内のコレクション。

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>注釈の選択を解除

注釈が表示され、マップで、ユーザーがタップしたときに、`DidDeselectAnnotationView`が実行されるイベントの起動、`OnDidDeselectAnnotationView`メソッド。

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

このメソッドを確認すると、既存の引き出し線が選択されていないコールアウト (Xamarin ロゴのイメージ) の拡張の一部が表示されているを停止しても、そのリソースが解放されます。

カスタマイズの詳細については、`MKMapView`インスタンスは、「 [iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)します。

### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

次のスクリーン ショットは、カスタマイズの前後に、マップを表示します。

![](customized-pin-images/map-layout-android.png "マップ コントロールの前に、と後のカスタマイズ")

Android、暗証番号 (pin) が呼び出された、*マーカー*、カスタム イメージまたはさまざまな色のシステム定義のマーカーを使用して指定できます。 マーカーを表示することができます、*情報ウィンドウ*マーカーのタップして、ユーザーへの応答に表示されます。 情報ウィンドウが表示されます、`Label`と`Address`のプロパティ、`Pin`インスタンス、およびその他のコンテンツを含めるにカスタマイズできます。 ただし、一度に 1 つだけの情報ウィンドウを表示できます。

次のコード例では、Android プラットフォーム用のカスタム レンダラーを示します。

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

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること、`OnElementChanged`メソッドの呼び出し、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`は、ビューに関連付けられています。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`オーバーライドが呼び出されます。 このメソッドのイベント ハンドラーの登録、`InfoWindowClick`ときに発生するイベント、[情報ウィンドウがクリックされた](#Clicking_on_the_Info_Window)レンダラーでは、要素が変更に関連付けられている場合にのみにから登録解除とします。 `OnMapReady`呼び出しでオーバーライドも、`SetInfoWindowAdapter`ことを指定するメソッド、`CustomMapRenderer`クラスのインスタンスは、情報ウィンドウをカスタマイズする方法を提供します。

`CustomMapRenderer`クラスが実装する、`GoogleMap.IInfoWindowAdapter`インターフェイス[情報ウィンドウをカスタマイズする](#Customizing_the_Info_Window)します。 このインターフェイスは、次のメソッドを実装する必要がありますを指定します。

- `public Android.Views.View GetInfoWindow(Marker marker)` – このメソッドはマーカーのカスタム情報ウィンドウを取得します。 返された場合`null`既定のウィンドウのレンダリングが使用されます。 返された場合、 `View`、しを`View`情報ウィンドウ フレーム内に配置されます。
- `public Android.Views.View GetInfoContents(Marker marker)` – このメソッドが返すために呼び出さ、`View`情報 ウィンドウの内容を格納していると、場合にのみ呼び出すことは、`GetInfoWindow`メソッドを返します。`null`します。 返された場合`null`、情報ウィンドウのコンテンツの既定のレンダリングが使用されます。

サンプル アプリケーションでは、情報ウィンドウのコンテンツのみをカスタマイズ、そのため、`GetInfoWindow`メソッドを返します。`null`これを有効にします。

#### <a name="customizing-the-marker"></a>マーカーのカスタマイズ

マーカーを表すために使用するアイコンを呼び出すことによってカスタマイズできる、`MarkerOptions.SetIcon`メソッド。 これは、オーバーライドすることで実現できます、`CreateMarker`ごとに呼び出されるメソッドは、`Pin`マップに追加されています。

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

このメソッドは、新しい作成`MarkerOption`インスタンスごとに`Pin`インスタンス。 アイコンを設定する位置、ラベル、およびマーカーのアドレスを設定した後、`SetIcon`メソッド。 このメソッドは、`BitmapDescriptor`で、アイコンを表示するために必要なデータを格納しているオブジェクト、`BitmapDescriptorFactory`の作成を簡略化のヘルパー メソッドを提供するクラス、`BitmapDescriptor`します。

使用しての詳細については、`BitmapDescriptorFactory`マーカーをカスタマイズするを参照してくださいクラス[マーカーをカスタマイズする](~/android/platform/maps-and-location/maps/maps-api.md)します。

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>情報ウィンドウのカスタマイズ

ユーザーは、マーカーをタップしたとき、`GetInfoContents`メソッドが実行されている、`GetInfoWindow`メソッドを返します。`null`します。 次のコード例は、`GetInfoContents`メソッド。

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

    if (customPin.Id.ToString() == "Xamarin") {
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

このメソッドが戻る、`View`情報ウィンドウの内容を格納しています。 これは、次のように実現されます。

- A`LayoutInflater`インスタンスを取得します。 これに対応するレイアウトの XML ファイルをインスタンス化に使用されます`View`します。
- `GetCustomPin`情報ウィンドウにピン留めするカスタム データを返すメソッドが呼び出されます。
- `XamarinMapInfoWindow`場合、レイアウトが大きく、`CustomPin.Id`プロパティは等しく`Xamarin`します。 それ以外の場合、`MapInfoWindow`レイアウトが膨らみます。 これにより、シナリオのさまざまなマーカーのウィンドウ レイアウトのさまざまな情報を表示できます。
- `InfoWindowTitle`と`InfoWindowSubtitle`リソースが高めのレイアウトから取得して、その`Text`プロパティから対応するデータに設定されて、`Marker`インスタンス、リソースがないこと`null`します。
- `View`マップに表示するインスタンスが返されます。

> [!NOTE]
> 情報ウィンドウがライブ`View`します。 Android を代わりに、変換は、`View`を静的なビットマップし、それをイメージとして表示します。 つまり、中、情報ウィンドウにすべてのタッチ イベントまたはジェスチャに応答できないし、情報ウィンドウには、個々 のコントロールは、独自に応答できません、click イベントに応答できるは、イベントをクリックします。

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>情報ウィンドウをクリックすると

情報ウィンドウで、ユーザーがクリックしたときに、`InfoWindowClick`が実行されるイベントの起動、`OnInfoWindowClick`メソッド。

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

このメソッドが web ブラウザーを開きに格納されているアドレスに移動、`Url`プロパティを取得して、`CustomPin`インスタンスに対して、`Marker`します。 作成するときに、アドレスが定義されたに注意してください、 `CustomPin` .NET Standard ライブラリ プロジェクト内のコレクション。

カスタマイズの詳細については、`MapView`インスタンスは、「[マップ API](~/android/platform/maps-and-location/maps/maps-api.md)します。

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームでのカスタム レンダラーの作成

次のスクリーン ショットは、カスタマイズの前後に、マップを表示します。

![](customized-pin-images/map-layout-uwp.png "マップ コントロールの前に、と後のカスタマイズ")

UWP、暗証番号 (pin) が呼び出された、*マップ アイコン*、カスタム イメージまたはシステム定義の既定のイメージを使用して指定できます。 マップ アイコンを表示することができます、 `UserControl`、マップ アイコンをタップして、ユーザーへの応答に表示されます。 `UserControl` 、任意のコンテンツを表示できるなど、`Label`と`Address`のプロパティ、`Pin`インスタンス。

次のコード例では、UWP のカスタム レンダラーを示します。

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

`OnElementChanged`カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされているメソッドは、次の操作を実行します。

- クリア、 `MapControl.Children` 、マップからのイベント ハンドラーを登録する前に既存のユーザー インターフェイス要素を削除するコレクション、`MapElementClick`イベント。 このイベント、ユーザーがタップまたはクリックしたときに発生、`MapElement`上、`MapControl`レンダラーでは、要素が変更に関連付けられている場合にのみにから登録解除とします。
- 各ピンで、`customPins`コレクションは、マップ上の適切な地理的な場所にある次のように表示されます。
  - ピンの場所として作成、`Geopoint`インスタンス。
  - A`MapIcon`暗証番号 (pin) を表すインスタンスを作成します。
  - 表すために使用されるイメージ、`MapIcon`設定で指定された、`MapIcon.Image`プロパティ。 ただし、マップ アイコンのイメージは常に保証されませんに表示されるように、マップ上の他の要素が隠される可能性があります。 そのため、マップ アイコンの`CollisionBehaviorDesired`プロパティに設定されて`MapElementCollisionBehavior.RemainVisible`をそのまま表示されていることを確認します。
  - 場所、`MapIcon`設定で指定された、`MapIcon.Location`プロパティ。
  - `MapIcon.NormalizedAnchorPoint`プロパティが、イメージの上にポインターのおおよその場所に設定します。 このプロパティがイメージの左上隅を表す (0, 0) の既定値を保持する場合、マップのズーム レベルの変更、イメージを別の場所を指している可能性があります。
  - `MapIcon`インスタンスに追加される、`MapControl.MapElements`コレクション。 これは、結果に表示されるマップ アイコン、`MapControl`します。

> [!NOTE]
> 複数のマップ アイコンを同じイメージを使用する場合、`RandomAccessStreamReference`インスタンスは、最適なパフォーマンス ページまたはアプリケーション レベルで宣言する必要があります。

#### <a name="displaying-the-usercontrol"></a>ユーザー コントロールを表示します。

[マップ] アイコンをタップすると、`OnMapElementClick`メソッドが実行されます。 次のコード例では、このメソッドは示しています。

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

            if (customPin.Id.ToString() == "Xamarin")
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

このメソッドを作成、 `UserControl` pin に関する情報を表示するインスタンス。 これは、次のように実現されます。

- `MapIcon`インスタンスを取得します。
- `GetCustomPin`表示されるピン留めするカスタム データを返すメソッドが呼び出されます。
- A`XamarinMapOverlay`ピン留めするカスタム データを表示するインスタンスが作成されます。 このクラスは、ユーザー コントロールです。
- 表示する地理的な場所、`XamarinMapOverlay`インスタンス、`MapControl`として作成されて、`Geopoint`インスタンス。
- `XamarinMapOverlay`インスタンスに追加される、`MapControl.Children`コレクション。 このコレクションには、マップに表示される XAML ユーザー インターフェイス要素が含まれています。
- 地理的場所、 `XamarinMapOverlay` 、マップ上のインスタンスが呼び出すことによって設定、`SetLocation`メソッド。
- 上の相対位置、`XamarinMapOverlay`呼び出すことによって、指定した場所に対応する、インスタンスが設定されて、`SetNormalizedAnchorPoint`メソッド。 これにより、マップの結果でのズーム レベルに変更を`XamarinMapOverlay`の正しい場所に常に表示されているインスタンスします。

また、pin に関する情報がマップに表示されて場合、マップをタップが削除されます。、`XamarinMapOverlay`インスタンスから、`MapControl.Children`コレクション。

#### <a name="tapping-on-the-information-button"></a>情報ボタンをタップします。

ユーザーをタップすると、*情報*ボタン、`XamarinMapOverlay`ユーザー コントロール、`Tapped`が実行されるイベントの起動、`OnInfoButtonTapped`メソッド。

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

このメソッドが web ブラウザーを開きに格納されているアドレスに移動、`Url`のプロパティ、`CustomPin`インスタンス。 作成するときに、アドレスが定義されたに注意してください、 `CustomPin` .NET Standard ライブラリ プロジェクト内のコレクション。

カスタマイズの詳細については、`MapControl`インスタンスは、「[マップと場所の概要](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx)msdn です。

## <a name="summary"></a>まとめ

この記事では、用のカスタム レンダラーを作成する方法を示しました、`Map`コントロール、開発者は独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドします。 Xamarin.Forms.Maps を高速で使い慣れたマップを提供する各プラットフォームでの Api が発生するネイティブのマップを使用して、ユーザーのマップを表示するため、クロス プラットフォームの抽象化を提供します。


## <a name="related-links"></a>関連リンク

- [マップ コントロール](~/xamarin-forms/user-interface/map.md)
- [iOS のマップ](~/ios/user-interface/controls/ios-maps/index.md)
- [Maps API](~/android/platform/maps-and-location/maps/maps-api.md)
- [カスタマイズされた Pin (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
