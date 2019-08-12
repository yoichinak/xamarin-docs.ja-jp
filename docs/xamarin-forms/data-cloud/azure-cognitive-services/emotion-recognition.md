---
title: Face API を使用した感情認識
description: Face API は、入力として画像の顔の式を受け取りし、一連のイメージ内の顔ごとの感情の信頼レベルを含むデータを返します。 この記事では、Face API を使用して、Xamarin.Forms アプリケーションを評価する、感情を認識する方法について説明します。
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 05dfa69a70bcd43b66cf6b572aee7d5720a81d76
ms.sourcegitcommit: 2e5a6b8bcd1a073b54604f51538fd108e1c2a8e5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/09/2019
ms.locfileid: "68869393"
---
# <a name="perceived-emotion-recognition-using-the-face-api"></a>Face API を使用した感情認識

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

Face API は、人間の占めるによって認識された注釈に基づいて、顔式で怒り、軽蔑、嫌悪感、恐怖、しあわせ度、ニュートラル、悲しみ、および驚くべき感情検出を実行できます。 ただし、顔式は、必ずしも人の内部状態を表すとは限らないことに注意する必要があります。

顔式の感情の結果を返すだけでなく、検出された面の境界ボックスを返すことも Face API できます。 Face API を使用して、API キーを取得する必要がありますに注意してください。 これから入手できる[Cognitive Services をお試しください](https://azure.microsoft.com/try/cognitive-services/?api=face-api)します。

クライアント ライブラリ、および REST API を使用して、感情認識を実行できます。 この記事では、REST API 経由での感情認識を実行する方法について説明します。 REST API の詳細については、次を参照してください。 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)します。

Face API では、ビデオでは、人物の表情を認識するためにも使用して、感情の概要を返すことができます。 詳細については、次を参照してください。[リアルタイムでビデオを分析する方法](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)します。

Face API の詳細については、次を参照してください。 [Face API](/azure/cognitive-services/face/overview/)します。

## <a name="authentication"></a>認証

Face API に加えられたすべての要求が API キーの値として指定する必要があります、`Ocp-Apim-Subscription-Key`ヘッダー。 次のコード例は、API キーを追加する方法を示しています、`Ocp-Apim-Subscription-Key`要求のヘッダー。

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Face API に有効な API キーを渡すの失敗は、401 の応答、エラーが発生されます。

## <a name="perform-emotion-recognition"></a>感情認識を実行する

イメージを含む POST 要求を行って感情の認識が実行される、`detect`で API を`https://[location].api.cognitive.microsoft.com/face/v1.0`ここで、`[location]]`は API キーを取得するために使用する領域です。 省略可能な要求のパラメーターは次のとおりです。

- `returnFaceId` – 検出された顔の faceIds を返すかどうか。 既定値は `true` です。
- `returnFaceLandmarks` – 検出された顔の顔の目印を返すかどうか。 既定値は `false` です。
- `returnFaceAttributes` – 属性が顔を分析し、指定された 1 つ以上を返すかどうか。 サポートされている顔の特徴を含める`age`、 `gender`、 `headPose`、 `smile`、 `facialHair`、 `glasses`、 `emotion`、 `hair`、 `makeup`、 `occlusion`、 `accessories`、 `blur`、 `exposure`、および`noise`します。 顔属性の分析に追加のコンピューティングおよび時間コストに注意してください。

イメージのコンテンツは、URL、またはバイナリ データとして POST 要求の本体に配置する必要があります。

> [!NOTE]
> サポートされているイメージ ファイル形式は JPEG、PNG、GIF、BMP、および、許可されているファイルのサイズが 1 KB から 4 MB には.

サンプル アプリケーションでは、感情の認識プロセスが呼び出すことによって呼び出される、`DetectAsync`メソッド。

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

このメソッドの呼び出しでは、顔を返すべきではないことと、イメージの感情を分析すること、faceIds を返す必要が、イメージ データを含むストリームを指定します。 配列として、結果が返されることも指定`Face`オブジェクト。 さらに、`DetectAsync`メソッドを呼び出す、`detect`感情認識を実行する REST API:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

このメソッドは要求 URI を生成しする要求を送信し、 `detect` API を使用して、`SendRequestAsync`メソッド。

> [!NOTE]
> サブスクリプション キーを取得するために使用すると、Face API の呼び出しでは、同じリージョンを使用する必要があります。 サブスクリプション キーを取得した場合など、`westus`リージョン、顔の検出エンドポイントになります`https://westus.api.cognitive.microsoft.com/face/v1.0/detect`します。

### <a name="send-the-request"></a>要求を送信する

`SendRequestAsync`メソッド Face API に POST 要求を出すし、その結果、`Face`配列。

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

イメージのストリームをラップすることによってメソッドが POST 要求をビルド イメージは、ストリームを使用して指定した場合、`StreamContent`インスタンスで、ストリームに基づく HTTP コンテンツを提供します。 代わりに、イメージは、URL 経由で指定した場合、メソッドが構築 POST 要求で URL をラップすることによって、`StringContent`インスタンスで、文字列に基づく HTTP コンテンツを提供します。

POST 要求に送信し、 `detect` API。 応答は、読み取られ、逆シリアル化、呼び出し元のメソッドが返されます。

`detect` API は、要求が有効である、要求が成功したことを示すし、の要求された情報は、応答で提供される応答には、HTTP 状態コード 200 (OK) を送信します。 想定されるエラー応答の一覧は、次を参照してください。 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)します。

### <a name="process-the-response"></a>応答を処理する

API 応答は JSON 形式で返されます。 次の JSON データには、サンプル アプリケーションによって要求されたデータを提供する標準的な成功の応答メッセージが表示されます。

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

正常な応答メッセージは、降順に並べ替え、空の応答が検出された顔を示しません間での顔四角形のサイズによってランク付けされた顔エントリの配列で構成されます。 顔には、一連省略可能な顔の特徴は、によって指定されますにはが含まれています。 各認識、`returnFaceAttributes`への引数、`DetectAsync`メソッド。

配列にサンプル アプリケーションで JSON 応答が逆シリアル化`Face`オブジェクト。 スコアが正規化されるように、検出された感情をスコアが最も高い emotion として解釈する Face API の結果を解釈するときに 1 つを合計します。 そのため、サンプル アプリケーションでは、画像のスコアが最も高い、最大の検出された顔の認識された感情が表示されます。 これは、次のコードで実現されます。

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

次のスクリーン ショットは、サンプル アプリケーションでは、感情の認識プロセスの結果を示しています。

![](emotion-recognition-images/emotion-recognition.png "感情認識")

## <a name="related-links"></a>関連リンク

- [Face API](/azure/cognitive-services/face/overview/)します。
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Face API の REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
