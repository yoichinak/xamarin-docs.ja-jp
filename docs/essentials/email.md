---
title: 'Xamarin.Essentials: 電子メール'
description: アプリケーションで Xamarin.Essentials の Email クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c2958cc4572c2f87c46c9edc5fc194284658f24
ms.sourcegitcommit: 704d4cfd418c17b0e85a20c33a16d2419db0be71
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2018
ms.locfileid: "51691751"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: 電子メール

![プレリリースの NuGet](~/media/shared/pre-release.png)

アプリケーションで **Email** クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-email"></a>Email の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

Email の機能を使用するには、電子メールに関する情報を含む `EmailMessage` を指定して `ComposeAsync` メソッドを呼び出します。

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```


## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

プラットフォームによる違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

プラットフォームによる違いはありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`Html` を送信しようとしている `BodyFormat` で `FeatureNotSupportedException` がスローされるため、`PlainText` のみをサポートします。

-----

## <a name="api"></a>API

- [Email のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email API のドキュメント](xref:Xamarin.Essentials.Email)
