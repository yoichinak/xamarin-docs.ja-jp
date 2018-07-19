---
title: Face API を使用して emotion 認識
description: Face API は、画像は入力としての顔の式を取得し、信頼度のレベルを含む一連の図の各面感情の間でデータを返します。 この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Face API を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049767"
---
# <a name="emotion-recognition-using-the-face-api"></a>Face API を使用して emotion 認識

_Face API は、画像は入力としての顔の式を取得し、信頼度のレベルを含む一連の図の各面感情の間でデータを返します。この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Face API を使用する方法について説明します。_

## <a name="overview"></a>概要

Face API では、顔の式で emotion 検出怒り、contempt、こう言い放ちました、心配、幸せ、ニュートラルを検出するために、悲しみ、および不測の事態を実行できます。 これらの感情は、同じ基本的な顔の式を使用してユニバーサルと数少ない伝達されます。 式の顔の感情結果を返す、だけでなく Face API こともできますを返します。 検出された面の境界ボックス。 Face API を使用して API キーを取得する必要がありますに注意してください。 これはに[認知サービスの再試行](https://azure.microsoft.com/try/cognitive-services/?api=face-api)です。

クライアント ライブラリを使用して、REST API を使用して、emotion 認識を実行できます。 この記事は、REST API を介して emotion 認識を実行する方法について説明します。 REST API の詳細については、次を参照してください。[フェイス REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)です。

Face API は、ビデオでは、ユーザーの顔の式を認識するためにも使用して、感情の概要を返すことができます。 詳細については、次を参照してください。[リアルタイムでビデオを分析する方法](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)です。

Face API の詳細については、次を参照してください。 [Face API](/azure/cognitive-services/face/overview/)です。

## <a name="authentication"></a>認証

Face API に加えられたすべての要求の値として指定する必要がある API キーが必要です、`Ocp-Apim-Subscription-Key`ヘッダー。 次のコード例は、API キーを追加する方法を示します、`Ocp-Apim-Subscription-Key`要求のヘッダー。

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Face API に有効な API キーを渡すエラー 401 応答エラーが発生します。

## <a name="performing-emotion-recognition"></a>感情認識を実行します。

感情認識は、イメージを含む POST 要求を行うことによって実行、 `detect` API`https://[location].api.cognitive.microsoft.com/face/v1.0`ここで、 `[location]]` API キーの取得に使用される領域です。 省略可能な要求のパラメーターは次のとおりです。

- `returnFaceId` – 検出された面の faceIds を返すかどうか。 既定値は `true` です。
- `returnFaceLandmarks` – 検出された面の面目印を返すかどうか。 既定値は `false` です。
- `returnFaceAttributes` – を分析し、指定された 1 つ以上を返すかどうかの属性に直面しています。 サポートされているフェイス属性には、 `age`、 `gender`、 `headPose`、 `smile`、 `facialHair`、 `glasses`、 `emotion`、 `hair`、 `makeup`、 `occlusion`、 `accessories`、 `blur`、 `exposure`、および`noise`です。 フェイス属性分析に追加の計算と時間のコストに注意してください。

イメージのコンテンツは、URL、またはバイナリ データとして POST 要求の本体に配置する必要があります。

> [!NOTE]
> サポートされているイメージ ファイルの形式は、JPEG、PNG、GIF、BMP、および、許可されているファイル サイズは 1 KB から 4 MB にします。

サンプル アプリケーションでは、emotion 認識プロセスが呼び出すことによって呼び出されます、`DetectAsync`メソッド。

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

このメソッドの呼び出しでは、faceIds 返される、フェイス目印を返すことはありませんとイメージの感情を分析する必要があります、イメージ データを含むストリームを指定します。 配列として、結果が返されることも指定`Face`オブジェクト。 さらに、`DetectAsync`メソッドを呼び出して、 `detect` emotion 認識を実行する REST API:

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

このメソッドは、要求 URI を生成し、後に要求を送信、 `detect` API を使用して、`SendRequestAsync`メソッドです。

> [!NOTE]
> サブスクリプション キーを取得するために使用すると、Face API 呼び出しでは、同じリージョンを使用する必要があります。 サブスクリプション キーを取得した場合など、`westus`領域、顔の検出エンドポイントがされます`https://westus.api.cognitive.microsoft.com/face/v1.0/detect`です。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッド Face API に POST 要求を出すし、結果として返します、`Face`配列。

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

メソッドが POST 要求で画像ストリームをラップすることによってビルド ストリームを使用して、イメージを指定する場合、`StreamContent`ストリームに基づいて HTTP コンテンツを提供するインスタンス。 代わりに、URL を使用して、イメージを指定する場合、メソッドが作成 POST 要求の URL をラップすることによって、`StringContent`インスタンスで、文字列に基く HTTP コンテンツを提供します。

POST 要求に送信し、 `detect` API です。 応答が読み取り、逆シリアル化し、呼び出し元のメソッドに返されます。

`detect` API は、要求が有効である、要求が成功したことを示すことと、要求された情報が応答で提供される、応答の HTTP ステータス コード 200 (OK) を送信します。 考えられるエラーの応答の一覧は、次を参照してください。[フェイス REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)です。

### <a name="processing-the-response"></a>応答の処理

API 応答は、JSON 形式で返されます。 次の JSON データをサンプル アプリケーションで要求されたデータを提供する標準的な成功の応答メッセージを示しています。

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

正常な応答メッセージは、降順で、空の応答に顔の検出されたことを示しないフェイス四角形のサイズで順位付けされたフェイス エントリの配列で構成されます。 フェイスには、一連によって指定される面が省略可能な属性にはが含まれています。 各認識、`returnFaceAttributes`への引数、`DetectAsync`メソッドです。

配列に、サンプル アプリケーションで JSON 応答が逆シリアル化`Face`オブジェクト。 スコアは正規化と検出された emotion、スコアが最も高い emotion として解釈するか、Face API からの結果を解釈する場合に 1 つに合計します。 そのため、サンプル アプリケーションは、イメージの最大検出された表面のスコアが最も高いと認識されている感情を表示します。 次のコードでこれを実現します。

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

次のスクリーン ショットは、サンプル アプリケーションでは感情認識プロセスの結果を示しています。

![](emotion-recognition-images/emotion-recognition.png "感情認識")

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Face API を使用する方法について説明します。 Face API は、画像は入力としての顔の式を取得し、一連の図の各面感情の間で信頼度を含むデータを返します。

## <a name="related-links"></a>関連リンク

- [API に直面](/azure/cognitive-services/face/overview/)です。
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [フェイス REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
