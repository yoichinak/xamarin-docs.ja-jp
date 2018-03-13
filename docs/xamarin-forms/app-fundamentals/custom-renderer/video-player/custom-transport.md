---
title: "カスタムのビデオ トランスポート コントロール"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: b0d871068f42a03b2aba3c1482a9236b19fe0db9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="custom-video-transport-controls"></a>カスタムのビデオ トランスポート コントロール

ビデオ プレーヤーのトランスポート コントロールには、機能を実行するボタンが含まれます。**再生**、**一時停止**、および**停止**です。 これらのボタンがテキストではなく、使い慣れたアイコンで識別される一般に、**再生**と**一時停止**関数が 1 つのボタンに結合され、通常します。

既定では、`VideoPlayer`表示トランスポート コントロールの各プラットフォームでサポートされています。 設定すると、`AreTransportControlsEnabled`プロパティを`false`、これらのコントロールが抑制されます。 制御できます、`VideoPlayer`プログラムによって。 または、独自のトランスポート コントロールを指定します。

## <a name="the-play-pause-and-stop-methods"></a>再生、一時停止、および停止方法

`VideoPlayer`クラスという 3 つのメソッドを定義`Play`、 `Pause`、および`Stop`イベントを発生させるにより実装されます。

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

これらのイベントのイベント ハンドラーが設定されて、`VideoPlayerRenderer`次に示すように、各プラットフォームのクラスします。

### <a name="ios-transport-implementations"></a>iOS のトランスポートの実装

IOS のバージョンの`VideoPlayerRenderer`を使用して、`OnElementChanged`これら 3 つのイベントのハンドラーを設定するメソッドをときに、`NewElement`プロパティは使用されません`null`とイベント ハンドラーの関連付けを解除時に`OldElement`は`null`:

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

イベント ハンドラーを実装してメソッドを呼び出すことによって、`AVPlayer`オブジェクト。 ない`Stop`メソッドを`AVPlayer`でビデオを一時停止と位置を先頭に移動、シミュレートしたため、します。

### <a name="android-transport-implementations"></a>Android のトランスポートを実装

Android の実装は、iOS の実装に似ています。 ときに 3 つの関数に対応するハンドラーが設定されます`NewElement`は`null`およびタイミングはデタッチ`OldElement`は`null`:

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

3 つの関数によって定義されたメソッドを呼び出す`VideoView`です。

### <a name="uwp-transport-implementations"></a>UWP トランスポートを実装

UWP、トランスポートの 3 つの関数の実装は、iOS と Android の実装の両方によく似ています。

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

## <a name="the-video-player-status"></a>ビデオ プレーヤー ステータス

実装する、**再生**、 **Pause**、および**停止**関数は、トランスポート コントロールをサポートするためだけでは不十分です。 多くの場合、**再生**と**一時停止**コマンドは、ビデオが再生中または一時停止中は現在あるかどうかを示すためにその外観を変更する同じボタンで実装されます。 さらに、このボタンは、ビデオがまだ読み込まれていないときにも有効べきではありません。

これらの要件は、ビデオ プレーヤーを利用できるように、現在の状態を示す再生または一時停止している場合、またはその準備ができていませんビデオを再生する必要があることを意味します。 (3 つのプラットフォームでサポートされていないために、これらのプロパティは、ビデオ ファイルではなく、ストリーミング ビデオの該当するかどうか、ビデオ、一時停止できるかの新しい位置に移動することができますを示すプロパティをサポートしても、`VideoPlayer`ここで説明します)。

**VideoPlayerDemos**プロジェクトに含まれる、 `VideoStatus` 3 つのメンバーを持つ列挙します。

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

`VideoPlayer`クラスという名前の実専用バインド可能なプロパティを定義する`Status`型の`VideoStatus`します。 このプロパティは、プラットフォーム レンダラーからしか設定する必要がありますので、読み取り専用として定義されます。

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

通常、読み取り専用のバインド可能なプロパティには、プライベート`set`でアクセサー、`Status`プロパティをクラス内から設定することを許可します。 `View`レンダラー、ただしでサポートされている派生する必要がありますプロパティからプラットフォーム レンダラーによってのみが、クラスの外部です。

名前を持つため、別のプロパティが定義されている`IVideoPlayerController.Status`です。 これは、明示的なインターフェイスの実装であり、によって可能になりますが、`IVideoPlayerController`インターフェイスを`VideoPlayer`クラスを実装します。

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

これは、方法に似ています[ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)コントロール、 [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/)インターフェイスを実装する、`CanGoBack`と`CanGoForward`プロパティです。 (のソース コードを参照してください[ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs)および詳細については、そのレンダラー)。

これにより、クラスの外部に`VideoPlayer`を設定する、`Status`プロパティを参照することによって、`IVideoPlayerController`インターフェイスです。 (わかりますコード間もなく。)同様に、他のクラスからプロパティを設定できますが、誤って設定される可能性はほとんどありません。 最も重要な`Status`プロパティを設定して、データ バインディングを使用できません。

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

### <a name="the-ios-status-setting"></a>IOS ステータスの設定

