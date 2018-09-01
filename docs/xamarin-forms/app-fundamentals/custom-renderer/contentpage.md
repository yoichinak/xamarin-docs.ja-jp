---
title: コンテンツ ページのカスタマイズ
description: コンテンツ ページは、1 つのビューを表示し、画面の大部分を占める視覚的要素です。 この記事では、開発者は独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドするコンテンツ ページのページのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 8888cb7d75f4469f2a1195b9a426a2e1fbf46bd8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "38995743"
---
# <a name="customizing-a-contentpage"></a>コンテンツ ページのカスタマイズ

_コンテンツ ページは、1 つのビューを表示し、画面の大部分を占める視覚的要素です。この記事では、開発者は独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドするコンテンツ ページのページのカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) iOS での Xamarin.Forms アプリケーションによって表示される、`PageRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`UIViewController`コントロール。 Android のプラットフォームで、`PageRenderer`クラスをインスタンス化、`ViewGroup`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`PageRenderer`クラスをインスタンス化、`FrameworkElement`コントロール。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)およびそれを実装するネイティブ コントロールの対応します。

![](contentpage-images/contentpage-classes.png "コンテンツ ページのクラスとネイティブ コントロールを実装する間のリレーションシップ")

レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装する[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Xamarin.Forms_Page)Xamarin.Forms ページ。
1. [消費](#Consuming_the_Xamarin.Forms_Page)Xamarin.Forms のページ。
1. [作成](#Creating_the_Page_Renderer_on_each_Platform)各プラットフォームで、ページのカスタム レンダラーです。

各項目が実装するためにさらに、説明するようになりましたが、`CameraPage`ライブ カメラのフィード、および写真をキャプチャする機能を提供します。

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms のページを作成します。

変更されていない[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)の XAML コードの例を次に示すように、共有の Xamarin.Forms プロジェクトに追加できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

同様に、分離コード ファイルの[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)の次のコード例に示すように変更されていないもする必要があります。

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

次のコード例では、c# では、ページを作成する方法を示します。

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

インスタンス、`CameraPage`ライブ カメラの各プラットフォームでフィードを表示するために使用されます。 コントロールのカスタマイズを実行するカスタムのレンダラーでの他の実装は必要ありませんので、`CameraPage`クラス。

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>使用している Xamarin.Forms のページ

空の`CameraPage`Xamarin.Forms アプリケーションで表示する必要があります。 これは、上のボタンの場合に発生、`MainPage`インスタンスをタップすると、順番に実行する、`OnTakePhotoButtonClicked`メソッドを次のコード例に示すようにします。

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

このコードに移動しただけで、 `CameraPage`、どのカスタム レンダラーでは各プラットフォームで、ページの外観をカスタマイズします。

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>各プラットフォームで、ページ レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`PageRenderer`クラス。
1. 上書き、`OnElementChanged`ページをカスタマイズするネイティブのページと書き込みのロジックをレンダリングするメソッド。 `OnElementChanged`メソッドは、対応する Xamarin.Forms コントロールが作成されたときに呼び出されます。
1. 追加、`ExportRenderer`ページ レンダラー クラスの Xamarin.Forms ページを表示するために使用するように指定する属性します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> 各プラットフォーム プロジェクトにページ レンダラーを提供する省略可能になります。 ページ レンダラーが登録されていない場合は、ページの既定のレンダラーが使用されます。

次の図は、それらの間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示します。

![](contentpage-images/solution-structure.png "CameraPage カスタム レンダラーのプロジェクトの責任")

`CameraPage`インスタンスは、プラットフォーム固有によってレンダリングされる`CameraPageRenderer`から派生するクラス、`PageRenderer`そのプラットフォーム用のクラス。 これは、結果、各`CameraPage`次のスクリーン ショットに示すように、カメラのライブ フィードを表示するインスタンスします。

![](contentpage-images/screenshots.png "各プラットフォームで CameraPage")

`PageRenderer`クラスでは、`OnElementChanged`メソッドで、対応するネイティブ コントロールを表示するために、Xamarin.Forms のページが作成されたときに呼び出されます。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティ。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラー*が*に接続されていると Xamarin.Forms 要素をレンダラー*は*に、それぞれに接続されています。 サンプル アプリケーションでは、`OldElement`プロパティになります`null`と`NewElement`プロパティへの参照には、`CameraPage`インスタンス。

オーバーライドされたバージョン、`OnElementChanged`メソッドで、`CameraPageRenderer`クラスは、ネイティブのページのカスタマイズを実行する場所。 レンダリングされている Xamarin.Forms のページ インスタンスへの参照を取得できます、`Element`プロパティ。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms のページの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、説明の実装、`CameraPageRenderer`各プラットフォーム用のカスタム レンダラーです。

### <a name="creating-the-page-renderer-on-ios"></a>IOS でのページ レンダラーの作成

次のコード例では、iOS プラットフォーム用のページ レンダラーを示します。

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
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドは、iOS をインスタンス化`UIViewController`コントロール。 カメラのライブ ストリームは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、カスタム レンダラーによってレンダリングされるページのインスタンスが存在することにのみ表示されます。

ページは、一連のメソッドを使用してカスタマイズし、`AVCapture`をカメラ写真をキャプチャする機能から、ライブ ストリームを提供する Api。

### <a name="creating-the-page-renderer-on-android"></a>Android でのページ レンダラーの作成

次のコード例では、Android プラットフォーム用のページ レンダラーを示します。

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
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドには、Android がインスタンス化`ViewGroup`コントロールで、ビューのグループです。 カメラのライブ ストリームは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、カスタム レンダラーによってレンダリングされるページのインスタンスが存在することにのみ表示されます。

ページは、一連を使用するメソッドを呼び出すことによってカスタマイズし、`Camera`カメラと、前に、写真をキャプチャする機能からライブ ストリームを提供する API、`AddView`ライブのカメラを追加するメソッドが呼び出されるストリーミングするための UI、`ViewGroup`します。 Android でですもオーバーライドするために必要な`OnLayout`ビューでメジャーとレイアウトの操作を実行するメソッド。 詳細については、次を参照してください。、 [ContentPage レンダラー サンプル](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)します。

### <a name="creating-the-page-renderer-on-uwp"></a>UWP のページ レンダラーを作成します。

次のコード例では、UWP 用ページ レンダラーを示します。

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
                SetupBasedOnStateAsync();

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

基本クラスの呼び出し`OnElementChanged`メソッドをインスタンス化、`FrameworkElement`コントロール、ページがレンダリングされます。 カメラのライブ ストリームは、レンダラーは、既存の Xamarin.Forms 要素に既に接続されていないし、カスタム レンダラーによってレンダリングされるページのインスタンスが存在することにのみ表示されます。 ページは、一連を使用するメソッドを呼び出すことによってカスタマイズし、`MediaCapture`カメラと、カスタマイズされたページに追加する前に、写真をキャプチャする機能からライブ ストリームを提供する API、`Children`表示用のコレクション。

派生したカスタム レンダラーを実装するときに`PageRenderer`、UWP の`ArrangeOverride`ベース レンダラーは、それらの処理方法を認識しないためも、メソッドを実装、ページ コントロールを配置する必要があります。 それ以外の場合、空白のページになります。 そのため、この例では、`ArrangeOverride`メソッドの呼び出し、`Arrange`メソッドを`Page`インスタンス。

> [!NOTE]
> 停止し、UWP アプリケーションでのカメラへのアクセスを提供するオブジェクトの破棄に重要です。 そのためにはエラーは、デバイスのカメラにアクセスしようとする他のアプリケーションに干渉します。 詳細については、次を参照してください。[カメラ プレビュー表示](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access)します。

## <a name="summary"></a>まとめ

この記事は、用のカスタム レンダラーを作成する方法を示しましたが、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) ページで、開発者は、独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングをオーバーライドします。 A`ContentPage`は 1 つのビューを表示し、画面の大部分を占める視覚的要素。


## <a name="related-links"></a>関連リンク

- [CustomRendererContentPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
