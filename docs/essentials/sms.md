---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials の SMS クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fa55a17e99a11861b98c4d515df882ed3af58a0b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102738"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![プレリリースの NuGet](~/media/shared/pre-release.png)

**SMS** クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。

## <a name="using-sms"></a>SMS の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

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
