---
title: 'Xamarin.Essentials: データ転送'
description: アプリケーションで Xamarin.Essentials の DataTransfer クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 179d4327aa768e7aa2c81dbbffd694d078327400
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674835"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: データ転送

![プレリリースの NuGet](~/media/shared/pre-release.png)

アプリケーションで **DataTransfer** クラスを使用すると、デバイス上の他のアプリケーションとテキストや Web リンクなどのデータを共有できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-data-transfer"></a>DataTransfer の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

データ転送機能は、他のアプリケーションと共有する情報が含まれているデータ要求ペイロードを指定して `RequestAsync` メソッドを呼び出すことにより動作します。 テキストと URI を混在させることができ、各プラットフォームはコンテンツに基づいてフィルターを処理します。

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

要求が行われたときに表示される、外部アプリケーションを共有するためのユーザー インターフェイス:

![データ転送](data-transfer-images/data-transfer.png)

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

- [DataTransfer のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [DataTransfer API のドキュメント](xref:Xamarin.Essentials.DataTransfer)
