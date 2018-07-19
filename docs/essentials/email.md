---
title: 'Xamarin.Essentials: 電子メール'
description: Xamarin.Essentials で電子メール クラスは、件名、本文、および受信者 (TO、CC、BCC) を含む指定した情報で、既定の電子メール アプリケーションを開くためのアプリケーションを使用できます。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848547"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: 電子メール

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**電子メール**クラスは、件名、本文、および受信者 (TO、CC、BCC) を含む指定した情報で、既定の電子メール アプリケーションを開くためのアプリケーションを使用できます。

## <a name="using-email"></a>電子メールを使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

電子メール機能を呼び出すことで機能、`ComposeAsync`メソッド、`EmailMessage`電子メールに関する情報を格納します。

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
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [電子メールのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [メール API ドキュメント](xref:Xamarin.Essentials.Email)
