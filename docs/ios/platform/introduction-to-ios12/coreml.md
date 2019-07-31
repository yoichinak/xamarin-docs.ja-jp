---
title: Xamarin のコア ML 2
description: このドキュメントでは、iOS 12 の一部として利用できるコア ML の更新について説明します。 具体的には、新しいバッチ予測 API に関連するパフォーマンスの向上に注目します。
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/15/2018
ms.openlocfilehash: 4fc72e855101f110310a46145c577b272a647ac3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645702"
---
# <a name="core-ml-2-in-xamarinios"></a>Xamarin のコア ML 2

コア ML は、iOS、macOS、tvOS、watchOS で利用できる機械学習テクノロジです。 これにより、アプリは機械学習モデルに基づいて予測を行うことができます。

IOS 12 では、コア ML にバッチ処理 API が含まれています。 この API により、コア ML がより効率的になり、モデルを使用して一連の予測を行うシナリオのパフォーマンスが向上します。

## <a name="sample-app-marshabitatcoremltimer"></a>サンプルアプリ:MarsHabitatCoreMLTimer

コア ML でバッチ予測を行う方法については、 [MarsHabitatCoreMLTimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)サンプルアプリを参照してください。 このサンプルでは、主要な ML モデルを使用して、さまざまな入力 (太陽のパネルの数、greenhouses の数、acres の数) に基づいて、Mars で habitat を構築するためのコストを予測します。

このドキュメントのコードスニペットは、このサンプルを基にしています。

## <a name="generate-sample-data"></a>サンプル データの生成

で`ViewController`は、サンプルアプリの`ViewDidLoad`メソッドを`LoadMLModel`呼び出して、含まれているコア ML モデルを読み込みます。

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

次に、サンプルアプリによっ`MarsHabitatPricerInput`て、順次コア ML 予測の入力として使用する10万オブジェクトが作成されます。 生成された各サンプルには、太陽のパネルの数、greenhouses の数、および acres の数に対してランダムな値が設定されています。

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

アプリの3つのボタンをタップすると、2つの予測のシーケンス`for`が実行されます。1つ`GetPredictions`はループを使用し、もう1つは iOS 12 で導入された新しい batch メソッドを使用します。

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## <a name="for-loop"></a>for ループ

テスト`for`ネイティブのループバージョンは、指定された数の入力を反復[`GetPrediction`](xref:CoreML.MLModel.GetPrediction*)処理し、それぞれに対してを呼び出し、結果を破棄します。 メソッドは、予測を行うためにかかる時間の長さを示します。

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="getpredictions-new-batch-api"></a>GetPredictions (新しい batch API)

テストのバッチバージョンは、入力配列`MLArrayBatchProvider`からオブジェクトを作成します (これは`GetPredictions`メソッドの必須の入力パラメーターであるため)。は、[`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
予測計算が CPU に限定されないようにするオブジェクト。この`GetPredictions` API を使用して予測をフェッチし、結果を破棄します。

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="results"></a>結果

シミュレーターとデバイスの両方で`GetPredictions` 、はループベースのコア ML 予測よりも高速に終了します。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– MarsHabitatCoreMLTimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
- [コア ML の新機能、パート 1 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [コア ML の新機能、パート 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Xamarin のコア ML の概要](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [コア ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [主要な ML モデルの使用](https://developer.apple.com/machine-learning/build-run-models/)
