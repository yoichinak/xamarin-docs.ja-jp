---
title: Translator API を使用してテキスト翻訳
description: Microsoft Translator API は、音声および REST API を介してテキスト翻訳を使用できます。 この記事では、Microsoft Translator Text API を使用して、Xamarin.Forms アプリケーションで 1 つの言語からテキストを翻訳する方法について説明します。
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 841b1d4abab5e4c09249174b221da20794771a86
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725569"
---
# <a name="text-translation-using-the-translator-api"></a>Translator API を使用してテキスト翻訳

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Translator API を使用すると、REST API を通じて音声とテキストを変換できます。この記事では、Microsoft Translator Text API を使用して、Xamarin. フォームアプリケーションでテキストをある言語から別の言語に変換する方法について説明します。_

## <a name="overview"></a>概要

Translator API では、2 つのコンポーネントがあります。

- テキストの翻訳 REST API を 1 つの言語からテキストを別の言語のテキストに変換します。 API は、自動的に変換する前に送信されたテキストの言語を検出します。
- 音声翻訳で REST API を別の言語のテキストに 1 つの言語から音声議事録を作成します。 この API では、翻訳されたテキストを読み上げるテキスト読み上げ機能も統合されています。

この記事では、Translator Text API を使用して 1 つの言語からテキストの翻訳について説明します。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

Translator Text API を使用する API キーを取得する必要があります。 これは[、「Microsoft Translator Text API にサインアップする方法](/azure/cognitive-services/translator/translator-text-how-to-signup/)」で入手できます。

Microsoft Translator Text API の詳細については、 [Translator Text API のドキュメント](/azure/cognitive-services/translator/)を参照してください。

## <a name="authentication"></a>認証

Translator Text API に対して行われるすべての要求には、JSON Web トークン (JWT) アクセストークンが必要です。これは、`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`で認知サービストークンサービスから取得できます。 トークンを取得するには、トークンサービスに POST 要求を作成し、その値として API キーを含む `Ocp-Apim-Subscription-Key` ヘッダーを指定します。

次のコード例は、トークン、トークン サービスからのアクセスを要求する方法を示します。

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

Base64 テキストには、返されたアクセス トークンが 10 分間の有効期限です。 そのため、サンプル アプリケーションは、9 分ごと、アクセス トークンを更新します。

次のコード例に示すように、アクセストークンは、各 Translator Text API 呼び出しで、文字列 `Bearer`で始まる `Authorization` ヘッダーとして指定する必要があります。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

認知サービストークンサービスの詳細については、「 [Authentication](/azure/cognitive-services/translator/reference/v3-0-reference#authentication)」を参照してください。

## <a name="performing-text-translation"></a>テキストの翻訳を実行します。

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

`TranslateTextAsync` メソッドは、要求 URI を生成し、トークンサービスからアクセストークンを取得します。 次に、テキスト変換要求が `translate` API に送信されます。これにより、結果を含む XML 応答が返されます。 XML 応答が解析され、呼び出し元のメソッドの表示、変換結果が返されます。

テキスト変換 REST Api の詳細については、「 [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)」を参照してください。

### <a name="configuring-text-translation"></a>テキストの翻訳を構成します。

テキストの翻訳プロセスは、HTTP クエリ パラメーターを指定して構成できます。

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

このメソッドは、翻訳するテキストとテキストを翻訳する言語を設定します。 Microsoft Translator でサポートされている言語の一覧については、「 [microsoft Translator Text API でサポートされる言語](/azure/cognitive-services/translator/languages/)」を参照してください。

> [!NOTE]
> テキストが含まれている言語をアプリケーションで認識する必要がある場合は、テキスト文字列の言語を検出するために `Detect` API を呼び出すことができます。

### <a name="sending-the-request"></a>要求を送信します。

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

このメソッドは、アクセストークンを `Authorization` ヘッダーに追加することによって GET 要求をビルドします。プレフィックスは文字列 `Bearer`です。 次に、GET 要求が `translate` API に送信されます。要求 URL には翻訳するテキストを指定し、テキストを変換する言語を指定します。 応答が読み取られ、呼び出し元メソッドに返されます。

要求が成功し、要求された情報が応答内にあることを示す場合、`translate` API は応答で HTTP 状態コード 200 (OK) を送信します。 考えられるエラー応答の一覧については、「[翻訳の取得](/azure/cognitive-services/translator/reference/v3-0-translate)」にある応答メッセージを参照してください。

### <a name="processing-the-response"></a>応答の処理

API 応答が XML 形式で返されます。 次の XML データは、一般的な正常な応答メッセージを示しています。

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

このサンプルアプリケーションでは、XML 応答が `XDocument` インスタンスに解析され、次のスクリーンショットに示すように、呼び出し元のメソッドに XML ルート値が返されて表示されます。

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>まとめ

この記事では、Microsoft Translator Text API を使用して、Xamarin.Forms アプリケーションで別の言語のテキストに 1 つの言語からテキストを変換する方法について説明します。 テキストを翻訳だけでなく、Microsoft Translator API も議事録を作成 1 つの言語からの音声を別の言語のテキストにします。

## <a name="related-links"></a>関連リンク

- [Translator Text API のドキュメント](/azure/cognitive-services/translator/)
- [RESTful Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)
