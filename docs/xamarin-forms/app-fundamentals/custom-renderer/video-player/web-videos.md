---
title: Web ビデオを再生します。
description: この記事では、Xamarin.Forms を使用して、ビデオ プレーヤー アプリケーションで web ビデオを再生する方法について説明します。
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 566f056bd616c918ce274b9c7406d94fdc265ea2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994560"
---
# <a name="playing-a-web-video"></a>Web ビデオを再生します。

`VideoPlayer`クラスを定義、 `Source` 、ビデオ ファイルのソースを指定するために使用するプロパティだけでなく、`AutoPlay`プロパティ。 `AutoPlay` 既定の設定が`true`、ビデオが後に自動的に再生を開始することを意味する`Source`が設定されています。

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

`Source`プロパティの型は`VideoSource`、Xamarin.Forms にならって作成されるされて[ `ImageSource` ](xref:Xamarin.Forms.ImageSource)抽象クラスとその 3 つの派生物[ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource)、 [`FileImageSource` ](xref:Xamarin.Forms.FileImageSource)、および[ `StreamImageSource`](xref:Xamarin.Forms.StreamImageSource)します。 使用できるストリームのオプションがない、`VideoPlayer`ただし、iOS および Android をサポートしないため、ストリームからのビデオを再生します。

## <a name="video-sources"></a>ビデオのソース

抽象`VideoSource`だけから派生する 3 つのクラスをインスタンス化する 3 つの静的メソッドのクラスで構成されます`VideoSource`:

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

`UriVideoSource`クラスを使用して、URI を持つ、ダウンロード可能なビデオ ファイルを指定します。 型の 1 つのプロパティを定義`string`:

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

型のオブジェクトを処理`UriVideoSource`以下に示します。

`ResourceVideoSource`も指定されているプラットフォーム アプリケーション内の埋め込みリソースとして格納されているビデオ ファイルにアクセスするクラスが使用される、`string`プロパティ。

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

型のオブジェクトを処理`ResourceVideoSource`、資料に記載されて[アプリケーション リソース ビデオの読み込み](loading-resources.md)します。 `VideoPlayer`クラスには、.NET Standard ライブラリにリソースとして格納されているビデオ ファイルを読み込むための機能がありません。

`FileVideoSource`クラスは、デバイスのビデオ ライブラリからビデオ ファイルへのアクセスに使用されます。 1 つのプロパティは型も`string`:

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

型のオブジェクトを処理`FileVideoSource`、資料に記載されて[デバイスのビデオ ライブラリへのアクセス](accessing-library.md)します。

`VideoSource`クラスが含まれています、`TypeConverter`参照属性`VideoSourceConverter`:

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

型コンバーターが呼び出されるときに、 `Source` XAML の文字列にプロパティを設定します。 ここでは、`VideoSourceConverter`クラス。

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

`ConvertFromInvariantString`メソッドは、文字列に変換しようとしています、`Uri`オブジェクト。 成功して、スキームが場合`file:`、メソッドから返されます、`UriVideoSource`します。 それ以外を返します、`ResourceVideoSource`します。

## <a name="setting-the-video-source"></a>ビデオのソースを設定します。

ビデオ ソースに関連するその他のすべてのロジックは、個別のプラットフォームのレンダラーに実装されます。 次のセクションでは、プラットフォームのレンダラーがビデオを再生する方法を表示するときに、`Source`プロパティに設定されて、`UriVideoSource`オブジェクト。

### <a name="the-ios-video-source"></a>IOS のビデオ ソース

2 つのセクション、`VideoPlayerRenderer`ビデオ プレーヤーのビデオ ソースを設定するには。 最初の Xamarin.Forms の型のオブジェクトの作成と`VideoPlayer`、`OnElementChanged`メソッドを呼び出すと、`NewElement`引数オブジェクトのプロパティを設定する`VideoPlayer`します。 `OnElementChanged`メソッド呼び出し`SetSource`:

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

後で、ときに、`Source`プロパティが変更された、`OnElementPropertyChanged`メソッドを呼び出すと、 `PropertyName` "Source"のプロパティと`SetSource`が再度呼び出されます。

