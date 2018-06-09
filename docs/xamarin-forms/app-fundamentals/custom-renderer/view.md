---
title: ビューを実装します。
description: この記事では、デバイスのカメラからプレビュー ビデオ ストリームの表示に使用される Xamarin.Forms のカスタム コントロールのカスタム レンダラーを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: eb0bc199bfd31ac631ca9d131796b606960d76aa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240487"
---
# <a name="implementing-a-view"></a>ビューを実装します。

_Xamarin.Forms カスタム ユーザー インターフェイス コントロールは、レイアウトと、画面上のコントロールを配置に使用されるビュー クラスから派生する必要があります。この記事では、デバイスのカメラからプレビュー ビデオ ストリームの表示に使用される Xamarin.Forms のカスタム コントロールのカスタム レンダラーを作成する方法を示します。_

各 Xamarin.Forms ビューには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ios では、Xamarin.Forms アプリケーションによって表示される、`ViewRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`UIView`コントロール。 Android のプラットフォームでは、`ViewRenderer`クラスのインスタンスを作成、ネイティブな`View`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`ViewRenderer`クラスのインスタンスを作成、ネイティブな`FrameworkElement`コントロール。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)とそれを実装する、対応するネイティブ コントロール。

![](view-images/view-classes.png "ビュー クラスとその実装のネイティブ クラス間のリレーションシップ")

カスタマイズを実装するプラットフォーム固有のカスタム レンダラーを作成することで、表示プロセスを使用できます、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Control)Xamarin.Forms のカスタム コントロールです。
1. [消費](#Consuming_the_Custom_Control)Xamarin.Forms からカスタム コントロールです。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームでコントロールのカスタム レンダラーです。

各項目を実装する順番に説明するようになりましたが、`CameraPreview`レンダラーにデバイスのカメラからプレビュー ビデオ ストリームを表示します。 ビデオ ストリームをタップするは停止し、それを開始します。

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>カスタム コントロールの作成

サブクラス化して、カスタム コントロールを作成することができます、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラスに、次のコード例に示すようにします。

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview`カスタム コントロールは、ポータブル クラス ライブラリ (PCL) プロジェクトに作成され、コントロールの API を定義します。 カスタム コントロールは、公開、`Camera`前面またはデバイスの背面カメラからビデオ ストリームを表示するかどうかを制御するために使用されるプロパティです。 値が指定されていない場合、`Camera`プロパティ、コントロールが作成されたときに、既定値は、背面カメラを指定します。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`CameraPreview`カスタム コントロールで参照できます XAML PCL プロジェクト内の場所の名前空間の宣言してカスタム コントロール要素で名前空間プレフィックスを使用します。 次のコード例に示す方法、`CameraPreview`カスタム コントロールを XAML ページで利用できることができます。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値がカスタム コントロールの詳細情報と一致する必要があります。 名前空間が宣言されると、カスタム コントロールを参照する、プレフィックスを使用します。

次のコード例に示す方法、`CameraPreview`カスタム コントロールは、c# のページで利用できることができます。

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

インスタンス、`CameraPreview`カスタム コントロールは、デバイスのカメラからプレビュー ビデオ ストリームを表示するために使用されます。 必要に応じて値を指定する場合を除いて、`Camera`プロパティ、コントロールのカスタマイズを実行するカスタムのレンダラーでします。

