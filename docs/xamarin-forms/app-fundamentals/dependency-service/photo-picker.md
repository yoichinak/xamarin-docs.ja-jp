---
title: 画像ライブラリから写真を選択
description: DependencyService を使用して、携帯電話の画像ライブラリから写真を選択するには
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 95ac9912f0ff6788a2a633b3f8d3495e286030f1
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="picking-a-photo-from-the-picture-library"></a>画像ライブラリから写真を選択

この記事は、ユーザーがスマート フォンの画像ライブラリから写真を選択できるアプリケーションの作成に使用について説明します。 使用する必要は Xamarin.Forms にこの機能が含まれていないため[ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)各プラットフォームでネイティブの Api にアクセスします。  この記事を使用して次の手順を説明します。 `DependencyService` 、このタスクに対する。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;共有コードで、インターフェイスを作成する方法を理解します。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android 実装](#Android_Implementation)** &ndash; for Android のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[ユニバーサル Windows プラットフォームの実装](#UWP_Implementation)** &ndash;ユニバーサル Windows プラットフォーム (UWP) のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Windows Phone 実装](#Windows_Phone_Implementation)** &ndash; Windows Phone 8.1 用のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法を学習`DependencyService`に共有コードからネイティブの実装を呼び出します。

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、必要な機能を表現する共有コードでインターフェイスを作成します。 アプリケーションの場合、写真を選択、1 つのメソッドが必要です。 定義されて、 [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs)サンプル コードのポータブル クラス ライブラリ内のインターフェイス。

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync`メソッドは、迅速に返す必要がありますが、返すことができないために、非同期メソッドが定義されて、`Stream`ユーザーが画像ライブラリを参照し、1 つを選択するまでは、選択した写真のオブジェクト。

このインターフェイスは、プラットフォーム固有のコードを使用するすべてのプラットフォームで実装されます。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

IOS の実装、`IPicturePicker`インターフェイスの使用、 [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) 」の説明に従って、 [**ギャラリーから写真を選択する**](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)レシピと[のサンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)です。

IOS の実装に含まれている、 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) iOS プロジェクトで、サンプル コードのクラスです。 このクラスに表示されるようにする、`DependencyService`マネージャーは、クラスを識別する必要があります、[`assembly`] 型の属性`Dependency`、クラスがパブリックであるされ、明示的に実装する必要があります、`IPicturePicker`インターフェイス。

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

`GetImageStreamAsync`メソッドを作成、`UIImagePickerController`フォト ライブラリからイメージを選択して初期化します。 2 つのイベント ハンドラーが必要: 写真とその他のユーザーがフォト ライブラリの表示をキャンセルするときのユーザーが選択したときのいずれか。 `PresentModalViewController`をユーザーに写真ライブラリを表示します。

この時点で、`GetImageStreamAsync`メソッドが返す必要があります、`Task<Stream>`オブジェクトを呼び出しているコードにします。 フォト ライブラリとの対話ユーザーを完了し、イベント ハンドラーのどちらを呼び出す場合にのみ、このタスクが完了します。 このような状況、 [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx)クラスが不可欠です。 クラスは、提供、`Task`から返される場合は、適切なジェネリック型のオブジェクト、`GetImageStreamAsync`メソッド、およびクラスが後で通知されるタスクが完了したときにします。

`FinishedPickingMedia`ユーザーが画像を選択したときにイベント ハンドラーが呼び出されます。 ただし、ハンドラーが用意されて、`UIImage`オブジェクトおよび`Task`.NET を返す必要があります`Stream`オブジェクト。 これは 2 つの手順で行います。`UIImage`オブジェクトに格納されているメモリ内の JPEG ファイルに変換されて、`NSData`オブジェクト、し、`NSData`オブジェクトが、.NET に変換`Stream`オブジェクト。 呼び出し、`SetResult`のメソッド、`TaskComkpletionSource`オブジェクトでは、タスクを完了することにより、`Stream`オブジェクト。

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

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
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

IOS アプリケーションには、ユーザーの電話の写真のライブラリへのアクセス許可が必要です。 次の追加、 `dict` Info.plist ファイルのセクション。

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android の実装

Android の実装で説明した手法を使用して、 [**イメージを選択**](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)レシピと[のサンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)です。 ただし、ユーザーが画像ライブラリからイメージを選択したときに呼び出されるメソッドは、`OnActivityResult`から派生したクラスでオーバーライド`Activity`です。 このため、通常の[ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Android プロジェクトでクラスが、フィールド、プロパティ、およびのオーバーライドで補完された、`OnActivityResult`メソッド。

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
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

`OnActivityResult`オーバーライドは、Android を選択した画像ファイルを示す`Uri`オブジェクトが、これは、.NET に変換する`Stream`を呼び出してオブジェクト、`OpenInputStream`のメソッド、`ContentResolver`オブジェクトから取得された、アクティビティの`ContentResolver`プロパティです。

Android の実装を使用して、iOS の実装と同様に、`TaskCompletionSource`をタスクが完了したときに通知します。 これは、`TaskCompletionSource`オブジェクトがパブリック プロパティとして定義されている、`MainActivity`クラスです。 これにより、プロパティで参照されている、 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Android プロジェクトのクラスです。 これは、クラス、`GetImageStreamAsync`メソッド。

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

このメソッドにアクセスする、`MainActivity`いくつかの目的のクラス: の`Instance`プロパティの`PickImageId`フィールドを`TaskCompletionSource`プロパティ、およびを呼び出す`StartActivityForResult`です。 このメソッドは、`FormsApplicationActivity`クラスの基底クラスでは`MainActivity`します。

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP 実装

IOS および Android の実装とは異なり、ユニバーサル Windows プラットフォームのフォト ピッカーの実装は不要、`TaskCompletionSource`クラスです。 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs)クラスの使用、 [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)フォト ライブラリへのアクセスを取得するクラス。 `PickSingleFileAsync`メソッドの`FileOpenPicker`非同期、自体が、`GetImageStreamAsync`メソッドを使用するだけ`await`メソッド (と他の非同期メソッド) を返すと、`Stream`オブジェクト。


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

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-81-implementation"></a>Windows Phone 8.1 の実装

Windows Phone 8.1 の実装は、主要な影響のある 1 つの大きな違いを除く UWP 実装に似ています。、`PickSingleFileAsync`メソッドの`FileOpenPicker`は使用できません。 代わりに、`GetImageStreamAsync`メソッドを呼び出す必要があります`PickSingleFileAndContinue`です。 このメソッドが戻るフォト ライブラリは、ユーザーに表示されますが、写真のユーザーの選択がへの呼び出しによって通知されるときに、`OnActivated`のメソッド、`App`クラスです。

このため、 [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/App.xaml.cs) Windows Phone 8.1 プロジェクト内の Xamarin.Forms プロジェクト テンプレートによって作成されたクラスが型のプロパティで補完された`TaskCompletionSource`およびのオーバーライド、`OnActivated`メソッド。

```csharp
namespace DependencyServiceSample.WinPhone
{
    public sealed partial class App : Application
    {
        ...
        public TaskCompletionSource<Stream> TaskCompletionSource { set; get; }

        protected async override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            IContinuationActivatedEventArgs continuationArgs = args as IContinuationActivatedEventArgs;

            if (continuationArgs != null &&
                continuationArgs.Kind == ActivationKind.PickFileContinuation)
            {
                FileOpenPickerContinuationEventArgs pickerArgs = args as FileOpenPickerContinuationEventArgs;

                if (pickerArgs.Files.Count > 0)
                {
                    // Get the file and a Stream
                    StorageFile storageFile = pickerArgs.Files[0];
                    IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
                    Stream stream = raStream.AsStreamForRead();

                    // Set the completion of the Task
                    TaskCompletionSource.SetResult(stream);
                }
                else
                {
                    TaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnActivated`いくつかの理由から、アプリケーションの起動を含むメソッドを呼び出すことがあります。 コードに制限呼び出しだけと、ファイル オープン ピッカーが完了したらを取得し、`Stream`オブジェクトから、`StorageFile`です。

[ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/PicturePickerImplementation.cs)クラスに含まれる、`GetImageStreamAsync`を作成するメソッド、`FileOpenPicker`と呼び出し`PickSingleFileAndContainue`:

```csharp
[assembly: Xamarin.Forms.Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.WinPhone
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
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

            // Display the picker for a single file (resumes in OnActivated in App.xaml.cs)
            openPicker.PickSingleFileAndContinue();

            // Create a TaskCompletionSource stored in App.xaml.cs
            TaskCompletionSource<Stream> taskCompletionSource = new TaskCompletionSource<Stream>();
            (Application.Current as App).TaskCompletionSource = taskCompletionSource;

            // Return the Task object
            return taskCompletionSource.Task;
        }
    }
}

```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードで実装します。

プラットフォームごとに、インターフェイスが実装されているようになりました、共通のポータブル クラス ライブラリ内のアプリケーションは、その利用できます。

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs)クラスを作成、`Button`写真を選択します。

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked`ハンドラーを使用して、`DependencyService`を呼び出すクラス`GetImageStreamAsync`です。 これは、結果、プラットフォームのプロジェクトでの呼び出し。 メソッドを返す場合、`Stream`オブジェクト、ハンドラーを作成し、`Image`とその図の要素、 `TabGestureRecognizer`、置き換えて、`StackLayout`を使用してページ上`Image`:

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

タップすると、`Image`要素が正常にページを返します。


## <a name="related-links"></a>関連リンク

- [ギャラリー (iOS) からの写真を選択します。](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [イメージ (Android) を選択します。](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
