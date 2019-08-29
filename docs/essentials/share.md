---
title: Xamarin.Essentials:共有
description: アプリケーションで Xamarin.Essentials の Share クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.custom: video
ms.openlocfilehash: b889573c0cefbd692b1e879c1612a143d4bb7c99
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120746"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials:共有

アプリケーションで **Share** クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Share の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

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

![共有](images/share.png)

## <a name="files"></a>ファイル

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

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `Subject` プロパティは、メッセージの望ましい件名に使用されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

- `Subject` は使用されません。
- `Title` は使用されません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `Title` が設定されていない場合の既定値はアプリケーション名です。
- `Subject` は使用されません。

-----


## <a name="api"></a>API

- [Share のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share API のドキュメント](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
