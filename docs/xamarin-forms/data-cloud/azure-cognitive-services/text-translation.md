---
title: "Translator API を使用したテキスト変換" 説明: "Microsoft Translator API を使用して、音声とテキストを REST API で変換することができます。 この記事では、Microsoft Translator Text API を使用して、アプリケーションでテキストをある言語から別の言語に変換する方法について説明し Xamarin.Forms ます。 "
ms. 製品: xamarin ms. assetid: 823: davidbritch: dabritch ms. date: 02/08/2017 のように指定しない場合は、xamarin-forms author: ms. author:: のように指定していないことを示します。 Xamarin.Forms Xamarin.Essentials
---

# <a name="text-translation-using-the-translator-api"></a>Translator API を使用したテキスト翻訳

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Translator API を使用すると、REST API を通じて音声とテキストを変換できます。この記事では、Microsoft Translator Text API を使用して、アプリケーションでテキストをある言語から別の言語に変換する方法について説明し Xamarin.Forms ます。_

## <a name="overview"></a>概要

Translator API には、次の2つのコンポーネントがあります。

- テキスト変換は、ある言語のテキストを別の言語のテキストに変換するために REST API します。 API は、翻訳前に送信されたテキストの言語を自動的に検出します。
- 音声を翻訳して、ある言語から別の言語のテキストに音声を REST API します。 この API では、翻訳されたテキストを読み上げるテキスト読み上げ機能も統合されています。

この記事では、Translator Text API を使用して、ある言語から別の言語にテキストを変換する方法について説明します。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

Translator Text API を使用するには、API キーを取得する必要があります。 これは[、「Microsoft Translator Text API にサインアップする方法](/azure/cognitive-services/translator/translator-text-how-to-signup/)」で入手できます。

Microsoft Translator Text API の詳細については、 [Translator Text API のドキュメント](/azure/cognitive-services/translator/)を参照してください。

## <a name="authentication"></a>認証

Translator Text API に対して行われるすべての要求には、JSON Web トークン (JWT) アクセストークンが必要です。これは、の認知サービストークンサービスから取得でき `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` ます。 トークンを取得するには、トークンサービスに POST 要求を作成し、 `Ocp-Apim-Subscription-Key` その値として API キーを含むヘッダーを指定します。

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

`Authorization` `Bearer` 次のコード例に示すように、アクセストークンは、文字列をプレフィックスとしたヘッダーとして、各 Translator Text API 呼び出しで指定する必要があります。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

認知サービストークンサービスの詳細については、「 [Authentication](/azure/cognitive-services/translator/reference/v3-0-reference#authentication)」を参照してください。

## <a name="performing-text-translation"></a>テキスト変換の実行

テキスト変換は、で API に対する GET 要求を行うことで実現でき `translate` `https://api.microsofttranslator.com/v2/http.svc/translate` ます。 サンプルアプリケーションでは、 `TranslateTextAsync` メソッドはテキスト変換プロセスを呼び出します。

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

メソッドは、 `TranslateTextAsync` 要求 URI を生成し、トークンサービスからアクセストークンを取得します。 次に、テキスト変換要求が API に送信され `translate` 、その結果を含む XML 応答が返されます。 XML 応答が解析され、変換結果が表示のために呼び出し元のメソッドに返されます。

テキスト変換 REST Api の詳細については、「 [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)」を参照してください。

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
> アプリケーションがテキストの言語を認識している必要がある場合は、API を呼び出して、 `Detect` テキスト文字列の言語を検出することができます。

### <a name="sending-the-request"></a>要求を送信しています

メソッドは、 `SendRequestAsync` テキスト変換に GET 要求を REST API し、応答を返します。

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

このメソッドは、プレフィックスが文字列であるヘッダーにアクセストークンを追加することによって、GET 要求をビルドし `Authorization` `Bearer` ます。 次に、GET 要求が API に送信され `translate` ます。要求 URL には翻訳するテキストを指定し、テキストを変換する言語を指定します。 次に、応答が読み取られ、呼び出し元のメソッドに返されます。

要求が有効であり、要求された `translate` 情報が応答内にあることを示すために、API は応答で HTTP 状態コード 200 (OK) を送信します。 考えられるエラー応答の一覧については、「[翻訳の取得](/azure/cognitive-services/translator/reference/v3-0-translate)」にある応答メッセージを参照してください。

### <a name="processing-the-response"></a>応答の処理

API 応答は XML 形式で返されます。 次の XML データは、一般的な成功応答メッセージを示しています。

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

サンプルアプリケーションでは、XML 応答がインスタンスに解析され、 `XDocument` 次のスクリーンショットに示すように、xml ルート値が表示のために呼び出し元のメソッドに返されます。

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>まとめ

この記事では、Microsoft Translator Text API を使用して、ある言語のテキストをアプリケーション内の別の言語のテキストに変換する方法について説明しました Xamarin.Forms 。 Microsoft Translator API は、テキストを翻訳するだけでなく、1つの言語から別の言語のテキストへの音声の議事録を行うこともできます。

## <a name="related-links"></a>関連リンク

- [Translator Text API のドキュメント](/azure/cognitive-services/translator/)
- [RESTful Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)
