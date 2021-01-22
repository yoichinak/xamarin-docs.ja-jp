---
title: Xamarin. iOS の進行状況とアクティビティインジケーター
description: このドキュメントでは、Xamarin. iOS で進行状況とアクティビティのインジケーターを使用する方法について説明します。 プログラムとストーリーボードの両方で使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2017
ms.openlocfilehash: 0bfb4168e3d990ae1afb3ee2022553053c383083
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697593"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Xamarin. iOS の進行状況とアクティビティインジケーター

アプリでは、データの読み込みや処理などの実行時間の長いタスクを実行する必要があり、この遅延によって UI の更新に遅延が生じる可能性があります。 この間、システムがビジー状態であることをユーザーに知らせるためには、常に進行状況インジケーターを使用する必要があります。 これにより、ユーザーは、アプリが要求に対して作業していること、入力を待機していないこと、および、待機する必要がある時間を正確に詳述する手段を提供できます。

iOS には、アプリでこの進行状況を示す2つの主な方法が用意されています。アクティビティインジケーター (特定の _ネットワーク_ アクティビティインジケーターを含む) と進行状況バーです。

## <a name="activity-indicator"></a>アクティビティのインジケーター

アプリが長いプロセスを実行していても、タスクに必要な時間が正確にわからない場合は、アクティビティインジケーターを表示する必要があります。

Apple では、アクティビティインジケーターの使用に関して次のような推奨事項があります。

- **可能な限り、進行状況** バーを使用します。これは、アクティビティインジケーターによって、実行されているプロセスの所要時間に関するフィードバックがユーザーに表示されないため、長さがわかっている場合は常に進行状況バーを使用します (たとえば、ファイルにダウンロードするバイト数)。
- **インジケーターをアニメーション** 化する-ユーザーは、継続したアクティビティインジケーターを停滞したアプリに関連付けるため、インジケーターが表示されている間は常にインジケーターをアニメーション化しておく必要があります。
- **処理中のタスクについて説明** します。アクティビティインジケーターだけを表示するだけでは十分ではありません。ユーザーは、待機しているプロセスについて通知する必要があります。 タスクを明確に定義するわかりやすいラベル (通常は1つの完全な文) を含めます。

### <a name="implementing-an-activity-indicator"></a>アクティビティインジケーターの実装

アクティビティインジケーターは、が実行 [`UIActivityIndictorView`](xref:UIKit.UIActivityIndicatorView) されていることを示すためにクラスによって実装され `UIActivity` ます。

### <a name="activity-indicators-and-storyboards"></a>アクティビティインジケーターとストーリーボード

IOS Designer を使用して UI を作成する場合は、[ツールボックス] からアクティビティインジケーターをレイアウトに追加できます。 Properties Pad から次のプロパティを調整できます。

![スクリーンショットには、スタイル、色、動作のプロパティを変更できる Properties Pad が示されています。](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>アクティビティインジケーターの動作の管理

`StartAnimating()` `StopAnimating()` アクティビティインジケーターアニメーションを開始または停止するには、メソッドとメソッドを使用します。

`HidesWhenStopped` `true` が呼び出された後にアクティビティインジケーターが非表示になるようにするには、プロパティをに設定し `StopAnimating()` ます。 既定では、これはに設定され `true` ています。 どの時点でも、プロパティをチェックすることで、アクティビティインジケーターがスピン中のアニメーションを実行しているかどうかを確認でき `IsAnimating` ます。 

### <a name="managing-activity-indicator-appearances"></a>アクティビティインジケーターの外観を管理する

`UIActivityIndicatorViewStyle`アクティビティインジケーターをインスタンス化するときに、列挙体をパラメーターとして渡すことができます。 これを使用すると、visual スタイルを、、またはに設定でき `Gray` `White` `WhiteLarge` ます。たとえば、次のようになります。

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

によって提供される色は、プロパティを設定することによってオーバーライドでき `UIActivityIndicatorViewStyle` `Color` ます。

## <a name="progress-bar"></a>進行状況バー

進行状況バーは、時間のかかるタスクの状態と長さを示すために色で塗りつぶす線として表示されます。 タスクの長さがわかっている場合、または計算できる場合は、進行状況バーを常に使用する必要があります。

Apple では、進行状況バーの操作に関して次のような推奨事項があります。

- **進行状況を正確に報告** する-進行状況バーは、タスクを完了するために必要な時間の正確な表現である必要があります。 アプリがビジー状態として表示されるまでの時間を詐称しないでください。
- **Well-Defined の期間に使用し** ます。進行状況バーには、時間のかかるタスクが発生したことを示すだけでなく、タスクが完了したかどうかと、残存時間の見積もりが表示されます。

### <a name="implementing-an-progress-bar"></a>プログレスバーの実装

進行状況バーは、 [`UIProgressView`](xref:UIKit.UIProgressView)

### <a name="progress-bars-and-storyboards"></a>進行状況バーとストーリーボード

IOS Designer を使用しているときに、進行状況バーを UI に追加することもできます。 **ツールボックス** で **進行状況ビュー** を検索し、ビューにドラッグします。

プロパティパッドでは、次のプロパティを調整できます。

![スクリーンショットには、スタイル、進行状況、進行状況の濃淡、トラックの濃淡、進行状況の画像、および画像のプロパティの追跡を行う Properties Pad が示されています。](progress-activity-indicator-images/progress-indicator3.png)

### <a name="managing-progress-bar-behavior"></a>進行状況バーの動作の管理

バーの進行状況は、プロパティを使用して最初に設定でき `Progress` ます。

```csharp
ProgressBar.Progress = 0f;
```

進行状況を調整するには、メソッドを使用して、 `SetProgress` 変更をアニメーション化するかどうかを宣言するブール値を渡します。

```csharp
ProgressBar.SetProgress(1.0f, true);
```

進行状況バーの使用方法の詳細については、「 [進行状況の報告](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) 」レシピと [UICatalog tvOS サンプル](/samples/xamarin/ios-samples/tvos-uicatalog)を参照してください。

### <a name="managing-progress-bar-appearance"></a>進行状況バーの外観の管理

アクティビティインジケーターと同様に、 `UIProgressViewStyle` 進行状況バーをインスタンス化するときに、列挙体をパラメーターとして渡すことができます。

次のプロパティを使用して、進行状況とトラックイメージおよび濃淡の色を調整できます。

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```