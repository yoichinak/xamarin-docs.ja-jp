---
title: カスタムのビデオ トランスポート コントロール
description: この記事では、Xamarin.Forms を使ってビデオ プレーヤー アプリケーションにカスタムのトランスポート コントロールを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c6aa4aed134667f25b3822c7604b85e27a404a3a
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870158"
---
# <a name="custom-video-transport-controls"></a>カスタムのビデオ トランスポート コントロール

[![Dサンプルのダウンロード](~/media/shared/download.png) サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)

ビデオ プレーヤーのトランスポート コントロールには、**再生**、**一時停止**、および**停止**機能を実行するボタンが含まれています。 これらのボタンは一般的に、テキストではなく使い慣れたアイコンで識別されます。また、**再生**と**一時停止**機能は一般的に、1 つのボタンに結合されています。

既定では、`VideoPlayer` には各プラットフォームでサポートされているトランスポート コントロールが表示されます。 `AreTransportControlsEnabled` プロパティを `false` に設定すると、これらのコントロールは表示されません。 設定後、`VideoPlayer` をプログラムで制御することも、独自のトランスポート コントロールを指定することもできます。

## <a name="the-play-pause-and-stop-methods"></a>再生、一時停止、および停止メソッド

`VideoPlayer` クラスでは、`Play`、`Pause`、および`Stop` という名前の 3 つのメソッドが定義されており、イベントを発生させることにより実装されます。

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

これらのイベントのイベント ハンドラーは、次に示すように、各プラットフォーム内で `VideoPlayerRenderer` により設定されます。

### <a name="ios-transport-implementations"></a>iOS でのトランスポートの実装

iOS バージョンの `VideoPlayerRenderer` の場合、`OnElementChanged` メソッドの使用により、`NewElement` プロパティが `null` 以外のときにこれらの 3 つのイベントのハンドラーが設定され、`OldElement` が `null`以外のときにイベント ハンドラーがデタッチされます。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

`AVPlayer` オブジェクト上でメソッドを呼び出すことにより、イベント ハンドラーが実装されます。 `AVPlayer` には `Stop` メソッドは存在しないため、ビデオを一時停止して位置を先頭に移動することで、同じ動作が実現されます。

### <a name="android-transport-implementations"></a>Android でのトランスポートの実装

Android での実装方法は、iOS での実装方法に似ています。 3 つの機能のハンドラーは `NewElement` が `null` 以外のときに設定され、`OldElement` が `null` 以外のときにデタッチされます。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

`VideoView` によって定義されたメソッドが、3 つの機能により呼び出されます。

### <a name="uwp-transport-implementations"></a>UWP でのトランスポートの実装

UWP での 3 つのトランスポート機能の実装方法は、iOS と Android での実装方法の両方によく似ています。

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>ビデオ プレーヤーの状態

**再生**、**一時停止**、**停止**機能を実装するだけでは、トランスポート コントロールをサポートするためには不十分です。 多くの場合、**再生**と**一時停止**コマンドは同じボタンに実装されます。このボタンの外観は、ビデオが現在再生中であるか一時停止中であるかによって変わります。 さらに、ビデオがまだ読み込まれていない場合、ボタンは有効にすべきではありません。

これらの要件は、ビデオ プレーヤーの現在の状態、つまり再生中か一時停止中か、またはまだ再生準備ができていないかということが示される必要があるということを、意味します。 (各プラットフォームでは、ビデオが一時停止可能であること、または新しい位置に移動可能であることを示すプロパティもサポートされています。しかし、これらのプロパティは、ビデオ ファイルではなくストリーミング ビデオに適用されるものであるため、ここで説明している `VideoPlayer` においてはサポートされていません。)

**VideoPlayerDemos** プロジェクトには、3 つのメンバーが存在する `VideoStatus` 列挙が含まれています。

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

`VideoPlayer` クラスにより、`Status` 型の `VideoStatus` という名前の、読み取り専用のバインド可能なプロパティが定義されます。 このプロパティの設定はプラットフォームのレンダラーからのみ行われる必要があるため、読み取り専用として定義されます。

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

通常、読み取り専用のバインド可能なプロパティには、クラス内からの設定を可能にするため、`Status` プロパティに対するプライベート `set` アクセサーが存在します。 ただし、レンダラーでサポートされている `View` 派生物の場合は、プロパティはクラスの外部から、プラットフォーム レンダラーのみによって設定される必要があります。

このため、`IVideoPlayerController.Status` という名前の別のプロパティが定義されます。 これは、明示的なインターフェイスの実装であり、`VideoPlayer` クラスによって実装された `IVideoPlayerController` インターフェイスによって可能となります。

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

これは、[`WebView`](xref:Xamarin.Forms.WebView) コントロールによる、[`IWebViewController`](xref:Xamarin.Forms.IWebViewController) インターフェイスを使用した `CanGoBack` と `CanGoForward` プロパティの実装方法に似ています。 (詳細については、[`WebView`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) のソース コードとそのレンダラーを参照してください。)

これにより、`VideoPlayer` の外部クラスに対して、`IVideoPlayerController` インターフェイスの参照による `Status` プロパティの設定を可能とすることができます。 (コードはこの後に示します。)プロパティは他のクラスからも同様に設定可能ですが、誤って設定される可能性はほとんどありません。 最も重要なこととして、`Status` プロパティはデータ バインドを通じて設定することはできません。

この `Status` プロパティがレンダラーで常に最新の状態に保たれることを支援するため、10 秒ごとにトリガーされる `UpdateStatus` イベントが `VideoPlayer` クラスにより定義されます。

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>iOS での状態の設定

