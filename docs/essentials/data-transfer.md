---
title: 'Xamarin.Essentials: データの転送'
description: Xamarin.Essentials で DataTransfer クラスは、デバイス上の他のアプリケーションへのテキストと web のリンクなどのデータを共有するアプリケーションを使用できます。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c1ed298e1317d0a3f78f4dbd9fc89a2b01c6958c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855111"
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
