---
title: Xamarin.Essentials:電子メール
description: アプリケーションで Xamarin.Essentials の Email クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 059405d4e3219162022b3f8c0208ee5cc4ac2d38
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434541"
---
# <a name="no-locxamarinessentials-email"></a>Xamarin.Essentials:電子メール

アプリケーションで **Email** クラスを使用すると、件名、本文、受信者 (TO、CC、BCC) などの情報を指定して既定のメール アプリケーションを開くことができます。

**Email** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

プロジェクトのターゲット Android バージョンが **Android 11 (R API 30)** に設定される場合、新しい[パッケージの可視性要件](https://developer.android.com/preview/privacy/package-visibility)で使用されるクエリで Android マニフェストを更新する必要があります。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.SENDTO" />
    <data android:scheme="mailto" />
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> iOS で Email API を使用するには、物理デバイスで実行されている必要があります。それ以外の場合、例外がスローされます。

## <a name="using-email"></a>Email の使用

クラスの Xamarin.Essentials への参照を追加します。

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

この機能により、アプリではデバイスのメール クライアントでファイルをメール送信できます。 Xamarin.Essentials では、ファイルの種類 (MIME) が自動的に検出されて、ファイルを添付ファイルとして追加することが要求されます。 メール クライアントはそれぞれ異なるものであり、特定のファイル拡張子のみをサポートできるか、またはいずれもサポートできません。

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

すべての電子メール クライアントが添付ファイルの送信をサポートするわけではありません。 詳細については、[ドキュメント](/windows/uwp/contacts-and-calendar/sending-email)をご覧ください。

-----

## <a name="api"></a>API

- [Email のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Email)
- [Email API のドキュメント](xref:Xamarin.Essentials.Email)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Email-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]