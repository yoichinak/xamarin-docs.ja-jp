---
title: Web ビデオの再生
description: この記事では、Xamarin.Forms を使ってビデオ プレーヤー アプリケーションで Web ビデオを再生する方法について説明します。
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 9beff615c39fc34b5a58a93d309bb20543cad77f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650418"
---
# <a name="playing-a-web-video"></a>Web ビデオの再生

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

`VideoPlayer` クラスでは、`AutoPlay` プロパティと共に、ビデオ ファイルのソースの指定に使用される `Source` プロパティが定義されます。 `AutoPlay` の既定の設定は `true` です。これは、`Source` の設定後、自動的にビデオの再生が開始されることを意味します。

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

`Source` プロパティの型は `VideoSource` です。これは、Xamarin.Forms の [`ImageSource`](xref:Xamarin.Forms.ImageSource) 抽象クラスとその 3 つの派生クラス [`UriImageSource`](xref:Xamarin.Forms.UriImageSource)、[`FileImageSource`](xref:Xamarin.Forms.FileImageSource)、および [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) の後に形成されます。 ただし、iOS と Android ではストリームからのビデオ再生をサポートしていないため、`VideoPlayer` のストリーム オプションを使用できません。

## <a name="video-sources"></a>ビデオ ソース

抽象クラス `VideoSource` は、`VideoSource` から派生する 3 つのクラスをインスタンス化する 3 つの静的メソッドのみで構成されます。

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

`UriVideoSource` クラスは URI でダウンロード可能なビデオ ファイルを指定するために使用します。 `string` 型の単一プロパティが定義されます。

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

`UriVideoSource` 型のオブジェクトの処理については、以下で説明します。

`ResourceVideoSource` クラスは、プラットフォーム アプリケーション内の埋め込みリソースとして格納されているビデオ ファイルにアクセスするために使用します。これは `string` プロパティでも指定されています。

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

`ResourceVideoSource` 型のオブジェクトの処理については、「[Loading Application Resource Videos](loading-resources.md)」(アプリケーション リソース ビデオの読み込み) の記事で説明しています。 `VideoPlayer` クラスには、.NET Standard ライブラリのリソースとして格納されているビデオ ファイルを読み込むための機能がありません。

`FileVideoSource` クラスは、デバイスのビデオ ライブラリからビデオ ファイルにアクセスするために使用します。 この単一プロパティも `string` 型です。

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

