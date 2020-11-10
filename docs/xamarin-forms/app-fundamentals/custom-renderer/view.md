---
title:ページの実装の説明: この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法について説明します。
ms.prod: xamarin ms.assetid:915E25E7-4A6B-4F34-B7B4-07D5F4B240F2 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:10/30/2020 no-loc:
- "Xamarin.Forms"
- "Xamarin.Essentials"

---
# <a name="implementing-a-view"></a>ページの実装

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-view)

_Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、View クラスから派生させる必要があります。これは画面上にレイアウトとコントロールを配置するために使われます。この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms ビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 Xamarin.Forms アプリケーションによって [`View`](xref:Xamarin.Forms.View) がレンダリングされると、iOS では `ViewRenderer` クラスがインスタンス化され、それによってネイティブの `UIView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`ViewRenderer` クラスによってネイティブの `View` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`ViewRenderer` クラスによってネイティブの `FrameworkElement` コントロールがインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスおよびネイティブ コントロール) を参照してください。

> [!NOTE]
> Android のコントロールの中には、`ViewRenderer` クラスを使用しない高速レンダラーを使用するものがあります。 高速レンダラーの詳細については、「[Xamarin.Forms 高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)」を参照してください。

次の図は、[`View`](xref:Xamarin.Forms.View) と、それを実装する、対応しているネイティブ コントロールの関係を示しています。

![View クラスとそれを実装するネイティブ クラス間の関係](view-images/view-classes.png)

レンダリング プロセスを使用して各プラットフォーム上の [`View`](xref:Xamarin.Forms.View) にカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装することができます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms カスタム コントロールを[作成](#creating-the-custom-control)します。
1. Xamarin.Forms からカスタム コントロールを[使用](#consuming-the-custom-control)します。
1. 各プラットフォーム上でコントロールのカスタム レンダラーを[作成](#creating-the-custom-renderer-on-each-platform)します。

デバイスのカメラからのプレビュー ビデオ ストリームを表示する `CameraPreview` レンダラーを実装する各項目について順番に説明します。 ビデオ ストリームをタップすると、停止/開始されます。

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

  public CameraOptions Camera
  {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview` カスタム コントロールは、.NET 標準ライブラリ プロジェクトで作成され、このコントロールの API を定義します。 カスタム コントロールは、デバイスの前面または背面のカメラからビデオ ストリームを表示するかどうかを制御するために使用される `Camera` プロパティを公開しています。 コントロールの作成時に `Camera` プロパティの値が指定されていない場合は、既定で背面のカメラが指定されます。

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`CameraPreview` カスタム コントロールは、その場所の名前空間を宣言し、カスタム コントロール要素上で名前空間プレフィックスを使用することで .NET Standard ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページがどのように `CameraPreview` カスタム コントロールを使用できるかを示しています。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <StackLayout>
        <Label Text="Camera Preview:" />
        <local:CameraPreview Camera="Rear"
                             HorizontalOptions="FillAndExpand"
                             VerticalOptions="FillAndExpand" />
    </StackLayout>
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
    Content = new StackLayout
    {
      Children =
      {
        new Label { Text = "Camera Preview:" },
        new CameraPreview
        {
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

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

iOS と UWP でカスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム コントロールをレンダリングする `ViewRenderer<T1,T2>` クラスのサブクラスを作成します。 最初の型引数は、レンダラーが使用するカスタム コントロール (この場合は `CameraPreview`) にする必要があります。 2 つ目の型引数は、カスタム コントロールを実装するネイティブ コントロールにする必要があります。
1. カスタム コントロールをレンダリングする `OnElementChanged` メソッドをオーバーライドして、ロジックを書き込んでカスタマイズします。 対応する Xamarin.Forms コントロールが作成されると、このメソッドが呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加し、それを使用して Xamarin.Forms のカスタム コントロールがレンダリングされることを指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

Android で高速レンダラーとしてカスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム コントロールをレンダリングする Android コントロールのサブクラスを作成します。 さらに、サブクラスで `IVisualElementRenderer` および `IViewRenderer` インターフェイスが実装されるように指定します。
1. 高速レンダラー クラスで `IVisualElementRenderer` および `IViewRenderer` インターフェイスを実装します。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加し、それを使用して Xamarin.Forms のカスタム コントロールがレンダリングされることを指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラス用の既定のレンダラーが使用されます。 ただし、[View](xref:Xamarin.Forms.View) 要素をレンダリングするときは、各プラットフォーム プロジェクトにカスタム レンダラーが必要です。

次の図に、サンプル アプリケーション内の各プロジェクトの役割と、それらの関係を示します。

![CameraPreview カスタム レンダラーのプロジェクトの役割](view-images/solution-structure.png)

`CameraPreview` カスタム コントロールはプラットフォーム固有のレンダラー クラスによってレンダリングされます。それは、iOS と UWP では `ViewRenderer` クラスから、Android では `FrameLayout` クラスから派生します。 この結果、次のスクリーンショットに示すように、プラットフォーム固有のコントロールを使用してそれぞれの `CameraPreview` カスタム コントロールがレンダリングされます。

![各プラットフォーム上の CameraPreview](view-images/screenshots.png)

`ViewRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 このメソッドでは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーがアタッチされて *いた* Xamarin.Forms 要素と、レンダラーが現在アタッチされて *いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `CameraPreview` インスタンスへの参照が含まれます。

各プラットフォーム固有のレンダラー クラス内の `OnElementChanged` メソッドのオーバーライドされたバージョンは、ネイティブ コントロールのインスタンス化とカスタマイズを実行する場所です。 `SetNativeControl` メソッドはネイティブ コントロールのインスタンス化に使用されます。このメソッドはまた、コントロール参照を `Control` プロパティに割り当てます。 さらに、レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

状況によっては、`OnElementChanged` メソッドが複数回呼び出されることがあります。 したがって、メモリ リークを防ぐため、新しいネイティブ コントロールをインスタンス化するときには慎重に行う必要があります。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {    
    if (Control == null)
    {
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

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms カスタム コントロールの型名と、カスタム レンダラーの型名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

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

次のコード例では、Android プラットフォーム用の高速レンダラーを示します。

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : FrameLayout, IVisualElementRenderer, IViewRenderer
    {
        // ...
        CameraPreview element;
        VisualElementTracker visualElementTracker;
        VisualElementRenderer visualElementRenderer;
        FragmentManager fragmentManager;
        CameraFragment cameraFragment;

        FragmentManager FragmentManager => fragmentManager ??= Context.GetFragmentManager();

        public event EventHandler<VisualElementChangedEventArgs> ElementChanged;
        public event EventHandler<PropertyChangedEventArgs> ElementPropertyChanged;

        CameraPreview Element
        {
            get => element;
            set
            {
                if (element == value)
                {
                    return;
                }

                var oldElement = element;
                element = value;
                OnElementChanged(new ElementChangedEventArgs<CameraPreview>(oldElement, element));
            }
        }

        public CameraPreviewRenderer(Context context) : base(context)
        {
            visualElementRenderer = new VisualElementRenderer(this);
        }

        void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            CameraFragment newFragment = null;

            if (e.OldElement != null)
            {
                e.OldElement.PropertyChanged -= OnElementPropertyChanged;
                cameraFragment.Dispose();
            }
            if (e.NewElement != null)
            {
                this.EnsureId();

                e.NewElement.PropertyChanged += OnElementPropertyChanged;

                ElevationHelper.SetElevation(this, e.NewElement);
                newFragment = new CameraFragment { Element = element };
            }

            FragmentManager.BeginTransaction()
                .Replace(Id, cameraFragment = newFragment, "camera")
                .Commit();
            ElementChanged?.Invoke(this, new VisualElementChangedEventArgs(e.OldElement, e.NewElement));
        }

        async void OnElementPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            ElementPropertyChanged?.Invoke(this, e);

            switch (e.PropertyName)
            {
                case "Width":
                    await cameraFragment.RetrieveCameraDevice();
                    break;
            }
        }       
        // ...
    }
}
```

この例では、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、`OnElementChanged` メソッドによって `CameraFragment` オブジェクトが作成されます。 `CameraFragment` 型は、`Camera2` API を使用してカメラからのプレビュー ストリームを提供するカスタム クラスです。 レンダラーがアタッチされている Xamarin.Forms 要素が変更されると、`CameraFragment` オブジェクトは破棄されます。

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

- [CustomRendererView (サンプル)](/samples/xamarin/xamarin-forms-samples/customrenderers-view)
