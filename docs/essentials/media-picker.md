---
title: 'Xamarin.Essentials: メディア ピッカー'
description: Xamarin.Essentials の MediaPicker クラスを使用すると、ユーザーはデバイス上で写真やビデオを選択したり、撮影したりすることができます。
ms.assetid: 23460875-6cf9-4440-a97b-46c55b0bca69
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4d080ab13bb04b8502ad547234d5b9592af406e1
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972254"
---
# <a name="no-locxamarinessentials-media-picker"></a>Xamarin.Essentials: メディア ピッカー

**MediaPicker** クラスを使用すると、ユーザーはデバイス上で写真やビデオを選択したり、撮影したりすることができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**MediaPicker** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

次のアクセス許可が必要であり、Android プロジェクトで構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
// Needed for Picking photo/video
[assembly: UsesPermission(Android.Manifest.Permission.ReadExternalStorage)]

// Needed for Taking photo/video
[assembly: UsesPermission(Android.Manifest.Permission.WriteExternalStorage)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]

// Add these properties if you would like to filter out devices that do not have cameras, or set to false to make them optional
[assembly: UsesFeature("android.hardware.camera", Required = true)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = true)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可]** 領域を探し、これらのアクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="ios"></a>[iOS](#tab/ios)

`Info.plist` で、次のキーを追加します。

```xml
<key>NSCameraUsageDescription</key>
<string>This app needs access to the camera to take photos.</string>
<key>NSMicrophoneUsageDescription</key>
<string>This app needs access to microphone for taking videos.</string>
<key>NSPhotoLibraryAddUsageDescription</key>
<string>This app needs access to the photo gallery for picking photos and videos.</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>This app needs access to photos gallery for picking photos and videos.</string>
```

それぞれの `<string>` がユーザーに表示されたら、アプリ固有の説明に必ず更新してください。

# <a name="uwp"></a>[UWP](#tab/uwp)

**[機能]** の `Package.appxmanifest` で、`Microphone` と `Webcam` の機能がオンになっていることを確認します。

-----

## <a name="using-media-picker"></a>メディア ピッカーの使用

`MediaPicker` クラスには、ファイルの場所を取得したり、それを `Stream` として読み取るために使用したりできる `FileResult` を返す、次のメソッドがあります。

* `PickPhotoAsync`: メディア ブラウザーを開いて、写真を選択します。
* `CapturePhotoAsync`: カメラを開いて、写真を撮影します。
* `PickVideoAsync`: メディア ブラウザーを開いて、ビデオを選択します。
* `CaptureVideoAsync`: カメラを開いて、ビデオを撮影します。

各メソッドでは、必要に応じて、ユーザーに表示される一部のオペレーティング システムで `Title` を設定できるようにする `MediaPickerOptions` パラメーターが取り込まれます。

> [!TIP]
> アクセス許可の確認と要求が Xamarin.Essentials によって自動的に処理されるため、UI スレッドではすべてのメソッドを呼び出す必要があります。

## <a name="general-usage"></a>一般的な使用法

```csharp
async Task TakePhotoAsync()
{
    try
    {
        var photo = await MediaPicker.CapturePhotoAsync();
        await LoadPhotoAsync(photo);
        Console.WriteLine($"CapturePhotoAsync COMPLETED: {PhotoPath}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"CapturePhotoAsync THREW: {ex.Message}");
    }
}

async Task LoadPhotoAsync(FileResult photo)
{
    // canceled
    if (photo == null)
    {
        PhotoPath = null;
        return;
    }
    // save the file into local storage
    var newFile = Path.Combine(FileSystem.CacheDirectory, photo.FileName);
    using (var stream = await photo.OpenReadAsync())
    using (var newStream = File.OpenWrite(newFile))
        await stream.CopyToAsync(newStream);

    PhotoPath = newFile;
}
```


## <a name="api"></a>API

- [MediaPicker ソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/MediaPicker)
- [MediaPicker API のドキュメント](xref:Xamarin.Essentials.MediaPicker)
