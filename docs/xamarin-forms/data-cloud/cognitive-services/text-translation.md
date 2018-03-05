---
title: "変換 API を使用してテキストに変換"
description: "Microsoft Translator API は、音声と REST API 経由でテキスト変換に使用できます。 この記事では、1 つの言語から、Xamarin.Forms アプリケーション内の別のテキストを翻訳する Microsoft テキスト変換 API を使用する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: f403ebaffdf742c22e61b73aee7a42648fe597dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="text-translation-using-the-translator-api"></a>変換 API を使用してテキストに変換

_Microsoft Translator API は、音声と REST API 経由でテキスト変換に使用できます。この記事では、1 つの言語から、Xamarin.Forms アプリケーション内の別のテキストを翻訳する Microsoft テキスト変換 API を使用する方法について説明します。_

## <a name="overview"></a>概要

トランスレーター API では、2 つのコンポーネントがあります。

- テキスト変換で REST API を 1 つの言語からテキストを別の言語のテキストに変換します。 API は、変換する前に送信されたテキストの言語を自動的に検出します。
- 音声認識の変換を別の言語のテキストに 1 つの言語から音声議事録の作成に REST API です。 また、API には、戻る翻訳済みテキストを読み上げるために音声合成の機能が統合されています。

この記事は、1 つの言語から、テキスト変換 API を使用してテキストの翻訳について説明します。

API キーは、テキスト変換 API を使用して取得する必要があります。 これは、手順に従って、によって取得できます[作業の開始](http://docs.microsofttranslator.com/text-translate.html)で[docs.microsofttranslator.com](http://docs.microsofttranslator.com/)です。

Microsoft Translator API の詳細については、次を参照してください。 [Microsoft Translator ドキュメント](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)microsoft.com でします。

## <a name="authentication"></a>認証

テキスト変換 API に加えられたすべての要求で認知サービス トークン サービスから取得できる JSON Web Token (JWT) アクセス トークンを必要と`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`です。 トークンのサービスを POST 要求をすることで、トークンを取得することができますを指定する、`Ocp-Apim-Subscription-Key`ヘッダーをその値として API キーが含まれています。

次のコード例は、トークンのサービスからトークンのアクセスを要求する方法を示します。

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Base64 テキストは、返されたアクセス トークンは、10 分間の有効期限がします。 したがって、サンプル アプリケーションは、9 分ごと、アクセス トークンを更新します。

各テキスト変換 API でアクセス トークンを指定する必要がありますとして呼び出す、`Authorization`ヘッダー文字列で始まる`Bearer`次のコード例のように。

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

認知サービス トークン サービスに関する詳細については、次を参照してください。[認証トークン API](http://docs.microsofttranslator.com/oauth-token.html)で[docs.microsofttranslator.com](http://docs.microsofttranslator.com/)です。

## <a name="performing-text-translation"></a>テキスト変換を実行します。

テキストに変換することによって実現できますへの GET 要求を行う、 `Translate` API`https://api.microsofttranslator.com/v2/http.svc/Translate`です。 サンプル アプリケーションで、`TranslateTextAsync`メソッドは、テキスト変換プロセスを呼び出します。

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

`TranslateTextAsync`メソッドが要求 URI を生成し、トークンのサービスからアクセス トークンを取得します。 テキスト変換の要求に送信し、 `Translate` API で、結果を含む XML 応答を返します。 XML 応答が解析され、表示するための呼び出し元のメソッドに変換結果が返されます。

テキスト変換の REST Api の詳細については、次を参照してください。[サンプル コード](http://docs.microsofttranslator.com/text-translate.html#/default)で[docs.microsofttranslator.com](http://docs.microsofttranslator.com/)です。

### <a name="configuring-text-translation"></a>テキスト変換を構成します。

テキスト変換プロセスは、HTTP クエリ パラメーターを指定して構成できます。 必須パラメーターと省略可能なパラメーターを設定する必要がある必須のパラメーターを示す次のメソッドがあります。

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

このメソッドは、変換するテキストとテキストを翻訳する言語を設定します。 Microsoft Translator でサポートされる言語の一覧は、次を参照してください。[言語](https://www.microsoft.com/translator/languages.aspx)microsoft.com でします。

> [!NOTE]
> アプリケーションは、テキストをどのような言語を理解する必要がある場合、`Detect`テキスト文字列の言語を検出するために API を呼び出すことができます。

必須および省略可能なパラメーターの詳細については、次を参照してください。[テキスト変換 API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate)で[docs.microsofttranslator.com](http://docs.microsofttranslator.com/)です。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドは、テキスト変換の REST API に GET 要求を出すし、応答を返します。

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

このメソッドが GET 要求を作成するアクセス トークンを追加することによって、`Authorization`文字列で始まるヘッダー`Bearer`です。 GET 要求に送信し、 `Translate` API を変換するテキストを指定する要求の URL とテキストを翻訳する言語を使用します。 応答が読み取られ、呼び出し元メソッドに返されます。

`Translate` API は、要求が有効である、要求が成功したことを示すことと、要求された情報が応答で提供される、応答の HTTP ステータス コード 200 (OK) を送信します。 考えられるエラーの応答の一覧での応答メッセージを参照してください。[変換取得](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate)で[docs.microsofttranslator.com](http://docs.microsofttranslator.com/)です。

### <a name="processing-the-response"></a>応答の処理

API 応答が XML 形式で返されます。 次の XML データは、標準的な成功の応答メッセージを示しています。

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

XML 応答の解析にサンプル アプリケーションで、`XDocument`で次のスクリーン ショットに示すように表示するための呼び出し元のメソッドに返される XML のルート値のインスタンス。

![](text-translation-images/text-translation.png "ドイツ語にテキストに変換")

## <a name="summary"></a>まとめ

この記事では、Microsoft テキスト変換 API を使用して、1 つの言語からテキストを Xamarin.Forms アプリケーションで別の言語のテキストに変換する方法について説明します。 テキストの翻訳、に加えて Microsoft Translator API できますも議事録の作成 1 つの言語から音声認識を別の言語のテキストにします。



## <a name="related-links"></a>関連リンク

- [Microsoft Translator ドキュメント](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)
- [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Text Translation API](http://docs.microsofttranslator.com/text-translate.html)
