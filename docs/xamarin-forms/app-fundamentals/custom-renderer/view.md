---
title: ビューを実装します。
description: この記事では、デバイスのカメラからのプレビュー ビデオ ストリームを表示するため、Xamarin.Forms カスタム コントロール用のカスタム レンダラーを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 8ee9926eb3b726673711141e7c75a68b607d02d3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994703"
---
# <a name="implementing-a-view"></a>ビューを実装します。

_Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、レイアウトと画面上のコントロールを配置するために使用するビュー クラスから派生する必要があります。この記事では、デバイスのカメラからのプレビュー ビデオ ストリームを表示するため、Xamarin.Forms カスタム コントロール用のカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms のビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `View` ](xref:Xamarin.Forms.View)で iOS、Xamarin.Forms アプリケーションによって表示される、`ViewRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`UIView`コントロール。 Android のプラットフォームで、`ViewRenderer`クラスのインスタンスを作成、ネイティブ`View`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`ViewRenderer`クラスのインスタンスを作成、ネイティブ`FrameworkElement`コントロール。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `View` ](xref:Xamarin.Forms.View)およびそれを実装するネイティブ コントロールの対応します。

![](view-images/view-classes.png "ビュー クラスとそのネイティブ クラスの実装間のリレーションシップ")

レンダリング プロセスのカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装するために使用できます、 [ `View` ](xref:Xamarin.Forms.View)各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Control)Xamarin.Forms カスタム コントロール。
1. [消費](#Consuming_the_Custom_Control)Xamarin.Forms からカスタム コントロール。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームでコントロールのカスタム レンダラーです。

各項目が実装するためにさらに、説明するようになりましたが、`CameraPreview`レンダラー デバイスのカメラからのプレビュー ビデオ ストリームを表示します。 ビデオ ストリームをタップは停止し、それを開始します。

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>カスタム コントロールを作成します。

サブクラス化して、カスタム コントロールを作成することができます、 [ `View` ](xref:Xamarin.Forms.View)クラスに、次のコード例に示すようにします。

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

`CameraPreview`カスタム コントロールは、ポータブル クラス ライブラリ (PCL) プロジェクトが作成され、コントロールの API を定義します。 カスタム コントロールは、公開、`Camera`前面またはデバイスの背面カメラからビデオ ストリームを表示するかどうかを制御するために使用されるプロパティです。 値が指定されていない場合、`Camera`プロパティ、コントロールが作成されたときに、背面カメラを指定することを既定値します。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`CameraPreview`カスタム コントロールで参照できます XAML PCL プロジェクトでの場所の名前空間の宣言してカスタム コントロール要素で名前空間プレフィックスを使用します。 次のコード例に示す方法、 `CameraPreview` XAML ページで、カスタム コントロールを使用できます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム コントロールを参照します。

次のコード例に示す方法、 `CameraPreview` (C#) ページで、カスタム コントロールを使用できます。

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

インスタンス、`CameraPreview`カスタム コントロールを使用して、デバイスのカメラからのプレビュー ビデオ ストリームを表示します。 必要に応じて値を指定するとは別に、`Camera`プロパティ、コントロールのカスタマイズをカスタム レンダラーで実行されます。

カスタム レンダラーは、プラットフォーム固有のカメラのプレビュー コントロールを作成するには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ViewRenderer<T1,T2>`カスタム コントロールをレンダリングするクラス。 最初の型引数は、この例では、レンダラーでは、カスタム コントロールをする必要があります`CameraPreview`します。 2 番目の型引数がカスタム コントロールを実装するネイティブ コントロールがあります。
1. 上書き、`OnElementChanged`それをカスタマイズするカスタム コントロールと書き込みロジックをレンダリングするメソッド。 対応する Xamarin.Forms コントロールの作成時に、このメソッドが呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラスは、Xamarin.Forms カスタム コントロールを表示するために使用するように指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラー必要は各プラットフォーム プロジェクトに表示するときに、[ビュー](xref:Xamarin.Forms.View)要素。

次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](view-images/solution-structure.png "CameraPreview カスタム レンダラーのプロジェクトの責任")

`CameraPreview`でプラットフォーム固有のレンダラー クラスから派生するカスタム コントロールが表示される、`ViewRenderer`各プラットフォームのクラス。 これは、結果、各`CameraPreview`カスタム コントロールが次のスクリーン ショットに示すようにプラットフォーム固有のコントロールを表示します。

![](view-images/screenshots.png "各プラットフォームで CameraPreview")

`ViewRenderer`クラスでは、`OnElementChanged`メソッドで、Xamarin.Forms カスタム コントロールが、対応するネイティブ コントロールを表示するために作成されたときに呼び出されます。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティ。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラー*が*に接続されていると Xamarin.Forms 要素をレンダラー*は*に、それぞれに接続されています。 サンプル アプリケーションで、`OldElement`プロパティになります`null`と`NewElement`プロパティへの参照には、`CameraPreview`インスタンス。

