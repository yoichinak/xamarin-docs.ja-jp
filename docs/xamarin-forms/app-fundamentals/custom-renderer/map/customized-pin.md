---
title: マップの暗証番号 (pin) をカスタマイズします。
description: この記事では、プラットフォームごとにカスタマイズした pin と暗証番号 (pin) のデータのカスタマイズされたビューを持つネイティブ マップを表示するマップ コントロールのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 353575bad91c9bade0207a0aa271d9de7ec50240
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-map-pin"></a>マップの暗証番号 (pin) をカスタマイズします。

_この記事では、プラットフォームごとにカスタマイズした pin と暗証番号 (pin) のデータのカスタマイズされたビューを持つネイティブ マップを表示するマップ コントロールのカスタム レンダラーを作成する方法を示します。_

各 Xamarin.Forms ビューには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) ios では、Xamarin.Forms アプリケーションによって表示される、`MapRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`MKMapView`コントロール。 Android のプラットフォームでは、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`MapRenderer`クラスのインスタンスを作成、ネイティブな`MapControl`します。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)とそれを実装する、対応するネイティブ コントロール。

![](customized-pin-images/map-classes.png "マップ コントロールと実装のネイティブ コントロール間のリレーションシップ")

カスタマイズを実装するプラットフォーム固有のカスタム レンダラーを作成することで、表示プロセスを使用できます、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Map)Xamarin.Forms カスタム マップします。
1. [消費](#Consuming_the_Custom_Map)Xamarin.Forms からカスタムのマップ。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームで、マップのカスタム レンダラーです。

各項目を実装する順番に説明するようになりましたが、`CustomMap`レンダラー プラットフォームごとにカスタマイズした pin と暗証番号 (pin) のデータのカスタマイズされたビューのネイティブのマップを表示します。

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) 初期化して使用する前に構成されている必要があります。 詳細については、「[`Maps Control`](~/xamarin-forms/user-interface/map.md)」を参照してください。

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>カスタム マップを作成します。

サブクラス化して、カスタムのマップ コントロールを作成することができます、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)クラスに、次のコード例に示すようにします。

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap`コントロールは、ポータブル クラス ライブラリ (PCL) プロジェクトが作成され、カスタム マップの API を定義します。 カスタムのマップを公開、`CustomPins`プロパティのコレクションを表す`CustomPin`各プラットフォームでネイティブ マップ コントロールによって表示されるオブジェクト。 `CustomPin`クラスが次のコード例に示すようにします。

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

このクラスを定義、`CustomPin`のプロパティを継承するように、 [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/)クラス、および追加する、`Url`プロパティです。

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>カスタムのマップの使用

`CustomMap`コントロールで参照できます XAML PCL プロジェクト内の場所の名前空間の宣言してカスタム マップ コントロールに名前空間プレフィックスを使用します。 次のコード例に示す方法、`CustomMap`コントロールを XAML ページで利用できることができます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム マップの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタムのマップを参照します。

次のコード例に示す方法、`CustomMap`コントロールは、c# のページで利用できることができます。

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

`CustomMap`インスタンスを各プラットフォームでネイティブのマップを表示するために使用されます。 [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/)プロパティの表示スタイルを設定する、 [ `Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)で定義されている値を含む、 [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/)列挙します。 IOS と Android の場合は、幅と高さマップの設定のプロパティを介して、`App`がプラットフォーム固有のプロジェクトで初期化されるクラスです。

マップ、およびピンの場所が含まれていますの次のコード例に示すように初期化されます。

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

この初期化は、独自の pin を追加し、マップのビューを配置、 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/)を作成することで位置と、マップのズーム レベルを変更するメソッドを[ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) から[`Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)と[ `Distance`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)です。

