---
title: 画像ライブラリから写真を選択する
description: この記事では、Xamarin.Forms の DependencyService クラスを使用して、電話の画像ライブラリから写真を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 5c4d43723bc23d8a16be8fec0a895a31ab8bcfdc
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233966"
---
# <a name="picking-a-photo-from-the-picture-library"></a>画像ライブラリから写真を選択する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)

この記事では、ユーザーが電話の画像ライブラリから写真を選択できるアプリケーションを作成する手順について説明します。 Xamarin.Forms にはこの機能は含まれていないため、[`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用して各プラットフォームのネイティブ API にアクセスする必要があります。  この記事では、このタスクのために `DependencyService` を使用する以下の手順について説明します。

- **[インターフェイスの作成](#Creating_the_Interface)** &ndash; 共有コードでインターフェイスを作成する方法を理解します。
- **[iOS での実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[Android での実装](#Android_Implementation)** &ndash; Android 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[ユニバーサル Windows プラットフォームでの実装](#UWP_Implementation)** &ndash; ユニバーサル Windows プラットフォーム (UWP) 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[共有コードでの実装](#Implementing_in_Shared_Code)** &ndash; `DependencyService` を使用して共有コードからネイティブ実装を呼び出す方法を学習します。

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、目的の機能を表すインターフェイスを共有コードで作成します。 写真選択アプリケーションの場合、必要なメソッドは 1 つだけです。 これは、サンプル コードの .NET Standard ライブラリの [`IPicturePicker`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) インターフェイスで定義されています。

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` メソッドは、すぐに戻る必要がありますが、ユーザーが画像ライブラリを参照して写真を選択するまで、選択された写真の `Stream` オブジェクトを返すことができないため、非同期として定義されます。

このインターフェイスは、すべてのプラットフォームにおいてプラットフォーム固有のコードを使用して実装されます。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS での実装

iOS での `IPicturePicker` インターフェイスの実装では、「[**Choose a Photo from the Gallery**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)」(ギャラリーから写真を選択する) レシピと[サンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)で説明されているように、[`UIImagePickerController`](xref:UIKit.UIImagePickerController) が使用されます。

iOS での実装は、サンプル コードの iOS プロジェクト内の [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) クラスに含まれます。 このクラスを `DependencyService` マネージャーで認識できるようにするには、クラスが `Dependency` 型の [`assembly`] 属性で識別されていて、パブリックとして指定され、`IPicturePicker` インターフェイスを明示的に実装している必要があります。

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
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
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

`GetImageStreamAsync` メソッドでは、`UIImagePickerController` が作成されて、フォト ライブラリから画像を選択するように初期化されます。 2 つのイベント ハンドラーが必要です。1 つはユーザーが写真を選択するときのためのもので、もう 1 つはユーザーがフォト ライブラリの表示を取り消すときのためのものです。 その後、`PresentModalViewController` で、ユーザーに対してフォト ライブラリが表示されます。

この時点で、`GetImageStreamAsync` メソッドから呼び出し元のコードに `Task<Stream>` オブジェクトを返す必要があります。 このタスクは、ユーザーがフォト ライブラリとの対話を終了し、イベント ハンドラーの 1 つが呼び出されたときにのみ、実行できます。 このような状況に対しては、[`TaskCompletionSource`](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) クラスが不可欠です。 このクラスでは、`GetImageStreamAsync` メソッドから戻すための適切なジェネリック型の `Task` オブジェクトが提供されており、後でタスクが完了したときに通知を受け取ることができます。

ユーザーが画像を選択すると、`FinishedPickingMedia` イベント ハンドラーが呼び出されます。 ただし、ハンドラーでは `UIImage` オブジェクトが提供されますが、`Task` からは .NET の `Stream` オブジェクトが返される必要があります。 これは 2 つのステップで行われます。最初に `UIImage` オブジェクトがメモリ内で JPEG ファイルに変換されて `NSData` オブジェクトに格納され、次に `NSData` オブジェクトが .NET の `Stream` オブジェクトに変換されます。 `TaskCompletionSource` オブジェクトの `SetResult` メソッドに対する呼び出しでは、`Stream` オブジェクトを提供することによってタスクが完了されます。

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
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
                NSData data = image.AsJPEG(1);
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


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android での実装

Android での実装では、[**イメージ選択**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)レシピと[サンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)で説明されている手法が使用されます。 ただし、ユーザーが画像ライブラリから画像を選択したときに呼び出されるメソッドは、`OnActivityResult` の派生クラスでの `Activity` のオーバーライドです。 このため、Android プロジェクトの通常の [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) クラスは、フィールド、プロパティ、および `OnActivityResult` メソッドのオーバーライドで補完されています。

```csharp
public class MainActivity : FormsAppCompatActivity
{
    ...
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

iOS での実装と同様に、Android での実装では `TaskCompletionSource` を使用してタスク完了時の通知を受け取ります。 この `TaskCompletionSource` オブジェクトは、`MainActivity` クラスでパブリック プロパティとして定義されています。 これにより、Android プロジェクトの [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) クラスでこのプロパティを参照できます。 これは、`GetImageStreamAsync` メソッドを含むクラスです。

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
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

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP での実装

iOS や Android での実装とは異なり、ユニバーサル Windows プラットフォーム用の写真ピッカーの実装では、`TaskCompletionSource` クラスは必要ありません。 [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) クラスでは、[`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) クラスを使用してフォト ライブラリへのアクセスが取得されます。 `FileOpenPicker` の `PickSingleFileAsync` メソッド自体が非同期なので、`GetImageStreamAsync` メソッドでは、そのメソッド (および他の非同期メソッド) で `await` を単に使用して、`Stream` オブジェクトを返すことができます。


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
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

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードでの実装

各プラットフォーム用のインターフェイスを実装したので、.NET Standard ライブラリのアプリケーションでそれを利用できます。

[`App`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) クラスでは、写真を選択するための `Button` が作成されます。

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` ハンドラーでは、`DependencyService` クラスをして `GetImageStreamAsync` が呼び出されます。 これは結果としてプラットフォーム プロジェクトでの呼び出しになります。 メソッドから `Stream` オブジェクトが返される場合、ハンドラーでは `TapGestureRecognizer` によってその画像の `Image` 要素が作成されて、ページ上の `StackLayout` がその `Image` で置き換えられます。

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

`Image` 要素をタップすると、ページは通常に戻ります。


## <a name="related-links"></a>関連リンク

- [ギャラリーから写真を選択する (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [画像を選択する (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
