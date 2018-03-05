---
title: "Bing Speech API を使用する音声認識"
description: "Bing Speech API は、使用言語言語を処理するアルゴリズムを提供するクラウド ベースの API です。 この記事では、Bing 音声認識の REST API を使用してオーディオを Xamarin.Forms アプリケーション内のテキストに変換する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>Bing Speech API を使用する音声認識

_Bing Speech API は、使用言語言語を処理するアルゴリズムを提供するクラウド ベースの API です。この記事では、Bing 音声認識の REST API を使用してオーディオを Xamarin.Forms アプリケーション内のテキストに変換する方法について説明します。_

![](~/media/shared/preview.png "この API は、現在のリリース前")

> [!NOTE]
> Bing Speech API は、プレビュー段階でです。 ある可能性がある重大な変更、最終リリースする前に、API です。

## <a name="overview"></a>概要

Bing Speech API では、2 つのコンポーネントがあります。

- テキストを読み上げられる単語を変換するための音声認識 API です。 音声認識は、REST API、クライアント ライブラリ、またはサービス ライブラリを使用して実行できます。
- テキストを音声に変換するテキストを音声 API です。 REST API を使用してテキスト読み上げ変換が実行されます。

この記事は、REST API を使用して、音声認識を実行する方法について説明します。 クライアントとサービス ライブラリは、部分的な結果を返すことをサポート、中に、REST API はのみ部分的な結果なしの 1 つの認識結果を返すことができます。

API キーは、Bing 音声認識の API を使用して取得する必要があります。 これはに[作業の開始を無料](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview)microsoft.com でします。

Bing Speech API の詳細については、次を参照してください。 [Bing Speech ドキュメント](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)microsoft.com でします。

## <a name="authentication"></a>認証

Bing Speech 認識 REST API に加えられたすべての要求で認知サービス トークン サービスから取得できる JSON Web Token (JWT) アクセス トークンを必要と`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`です。 トークンのサービスを POST 要求をすることで、トークンを取得することができますを指定する、`Ocp-Apim-Subscription-Key`ヘッダーをその値として API キーが含まれています。

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

各 Bing 音声認識の REST API でアクセス トークンを指定する必要がありますとして呼び出す、`Authorization`ヘッダー文字列で始まる`Bearer`次のコード例のように。

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Bing Speech 認識 REST API に有効なアクセス トークンを渡すエラー 403 応答エラーが発生します。

## <a name="performing-speech-recognition"></a>音声認識を実行します。

音声認識はへの POST 要求をすることで実現`recognize`API`https://speech.platform.bing.com/recognize`です。 1 つの要求は、オーディオの 10 秒以上を含めることはできませんし、要求数の合計期間は 14 秒を超えることはできません。

Wav 形式で要求の POST の本文には、オーディオ コンテンツを配置する必要があります。 サポートされているコーデックについては、次を参照してください。[サポートされているコーデック](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs)microsoft.com でします。

サンプル アプリケーションで、`RecognizeSpeechAsync`メソッドは、音声認識プロセスを呼び出します。

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

、PCM の wav データとして各プラットフォームに固有のプロジェクトでオーディオを記録し、`RecognizeSpeechAsync`メソッドを使用、 `PCLStorage` NuGet パッケージをストリームとしてオーディオ ファイルを開きます。 音声認識要求 URI が生成されると、アクセス トークンは、トークンのサービスから取得されます。 音声認識の要求にポストされた、 `recognize` API で、結果を含む JSON 応答を返します。 JSON 応答の逆シリアル化、表示するための呼び出し元のメソッドに返される結果を使用します。

### <a name="configuring-speech-recognition"></a>音声認識を構成します。

音声認識プロセスは、HTTP クエリ パラメーターを指定して構成できます。 必須パラメーターと省略可能なパラメーターを設定する必要がある必須のパラメーターを示す次のメソッドがあります。

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

によって実行される主な構成、`GenerateRequestUri`方法は、オーディオ コンテンツのロケールを設定します。 サポートされているロケールの一覧は、次を参照してください。[ロケールのサポートされている](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales)microsoft.com でします。

必須のパラメーターの値の詳細については、次を参照してください。[パラメーターのために必要な](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters)microsoft.com でします。省略可能なパラメーターについては、次を参照してください。[省略可能なパラメーター](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) microsoft.com でします。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドは Bing 音声認識の REST API に POST 要求を出すし、応答を返します。

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

このメソッドで POST 要求を作成します。

- オーディオ ストリームでの折り返し、`StreamContent`ストリームに基づいて HTTP コンテンツを提供するインスタンス。
- 設定、`Content-Type`への要求のヘッダー`audio/wav; codec="audio/pcm"; samplerate=16000`です。
- アクセス トークンを追加する、`Authorization`文字列で始まるヘッダー`Bearer`です。

POST 要求に送信し、 `recognize` API です。 応答が読み取られ、呼び出し元メソッドに返されます。

`recognize` API は、要求が有効である、要求が成功したことを示すことと、要求された情報が応答で提供される、応答の HTTP ステータス コード 200 (OK) を送信します。 考えられるエラーの応答の一覧は、次を参照してください。[エラー応答](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses)microsoft.com でします。

### <a name="processing-the-response"></a>応答の処理

API の応答が認識されたテキストに含まれていることを JSON 形式で返される、`name`タグ。 次の JSON データは、標準的な成功の応答メッセージを示しています。

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

サンプル アプリケーションで JSON 応答が逆シリアル化、`SpeechResult`次のスクリーン ショットに示すように、表示、呼び出し元のメソッドに返される結果のインスタンス。

![](speech-recognition-images/speech-recognition.png "音声認識")

各 JSON タグの値については、次を参照してください。[音声認識応答](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses)microsoft.com でします。

## <a name="summary"></a>まとめ

この記事では、Bing 音声認識の REST API を使用してオーディオを Xamarin.Forms アプリケーション内のテキストに変換する方法について説明します。 音声認識機能に加え、Bing Speech API はテキストを読み上げられる単語に変換することもできます。



## <a name="related-links"></a>関連リンク

- [Bing Speech ドキュメント](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing Speech Recognition REST API](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