カスタム レンダラーは、ネイティブ マップ コントロールをカスタマイズするには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでは、カスタム レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`MapRenderer`カスタム マップを表示するクラス。
1. 上書き、`OnElementChanged`をカスタマイズする書き込みロジックとカスタムのマップを表示するメソッド。 このメソッドは、Xamarin.Forms の対応する、カスタム マップの作成時に呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms カスタム マップを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> 各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能であります。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。

次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](customized-pin-images/solution-structure.png "CustomMap カスタム レンダラーのプロジェクトの責任")

`CustomMap`から派生したプラットフォームに固有のレンダラー クラスによってコントロールが表示される、`MapRenderer`各プラットフォームのクラスです。 これは、結果、各`CustomMap`次のスクリーン ショットに示すように、プラットフォーム固有のコントロールに表示されるを制御します。

![](customized-pin-images/screenshots.png "各プラットフォームで CustomMap")

`MapRenderer`クラスが公開、 `OnElementChanged` Xamarin.Forms のカスタム マップが、対応するネイティブ コントロールを表示するために作成されるときに呼び出されるメソッド。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティです。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラーでは、*が*に接続されていると Xamarin.Forms の要素をレンダラーでは、*は*に、それぞれをアタッチします。 サンプル アプリケーションで、`OldElement`プロパティ`null`と`NewElement`プロパティへの参照が格納されます、`CustomMap`インスタンス。

オーバーライドのバージョン、`OnElementChanged`メソッドは、各プラットフォームに固有のレンダラー クラスでは、ネイティブ コントロールのカスタマイズを実行する場所です。 型指定されたコントロールへの参照、ネイティブ プラットフォームで使用されている経由でアクセスできる、`Control`プロパティです。 さらに、を通じて表示される Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティです。

注意が必要でイベント ハンドラーをサブスクライブしているとき、`OnElementChanged`メソッドを次のコード例で示したようにします。

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

ネイティブのコントロールを構成する必要があり、イベント ハンドラーは、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられている場合にのみにサブスクライブします。 同様に、イベント ハンドラーをサブスクライブしていたをすることはできませんのみに、レンダラーがアタッチされている要素が変更されたときからの購読します。 このアプローチを採用することは、メモリ リークは発生しないこと、カスタム レンダラーを作成するのに役立ちます。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、–、表示される Xamarin.Forms カスタム コントロールの型名とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

次のスクリーン ショットは、カスタマイズの前後に、マップを表示します。

![](customized-pin-images/map-layout-ios.png "マップ コントロールの前に、と後のカスタマイズ")

IOS pin と呼ばれる、*注釈*、カスタム イメージまたはさまざまな色のシステム定義の pin のいずれかを指定できます。 注釈を表示することができます必要に応じて、*コールアウト*注釈を選択すると、ユーザーへの応答に表示されます。 引き出し線を表示、`Label`と`Address`のプロパティ、`Pin`省略可能な左と右付属品のビューのインスタンス。 上記のスクリーン ショット、左側の付属品ビューは、サルのイメージで右付属品のビューは、*情報*ボタンをクリックします。

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

