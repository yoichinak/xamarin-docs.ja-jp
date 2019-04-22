---
title: Xamarin.iOS で ML 2 コア
description: このドキュメントでは、iOS 12 の一部として core ML が使用可能な更新プログラムについて説明します。 具体的には、新しいバッチ予測 API に関連付けられているパフォーマンスの向上に見えます。
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/15/2018
ms.openlocfilehash: 50d59f0b6ff2133c5870d84a1d740547768116e0
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58869729"
---
# <a name="core-ml-2-in-xamarinios"></a>Xamarin.iOS で ML 2 コア

コア ML は、機械学習では、iOS、macOS、tvOS、watchOS、使用可能なテクノロジです。 アプリの機械学習モデルに基づいて予測を行うことができます。

12、iOS では、Core ML には、バッチ処理 API が含まれています。 この API より効率的に Core ML し、モデルを使用して一連の予測を作成するシナリオでパフォーマンスの向上を提供します。

## <a name="sample-app-marshabitatcoremltimer"></a>サンプル アプリ:MarsHabitatCoreMLTimer

コア ML を使用したバッチ予測を示すためを見て、 [MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer)サンプル アプリです。 このサンプルを使用して、habitat、Mars で構築するためのコストを予測するコア ML モデルのトレーニングは、さまざまな入力に基づく: ソーラー パネル、greenhouses、数、および 2万の数の数値。

このドキュメントでのコード スニペットは、このサンプルから取得されます。

## <a name="generate-sample-data"></a>サンプル データの生成

`ViewController`、サンプル アプリの`ViewDidLoad`メソッド呼び出し`LoadMLModel`、含まれるコア ML モデルが読み込まれます。

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

サンプル アプリが 100,000 を作成し、`MarsHabitatPricerInput`シーケンシャルの Core ML 予測の入力として使用するオブジェクト。 生成された各サンプルでは、ランダムな値に設定ソーラー パネル、greenhouses、数、および 2万の数の数があります。

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

アプリの 3 つのボタンをタップすると予測の 2 つのシーケンスを実行します。 1 つを使用して、`for`ループ、および新しいバッチを使用して別`GetPredictions`iOS 12 で導入されたメソッド。

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

`for`指定の数の入力を反復処理単純ループのバージョンのテストを呼び出す[ `GetPrediction` ](xref:CoreML.MLModel.GetPrediction*)ごとと、結果を破棄します。 メソッドは、予測にかかる時間を時間します。

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

## <a name="getpredictions-new-batch-api"></a>GetPredictions (新しいバッチ API)

テストのバッチのバージョンを作成します、`MLArrayBatchProvider`入力配列からオブジェクト (これは必須の入力パラメーターのため、`GetPredictions`メソッド)、作成、 [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
オブジェクトを使用して、CPU に制限されない予測計算を防止、`GetPredictions`をもう一度、結果を破棄することで予測をフェッチする API。

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

シミュレーターとデバイスの両方で`GetPredictions`Core ML のループに基づく予測よりもすぐに終了します。

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer)
- [コア ML、第 1 部 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/708/)
- [コア ML、第 2 部 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Xamarin.iOS の Core ML の概要](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [コア ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [コア ML モデルの操作](https://developer.apple.com/machine-learning/build-run-models/)
