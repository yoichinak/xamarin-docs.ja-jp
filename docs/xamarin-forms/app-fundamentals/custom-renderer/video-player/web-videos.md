---
title: Web ビデオを再生します。
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 322ae03fc813d180a6678f63b04488a17705523d
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="playing-a-web-video"></a>Web ビデオを再生します。

`VideoPlayer`クラスを定義、 `Source` 、ビデオ ファイルのソースを指定するために使用するプロパティだけでなく、`AutoPlay`プロパティです。 `AutoPlay` 既定の設定を持つ`true`、ビデオが後に自動的に再生を開始することを意味する`Source`が設定されています。

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }
        
        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

`Source`プロパティの型は`VideoSource`、Xamarin.Forms 後でパターン化されている[ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/)クラス、およびその 3 つの派生クラスを抽象化[ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/)、 [`FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/)、および[ `StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/)です。 使用できるストリームのオプションがない、`VideoPlayer`ただし、iOS および Android をサポートしないため、ストリームからビデオを再生します。

## <a name="video-sources"></a>ビデオ ソース

抽象`VideoSource`クラスのみで構成される 3 つの静的メソッドから派生した 3 つのクラスをインスタンス化を`VideoSource`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

`UriVideoSource`の URI でダウンロード可能なビデオ ファイルを指定するクラスを使用します。 型の 1 つのプロパティを定義`string`:

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

型のオブジェクトを処理`UriVideoSource`次のとおりです。

`ResourceVideoSource`も指定されて、プラットフォームのアプリケーション内の埋め込みリソースとして格納されているビデオ ファイルにアクセスするクラスが使用される、`string`プロパティ。

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

型のオブジェクトを処理`ResourceVideoSource`、資料に記載されて[アプリケーション リソースのビデオの読み込み](loading-resources.md)です。 `VideoPlayer`クラスには、標準の .NET ライブラリのリソースとして保存されたビデオ ファイルを読み込むための機能はありません。

`FileVideoSource`クラスは、デバイスのビデオ ライブラリからのビデオ ファイルのアクセスに使用します。 1 つのプロパティは型も`string`:

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

型のオブジェクトを処理`FileVideoSource`、資料に記載されて[、デバイスのビデオ ライブラリへのアクセス](accessing-library.md)です。

`VideoSource`クラスが含まれています、`TypeConverter`を参照する属性`VideoSourceConverter`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

この型コンバーターが呼び出されるときに、`Source`プロパティが XAML 内の文字列に設定します。 ここでは、`VideoSourceConverter`クラス。

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ? 
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

`ConvertFromInvariantString`メソッドが、対象の文字列を変換しようとしています。、`Uri`オブジェクト。 成功すると、スキームがない場合`file:`、メソッドから返されます、`UriVideoSource`です。 返しますそれ以外の場合、`ResourceVideoSource`です。

## <a name="setting-the-video-source"></a>ビデオ ソースを設定します。

ビデオ ソースに関連するその他のすべてのロジックは、個々 のプラットフォームのレンダラーに実装されます。 次のセクションでは、プラットフォームのレンダラーでビデオを再生する方法を表示するときに、`Source`プロパティに設定されている、`UriVideoSource`オブジェクト。

### <a name="the-ios-video-source"></a>IOS のビデオ ソース

2 つのセクション、`VideoPlayerRenderer`ビデオ プレーヤーのビデオ ソースを設定するのには必要です。 型のオブジェクトが最初に作成 Xamarin.Forms とき`VideoPlayer`、`OnElementChanged`メソッドが呼び出された、`NewElement`引数オブジェクトのプロパティを設定する`VideoPlayer`です。 `OnElementChanged`メソッド呼び出し`SetSource`:

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
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

後で、ときに、`Source`プロパティが変更された、`OnElementPropertyChanged`メソッドが呼び出された、 `PropertyName` "Source"のプロパティと`SetSource`が再度呼び出されます。

