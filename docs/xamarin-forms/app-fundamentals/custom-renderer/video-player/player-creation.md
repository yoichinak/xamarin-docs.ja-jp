---
title: Platform video player の作成
description: この記事では、Xamarin.Forms を使用して、各プラットフォームでビデオ プレーヤーのカスタム レンダラーを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 0090ec798e8d7b1dfb9bd8e25f09d71ec0353b45
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171912"
---
# <a name="creating-the-platform-video-players"></a>Platform video player の作成

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)ソリューションには、Xamarin.Forms 用ビデオ プレーヤーを実装するすべてのコードが含まれています。 アプリケーション内で、ビデオ プレーヤーを使用する方法については、一連のページも含まれています。 すべての`VideoPlayer`コードとそのプラットフォーム レンダラーという名前のプロジェクト フォルダーに存在する`FormsVideoLibrary`、名前空間を使用しても、`FormsVideoLibrary`します。 これは、ため、簡単に独自のアプリケーションに、ファイルをコピーし、クラスを参照するする必要があります。

## <a name="the-video-player"></a>ビデオ プレーヤー

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs)クラスの一部である、 **VideoPlayerDemos**プラットフォーム間で共有されている .NET Standard ライブラリ。 派生した`View`:

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

このクラスのメンバー (および`IVideoPlayerController`インターフェイス) は、次の記事の中で説明します。

という名前のクラスを含む、各プラットフォーム`VideoPlayerRenderer`ビデオ プレーヤーを実装するためにプラットフォーム固有のコードを格納しています。 このレンダラーの主要なタスクでは、そのプラットフォーム用のビデオ プレーヤーを作成します。

### <a name="the-ios-player-view-controller"></a>IOS プレーヤーのビュー コント ローラー

Ios ビデオ プレーヤーを実装するときに、いくつかのクラスが関わります。 アプリケーションを作成、 [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/)し、設定、 [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/)プロパティ型のオブジェクトを[ `AVPlayer`](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/)します。 プレーヤーでビデオのソースが割り当てられている場合、追加のクラスが必要です。

などのすべてのレンダラーでは、iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs)が含まれています、`ExportRenderer`を識別する属性、`VideoPlayer`レンダラーで表示します。

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

派生したプラットフォームの制御を設定するレンダラーの通常、 [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs)クラス、場所`View`、Xamarin.Forms は、`View`派生物 (この場合、 `VideoPlayer`) と`NativeView`iOS は、`UIView`レンダラー クラスの派生です。 このレンダラーでは、そのジェネリック引数が単に設定`UIView`、上の理由からすぐにわかります。

レンダラーのに基づいてときに、`UIViewController`派生物 (この 1 つとしての)、クラスでオーバーライドする必要があります、`ViewController`プロパティとビュー コント ローラーで、この場合の戻り`AVPlayerViewController`します。 目的は、`_playerViewController`フィールド。

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

主な役割、`OnElementChanged`どうかを確認のオーバーライドは、`Control`プロパティは`null`とそうである場合は、プラットフォームのコントロールを作成に渡すと、`SetNativeControl`メソッド。 この場合、そのオブジェクトはから利用可能なのみ、`View`のプロパティ、`AVPlayerViewController`します。 ある`UIView`導関数がという名前のプライベート クラスである`AVPlayerView`がプライベートであるため、そのことはできません明示的に指定する 2 番目の汎用引数として`ViewRenderer`します。

一般に、`Control`レンダラー クラスのプロパティは、その後を参照、 `UIView` 、レンダラーの実装に使用されるが、ここで、`Control`プロパティは他の場所で使用されません。

### <a name="the-android-video-view"></a>Android のビデオの表示

Android のレンダラーの`VideoPlayer`Android に基づいて[ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/)クラス。 ただし場合、`VideoView`領域割り当てられたビデオの塗りつぶしの Xamarin.Forms アプリケーションでビデオを再生する単独で使用、`VideoPlayer`正しい縦横比を維持することがなく。 この (後述します) との理由で、 `VideoView` Android の子になって`RelativeLayout`します。 A`using`ディレクティブ定義`ARelativeLayout`Xamarin.Forms から区別するために`RelativeLayout`、2 番目の汎用引数は、 `ViewRenderer`:

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

