---
title: Xamarin. iOS の進行状況とアクティビティインジケーター
description: このドキュメントでは、Xamarin. iOS で進行状況とアクティビティのインジケーターを使用する方法について説明します。 プログラムとストーリーボードの両方で使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2017
ms.openlocfilehash: 76e1ee54a5e1b729fdcb0b0a2c1f278703b2b4d6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021971"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Xamarin. iOS の進行状況とアクティビティインジケーター

アプリでは、データの読み込みや処理などの実行時間の長いタスクを実行する必要があり、この遅延によって UI の更新に遅延が生じる可能性があります。 この間、システムがビジー状態であることをユーザーに知らせるためには、常に進行状況インジケーターを使用する必要があります。 これにより、ユーザーは、アプリが要求に対して作業していること、入力を待機していないこと、および、待機する必要がある時間を正確に詳述する手段を提供できます。

iOS には、アプリでこの進行状況を示す2つの主な方法が用意されています。アクティビティインジケーター (特定の_ネットワーク_アクティビティインジケーターを含む) と進行状況バーです。

## <a name="activity-indicator"></a>アクティビティインジケーター

アプリが長いプロセスを実行していても、タスクに必要な時間が正確にわからない場合は、アクティビティインジケーターを表示する必要があります。

Apple では、アクティビティインジケーターの使用に関して次のような推奨事項があります。

- **可能な限り、進行状況**バーを使用します。これは、アクティビティインジケーターによって、実行されているプロセスの所要時間に関するフィードバックがユーザーに表示されないため、長さがわかっている場合は常に進行状況バーを使用します (たとえば、ファイルにダウンロードするバイト数)。
- **インジケーターをアニメーション**化する-ユーザーは、継続したアクティビティインジケーターを停滞したアプリに関連付けるため、インジケーターが表示されている間は常にインジケーターをアニメーション化しておく必要があります。
- **処理中のタスクについて説明**します。アクティビティインジケーターだけを表示するだけでは十分ではありません。ユーザーは、待機しているプロセスについて通知する必要があります。 タスクを明確に定義するわかりやすいラベル (通常は1つの完全な文) を含めます。

### <a name="implementing-an-activity-indicator"></a>アクティビティインジケーターの実装

アクティビティインジケーターは、`UIActivity` が発生していることを示すために[`UIActivityIndictorView`](xref:UIKit.UIActivityIndicatorView)クラスによって実装されます。

### <a name="activity-indicators-and-storyboards"></a>アクティビティインジケーターとストーリーボード

IOS Designer を使用して UI を作成する場合は、[ツールボックス] からアクティビティインジケーターをレイアウトに追加できます。 Properties Pad から次のプロパティを調整できます。

![Properties Pad](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>アクティビティインジケーターの動作の管理

`StartAnimating()` メソッドと `StopAnimating()` メソッドを使用して、アクティビティインジケーターアニメーションを開始または停止します。

`StopAnimating()` が呼び出された後にアクティビティインジケーターが非表示になるようにするには、`HidesWhenStopped` プロパティを `true` に設定します。 既定では、これは `true` に設定されています。 任意の時点で、`IsAnimating` プロパティをチェックすることで、アクティビティインジケーターがスピン中のアニメーションを実行しているかどうかを確認できます。 

### <a name="managing-activity-indicator-appearances"></a>アクティビティインジケーターの外観を管理する

`UIActivityIndicatorViewStyle` 列挙体は、アクティビティインジケーターをインスタンス化するときにパラメーターとして渡すことができます。 これを使用すると、visual スタイルを `Gray`、`White`、または `WhiteLarge`に設定できます。次に例を示します。

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

`UIActivityIndicatorViewStyle` によって提供される色は、`Color` プロパティを設定することによってオーバーライドできます。

## <a name="progress-bar"></a>進行状況バー

進行状況バーは、時間のかかるタスクの状態と長さを示すために色で塗りつぶす線として表示されます。 タスクの長さがわかっている場合、または計算できる場合は、進行状況バーを常に使用する必要があります。

Apple では、進行状況バーの操作に関して次のような推奨事項があります。

- **進行状況を正確に報告**する-進行状況バーは、タスクを完了するために必要な時間の正確な表現である必要があります。 アプリがビジー状態として表示されるまでの時間を詐称しないでください。
- **明確に定義された期間に使用**します。進行状況バーには、時間のかかるタスクが発生したことを示すだけでなく、タスクが完了したかどうかと、残存時間の見積もりが表示されます。

### <a name="implementing-an-progress-bar"></a>プログレスバーの実装

進行状況バーは、 [`UIProgressView`](xref:UIKit.UIProgressView)をインスタンス化することによって作成されます。

### <a name="progress-bars-and-storyboards"></a>進行状況バーとストーリーボード

IOS Designer を使用しているときに、進行状況バーを UI に追加することもできます。 **ツールボックス**で**進行状況ビュー**を検索し、ビューにドラッグします。

プロパティパッドでは、次のプロパティを調整できます。

![Properties Pad](progress-activity-indicator-images/progress-indicator3.png)

### <a name="managing-progress-bar-behavior"></a>進行状況バーの動作の管理

バーの進行状況は、最初に `Progress` プロパティを使用して設定できます。

```csharp
ProgressBar.Progress = 0f;
```

進行状況は、`SetProgress` メソッドを使用して、変更をアニメーション化するかどうかを示すブール値を渡すことによって調整できます。

```csharp
ProgressBar.SetProgress(1.0f, true);
```

進行状況バーの使用方法の詳細については、「[進行状況の報告](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress)」レシピと[UICatalog tvOS サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-uicatalog)を参照してください。

### <a name="managing-progress-bar-appearance"></a>進行状況バーの外観の管理

アクティビティインジケーターと同様に、進行状況バーをインスタンス化するときに、`UIProgressViewStyle` 列挙体をパラメーターとして渡すことができます。

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
