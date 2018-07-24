---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials で Sms クラスは、指定されたメッセージの受信者に送信すると、既定の SMS アプリケーションを開くためのアプリケーションを使用できます。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815598"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**Sms**クラスは、指定されたメッセージの受信者に送信すると、既定の SMS アプリケーションを開くためのアプリケーションを使用できます。

## <a name="using-sms"></a>Sms を使用します。

クラスで Xamarin.Essentials への参照を追加します。

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
