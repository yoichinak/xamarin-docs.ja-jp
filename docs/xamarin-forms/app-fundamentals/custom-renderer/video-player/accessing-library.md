---
title: "デバイスのビデオ ライブラリへのアクセス" の説明: "この記事では、Xamarin.Forms を使用してビデオ プレーヤー アプリケーションでデバイスのビデオ ライブラリにアクセスする方法について説明します。"
ms.prod: xamarin ms.assetid:364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:02/12/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="accessing-the-devices-video-library"></a>デバイスのビデオ ライブラリへのアクセス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

多くの最新のモバイル デバイスおよびデスクトップ コンピューターには、デバイスのカメラを使用してビデオを録画する機能があります。 ユーザーが作成するビデオは、デバイスにファイルとして格納されます。 これらのファイルは、イメージ ライブラリから取得して、他のビデオと同様、`VideoPlayer` クラスで再生できます。

## <a name="the-photo-picker-dependency-service"></a>フォト ピッカー依存関係サービス

各プラットフォームには、ユーザーがデバイスのイメージ ライブラリから写真またはビデオを選択できる機能が含まれています。 デバイスのイメージ ライブラリからビデオを再生するには、まず、各プラットフォームでイメージ ピッカーを起動する依存関係サービスを構築します。 次に説明する依存関係サービスは、記事「[**Picking a Photo from the Picture Library**](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)」(画像ライブラリから写真を選択する) で定義されているサービスと非常によく似ています。ただし、ビデオ ピッカーは、`Stream` オブジェクトではなくファイル名を返します。

.NET Standard ライブラリ プロジェクトでは、この依存関係サービス用に `IVideoPicker` という名前のインターフェイスが定義されます。

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

各プラットフォームには、このインターフェイスを実装する `VideoPicker` という名前のクラスが含まれます。

### <a name="the-ios-video-picker"></a>iOS ビデオ ピッカー

iOS `VideoPicker` では、iOS [`UIImagePickerController`](xref:UIKit.UIImagePickerController) を使用して、iOS `MediaType` プロパティでアクセスをビデオ (「ムービー」と呼ばれる) に制限することを指定し、イメージ ライブラリにアクセスします。 `VideoPicker` は、明示的に `IVideoPicker` インターフェイスを実装することに注意してください。 また、このクラスを依存関係サービスとして識別する `Dependency` 属性にも注意してください。 これらは、Xamarin.Forms でプラットフォーム プロジェクト内の依存関係サービスを検出できるようにするための 2 つの要件です。

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
            viewController.PresentViewController(imagePicker, true, null);

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

`IVideoPicker` の Android 実装には、アプリケーションのアクティビティの一部であるコールバック メソッドが必要です。 このため、`MainActivity` クラスでは、フィールドとコールバック メソッドの 2 つのプロパティを定義します。

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

`MainActivity` 内の `OnCreate` メソッドは、自身のインスタンスを静的な `Current` プロパティに格納します。 これにより、`IVideoPicker` の実装は、**Select Video** セレクタを開始するための `MainActivity` インスタンスを取得できます。

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

`MainActivity` オブジェクトに追加されるのは、`FormsVideoLibrary` クラスをサポートするために通常のアプリケーション コードを変更する必要がある、[**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) ソリューションのコードのみです。

### <a name="the-uwp-video-picker"></a>UWP ビデオ ピッカー

`IVideoPicker` インターフェイスの UWP 実装では、UWP [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) を使用します。 これは、ファイル検索を画像ライブラリから開始し、ファイルの種類を MP4 と WMV (Windows Media オーディオ) に制限します。

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

## <a name="invoking-the-dependency-service"></a>依存関係サービスの起動

[**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) プログラムの **Play Library Video** ページは、ビデオ ピッカー依存関係サービスの使用方法の例を示します。 次の XAML ファイルには、`VideoPlayer` インスタンスと、**Show Video Library** というラベルの付いた `Button` が含まれます。

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

分離コード ファイルには、その `Button` の `Clicked` ハンドラーが含まれます。 依存関係サービスを起動するには、`DependencyService.Get` を呼び出して、プラットフォーム プロジェクト内の `IVideoPicker` インターフェイスの実装を取得する必要があります。 次に、そのインスタンスで `GetVideoFileAsync` メソッドが呼び出されます。

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

`Clicked` ハンドラーは、そのファイル名を使用して、`FileVideoSource` オブジェクトを作成し、そのオブジェクトを `VideoPlayer` の `Source` プロパティに設定します。

各 `VideoPlayerRenderer` クラスには、`FileVideoSource` 型のオブジェクト用の `SetSource` メソッドのコードが含まれます。 これらを以下に示します。

### <a name="handling-ios-files"></a>iOS ファイルの処理

iOS バージョンの `VideoPlayerRenderer` では、ファイル名を含む静的な `Asset.FromUrl` メソッドを使用して `FileVideoSource` オブジェクトを処理します。 これにより、デバイスのイメージ ライブラリ内にあるファイルを表す `AVAsset` オブジェクトが作成されます。

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

### <a name="handling-android-files"></a>Android ファイルの処理

`VideoPlayerRenderer` の Android 実装では、`FileVideoSource` 型のオブジェクトを処理する場合、`VideoView` の `SetVideoPath` メソッドを使用して、デバイスのイメージ ライブラリ内にあるファイルを指定します。

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

`SetSource` メソッドの UWP 実装では、`FileVideoSource` 型のオブジェクトを処理する場合、`StorageFile` オブジェクトを作成し、そのファイルを読み取り用として開いて、ストリーム オブジェクトを `MediaElement` の `SetSource` メソッドに渡す必要があります。

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

各プラットフォームでは、ファイルがデバイス上にあり、ダウンロードする必要がないため、ビデオ ソースが設定されたすぐ後に、ビデオの再生が開始されます。

## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
- [画像ライブラリから写真を選択する](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
