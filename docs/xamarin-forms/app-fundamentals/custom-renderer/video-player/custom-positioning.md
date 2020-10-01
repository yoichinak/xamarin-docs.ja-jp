---
title: カスタムのビデオ位置
description: この記事では、Xamarin.Forms を使用してビデオ プレーヤー アプリケーションのカスタムの位置バーを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 00b6617f754eb251eae0a7be9715deb840c33102
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562744"
---
# <a name="custom-video-positioning"></a>カスタムのビデオ位置

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

各プラットフォームで実装されているトランスポート コントロールには、位置バーが含まれます。 このバーはスライダーまたはスクロール バーに似ており、ビデオの合計時間内の現在の位置を示します。 さらに、ユーザーは位置バーを操作して、ビデオ内の新しい位置へと前後に移動することができます。

この記事では、独自のカスタムの位置バーを実装する方法について説明します。

## <a name="the-duration-property"></a>Duration プロパティ

`VideoPlayer` でカスタムの位置バーをサポートするために必要な情報の 1 つは、ビデオの再生時間です。 `VideoPlayer` には、種類が `TimeSpan` の読み取り専用の `Duration` プロパティが定義されています。

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

[前の記事](custom-transport.md)で説明した `Status` プロパティと同様に、この `Duration` プロパティは読み取り専用です。 プライベート `BindablePropertyKey` を使用して定義されており、設定するには、この `Duration` プロパティを含む `IVideoPlayerController` インターフェイスを参照する必要があります。

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

この記事で後述する `SetTimeToEnd` というメソッドを呼び出すプロパティ変更ハンドラーにも注意してください。

`VideoPlayer` の `Source` プロパティが設定された直後には、ビデオの再生時間を*取得できません*。 基となるビデオ プレーヤーで再生時間が決定されるには、ビデオ ファイルの一部がダウンロードされる必要があります。

ここでは、各プラットフォーム レンダラーがビデオの再生時間を取得する方法について説明します。

### <a name="video-duration-in-ios"></a>iOS のビデオの再生時間

iOS では、ビデオの再生時間は `AVPlayerItem` の `Duration` プロパティから取得されますが、`AVPlayerItem` の作成直後は取得されません。 `Duration` プロパティの iOS オブザーバーを設定できますが、`VideoPlayerRenderer` では、1 秒に 10 回呼び出される `UpdateStatus` メソッドで再生時間が取得されます。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

`ConvertTime` メソッドによって `CMTime` オブジェクトが `TimeSpan` 値に変換されます。

### <a name="video-duration-in-android"></a>Android のビデオの再生時間

Android の `VideoView` の `Duration` プロパティでは、`VideoView` の `Prepared` イベントが発生したときに有効な再生時間がミリ秒単位で報告されます。 Android の `VideoPlayerRenderer` クラスでは、そのハンドラーを使用して `Duration` プロパティが取得されます。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>UWP のビデオの再生時間

`MediaElement` の `NaturalDuration` プロパティは `TimeSpan` 値であり、`MediaElement` によって `MediaOpened` イベントを発生したときに有効になります。

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Position プロパティ

`VideoPlayer` には、ビデオの再生時に 0 から `Duration` まで増加する `Position` プロパティも必要です。 `VideoPlayer` では、UWP `MediaElement` の `Position` プロパティと同様にこのプロパティが実装されています。これは、パブリック `set` および `get` アクセサーとの通常のバインドが可能なプロパティです。

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

`get` アクセサーからは、再生されているビデオの現在の位置が返されますが、`set` アクセサーは、ユーザーの位置バー操作に対してビデオ位置を前後に動かして応答するためのものです。

iOS と Android では、現在の位置を取得するプロパティには `get` アクセサーしかありません。また、`Seek` メソッドを使用して、この 2 つ目のタスクを実行できます。 この点について考えると、別の `Seek` メソッドの方が、単一の `Position` プロパティよりも実用的なアプローチのようです。 単一の `Position` プロパティには固有の問題があります。ビデオを再生する場合、新しい位置を反映するために `Position` プロパティを継続的に更新する必要があります。 ただし、ほとんどの場合、`Position` プロパティの変更によって、ビデオ プレーヤーでビデオの新しい位置に移動することは望ましくありません。 このような処理の結果、ビデオ プレーヤーは `Position` プロパティの最後の値までシークする処理で応答するので、ビデオは進みません。

`set` および `get` アクセサーを使用して `Position` プロパティを実装することは困難ですが、このアプローチは、UWP `MediaElement` と一貫性があり、データ バインディングに関する大きな利点があります。つまり、`VideoPlayer` の `Position` プロパティは、位置の表示と新しい位置のシークの両方に使用されるスライダーにバインドすることができます。 ただし、この `Position` プロパティを実装する場合は、フィードバック ループを避けるためにいくつかの事前の注意が必要です。

### <a name="setting-and-getting-ios-position"></a>iOS の位置の設定と取得

iOS では、`AVPlayerItem` オブジェクトの `CurrentTime` プロパティは再生中のビデオの現在の位置を示します。 iOS の `VideoPlayerRenderer` では、`Duration` プロパティの設定と同時に、`UpdateStatus` ハンドラーの `Position` プロパティを設定します。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

レンダラーによって、`VideoPlayer` から設定された `Position` プロパティが `OnElementPropertyChanged` のオーバーライドで変更されたときが検出され、その新しい値を使用して `AVPlayer` オブジェクトに対して `Seek` メソッドが呼び出されます。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

