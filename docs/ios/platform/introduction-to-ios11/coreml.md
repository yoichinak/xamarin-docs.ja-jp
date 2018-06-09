---
title: Introduction to Xamarin.iOS で CoreML
description: このドキュメントでは、iOS での機械学習できる CoreML について説明します。 このドキュメントでは、ビジョンのフレームワークで使用する方法と CoreML を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 8b489fd1a1bcce474decf6881e8eb6620c2ee2e3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240737"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Introduction to Xamarin.iOS で CoreML

CoreML iOS に機械学習の表示 – からを実行するあらゆる種類の作業、画像認識に問題が解決するためのトレーニング済みの機械学習モデルのアプリを利用できます。

この概要では、次の項目について説明します。

- [CoreML の概要](#coreml)
- [ビジョン フレームワークで CoreML の使用](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML の概要

次の手順では、iOS プロジェクトを CoreML を追加する方法について説明します。 参照してください、 [Mars 汚い Pricer サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)の実用的な例です。

![Mars 汚い価格の予測のサンプルのスクリーン ショット](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1.CoreML モデルをプロジェクトに追加します。

CoreML モデルの追加 (ファイルが、 **.mlmodel**拡張機能) を**リソース**プロジェクトのディレクトリ。 

モデル ファイルのプロパティでは、その**ビルド アクション**に設定されている**CoreMLModel**です。 つまりにコンパイルすること、 **.mlmodelc**ファイルのアプリケーションのビルド時にします。

### <a name="2-load-the-model"></a>2.モデルを読み込む

使用してモデルを読み込み、`MLModel.Create`静的メソッド。

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3.パラメーターを設定します。

アウトを実装するコンテナー クラスを使用してモデル パラメーターが渡される`IMLFeatureProvider`です。

機能プロバイダー クラスは、文字列の辞書のように動作し、 `MLFeatureValue`s、各機能の値が、単純な文字列または数値、配列またはデータ、またはイメージを含むピクセル バッファーをする可能性があります。

単一値機能プロバイダーのコードを次に示します。

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

次のようにクラスを使用して、入力パラメーターは、CoreML で認識できるように指定できます。 機能の名前 (など`myParam`コード例では)、モデルが必要ですが一致する必要があります。

### <a name="4-run-the-model"></a>4.モデルを実行します。

モデルを使用する場合は、機能プロバイダーのインスタンス化してパラメーターに設定されて、しする必要がある、`GetPrediction`メソッドを呼び出します。

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5.結果を抽出します。

予測結果`outFeatures`のインスタンスではまた`IMLFeatureProvider`は出力値を使用してアクセスできます`GetFeatureValue`を各出力パラメーターの名前 (など`theResult`)、この例のように。

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>ビジョン フレームワークで CoreML の使用

CoreML はシェイプの認識、オブジェクトの識別、およびその他のタスクなどの画像の操作を実行ビジョン フレームワークと組み合わせて使用することもできます。

次の手順使用方法について説明 CoreML とビジョンは一緒に、 [CoreMLVision サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)です。 サンプルを組み合わせて、[四角形の認識](~/ios/platform/introduction-to-ios11/vision.md#rectangles)ビジョン フレームワークから、 _MNINSTClassifier_写真の手書きの桁を識別する CoreML モデル。

![番号 3 の画像認識](coreml-images/vision3.png) ![数字の 5 の画像認識](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1.ビジョン CoreML モデルを作成します。

CoreML モデル_MNISTClassifier_は読み込まれでラップし、`VNCoreMLModel`モデルがビジョン タスクの利用可能です。 このコードでは、2 つのビジョン要求も作成されます: をイメージに四角形を検索するためには、まずし CoreML モデルを含む四角形を処理します。

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

クラスが実装する必要がある、`HandleRectangles`と`HandleClassification`手順 3. および 4. を以下に示すように、ビジョンの要求のメソッドです。

### <a name="2-start-the-vision-processing"></a>2.ビジョンの処理を開始します。

次のコードでは、要求の処理を開始します。 **CoreMLVision**サンプルをユーザーがイメージを選択した後、このコードが実行されます。

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

このハンドラーは、`ciImage`ビジョン framework`VNDetectRectanglesRequest`手順 1. で作成されました。

### <a name="3-handle-the-results-of-vision-processing"></a>3.ビジョン処理の結果を処理します。

四角形の検出が完了したらを実行して、`HandleRectangles`メソッドで、最初の四角形を抽出する画像をトリミングするには、グレースケール、四角形イメージを変換し、分類の CoreML モデルに渡されます。

`request`このメソッドに渡されるパラメーターには、ビジョンの要求の詳細が含まれています。 使用すると、`GetResults<VNRectangleObservation>()`イメージで見つかった四角形の一覧を返します。 最初の四角形`observations[0]`が抽出され、CoreML モデルに渡されます。

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

`ClassificationRequest`を使用する、手順 1 で初期化された、`HandleClassification`次の手順で定義されたメソッドです。

### <a name="4-handle-the-coreml"></a>4.ハンドル、CoreML

`request`このメソッドに渡されるパラメーターには、CoreML 要求の詳細が含まれています。 使用すると、`GetResults<VNClassificationObservation>()`信頼度で順序付けられた考えられる結果の一覧を返します (最も高い信頼度最初)。

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

試す CoreML サンプルを次の 3 つあります。

* [Mars 汚い価格を予測サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)が単純な数値入力と出力します。

* [ビジョン & CoreML サンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)イメージ パラメーターを受け取り、図では、1 桁の数字を認識する CoreML モデルに渡される四角形の領域を識別する、ビジョンのフレームワークを使用します。

* 最後に、 [CoreML 画像認識のサンプル](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)CoreML を使用して、写真の機能を識別します。 既定では、小さい**SqueezeNet**モデル (5 MB) が、これは、書き込まれたをダウンロードして、大きい方を組み込むように**VGG16**モデル (553 MB)。 詳細については、次を参照してください。、[サンプルの readme](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)です。

## <a name="related-links"></a>関連リンク

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML 例 (Mars 汚い) (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML とビジョン (番号認識) (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML 画像認識 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML Azure カスタム ビジョン (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [紹介 CoreML (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/703/)
