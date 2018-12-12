---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials の SMS クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d0655f3e687750e0fca626fb017096a946c0abb3
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898772"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

**SMS** クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

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
