---
title: Translator API を使用したテキスト翻訳
description: Microsoft Translator API を使用すると、REST API を通じて音声とテキストを変換できます。 この記事では、Microsoft Translator Text API を使用して、Xamarin. フォームアプリケーションでテキストをある言語から別の言語に変換する方法について説明します。
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 50d13532585e6edc3dac530558937ee6e0a02268
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032797"
---
# <a name="text-translation-using-the-translator-api"></a>Translator API を使用したテキスト翻訳

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Translator API を使用すると、REST API を通じて音声とテキストを変換できます。この記事では、Microsoft Translator Text API を使用して、Xamarin. フォームアプリケーションでテキストをある言語から別の言語に変換する方法について説明します。_

## <a name="overview"></a>概要

Translator API には、次の2つのコンポーネントがあります。

- テキスト変換は、ある言語のテキストを別の言語のテキストに変換するために REST API します。 API は、翻訳前に送信されたテキストの言語を自動的に検出します。
- 音声を翻訳して、ある言語から別の言語のテキストに音声を REST API します。 また、この API は、翻訳されたテキストを読み上げるようにテキスト読み上げ機能を統合しています。

この記事では、Translator Text API を使用して、ある言語から別の言語にテキストを変換する方法について説明します。

Translator Text API を使用するには、API キーを取得する必要があります。 これは[、「Microsoft Translator Text API にサインアップする方法](/azure/cognitive-services/translator/translator-text-how-to-signup/)」で入手できます。

Microsoft Translator Text API の詳細については、 [Translator Text API のドキュメント](/azure/cognitive-services/translator/)を参照してください。

## <a name="authentication"></a>認証

Translator Text API に対して行われるすべての要求には、JSON Web トークン (JWT) アクセストークンが必要です。これは、`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`で認知サービストークンサービスから取得できます。 トークンを取得するには、トークンサービスに POST 要求を作成し、その値として API キーを含む `Ocp-Apim-Subscription-Key` ヘッダーを指定します。

次のコード例は、トークンサービスからアクセストークンを要求する方法を示しています。

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

返されるアクセストークンの Base64 テキストは、有効期限が10分です。 このため、サンプルアプリケーションでは、アクセストークンが9分ごとに更新されます。

次のコード例に示すように、アクセストークンは、各 Translator Text API 呼び出しで、文字列 `Bearer`で始まる `Authorization` ヘッダーとして指定する必要があります。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

認知サービストークンサービスの詳細については、「 [Authentication TOKEN API](https://docs.microsofttranslator.com/oauth-token.html)」を参照してください。

## <a name="performing-text-translation"></a>テキスト変換の実行

テキスト変換は、`https://api.microsofttranslator.com/v2/http.svc/translate`で `translate` API に対して GET 要求を行うことによって実現できます。 サンプルアプリケーションでは、`TranslateTextAsync` メソッドによってテキスト変換プロセスが呼び出されます。

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync` メソッドは、要求 URI を生成し、トークンサービスからアクセストークンを取得します。 次に、テキスト変換要求が `translate` API に送信されます。これにより、結果を含む XML 応答が返されます。 XML 応答が解析され、変換結果が表示のために呼び出し元のメソッドに返されます。

テキスト変換 REST Api の詳細については、「 [Microsoft Translator Text API](https://docs.microsofttranslator.com/text-translate.html)」を参照してください。

### <a name="configuring-text-translation"></a>テキスト変換の構成

テキスト変換プロセスは、HTTP クエリパラメーターを指定することによって構成できます。

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

このメソッドは、翻訳されるテキストと、テキストを変換する言語を設定します。 Microsoft Translator でサポートされている言語の一覧については、「 [microsoft Translator Text API でサポートされる言語](/azure/cognitive-services/translator/languages/)」を参照してください。

> [!NOTE]
> テキストが含まれている言語をアプリケーションで認識する必要がある場合は、テキスト文字列の言語を検出するために `Detect` API を呼び出すことができます。

### <a name="sending-the-request"></a>要求を送信しています

`SendRequestAsync` メソッドは、テキスト変換 REST API に GET 要求を行い、応答を返します。

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

このメソッドは、アクセストークンを `Authorization` ヘッダーに追加することによって GET 要求をビルドします。プレフィックスは文字列 `Bearer`です。 次に、GET 要求が `translate` API に送信されます。要求 URL には翻訳するテキストを指定し、テキストを変換する言語を指定します。 次に、応答が読み取られ、呼び出し元のメソッドに返されます。

要求が成功し、要求された情報が応答内にあることを示す場合、`translate` API は応答で HTTP 状態コード 200 (OK) を送信します。 考えられるエラー応答の一覧については、「[翻訳の取得](https://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate)」にある応答メッセージを参照してください。

### <a name="processing-the-response"></a>応答の処理

API 応答は XML 形式で返されます。 次の XML データは、一般的な成功応答メッセージを示しています。

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

このサンプルアプリケーションでは、XML 応答が `XDocument` インスタンスに解析され、次のスクリーンショットに示すように、呼び出し元のメソッドに XML ルート値が返されて表示されます。

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>まとめ

この記事では、Microsoft Translator Text API を使用して、1つの言語のテキストを Xamarin. フォームアプリケーションで別の言語のテキストに変換する方法について説明しました。 Microsoft Translator API は、テキストを翻訳するだけでなく、1つの言語から別の言語のテキストへの音声の議事録を行うこともできます。

## <a name="related-links"></a>関連リンク

- [Translator Text API ドキュメント](/azure/cognitive-services/translator/)。
- [RESTful Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Microsoft Translator Text API](https://docs.microsofttranslator.com/text-translate.html)。
