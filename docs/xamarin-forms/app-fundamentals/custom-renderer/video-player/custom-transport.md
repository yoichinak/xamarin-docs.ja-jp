---
title: カスタムのビデオ トランスポート コントロール
description: この記事では、Xamarin.Forms を使用して、ビデオ プレーヤー アプリケーションにカスタム トランスポート コントロールを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 3397c931dcb23a29b0682699512a5b4c9018de38
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171067"
---
# <a name="custom-video-transport-controls"></a>カスタムのビデオ トランスポート コントロール

ビデオ プレーヤーのトランスポート コントロールに、機能を実行するボタンは、**再生**、**一時停止**、および**停止**します。 これらのボタンがテキストではなく、使い慣れたアイコンで識別される一般に、**再生**と**一時停止**関数は 1 つのボタンに一般的に結合されます。

既定で、`VideoPlayer`トランスポート コントロールの各プラットフォームでサポートされているが表示されます。 設定すると、`AreTransportControlsEnabled`プロパティを`false`、これらのコントロールは表示されません。 制御することができますし、`VideoPlayer`プログラムで。 または、独自のトランスポート コントロールを指定します。

## <a name="the-play-pause-and-stop-methods"></a>再生、一時停止、および停止方法

`VideoPlayer`クラスという 3 つのメソッドを定義する`Play`、 `Pause`、および`Stop`イベントを発生させるにより実装されます。

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

これらのイベントのイベント ハンドラーによって設定、`VideoPlayerRenderer`次に示すように、各プラットフォームのクラスします。

### <a name="ios-transport-implementations"></a>iOS のトランスポートの実装

IOS バージョン`VideoPlayerRenderer`を使用して、`OnElementChanged`これら 3 つのイベントのハンドラーを設定する方法と、`NewElement`プロパティが`null`をイベント ハンドラーをデタッチするとき`OldElement`でない`null`:

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

イベント ハンドラーを実装してメソッドを呼び出すことによって、`AVPlayer`オブジェクト。 ない`Stop`メソッドの`AVPlayer`ビデオを一時停止して、位置を先頭に移動して、シミュレートしたため、します。

### <a name="android-transport-implementations"></a>Android のトランスポートの実装

Android の実装では、iOS のカスタム実装に似ています。 ときに 3 つの関数のハンドラーが設定されます`NewElement`ない`null`デタッチされたときに、`OldElement`ない`null`:

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

3 つの関数によって定義されたメソッドを呼び出す`VideoView`します。

### <a name="uwp-transport-implementations"></a>UWP のトランスポートの実装

トランスポートの 3 つの関数の UWP 実装は、iOS と Android の実装の両方によく似ています。

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

実装する、**再生**、**一時停止**、および**停止**関数は、トランスポート コントロールをサポートするためには、不十分です。 多くの場合、**再生**と**一時停止**ビデオが再生または一時停止中では現在あるかどうかを示すには、その外観を変更する ボタンが同じコマンドが実装されます。 さらに、ビデオがまだ読み込まれていない場合、ボタンが有効でもべきではないにします。

これらの要件は、ビデオ プレーヤーを使用できるようにする、現在の状態を示すがまだビデオを再生する準備ができていない場合は再生または一時停止すると場合、または必要があることを意味します。 (各プラットフォームでサポートされていないために、これらのプロパティは、ビデオ ファイルではなく、ストリーミング ビデオの該当するかどうかは、ビデオの一時停止できる、またはの新しい位置に移動することができますを示すプロパティもサポートしています、`VideoPlayer`ここで説明します)。

**VideoPlayerDemos**プロジェクトが含まれています、 `VideoStatus` 3 つのメンバーの列挙。

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

`VideoPlayer`クラスという名前の現実専用バインド可能なプロパティを定義する`Status`型の`VideoStatus`します。 このプロパティは、プラットフォームのレンダラーからしか設定する必要がありますので、読み取り専用として定義されます。

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

プライベートする読み取り専用のバインド可能なプロパティが、通常なら`set`でアクセサー、`Status`プロパティをクラス内から設定することを許可します。 `View`レンダラー、ただしでサポートされている派生クラス、プロパティからプラットフォーム レンダラーによってのみが、クラスの外部です。

名前を持つため、別のプロパティが定義されている`IVideoPlayerController.Status`します。 これは、明示的なインターフェイスの実装でありはによって可能になります、`IVideoPlayerController`インターフェイス、`VideoPlayer`クラスが実装します。

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

これは、方法に似ています[ `WebView` ](xref:Xamarin.Forms.WebView)コントロール、 [ `IWebViewController` ](xref:Xamarin.Forms.IWebViewController)インターフェイスを実装するために、`CanGoBack`と`CanGoForward`プロパティ。 (のソース コードを参照してください[ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs)と詳細については、そのレンダラー。)。

これにより、クラスの外部に`VideoPlayer`を設定する、`Status`プロパティを参照して、`IVideoPlayerController`インターフェイス。 (わかりますコードすぐ。)同様に、他のクラスからプロパティを設定できますが、誤って設定される可能性はほとんどありません。 最も重要なこととして、`Status`データ バインドを通じてプロパティを設定することはできません。

これを保つためのレンダラーを支援するために`Status`更新されると、プロパティ、`VideoPlayer`クラスを定義、`UpdateStatus`イベントはトリガーの秒部分の 10 分ごと。

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

### <a name="the-ios-status-setting"></a>IOS の状態の設定

