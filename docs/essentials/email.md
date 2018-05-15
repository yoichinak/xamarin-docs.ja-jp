---
title: Xamarin.Essentials 電子メール
description: 電子メールのクラスには、件名、本文、および受信者 (TO、CC、BCC) を含む指定した情報と、既定の電子メール アプリケーションを開くためのアプリケーションができるようにします。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: dde3f7f160d796afe3184ef1d61d8b437ec09ea8
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials 電子メール

![プレリリース NuGet](~/media/shared/pre-release.png)

**電子メール**クラスには、件名、本文、および受信者 (TO、CC、BCC) を含む指定した情報と、既定の電子メール アプリケーションを開くためのアプリケーションができるようにします。

## <a name="using-email"></a>電子メールを使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

電子メール機能が呼び出すことによって、`ComposeAsync`メソッド、`EmailMessage`電子メールに関する情報を格納します。

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string, body, List<string> recipients)
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
            }
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Sms is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [電子メールのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [電子メール API ドキュメント](xref:Xamarin.Essentials.Email)
