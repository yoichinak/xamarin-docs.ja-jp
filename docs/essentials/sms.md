---
title: Xamarin.Essentials:SMS
description: Xamarin.Essentials の Sms クラスを使用すると、アプリケーションから既定の SMS アプリケーションを開き、受信者に送信するメッセージを指定することができます。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d981a7ed2bffbbff12cf69ee4d0cda27ce319040
ms.sourcegitcommit: 3a15d9b29d65139b18dcf0871fe00cffb2a56357
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91353383"
---
# <a name="no-locxamarinessentials-sms"></a>Xamarin.Essentials:SMS

**SMS** クラスを使用すると、受信者に送信する特定のメッセージと共に、既定の SMS アプリケーションをアプリケーションで開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Sms** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

プロジェクトのターゲット Android バージョンが **Android 11 (R API 30)** に設定される場合、新しい[パッケージの可視性要件](https://developer.android.com/preview/privacy/package-visibility)で使用されるクエリで Android マニフェストを更新する必要があります。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.VIEW" />
    <data android:scheme="smsto"/>
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="using-sms"></a>SMS の使用

クラスの Xamarin.Essentials への参照を追加します。

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

- [SMS のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Sms)
- [SMS API ドキュメント](xref:Xamarin.Essentials.Sms)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/SMS-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
