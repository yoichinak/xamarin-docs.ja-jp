---
title: Face API を使用した感情認識
description: Face API は、イメージ内の顔式を入力として受け取り、イメージ内の各顔の感情のセットに対して信頼度レベルを含むデータを返します。 この記事では、Face API を使用して感情を認識し、アプリケーションを評価する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ff384605b35f6406b628da99de500b550da811c9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136059"
---
# <a name="perceived-emotion-recognition-using-the-face-api"></a>Face API を使用した感情認識

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

Face API は、人間の占めるによって認識された注釈に基づいて、顔式で怒り、軽蔑、嫌悪感、恐怖、しあわせ度、ニュートラル、悲しみ、および驚くべき感情検出を実行できます。 ただし、顔式は、必ずしも人の内部状態を表すとは限らないことに注意する必要があります。

顔式の感情の結果を返すだけでなく、検出された面の境界ボックスを返すことも Face API できます。

感情認識は、クライアントライブラリと REST API 経由で実行できます。 この記事では、REST API による感情認識の実行に焦点を当てています。 REST API の詳細については、「 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」を参照してください。

Face API を使用して、ビデオ内のユーザーの顔式を認識し、感情の概要を返すこともできます。 詳細については、「[リアルタイムでビデオを分析する方法](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)」を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

Face API を使用するには、API キーを取得する必要があります。 これは、 [Try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api)で取得できます。

Face API の詳細については、「 [Face API](/azure/cognitive-services/face/overview/)」を参照してください。

## <a name="authentication"></a>認証

Face API に対して行われるすべての要求には、ヘッダーの値として指定する必要がある API キーが必要です `Ocp-Apim-Subscription-Key` 。 次のコード例は、API キーを要求のヘッダーに追加する方法を示してい `Ocp-Apim-Subscription-Key` ます。

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Face API に有効な API キーを渡さないと、401応答エラーが発生します。

## <a name="perform-emotion-recognition"></a>感情認識を実行する

感情認識は、の api のイメージを含む POST 要求を行うことによって実行され `detect` `https://[location].api.cognitive.microsoft.com/face/v1.0` ます。ここで、 `[location]]` は api キーを取得するために使用したリージョンです。 省略可能な要求パラメーターは次のとおりです。

- `returnFaceId`–検出された顔の faceIds を返すかどうかを指定します。 既定値は `true` です。
- `returnFaceLandmarks`–検出された顔の表面の目印を返すかどうかを指定します。 既定値は `false` です。
- `returnFaceAttributes`–指定された1つ以上のフェイス属性を分析して返すかどうかを指定します。 サポートされている face 属性には、、、、、、、、、、、、、、およびがあり `age` `gender` `headPose` `smile` `facialHair` `glasses` `emotion` `hair` `makeup` `occlusion` `accessories` `blur` `exposure` `noise` ます。 Face 属性の分析には、追加の計算と時間のコストがあることに注意してください。

イメージコンテンツは、POST 要求の本文に URL またはバイナリデータとして配置する必要があります。

> [!NOTE]
> サポートされているイメージファイル形式は、JPEG、PNG、GIF、および BMP で、許可されているファイルサイズは 1 KB ~ 4MB です。

サンプルアプリケーションでは、次のメソッドを呼び出すことによって、感情認識プロセスが呼び出され `DetectAsync` ます。

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

このメソッドの呼び出しでは、イメージデータを含むストリームを指定します。これは、faceIds 返される必要があります。また、目印が返されないようにし、イメージの感情を分析する必要があることを示します。 また、結果がオブジェクトの配列として返されるように指定し `Face` ます。 次に、メソッドは、 `DetectAsync` `detect` 感情認識を実行する REST API を呼び出します。

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

このメソッドは、要求 URI を生成し、メソッドを使用して API に要求を送信し `detect` `SendRequestAsync` ます。

> [!NOTE]
> サブスクリプションキーの取得に使用したのと同じリージョンを Face API の呼び出しで使用する必要があります。 たとえば、リージョンからサブスクリプションキーを取得した場合、 `westus` 顔検出エンドポイントはになり `https://westus.api.cognitive.microsoft.com/face/v1.0/detect` ます。

### <a name="send-the-request"></a>要求を送信する

メソッドは、 `SendRequestAsync` Face API に POST 要求を行い、結果を配列として返し `Face` ます。

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

ストリームによってイメージが提供される場合、メソッドは、 `StreamContent` ストリームに基づく HTTP コンテンツを提供するインスタンスにイメージストリームをラップすることによって、POST 要求をビルドします。 また、URL を使用してイメージを指定した場合、メソッドは、 `StringContent` 文字列に基づいて HTTP コンテンツを提供するインスタンスに url をラップすることによって、POST 要求をビルドします。

POST 要求が API に送信され `detect` ます。 応答が読み取られ、逆シリアル化され、呼び出し元のメソッドに返されます。

要求が有効であり、要求された `detect` 情報が応答内にあることを示すために、API は応答で HTTP 状態コード 200 (OK) を送信します。 考えられるエラー応答の一覧については、「 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」を参照してください。

### <a name="process-the-response"></a>応答の処理

API 応答が JSON 形式で返されます。 次の JSON データは、サンプルアプリケーションによって要求されたデータを提供する、成功した一般的な応答メッセージを示しています。

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

成功した応答メッセージは、face 四角形のサイズによって降順にランク付けされた顔エントリの配列で構成され、空の応答は顔が検出されていないことを示します。 認識される各面には、メソッドの引数で指定される一連のオプションの face 属性が含まれてい `returnFaceAttributes` `DetectAsync` ます。

サンプルアプリケーションでは、JSON 応答はオブジェクトの配列に逆シリアル化され `Face` ます。 Face API の結果を解釈する場合は、スコアが1に正規化されるため、検出された感情は最高スコアの感情として解釈される必要があります。 そのため、このサンプルアプリケーションでは、イメージ内で検出された最大の顔の最高スコアで認識された感情を表示します。 これは次のコードで実現します。

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

次のスクリーンショットは、サンプルアプリケーションの感情認識プロセスの結果を示しています。

![](emotion-recognition-images/emotion-recognition.png "Emotion Recognition")

## <a name="related-links"></a>関連リンク

- [Face API](/azure/cognitive-services/face/overview/)。
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [顔 REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