IOS では、型のオブジェクトでビデオ ファイルを再生する[ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/)が最初に、ビデオ ファイルをカプセル化する作成を作成するために使用する、 [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/)、に渡しますが、`AVPlayer`オブジェクト。 ここでは、方法、`SetSource`メソッド ハンドル、`Source`プロパティ型であるときに`UriVideoSource`:

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

`AutoPlay`プロパティを持たないアナログ iOS ビデオ クラスでの最後に、プロパティが調べられるように、`SetSource`メソッドを呼び出す、`Play`メソッドを`AVPlayer`オブジェクト。

ページの後に再生がビデオでは、場合によっては、続き、`VideoPlayer`ホーム ページに戻ります。 ビデオを停止する、`ReplaceCurrentItemWithPlayerItem`設定されても、`Dispose`オーバーライドします。

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

Android`VideoPlayerRenderer`プレーヤーのビデオ ソースを設定する必要がときに、`VideoPlayer`が最初に作成され以降と、`Source`プロパティの変更。

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

`SetSource`メソッドは型のオブジェクトを処理`UriVideoSource`呼び出して`SetVideoUri`上、 `VideoView` Android で`Uri`文字列 URI から作成されたオブジェクト。 `Uri` .NET から区別するためにクラスがここで完全に修飾された`Uri`クラス。

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

Android`VideoView`しないのは、対応する`AutoPlay`プロパティ、そのため、`Start`新しい動画が設定されている場合、メソッドが呼び出されます。

IOS の動作と Android のレンダラーの違いがある場合、`Source`プロパティの`VideoPlayer`に設定されている`null`、または、`Uri`プロパティの`UriVideoSource`に設定されている`null`または空の文字列。 IOS ビデオ プレーヤーはビデオについては、現在再生中の場合と`Source`に設定されている`null`(または文字列が`null`または空白)、`ReplaceCurrentItemWithPlayerItem`を使用して呼び出した`null`値。 現在のビデオが置き換えられ、再生を停止します。

Android は、同様の機能をサポートしていません。 場合、`Source`プロパティに設定されて`null`、`SetSource`メソッドは単にそれを無視し、現在のビデオの再生を続行します。

### <a name="the-uwp-video-source"></a>UWP のビデオ ソース

UWP`MediaElement`定義、`AutoPlay`プロパティで、他のプロパティと同様のレンダラーで処理されます。

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

次の 3 つのレンダラーでこれらのプロパティの実装は、URL のソースからビデオを再生できます。 **Web ビデオの再生** ページで、 [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md)プログラムが次の XAML ファイルで定義されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter`に変換する文字列のクラス、`UriVideoSource`します。 移動すると、 **Web ビデオの再生** ページで、ビデオは、読み込みとデータのための十分な量をダウンロードし、バッファーの再生開始を開始します。 このビデオでは、約 10 分の長さです。

[![Web ビデオを再生する](web-videos-images/playwebvideo-small.png "Web ビデオを再生する")](web-videos-images/playwebvideo-large.png#lightbox "Web ビデオを再生します。")

各 3 つのプラットフォームで、トランスポート コントロールをフェードアウトする場合、これらの使用に慣れていないが、ビデオをタップして表示状態に復元できます。

ビデオを防ぐ設定を自動的に開始することができます、`AutoPlay`プロパティを`false`:

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

両方のプロパティを設定した場合`false`ビデオの再生を開始しないし、それを開始することはありません。 呼び出すことになる`Play`分離コード ファイル、または、記事の説明に従って、独自のトランスポート コントロールを作成する[を実装するカスタムのビデオ トランスポート コントロール](custom-transport.md)します。

**App.xaml**ファイルに追加のビデオを 2 つのリソースが含まれています。

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

これらの他の作品のいずれかの参照にで明示的な URL を置き換えることができます、 **PlayWebVideo.xaml**ファイルと、`StaticResource`マークアップ拡張機能の場合、`VideoSourceConverter`を作成する必要はありません、`UriVideoSource`オブジェクト。

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

または、設定、`Source`でビデオ ファイルからプロパティを`ListView`」の説明に従って、次の記事[プレーヤーへのビデオのソースをバインド](source-bindings.md)。





## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
