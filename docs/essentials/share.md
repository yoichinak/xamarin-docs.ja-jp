---
title: Xamarin.Essentials:共有
description: アプリケーションで Xamarin.Essentials の Share クラスを使用すると、テキストや Web リンクなどのデータを、デバイス上の他のアプリケーションと共有できます。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b6bc8383f1c19e94c6760a213b4b9813ea77139a
ms.sourcegitcommit: 2a7bbe9cbee3727ba20ee755c1713bcfdb4d8ecb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2021
ms.locfileid: "98950959"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials:共有

アプリケーションで **Share** クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Share の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Share 機能は、他のアプリケーションと共有する情報が含まれているデータ要求ペイロードを指定して `RequestAsync` メソッドを呼び出すことにより動作します。 テキストと URI を混在させることができ、各プラットフォームはコンテンツに基づいてフィルターを処理します。

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

要求が行われたときに表示される、外部アプリケーションを共有するためのユーザー インターフェイス:

![外部アプリケーション UI に共有する](images/share.png)

## <a name="file"></a>ファイル

この機能により、アプリではデバイス上の他のアプリケーションとファイルを共有できます。 Xamarin.Essentials では、自動的にファイルの種類 (MIME) が検出されて、共有が要求されます。 各プラットフォームでは、特定のファイル拡張子のみをサポートできます。

テキストをディスクに書き込んで他のアプリと共有するサンプルを次に示します。

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## <a name="multiple-files"></a>複数のファイル

複数ファイルの共有を使用することは、一度に複数のファイルを送信できるという点のみが単一ファイルと異なります。

```csharp
var file1 = Path.Combine(FileSystem.CacheDirectory, "Attachment1.txt");
File.WriteAllText(file, "Content 1");
var file2 = Path.Combine(FileSystem.CacheDirectory, "Attachment2.txt");
File.WriteAllText(file, "Content 2");

await Share.RequestAsync(new ShareMultipleFilesRequest
{
    Title = ShareFilesTitle,
    Files = new ShareFile[] { new ShareFile(file1), new ShareFile(file2) }
});
```

## <a name="presentation-location"></a>表示の位置

[!include[](~/essentials/includes/ios-PresentationSourceBounds.md)]

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

- `Subject` プロパティは、メッセージの望ましい件名に使用されます。

# <a name="ios"></a>[iOS](#tab/ios)

- `Subject` は使用されません。
- `Title` は使用されません。

# <a name="uwp"></a>[UWP](#tab/uwp)

- `Title` が設定されていない場合の既定値はアプリケーション名です。
- `Subject` は使用されません。

-----

## <a name="api"></a>API

- [Share のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Share)
- [Share API のドキュメント](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
