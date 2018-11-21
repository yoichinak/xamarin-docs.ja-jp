---
title: アプリケーション リソース ビデオの読み込み
description: この記事では、Xamarin.Forms を使用してビデオ プレーヤー アプリケーションでは、アプリケーション リソースとして格納されているビデオを読み込む方法について説明します。
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 17e9e7061e4329431a0f34abdbbb616a1aff1b43
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171327"
---
# <a name="loading-application-resource-videos"></a>アプリケーション リソース ビデオの読み込み

カスタム レンダラー、`VideoPlayer`ビューは、アプリケーション リソースとして個別のプラットフォーム プロジェクトに埋め込まれたビデオ ファイルを再生します。 ただし、現在のバージョンの`VideoPlayer`.NET Standard ライブラリに埋め込まれているリソースにアクセスできません。

これらのリソースを読み込むには、インスタンスを作成`ResourceVideoSource`を設定して、`Path`にファイル名 (または、フォルダーまたはファイル名)、リソースのプロパティ。 また、静的なを呼び出すことができます`VideoSource.FromResource`リソースを参照するメソッド。 次に、設定、`ResourceVideoSource`オブジェクトを`Source`プロパティの`VideoPlayer`します。

## <a name="storing-the-video-files"></a>ビデオ ファイルを保存します。

プラットフォーム プロジェクトにビデオ ファイルを格納するは、プラットフォームごとに異なります。

### <a name="ios-video-resources"></a>iOS のビデオ リソース

IOS プロジェクトでビデオを保存することができます、**リソース**フォルダー、またはのサブフォルダー、**リソース**フォルダー。 ビデオ ファイルが必要、`Build Action`の`BundleResource`します。 設定、`Path`プロパティの`ResourceVideoSource`をファイル名、たとえば、 **MyFile.mp4**内のファイル、**リソース**フォルダー、または**MyFolder/MyFile.mp4**、場所**MyFolder**のサブフォルダー**リソース**します。

**VideoPlayerDemos**ソリューション、 **VideoPlayerDemos.iOS**プロジェクトにはサブフォルダーが含まれています**リソース**という**ビデオ**という名前のファイルを含む**iOSApiVideo.mp4**します。 これは、Xamarin の web サイトを使用して、iOS 用のドキュメントを検索する方法を示した短いビデオを`AVPlayerViewController`クラス。

### <a name="android-video-resources"></a>Android のビデオ リソース

Android プロジェクトでは、ビデオのサブフォルダーに格納する必要があります**リソース**という**生**します。 **生**フォルダーのサブフォルダーを含めることはできません。 ビデオ ファイル、`Build Action`の`AndroidResource`します。 設定、`Path`プロパティの`ResourceVideoSource`をファイル名、たとえば、 **MyFile.mp4**します。

**VideoPlayerDemos.Android**プロジェクトのサブフォルダーに含まれる**リソース**という**生**、という名前のファイルを含む**AndroidApiVideo.mp4**.

### <a name="uwp-video-resources"></a>UWP のビデオ リソース

ユニバーサル Windows プラットフォーム プロジェクトでは、任意のフォルダー、プロジェクト内でのビデオを格納できます。 ファイルに付ける、`Build Action`の`Content`します。 設定、`Path`プロパティの`ResourceVideoSource`フォルダーとファイル名、たとえば、 **MyFolder/MyVideo.mp4**します。

**VideoPlayerDemos.UWP**という名前のフォルダーがプロジェクトに含まれている**ビデオ**ファイル**UWPApiVideo.mp4**します。

## <a name="loading-the-video-files"></a>ビデオ ファイルの読み込み

コード内の各プラットフォームのレンダラー クラスが含まれています、`SetSource`リソースとして格納されているビデオ ファイルを読み込むためのメソッド。

### <a name="ios-resource-loading"></a>iOS のリソースの読み込み

IOS のバージョンの`VideoPlayerRenderer`を使用して、`GetUrlForResource`メソッドの`NSBundle`リソースを読み込むためです。 完全なパスは、filename、extension、およびディレクトリに分割する必要があります。 コードを使用して、 `Path` 、.net クラス`System.IO`ファイルのパスをこれらのコンポーネントに分割するための名前空間。

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

Android `VideoPlayerRenderer` 、ファイル名とパッケージ名を使用して構築、`Uri`オブジェクト。 パッケージ名は、アプリケーションの名前をここでは**VideoPlayerDemos.Android**、これは、静的なから取得できます`Context.PackageName`プロパティ。 結果として得られる`Uri`オブジェクトに渡されます、`SetVideoURI`メソッドの`VideoView`:

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

### <a name="uwp-resource-loading"></a>UWP のリソースの読み込み

UWP`VideoPlayerRenderer`を構築、`Uri`パスのオブジェクトに設定し、`Source`プロパティの`MediaElement`:

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

**ビデオ リソースの再生**ページで、 **VideoPlayerDemos**ソリューションで使用、`OnPlatform`クラスを各プラットフォームのビデオ ファイルを指定します。

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

IOS のリソースが格納されている場合、**リソース**フォルダー、および UWP のリソースが、プロジェクトのルート フォルダーに格納されている場合は、プラットフォームごとに同じファイル名を使用することができます。 その場合は場合に直接その名前を設定することができます、`Source`プロパティの`VideoPlayer`します。

実行しているページを次に示します。

[![リソースのビデオを再生](loading-resources-images/playvideoresource-small.png "リソース ビデオの再生")](loading-resources-images/playvideoresource-large.png#lightbox "リソース ビデオの再生")

表示された方法[Web URI からビデオを読み込んで、](web-videos.md)と埋め込みリソースを再生する方法。 また、[デバイスのビデオ ライブラリからビデオを読み込んで、](accessing-library.md)します。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