オーバーライドされたバージョン、`OnElementChanged`各プラットフォームに固有のレンダラー クラスのメソッドはネイティブ コントロールのインスタンス化およびカスタマイズを実行する場所です。 `SetNativeControl`ネイティブ コントロールをインスタンス化するメソッドを使用する必要があり、このメソッドは、コントロールの参照を割り当てることも、`Control`プロパティ。 さらでレンダリングされている Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティ。

いくつかの状況で、`OnElementChanged`メソッドは、複数回呼び出すことができます。 そのため、メモリ リークを防ぐためには、注意が必要、新しいネイティブ コントロールをインスタンス化するとき。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

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

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを設定し、イベント ハンドラーをサブスクライブします。 同様に、イベント ハンドラーがサブスクライブしている必要がありますのみサブスクリプションを解除するのには、レンダラーが関連付けられている要素が変更されたとき。 このアプローチを採用すると、メモリ リークが発生しないパフォーマンスの高い、カスタム レンダラーを作成するのに役立ちます。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms カスタム コントロールの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

次のコード例では、iOS プラットフォーム用のカスタム レンダラーを示します。

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

されるとき、`Control`プロパティは`null`、`SetNativeControl`メソッドを呼び出して、新しいインスタンスを作成する`UICameraPreview`コントロールにへの参照を割り当てると、`Control`プロパティ。 `UICameraPreview`コントロールが使用するプラットフォームに固有のカスタム コントロール、`AVCapture`をカメラからのストリームをプレビューを提供する Api。 これは、公開、`Tapped`によって処理されるイベント、`OnCameraPreviewTapped`メソッドを停止し、それがタップされたときに、ビデオのプレビューを開始します。 `Tapped`イベントをサブスクライブするときに、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされ、レンダラーでは、要素が変更にアタッチされている場合にのみからサブスクライブを解除します。

### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

次のコード例では、Android プラットフォーム用のカスタム レンダラーを示します。

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

されるとき、`Control`プロパティは`null`、`SetNativeControl`メソッドを呼び出して、新しいインスタンスを作成する`CameraPreview`制御し、そのにへの参照を割り当てる、`Control`プロパティ。 `CameraPreview`コントロールが使用するプラットフォームに固有のカスタム コントロール、`Camera`カメラからのストリームをプレビューを提供する API。 `CameraPreview`コントロールが、構成されている、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること。 この構成では、新しいネイティブを作り`Camera`オブジェクトを特定のハードウェア カメラへのアクセスおよび処理するイベント ハンドラーを登録、`Click`イベント。 さらにこのハンドラーは停止し、それがタップされたときに、ビデオのプレビューを開始します。 `Click` Xamarin.Forms 要素は、レンダラーが変更にアタッチされている場合、イベントを購読解除します。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP のカスタム レンダラーを作成します。

次のコード例では、UWP 用のカスタム レンダラーを示します。

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

されるとき、`Control`プロパティが`null`、新しい`CaptureElement`がインスタンス化されると、`SetupCamera`メソッドが呼び出されると、使用、`MediaCapture`カメラからのストリームをプレビューを提供する API。 `SetNativeControl`への参照を割り当てるにはメソッドが呼び出されます、`CaptureElement`インスタンスを`Control`プロパティ。 `CaptureElement`公開を制御する`Tapped`によって処理されるイベント、`OnCameraPreviewTapped`メソッドを停止し、それがタップされたときに、ビデオのプレビューを開始します。 `Tapped`イベントをサブスクライブするときに、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされ、レンダラーでは、要素が変更にアタッチされている場合にのみからサブスクライブを解除します。

> [!NOTE]
> 停止し、UWP アプリケーションでのカメラへのアクセスを提供するオブジェクトの破棄に重要です。 そのためにはエラーは、デバイスのカメラにアクセスしようとする他のアプリケーションに干渉します。 詳細については、次を参照してください。[カメラ プレビュー表示](/windows/uwp/audio-video-camera/simple-camera-preview-access/)します。

## <a name="summary"></a>まとめ

この記事では、デバイスのカメラからのプレビュー ビデオ ストリームを表示するため、Xamarin.Forms カスタム コントロール用のカスタム レンダラーを作成する方法について説明しました。 Xamarin.Forms のカスタム ユーザー インターフェイス コントロールがから派生する必要があります、 [ `View` ](xref:Xamarin.Forms.View)クラスは、レイアウトと画面上のコントロールを配置するために使用します。


## <a name="related-links"></a>関連リンク

- [CustomRendererView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
