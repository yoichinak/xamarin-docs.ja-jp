---
title: Xamarin. iOS の CoreML の概要
description: このドキュメントでは、iOS で machine learning を使用できる CoreML について説明します。 このドキュメントでは、CoreML の使用を開始する方法と、それをビジョンフレームワークと共に使用する方法について説明します。
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 4319d9ab07682795e8890779a65a0e2289f4501c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032214"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin. iOS の CoreML の概要

CoreML は機械学習を iOS にもたらします。アプリはトレーニング済みの機械学習モデルを活用して、問題解決からイメージ認識まで、あらゆる種類のタスクを実行できます。

この概要では、次の内容について説明します。

- [CoreML のはじめに](#coreml)
- [CoreML とビジョンフレームワークの使用](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML のはじめに

次の手順では、iOS プロジェクトに CoreML を追加する方法について説明します。 実際の例については、 [Mars Habitat Pricer サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)を参照してください。

![Mars Habitat Price 予測のサンプルのスクリーンショット](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. CoreML モデルをプロジェクトに追加します。

CoreML モデル ( **mlmodel**拡張子を持つファイル) をプロジェクトの**Resources**ディレクトリに追加します。 

モデルファイルのプロパティでは、その**ビルドアクション**は**Coremlmodel**に設定されます。 これは、アプリケーションのビルド時に、このファイルが**mlmodelc**ファイルにコンパイルされることを意味します。

### <a name="2-load-the-model"></a>2. モデルを読み込む

`MLModel.Create` の静的メソッドを使用してモデルを読み込みます。

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. パラメーターを設定する

モデルパラメーターは `IMLFeatureProvider`を実装するコンテナークラスを使用して渡されます。

機能プロバイダークラスは、文字列と `MLFeatureValue`s の辞書のように動作します。各特徴値には、単純な文字列または数値、配列またはデータ、またはイメージを含むピクセルバッファーを指定できます。

単一値の機能プロバイダーのコードを次に示します。

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

このようなクラスを使用して、CoreML で認識される方法で入力パラメーターを指定できます。 機能の名前 (コード例では `myParam` など) は、モデルで想定されているものと一致する必要があります。

### <a name="4-run-the-model"></a>4. モデルを実行する

このモデルを使用するには、機能プロバイダーをインスタンス化し、パラメーターを設定し、`GetPrediction` メソッドを呼び出す必要があります。

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. 結果を抽出する

予測結果 `outFeatures` は `IMLFeatureProvider`のインスタンスでもあります。出力値には、次の例のように、各出力パラメーターの名前 (`theResult`など) と共に `GetFeatureValue` を使用してアクセスできます。

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>CoreML とビジョンフレームワークの使用

CoreML をビジョンフレームワークと組み合わせて使用して、図形の認識、オブジェクトの識別、その他のタスクなどのイメージに対する操作を実行することもできます。

次の手順では、 [Coremlビジョンのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)で coreml とビジョンを一緒に使用する方法について説明します。 このサンプルでは、ビジョンフレームワークの[四角形認識](~/ios/platform/introduction-to-ios11/vision.md#rectangles)と_MNINSTClassifier_ coreml モデルを組み合わせて、写真内の手書き数字を識別します。

![数値3の画像認識](coreml-images/vision3.png) ![番号5の画像認識](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. ビジョン CoreML モデルを作成する

CoreML モデル_MNISTClassifier_が読み込まれ、`VNCoreMLModel` にラップされます。これにより、モデルをビジョンタスクで使用できるようになります。 このコードでは、イメージ内の四角形を検索するために最初に2つのビジョン要求を作成し、次に CoreML モデルで四角形を処理します。

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

このクラスでは、次の手順3と4に示すように、ビジョン要求の `HandleRectangles` および `HandleClassification` メソッドを実装する必要があります。

### <a name="2-start-the-vision-processing"></a>2. ビジョン処理を開始する

次のコードでは、要求の処理を開始します。 **Coremlvision**のサンプルでは、ユーザーがイメージを選択した後に、このコードが実行されます。

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

このハンドラーは、手順 1. で作成したビジョンフレームワーク `VNDetectRectanglesRequest` に `ciImage` を渡します。

### <a name="3-handle-the-results-of-vision-processing"></a>3. ビジョン処理の結果を処理する

四角形の検出が完了すると、`HandleRectangles` メソッドが実行されます。このメソッドは、イメージをトリミングして最初の四角形を抽出し、四角形のイメージをグレースケールに変換して、それを CoreML モデルに渡して分類します。

このメソッドに渡される `request` パラメーターには、ビジョン要求の詳細が含まれており、`GetResults<VNRectangleObservation>()` メソッドを使用して、イメージ内で見つかった四角形のリストを返します。 最初の四角形 `observations[0]` が抽出され、CoreML モデルに渡されます。

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

次の手順で定義されている `HandleClassification` メソッドを使用するために、手順1で `ClassificationRequest` が初期化されました。

### <a name="4-handle-the-coreml"></a>4. CoreML を処理する

このメソッドに渡される `request` パラメーターには、CoreML 要求の詳細が含まれています。また、`GetResults<VNClassificationObservation>()` メソッドを使用すると、考えられる結果の一覧が信頼度の高い順序で並べ替えられます (最も信頼度が高い順)。

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

次の3つの CoreML サンプルを試すことができます。

- [Mars Habitat Price の予測サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)には、単純な数値入力と出力があります。

- この[ビジョン & coreml サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)は image パラメーターを受け取り、ビジョンフレームワークを使用してイメージ内の四角形領域を識別します。これは、1桁の数字を認識する coreml モデルに渡されます。

- 最後に、 [Coreml イメージ認識サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)では、coreml を使用して写真内の機能を識別します。 既定では、小さい**SqueezeNet**モデル (5 mb) を使用しますが、大規模な**VGG16**モデル (553mb) をダウンロードして組み込むことができるように記述されています。 詳細については、[サンプルの readme](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML の例 (Mars Habitat) (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)
- [CoreML とビジョン (数値認識) (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [CoreML イメージ認識 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [CoreML と Azure Custom Vision (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [CoreML (WWDC) の概要 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/703/)
