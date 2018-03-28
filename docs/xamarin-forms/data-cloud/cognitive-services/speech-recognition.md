---
title: Microsoft Speech API を使用する音声認識
description: Microsoft Speech API は、使用言語言語を処理するアルゴリズムを提供するクラウド ベースの API です。 この記事では、Microsoft の音声認識の REST API を使用してオーディオを Xamarin.Forms アプリケーション内のテキストに変換する方法について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 2230b11f9553fb779a86d7504ed5507d2e7cbaa7
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Microsoft Speech API を使用する音声認識

_Microsoft Speech API は、使用言語言語を処理するアルゴリズムを提供するクラウド ベースの API です。この記事では、Microsoft の音声認識の REST API を使用してオーディオを Xamarin.Forms アプリケーション内のテキストに変換する方法について説明します。_

## <a name="overview"></a>概要

Microsoft Speech API では、2 つのコンポーネントがあります。

- テキストを読み上げられる単語を変換するための音声認識 API です。 音声認識は、REST API、クライアント ライブラリ、またはサービス ライブラリを使用して実行できます。
- テキストを音声に変換するテキストを音声 API です。 REST API を使用してテキスト読み上げ変換が実行されます。

この記事は、REST API を使用して、音声認識を実行する方法について説明します。 クライアントとサービス ライブラリは、部分的な結果を返すことをサポート、中に、REST API はのみ部分的な結果なしの 1 つの認識結果を返すことができます。

Microsoft Speech API を使用して API キーを取得する必要があります。 これはに[認知サービスの再試行](https://azure.microsoft.com/try/cognitive-services/)です。

Microsoft Speech API の詳細については、次を参照してください。 [Microsoft Speech API ドキュメント](/azure/cognitive-services/speech/home/)です。

## <a name="authentication"></a>認証

Microsoft Speech REST API に加えられたすべての要求で認知サービス トークン サービスから取得できる JSON Web Token (JWT) アクセス トークンを必要と`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`です。 トークンのサービスを POST 要求をすることで、トークンを取得することができますを指定する、`Ocp-Apim-Subscription-Key`ヘッダーをその値として API キーが含まれています。

次のコード例は、トークンのサービスからトークンのアクセスを要求する方法を示します。

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

Base64 テキストは、返されたアクセス トークンは、10 分間の有効期限がします。 したがって、サンプル アプリケーションは、9 分ごと、アクセス トークンを更新します。

各 Microsoft Speech REST API でアクセス トークンを指定する必要がありますとして呼び出す、`Authorization`ヘッダー文字列で始まる`Bearer`次のコード例のように。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Microsoft Speech REST API に有効なアクセス トークンを渡すエラー 403 応答エラーが発生します。

## <a name="performing-speech-recognition"></a>音声認識を実行します。

音声認識がへの POST 要求をすることで実現は、 `recognition` API`https://speech.platform.bing.com/speech/recognition/`です。 1 つの要求は、オーディオの 10 秒以上を含めることはできませんし、要求数の合計期間は 14 秒を超えることはできません。

Wav 形式で要求の POST の本文には、オーディオ コンテンツを配置する必要があります。

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
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

、PCM の wav データとして各プラットフォームに固有のプロジェクトでオーディオを記録し、`RecognizeSpeechAsync`メソッドを使用、 `PCLStorage` NuGet パッケージをストリームとしてオーディオ ファイルを開きます。 音声認識要求 URI が生成されると、アクセス トークンは、トークンのサービスから取得されます。 音声認識の要求にポストされた、 `recognition` API で、結果を含む JSON 応答を返します。 JSON 応答の逆シリアル化、表示するための呼び出し元のメソッドに返される結果を使用します。

### <a name="configuring-speech-recognition"></a>音声認識を構成します。

音声認識プロセスは、HTTP クエリ パラメーターを指定して構成できます。

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

によって実行される主な構成、`GenerateRequestUri`方法は、オーディオ コンテンツのロケールを設定します。 サポートされているロケールの一覧は、次を参照してください。[サポートされる言語](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/)します。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドの Microsoft Speech REST API に POST 要求を出すし、応答を返します。

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

このメソッドで POST 要求を作成します。

- オーディオ ストリームでの折り返し、`StreamContent`ストリームに基づいて HTTP コンテンツを提供するインスタンス。
- 設定、`Content-Type`への要求のヘッダー`audio/wav; codec="audio/pcm"; samplerate=16000`です。
- アクセス トークンを追加する、`Authorization`文字列で始まるヘッダー`Bearer`です。

POST 要求に送信し、 `recognition` API です。 応答が読み取られ、呼び出し元メソッドに返されます。

`recognition` API は、要求が有効である、要求が成功したことを示すことと、要求された情報が応答で提供される、応答の HTTP ステータス コード 200 (OK) を送信します。 考えられるエラーの応答の一覧は、次を参照してください。[トラブルシューティング](/azure/cognitive-services/speech/troubleshooting)です。

### <a name="processing-the-response"></a>応答の処理

API の応答が認識されたテキストに含まれていることを JSON 形式で返される、`name`タグ。 次の JSON データは、標準的な成功の応答メッセージを示しています。

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

サンプル アプリケーションで JSON 応答が逆シリアル化、`SpeechResult`次のスクリーン ショットに示すように、表示、呼び出し元のメソッドに返される結果のインスタンス。

![](speech-recognition-images/speech-recognition.png "音声認識")

## <a name="summary"></a>まとめ

この記事では、Microsoft Speech REST API を使用してオーディオを Xamarin.Forms アプリケーション内のテキストに変換する方法について説明します。 音声認識機能に加え、Microsoft Speech API はテキストを読み上げられる単語に変換することもできます。

## <a name="related-links"></a>関連リンク

- [Microsoft Speech API ドキュメント](/azure/cognitive-services/speech/home/)です。
- [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
