---
title: プラットフォームのビデオ プレーヤーを作成します。
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 9efdc8376d9970cb429654e3d3aa2eef75ac2996
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="creating-the-platform-video-players"></a>プラットフォームのビデオ プレーヤーを作成します。

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)ソリューションには、Xamarin.Forms のビデオ プレーヤーを実装するすべてのコードが含まれています。 アプリケーション内で、ビデオ プレーヤーを使用する方法を示す一連のページも含まれています。 すべての`VideoPlayer`コードとそのプラットフォーム レンダラーという名前のプロジェクト フォルダーに存在する`FormsVideoLibrary`、名前空間を使用しても、`FormsVideoLibrary`です。 これは、ため、簡単に独自のアプリケーションにファイルをコピーして、クラスを参照する必要があります。

## <a name="the-video-player"></a>ビデオ プレーヤー

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs)クラスの一部である、 **VideoPlayerDemos**プラットフォーム間で共有されている標準の .NET ライブラリです。 派生して`View`:

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

このクラスのメンバー (および`IVideoPlayerController`インターフェイス) については次の記事で説明します。

という名前のクラスを含む 3 つのプラットフォームの各`VideoPlayerRenderer`ビデオ プレーヤーを実装するプラットフォーム固有のコードを格納しています。 このレンダラーの主要なタスクでは、そのプラットフォームのビデオ プレーヤーを作成します。

### <a name="the-ios-player-view-controller"></a>IOS player ビュー コント ローラー

いくつかのクラスは、iOS でビデオ プレーヤーを実装する際に関係しています。 アプリケーションが最初に作成、 [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/)し、設定、 [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/)プロパティ型のオブジェクトを[ `AVPlayer`](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/)です。 Player でビデオ ソースが割り当てられている場合、その他のクラスが必要です。

などのすべてのレンダラーで、iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs)が含まれています、`ExportRenderer`属性を識別する、`VideoPlayer`レンダラーで表示します。

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

派生したプラットフォーム コントロールを設定するレンダラーの一般に、 [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs)クラス、場所`View`は、Xamarin.Forms`View`から派生した (この場合、 `VideoPlayer`) および`NativeView`iOS は、`UIView`レンダラー クラスを派生します。 このレンダラーでは、そのジェネリック引数は単に設定`UIView`、後ほど上の理由からです。

レンダラーに基づいている場合、`UIViewController`から派生した (ものが)、クラスをオーバーライドし、`ViewController`プロパティと戻りここでは、ビュー、コント ローラー`AVPlayerViewController`です。 目的は、`_playerViewController`フィールド。

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

主な役割、`OnElementChanged`オーバーライドがかどうか確認するには、`Control`プロパティは`null`、プラットフォームのコントロールの作成し、それに渡すこと、`SetNativeControl`メソッドです。 この場合、そのオブジェクトはから利用可能な`View`のプロパティ、`AVPlayerViewController`です。 ある`UIView`という名前のプライベート クラスから派生したしよう`AVPlayerView`がプライベートであるため、できません明示的に指定する 2 番目の汎用引数として`ViewRenderer`です。

一般に、`Control`レンダラー クラスのプロパティがその後に参照、 `UIView` 、レンダラーを実装するために使用が、ここでは、`Control`プロパティは他の場所で使用されません。

### <a name="the-android-video-view"></a>Android のビデオの表示

Android のレンダラーでは、 `VideoPlayer` Android に基づいて[ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/)クラスです。 ただし場合、`VideoView`の領域割り当てられた Xamarin.Forms のアプリケーションでは、ビデオの塗りつぶしでビデオを再生するを単独で使用される、`VideoPlayer`正しい縦横比を維持することがなくです。 この理由で (ように間もなく表示されます)、 `VideoView` 、Android の子が行われる`RelativeLayout`です。 A`using`ディレクティブ定義`ARelativeLayout`、Xamarin.Forms を区別するために`RelativeLayout`、2 番目の汎用引数は、 `ViewRenderer`:

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

