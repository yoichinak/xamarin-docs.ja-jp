---
title: "コンテンツ ページをカスタマイズします。"
description: "コンテンツ ページは、1 つのビューを表示し、画面の大部分を占める visual 要素です。 この記事では、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドするコンテンツ ページのページで、カスタム レンダラーを作成する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d7f7e031d91cd1505ee255bbf0d25198bd9ae82a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-contentpage"></a>コンテンツ ページをカスタマイズします。

_コンテンツ ページは、1 つのビューを表示し、画面の大部分を占める visual 要素です。この記事では、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドするコンテンツ ページのページで、カスタム レンダラーを作成する方法を示します。_

各 Xamarin.Forms コントロールには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) iOS 内の Xamarin.Forms アプリケーションによって表示される、`PageRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`UIViewController`コントロール。 Android のプラットフォームでは、`PageRenderer`クラスをインスタンス化、`ViewGroup`コントロール。 Windows Phone でユニバーサル Windows プラットフォーム (UWP)、`PageRenderer`クラスをインスタンス化、`FrameworkElement`コントロール。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)とそれを実装する、対応するネイティブ コントロール。

![](contentpage-images/contentpage-classes.png "コンテンツ ページ クラスとネイティブの制御を実装する間のリレーションシップ")

表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズ設定を実装する、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Xamarin.Forms_Page)Xamarin.Forms ページ。
1. [消費](#Consuming_the_Xamarin.Forms_Page)Xamarin.Forms からページ。
1. [作成](#Creating_the_Page_Renderer_on_each_Platform)各プラットフォームでのページのカスタム レンダラーです。

各項目を実装する順番に説明するようになりましたが、`CameraPage`フィード ライブ カメラと写真をキャプチャする権限を提供します。

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms ページを作成します。

変更されていない[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)の XAML コードの例を次に示すように、共有の Xamarin.Forms プロジェクトに追加できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

分離コード ファイルを同様に、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)の次のコード例に示すように変更されていないもする必要があります。

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

次のコード例は、c# でのページの作成方法を示しています。

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

インスタンス、`CameraPage`フィードの各プラットフォームで実際のカメラを表示するために使用されます。 コントロールのカスタマイズを実行するカスタムのレンダラーでの他の実装は必要ありませんので、`CameraPage`クラスです。

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Xamarin.Forms ページを使用

空の`CameraPage`Xamarin.Forms アプリケーションで表示される必要があります。 これは、ボタン、`MainPage`インスタンスをタップすると、順番が実行される、`OnTakePhotoButtonClicked`メソッドを次のコード例に示すように。

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

このコードは、単に移動、 `CameraPage`、どのカスタム レンダラーでは各プラットフォームでのページの外観をカスタマイズします。

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>各プラットフォームで、ページ レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`PageRenderer`クラスです。
1. 上書き、`OnElementChanged`ページをカスタマイズするネイティブのページと書き込みのロジックを表示するメソッド。 `OnElementChanged` Xamarin.Forms、対応するコントロールの作成時に、メソッドが呼び出されます。
1. 追加、`ExportRenderer`属性、ページ レンダラーをクラスに Xamarin.Forms ページを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> 省略可能なページ レンダラーな各プラットフォームのプロジェクトでの提供になります。 ページ レンダラーが登録されていない場合は、ページの既定のレンダラーが使用されます。

次の図は、その間の関係と共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](contentpage-images/solution-structure.png "CameraPage カスタム レンダラーのプロジェクトの責任")

`CameraPage`インスタンスがプラットフォーム固有の仕様を表示する`CameraPageRenderer`から派生するクラス、`PageRenderer`そのプラットフォームのクラスです。 これは、結果、各`CameraPage`インスタンス化、カメラのライブ フィードに表示される次のスクリーン ショットに示すようにします。

![](contentpage-images/screenshots.png "各プラットフォームで CameraPage")

`PageRenderer`クラスが公開、 `OnElementChanged` Xamarin.Forms ページが、対応するネイティブ コントロールを表示するために作成されるときに呼び出されるメソッド。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティです。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラーでは、*が*に接続されていると Xamarin.Forms の要素をレンダラーでは、*は*に、それぞれをアタッチします。 サンプル アプリケーションで、`OldElement`プロパティ`null`と`NewElement`プロパティへの参照が格納されます、`CameraPage`インスタンス。

オーバーライドのバージョン、`OnElementChanged`メソッドで、`CameraPageRenderer`クラスは、ネイティブのページのカスタマイズを実行する場所です。 表示される Xamarin.Forms ページ インスタンスへの参照を取得できます、`Element`プロパティです。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、– 表示するには、Xamarin.Forms ページの型名とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、説明の実装、`CameraPageRenderer`各プラットフォーム用のカスタム レンダラーです。

### <a name="creating-the-page-renderer-on-ios"></a>IOS でページ レンダラーを作成します。

次のコード例は、iOS プラットフォーム用のページ レンダラーを示しています。

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドは、iOS をインスタンス化`UIViewController`コントロール。 カメラのライブ ストリームがレンダリングされるは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、なるは、カスタム レンダラーによってレンダリングされるページ インスタンスが存在するだけです。

ページが、一連のメソッドを使用するしてカスタマイズし、`AVCapture`をカメラ写真をキャプチャする権限からがライブ ストリームを提供する Api。

### <a name="creating-the-page-renderer-on-android"></a>Android でページ レンダラーを作成します。

次のコード例は、Android プラットフォーム用のページ レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドは、Android をインスタンス化`ViewGroup`ビューのグループにあるコントロールです。 カメラのライブ ストリームがレンダリングされるは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、なるは、カスタム レンダラーによってレンダリングされるページ インスタンスが存在するだけです。

ページは、一連を使用するメソッドを呼び出すことによってカスタマイズし、`Camera`カメラと前に、写真をキャプチャする権限からライブ ストリームを提供する API、`AddView`ライブのカメラを追加するメソッドが呼び出されるストリームの UI を`ViewGroup`です。

### <a name="creating-the-page-renderer-on-windows-phone"></a>Windows Phone でページ レンダラーを作成します。

次のコード例は、Windows Phone プラットフォーム用のページ レンダラーを示しています。

```csharp
[assembly: ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                ...
                var container = ContainerElement as Canvas;

                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                container.Children.Add (page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドは、Windows Phone をインスタンス化`Canvas`コントロール、ページがレンダリングされます。 カメラのライブ ストリームがレンダリングされるは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、なるは、カスタム レンダラーによってレンダリングされるページ インスタンスが存在するだけです。

Windows Phone のプラットフォームでネイティブに、プラットフォームで使用されているページへの参照を型指定されたをを介してアクセスできる、`ContainerElement`プロパティで、`Canvas`制御への参照を型指定されている、`FrameworkElement`です。 ページは、一連を使用するメソッドを呼び出すことによってカスタマイズし、`MediaCapture`カメラと、カスタマイズされたページに追加する前に、写真をキャプチャする権限からライブ ストリームを提供する API、`Canvas`表示用です。

派生するカスタム レンダラーを実装するときに`PageRenderer`Windows ランタイムで、`ArrangeOverride`基本レンダラーが不明な関連付けを行うためにも、メソッドを実装ページ コントロールを配置する必要があります。 それ以外の場合、空白のページが発生します。 そのため、この例では、`ArrangeOverride`メソッドの呼び出し、`Arrange`メソッドを`Page`インスタンス。

> [!NOTE]
> 停止し、Windows Phone 8.1 WinRT アプリケーションにカメラへのアクセスを提供するオブジェクトの破棄は重要です。 デバイスのカメラにアクセスしようとする他のアプリケーションと干渉するようにエラーがあります。 詳細については、次を参照してください。、`CleanUpCaptureResourcesAsync`サンプル ソリューションで Windows Phone プロジェクト内のメソッドと[クイック スタート: MediaCapture API を使用してビデオを取り込む](https://msdn.microsoft.com/library/windows/apps/xaml/dn642092.aspx)です。

### <a name="creating-the-page-renderer-on-uwp"></a>UWP にページ レンダラーを作成します。

次のコード例は、UWP 用ページ レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupCamera();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

基本クラスの呼び出し`OnElementChanged`メソッドがインスタンス化、`FrameworkElement`ページがレンダリングされたコントロール。 カメラのライブ ストリームがレンダリングされるは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、なるは、カスタム レンダラーによってレンダリングされるページ インスタンスが存在するだけです。 ページは、一連を使用するメソッドを呼び出すことによってカスタマイズし、`MediaCapture`カメラと、カスタマイズされたページに追加する前に、写真をキャプチャする権限からライブ ストリームを提供する API、`Children`表示用のコレクション。

派生するカスタム レンダラーを実装するときに`PageRenderer`UWP、上、`ArrangeOverride`基本レンダラーが不明な関連付けを行うためにも、メソッドを実装ページ コントロールを配置する必要があります。 それ以外の場合、空白のページが発生します。 そのため、この例では、`ArrangeOverride`メソッドの呼び出し、`Arrange`メソッドを`Page`インスタンス。

> [!NOTE]
> 停止し、UWP アプリケーションでカメラへのアクセスを提供するオブジェクトの破棄に重要です。 デバイスのカメラにアクセスしようとする他のアプリケーションと干渉するようにエラーがあります。 詳細については、次を参照してください。[カメラのプレビューを表示](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access)です。

## <a name="summary"></a>まとめ

この記事は、用のカスタム レンダラーを作成する方法を示しましたが、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ページで、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドします。 A`ContentPage`は、1 つのビューを表示し、画面の大部分を占める視覚的要素。


## <a name="related-links"></a>関連リンク

- [CustomRendererContentPage (sample)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
