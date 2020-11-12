---
title: 画像ライブラリから写真を選択する
description: この記事では、Xamarin.Forms の DependencyService クラスを使用して、電話の画像ライブラリから写真を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2d66464798860848e42e05bb3aaa36fc80e115b1
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93369403"
---
# <a name="picking-a-photo-from-the-picture-library"></a>画像ライブラリから写真を選択する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/dependencyservice/)

この記事では、ユーザーが電話の画像ライブラリから写真を選択できるアプリケーションを作成する手順について説明します。 Xamarin.Forms にはこの機能は含まれていないため、[`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用して各プラットフォームのネイティブ API にアクセスする必要があります。

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、目的の機能を表すインターフェイスを共有コード内に作成します。 写真選択アプリケーションの場合、必要なメソッドは 1 つだけです。 これは、サンプル コードの .NET Standard ライブラリの [`IPhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos/Services/IPhotoPickerService.cs) インターフェイスで定義されています。

```csharp
namespace DependencyServiceDemos
{
    public interface IPhotoPickerService
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` メソッドは、すぐに戻る必要がありますが、ユーザーが画像ライブラリを参照して写真を選択するまで、選択された写真の `Stream` オブジェクトを返すことができないため、非同期として定義されます。

このインターフェイスは、すべてのプラットフォームにおいてプラットフォーム固有のコードを使用して実装されます。

## <a name="ios-implementation"></a>iOS での実装

iOS での `IPhotoPickerService` インターフェイスの実装では、「 [**Choose a Photo from the Gallery**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)」(ギャラリーから写真を選択する) レシピと [サンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)で説明されているように、 [`UIImagePickerController`](xref:UIKit.UIImagePickerController) が使用されます。

iOS での実装は、サンプル コードの iOS プロジェクト内の [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.iOS/Services/PhotoPickerService.cs) クラスに含まれます。 このクラスを `DependencyService` マネージャーで認識できるようにするには、クラスが `Dependency` 型の [`assembly`] 属性で識別されていて、パブリックとして指定され、`IPhotoPickerService` インターフェイスを明示的に実装している必要があります。

```csharp
[assembly: Dependency (typeof (PhotoPickerService))]
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentViewController(imagePicker, true, null);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

`GetImageStreamAsync` メソッドでは、`UIImagePickerController` が作成されて、フォト ライブラリから画像を選択するように初期化されます。 2 つのイベント ハンドラーが必要です。1 つはユーザーが写真を選択するときのためのもので、もう 1 つはユーザーがフォト ライブラリの表示を取り消すときのためのものです。 その後、`PresentViewController` メソッドにより、ユーザーに対してフォト ライブラリが表示されます。

この時点で、`GetImageStreamAsync` メソッドから呼び出し元のコードに `Task<Stream>` オブジェクトを返す必要があります。 このタスクは、ユーザーがフォト ライブラリとの対話を終了し、イベント ハンドラーの 1 つが呼び出されたときにのみ、実行できます。 このような状況に対しては、[`TaskCompletionSource`](/dotnet/api/system.threading.tasks.taskcompletionsource-1) クラスが不可欠です。 このクラスでは、`GetImageStreamAsync` メソッドから戻すための適切なジェネリック型の `Task` オブジェクトが提供されており、後でタスクが完了したときに通知を受け取ることができます。

ユーザーが画像を選択すると、`FinishedPickingMedia` イベント ハンドラーが呼び出されます。 ただし、ハンドラーでは `UIImage` オブジェクトが提供されますが、`Task` からは .NET の `Stream` オブジェクトが返される必要があります。 これは 2 つのステップで行われます。最初に `UIImage` オブジェクトが、`NSData` オブジェクトに格納されているメモリ内の PNG ファイルまたは JPEG ファイルに変換され、次に `NSData` オブジェクトが .NET の `Stream` オブジェクトに変換されます。 `TaskCompletionSource` オブジェクトの `SetResult` メソッドに対する呼び出しでは、`Stream` オブジェクトを提供することによってタスクが完了されます。

```csharp
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data;
                if (args.ReferenceUrl.PathExtension.Equals("PNG") || args.ReferenceUrl.PathExtension.Equals("png"))
                {
                    data = image.AsPNG();
                }
                else
                {
                    data = image.AsJPEG(1);
                }
                Stream stream = data.AsStream();

                UnregisterEventHandlers();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                UnregisterEventHandlers();
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            UnregisterEventHandlers();
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }

        void UnregisterEventHandlers()
        {
            imagePicker.FinishedPickingMedia -= OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled -= OnImagePickerCancelled;
        }
    }
}
```

iOS アプリケーションが電話のフォト ライブラリにアクセスするには、ユーザーからのアクセス許可が必要です。 Info.plist ファイルの `dict` セクションに、以下を追加します。

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```

