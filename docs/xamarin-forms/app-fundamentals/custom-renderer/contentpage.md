---
title: ContentPage のカスタマイズ
description: ContentPage は、単一ビューを表示し、画面の大部分を占めるビジュアル要素です。 この記事では、ContentPage ページ用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4d9749c110019f2cf711c1df56196d3296223641
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557973"
---
# <a name="customizing-a-contentpage"></a>ContentPage のカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)

_ContentPage は、単一ビューを表示し、画面の大部分を占めるビジュアル要素です。この記事では、ContentPage ページ用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。_

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 Xamarin.Forms アプリケーションによって [`ContentPage`](xref:Xamarin.Forms.ContentPage) がレンダリングされると、iOS では `PageRenderer` クラスがインスタンス化され、それによってネイティブの `UIViewController` コントロールもインスタンス化されます。 Android プラットフォーム上では、`PageRenderer` クラスによって `ViewGroup` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`PageRenderer` クラスによって `FrameworkElement` コントロールがインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスおよびネイティブ コントロール) を参照してください。

次の図は、[`ContentPage`](xref:Xamarin.Forms.ContentPage) と、それを実装する、対応しているネイティブ コントロールの関係を示しています。

![ContentPage クラスと実装するネイティブ コントロール間の関係](contentpage-images/contentpage-classes.png)