IOS`VideoPlayerRenderer`のハンドラーを設定、`UpdateStatus`イベント (とそのハンドラーをデタッチときに、基になる`VideoPlayer`要素が存在しない)、ハンドラーを使用して設定して、`Status`プロパティ。

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

2 つのプロパティの`AVPlayer`アクセスする必要があります。 [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/)型のプロパティ`AVPlayerStatus`と[ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/)型のプロパティ`AVPlayerTimeControlStatus`します。 注意、`Element`プロパティ (これは、 `VideoPlayer`) にキャストする必要があります`IVideoPlayerController`を設定する、`Status`プロパティ。

### <a name="the-android-status-setting"></a>Android のステータスの設定

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/)プロパティは、Android の`VideoView`のみ、ビデオは再生または一時停止中のかどうかを示すブール値です。 あるかどうかを`VideoView`したりすることも再生はビデオを一時停止をまだ、`Prepared`のイベント`VideoView`処理する必要があります。 これら 2 つのハンドラーが設定されて、`OnElementChanged`メソッドをおよびデタッチ中に、`Dispose`オーバーライドします。

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

`UpdateStatus`ハンドラーを使用して、`isPrepared`フィールド (設定、`Prepared`ハンドラー) および`IsPlaying`プロパティを設定する、`Status`プロパティ。

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

### <a name="the-uwp-status-setting"></a>UWP のステータス設定

UWP`VideoPlayerRenderer`の利用、`UpdateStatus`イベントではなく、不要の設定、`Status`プロパティ。 `MediaElement`定義、 [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged)レンダラーは、イベント時に通知が、 [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState)プロパティが変更されました。 プロパティがデタッチ、`Dispose`をオーバーライドします。

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

`CurrentState`プロパティの型は[ `MediaElementState`](/uwp/api/windows.ui.xaml.media.mediaelementstate)に簡単にマップし、 `VideoStatus`:

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

## <a name="play-pause-and-stop-buttons"></a>再生、一時停止、および中止 ボタン

Unicode 文字を使用してシンボリック**再生**、**一時停止**、および**停止**イメージは問題になります。 [その他のテクニカル](https://unicode-table.com/en/blocks/miscellaneous-technical/)Unicode 標準のセクションは、一見、この目的に適した次の 3 つの記号を定義します。 これらの数値は、次のとおりです。

- 0x23F5 (黒中程度の右向き三角形) または&#x23F5;の**再生**
- 0x23F8 (二重の縦棒) または&#x23F8;の**一時停止**
- 0x23F9 (黒い四角) または&#x23F9;の**停止**

関係なく方法お使いのブラウザーでこれらのシンボルが表示されます (およびさまざまな方法でさまざまなブラウザーの処理)、Xamarin.Forms でサポートされるプラットフォームで一貫した方法が表示されません。 IOS および UWP デバイスで、**一時停止**と**停止**文字がある 3D 青色の背景と白い前景色をグラフィカル表示します。 Android では、シンボルが単に青は発生しません。 ただしの 0x23F5 codepoint**再生**UWP とで同じ外観が iOS および Android 上であってもサポートされていることはありません。

そのための 0x23F5 コード ポイントを使用することはできません**再生**します。 有効な代替方法は次のとおりです。

- 0x25B6 (黒右向き三角形) または&#x25B6;の**再生**

3D の外観が似ていないプレーンの黒い三角形が点が各プラットフォームでサポートされる**一時停止**と**停止**します。 1 つの方法では、バリアントのコードで 0x25B6 codepoint に従ってください。

- 0xFE0F (バリアント型は 16) 続けて 0x25B6 または&#x25B6;&#xFE0F;の**再生**

これは、次に示すマークアップで使用されるものです。 Ios では、その、**再生**記号と同じ 3D の外観、**一時停止**と**停止**ボタンが、バリアントは、Android、UWP で機能しません。

**カスタム トランスポート**ページ セット、 **AreTransportControlsEnabled**プロパティを**false**が含まれています、`ActivityIndicator`ビデオの読み込み時に表示されると 2 つのボタン。 `DataTrigger` オブジェクトを使用して有効にして、無効にする、`ActivityIndicator`とボタンの最初のボタン間を切り替えると**再生**と**一時停止**:

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

データ トリガーは、情報の記事で詳しく説明[データ トリガー](~/xamarin-forms/app-fundamentals/triggers.md#data)します。

分離コード ファイルは、ボタンのハンドラーを持つ`Clicked`イベント。

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

`AutoPlay`に設定されている`false`で、 **CustomTransport.xaml**ファイル、キーを押す必要があります、**再生**ビデオの開始を有効になるときにボタンをクリックします。 ボタンが定義され、前に説明した Unicode 文字には、対応するテキストが付いています。 ボタンは、各プラットフォームで、一貫した外観があるビデオを再生するとき。

[![カスタム トランスポート再生](custom-transport-images/customtransportplaying-small.png "カスタム トランスポート再生")](custom-transport-images/customtransportplaying-large.png#lightbox "カスタム トランスポートの再生")

Android、UWP、**再生**ビデオを一時停止すると、ボタンが大きく異なります。

[![カスタム トランスポートを一時停止](custom-transport-images/customtransportpaused-small.png "カスタム トランスポートを一時停止")](custom-transport-images/customtransportpaused-large.png#lightbox "カスタム トランスポートを一時停止")

実稼働アプリケーションでは、visual 統一性を実現するために、独自のビットマップ イメージのボタンを使用するをする可能性があります。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
