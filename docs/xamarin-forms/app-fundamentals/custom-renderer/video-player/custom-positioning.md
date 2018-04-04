---
title: ビデオのカスタムの配置
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 51256f078411f0ade72867544ced3d9a3a91d7b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="custom-video-positioning"></a>ビデオのカスタムの配置

各プラットフォームによって実装されるトランスポート コントロールには、位置バーが含まれます。 このバーは、スクロール バーのスライダーに似ていて、その合計の時間内でビデオの現在の場所を示します。 さらに、ユーザーは、ビデオ内の新しい位置に前方または後方に移動する位置バーを操作できます。

この記事では、独自のカスタム位置バーを実装する方法を示します。

## <a name="the-duration-property"></a>Duration プロパティ

情報の 1 つの項目を`VideoPlayer`カスタムをサポートする必要があるバーの位置は、ビデオの期間。 `VideoPlayer`読み取り専用に定義`Duration`型のプロパティ`TimeSpan`:

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

同様に、`Status`で説明されているプロパティ、[前の記事](custom-transport.md)、この`Duration`プロパティは読み取り専用です。 プライベートで定義されて`BindablePropertyKey`参照することによってのみ設定でき、`IVideoPlayerController`インターフェイスで、これが含まれています`Duration`プロパティ。

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

また、プロパティ変更ハンドラーをという名前のメソッドを呼び出すことを確認`SetTimeToEnd`この記事の後半に記載されています。

ビデオの期間は*いない*後すぐに利用可能な`Source`プロパティの`VideoPlayer`設定されています。 基になる、ビデオ プレーヤーはその期間を特定できる前に、ビデオ ファイルを部分的にダウンロードする必要があります。

ビデオの期間を取得する各プラットフォームのレンダラーの方法を次に示します。

### <a name="video-duration-in-ios"></a>IOS でのビデオの期間

Ios では、ビデオの期間がから取得した、`Duration`のプロパティ`AVPlayerItem`、した直後には、`AVPlayerItem`が作成します。 IOS オブザーバーを設定することは、`Duration`プロパティ、ですが、`VideoPlayerRenderer`期間の長さを取得、`UpdateStatus`秒間に 10 回呼び出されるメソッド。

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

`ConvertTime`メソッドに変換する`CMTime`オブジェクトを`TimeSpan`値。

### <a name="video-duration-in-android"></a>Android でのビデオの期間

`Duration` 、Android のプロパティ`VideoView`(ミリ秒単位) の有効期間を報告時に、`Prepared`のイベント`VideoView`が発生します。 Android`VideoPlayerRenderer`クラスでは、そのハンドラーを使用して、取得、`Duration`プロパティ。

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

### <a name="video-duration-in-uwp"></a>UWP でビデオの期間

`NaturalDuration`プロパティ`MediaElement`は、`TimeSpan`値になり、有効な場合に`MediaElement`発生、`MediaOpened`イベント。

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

## <a name="the-position-property"></a>位置プロパティ

`VideoPlayer` 必要があります、`Position`プロパティ値が 0 ~ 増加`Duration`ビデオの再生中にします。 `VideoPlayer` 同様に、このプロパティを実装、 `Position` 、UWP でプロパティ`MediaElement`、これは通常のバインド可能なプロパティのパブリック`set`と`get`アクセサー。

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

`get`アクセサーは、再生、ビデオの現在の位置を返しますが、`set`アクセサーが前後ビデオの位置に移動して、位置バーのユーザーの操作に応答する対象としています。

IOS および Android では、現在の位置を取得するプロパティにのみ、`get`アクセサー、および`Seek`メソッドは、この 2 つ目のタスクの実行に使用します。 個別であることを考慮する場合`Seek`メソッドは、1 つよりもより実用的なアプローチ`Position`プロパティです。 1 つ`Position`プロパティに固有の問題: ビデオの再生中、`Position`プロパティを新しい位置を反映する継続的に更新する必要があります。 ほとんどの変更をするようにすることが、`Position`ビデオ内の新しい位置に移動するビデオ プレーヤーが発生するプロパティです。 かどうかが発生すると、ビデオ プレーヤーでは、最後の値を取得することで応答、`Position`プロパティ、およびビデオを進めるはありません。

実装するの困難であるにもかかわらず、`Position`を持つプロパティ`set`と`get`アクセサー、UWP と一致しているために、この方法を選択した`MediaElement`、データ バインディングで、大きな利点があり、: `Position`プロパティ、`VideoPlayer`の新しい位置に位置を表示してシークの両方に使用される、スライダーをバインドできます。 ただし、予防策を講じるは、必要に応じてこれを実装するときに`Position`フィードバック ループを避けるためのプロパティです。

### <a name="setting-and-getting-ios-position"></a>設定または iOS の位置を取得します。

IOS、`CurrentTime`のプロパティ、`AVPlayerItem`オブジェクトは、ビデオの再生の現在位置を示します。 IOS`VideoPlayerRenderer`設定、`Position`プロパティに、`UpdateStatus`ハンドラーに設定する、同時に、`Duration`プロパティ。

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

レンダラーを検出したときに、`Position`プロパティから設定`VideoPlayer`で変更された、`OnElementPropertyChanged`を上書きして、その新しい値を使用して呼び出す、`Seek`メソッドを`AVPlayer`オブジェクト。

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

