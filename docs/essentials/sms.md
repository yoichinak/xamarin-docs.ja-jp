---
title: Xamarin.Essentials:SMS
description: Xamarin.Essentials の SMS クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 38651b8e29a2119f777fdbcdd82c9a8277c02497
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149910"
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

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/SMS-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
