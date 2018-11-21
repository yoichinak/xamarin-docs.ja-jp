---
title: デバイスのビデオ ライブラリへのアクセス
description: この記事では、ビデオ プレーヤー アプリケーションでは、Xamarin.Forms を使用して、デバイスのビデオ ライブラリにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 619469e4c4fd3901491c20d6215ec0a25c49f69d
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171184"
---
# <a name="accessing-the-devices-video-library"></a>デバイスのビデオ ライブラリへのアクセス

ほとんどの最新のモバイル デバイスとデスクトップ コンピューター、デバイスのカメラを使用してレコードのビデオ機能があります。 ユーザーが作成するビデオは、デバイス上のファイルとして保存されます。 これらのファイルをイメージ ライブラリから取得され、によって実行される、`VideoPlayer`クラスがその他のビデオと同じようにします。

## <a name="the-photo-picker-dependency-service"></a>写真の選択の依存関係サービス

各プラットフォームには、デバイスのイメージのライブラリから写真やビデオを選択するユーザーを許可する機能が含まれています。 デバイスのイメージのライブラリからビデオを再生する最初の手順では、各プラットフォームで、イメージのピッカーを起動する依存関係サービスを作成します。 次に示す依存関係サービスで定義されているいずれかによく似ています、 [**画像ライブラリから写真を選択**](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)記事、ビデオのピッカーをではなくファイル名を返す点が`Stream`オブジェクト。

.NET Standard ライブラリ プロジェクトは、という名前のインターフェイスを定義します。`IVideoPicker`の依存関係サービス。

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

という名前のクラスを含む、各プラットフォーム`VideoPicker`このインターフェイスを実装します。

### <a name="the-ios-video-picker"></a>IOS のビデオ ピッカー

IOS `VideoPicker` iOS を使用して[ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/)されるべきである、iOS でのビデオ ("movies"と呼ばれます) に制限を指定する、イメージ ライブラリへのアクセス`MediaType`プロパティ。 注意`VideoPicker`明示的に実装、`IVideoPicker`インターフェイス。 また、`Dependency`依存関係サービスとしてこのクラスを識別する属性。 これらは、Xamarin.Forms プラットフォーム プロジェクトでの依存関係サービスの検索を許可する 2 つの要件です。

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

### <a name="the-android-video-picker"></a>Android のビデオの選択

Android の実装の`IVideoPicker`コールバック メソッドは、アプリケーションのアクティビティの一部である必要があります。 そのため、`MainActivity`クラスは、2 つのプロパティ、フィールド、およびコールバック メソッドを定義します。

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

`OnCreate`メソッド`MainActivity`独自のインスタンスに静的な格納`Current`プロパティ。 これにより、実装の`IVideoPicker`を取得する、`MainActivity`インスタンスを開始するため、**ビデオの選択**の選択。

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

追加、`MainActivity`オブジェクトが唯一のコードで、 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)ソリューションをサポートするために通常のアプリケーション コードが必要な`FormsVideoLibrary`クラス。

### <a name="the-uwp-video-picker"></a>UWP ビデオ ピッカー

UWP 実装、`IVideoPicker`インターフェイスは、UWP を使用して[ `FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)します。 画像ライブラリ ファイルの検索を開始し、MP4 および WMV (Windows Media ビデオ) へのファイルの種類の制限します。

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

## <a name="invoking-the-dependency-service"></a>依存関係サービスの呼び出し

**ライブラリのビデオの再生**のページ、 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)プログラムは、ビデオの選択の依存関係サービスを使用する方法を示します。 XAML ファイルが含まれています、`VideoPlayer`インスタンスと`Button`というラベルの付いた**表示のビデオ ライブラリ**:

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

分離コード ファイルが含まれています、`Clicked`のハンドラー、`Button`します。 呼び出しの依存関係サービスを呼び出す必要があります`DependencyService.Get`の実装を取得する、`IVideoPicker`プラットフォーム プロジェクトのインターフェイス。 `GetVideoFileAsync`メソッドがそのインスタンスで呼び出されます。

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

`Clicked`ハンドラーを使用して、そのファイル名を作成、`FileVideoSource`オブジェクトに設定して、`Source`のプロパティ、`VideoPlayer`します。

各、`VideoPlayerRenderer`クラスには、コードが含まれています。 その`SetSource`型のオブジェクトのメソッド`FileVideoSource`します。 これらは、以下に示します。

### <a name="handling-ios-files"></a>IOS ファイルの処理

IOS のバージョンの`VideoPlayerRenderer`プロセス`FileVideoSource`、静的なを使用してオブジェクト`Asset.FromUrl`ファイル名を持つメソッド。 これにより、作成、`AVAsset`デバイスのイメージ ライブラリにあるファイルを表すオブジェクト。

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

型のオブジェクトを処理するときに`FileVideoSource`、Android の実装の`VideoPlayerRenderer`を使用して、`SetVideoPath`メソッドの`VideoView`デバイスのイメージ ライブラリにあるファイルを指定します。

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

### <a name="handling-uwp-files"></a>UWP のファイルの処理

型のオブジェクトを処理するときに`FileVideoSource`の UWP 実装、`SetSource`メソッドを作成する必要があります、`StorageFile`オブジェクト、読み取りについては、そのファイルを開き、ストリーム オブジェクトを渡す、`SetSource`のメソッド、 `MediaElement`:

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

ビデオのほぼすぐ後に、ビデオ再生が開始、各プラットフォームのファイルは、デバイスで、ダウンロードする必要があるために、ソースを設定します。



## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