## <a name="android-implementation"></a>Android での実装

Android での実装では、 [**イメージ選択**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)レシピと [サンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)で説明されている手法が使用されます。 ただし、ユーザーが画像ライブラリから画像を選択したときに呼び出されるメソッドは、`OnActivityResult` の派生クラスでの `Activity` のオーバーライドです。 このため、Android プロジェクトの通常の [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/MainActivity.cs) クラスは、フィールド、プロパティ、および `OnActivityResult` メソッドのオーバーライドで補完されています。

```csharp
public class MainActivity : FormsAppCompatActivity
{
    internal static MainActivity Instance { get; private set; }  

    protected override void OnCreate(Bundle savedInstanceState)
    {
        // ...
        Instance = this;
    }
    // ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}
```

`OnActivityResult` のオーバーライドでは選択された画像ファイルが Android の `Uri` オブジェクトで示されていますが、これは、アクティビティの `ContentResolver` プロパティから取得された `ContentResolver` オブジェクトの `OpenInputStream` メソッドを呼び出すことによって、.NET の `Stream` オブジェクトに変換できます。

iOS での実装と同様に、Android での実装では `TaskCompletionSource` を使用してタスク完了時の通知を受け取ります。 この `TaskCompletionSource` オブジェクトは、`MainActivity` クラスでパブリック プロパティとして定義されています。 これにより、Android プロジェクトの [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/Services/PhotoPickerService.cs) クラスでこのプロパティを参照できます。 これは、`GetImageStreamAsync` メソッドを含むクラスです。

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.Droid
{
    public class PhotoPickerService : IPhotoPickerService
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

このメソッドでは、`Instance` プロパティ、`PickImageId` フィールド、`TaskCompletionSource` プロパティ、`StartActivityForResult` の呼び出しなど、複数の目的で `MainActivity` クラスがアクセスされます。 このメソッドは、`MainActivity` の基底クラスである `FormsAppCompatActivity` クラスによって定義されています。

## <a name="uwp-implementation"></a>UWP での実装

iOS や Android での実装とは異なり、ユニバーサル Windows プラットフォーム用の写真ピッカーの実装では、`TaskCompletionSource` クラスは必要ありません。 [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.UWP/Services/PhotoPickerService.cs) クラスでは、[`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) クラスを使用してフォト ライブラリへのアクセスが取得されます。 `FileOpenPicker` の `PickSingleFileAsync` メソッド自体が非同期なので、`GetImageStreamAsync` メソッドでは、そのメソッド (および他の非同期メソッド) で `await` を単に使用して、`Stream` オブジェクトを返すことができます。

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.UWP
{
    public class PhotoPickerService : IPhotoPickerService
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

## <a name="implementing-in-shared-code"></a>共有コードでの実装

各プラットフォーム用のインターフェイスを実装したので、.NET Standard ライブラリの共有コードでそれを利用できます。

UI には、クリックすると写真を選択できる [`Button`](xref:Xamarin.Forms.Button) が含まれます。

```xaml
<Button Text="Pick Photo"
        Clicked="OnPickPhotoButtonClicked" />
```

`Clicked` イベント ハンドラーでは `DependencyService` クラスが使われ、`GetImageStreamAsync` が呼び出されます。 これによって、プラットフォーム プロジェクトが呼び出されます。 メソッドにより `Stream` オブジェクトが返された場合、ハンドラーによって `image` オブジェクトの `Source` プロパティがその `Stream` データに設定されます。

```csharp
async void OnPickPhotoButtonClicked(object sender, EventArgs e)
{
    (sender as Button).IsEnabled = false;

    Stream stream = await DependencyService.Get<IPhotoPickerService>().GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }

    (sender as Button).IsEnabled = true;
}
```

## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル)](/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [ギャラリーから写真を選択する (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [画像を選択する (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)