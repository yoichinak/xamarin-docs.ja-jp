---
title: 'Xamarin.Essentials: データの転送'
description: Xamarin.Essentials で DataTransfer クラスは、デバイス上の他のアプリケーションへのテキストと web のリンクなどのデータを共有するアプリケーションを使用できます。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 31e27556a6681b144084d2177cf3fde8fe8e5459
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353520"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: データの転送

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**DataTransfer**クラスは、デバイス上の他のアプリケーションにテキストと web のリンクなどのデータを共有するアプリケーションを使用できます。

## <a name="using-data-transfer"></a>データ転送を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

データ転送の機能を呼び出すことで機能、`RequestAsync`メソッド データを要求ペイロードであり、他のアプリケーションを共有する情報が含まれています。 テキストと Uri を混在させることができ、各プラットフォームはコンテンツに基づくフィルタ リングを処理します。

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

要求が行われたときに表示される外部のアプリケーションに共有するユーザー インターフェイス:

![データ転送](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>プラットフォームの違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` プロパティは、メッセージの件名を目的に使用されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` 使用されません。
* `Title` 使用されません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `Title` 既定のアプリケーション名に設定されていない場合。
* `Subject` 使用されません。

-----

## <a name="api"></a>API

- [データ転送のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [データ転送の API のドキュメント](xref:Xamarin.Essentials.DataTransfer)