Xamarin.Forms 2.5 以降、Android レンダラーを持つコンス トラクターを含める必要があります、`Context`引数。

`OnElementChanged`オーバーライドでは、両方を作成、`VideoView`と`RelativeLayout`レイアウトのパラメーターを設定し、`VideoView`内で中央揃えに、`RelativeLayout`です。


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

ハンドラーを`Prepared`イベントがこのメソッドでアタッチされでデタッチ、`Dispose`メソッドです。 このイベントが発生したときに、`VideoView`ビデオ ファイルの再生を開始するための十分な情報が含まれています。

### <a name="the-uwp-media-element"></a>UWP メディア要素

ユニバーサル Windows プラットフォーム (UWP) で、最も一般的なビデオ プレーヤーは[ `MediaElement`](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)です。 そのドキュメントの`MediaElement`ことを示します、 [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/)のみビルド 1607 で Windows 10 以降のバージョンをサポートするために必要な場合は、代わりに使用する必要があります。

`OnElementChanged`上書きを作成する必要があります、`MediaElement`をいくつかのイベント ハンドラーを設定して渡す、`MediaElement`オブジェクトを`SetNativeControl`:

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

2 つのイベント ハンドラーはでデタッチ、`Dispose`のレンダラーのイベントです。

## <a name="showing-the-transport-controls"></a>トランスポート コントロールの表示

3 つのプラットフォームに含まれるすべてのビデオ プレーヤーは、再生および一時停止、およびビデオ内の現在位置を示すために、新しい位置に移動し、バー ボタンが含まれているトランスポート コントロールの既定のセットをサポートします。

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

このプロパティは、両方が`set`と`get`プロパティが設定されている場合にのみ、ケースを処理するアクセサー、レンダラーがします。 `get`アクセサーは単にプロパティの現在の値を返します。

などのプロパティ`AreTransportControlsEnabled`プラットフォーム レンダラーが 2 つの方法で処理されます。

- 最初の時間は、Xamarin.Forms を作成するとき、`VideoPlayer`要素。 これを指定する、`OnElementChanged`レンダラーのオーバーライドときに、`NewElement`プロパティは使用されません`null`です。 この時点で、レンダラーを設定できますで定義されているプロパティの初期値から独自のプラットフォームのビデオ プレーヤーは、`VideoPlayer`です。

- 場合内のプロパティ`VideoPlayer`後で変更されると、次に、`OnElementPropertyChanged`レンダラーでメソッドが呼び出されます。 これにより、レンダラーでは、新しいプロパティの設定に基づいた、プラットフォーム ビデオ プレーヤーを更新できます。

ここでは、どのように`AreTransportControlsEnabled`プロパティは、3 つのプラットフォームで処理されます。

### <a name="ios-playback-controls"></a>iOS 再生コントロール

IOS のプロパティは、`AVPlayerViewController`表示を制御するコントロールは、トランスポートの[ `ShowsPlaybackControls`](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/)です。 IOS でそのプロパティを設定する方法を次に示します`VideoViewRenderer`:

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

`Element`レンダラーのプロパティを指す、`VideoPlayer`クラスです。

### <a name="the-android-media-controller"></a>Android メディア コント ローラー

Android で、トランスポート コントロールを表示する必要がありますを作成する、 [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/)オブジェクトと関連付けることで、`VideoView`オブジェクト。 しくみについてに示されている、`SetAreTransportControlsEnabled`メソッド。

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

### <a name="the-uwp-transport-controls-property"></a>UWP トランスポート コントロールのプロパティ

UWP`MediaElement`という名前のプロパティを定義[ `AreTransportControlsEnabled`](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled)からプロパティを設定するように、`VideoPlayer`同じ名前のプロパティ。

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

1 つ以上のプロパティは、ビデオの再生を開始するために必要: これは、重要な`Source`ビデオ ファイルを参照するプロパティです。 実装する、`Source`プロパティが次の記事で説明されている[Web ビデオを再生する](web-videos.md)です。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
