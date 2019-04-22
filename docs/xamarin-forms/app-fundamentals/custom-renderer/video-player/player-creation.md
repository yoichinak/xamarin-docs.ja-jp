---
title: プラットフォーム ビデオ プレーヤーの作成
description: この記事では、Xamarin.Forms を使用して、各プラットフォームでビデオ プレーヤーのカスタム レンダラーを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 4bfbd065c9b17ce402c5a15289c7ff608eb58b23
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870015"
---
# <a name="creating-the-platform-video-players"></a>プラットフォーム ビデオ プレーヤーの作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)

[**VideoPlayerDemos**](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) ソリューションには、Xamarin.Forms 用のビデオ プレーヤーを実装するためのコードが含まれます。 また、アプリケーション内でビデオ プレーヤーを使用する方法を示す一連のページも含まれます。 `VideoPlayer` コードとそのプラットフォーム レンダラーはすべて、`FormsVideoLibrary` という名前のプロジェクト フォルダー内にあり、名前空間 `FormsVideoLibrary` も使用します。 このため、自身のアプリケーションへのファイルのコピーも、クラスの参照も簡単に行うことができます。

## <a name="the-video-player"></a>ビデオ プレーヤー

[`VideoPlayer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) クラスは、プラットフォーム間で共有される **VideoPlayerDemos** .NET Standard ライブラリに含まれます。 これは、`View` から派生します。

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

このクラスのメンバー (および `IVideoPlayerController` インターフェイス) については、この後の記事で説明します。

各プラットフォームには、`VideoPlayerRenderer` という名前のクラスが含まれており、このクラスには、ビデオ プレーヤーを実装するためのプラットフォーム固有のコードが含まれています。 このレンダラーの主要なタスクは、そのプラットフォーム用のビデオ プレーヤーを作成することです。

### <a name="the-ios-player-view-controller"></a>iOS プレーヤーのビュー コント ローラー

iOS でビデオ プレーヤーを実装する場合、関係のあるクラスがいくつかあります。 まず、アプリケーションは [`AVPlayerViewController`](xref:AVKit.AVPlayerViewController) を作成し、[`AVPlayer`](xref:AVFoundation.AVPlayer) 型のオブジェクトに [`Player`](xref:AVKit.AVPlayerViewController.Player*) プロパティを設定します。 プレーヤーにビデオ ソースを割り当てる場合は、追加のクラスが必要です。

すべてのレンダラーと同様、iOS [`VideoPlayerRenderer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs)には、レンダラーで `VideoPlayer` ビューを識別する `ExportRenderer` 属性が含まれます。

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