レンダリング プロセスを活用して各プラットフォーム上で [`ContentPage`](xref:Xamarin.Forms.ContentPage) 用のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装できます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms ページを[作成](#creating-the-xamarinforms-page)します。
1. Xamarin.Forms からページを[使用](#consuming-the-xamarinforms-page)します。
1. 各プラットフォーム上でページのカスタム レンダラーを[作成](#creating-the-page-renderer-on-each-platform)します。

ライブ カメラのフィードと写真をキャプチャする機能を提供する `CameraPage` を実装する各項目について順番に説明します。

## <a name="creating-the-no-locxamarinforms-page"></a>Xamarin.Forms ページを作成する

次の XAML コード例に示すように、変更されていない [`ContentPage`](xref:Xamarin.Forms.ContentPage) を共有 Xamarin.Forms プロジェクトに追加できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

同様に、次のコード例に示すように、[`ContentPage`](xref:Xamarin.Forms.ContentPage) の分離コード ファイルも変更が加えられません。

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

次のコード例は、このページを C# で作成する方法を示しています。

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

`CameraPage` のインスタンスは、各プラットフォームでライブ カメラ フィードを表示するために使用されます。 コントロールのカスタマイズはカスタム レンダラーで実行されるため、`CameraPage` クラスに追加の実装は必要ありません。

## <a name="consuming-the-no-locxamarinforms-page"></a>Xamarin.Forms ページを使用する

Xamarin.Forms アプリケーションには、必ず空の `CameraPage` が表示されます。 これは、次のコード例に示すように、`MainPage` インスタンスのボタンがタップされてから `OnTakePhotoButtonClicked` メソッドが実行されたときに起こります。

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

このコードでは単に `CameraPage` にナビゲートされます。このカスタム レンダラーによって、各プラットフォームのページの外観がカスタマイズされます。

## <a name="creating-the-page-renderer-on-each-platform"></a>各プラットフォーム上でページ レンダラーを作成する

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. `PageRenderer` クラスのサブクラスを作成します。
1. ネイティブ ページをレンダリングする `OnElementChanged` メソッドをオーバーライドして、ロジックを書き込み、ページをカスタマイズします。 対応する Xamarin.Forms コントロールが作成されると、`OnElementChanged` メソッドが呼び出されます。
1. `ExportRenderer` 属性をページ レンダラー クラスに追加して、Xamarin.Forms ページのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> プラットフォーム プロジェクトごとにページ レンダラーを指定するかどうかは任意です。 ページ レンダラーが登録されていない場合は、ページの既定のレンダラーが使用されます。

次の図に、サンプル アプリケーション内の各プロジェクトの役割とそれらの関係を示します。

![CameraPage カスタム レンダラーのプロジェクトの役割](contentpage-images/solution-structure.png)

`CameraPage` インスタンスはプラットフォーム固有の `CameraPageRenderer` クラスによってレンダリングされます。このクラスはすべてそのプラットフォームの `PageRenderer` クラスから派生しています。 この結果、次のスクリーンショットに示すように、各 `CameraPage` インスタンスがライブ カメラ フィードでレンダリングされます。

![各プラットフォーム上の CameraPage](contentpage-images/screenshots.png)

`PageRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms ページの作成時に呼び出されます。 このメソッドでは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーがアタッチされて*いた* Xamarin.Forms 要素と、レンダラーが現在アタッチされて*いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `CameraPage` インスタンスへの参照が含まれます。

`CameraPageRenderer` クラスの `OnElementChanged` メソッドのオーバーライドされたバージョンで、ネイティブ ページのカスタマイズが実行されます。 レンダリングされている Xamarin.Forms ページ インスタンスへの参照は、`Element` プロパティを使用して取得することができます。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性では、レンダリングされる Xamarin.Forms ページの型名とカスタム レンダラーの型名という 2 つのパラメーターが使用されます。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォーム用の `CameraPageRenderer` カスタム レンダラーの実装について説明します。

### <a name="creating-the-page-renderer-on-ios"></a>iOS 上でページ レンダラーを作成する

次のコード例は、iOS プラットフォーム用のページ レンダラーを示します。

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

基底クラスの `OnElementChanged` メソッドの呼び出しによって、iOS `UIViewController` コントロールがインスタンス化されます。 レンダラーが既存の Xamarin.Forms 要素にまだアタッチされておらず、カスタム レンダラーによってレンダリングされているページ インスタンスが存在する場合にのみ、ライブ カメラ ストリームがレンダリングされます。

このページは、カメラからライブ ストリームを提供する `AVCapture` API と写真をキャプチャする機能を使用する一連のメソッドによってカスタマイズされます。

### <a name="creating-the-page-renderer-on-android"></a>Android 上でページ レンダラーを作成する

次のコード例は、Android プラットフォーム用のページ レンダラーを示します。

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

基底クラスの `OnElementChanged` メソッドを呼び出すことで、Android の `ViewGroup` コントロールがインスタンス化されます。これは、ビューのグループになります。 レンダラーが既存の Xamarin.Forms 要素にまだアタッチされておらず、カスタム レンダラーによってレンダリングされているページ インスタンスが存在する場合にのみ、ライブ カメラ ストリームがレンダリングされます。

このページは、カメラからライブ ストリームを提供する `Camera` API と写真をキャプチャする機能を使用する一連のメソッドを呼び出すことでカスタマイズされます。それから、ライブ カメラ ストリーム UI を `ViewGroup` に追加する `AddView` メソッドが呼び出されます。 Android では、ビューに対してメジャーおよびレイアウト操作を実行するために `OnLayout` メソッドをオーバーライドする必要もある点に注意してください。 詳細については、[ContentPage レンダラーのサンプル](/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)に関するページを参照してください。

### <a name="creating-the-page-renderer-on-uwp"></a>UWP 上でページ レンダラーを作成する

次のコード例は、UWP 用のページ レンダラーを示します。

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

基底クラスの `OnElementChanged` メソッドを呼び出すと、`FrameworkElement` コントロールがインスタンス化され、ページがレンダリングされます。 レンダラーが既存の Xamarin.Forms 要素にまだアタッチされておらず、カスタム レンダラーによってレンダリングされているページ インスタンスが存在する場合にのみ、ライブ カメラ ストリームがレンダリングされます。 このページは、カメラからライブ ストリームを提供する `MediaCapture` API と写真をキャプチャする機能を使用する一連のメソッドを呼び出すことでカスタマイズされます。それから、カスタマイズされたページが表示のために `Children` コレクションに追加されます。

UWP 上で `PageRenderer` から派生したカスタム レンダラーを実装する場合、ベース レンダラーはその処理方法を認識していないため、ページ コントロールを配置するために `ArrangeOverride` メソッドも実装する必要があります。 それ以外の場合は、結果として空のページになります。 そのため、この例では、`Page` インスタンスに対して `ArrangeOverride` メソッドによって `Arrange` メソッドが呼び出されます。

> [!NOTE]
> UWP アプリケーションでは、カメラにアクセスできるオブジェクトを停止して破棄することが重要です。 そうしないと、デバイスのカメラにアクセスしようとする他のアプリケーションの妨げになる可能性があります。 詳細については、「[Display the camera preview](/windows/uwp/audio-video-camera/simple-camera-preview-access)」(カメラ プレビューの表示) を参照してください。

## <a name="summary"></a>まとめ

この記事では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) ページ用のカスタム レンダラーを作成する方法について説明しました。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。 `ContentPage` は、単一ビューを表示し、画面の大部分を占めるビジュアル要素です。

## <a name="related-links"></a>関連リンク

- [CustomRendererContentPage (サンプル)](/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)