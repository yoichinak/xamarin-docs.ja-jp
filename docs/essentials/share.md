---
title: 'Xamarin.Essentials: Share'
description: アプリケーションで Xamarin.Essentials の Share クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 6fd3fd90d1e2ada225dafdd855f8903688677660
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899375"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials: Share

アプリケーションで **Share** クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>共有の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

共有機能は、他のアプリケーションと共有する情報が含まれているデータ要求ペイロードを指定して `RequestAsync` メソッドを呼び出すことにより動作します。 テキストと URI を混在させることができ、各プラットフォームはコンテンツに基づいてフィルターを処理します。

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

![共有](share-images/share.png)

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` プロパティは、メッセージの望ましい件名に使用されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` は使用されません。
* `Title` は使用されません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `Title` が設定されていない場合の既定値はアプリケーション名です。
* `Subject` は使用されません。

-----

## <a name="api"></a>API

- [Share のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share API のドキュメント](xref:Xamarin.Essentials.Share)