通常、`View` が Xamarin.Forms `View` の派生物 (この場合は、`VideoPlayer`) で、`NativeView` がレンダラー クラス用の iOS `UIView` の派生物である場合、プラットフォーム コントロールを設定するレンダラーは、[`ViewRenderer<View, NativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) クラスから派生します。 わかりやすくするために、このレンダラーでは、ジェネリック引数を `UIView` に設定しているだけです。

レンダラーが (このレンダラーのように) `UIViewController` の派生物に基づく場合、クラスは、`ViewController` プロパティをオーバーライドして、ビュー コントローラー (この場合は、`AVPlayerViewController`) を返す必要があります。 それが、`_playerViewController` フィールドの目的です。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

`OnElementChanged` オーバーライドの主な役割は、`Control` プロパティが `null` であるかどうかをチェックし、そうである場合はプラットフォーム コントロールを作成して、それを `SetNativeControl` メソッドに渡すことです。 この場合、`AVPlayerViewController` の `View` から使用できるのは、そのオブジェクトのみになります。 その `UIView`の派生物が `AVPlayerView` という名前のプライベート クラスである場合、プライベートであるため、それを `ViewRenderer` の 2 番目のジェネリック引数として明示的に指定することはできません。

通常、レンダラー クラスの `Control` プロパティは、その後、レンダラーの実装に使用される `UIView` を参照しますが、この場合、`Control` プロパティは他の場所で使用されません。

### <a name="the-android-video-view"></a>Android のビデオ ビュー

`VideoPlayer` 用の Android レンダラーは、Android [`VideoView`](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) クラスに基づきます。 しかし、`VideoView` を単独で使用して、Xamarin.Forms アプリケーションでビデオを再生すると、ビデオは `VideoPlayer` 用に割り当てられた領域を塗りつぶし、正確な縦横比は保持されません。 このため、(後述するように) `VideoView` を Android `RelativeLayout` の子にします。 `using` ディレクティブは、それを Xamarin.Forms `RelativeLayout` と区別する `ARelativeLayout` を定義し、それが `ViewRenderer` の 2 番目のジェネリック引数になります。

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Xamarin.Forms 2.5 以降、Android レンダラーには、`Context` 引数を持つコンストラクターが含まれるようになりました。

`OnElementChanged` オーバーライドは、`VideoView` と `RelativeLayout` の両方を作成し、`VideoView` を `RelativeLayout` の中央に配置するようにレイアウト パラメーターを設定します。


```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

`Prepared` イベントのハンドラーは、このメソッドでアタッチされ、`Dispose` メソッドでデタッチされます。 このイベントは、`VideoView` に、ビデオ ファイルの再生を開始するための情報が十分にある場合に起動されます。

### <a name="the-uwp-media-element"></a>UWP のメディア要素

ユニバーサル Windows プラットフォーム (UWP) で最も一般的なビデオ プレーヤーは、[`MediaElement`](xref:Windows.UI.Xaml.Controls.MediaElement) です。 `MediaElement` のドキュメントでは、ビルド 1607 以降のバージョンの Windows 10 をサポートするために必要な場合のみ、[`MediaPlayerElement`](xref:Windows.UI.Xaml.Controls.MediaPlayerElement) を代わりに使用する必要があると説明されています。

`OnElementChanged` オーバーライドは、`MediaElement` を作成し、いくつかのイベント ハンドラーを設定し、`MediaElement` オブジェクトを `SetNativeControl` に設定する必要があります。

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

2 つのイベント ハンドラーは、レンダラー用の `Dispose` でデタッチされます。

## <a name="showing-the-transport-controls"></a>トランスポート コントロールの表示

プラットフォームに含まれるすべてのビデオ プレーヤーは、既定のトランスポート コントロール セットをサポートします。このコントロール セットには、ビデオの再生ボタンと一時停止ボタン、ビデオ内の現在の位置を示し、新しい位置に移動できるバーが含まれます。

`VideoPlayer` クラスは、`AreTransportControlsEnabled` という名前のプロパティを定義し、既定値を `true` に設定します。


```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

このプロパティには、`set` と `get` の両方のアクセサーがありますが、レンダラーは、このプロパティが設定されている場合のみケースを処理する必要があります。 `get` アクセサーは、プロパティの現在値を返します。

`AreTransportControlsEnabled` などのプロパティは、プラットフォーム レンダラー内で次の 2 つの方法により処理されます。

- 最初は、Xamarin.Forms で `VideoPlayer` 要素が作成されるときです。 これは、`NewElement` プロパティが `null` ではない場合にレンダラーの `OnElementChanged` オーバーライドで示されます。 この時点で、レンダラーは、`VideoPlayer` で定義されたプロパティの初期値から自身のプラットフォーム ビデオ プレーヤーを設定することができます。

- この後、`VideoPlayer` 内のプロパティが変更されると、レンダラーの `OnElementPropertyChanged` メソッドが呼び出されます。 これにより、レンダラーは、新しいプロパティ設定に基づいてプラットフォーム ビデオ プレーヤーを更新することができます。

以降のセクションでは、各プラットフォームでの `AreTransportControlsEnabled` プロパティの処理方法について説明します。

### <a name="ios-playback-controls"></a>iOS の再生コントロール

トランスポート コントロールの表示を制御する iOS `AVPlayerViewController` のプロパティは、[`ShowsPlaybackControls`](xref:AVKit.AVPlayerViewController.ShowsPlaybackControls*) です。 そのプロパティを iOS `VideoViewRenderer` で設定する方法を次に示します。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

レンダラーの `Element` プロパティは、`VideoPlayer` クラスを参照します。

### <a name="the-android-media-controller"></a>Android のメディア コントローラー

Android でトランスポート コントロールを表示するには、[`MediaController`](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) オブジェクトを作成し、それを `VideoView` オブジェクトと関連付ける必要があります。 しくみは、`SetAreTransportControlsEnabled` メソッドで示されています。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>UWP のトランスポート コントロールのプロパティ

UWP `MediaElement` は [`AreTransportControlsEnabled`](xref:Windows.UI.Xaml.Controls.MediaElement.AreTransportControlsEnabled*) という名前のプロパティを定義するため、そのプロパティは、同じ名前の `VideoPlayer` プロパティから設定されます。

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

ビデオの再生を開始するには、1 つ以上のプロパティが必要です。これは、ビデオ ファイルを参照する重要な `Source` プロパティです。 次の記事「[Playing a Web Video](web-videos.md)」(Web ビデオの再生) では、`Source` プロパティの実装について説明します。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