`VideoPlayer` の `Position` プロパティが `OnUpdateStatus` ハンドラーから設定されるたびに、`Position` プロパティから `PropertyChanged` イベントが発生され、`OnElementPropertyChanged` のオーバーライドで検出される点に留意してください。 このような変更のほとんどの場合、`OnElementPropertyChanged` メソッドで何も実行する必要はありません。 それ以外の場合は、ビデオの位置が変更されるたびに、到達していた同じ位置まで移動されます。

このフィードバック ループを避けるために、`Position` プロパティと `AVPlayer` の現在の位置との違いが 1 秒を超える場合にのみ、`OnElementPropertyChanged` メソッドから `Seek` が呼び出されます。

### <a name="setting-and-getting-android-position"></a>Android の位置の設定と取得

iOS レンダラーと同様に、Android の `VideoPlayerRenderer` では、`OnUpdateStatus` ハンドラーで `Position` プロパティの新しい値を設定します。 `VideoView` の `CurrentPosition` プロパティには、新しい位置がミリ秒単位で設定されています。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

また、iOS レンダラーの場合と同様に、Android レンダラーでは、`Position` プロパティが変更され、さらに、`VideoView` の `CurrentPosition` 値との違いが 1 秒を超える変更の場合にのみ、`VideoView` の `SeekTo` メソッドが呼び出されます。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>UWP の位置の設定と取得

UWP の `VideoPlayerRenderer` では、iOS や Android のレンダラーと同じ方法で `Position` が処理されますが、UWP の `MediaElement` の `Position` プロパティも `TimeSpan` 値なので、変換は必要ありません。

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>TimeToEnd プロパティの計算

ビデオ プレーヤーにビデオの残り時間が表示されることがあります。 この値は、ビデオの開始時にはビデオの再生時間から始まり、ビデオの終了時には 0 まで減少します。

`VideoPlayer` には読み取り専用の `TimeToEnd` プロパティがあります。これは、`Duration` および `Position` プロパティの変更に基づいて完全にクラス内で処理されます。

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

`SetTimeToEnd` メソッドは、`Duration` と `Position` の両方のプロパティ変更ハンドラーから呼び出されます。

## <a name="a-custom-slider-for-video"></a>ビデオのカスタム スライダー

位置バーのカスタム コントロールを記述することや、Xamarin.Forms の `Slider`、または `Slider` から派生するクラス (たとえば、次の `PositionSlider` クラスなど) を使用することができます。 このクラスでは、種類が `TimeSpan` の `Duration` および `Position` という 2 つの新しいプロパティを定義します。これらは、`VideoPlayer` の同じ名前の 2 つのプロパティにデータをバインドすることを目的としたプロパティです。 `Position` プロパティの既定のバインド モードは双方向である点に注意してください。

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

`Duration` プロパティのプロパティ変更ハンドラーによって、基となる `Slider` の `Maximum` プロパティが `TimeSpan` 値の `TotalSeconds` プロパティに設定されます。 同様に、`Position` のプロパティ変更ハンドラーによって、`Slider` の `Value` プロパティが設定されます。 このようにして、基となる `Slider` で `PositionSlider` の位置が追跡されます。

`PositionSlider` は、ある場合にのみ、基となる `Slider` から更新されます。つまり、ユーザーが `Slider` を操作して、ビデオを新しい位置に進めるか戻るかを示すときです。 これは、`PositionSlider` のコンストラクターの `PropertyChanged` ハンドラーで検出されます。 このハンドラーでは `Value` プロパティの変更が確認され、`Position` プロパティと異なる場合は、`Position` プロパティが `Value` プロパティから設定されます。

理論的には、内部の `if` ステートメントは次のように記述することができます。

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

ただし、Android の `Slider` の実装には、`Minimum` と `Maximum` の設定に関係なく、個別の 1,000 個のステップのみが含まれています。 ビデオの長さが 1,000 秒を超えると、2 つの異なる `Position` の値が `Slider` の同じ `Value` 設定に対応し、この `if` ステートメントによって `Slider` のユーザー操作に対して偽陽性がトリガーされます。 代わりに、新しい位置と既存の位置が全体の再生時間の 100 分の 1 を超えているかどうかを確認する方が安全です。

## <a name="using-the-positionslider"></a>PositionSlider の使用

UWP [`MediaElement`](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) に関するドキュメントでは、プロパティの更新頻度が高くなるため、`Position` プロパティへのバインドについて警告しています。 ドキュメントでは、`Position` プロパティのクエリにタイマーを使用することを推奨しています。

これは適切な推奨事項ですが、3 つの `VideoPlayerRenderer` クラスは、`Position` プロパティを更新するために、既にタイマーを間接的に使用しています。 `Position` プロパティは、`UpdateStatus` イベントのハンドラーで変更されます。このイベントの発生は 1 秒に 10 回のみです。

そのため、 **[Custom Position Bar]\(カスタムの位置バー\)** ページに示されているように、パフォーマンスの問題を起こすことなく `VideoPlayer` の `Position` プロパティを `PositionSlider` の `Position` プロパティにバインドできます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

最初の省略記号 (...) には `ActivityIndicator` が隠れています。以前の **[Custom Transport]\(カスタム トランスポート\)** ページと同じです。 2 つの `Label` 要素は、`Position` および `TimeToEnd` プロパティを示している点に注意してください。 これら 2 つの `Label` 要素の間にある省略記号には、再生、一時停止、および停止に関する **[Custom Transport]\(カスタム トランスポート\)** ページに表示される 2 つの `Button` 要素が隠れています。 コードビハインド ロジックは、 **[Custom Transport]\(カスタム トランスポート\)** ページと同じです。

[![カスタム位置](custom-positioning-images/custompositioning-small.png "カスタム位置")](custom-positioning-images/custompositioning-large.png#lightbox "カスタム位置")

以上で `VideoPlayer` の説明は終了です。

## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)