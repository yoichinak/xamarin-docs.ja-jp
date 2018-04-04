---
title: ビジョン フレームワーク
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: 698bf829128cff1263e98b49d29a77b75ec32ad9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="vision-framework"></a>ビジョン フレームワーク

ビジョン フレームワークは、新しいイメージが iOS を含む 11 の機能を処理の数を追加します。

- [四角形の検出](#rectangles)
- [顔の検出](#faces)
- 機械学習画像分析 (で説明した[CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- バーコードの検出
- イメージの配置の分析
- テキストの検出
- 水平線の検出
- オブジェクトの検出および追跡

![検出された 3 つの四角形を写真します。](vision-images/found-rectangles-tiny.png) ![検出された 2 つの面での写真します。](vision-images/xamarin-home-faces-tiny.png)

四角形の検出と顔の検出で詳しく説明します。

<a name="rectangles" />

## <a name="rectangle-detection"></a>四角形の検出

[VisionRects サンプル](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)イメージを処理し、それに検出された四角形を描画する方法を示します。

### <a name="1-initialize-the-vision-request"></a>1.ビジョン要求を初期化します。

`ViewDidLoad`、作成、`VNDetectRectanglesRequest`を参照する、`HandleRectangles`要求ごとの最後に呼び出されるメソッド。

`MaximumObservations`プロパティを設定することも、それ以外の場合、既定値は 1 および 1 つの結果のみが返されます。

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2.ビジョンの処理を開始します。

次のコードでは、要求の処理を開始します。 **VisionRects**サンプルをユーザーがイメージを選択した後、このコードが実行されます。

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

このハンドラーは、`ciImage`ビジョン framework`VNDetectRectanglesRequest`手順 1. で作成されました。

### <a name="3-handle-the-results-of-vision-processing"></a>3.ビジョン処理の結果を処理します。

四角形の検出が完了したら、フレームワークが実行される、`HandleRectangles`メソッドの概要を次に示します。

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

### <a name="4-display-the-results"></a>4.結果を表示します。

`OverlayRectangles`メソッドで、 **VisionRectangles**サンプルでは、3 つの関数。

- ソース イメージのレンダリング
- 1 つずつが検出されたを示すために四角形を描画し、
- CoreGraphics を使用して各四角形のテキスト ラベルを追加します。

ビュー、[サンプルのソース](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)正確な CoreGraphics メソッドです。

![検出された 3 つの四角形を写真します。](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5.さらに処理します。

四角形の検出は多くの場合、操作のチェーン内の第一歩にすぎませんなどで[この CoreMLVision 例](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)、四角形が手書きの桁を解析する CoreML モデルに渡しています。


<a name="faces" />

## <a name="face-detection"></a>顔の検出

[VisionFaces サンプル](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)と同様の方法で動作、 **VisionRectangles**サンプルは、異なるビジョン要求クラスを使用します。

### <a name="1-initialize-the-vision-request"></a>1.ビジョン要求を初期化します。

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

### <a name="3-handle-the-results-of-vision-processing"></a>3.ビジョン処理の結果を処理します。

ハンドラーが実行される顔の検出が完了した後、`HandleRectangles`エラー処理を実行し、検出された面、および呼び出しの境界を表示する方法、`OverlayRectangles`元の画像に外接する四角形を描画します。

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

### <a name="4-display-the-results"></a>4.結果を表示します。

`OverlayRectangles`メソッドで、 **VisionFaces**サンプルでは、3 つの関数。

- ソース イメージのレンダリング
- 検出されると、各フォント フェイスの四角形を描画し、
- CoreGraphics を使用して各フォント フェイスのテキスト ラベルを追加します。

ビュー、[サンプルのソース](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)正確な CoreGraphics メソッドです。

![検出された 2 つの面での写真します。](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5.さらに処理します。

ビジョン フレームワークには、目と口などの顔の機能を検出するためにその他の機能が含まれています。 使用して、`VNDetectFaceLandmarksRequest`を返す型`VNFaceObservation`結果、上記の手順 3 と同様に、追加された`VNFaceLandmark`データ。


## <a name="related-links"></a>関連リンク

- [ビジョン四角形 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [ビジョン面 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [強化され、Core のイメージのフィルター、金属、ビジョン、および複数 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/510/)
