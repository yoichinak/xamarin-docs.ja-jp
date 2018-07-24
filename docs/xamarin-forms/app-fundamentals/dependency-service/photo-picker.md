---
title: 画像ライブラリから写真を選択
description: この記事では、Xamarin.Forms DependencyService クラスを使用して、電話の画像ライブラリから写真を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: b593815df9ce942a98496806116bacfa63e2a2d9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999088"
---
# <a name="picking-a-photo-from-the-picture-library"></a>画像ライブラリから写真を選択

この記事では、電話の画像ライブラリから写真を選択するユーザーを許可するアプリケーションの作成手順について説明します。 使用する必要が Xamarin.Forms にこの機能が含まれていないため[ `DependencyService` ](xref:Xamarin.Forms.DependencyService)各プラットフォームでネイティブ Api にアクセスします。  この記事では、次の手順を使用するために取り上げる`DependencyService`このタスク。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;共有コードで、インターフェイスを作成する方法を理解します。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android の実装](#Android_Implementation)** &ndash; Android のネイティブ コードでインターフェイスを実装する方法について説明します。
- **[ユニバーサル Windows プラットフォームの実装](#UWP_Implementation)** &ndash;ユニバーサル Windows プラットフォーム (UWP) のネイティブ コードでインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法について説明します`DependencyService`共有コードからネイティブの実装を呼び出す。

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、目的の機能を表す共有コードでインターフェイスを作成します。 フォト ピッキング アプリケーションの場合は、1 つのメソッドが必要です。 これで定義されている、 [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs)インターフェイスのサンプル コードは、.NET Standard ライブラリ。

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync`メソッドは、すぐに返す必要がありますが、返すことができないため、非同期メソッドが定義されている、`Stream`ユーザーが画像ライブラリを参照し、1 つを選択するまで、選択した写真のオブジェクト。

このインターフェイスは、プラットフォーム固有のコードを使用してすべてのプラットフォームに実装されます。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

IOS のカスタム実装の`IPicturePicker`インターフェイスの使用、 [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) 」の説明に従って、 [**写真をギャラリーから選択**](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)レシピと[サンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)します。

IOS のカスタム実装に含まれている、 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs)サンプル コードの iOS プロジェクト内のクラス。 このクラスを表示する、`DependencyService`マネージャーは、クラスを識別する必要があります、[`assembly`] 型の属性`Dependency`、クラスがパブリックであるされ、明示的に実装する必要があります、`IPicturePicker`インターフェイス。

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

`GetImageStreamAsync`メソッドを作成、`UIImagePickerController`して初期化し、フォト ライブラリからイメージを選択します。 2 つのイベント ハンドラーが必要です。 ユーザーが写真とその他のユーザーがフォト ライブラリの表示を取り消した場合に選択するとに 1 つ。 `PresentModalViewController`フォト ライブラリを次に、ユーザーに表示します。

この時点で、`GetImageStreamAsync`メソッドが返す必要があります、`Task<Stream>`オブジェクトから呼び出されているコードにします。 フォト ライブラリと対話するユーザーを完了し、イベント ハンドラーの 1 つと呼ばれるときにのみ、このタスクを完了します。 、このような場合の、 [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx)クラスが不可欠です。 クラスは、提供、`Task`から返される場合は、適切なジェネリック型のオブジェクト、`GetImageStreamAsync`メソッド、およびクラスは、後で合図ことができます、タスクが完了したときにします。

`FinishedPickingMedia`ユーザーが画像を選択すると、イベント ハンドラーが呼び出されます。 ただし、ハンドラーが用意されて、`UIImage`オブジェクトと`Task`.NET を返す必要があります`Stream`オブジェクト。 これは 2 つの手順で行います。`UIImage`オブジェクトが最初に格納されているメモリの JPEG ファイルに変換、`NSData`オブジェクトをクリックし、`NSData`オブジェクトを .NET に変換されます`Stream`オブジェクト。 呼び出し、`SetResult`のメソッド、`TaskCompletionSource`オブジェクトの提供することで、タスクが完了すると、`Stream`オブジェクト。

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

IOS アプリケーションでは、ユーザーから電話のフォト ライブラリへのアクセス許可が必要です。 次の追加、 `dict` Info.plist ファイルのセクション。

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android の実装

Android の実装で説明した手法を使用して、 [**イメージを選択**](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)レシピと[サンプル コード](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)します。 ただし、ユーザーが画像ライブラリからイメージを選択したときに呼び出されるメソッドは、`OnActivityResult`から派生したクラスでオーバーライド`Activity`します。 このため、通常の[ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Android プロジェクトでクラスが、フィールド、プロパティのオーバーライドと補完された、`OnActivityResult`メソッド。

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

`OnActivityResult`オーバーライドを Android で選択した画像ファイルを示します`Uri`オブジェクトが、これは、.NET に変換`Stream`オブジェクトを呼び出すことによって、`OpenInputStream`のメソッド、`ContentResolver`オブジェクトから取得された、アクティビティの`ContentResolver`プロパティ。

Android の実装を使用して iOS のカスタム実装のように、`TaskCompletionSource`をタスクが完了したときに通知します。 これは、`TaskCompletionSource`オブジェクトがパブリック プロパティとして定義されている、`MainActivity`クラス。 これにより、プロパティで参照される、 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Android プロジェクトでクラス。 これは、使用して、クラス、`GetImageStreamAsync`メソッド。

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

このメソッドにアクセスする、`MainActivity`いくつかの目的のクラス: の`Instance`プロパティの`PickImageId`フィールドを`TaskCompletionSource`プロパティを呼び出すと`StartActivityForResult`します。 このメソッドが定義されている、`FormsAppCompatActivity`クラス、基底クラスの`MainActivity`します。

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP の実装

ユニバーサル Windows プラットフォーム用の写真の選択コントロールの実装は必要としない、iOS と Android の実装とは異なり、`TaskCompletionSource`クラス。 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs)クラスで使用、 [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)フォト ライブラリへのアクセスを取得するクラス。 `PickSingleFileAsync`メソッドの`FileOpenPicker`自体は、非同期で、`GetImageStreamAsync`メソッドを使用できます`await`メソッド (およびその他の非同期メソッド) を返すと、`Stream`オブジェクト。


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

## <a name="implementing-in-shared-code"></a>共有コードで実装します。

これで、各プラットフォームのインターフェイスが実装されている .NET Standard ライブラリに、アプリケーションは、その利用できます。

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

`Clicked`ハンドラーを使用して、`DependencyService`クラスを呼び出す`GetImageStreamAsync`します。 これにより、プラットフォーム プロジェクトで呼び出しをします。 メソッドを返す場合、`Stream`オブジェクト、ハンドラーを作成し、`Image`要素では、その画像の`TabGestureRecognizer`、し、置き換えます、 `StackLayout` 、ページを`Image`:

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
