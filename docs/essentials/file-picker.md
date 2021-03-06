---
title: Xamarin.Essentials:ファイル ピッカー
description: Xamarin.Essentials の FilePicker クラスを利用すると、ユーザーはクラスから 1 つまたは複数のファイルを選択できます。
ms.assetid: 00bdbd57-56b1-47ca-8abe-cebe1b01f61a
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7a9701a1607333022b7356f4485f041d66b410c6
ms.sourcegitcommit: 3a68a8a48eb5a0ea7d6e0f861e2f7ecefcc49715
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2021
ms.locfileid: "98810732"
---
# <a name="no-locxamarinessentials-file-picker"></a>Xamarin.Essentials:ファイル ピッカー

**FilePicker** クラスを利用すると、ユーザーはクラスから 1 つまたは複数のファイルを選択できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**FilePicker** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

`ReadExternalStorage` アクセス許可が必要です。Android プロジェクト内で構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.ReadExternalStorage)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可]** 領域を探し、このアクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="ios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

追加の設定は必要ありません。

-----

> [!TIP]
> アクセス許可の確認と要求が Xamarin.Essentials によって自動的に処理されるため、UI スレッドではすべてのメソッドを呼び出す必要があります。

## <a name="pick-file"></a>ファイルを選択する

`FilePicker.PickAsync()` メソッドを使用すると、ユーザーはデバイスからファイルを選択できます。 このメソッドを呼び出すとき、別の `PickOptions` を指定できます。表示するタイトルやユーザーに選択を許可するファイルの種類を指定できます。 既定

```csharp
async Task<FileResult> PickAndShow(PickOptions options)
{
    try
    {
        var result = await FilePicker.PickAsync();
        if (result != null)
        {
            Text = $"File Name: {result.FileName}";
            if (result.FileName.EndsWith("jpg", StringComparison.OrdinalIgnoreCase) ||
                result.FileName.EndsWith("png", StringComparison.OrdinalIgnoreCase))
            {
                var stream = await result.OpenReadAsync();
                Image = ImageSource.FromStream(() => stream);
            }
        }
    }
    catch (Exception ex)
    {
        // The user canceled or something went wrong
    }
}
```

既定のファイルの種類には `FilePickerFileType.Images`、`FilePickerFileType.Png`、`FilePickerFilerType.Videos` が表示されます。 `PickOptions` を作成するとき、ファイルの種類としてカスタムを指定でき、プラットフォーム別にカスタマイズできます。 たとえば、ファイルの種類として comic を指定すると次のようになります。

```csharp
var customFileType =
    new FilePickerFileType(new Dictionary<DevicePlatform, IEnumerable<string>>
    {
        { DevicePlatform.iOS, new[] { "public.my.comic.extension" } }, // or general UTType values
        { DevicePlatform.Android, new[] { "application/comics" } },
        { DevicePlatform.UWP, new[] { ".cbr", ".cbz" } },
        { DevicePlatform.Tizen, new[] { "*/*" } },
        { DevicePlatform.macOS, new[] { "cbr", "cbz" } }, // or general UTType values
    });
var options = new PickOptions
{
    PickerTitle = "Please select a comic file",
    FileTypes = customFileType,
};
```

## <a name="pick-multiple-files"></a>複数のファイルを選択する

ユーザーに複数のファイルを選択してもらうなら、`FilePicker.PickMultipleAsync()` メソッドを使用できます。 追加情報を指定する目的でパラメーターとして `PickOptions` も受け取ります。 結果は `PickAsync` と同じですが、1 つの `FileResult` ではなく、繰り返すことができる `IEnumerable<FileResult>` が返されます。


## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

- 結果のファイルの URI は、再起動後に保持されない可能性があります。

# <a name="ios"></a>[iOS](#tab/ios)

プラットフォームによる違いはありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="api"></a>API

- [FilePicker ソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/FilePicker)
- [FilePicker API ドキュメント](xref:Xamarin.Essentials.FilePicker)