IOS`VideoPlayerRenderer`のハンドラーを設定、`UpdateStatus`イベント (とそのハンドラーの関連付けを解除するときに、基になる`VideoPlayer`要素が存在しない)、ハンドラーを使用して設定して、`Status`プロパティ。

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

2 つのプロパティの`AVPlayer`アクセスする必要があります。 [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/)型のプロパティ`AVPlayerStatus`と[ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/)型のプロパティ`AVPlayerTimeControlStatus`です。 注意して、`Element`プロパティ (これは、 `VideoPlayer`) にキャストする必要があります`IVideoPlayerController`を設定する、`Status`プロパティです。

### <a name="the-android-status-setting"></a>Android のステータスの設定

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) 、Android のプロパティ`VideoView`を示すブール値のみ、ビデオが再生中または一時停止しているかどうかは、します。 かどうかを`VideoView`したりすることも再生は、ビデオを一時停止をまだ、`Prepared`のイベント`VideoView`処理する必要があります。 これら 2 つのハンドラーが設定されている、`OnElementChanged`メソッド、およびデタッチ中に、`Dispose`オーバーライドします。

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

`UpdateStatus`ハンドラーを使用して、`isPrepared`フィールド (設定、`Prepared`ハンドラー) および`IsPlaying`プロパティを設定、`Status`プロパティ。

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

### <a name="the-uwp-status-setting"></a>UWP ステータスの設定

UWP`VideoPlayerRenderer`では、使用、`UpdateStatus`イベントでなく、その必要はありません、設定、`Status`プロパティです。 `MediaElement`定義、 [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged)レンダラーは、イベント通知を受けるときに、 [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState)プロパティが変更されました。 プロパティにデタッチ、`Dispose`をオーバーライドします。

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

Unicode 文字を使用してシンボリック**再生**、**一時停止**と**停止**イメージは問題が発生します。 [その他の技術](https://unicode-table.com/en/blocks/miscellaneous-technical/)Unicode 標準のセクションは、この目的に適した見かけ上の 3 つの記号を定義します。 これらの数値は、次のとおりです。

- 0x23F5 (黒中程度の右向きの三角形) または & #x23F5 です。**再生**
- 0x23F8 (二重の縦棒) または & #x23F8 です。**一時停止**
- 0x23F9 (黒の四角形) または & #x23F9 です。**停止**

関係なく方法お使いのブラウザーでこれらのシンボルが表示されます (およびさまざまな方法でさまざまなブラウザーの処理)、Xamarin.Forms でサポートされているプラットフォームで一貫して、表示されません。 IOS および UWP デバイスで、**一時停止**と**停止**文字がグラフィカルな外観を 3D の背景色を青と白のフォア グラウンドがあります。 これは、Android、シンボルが単に青色でケースがありません。 ただしの 0x23F5 codepoint**再生**UWP と上の同じ外観が iOS および Android 上であってもサポートされていることはありません。

そのための 0x23F5 コード ポイントを使用できません**再生**です。 適切な代替です。

- 0x25B6 (黒の右向き三角形) または & #x25B6 です。**再生**

3D の外観は似ていませんプレーンの黒い三角形がする点を除いてすべての 3 つのプラットフォームでサポートされる**Pause**と**停止**です。 1 つの可能性は、バリアント型のコードで 0x25B6 コード ポイントには。

- 0xFE0F (バリアント型 16) 続けて 0x25B6 または & #x25B6; & #xFE0F です。**再生**

これは、次に示すマークアップで使用されるものです。 これにより、iOS で、**再生**記号と同じ 3D 外観、**一時停止**と**停止**Android および UWP でボタンが、バリアント型は機能しません。

**カスタム トランスポート**セット ページ、 **AreTransportControlsEnabled**プロパティを**false**が含まれています、`ActivityIndicator`ビデオの読み込み時に表示されると 2 つのボタン。 `DataTrigger` オブジェクトを使用して有効にして、無効化、`ActivityIndicator`とボタンのとの間での最初のボタンを切り替えるには**再生**と**一時停止**:

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

データ トリガーが、記事で詳しく説明されている[データ トリガー](~/xamarin-forms/app-fundamentals/triggers.md#data)です。

分離コード ファイルは、ボタンに対応するハンドラー`Clicked`イベント。

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

`AutoPlay`に設定されている`false`で、 **CustomTransport.xaml**ファイル、キーを押す必要があります、**再生**が有効にビデオを開始するときにボタンをクリックします。 ボタンが定義され、上記で説明した Unicode 文字が、対応するテキスト付加されます。 ボタンは、各プラットフォームで、一貫した外観があるビデオを再生する場合。

[![カスタム トランスポート再生](custom-transport-images/customtransportplaying-small.png "カスタム トランスポート再生")](custom-transport-images/customtransportplaying-large.png#lightbox "カスタム トランスポートの再生")

Android と UWP、ですが、**再生**ボタンの外観、ビデオが一時停止しているときに非常に異なる。

[![カスタム トランスポートを一時停止](custom-transport-images/customtransportpaused-small.png "カスタム トランスポートを一時停止")](custom-transport-images/customtransportpaused-large.png#lightbox "カスタム トランスポートを一時停止")

実稼働アプリケーションでおそらくにしておく visual 統一性を実現するために、ボタンの独自のビットマップ イメージを使用します。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
