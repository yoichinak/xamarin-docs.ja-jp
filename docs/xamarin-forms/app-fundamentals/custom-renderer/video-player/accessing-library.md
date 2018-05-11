---
title: デバイスのビデオ ライブラリへのアクセス
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d0f58a4a53d41c23e993f8b8b89b3fca44e0733d
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="accessing-the-devices-video-library"></a>デバイスのビデオ ライブラリへのアクセス

最近のほとんどのモバイル デバイスとデスクトップ コンピューター、デバイスのカメラを使用してレコードのビデオ機能があります。 ユーザーを作成するビデオは、デバイス上のファイルとして保存されます。 これらのファイルをイメージ ライブラリから取得されで再生されることができます、`VideoPlayer`その他のビデオと同じようにクラスです。

## <a name="the-photo-picker-dependency-service"></a>フォト ピッカーの依存関係サービス

各プラットフォームの 3 つには、デバイスのイメージのライブラリから写真やビデオを選択するユーザーを許可する機能が含まれています。 デバイスのイメージのライブラリからビデオを再生する最初の手順は各プラットフォームのイメージの選択を起動する依存関係サービスを作成しています。 以下に示す依存関係サービスは非常にいずれかのように定義されている、 [**画像ライブラリから写真をピッキング**](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)記事、ビデオの選択、ではなく、ファイル名を返す点を除いて`Stream`オブジェクト。

という名前のインターフェイスを定義する .NET 標準のライブラリ プロジェクト`IVideoPicker`の依存関係サービス。

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

という名前のクラスを含む 3 つのプラットフォームの各`VideoPicker`このインターフェイスを実装します。

### <a name="the-ios-video-picker"></a>IOS ビデオ ピッカー

IOS `VideoPicker` iOS を使用して[ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/)されるべきである、iOS でのビデオ (「映画」と呼ばれる) に制限を指定する、イメージのライブラリへのアクセス`MediaType`プロパティです。 注意して`VideoPicker`を明示的に実装、`IVideoPicker`インターフェイスです。 また、`Dependency`依存関係サービスとしてこのクラスを識別する属性。 プラットフォームのプロジェクトの依存関係サービスを検索する Xamarin.Forms を許可する 2 つの要件を次に示します。

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Android ビデオ ピッカー

Android 実装`IVideoPicker`コールバック メソッドは、アプリケーションのアクティビティの一部である必要があります。 そのため、`MainActivity`クラスは、2 つのプロパティ、フィールド、およびコールバック メソッドを定義します。

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnCreate`メソッド`MainActivity`、静的に独自のインスタンスを格納`Current`プロパティです。 これによりの実装`IVideoPicker`を取得する、`MainActivity`インスタンスを開始するため、**選択ビデオ**の選択。

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

追加、`MainActivity`オブジェクトは、コード内にのみ、 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)通常アプリケーション コードがサポートするために変更する必要があるソリューション、`FormsVideoLibrary`クラスです。

### <a name="the-uwp-video-picker"></a>UWP ビデオ ピッカー

UWP 実装、`IVideoPicker`インターフェイスは、UWP [ `FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)です。 画像ライブラリにファイルの検索を開始し、MP4 および WMV (Windows Media Video) へのファイルの種類を制限します。

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>依存関係サービスを呼び出す

**ライブラリのビデオの再生**のページ、 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)プログラムは、ビデオ ピッカーの依存関係サービスを使用する方法を示します。 XAML ファイルに含まれる、`VideoPlayer`インスタンスおよび`Button`というラベルの付いた**ビデオ ライブラリを表示**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルが含まれています、`Clicked`のハンドラーは、`Button`です。 依存関係サービスを呼び出すへの呼び出しが必要です。`DependencyService.Get`の実装を取得する、`IVideoPicker`プラットフォーム プロジェクトでのインターフェイスです。 `GetVideoFileAsync`メソッドがそのインスタンスで呼び出されます。

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

`Clicked`ハンドラーを使用して、そのファイル名を作成、`FileVideoSource`オブジェクトに設定して、`Source`のプロパティ、`VideoPlayer`です。

各、`VideoPlayerRenderer`クラスには、コードが含まれています。 その`SetSource`型のオブジェクトのメソッド`FileVideoSource`です。 これらを次に示します。

### <a name="handling-ios-files"></a>IOS ファイルの処理

IOS のバージョンの`VideoPlayerRenderer`プロセス`FileVideoSource`、静的なを使用して、オブジェクト`Asset.FromUrl`ファイル名を持つメソッドです。 これにより、作成、`AVAsset`デバイスのイメージのライブラリ内のファイルを表すオブジェクト。

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
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Android のファイルの処理

型のオブジェクトを処理するときに`FileVideoSource`、Android の実装の`VideoPlayerRenderer`を使用して、`SetVideoPath`メソッドの`VideoView`デバイスのイメージのライブラリにファイルを指定します。

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
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>UWP ファイルの処理

型のオブジェクトを処理するときに`FileVideoSource`の UWP 実装、`SetSource`メソッドを作成する必要があります、`StorageFile`オブジェクト、そのファイルを開けません、およびストリーム オブジェクトを`SetSource`のメソッド、 `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

ビデオのビデオの後にほぼ即座に再生が開始、3 つすべてのプラットフォームのファイルがデバイスにダウンロードする必要があるために、ソースを設定します。



## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