IOS、型のオブジェクトでのビデオ ファイルを再生する[ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/)をカプセル化、ビデオ ファイルを最初に作成されたの作成に使用して、 [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/)、これに渡す、 `AVPlayer`オブジェクト。 ここでは、方法、`SetSource`メソッド ハンドル、`Source`プロパティ型であるときに`UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

`AutoPlay`プロパティを持たないアナログ iOS ビデオ クラスでの最後に、プロパティを調べるので、`SetSource`メソッドを呼び出す、`Play`メソッドを`AVPlayer`オブジェクト。

ページの後の再生がビデオ場合によっては、引き続き、`VideoPlayer`ホーム ページに移動します。 ビデオを停止する、`ReplaceCurrentItemWithPlayerItem`で設定されても、`Dispose`オーバーライドします。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Android のビデオ ソース

Android`VideoPlayerRenderer`プレーヤーのビデオ ソースを設定する必要があるときに、`VideoPlayer`が最初に作成され、後でいつ、`Source`プロパティの変更。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

`SetSource`メソッドの型のオブジェクトを処理する`UriVideoSource`を呼び出して`SetVideoUri`上、 `VideoView` 、Android と`Uri`文字列 URI から作成されたオブジェクト。 `Uri`クラスは、完全修飾ここで、.NET から区別するために`Uri`クラス。

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Android`VideoView`対応がない`AutoPlay`プロパティのため、`Start`新しいビデオが設定されている場合、メソッドが呼び出されます。

IOS の動作と Android のレンダラーの違いがある場合、`Source`プロパティの`VideoPlayer`に設定されている`null`、または、`Uri`プロパティの`UriVideoSource`に設定されている`null`または空の文字列。 IOS ビデオ プレーヤーはビデオについては、現在再生中の場合と`Source`に設定されている`null`(または文字列が`null`または空白)、`ReplaceCurrentItemWithPlayerItem`で呼び出された`null`値。 現在のビデオは置き換えられ、再生を停止します。

Android では、同様の機能をサポートしていません。 場合、`Source`プロパティに設定されている`null`、`SetSource`メソッドだけでは無視されます、および現在のビデオの再生を続行します。

### <a name="the-uwp-video-source"></a>UWP ビデオ ソース

UWP`MediaElement`定義、`AutoPlay`プロパティで、他のプロパティなどのレンダラーで処理されます。

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
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

`SetSource`プロパティ ハンドル、`UriVideoSource`オブジェクトを設定して、`Source`プロパティの`MediaElement`.net`Uri`値、または`null`場合、`Source`プロパティの`VideoPlayer`に設定されている`null`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>URL のソースを設定します。

3 つのレンダラーでこれらのプロパティの実装では、URL ソースからビデオを再生することはできます。 **Web ビデオの再生** ページで、 [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md)プログラムが次の XAML ファイルで定義されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter`クラスを変換する文字列、`UriVideoSource`です。 移動すると、 **Web ビデオの再生** ページで、ビデオの開始と読み込みのための十分な量のデータをダウンロードし、バッファー内のときに、再生を開始します。 ビデオは、約 10 分の長さを示します。

[![Web のビデオの再生](web-videos-images/playwebvideo-small.png "Web ビデオの再生")](web-videos-images/playwebvideo-large.png#lightbox "Web ビデオの再生")

各 3 つのプラットフォームで、トランスポート コントロールをフェードアウトする場合は、使用されていないが、ビデオをタップして表示するのには復元できます。

設定を自動的に開始からビデオを防ぐことができます、`AutoPlay`プロパティを`false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

キーを押す必要があります、**再生**ビデオを開始するボタンをクリックします。

同様に、トランスポート コントロールの表示を抑制を設定して、`AreTransportControlsEnabled`プロパティを`false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

両方のプロパティを設定した場合`false`ビデオの再生を開始しないおよびは行われませんを起動する方法です。 呼び出す必要がある`Play`分離コード ファイル、またはアーティクルの説明に従って、独自のトランスポート コントロールを作成する[カスタム ビデオ トランスポート コントロールを実装する](custom-transport.md)です。 

**App.xaml**ファイルには、追加のビデオを 2 つのリソースが含まれています。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />
            
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

これらの他のビデオのいずれかの参照 を明示的な URL を置き換えることができます、 **PlayWebVideo.xaml**ファイルと、`StaticResource`マークアップ拡張機能の場合、`VideoSourceConverter`を作成する必要はありません、`UriVideoSource`オブジェクト。

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

また、設定することができます、`Source`でビデオ ファイルのプロパティ、`ListView`次の記事で説明されている[プレーヤーにビデオ ソースをバインド](source-bindings.md)です。





## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
