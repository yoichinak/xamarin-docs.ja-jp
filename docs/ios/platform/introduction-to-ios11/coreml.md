---
title: Xamarin.iOS で CoreML の概要
description: このドキュメントでは、CoreML で、iOS での machine learning の使用について説明します。 このドキュメントでは、ビジョンのフレームワークで使用する方法と CoreML を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/30/2017
ms.openlocfilehash: 3a00a7256cace9cbcff3478d866646d48cfdc50b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120074"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin.iOS で CoreML の概要

CoreML は machine learning を iOS – アプリは画像認識に問題が解決からあらゆる種類のタスクを実行するトレーニングされた機械学習モデルの活用できます。

この概要では、次の項目について説明します。

- [CoreML の概要](#coreml)
- [ビジョン フレームワークで CoreML の使用](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML の概要

次の手順では、CoreML を iOS プロジェクトに追加する方法について説明します。 参照してください、 [Mars Habitat Pricer サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)実用的な例です。

![Mars Habitat 価格を予測のサンプルのスクリーン ショット](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1.CoreML モデルをプロジェクトに追加します。

CoreML モデルの追加 (ファイル、 **.mlmodel**拡張機能) を**リソース**プロジェクトのディレクトリ。 

モデル ファイルのプロパティでは、その**ビルド アクション**に設定されている**CoreMLModel**します。 つまり、コンパイルには、 **.mlmodelc**ファイル、アプリケーションのビルド時にします。

### <a name="2-load-the-model"></a>2.モデルを読み込む

使用して、モデルを読み込み、`MLModel.Create`静的メソッド。

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3.パラメーターを設定します

アウトを実装するコンテナー クラスを使用してモデル パラメーターが渡される`IMLFeatureProvider`します。

機能のプロバイダー クラスは、文字列の辞書のように動作し、 `MLFeatureValue`s、各機能の値が単純な文字列または数値、配列またはデータ、またはイメージを格納しているピクセル バッファーをする可能性があります。

単一値機能プロバイダーのコードは、以下に示します。

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

このようなクラスを使用して、入力パラメーターは、CoreML で認識される方法で指定できます。 機能の名前 (など`myParam`コード例では)、モデルで必要なものと一致する必要があります。

### <a name="4-run-the-model"></a>4.モデルを実行します。

モデルを使用して設定する必要がインスタンス化する機能のプロバイダーとパラメーター、しを`GetPrediction`メソッドが呼び出されます。

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5.結果を抽出します。

予測結果`outFeatures`のインスタンスも`IMLFeatureProvider`は出力値を使用してアクセスできます`GetFeatureValue`を各出力パラメーターの名前 (など`theResult`)、この例のように。

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>ビジョン フレームワークで CoreML の使用

CoreML は、形状認識、オブジェクトの識別、およびその他のタスクなど、イメージ上の操作を実行するビジョン フレームワークと組み合わせても使用できます。

次の手順では、どの CoreML とビジョンが一緒に使用でについて説明します、 [CoreMLVision サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)します。 サンプルを組み合わせて、[四角形認識](~/ios/platform/introduction-to-ios11/vision.md#rectangles)ビジョン フレームワークから、 _MNINSTClassifier_写真で手書き数字を識別するために、CoreML モデル。

![番号 3 の画像認識](coreml-images/vision3.png) ![数字の 5 の画像認識](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1.ビジョン CoreML モデルを作成します。

CoreML モデル_MNISTClassifier_が読み込まれでラップし、`VNCoreMLModel`モデルをビジョン タスクで使用できるようにします。 このコードでは、2 つのビジョン要求も作成します。 最初に、イメージ内の四角形の検索し、CoreML モデルを使用して四角形を処理するため。

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

クラスが実装する必要がある、`HandleRectangles`と`HandleClassification`手順 3. および 4. を以下に示すように、Vision 要求メソッド。

### <a name="2-start-the-vision-processing"></a>2.ビジョンの処理を開始します。

次のコードでは、要求の処理を開始します。 **CoreMLVision**サンプルでは、ユーザーがイメージを選択した後、このコードが実行されます。

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

このハンドラーは、`ciImage`ビジョン framework`VNDetectRectanglesRequest`手順 1. で作成されました。

### <a name="3-handle-the-results-of-vision-processing"></a>3.ビジョンの処理の結果を処理します。

四角形の検出が完了すると実行、`HandleRectangles`メソッドは、最初の四角形を抽出する画像をトリミングするには、四角形のイメージをグレースケールに変換および分類 CoreML モデルに渡します。

`request`このメソッドに渡されるパラメーターには、ビジョンの要求の詳細が含まれています。 使用すると、 `GetResults<VNRectangleObservation>()` 、イメージ内の四角形の一覧を返します。 最初の四角形`observations[0]`抽出され、CoreML モデルに渡されます。

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

`ClassificationRequest`を使用する、手順 1 で初期化された、`HandleClassification`メソッドは、次の手順で定義されています。

### <a name="4-handle-the-coreml"></a>4.ハンドル、CoreML

`request`このメソッドに渡されるパラメーターには、CoreML 要求の詳細が含まれていてを使用して、`GetResults<VNClassificationObservation>()`信頼度で順序付け可能な結果の一覧を返します (最も高い信頼度最初)。

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>サンプル

次の 3 つの CoreML サンプルを試すにがあります。

* [Mars Habitat 価格を予測サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)が単純な数値の入力と出力します。

* [Vision & CoreML サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)イメージ パラメーターを受け取り、1 桁の数字を認識する CoreML モデルに渡される画像の正方形の領域を識別するビジョン フレームワークを使用します。

* 最後に、 [CoreML 画像認識のサンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)CoreML を写真で機能を識別するために使用します。 既定では、小さい**SqueezeNet**モデル (5 MB) が、書き込まれたダウンロードされ、大きい方を組み込むように**VGG16**モデル (553 MB)。 詳細については、次を参照してください。、[サンプルの readme](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)します。

## <a name="related-links"></a>関連リンク

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML 例 (Mars Habitat) (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML とビジョン (数字認識) (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML 画像認識 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML と Azure の Custom Vision (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [CoreML (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/703/)
