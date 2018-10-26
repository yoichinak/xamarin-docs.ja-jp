---
title: Xamarin.iOS でビジョン フレームワーク
description: このドキュメントは、iOS 11 を使用する方法を説明します。 Xamarin.iOS でビジョン フレームワーク。 四角形の検出について具体的には、説明や、顔検出します。
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/31/2017
ms.openlocfilehash: 291cbdb93cfb6ac123d740e98065ba877bb44da5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104831"
---
# <a name="vision-framework-in-xamarinios"></a>Xamarin.iOS でビジョン フレームワーク

ビジョン フレームワークは、さまざまな処理機能を iOS 11 を含む新しいイメージを追加します。

- [四角形の検出](#rectangles)
- [顔検出](#faces)
- Machine Learning の画像の分析 (で説明した[CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- バーコードの検出
- イメージの配置の分析
- テキストの検出
- 水平線の検出
- オブジェクトの検出と追跡

![検出された 3 つの四角形を写真します。](vision-images/found-rectangles-tiny.png) ![検出された 2 つの顔を写真します。](vision-images/xamarin-home-faces-tiny.png)

四角形の検出と顔検出については、以下で詳しく説明します。

<a name="rectangles" />

## <a name="rectangle-detection"></a>四角形の検出

[VisionRects サンプル](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)イメージを処理し、検出された四角形を描画する方法を示します。

### <a name="1-initialize-the-vision-request"></a>1.ビジョンの要求を初期化します。

`ViewDidLoad`、作成、`VNDetectRectanglesRequest`を参照する、`HandleRectangles`要求ごとの最後に呼び出されるメソッド。

`MaximumObservations`プロパティも設定する必要がありますを 1 に既定では、それ以外の場合、単一の結果のみが返されます。

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2.ビジョンの処理を開始します。

次のコードでは、要求の処理を開始します。 **VisionRects**サンプルでは、ユーザーがイメージを選択した後、このコードが実行されます。

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

このハンドラーは、`ciImage`ビジョン framework`VNDetectRectanglesRequest`手順 1. で作成されました。

### <a name="3-handle-the-results-of-vision-processing"></a>3.ビジョンの処理の結果を処理します。

四角形の検出が完了すると、フレームワークは、実行、`HandleRectangles`メソッドのうちの概要を次に示します。

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4.結果を表示します

`OverlayRectangles`メソッドで、 **VisionRectangles**サンプルでは、3 つの関数。

- ソース イメージのレンダリング
- それぞれが検出されたかを示す四角形を描画し、
- CoreGraphics を使用して各四角形のテキスト ラベルを追加します。

ビュー、[サンプルのソース](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)の正確な CoreGraphics メソッド。

![検出された 3 つの四角形を写真します。](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5.さらに処理します。

四角形の検出は、多くの場合、操作のチェーンの最初の手順だけなどで[この CoreMLVision 例](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)、四角形が手書き数字を解析する CoreML モデルに渡しています。


<a name="faces" />

## <a name="face-detection"></a>顔検出

[VisionFaces サンプル](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)と同様の方法で動作、 **VisionRectangles**サンプリングには、さまざまなビジョン要求クラスを使用します。

### <a name="1-initialize-the-vision-request"></a>1.ビジョンの要求を初期化します。

`ViewDidLoad`、作成、`VNDetectFaceRectanglesRequest`を参照する、`HandleRectangles`要求ごとの最後に呼び出されるメソッド。

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2.ビジョンの処理を開始します。

次のコードでは、要求の処理を開始します。 **VisionFaces**サンプルは、ユーザーがイメージを選択した後、このコードが実行されます。

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

このハンドラーは、`ciImage`ビジョン framework`VNDetectFaceRectanglesRequest`手順 1. で作成されました。

### <a name="3-handle-the-results-of-vision-processing"></a>3.ビジョンの処理の結果を処理します。

ハンドラーが実行される顔の検出が完了すると、`HandleRectangles`エラー処理を実行し、検出された顔と呼び出しの境界を表示するメソッド、`OverlayRectangles`元の画像に外接する四角形を描画します。

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4.結果を表示します

`OverlayRectangles`メソッドで、 **VisionFaces**サンプルでは、3 つの関数。

- ソース イメージのレンダリング
- 顔が検出されると、各四角形を描画し、
- 各面 CoreGraphics を使用してテキスト ラベルを追加します。

ビュー、[サンプルのソース](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)の正確な CoreGraphics メソッド。

![検出された 2 つの顔を写真します。](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5.さらに処理します。

ビジョン フレームワークには、口目など、顔の機能を検出する追加の機能が含まれています。 使用して、`VNDetectFaceLandmarksRequest`を返しますの種類`VNFaceObservation`結果、上記の手順 3 のように、追加`VNFaceLandmark`データ。


## <a name="related-links"></a>関連リンク

- [ビジョンの四角形 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [ビジョン面 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Core のイメージのフィルター、金属、ビジョン、およびより (WWDC) (ビデオ) の進歩](https://developer.apple.com/videos/play/wwdc2017/510/)
