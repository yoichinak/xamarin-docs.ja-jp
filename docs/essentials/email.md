---
title: 'Xamarin.Essentials: 電子メール'
description: アプリケーションで Xamarin.Essentials の Email クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d7d2536fca32fe3ae9f9692031645c42edb4ea61
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898666"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: 電子メール

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

Android の一部のメール クライアントは `Html` を検出する手段がなく、これをサポートしていないため、メールを送信するときは `PlainText` を使用することをお勧めします。

# <a name="iostabios"></a>[iOS](#tab/ios)

プラットフォームによる違いはありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`Html` を送信しようとしている `BodyFormat` で `FeatureNotSupportedException` がスローされるため、`PlainText` のみをサポートします。

-----

## <a name="api"></a>API

- [Email のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email API のドキュメント](xref:Xamarin.Essentials.Email)
