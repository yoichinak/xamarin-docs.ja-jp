---
title: Translator API を使用してテキスト翻訳
description: Microsoft Translator API は、音声および REST API を介してテキスト翻訳を使用できます。 この記事では、Microsoft Translator Text API を使用して、Xamarin.Forms アプリケーションで 1 つの言語からテキストを翻訳する方法について説明します。
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 18e5b430d9a56b22a0b4cc72d6aff1c4e3049362
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61329728"
---
# <a name="text-translation-using-the-translator-api"></a>Translator API を使用してテキスト翻訳

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Microsoft Translator API は、音声および REST API を介してテキスト翻訳を使用できます。この記事では、Microsoft Translator Text API を使用して、Xamarin.Forms アプリケーションで 1 つの言語からテキストを翻訳する方法について説明します。_

## <a name="overview"></a>概要

Translator API では、2 つのコンポーネントがあります。

- テキストの翻訳 REST API を 1 つの言語からテキストを別の言語のテキストに変換します。 API は、自動的に変換する前に送信されたテキストの言語を検出します。
- 音声翻訳で REST API を別の言語のテキストに 1 つの言語から音声議事録を作成します。 API は、翻訳されたテキストを読み上げるテキストを音声機能も統合します。

この記事では、Translator Text API を使用して 1 つの言語からテキストの翻訳について説明します。

Translator Text API を使用する API キーを取得する必要があります。 これから入手できる[Microsoft Translator Text API にサインアップする方法](/azure/cognitive-services/translator/translator-text-how-to-signup/)します。

Microsoft Translator Text API の詳細については、次を参照してください。 [Translator Text API ドキュメント](/azure/cognitive-services/translator/)します。

## <a name="authentication"></a>認証

Translator Text API に加えられたすべての要求で cognitive services のトークン サービスから取得できる JSON Web トークン (JWT) アクセス トークンを必要と`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`します。 トークンのサービスに POST 要求を行うことによって、トークンを取得できますを指定する、 `Ocp-Apim-Subscription-Key` API キーとしてその値を含むヘッダー。

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

各 Translator Text API でアクセス トークンを指定する必要がありますとして呼び出す、`Authorization`ヘッダー文字列で始まる`Bearer`次のコード例のように。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Cognitive services のトークン サービスの詳細については、次を参照してください。[認証トークン API](http://docs.microsofttranslator.com/oauth-token.html)します。

## <a name="performing-text-translation"></a>テキストの翻訳を実行します。

GET 要求を送信して、テキストの翻訳を実現できます、`translate`で API を`https://api.microsofttranslator.com/v2/http.svc/translate`します。 サンプル アプリケーションで、`TranslateTextAsync`メソッドは、テキスト変換プロセスを呼び出します。

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

`TranslateTextAsync`メソッドは、要求 URI を生成し、トークン サービスからアクセス トークンを取得します。 テキストの翻訳の要求に送信し、 `translate` API で、結果を格納する XML 応答を返します。 XML 応答が解析され、呼び出し元のメソッドの表示、変換結果が返されます。

テキストの翻訳の REST Api の詳細については、次を参照してください。 [Microsoft Translator Text API](http://docs.microsofttranslator.com/text-translate.html)します。

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

このメソッドは、翻訳するテキストとテキストを翻訳する言語を設定します。 Microsoft Translator でサポートされる言語の一覧は、次を参照してください。 [Microsoft Translator Text API でサポートされる言語](/azure/cognitive-services/translator/languages/)します。

> [!NOTE]
> テキストが、どのような言語を理解する必要がある場合、`Detect`テキスト文字列の言語を検出するために API を呼び出すことができます。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドがテキストの翻訳の REST API に GET 要求を出すし、応答を返します。

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

このメソッドが GET 要求を作成するアクセス トークンを追加することで、`Authorization`文字列で始まるヘッダー`Bearer`します。 GET 要求に送信し、 `translate` API を変換するテキストを指定する要求 URL とにテキストを翻訳する言語を使用します。 応答が読み取られ、呼び出し元メソッドに返されます。

`translate` API は、要求が有効である、要求が成功したことを示すし、の要求された情報は、応答で提供される応答には、HTTP 状態コード 200 (OK) を送信します。 想定されるエラー応答の一覧は、応答メッセージを参照してください。[変換取得](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate)します。

### <a name="processing-the-response"></a>応答の処理

API 応答が XML 形式で返されます。 次の XML データは、一般的な正常な応答メッセージを示しています。

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

XML 応答の解析にサンプル アプリケーションで、`XDocument`の次のスクリーン ショットに示すように表示するため、呼び出し元メソッドに返される XML のルート値のインスタンス。

![](text-translation-images/text-translation.png "テキスト翻訳、ドイツ語")

## <a name="summary"></a>まとめ

この記事では、Microsoft Translator Text API を使用して、Xamarin.Forms アプリケーションで別の言語のテキストに 1 つの言語からテキストを変換する方法について説明します。 テキストを翻訳だけでなく、Microsoft Translator API も議事録を作成 1 つの言語からの音声を別の言語のテキストにします。

## <a name="related-links"></a>関連リンク

- [Translator Text API ドキュメント](/azure/cognitive-services/translator/)します。
- [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo Cognitive Services (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft Translator Text API](http://docs.microsofttranslator.com/text-translate.html)します。
