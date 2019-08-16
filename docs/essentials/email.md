---
title: Xamarin.Essentials:電子メール
description: アプリケーションで Xamarin.Essentials の Email クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: f2c275260625fe3842b4473e404f49c71d1d28ae
ms.sourcegitcommit: 9f37dc00c2adab958025ad1cdba9c37f0acbccd0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2019
ms.locfileid: "69012485"
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

![プレビュー機能](~/media/shared/preview.png)

ファイルのメール送信は、Xamarin.Essentials バージョン 1.1.0 で実験的プレビューとして利用できます。 この機能により、アプリではデバイスのメール クライアントでファイルをメール送信できます。 この機能を有効にするには、アプリのスタートアップ コードで次のプロパティを設定します。

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.EmailAttachments);
```

機能を有効にした後は、すべてのファイルをメールで送信できます。 Xamarin.Essentials では、自動的にファイルの種類 (MIME) が検出されて、ファイルを添付として追加することが要求されます。 すべてのメール クライアントは異なり、特定のファイル拡張子のみをサポートできるか、またはまったくサポートできません。

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

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android の一部のメール クライアントは `Html` を検出する手段がなく、これをサポートしていないため、メールを送信するときは `PlainText` を使用することをお勧めします。

# <a name="iostabios"></a>[iOS](#tab/ios)

プラットフォームによる違いはありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`Html` を送信しようとしている `BodyFormat` で `FeatureNotSupportedException` がスローされるため、`PlainText` のみをサポートします。

すべての電子メール クライアントが添付ファイルの送信をサポートするわけではありません。 詳細については、[ドキュメント](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email)をご覧ください。

-----

## <a name="api"></a>API

- [Email のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email API のドキュメント](xref:Xamarin.Essentials.Email)