`OnElementChanged`メソッドは、次の構成の実行、 [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/)インスタンス、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされています。

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/)プロパティに設定されている、`GetViewForAnnotation`メソッドです。 このメソッドが呼び出されます、[注釈の位置が、マップに表示される](#Displaying_the_Annotation)、表示する前に、注釈をカスタマイズするために使用されます。
- イベント ハンドラー、 `CalloutAccessoryControlTapped`、 `DidSelectAnnotationView`、および`DidDeselectAnnotationView`イベントが登録されます。 これらのイベントの発生時にユーザー[引き出し線の右側のアクセサリをタップ](#Tapping_on_the_Right_Callout_Accessory_View)とタイミング ユーザー[選択](#Selecting_the_Annotation)と[の選択を解除](#Deselecting_the_Annotation)注釈、それぞれします。 イベントは、レンダラーでは、要素が変更に関連付けられている場合にのみからの購読されません。

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>注釈を表示します。

`GetViewForAnnotation`メソッドは、注釈の位置が、マップに表示になりを表示する前に、注釈をカスタマイズするために使用します。 注釈には、2 つの部分があります。

- `MkAnnotation` – タイトル、サブタイトル、および注釈の位置が含まれています。
- `MkAnnotationView` – 注釈、および必要に応じて、ユーザーが注釈をタップしたときに表示されているコールアウトを表すイメージが含まれています。

`GetViewForAnnotation`メソッドを受け入れる、`IMKAnnotation`を注釈のデータを格納して返す、`MKAnnotationView`マップに表示し、次のコード例に示します。

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

このメソッドを確認する、注釈は、カスタム イメージとして表示されますではなくシステム定義暗証番号 (pin)、およびと注釈がタップされたときに、引き出し線が表示されます注釈のタイトルとアドレスの右側および左側に追加のコンテンツが含まれる. これは、次のように行われます。

1. `GetCustomPin`注釈の独自の pin のデータを返すメソッドが呼び出されます。
1. 呼び出しで再利用できるようにプールされている注釈の表示メモリを節約するために[ `DequeueReusableAnnotation`](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/)です。
1. `CustomMKAnnotationView`クラスを拡張、`MKAnnotationView`クラス`Id`と`Url`で同じプロパティに対応するプロパティ、`CustomPin`インスタンス。 新しいインスタンス、`CustomMKAnnotationView`作成されると、注釈がある`null`:
  - `CustomMKAnnotationView.Image`プロパティがマップに注釈を表すイメージに設定します。
  - `CustomMKAnnotationView.CalloutOffset`プロパティに設定されている、`CGPoint`引き出し線を注釈の上中央に配置することを指定します。
  - `CustomMKAnnotationView.LeftCalloutAccessoryView`プロパティは、注釈のタイトルとアドレスの左側に表示されるサルのイメージに設定します。
  - `CustomMKAnnotationView.RightCalloutAccessoryView`プロパティに設定されている、*情報*注釈のタイトルとアドレスの右側に表示されるボタンをクリックします。
  - `CustomMKAnnotationView.Id`プロパティに設定されている、`CustomPin.Id`プロパティから返される、`GetCustomPin`メソッドです。 これにより、を持つように識別できる注釈[引き出し、さらにカスタマイズできます](#Selecting_the_Annotation)必要な場合、します。
  - `CustomMKAnnotationView.Url`プロパティに設定されている、`CustomPin.Url`プロパティから返される、`GetCustomPin`メソッドです。 ときに移動する URL、ユーザー[が右のコールアウト アクセサリ ビューに表示されるボタンをタップ](#Tapping_on_the_Right_Callout_Accessory_View)です。
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/)プロパティに設定されている`true`注釈がタップされたときに、引き出し線が表示されないようにします。
1. マップに表示するため、注釈が返されます。

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>注釈を選択します。

ユーザーが、目的のコメントがタップすると、`DidSelectAnnotationView`が実行されるイベントの起動、`OnDidSelectAnnotationView`メソッド。

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

このメソッドを追加して (を左と右付属品のビューを含む) 既存の引き出し線の拡張、`UIView`を選択した注釈を持っていれば、Xamarin のロゴのイメージを含むインスタンスにその`Id`プロパティに設定する`Xamarin`. これにより、シナリオが異なるコールアウトを異なる注釈を表示できます。 `UIView`既存の引き出し線の上の中央のインスタンスが表示されます。

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>右側のコールアウト アクセサリ ビューをタップします。

ユーザーをタップすると、*情報*右コールアウト アクセサリ ビューで、ボタン、`CalloutAccessoryControlTapped`が実行されるイベントの起動、`OnCalloutAccessoryControlTapped`メソッド。

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

このメソッドは、web ブラウザーを起動しに格納されているアドレスに移動、`CustomMKAnnotationView.Url`プロパティです。 作成するときにアドレスが定義されたことに注意してください、 `CustomPin` PCL プロジェクト内のコレクション。

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>注釈の選択を解除します。

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

このメソッド確実に既存のコールアウトが選択されていないとコールアウト (Xamarin ロゴのイメージ) の拡張の一部が表示されているを停止しても、そのリソースは解放されます。

カスタマイズの詳細については、`MKMapView`インスタンスは、「 [iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)です。

### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

次のスクリーン ショットは、カスタマイズの前後に、マップを表示します。

![](customized-pin-images/map-layout-android.png "マップ コントロールの前に、と後のカスタマイズ")

Android で、暗証番号 (pin) と呼ばれる、*マーカー*、カスタム イメージまたはさまざまな色のシステム定義のマーカーを使用して指定できます。 マーカーを表示することができます、*情報ウィンドウ*マーカーをタップすると、ユーザーへの応答に表示されます。 情報ウィンドウが表示されます、`Label`と`Address`のプロパティ、`Pin`インスタンス、およびその他のコンテンツを含むようにカスタマイズできます。 ただし、一度に 1 つだけの情報ウィンドウを表示できます。

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

カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされている、`OnElementChanged`メソッドの呼び出し、`MapView.GetMapAsync`メソッドは、基になる`GoogleMap`をビューに関連付けられています。 1 回、`GoogleMap`インスタンスが、使用可能な`OnMapReady`オーバーライドが呼び出されます。 このメソッドのイベント ハンドラーの登録、`InfoWindowClick`ときに発生するイベント、[情報ウィンドウがクリックされた](#Clicking_on_the_Info_Window)レンダラーでは、要素が変更に関連付けられている場合にのみ、購読解除とします。 `OnMapReady`オーバーライドの呼び出しではまた、`SetInfoWindowAdapter`ことを指定するメソッド、`CustomMapRenderer`クラスのインスタンスは、情報ウィンドウをカスタマイズする方法を提供します。

`CustomMapRenderer`クラスが実装する、`GoogleMap.IInfoWindowAdapter`インターフェイスを[情報ウィンドウをカスタマイズする](#Customizing_the_Info_Window)です。 このインターフェイスは、次のメソッドを実装する必要があるかを指定します。

- `public Android.Views.View GetInfoWindow(Marker marker)` このメソッドは、マーカーのカスタム情報ウィンドウに戻すに呼び出されます。 返された場合`null`、その既定のウィンドウのレンダリングが使われます。 返す場合、 `View`、しを`View`情報ウィンドウ フレーム内に配置されます。
- `public Android.Views.View GetInfoContents(Marker marker)` – 返すこのメソッドは、`View`情報ウィンドウの内容が含まれていると、場合にのみ呼び出すことは、`GetInfoWindow`メソッドを返します。`null`です。 返された場合`null`、その情報のウィンドウ コンテンツの既定のレンダリングが使われます。

サンプル アプリケーションの情報ウィンドウのコンテンツのみをカスタマイズするとため、`GetInfoWindow`メソッドを返します。`null`これを有効にします。

#### <a name="customizing-the-marker"></a>マーカーのカスタマイズ

マーカーを表すために使用するアイコンを呼び出すことによってカスタマイズできる、`MarkerOptions.SetIcon`メソッドです。 これには、オーバーライドすることで、`CreateMarker`ごとに呼び出されるメソッド`Pin`マップに追加します。

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

このメソッドは、新しい作成`MarkerOption`ごとにインスタンス`Pin`インスタンス。 設定されているアイコンの位置、ラベル、およびマーカーのアドレスを設定した後、`SetIcon`メソッドです。 このメソッドは、`BitmapDescriptor`で、アイコンを表示するために必要なデータを格納しているオブジェクト、`BitmapDescriptorFactory`の作成を簡略化のヘルパー メソッドを提供するクラス、`BitmapDescriptor`です。

使用しての詳細については、`BitmapDescriptorFactory`マーカーをカスタマイズするを参照してくださいクラス[マーカーをカスタマイズする](~/android/platform/maps-and-location/maps/maps-api.md)です。

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>情報ウィンドウのカスタマイズ

ユーザーが、マーカーをタップしたときに、`GetInfoContents`メソッドを実行すると、提供する、`GetInfoWindow`メソッドを返します。`null`です。 次のコード例は、`GetInfoContents`メソッド。

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

このメソッドが戻る、`View`情報ウィンドウの内容を含むです。 これは、次のように行われます。

- A`LayoutInflater`インスタンスを取得します。 これに対応するレイアウトの XML ファイルをインスタンス化に使用`View`です。
- `GetCustomPin`情報ウィンドウにカスタムの暗証番号 (pin) データを返すメソッドが呼び出されます。
- `XamarinMapInfoWindow`場合、レイアウトが大きく、`CustomPin.Id`プロパティと等しい`Xamarin`です。 それ以外の場合、`MapInfoWindow`レイアウトを拡大します。 これにより、シナリオのさまざまなマーカーのウィンドウ レイアウトのさまざまな情報を表示できます。
- `InfoWindowTitle`と`InfoWindowSubtitle`リソースは、高めのレイアウトから取得され、その`Text`プロパティから対応するデータに設定されて、`Marker`するリソースがありませんが、インスタンス`null`です。
- `View`マップの表示のインスタンスが返されます。

> [!NOTE]
> 情報ウィンドウがライブ`View`です。 Android を代わりに、変換、`View`静的ビットマップし、それをイメージとして表示します。 つまり、情報ウィンドウを中にタッチ イベントまたはジェスチャに応答できないし、情報ウィンドウに個別のコントロールは、独自に応答できません、click イベントに応答できるは、イベントをクリックします。

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

このメソッドは、web ブラウザーを起動しに格納されているアドレスに移動、`Url`プロパティを取得して、`CustomPin`インスタンスの場合、`Marker`です。 作成するときにアドレスが定義されたことに注意してください、 `CustomPin` PCL プロジェクト内のコレクション。

カスタマイズの詳細については、`MapView`インスタンスは、「[マップ API](~/android/platform/maps-and-location/maps/maps-api.md)です。

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>ユニバーサル Windows プラットフォームには、カスタム レンダラーを作成します。

次のスクリーン ショットは、カスタマイズの前後に、マップを表示します。

![](customized-pin-images/map-layout-uwp.png "マップ コントロールの前に、と後のカスタマイズ")

UWP、暗証番号 (pin) と呼ばれる、*マップ アイコン*、カスタム イメージまたはシステム定義の既定のイメージを使用して指定できます。 マップのアイコンを表示することができます、`UserControl`マップのアイコンをタップすると、ユーザーへの応答に表示されます。 `UserControl`任意のコンテンツを表示できますを含め、`Label`と`Address`のプロパティ、`Pin`インスタンス。

次のコード例は、UWP カスタム レンダラーを示しています。

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

`OnElementChanged`メソッドは、カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされている、次の操作を実行します。

- クリア、 `MapControl.Children` 、マップからのイベント ハンドラーを登録する前に既存のユーザー インターフェイス要素を削除するコレクション、`MapElementClick`イベント。 このイベントは、ユーザーがタップまたはクリックしたときに発生、`MapElement`で、`MapControl`レンダラーでは、要素が変更に関連付けられている場合にのみ、購読解除とします。
- それぞれの暗証番号 (pin)、`customPins`コレクションがマップ上の正しい地理的な場所に次のように表示されます。
  - ピンの場所として作成、`Geopoint`インスタンス。
  - A`MapIcon`暗証番号 (pin) を表すインスタンスが作成されます。
  - 表すために使用される画像、`MapIcon`設定で指定された、`MapIcon.Image`プロパティです。 ただし、マップのアイコンの画像は常に限りませんに表示されるようにマップの他の要素が隠される可能性があります。 そのため、マップ アイコンの`CollisionBehaviorDesired`プロパティに設定されている`MapElementCollisionBehavior.RemainVisible`をそのまま表示されていることを確認します。
  - 場所、`MapIcon`設定で指定された、`MapIcon.Location`プロパティです。
  - `MapIcon.NormalizedAnchorPoint`プロパティは、画像上にポインターのおおよその場所に設定します。 このプロパティが既定値 (0, 0) のイメージの左上隅を表すを保持する場合、マップのズーム レベルの変更、イメージを別の場所を指している可能性があります。
  - `MapIcon`インスタンスに追加される、`MapControl.MapElements`コレクション。 これは、結果に表示されるマップ アイコンで、`MapControl`です。

> [!NOTE]
> 複数のマップのアイコンを同じイメージを使用する場合、`RandomAccessStreamReference`インスタンスは、最適なパフォーマンスをページまたはアプリケーション レベルで宣言する必要があります。

#### <a name="displaying-the-usercontrol"></a>ユーザー コントロールを表示します。

[マップ] アイコンをタップすると、`OnMapElementClick`メソッドを実行します。 次のコード例では、このメソッドを示します。

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

このメソッドを作成、`UserControl`暗証番号 (pin) に関する情報を表示するインスタンス。 これは、次のように行われます。

- `MapIcon`インスタンスを取得します。
- `GetCustomPin`表示されるカスタムの暗証番号 (pin) データを返すメソッドが呼び出されます。
- A`XamarinMapOverlay`カスタム暗証番号 (pin) のデータを表示するインスタンスを作成します。 このクラスは、ユーザー コントロールです。
- 地理的な場所を表示する、`XamarinMapOverlay`インスタンス、`MapControl`として作成された、`Geopoint`インスタンス。
- `XamarinMapOverlay`インスタンスに追加される、`MapControl.Children`コレクション。 このコレクションには、マップに表示される XAML ユーザー インターフェイス要素が含まれています。
- 地理的な場所、`XamarinMapOverlay`呼び出すことによって、マップ上のインスタンスを設定、`SetLocation`メソッドです。
- 上の相対位置、`XamarinMapOverlay`呼び出すことによって、指定した場所に対応するインスタンスを設定、`SetNormalizedAnchorPoint`メソッドです。 これにより、マップ内の結果のズーム レベルに変更を`XamarinMapOverlay`の正しい場所に常に表示されているインスタンスします。

代わりに場合、暗証番号 (pin) に関する情報がマップに表示されて、マップ上のタップを削除します、`XamarinMapOverlay`インスタンスから、`MapControl.Children`コレクション。

#### <a name="tapping-on-the-information-button"></a>[情報] ボタンをタップします。

ユーザーをタップすると、*情報*ボタンをクリックして、`XamarinMapOverlay`ユーザー コントロール、`Tapped`が実行されるイベントの起動、`OnInfoButtonTapped`メソッド。

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

このメソッドは、web ブラウザーを起動しに格納されているアドレスに移動、`Url`のプロパティ、`CustomPin`インスタンス。 作成するときにアドレスが定義されたことに注意してください、 `CustomPin` PCL プロジェクト内のコレクション。

カスタマイズの詳細については、`MapControl`インスタンスは、「[マップや場所概要](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx)msdn です。

## <a name="summary"></a>まとめ

この記事のカスタム レンダラーを作成する方法を説明する、`Map`コントロール、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドします。 Xamarin.Forms.Maps は、ユーザーを高速で使い慣れたマップを提供する各プラットフォームでの Api が発生するネイティブのマップを使用してマップを表示するため、クロスプラット フォームの抽象化を提供します。


## <a name="related-links"></a>関連リンク

- [マップ コントロール](~/xamarin-forms/user-interface/map.md)
- [iOS のマップ](~/ios/user-interface/controls/ios-maps/index.md)
- [Maps API](~/android/platform/maps-and-location/maps/maps-api.md)
- [カスタマイズされた暗証番号 (pin) (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
