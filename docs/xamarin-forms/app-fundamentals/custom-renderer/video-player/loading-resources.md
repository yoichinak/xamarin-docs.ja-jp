---
title: "アプリケーション リソースのビデオの読み込み"
ms.topic: article
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 08d07a82651887c9d87b908acd82296a3d80e43f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="loading-application-resource-videos"></a>アプリケーション リソースのビデオの読み込み

カスタム レンダラー、`VideoPlayer`ビューでも、アプリケーションのリソースとして個々 のプラットフォームのプロジェクトに埋め込まれたビデオ ファイルを再生します。 ただし、現在のバージョンの`VideoPlayer`ポータブル クラス ライブラリに埋め込まれたリソースにアクセスできません。

これらのリソースを読み込むには、インスタンスを作成`ResourceVideoSource`を設定して、`Path`にファイル名 (または、フォルダーとファイル名)、リソースのプロパティです。 また、静的なを呼び出すことができます`VideoSource.FromResource`メソッドからリソースを参照します。 次に、設定、`ResourceVideoSource`オブジェクトを`Source`プロパティ`VideoPlayer`です。 

## <a name="storing-the-video-files"></a>ビデオ ファイルを格納します。

プラットフォームのプロジェクトでビデオ ファイルを保存することは、3 つのプラットフォームの異なるは。

### <a name="ios-video-resources"></a>iOS のビデオ リソース

IOS プロジェクトでビデオを保存することができます、**リソース**フォルダー、またはのサブフォルダー、**リソース**フォルダーです。 ビデオ ファイルが必要な`Build Action`の`BundleResource`します。 設定、`Path`プロパティ`ResourceVideoSource`のファイル名に、たとえば、 **MyFile.mp4**でのファイル、**リソース**フォルダー、または**MyFolder/MyFile.mp4**、ここで**MyFolder**のサブフォルダーになって**リソース**です。

**VideoPlayerDemos**ソリューションでは、 **VideoPlayerDemos.iOS**プロジェクトには、サブフォルダーが含まれている**リソース**という**ビデオ**という名前のファイルを含む**iOSApiVideo.mp4**です。 これは、iOS 用のドキュメントを検索する Xamarin の web サイトを使用する方法を説明する短いビデオ`AVPlayerViewController`クラスです。

### <a name="android-video-resources"></a>Android のビデオ リソース

Android プロジェクトでは、ビデオのサブフォルダーに格納する必要があります**リソース**という**生**です。 **生**フォルダーのサブフォルダーを含めることはできません。 ビデオ ファイルを付け、`Build Action`の`AndroidResource`します。 設定、`Path`プロパティ`ResourceVideoSource`のファイル名に、たとえば、 **MyFile.mp4**です。 

**VideoPlayerDemos.Android**プロジェクトには、サブフォルダーが含まれている**リソース**という**生**、という名前のファイルが含まれています**AndroidApiVideo.mp4**. 

### <a name="uwp-video-resources"></a>UWP ビデオ リソース

ユニバーサル Windows プラットフォームのプロジェクトでは、任意のフォルダー、プロジェクト内でのビデオを格納できます。 ファイルに付けます、`Build Action`の`Content`します。 設定、`Path`プロパティ`ResourceVideoSource`フォルダーとファイル名、たとえば、 **MyFolder/MyVideo.mp4**です。 

**VideoPlayerDemos.UWP**という名前のフォルダーがプロジェクトに含まれている**ビデオ**ファイルと共に**UWPApiVideo.mp4**です。

## <a name="loading-the-video-files"></a>ビデオ ファイルを読み込んでいます

コード内の各プラットフォームのレンダラー クラスが含まれています、`SetSource`リソースとして格納されているビデオ ファイルを読み込むためのメソッドです。

### <a name="ios-resource-loading"></a>iOS リソースの読み込み

IOS のバージョンの`VideoPlayerRenderer`を使用して、`GetUrlForResource`メソッドの`NSBundle`リソースを読み込むためです。 完全なパスは、ファイル名、拡張子、およびディレクトリに分割する必要があります。 コードを使用して、 `Path` 、.net クラス`System.IO`ファイルのパスをこれらのコンポーネントに分割するための名前空間。

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Android のリソースの読み込み

Android`VideoPlayerRenderer`構築するために、ファイル名とパッケージ名を使用して、`Uri`オブジェクト。 パッケージの名前をここでは、アプリケーションの名前が**VideoPlayerDemos.Android**、静的なから取得できる`Context.PackageName`プロパティです。 結果として得られる`Uri`にオブジェクトが渡され、`SetVideoURI`メソッドの`VideoView`:

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
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>UWP リソースの読み込み

UWP`VideoPlayerRenderer`構築、`Uri`パスのオブジェクト設定し、`Source`プロパティ`MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>リソース ファイルの再生

**ビデオ リソースの再生** ページで、 **VideoPlayerDemos**ソリューションで使用、`OnPlatform`クラスを各プラットフォームのビデオ ファイルを指定します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

IOS リソースが格納されている場合、**リソース**フォルダー、UWP リソースがプロジェクトのルート フォルダーに格納されている場合は、3 つのプラットフォームの同じファイル名を使用できます。 その場合は場合に直接その名前を設定することができます、`Source`プロパティの`VideoPlayer`します。 

3 つのプラットフォームで実行されているそのページを次に示します。

[![リソースのビデオを再生](loading-resources-images/playvideoresource-small.png "ビデオ リソースを再生")](loading-resources-images/playvideoresource-large.png#lightbox "リソースのビデオの再生")

見たようになりました方法[Web URI からビデオを読み込む](web-videos.md)と埋め込みリソースを再生する方法です。 さらを実行できます[、デバイスのビデオ ライブラリからビデオを読み込む](accessing-library.md)します。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
