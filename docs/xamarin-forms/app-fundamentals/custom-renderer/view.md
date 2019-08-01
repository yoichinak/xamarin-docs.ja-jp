---
title: ページの実装
description: この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: e8070894bab89ab2e38772518c94482409e4d17f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650426"
---
# <a name="implementing-a-view"></a>ページの実装

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-view)

_Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、View クラスから派生させる必要があります。これは画面上にレイアウトとコントロールを配置するために使われます。この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms ビューに、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 iOS で Xamarin.Forms アプリケーションによって [`View`](xref:Xamarin.Forms.View) がレンダリングされると、`ViewRenderer` クラスがインスタンス化され、次に、ネイティブの `UIView` コントロールがインスタンス化されます。 Android プラットフォーム上では、`ViewRenderer` クラスによってネイティブの `View` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`ViewRenderer` クラスによってネイティブの `FrameworkElement` コントロールがインスタンス化されます。 Xamarin.Forms コントロールがマップするレンダラーとネイティブ コントロール クラスの詳細については、「[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」を参照してください。

次の図は、[`View`](xref:Xamarin.Forms.View) と、それを実装する、対応しているネイティブ コントロールの関係を示しています。

![](view-images/view-classes.png "View クラスとそれを実装するネイティブ クラス間の関係")

レンダリング プロセスを使用して各プラットフォーム上の [`View`](xref:Xamarin.Forms.View) にカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装することができます。 これを行うプロセスは次のとおりです。

1. Xamarin.Forms カスタム コントロールを[作成](#Creating_the_Custom_Control)します。
1. Xamarin.Forms からカスタム コントロールを[使用](#Consuming_the_Custom_Control)します。
1. 各プラットフォーム上でコントロールのカスタム レンダラーを[作成](#Creating_the_Custom_Renderer_on_each_Platform)します。

デバイスのカメラからのプレビュー ビデオ ストリームを表示する `CameraPreview` レンダラーを実装する各項目について順番に説明します。 ビデオ ストリームをタップすると、停止/開始されます。

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>カスタム コントロールの作成

カスタム コントロールは、次のコード例のように、[`View`](xref:Xamarin.Forms.View) クラスをサブクラス化することで作成できます。

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

`CameraPreview` カスタム コントロールは、.NET 標準ライブラリ プロジェクトで作成され、このコントロールの API を定義します。 カスタム コントロールは、デバイスの前面または背面のカメラからビデオ ストリームを表示するかどうかを制御するために使用される `Camera` プロパティを公開しています。 コントロールの作成時に `Camera` プロパティの値が指定されていない場合は、既定で背面のカメラが指定されます。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`CameraPreview` カスタム コントロールは、その場所の名前空間を宣言し、カスタム コントロール要素上で名前空間プレフィックスを使用することで .NET Standard ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページがどのように `CameraPreview` カスタム コントロールを使用できるかを示しています。

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

`local` 名前空間プレフィックスには任意の名前を付けることができます。 ただし、`clr-namespace` と `assembly` の値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム コントロールが参照されます。

次のコード例は、C# ページがどのように `CameraPreview` カスタム コントロールを使用できるかを示しています。

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

`CameraPreview` カスタム コントロールのインスタンスは、デバイスのカメラからプレビュー ビデオ ストリームを表示するために使用されます。 必要に応じて `Camera` プロパティの値を指定する以外に、コントロールのカスタマイズはカスタム レンダラーで実行されます。

これで、カスタム レンダラーを各アプリケーション プロジェクトに追加して、プラットフォーム固有のカメラ プレビュー コントロールを作成できるようになりました。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム コントロールをレンダリングする `ViewRenderer<T1,T2>` クラスのサブクラスを作成します。 最初の型引数は、レンダラーが使用するカスタム コントロール (この場合は `CameraPreview`) にする必要があります。 2 つ目の型引数は、カスタム コントロールを実装するネイティブ コントロールにする必要があります。
1. カスタム コントロールをレンダリングする `OnElementChanged` メソッドをオーバーライドして、ロジックを書き込んでカスタマイズします。 対応する Xamarin.Forms コントロールが作成されると、このメソッドが呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加して、Xamarin.Forms カスタム コントロールのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用します。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、[View](xref:Xamarin.Forms.View) 要素をレンダリングするときは、各プラットフォーム プロジェクトにカスタム レンダラーが必要です。

次の図に、サンプル アプリケーション内の各プロジェクトの役割とそれらの関係を示します。

![](view-images/solution-structure.png "CameraPreview カスタム レンダラーのプロジェクトの役割")

`CameraPreview` カスタム コントロールはプラットフォーム固有のレンダラー クラスによってレンダリングされます。このクラスはすべて各プラットフォームの `ViewRenderer` クラスから派生しています。 この結果、次のスクリーンショットに示すように、プラットフォーム固有のコントロールを使用してそれぞれの `CameraPreview` カスタム コントロールがレンダリングされます。

![](view-images/screenshots.png "各プラットフォーム上の CameraPreview")

`ViewRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 このメソッドは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを取得します。 これらのプロパティは、レンダラーが接続して*いた* Xamarin.Forms 要素と、レンダラーが現在接続して*いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `CameraPreview` インスタンスへの参照が含まれます。

各プラットフォーム固有のレンダラー クラス内の `OnElementChanged` メソッドのオーバーライドされたバージョンは、ネイティブ コントロールのインスタンス化とカスタマイズを実行する場所です。 `SetNativeControl` メソッドはネイティブ コントロールのインスタンス化に使用されます。このメソッドはまた、コントロール参照を `Control` プロパティに割り当てます。 さらに、レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

状況によっては、`OnElementChanged` メソッドが複数回呼び出されることがあります。 したがって、メモリ リークを防ぐため、新しいネイティブ コントロールをインスタンス化するときには慎重に行う必要があります。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    if (Control == null) {
      // Instantiate the native control and assign it to the Control property with
      // the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 さらに、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを作成および構成し、イベント ハンドラーを登録する必要があります。 同様に、レンダラーが関連付けられている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーを登録解除します。 この手法を採用すると、メモリ リークが発生しない効率的なカスタム レンダラーを作成できます。

> [!IMPORTANT]
> `SetNativeControl` メソッドは、`e.NewElement` が `null` ではない場合にのみ、呼び出す必要があります。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms カスタム コントロールの種類名と、カスタム レンダラーの種類名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

次のセクションで、各プラットフォーム固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示します。

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

            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                if (Control == null) {
                  uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                  SetNativeControl (uiCameraPreview);
                }
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

`Control` プロパティが `null` の場合、`SetNativeControl` メソッドは、新しいネイティブの `UICameraPreview` コントロールをインスタンス化し、それに対する参照を `Control` プロパティに割り当てるために呼び出されます。 `UICameraPreview` コントロールは、`AVCapture` API を使用してカメラからプレビュー ストリームを提供するプラットフォーム固有のカスタム コントロールです。 タップ時にビデオ プレビューを停止/開始するために、`OnCameraPreviewTapped` メソッドで処理される `Tapped` イベントが公開されています。 `Tapped` イベントは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされているときにサブスクライブされ、レンダラーがアタッチされている要素が変わったときにのみサブスクライブが解除されます。

### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示します。

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

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                if (Control == null)
                {
                  cameraPreview = new CameraPreview(Context);
                  SetNativeControl(cameraPreview);
                }
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

`Control` プロパティが `null` の場合、`SetNativeControl` メソッドは、新しいネイティブの `CameraPreview` コントロールをインスタンス化し、それに対する参照を `Control` プロパティに割り当てるために呼び出されます。 `CameraPreview` コントロールは、`Camera` API を使用してカメラからプレビュー ストリームを提供するプラットフォーム固有のカスタム コントロールです。 カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、`CameraPreview` コントロールが構成されます。 この構成には、特定のハードウェア カメラにアクセスするために新しいネイティブの `Camera` オブジェクトを作成する処理と、`Click` イベントを処理するためにイベント ハンドラーを登録する処理が含まれています。 さらに、このハンドラーによって、タップ時にビデオ プレビューが停止/開始されます。 レンダラーがアタッチされている Xamarin.Forms 要素が変更された場合に、`Click` イベントのサブスクライブが解除されます。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP 上でのカスタム レンダラーの作成

次のコード例で、UWP 用のカスタム レンダラーを示します。

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

            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                if (Control == null)
                {
                  ...
                  _captureElement = new CaptureElement();
                  _captureElement.Stretch = Stretch.UniformToFill;

                  SetupCamera();
                  SetNativeControl(_captureElement);
                }
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

`Control` プロパティが `null` の場合、新しい `CaptureElement` がインスタンス化され、`SetupCamera` メソッドが呼び出されます。ここで、カメラからプレビュー ストリームを提供するために `MediaCapture` API が使用されます。 `CaptureElement` の参照を `Control` プロパティに割り当てるために `SetNativeControl` メソッドが呼び出されます。 `CaptureElement` コントロールでは、タップ時にビデオ プレビューを停止/開始するために、`OnCameraPreviewTapped` メソッドで処理される `Tapped` イベントが公開されています。 `Tapped` イベントは、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされているときにサブスクライブされ、レンダラーがアタッチされている要素が変わったときにのみサブスクライブが解除されます。

> [!NOTE]
> UWP アプリケーションでは、カメラにアクセスできるオブジェクトを停止して破棄することが重要です。 そうしないと、デバイスのカメラにアクセスしようとする他のアプリケーションの妨げになる可能性があります。 詳細については、「[Display the camera preview](/windows/uwp/audio-video-camera/simple-camera-preview-access/)」(カメラ プレビューの表示) を参照してください。

## <a name="summary"></a>まとめ

この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法について説明しました。 Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、[`View`](xref:Xamarin.Forms.View) クラスから派生させる必要があります。これは画面上にレイアウトとコントロールを配置するために使われます。


## <a name="related-links"></a>関連リンク

- [CustomRendererView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-view)