カスタム レンダラーは、プラットフォーム固有のカメラ プレビュー コントロールを作成するには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでは、カスタム レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ViewRenderer<T1,T2>`カスタム コントロールを表示するクラス。 最初の型引数は、この例では、レンダラーでは、カスタム コントロールをする必要があります`CameraPreview`です。 2 番目の型引数は、カスタム コントロールを実装するネイティブ コントロールにする必要があります。
1. 上書き、`OnElementChanged`をカスタマイズするカスタム コントロールと書き込みのロジックを表示するメソッド。 このメソッドは、Xamarin.Forms、対応するコントロールの作成時に呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms のカスタム コントロールを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> Xamarin.Forms のほとんどの要素は各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラーが必要に各プラットフォームのプロジェクトでレンダリングするときに、[ビュー](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)要素。

次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](view-images/solution-structure.png "CameraPreview カスタム レンダラーのプロジェクトの責任")

`CameraPreview`カスタム コントロールがから派生するプラットフォーム固有のレンダラー クラスによって表示される、`ViewRenderer`各プラットフォームのクラスです。 これは、結果、各`CameraPreview`次のスクリーン ショットに示すように、プラットフォーム固有のコントロールに表示されるカスタム コントロール。

![](view-images/screenshots.png "各プラットフォームで CameraPreview")

`ViewRenderer`クラスが公開、 `OnElementChanged` Xamarin.Forms のカスタム コントロールが、対応するネイティブ コントロールを表示するために作成されたときに呼び出されるメソッド。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティです。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラーでは、*が*に接続されていると Xamarin.Forms の要素をレンダラーでは、*は*に、それぞれをアタッチします。 サンプル アプリケーションで、`OldElement`プロパティ`null`と`NewElement`プロパティへの参照が格納されます、`CameraPreview`インスタンス。

オーバーライドのバージョン、`OnElementChanged`メソッドは、各プラットフォームに固有のレンダラー クラスでは、ネイティブ コントロールのインスタンス化とカスタマイズを実行する場所です。 `SetNativeControl`ネイティブのコントロールをインスタンス化するメソッドを使用し、このメソッドはコントロールの参照を代入しても、`Control`プロパティです。 さらに、を通じて表示される Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティです。

いくつかの状況で、`OnElementChanged`メソッドは、複数回呼び出すことができます。 したがって、メモリ リークを防ぐためには、注意が必要、新しいネイティブのコントロールをインスタンス化するとき。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを設定し、イベント ハンドラーをサブスクライブします。 同様に、イベント ハンドラーがサブスクライブされたのみすることはできません、レンダラーにアタッチされている要素が変更されたときからの購読します。 このアプローチを採用することは、メモリ リークを回避することが、パフォーマンスの高いカスタム レンダラーを作成するのに役立ちます。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、–、表示される Xamarin.Forms カスタム コントロールの型名とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

`Control`プロパティは`null`、`SetNativeControl`メソッドは、新しいインスタンスを作成する`UICameraPreview`コントロールにへの参照を割り当てると、`Control`プロパティです。 `UICameraPreview`コントロールは、プラットフォーム固有のカスタム コントロールを使用する、`AVCapture`をカメラからプレビュー ストリームを提供する Api。 公開される、`Tapped`によって処理されるイベント、`OnCameraPreviewTapped`メソッドを停止およびがタップされたときに、ビデオのプレビューを開始します。 `Tapped`イベントがサブスクライブするいるとカスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされているレンダラーでは、要素が変更にアタッチされている場合にのみを購読解除します。

### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

`Control`プロパティは`null`、`SetNativeControl`新しいのインスタンスを作成するメソッドが呼び出された`CameraPreview`を制御し、するために参照を代入、`Control`プロパティです。 `CameraPreview`コントロールは、プラットフォーム固有のカスタム コントロールを使用する、`Camera`カメラからプレビュー ストリームを提供する API。 `CameraPreview`コントロールは、構成、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされています。 この構成では、新しいネイティブを作成する`Camera`、特定のハードウェア カメラにアクセスするオブジェクトを処理するイベント ハンドラーを登録、`Click`イベント。 さらにこのハンドラーは停止しがタップされたときに、ビデオのプレビューを開始します。 `Click` Xamarin.Forms 要素は、レンダラーが変更に関連付けられている場合、イベントを購読解除します。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP にカスタム レンダラーを作成します。

次のコード例は、UWP のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

`Control`プロパティは`null`、新しい`CaptureElement`がインスタンス化されると、`SetupCamera`メソッドは、使用する、`MediaCapture`カメラからプレビュー ストリームを提供する API。 `SetNativeControl`への参照を割り当てるにはメソッドが呼び出されます、`CaptureElement`インスタンスを`Control`プロパティです。 `CaptureElement`公開を制御、`Tapped`によって処理されるイベント、`OnCameraPreviewTapped`メソッドを停止およびがタップされたときに、ビデオのプレビューを開始します。 `Tapped`イベントがサブスクライブするいるとカスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされているレンダラーでは、要素が変更にアタッチされている場合にのみを購読解除します。

> [!NOTE]
> 停止し、UWP アプリケーションでカメラへのアクセスを提供するオブジェクトの破棄に重要です。 デバイスのカメラにアクセスしようとする他のアプリケーションと干渉するようにエラーがあります。 詳細については、次を参照してください。[カメラのプレビューを表示](/windows/uwp/audio-video-camera/simple-camera-preview-access/)です。

## <a name="summary"></a>まとめ

この記事では、デバイスのカメラからプレビュー ビデオ ストリームの表示に使用される Xamarin.Forms のカスタム コントロールのカスタム レンダラーを作成する方法を説明しました。 Xamarin.Forms カスタム ユーザー インターフェイス コントロールがから派生する必要があります、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラスは、レイアウトと、画面上のコントロールを配置するために使用します。


## <a name="related-links"></a>関連リンク

- [CustomRendererView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