考慮するたびに、`Position`プロパティ`VideoPlayer`から設定されている、`OnUpdateStatus`ハンドラー、`Position`プロパティ発生、`PropertyChanged`で検出されるイベント、`OnElementPropertyChanged`をオーバーライドします。 これらの変更のほとんどの`OnElementPropertyChanged`メソッドには、何もしない必要があります。 それ以外の場合、ビデオの位置が変わるたびに、これに移動するだけに達すると同じ位置!

このフィードバック ループを回避する、`OnElementPropertyChanged`のみメソッドを呼び出して`Seek`ときの違い、`Position`プロパティとの現在の位置、`AVPlayer`が 1 秒より大きい。

### <a name="setting-and-getting-android-position"></a>設定または Android の位置を取得します。

Android、iOS レンダラーのように`VideoPlayerRenderer`の新しい値を設定、`Position`プロパティに、`OnUpdateStatus`ハンドラー。 `CurrentPosition`プロパティ`VideoView`単位で時間をミリ秒単位の新しい位置が含まれています。

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

また、iOS レンダラーの場合と同様に、Android のレンダラーでは、呼び出し、`SeekTo`メソッドの`VideoView`ときに、`Position`プロパティが変更が加えられた変更が 1 秒以上と異なる場合にのみ、`CurrentPosition`の値`VideoView`:

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

### <a name="setting-and-getting-uwp-position"></a>設定または UWP 位置を取得します。

UWP`VideoPlayerRenderer`ハンドル、 `Position` iOS および Android のレンダラーと同じようにするので、 `Position` UWP のプロパティ`MediaElement`も、`TimeSpan`値の変換は必要ありません。

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

## <a name="calculating-a-timetoend-property"></a>TimeToEnd プロパティを計算します。

場合によってビデオ プレーヤーは、ビデオの残りの期間を表示します。 この値は、ビデオの期間、ビデオの開始時とビデオの終了時に 0 まで減少で開始されます。

`VideoPlayer` 読み取り専用が含まれています`TimeToEnd`クラス内に完全に処理されるプロパティが変更に基づき、`Duration`と`Position`プロパティ。

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

`SetTimeToEnd`両方のプロパティ変更ハンドラーからメソッドを呼び出した`Duration`と`Position`です。

## <a name="a-custom-slider-for-video"></a>ビデオのカスタム スライダー

位置バーのカスタム コントロールを作成するか、Xamarin.Forms を使用する可能性が`Slider`から派生したクラスまたは`Slider`、次のよう`PositionSlider`クラスです。 クラスはという 2 つの新しいプロパティを定義`Duration`と`Position`型の`TimeSpan`をするデータ バインドで同じ名前の 2 つのプロパティには、`VideoPlayer`です。 既定のモードのバインディングに注意してください、`Position`プロパティが双方向。

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

プロパティ変更のハンドラーを`Duration`プロパティ セット、 `Maximum` 、基になるプロパティ`Slider`を`TotalSeconds`のプロパティ、`TimeSpan`値。 同様に、プロパティ変更のハンドラーは、`Position`設定、`Value`のプロパティ、`Slider`です。 これにより、基になる`Slider`の位置を追跡、`PositionSlider`です。

`PositionSlider`は、基になるから更新`Slider`インスタンス 1 つだけで: ユーザーが操作するときに、`Slider`ビデオを高度なするか、新しい位置に元に戻すことを示すためにします。 これは、検出で、`PropertyChanged`のコンス トラクターでハンドラー、`PositionSlider`です。 ハンドラーでの変化を確認、`Value`プロパティとは異なる場合、`Position`プロパティ、`Position`プロパティが設定されてから、`Value`プロパティです。

理論上は、内部`if`ステートメントは、次のように記述できます。

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

ただしの Android 実装`Slider`に関係なく 1,000 件を超える個別の手順があります、`Minimum`と`Maximum`設定します。 ビデオの長さは 1,000 (秒単位) を 2 つの異なる超えた`Position`値と同じに対応して`Value`の設定、 `Slider`、され、この`if`偽陽性のユーザー操作のステートメントをトリガー`Slider`です。 代わりに、新しい位置と既存の位置が全体の期間の 1/100 より大きいことを確認することができます。

## <a name="using-the-positionslider"></a>使用して、PositionSlider

UWP について[ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)へのバインドに関する注意事項について、`Position`プロパティ、プロパティが頻繁に更新されるためです。 ドキュメントでは、タイマーを使用することをお勧めするクエリ、`Position`プロパティです。

3 つが、適切な推奨事項`VideoPlayerRenderer`クラスは、既に間接的に使用して、タイマーを更新する、`Position`プロパティです。 `Position`のハンドラーのプロパティが変更された、`UpdateStatus`秒間に 10 回だけ発生するイベントです。

したがって、`Position`のプロパティ、`VideoPlayer`にバインドすることができます、`Position`のプロパティ、`PositionSlider`せずで示したように、パフォーマンスの問題、**カスタム位置バー**ページ。

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

最初の省略記号ボタン (···) を非表示に、 `ActivityIndicator`; は、前のものと同じ**カスタム トランスポート**ページ。 2 つのことを確認`Label`要素を表示する、`Position`と`TimeToEnd`プロパティです。 これら 2 つの間で、省略記号`Label`要素には、2 つが非表示に`Button`に表示される要素、**カスタム トランスポート**ページの再生、一時停止、および停止します。 分離コード ロジックと同じではまた、**カスタム トランスポート**ページ。

[![カスタム配置](custom-positioning-images/custompositioning-small.png "カスタム配置")](custom-positioning-images/custompositioning-large.png#lightbox "カスタム配置")

説明は、この時点で、`VideoPlayer`です。

## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
