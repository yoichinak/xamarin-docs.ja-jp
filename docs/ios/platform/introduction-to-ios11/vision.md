---
title: Xamarin のビジョンフレームワーク
description: このドキュメントでは、Xamarin の ios 11 ビジョンフレームワークを使用する方法について説明します。 具体的には、四角形の検出と顔検出について説明します。
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/31/2017
ms.openlocfilehash: 9a31cd31f7cbfb4748991d45204ca867b5d62c8c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436627"
---
# <a name="vision-framework-in-xamarinios"></a>Xamarin のビジョンフレームワーク

ビジョンフレームワークは、次のような新しいイメージ処理機能を iOS 11 に追加します。

- [四角形の検出](#rectangles)
- [顔検出](#faces)
- Machine Learning イメージ分析 ( [Coreml](~/ios/platform/introduction-to-ios11/coreml.md)で説明)
- バーコードの検出
- イメージの配置の分析
- テキストの検出
- 水平の検出
- オブジェクト検出 & 追跡

![3つの四角形が検出された写真](vision-images/found-rectangles-tiny.png) ![2つの顔が検出された写真](vision-images/xamarin-home-faces-tiny.png)

四角形の検出と顔検出については、以下で詳しく説明します。

<a name="rectangles"></a>

## <a name="rectangle-detection"></a>四角形の検出

[VisionRects サンプル](/samples/xamarin/ios-samples/ios11-visionrectangles)では、イメージを処理し、検出された四角形を描画する方法を示します。

### <a name="1-initialize-the-vision-request"></a>1. ビジョン要求を初期化します

で、 `ViewDidLoad` `VNDetectRectanglesRequest` `HandleRectangles` 各要求の最後に呼び出されるメソッドを参照するを作成します。

`MaximumObservations`プロパティも設定する必要があります。それ以外の場合は、既定で1になり、1つの結果のみが返されます。

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. ビジョン処理を開始する

次のコードでは、要求の処理を開始します。 **VisionRects**サンプルでは、ユーザーがイメージを選択した後に、このコードが実行されます。

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

このハンドラーは、 `ciImage` 手順 1. で作成したビジョンフレームワークにを渡し `VNDetectRectanglesRequest` ます。

### <a name="3-handle-the-results-of-vision-processing"></a>3. ビジョン処理の結果を処理する

四角形の検出が完了すると、フレームワークによってメソッドが実行 `HandleRectangles` されます。次にその概要を示します。

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

### <a name="4-display-the-results"></a>4. 結果を表示する

`OverlayRectangles` **VisionRectangles**サンプルのメソッドには、次の3つの関数があります。

- ソースイメージのレンダリング
- 四角形を描画して、それぞれが検出された場所を示します。
- CoreGraphics を使用して各四角形のテキストラベルを追加する。

正確な CoreGraphics メソッドの [サンプルのソース](/samples/xamarin/ios-samples/ios11-visionrectangles) を表示します。

![3つの四角形が検出された写真](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. さらに処理

四角形の検出は、多くの場合、操作チェーンの最初の手順にすぎません。 [たとえば、この CoreMLVision の例](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)では、四角形を coreml モデルに渡して、手書きの数字を解析します。

<a name="faces"></a>

## <a name="face-detection"></a>顔検出

[VisionFaces サンプル](/samples/xamarin/ios-samples/ios11-visionfaces)は、別のビジョン要求クラスを使用して、 **VisionRectangles**サンプルと同様の方法で動作します。

### <a name="1-initialize-the-vision-request"></a>1. ビジョン要求を初期化します

で、 `ViewDidLoad` `VNDetectFaceRectanglesRequest` `HandleRectangles` 各要求の終了時に呼び出されるメソッドを参照するを作成します。

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. ビジョン処理を開始する

次のコードでは、要求の処理を開始します。 **VisionFaces**サンプルでは、ユーザーがイメージを選択した後にこのコードが実行されます。

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

このハンドラーは、 `ciImage` 手順 1. で作成したビジョンフレームワークにを渡し `VNDetectFaceRectanglesRequest` ます。

### <a name="3-handle-the-results-of-vision-processing"></a>3. ビジョン処理の結果を処理する

顔検出が完了すると、ハンドラーは、 `HandleRectangles` エラー処理を実行するメソッドを実行し、検出された面の境界を表示し、を呼び出して、 `OverlayRectangles` 外接する四角形を元の画像に描画します。

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

### <a name="4-display-the-results"></a>4. 結果を表示する

`OverlayRectangles` **VisionFaces**サンプルのメソッドには、次の3つの関数があります。

- ソースイメージのレンダリング
- 検出された各顔に四角形を描画します。
- CoreGraphics を使用して、各面にテキストラベルを追加します。

正確な CoreGraphics メソッドの [サンプルのソース](/samples/xamarin/ios-samples/ios11-visionfaces) を表示します。

![2つの顔が検出された写真](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. さらに処理

ビジョンフレームワークには、顔の特徴 (目や口など) を検出するための追加機能が含まれています。 この `VNDetectFaceLandmarksRequest` 型を使用します。これは、 `VNFaceObservation` 上記の手順3のように結果を返しますが、追加のデータが含まれ `VNFaceLandmark` ます。

## <a name="related-links"></a>関連リンク

- [ビジョン四角形 (サンプル)](/samples/xamarin/ios-samples/ios11-visionrectangles)
- [ビジョン (サンプル)](/samples/xamarin/ios-samples/ios11-visionfaces)
- [主要なイメージ (フィルター、金属、ビジョンなど) の進歩 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/510/)