iOS `VideoPlayerRenderer` により、`UpdateStatus` イベントのハンドラーが設定されます (基になる `VideoPlayer` 要素が存在しない場合、そのハンドラーはデタッチされます)。そのハンドラーは、`Status` プロパティの設定に使用されます。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

`AVPlayer` の 2 つのプロパティにアクセスする必要があります:`AVPlayerStatus` 型の [`Status`](xref:AVFoundation.AVPlayer.Status*) プロパティと、`AVPlayerTimeControlStatus` 型の [`TimeControlStatus`](xref:AVFoundation.AVPlayer.TimeControlStatus*) プロパティです。 `Element` プロパティ (`VideoPlayer`) は、`Status` プロパティを設定するために `IVideoPlayerController` へとキャストする必要があることに注意してください。

### <a name="the-android-status-setting"></a>Android での状態の設定

Android `VideoView` の [`IsPlaying`](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) プロパティは、ビデオが再生中か一時停止中かのみを示すブール値です。 `VideoView` でビデオがまだ再生や一時停止できないことを確認するには、`Prepared` の `VideoView` イベントを処理する必要があります。 これらの 2 つのハンドラーは `OnElementChanged` メソッド内で設定され、`Dispose` のオーバーライド中にデタッチされます。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`UpdateStatus` ハンドラーでは、`isPrepared` フィールド (`Prepared` ハンドラー内に設定) と `IsPlaying` プロパティの使用により、`Status` プロパティが設定されます。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>UWP での状態の設定

UWP `VideoPlayerRenderer` では `UpdateStatus` イベントが利用されていますが、`Status` プロパティの設定には必要ありません。 `MediaElement` によって定義される [ `CurrentStateChanged`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentStateChanged) イベントにより、[`CurrentState`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentState*) プロパティの変更時にレンダラーに通知が送られます。 このプロパティは `Dispose` のオーバーライド時にデタッチされます。

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`CurrentState` プロパティの型は [`MediaElementState`](/uwp/api/windows.ui.xaml.media.mediaelementstate) であり、`VideoStatus` に簡単にマッピング可能です。

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>再生、一時停止、および停止ボタン

シンボリックな**再生**、**一時停止**、および**停止**イメージに対して Unicode 文字を使用することには、問題があります。 Unicode 標準の「[Miscellaneous Technical](https://unicode-table.com/en/blocks/miscellaneous-technical/)」 (その他の技術用記号) セクションに、外見的にはこの目的に適した 3 つの記号が定義されています。 これらの数値は、次のとおりです。

- 0x23F5 (黒の中サイズの右向き三角形) &#x23F5;、**再生**向け
- 0x23F8 (二重の縦棒) &#x23F8;、**一時停止**向け
- 0x23F9 (黒い四角) &#x23F9;、**停止**向け

ご利用のブラウザーでこれらの記号がどのように表示されるか (および各ブラウザーによる記号の取り扱い方法の違い) に関わらず、Xamarin.Forms でサポートされる各プラットフォーム上での表示は一貫していません。 iOS および UWP デバイスの場合、**一時停止**と**停止**文字は、青色の 3D 背景と白い前景という外観で表示されます。 Android の場合は、単純な青の記号として表示されます。 一方で、**再生**向けのコードポイント 0x23F5 の場合、UWP 上では同じような外観では表示されません、iOS と Android ではサポートもされていません。

そのため、0x23F5 コード ポイントは**再生**用に使用することはできません。 有効な代替方法は次のとおりです。

- 0x25B6 (黒の右向き三角形) &#x25B6;、**再生**向け

これは各プラットフォームでサポートされています。ただし、**一時停止**と**停止**の 3D 外観とは異なる、単純な黒の三角形が表示されます。 この場合、0x25B6 コードポイントの後にバリアント コードを付けるという方法があります。

- 0x25B6 の後に 0xFE0F (バリアント 16) &#x25B6;&#xFE0F;、**再生**向け

これは、次に示すマークアップで使用されているものです。 iOS の場合、これで**一時停止**および**停止**ボタンと同じ 3D の外観が**再生**記号に与えられますが、このバリアントは Android と UWP の場合は機能しません。

**Custom Transport** ページにより **AreTransportControlsEnabled** プロパティが **false** に設定され、ビデオのロード時に表示される `ActivityIndicator` と、2 つのボタンが含まれます。 `DataTrigger` オブジェクトが、`ActivityIndicator` とボタンの有効と無効の切り替え、最初のボタンの**再生**と**一時停止**間の切り替えに、使用されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

データ トリガーの詳細については、「[データ トリガー](~/xamarin-forms/app-fundamentals/triggers.md#data)」の記事を参照してください。

分離コード ファイルには、ボタンの `Clicked` イベントに対するハンドラーが存在します。

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

**CustomTransport.xaml** ファイル内で `AutoPlay` が `false` に設定されているため、ビデオの再生が可能になったら、**再生**ボタンを押す必要があります。 ボタンは、上で説明した Unicode 文字に対応するテキストが共に表示されるように、定義されています。 ビデオ再生時のボタンの外観は、各プラットフォーム間で一貫したものとなります。

[![Custom Transport 再生中](custom-transport-images/customtransportplaying-small.png "Custom Transport 再生中")](custom-transport-images/customtransportplaying-large.png#lightbox "Custom Transport 再生中")

ただし、Android と UWP では、ビデオを一時停止したときの**再生**ボタンの外観が大きく異なります。

[![Custom Transport 一時停止中](custom-transport-images/customtransportpaused-small.png "Custom Transport 一時停止中")](custom-transport-images/customtransportpaused-large.png#lightbox "Custom Transport 一時停止中")

実稼働アプリケーションでは、外観に統一性を持たせるため、独自のビットマップ イメージをボタンに使用することもできます。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
