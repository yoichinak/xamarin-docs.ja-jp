---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials で Sms クラスには、指定したメッセージの受信者に送信する既定の SMS アプリケーションを開くためのアプリケーションができるようにします。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783088"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![プレリリース NuGet](~/media/shared/pre-release.png)

**Sms**クラスには、指定したメッセージの受信者に送信する既定の SMS アプリケーションを開くためのアプリケーションができるようにします。

## <a name="using-sms"></a>Sms を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

SMS 機能が呼び出すことによって、`ComposeAsync`メソッド、`SmsMessage`メッセージの受信者と、どちらも省略可能なメッセージの本文を格納しています。

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Sms ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Sms API ドキュメント](xref:Xamarin.Essentials.Sms)