`FileVideoSource` 型のオブジェクトの処理については、「[Accessing the Device's Video Library](accessing-library.md)」(デバイスのビデオ ライブラリへのアクセス) の記事で説明しています。

`VideoSource` クラスには、`VideoSourceConverter` を参照する `TypeConverter` 属性が含まれています。

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

この型コンバーターは、XAML の文字列に `Source` プロパティが設定されると起動します。 `VideoSourceConverter` クラスは次のようになります。

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

`ConvertFromInvariantString` メソッドが文字列を `Uri` オブジェクトに変換しようとします。 これが成功し、スキームが `file:` でない場合は、メソッドが `UriVideoSource` を返します。 それ以外の場合は `ResourceVideoSource` を返します。

## <a name="setting-the-video-source"></a>ビデオ ソースの設定

ビデオ ソースに関連するその他のすべてのロジックは、個別のプラットフォーム レンダラーに実装されます。 次のセクションでは、`Source` プロパティが `UriVideoSource` オブジェクトに設定されているときにプラットフォーム レンダラーがどのようにビデオを再生するかを示します。

### <a name="the-ios-video-source"></a>iOS のビデオ ソース

ビデオ プレーヤーのビデオ ソースの設定には、2 つのセクションの`VideoPlayerRenderer` がともないます。 Xamarin.Forms で最初に `VideoPlayer` 型のオブジェクトが作成されたときに、その `VideoPlayer` に設定された引数オブジェクトの `NewElement` プロパティで `OnElementChanged` メソッドが呼び出されます。 `OnElementChanged` メソッドは `SetSource` を呼び出します。

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

その後、`Source` プロパティが変更されると、`OnElementPropertyChanged` メソッドが "Source" の `PropertyName` プロパティで呼び出され、`SetSource` が再度呼び出されます。

iOS でビデオ ファイルを再生するには、最初にビデオ ファイルをカプセル化するために [`AVAsset`](xref:AVFoundation.AVAsset) 型のオブジェクトが作成され、これを使用して [`AVPlayerItem`](xref:AVFoundation.AVPlayerItem) が作成され、`AVPlayer` オブジェクトに渡されます。 `SetSource` メソッドの型が `UriVideoSource` である場合の `Source` プロパティの処理方法を示します。

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

`AutoPlay` プロパティには iOS ビデオ クラスのアナログがないため、`AVPlayer` オブジェクト上で `Play` メソッドを呼び出すために `SetSource` メソッドの最後にプロパティが調べられます。

場合によっては、`VideoPlayer` が含まれるページからホーム ページに戻った後もビデオの再生が続行します。 ビデオを停止するため、`Dispose` のオーバーライドに `ReplaceCurrentItemWithPlayerItem` も設定します。

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

Android の `VideoPlayerRenderer` は、`VideoPlayer` が最初に作成され、後で `Source` プロパティが変更されたときに、プレーヤーのビデオ ソースを設定する必要があります。

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

`SetSource` メソッドは文字列 URI から作成された Android の `Uri` オブジェクトを含む `VideoView` 上で `SetVideoUri` を呼び出して、`UriVideoSource` 型のオブジェクトを処理します。 `Uri` クラスは .NET の `Uri` クラスと区別するためにここで完全修飾されます。

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

Android の `VideoView` には対応する `AutoPlay` プロパティがないため、新しいビデオが設定されている場合には `Start` メソッドが呼び出されます。

`VideoPlayer` の `Source` プロパティが `null` に設定されている場合、または `UriVideoSource` の `Uri` プロパティが `null` もしくは空の文字列に設定されている場合、iOS と Android でレンダラーの動作が異なります。 iOS ビデオ プレーヤーでビデオを再生していて、`Source` が `null` に設定されている (または文字列が `null` もしくは空白である) 場合は、`ReplaceCurrentItemWithPlayerItem` が `null` の値で呼び出されます。 現在のビデオが置き換えられ、再生が停止します。

Android では、同様の機能をサポートしていません。 `Source` プロパティが `null` に設定されている場合、`SetSource` メソッドは単にそれを無視し、現在のビデオの再生が続行します。

### <a name="the-uwp-video-source"></a>UWP のビデオ ソース

UWP の `MediaElement` では、他のすべてのプロパティと同様にレンダラーで処理される `AutoPlay` プロパティが定義されます。

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

`SetSource` プロパティは `VideoPlayer` の `Source` プロパティが `null` に設定されている場合、`MediaElement` の `Source` プロパティを .NET の `Uri` 値または `null` に設定して `UriVideoSource` オブジェクトを処理します。

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

## <a name="setting-a-url-source"></a>URL のソースの設定

3 つのレンダラーでこれらのプロパティを実装すると、URL のソースからビデオを再生することができます。 [**VideoPlayDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) プログラムの「**Play Web Video**」(Web ビデオを再生する) ページは次の XAML ファイルによって定義されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` クラスは、文字列を `UriVideoSource` に変換します。 「**Play Web Video**」(Web ビデオを再生する) ページに移動すると、十分な量のデータがダウンロードされバッファされた時点でビデオの読み込みと再生が開始します。 このビデオの長さは、約 10 分です。

[![Web ビデオを再生する](web-videos-images/playwebvideo-small.png "Web ビデオを再生する")](web-videos-images/playwebvideo-large.png#lightbox "Web ビデオを再生する")

各プラットフォームで、トランスポート コントロールが使用されていない場合はフェード アウトしますが、ビデオをタップすることで表示を復元することができます。

`AutoPlay` プロパティを `false` に設定すると、ビデオが自動的に開始することを防ぐことができます。

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

ビデオを開始するには、 **[再生]** ボタンを押す必要があります。

同様に、`AreTransportControlsEnabled` プロパティを `false` に設定すると、トランスポート コントロールを非表示にすることができます。

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

両方のプロパティを `false` に設定すると、ビデオの再生が開始せず、また開始することができなくなります。 分離コード ファイルから `Play` を呼び出すか、[カスタムのビデオ トランスポート コントロールの実装](custom-transport.md)に関する記事の説明に従って、独自のトランスポート コントロールを作成する必要があります。

**App.xaml** ファイルには追加の 2 つのビデオのリソースが含まれています。

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

これらの他のムービーのいずれかを参照するには、**PlayWebVideo.xaml** ファイルの明示的な URL を `StaticResource` マークアップ拡張に置き換えます。この場合、`UriVideoSource` オブジェクトを作成するための `VideoSourceConverter` は不要です。

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

または、次の記事「[プレーヤーへのビデオ ソースのバインド](source-bindings.md)」の説明に従って、`ListView` のビデオ ファイルから `Source` プロパティを設定することもできます。





## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
