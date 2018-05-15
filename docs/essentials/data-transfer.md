---
title: Xamarin.Essentials データ転送
description: DataTransfer クラスにより、アプリケーションをデバイス上の他のアプリケーションへのテキストと web リンクなどのデータを共有します。
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b03ec1330aff1210350adf2600c63d7d84bc1125
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials データ転送

![プレリリース NuGet](~/media/shared/pre-release.png)

**DataTransfer**クラスにより、アプリケーションをデバイス上の他のアプリケーションへのテキストと web リンクなどのデータを共有します。

## <a name="using-data-transfer"></a>データ転送を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

データの転送機能の動作を呼び出して、`RequestAsync`を他のアプリケーションを共有する情報が含まれるデータ要求のペイロードを持つメソッドです。 テキストと Uri を混在させることができ、各プラットフォームはコンテンツに基づくフィルタ リングを処理します。

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

要求が行われたときに表示される外部のアプリケーションを共有するユーザー インターフェイス:

![データ転送](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>プラットフォームの違い

| プラットフォーム | 相違点 |
| --- | --- |
| Android | Subject プロパティは、メッセージの件名を目的に使用されます。 |
| iOS | サブジェクトが使用されません。 |
| iOS | タイトルは使用されません。 |
| UWP | タイトルが既定のアプリケーション名に設定されていない場合。 |
| UWP | サブジェクトが使用されません。 |

## <a name="api"></a>API

- [データ転送のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [データ転送の API ドキュメント](xref:Xamarin.Essentials.DataTransfer)
