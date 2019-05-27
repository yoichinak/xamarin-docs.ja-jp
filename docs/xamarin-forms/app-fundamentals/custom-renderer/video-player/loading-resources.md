---
title: アプリケーション リソース ビデオの読み込み
description: この記事では、Xamarin.Forms を使って、ビデオ プレーヤー アプリケーションでアプリケーション リソースとして格納されたビデオを読み込む方法について説明します。
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 505b2ab60a4fc828790aa2b351460de8980c6b9d
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926794"
---
# <a name="loading-application-resource-videos"></a>アプリケーション リソース ビデオの読み込み

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/VideoPlayerDemos/)

`VideoPlayer` ビュー用のカスタム レンダラーは、アプリケーション リソースとして個々のプラットフォーム プロジェクトに埋め込まれたビデオ ファイルを再生できます。 しかし、現在のバージョンの `VideoPlayer` は、.NET Standard ライブラリに埋め込まれたリソースにアクセスすることはできません。

これらのリソースを読み込むには、`Path` プロパティをリソースのファイル名 (またはフォルダーとファイル名) に設定して、`ResourceVideoSource` のインスタンスを作成します。 また、リソースを参照する静的な `VideoSource.FromResource` メソッドを呼び出すこともできます。 この後、`ResourceVideoSource` オブジェクトを `VideoPlayer` の `Source` プロパティに設定します。

## <a name="storing-the-video-files"></a>ビデオ ファイルの格納

プラットフォーム プロジェクトにビデオ ファイルを格納する方法は、プラットフォームごとに異なります。

### <a name="ios-video-resources"></a>iOS ビデオ リソース

iOS プロジェクトでは、ビデオを**リソース** フォルダー、または**リソース** フォルダーのサブフォルダーに格納できます。 ビデオ ファイルには、`BundleResource` の `Build Action` が含まれている必要があります。 `ResourceVideoSource` の `Path` プロパティをファイル名に設定します。たとえば、 **MyFile.mp4 (** **リソース** フォルダー内のファイルの場合) や **MyFolder/MyFile.mp4** (ここで、 **MyFolder** は、 **リソース** フォルダーのサブフォルダー) などに設定します。

**VideoPlayerDemos** ソリューションでは、**VideoPlayerDemos.iOS** に、**Videos** という名前の**リソース** フォルダーのサブフォルダーが含まれ、このサブフォルダーには、**iOSApiVideo.mp4** という名前のファイルが含まれます。 これは、Xamarin Web サイトを使用して iOS `AVPlayerViewController` クラス用のドキュメントを検索する方法を示す短いビデオです。

### <a name="android-video-resources"></a>Android ビデオ リソース

Android プロジェクトでは、**raw** という名前の**リソース** フォルダーのサブフォルダーにビデオを格納する必要があります。 **raw** フォルダーにサブフォルダーを含めることはできません。 ビデオ ファイルに `AndroidResource` の `Build Action` を指定します。 `ResourceVideoSource` の `Path` プロパティをファイル名 (たとえば、**MyFile.mp4**) に設定します。

**VideoPlayerDemos.Android** プロジェクトには、**raw** という名前の**リソース** フォルダーのサブフォルダーが含まれ、このサブフォルダーには、**AndroidApiVideo.mp4** という名前のファイルが含まれます。

### <a name="uwp-video-resources"></a>UWP ビデオ リソース

ユニバーサル Windows プラットフォーム プロジェクトでは、プロジェクト内の任意のフォルダーにビデオを格納できます。 ファイルに `Content` の `Build Action` を指定します。 `ResourceVideoSource` の `Path` プロパティをフォルダーとファイル名 (たとえば、**MyFolder/MyVideo.mp4**) に設定します。

**VideoPlayerDemos.UWP** プロジェクトには **Videos** という名前のフォルダーが含まれ、このフォルダーにはファイル **UWPApiVideo.mp4** が含まれます。

## <a name="loading-the-video-files"></a>ビデオ ファイルの読み込み

各プラットフォーム レンダラー クラスには、リソースとして格納されたビデオ ファイルを読み込むための `SetSource` メソッドのコードが含まれます。

### <a name="ios-resource-loading"></a>iOS リソースの読み込み

iOS バージョンの `VideoPlayerRenderer` では、`NSBundle` の `GetUrlForResource` メソッドを使用してリソースを読み込みます。 完全なパスをファイル名、拡張子、ディレクトリに分割する必要があります。 コードでは、.NET `System.IO` 名前空間の `Path` クラスを使用して、ファイルのパスをこれらのコンポーネントに分割します。

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

                if (!String.IsNullOrWhiteSpace(path))
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

### <a name="android-resource-loading"></a>Android リソースの読み込み

Android `VideoPlayerRenderer` では、ファイル名とパッケージ名を使用して、`Uri` オブジェクトを構築します。 パッケージ名は、静的な `Context.PackageName` プロパティから取得できるアプリケーションの名前 (この場合は **VideoPlayerDemos.Android**) です。 この後、生成された `Uri` オブジェクトは、`VideoView` の `SetVideoURI` メソッドに渡されます。

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

UWP `VideoPlayerRenderer` では、パス用の `Uri` オブジェクトを構築し、それを `MediaElement` の `Source` プロパティに設定します。

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

**VideoPlayerDemos** ソリューションの **Play Video Resource** ページでは、`OnPlatform` クラスを使用して、各プラットフォームのビデオ ファイルを指定します。

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

iOS リソースを**リソース** フォルダーに格納する場合、および UWP リソースをプロジェクトのルート フォルダーに格納する場合、各プラットフォームに同じファイル名を使用できます。 その場合は、その名前を `VideoPlayer` の `Source` に直接設定できます。

以下に、実行中のページを示します。

[![ビデオ リソースの再生](loading-resources-images/playvideoresource-small.png "ビデオ リソースの再生")](loading-resources-images/playvideoresource-large.png#lightbox "ビデオ リソースの再生")

以上で、[Web URI からビデオを読み込む](web-videos.md)方法と埋め込まれたリソースを再生する方法がわかりました。 さらに、[デバイスのビデオ ライブラリからビデオを読み込む](accessing-library.md)こともできます。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/VideoPlayerDemos/)
