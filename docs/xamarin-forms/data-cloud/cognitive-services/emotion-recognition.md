---
title: "感情認識 Emotion API を使用します。"
description: "Emotion API は、画像は入力としての顔の式を取得し、一連の図の各面感情の間で信頼レベルを返します。 この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Emotion API を使用する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>感情認識 Emotion API を使用します。

_Emotion API は、画像は入力としての顔の式を取得し、一連の図の各面感情の間で信頼レベルを返します。この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Emotion API を使用する方法について説明します。_

![](~/media/shared/preview.png "この API は、現在のリリース前")

> [!NOTE]
> Emotion API は、プレビュー段階であります。 ある可能性がある重大な変更、最終リリースする前に、API です。

## <a name="overview"></a>概要

Emotion API では、顔の式で怒り、contempt、こう言い放ちました、心配、幸せ、neutral、悲しみ、および驚くことを検出できます。 これらの感情は、同じ基本的な顔の式を使用してユニバーサルと数少ない伝達されます。 Emotion API には、式の顔の感情結果を返すだけでなく、Face API を使用して検出された面の境界ボックスも返されます。 ユーザーが既に呼び出された場合、Face API、省略可能な入力値としてフェイス四角形を送信できます。 API キーは Emotion API を使用して取得することに注意してください。 これはに[作業の開始を無料](https://www.microsoft.com/cognitive-services/sign-up)microsoft.com でします。

クライアント ライブラリを使用して、REST API を使用して、emotion 認識を実行できます。 この記事でを介して emotion 認識を実行する方法について説明、 [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)クライアント ライブラリは、NuGet からダウンロードできます。

Emotion API は、ビデオでは、ユーザーの顔の式を認識するためにも使用して、感情の概要を返します。 詳細については、次を参照してください。[ビデオでは感情](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video)microsoft.com でします。

Emotion API の詳細については、次を参照してください。 [Emotion API ドキュメント](https://www.microsoft.com/cognitive-services/emotion-api/documentation)microsoft.com でします。

## <a name="performing-emotion-recognition"></a>感情認識を実行します。

感情認識は Emotion api をイメージ ストリームをアップロードすることによって実現されます。 イメージ ファイルのサイズは 4 MB より大きくすることはできませんし、サポートされているファイル形式は、JPEG、PNG、GIF、BMP、します。 イメージ内では、検知できる程度フェイス サイズの範囲は、36 x 36 ~ 4096 x 4096 ピクセルです。 この範囲外の任意の顔を検出しません。

次のコード例は、emotion 認識プロセスを示しています。

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

`EmotionServiceClient`への引数として渡される Emotion API キーを持つ emotion 認識を実行するインスタンスを作成する必要があります、`EmotionServiceClient`コンス トラクターです。

`RecognizeAsync`で呼び出されるメソッド、`EmotionServiceClient`インスタンス、として Emotion API にイメージをアップロード、`Stream`です。 API キーは、この操作が呼び出されたときに、Emotion API に送信されます。 有効な API キーを送信するエラーが発生、`Microsoft.ProjectOxford.Common.ClientException`がスローされ、無効な API キーが送信されたことを示す例外メッセージ。

`RecognizeAsync`メソッドは、`Emotion`面が認識されている指定の配列。 各イメージの検出できる面の最大数は 64、および面が降順でフェイス四角形のサイズで順位付けされます。 フェイスが検出されない場合、空`Emotion`配列が返されます。

スコアは正規化と検出された emotion、スコアが最も高い emotion として解釈するか、Emotion API からの結果を解釈する場合に 1 つに合計します。 したがって、サンプル アプリケーションは、次のスクリーン ショットに示すように、図では、最大検出された表面のスコアが最も高いと認識されている感情を表示します。

![](emotion-recognition-images/emotion-recognition.png "感情認識")

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションを評価する、感情を認識するように、Emotion API を使用する方法について説明します。 Emotion API は入力としてイメージに顔の式を取得し、一連の図の各面感情の間で信頼度を返します。


## <a name="related-links"></a>関連リンク

- [Emotion API ドキュメント](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [Emotion API](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
