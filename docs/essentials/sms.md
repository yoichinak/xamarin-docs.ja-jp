---
title: Xamarin.Essentials:SMS
description: Xamarin.Essentials の SMS クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: a7b52bac0e9e2061cf9ff277db044ab232b1e9e5
ms.sourcegitcommit: 6e84adf7358dc05f4d888ab2674de70d88214090
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2018
ms.locfileid: "53815192"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials:SMS

**SMS** クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-sms"></a>SMS の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

SMS 機能を動作させるには、`ComposeAsync` メソッドを呼び出します。`SmsMessage` にはメッセージの受信者とメッセージの本文 (どちらも省略可能) が含まれます。

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, new []{ recipient });
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

さらに、`SmsMessage` に複数の受信者を渡すことができます。

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string[] recipients)
    {
        try
        {
            var message = new SmsMessage(messageText, recipients);
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

- [SMS のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [SMS API ドキュメント](xref:Xamarin.Essentials.Sms)