Xamarin.Forms 2.5 以降、Android のレンダラーはコンス トラクターを含める必要があります、`Context`引数。

`OnElementChanged`両方の上書きが作成され、`VideoView`と`RelativeLayout`のレイアウトのパラメーターを設定し、`VideoView`内で中央揃えに、`RelativeLayout`します。


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

ハンドラーを`Prepared`イベントがこのメソッドでアタッチおよびデタッチで、`Dispose`メソッド。 このイベントが発生したときに、`VideoView`ビデオ ファイルの再生を開始するための十分な情報があります。

### <a name="the-uwp-media-element"></a>UWP のメディア要素

ユニバーサル Windows プラットフォーム (UWP) で、最も一般的なビデオ プレーヤーは[ `MediaElement`](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)します。 そのドキュメントの`MediaElement`ことを示します、 [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/)のみのバージョンの Windows 10 ビルド 1607 以降をサポートするために必要なときは、代わりに使用する必要があります。

`OnElementChanged`上書きを作成する必要があります、 `MediaElement`、いくつかのイベントのハンドラーを設定して渡す、`MediaElement`オブジェクトを`SetNativeControl`:

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

2 つのイベント ハンドラーがデタッチされて、`Dispose`レンダラー イベント。

## <a name="showing-the-transport-controls"></a>トランスポート コントロールを表示

プラットフォームに含まれるすべてのビデオ プレーヤーは、再生および一時停止、および新しい位置に移動して、ビデオ内の現在位置を示すバー ボタンが含まれているトランスポート コントロールの既定のセットをサポートします。

`VideoPlayer`クラスという名前のプロパティを定義する`AreTransportControlsEnabled`既定値を設定および`true`:


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

このプロパティはどちらも`set`と`get`プロパティが設定されている場合にのみ、ケースを処理するために、アクセサー、レンダラーが。 `get`アクセサーが単純に、プロパティの現在の値を返します。

などのプロパティ`AreTransportControlsEnabled`プラットフォーム レンダラーが 2 つの方法で処理されます。

- 1 回目は Xamarin.Forms を作成するとき、`VideoPlayer`要素。 これで示されます、`OnElementChanged`レンダラーのオーバーライド時に、`NewElement`プロパティは`null`します。 現時点では、レンダラーを設定できるプロパティの初期値から独自のプラットフォームのビデオ プレーヤーで定義されている、 `VideoPlayer`。

- 場合、プロパティで`VideoPlayer`後で変更されると、`OnElementPropertyChanged`レンダラーでメソッドが呼び出されます。 これにより、新しいプロパティの設定に基づいて、プラットフォームのビデオ プレーヤーを更新するレンダラーです。

次のセクションで説明する方法、`AreTransportControlsEnabled`プロパティは、各プラットフォームで処理されます。

### <a name="ios-playback-controls"></a>iOS の再生コントロール

IOS のプロパティは、`AVPlayerViewController`表示を制御するコントロールは、トランスポートの[ `ShowsPlaybackControls`](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/)します。 IOS でそのプロパティを設定する方法を次に示します`VideoViewRenderer`:

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

`Element`レンダラーのプロパティを指す、`VideoPlayer`クラス。

### <a name="the-android-media-controller"></a>Android のメディアのコント ローラー

Android では、トランスポート コントロールを表示する必要がありますを作成する、 [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/)オブジェクトと関連付けること、`VideoView`オブジェクト。 しくみがで示されています、`SetAreTransportControlsEnabled`メソッド。

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

UWP`MediaElement`という名前のプロパティを定義します。 [ `AreTransportControlsEnabled`](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled)からプロパティを設定するように、 `VideoPlayer` 、同じ名前のプロパティ。

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

1 つ以上のプロパティは、ビデオの再生を開始するために必要な: これは、重要な`Source`ビデオ ファイルを参照するプロパティ。 実装する、`Source`プロパティが次の記事で説明されている[Web ビデオの再生](web-videos.md)します。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
