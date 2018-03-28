---
title: Face API を使用して emotion 認識
description: Face API は、画像は入力としての顔の式を取得し、信頼度のレベルを含む一連の図の各面感情の間でデータを返します。 この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Face API を使用する方法について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 0fc69fb1283ea2afd95900348cdecec5d6514ae0
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Face API を使用して emotion 認識

_Face API は、画像は入力としての顔の式を取得し、信頼度のレベルを含む一連の図の各面感情の間でデータを返します。この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Face API を使用する方法について説明します。_

## <a name="overview"></a>概要

Face API では、顔の式で emotion 検出怒り、contempt、こう言い放ちました、心配、幸せ、ニュートラルを検出するために、悲しみ、および不測の事態を実行できます。 これらの感情は、同じ基本的な顔の式を使用してユニバーサルと数少ない伝達されます。 式の顔の感情結果を返す、だけでなく Face API こともできますを返します。 検出された面の境界ボックス。 Face API を使用して API キーを取得する必要がありますに注意してください。 これはに[認知サービスの再試行](https://azure.microsoft.com/try/cognitive-services/?api=face-api)です。

クライアント ライブラリを使用して、REST API を使用して、emotion 認識を実行できます。 この記事でを介して emotion 認識を実行する方法について説明、 [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)クライアント ライブラリは、NuGet からダウンロードできます。

Face API は、ビデオでは、ユーザーの顔の式を認識するためにも使用して、感情の概要を返すことができます。 詳細については、次を参照してください。[リアルタイムでビデオを分析する方法](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)です。

Face API の詳細については、次を参照してください。 [Face API](/azure/cognitive-services/face/overview/)です。

## <a name="performing-emotion-recognition"></a>感情認識を実行します。

感情認識は Face api をイメージ ストリームをアップロードすることによって実現されます。 イメージ ファイルのサイズは 4 MB より大きくすることはできませんし、サポートされているファイル形式は、JPEG、PNG、GIF、BMP、します。

次のコード例は、emotion 認識プロセスを示しています。

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

`FaceServiceClient` Face API キーおよびエンドポイントへの引数として渡されると、emotion 認識を実行するインスタンスを作成する必要があります、`FaceServiceClient`コンス トラクターです。

> [!NOTE]
> サブスクリプション キーを取得するために使用すると、Face API 呼び出しでは、同じリージョンを使用する必要があります。 サブスクリプション キーを取得した場合など、`westus`領域、顔の検出エンドポイントがされます`https://westus.api.cognitive.microsoft.com/face/v1.0/detect`です。

`DetectAsync`で呼び出されるメソッド、`FaceServiceClient`インスタンス、として Face API にイメージをアップロード、`Stream`です。 API キーは、この操作が呼び出されたときに、Face API に送信されます。 有効な API キーを送信するエラーが発生、`Microsoft.ProjectOxford.Face.FaceAPIException`がスローされ、無効な API キーが送信されたことを示す例外メッセージ。

`DetectAsync`メソッドは、`Face`面が認識されている指定の配列。 フェイスには、一連のによって指定される面が省略可能な属性と組み合わせて、その場所を示す四角形が含まれています。 返された、`faceAttributes`への引数、`DetectAsync`メソッドです。 フェイスが検出されない場合、空`Face`配列が返されます。

スコアは正規化と検出された emotion、スコアが最も高い emotion として解釈するか、Face API からの結果を解釈する場合に 1 つに合計します。 したがって、サンプル アプリケーションは、次のスクリーン ショットに示すように、図では、最大検出された表面のスコアが最も高いと認識されている感情を表示します。

![](emotion-recognition-images/emotion-recognition.png "感情認識")

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Face API を使用する方法について説明します。 Face API は、画像は入力としての顔の式を取得し、一連の図の各面感情の間で信頼度を含むデータを返します。

## <a name="related-links"></a>関連リンク

- [API に直面](/azure/cognitive-services/face/overview/)です。
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
