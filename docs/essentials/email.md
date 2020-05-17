---
title: Xamarin.Essentials:電子メール
description: アプリケーションで Xamarin.Essentials の Email クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: 77fcadf3ec58a38acac5eca14b43d937414a4a60
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83150095"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials:電子メール

アプリケーションで **Email** クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> iOS で Email API を使用するには、物理デバイスで実行されている必要があります。それ以外の場合、例外がスローされます。

## <a name="using-email"></a>Email の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

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

## <a name="file-attachments"></a>添付ファイル

この機能により、アプリではデバイスのメール クライアントでファイルをメール送信できます。 Xamarin.Essentials では、自動的にファイルの種類 (MIME) が検出されて、ファイルを添付として追加することが要求されます。 メール クライアントはそれぞれ異なるものであり、特定のファイル拡張子のみをサポートできるか、またはいずれもサポートできません。

テキストをディスクに書き込み、メールの添付ファイルとして追加するサンプルを次に示します。

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

Android の一部のメール クライアントは `Html` を検出する手段がなく、これをサポートしていないため、メールを送信するときは `PlainText` を使用することをお勧めします。

# <a name="ios"></a>[iOS](#tab/ios)

プラットフォームによる違いはありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

`Html` を送信しようとしている `BodyFormat` で `FeatureNotSupportedException` がスローされるため、`PlainText` のみをサポートします。

すべての電子メール クライアントが添付ファイルの送信をサポートするわけではありません。 詳細については、[ドキュメント](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email)をご覧ください。

-----

## <a name="api"></a>API

- [Email のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email API のドキュメント](xref:Xamarin.Essentials.Email)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Email-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
