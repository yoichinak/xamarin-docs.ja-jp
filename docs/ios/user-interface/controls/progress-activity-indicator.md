---
title: 進行状況と Xamarin.iOS でのアクティビティのインジケーター
description: このドキュメントでは、Xamarin.iOS で進行状況とアクティビティのインジケーターを使用する方法について説明します。 これには、プログラムとストーリー ボードの両方に使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: cb56af300444020a543c16afb0dfb48015fc2153
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102595"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>進行状況と Xamarin.iOS でのアクティビティのインジケーター

長時間実行するために、アプリがある可能性がありますを読み込んで、データと、この遅延が、UI の更新で遅延が発生する処理などのタスクを実行します。 この期間中には、常に、システムがビジー状態の作業を行うことをユーザーに安心して進行状況インジケーターを使用してください。 これにより、ユーザー コントロールは、その入力を待機していませんが、その要求でアプリが動作しているを待機するが、正確にどのくらいの期間の詳細を示す手段を提供できます。

iOS アプリでこの進行状況の表示を提供する 2 つの主な方法を提供します。 アクティビティのインジケーター (など、特定_ネットワーク_アクティビティのインジケーター) と進行状況バー。

## <a name="activity-indicator"></a>アクティビティのインジケーター

アプリには、長いプロセスの実行時間が必要なタスクの正確な長さがわからない場合、アクティビティのインジケーターを表示する必要があります。

Apple では、アクティビティのインジケーターを操作するための次の推奨事項があります。

- **可能であれば、代わりに進行状況バー** - アクティビティのインジケーターは、ユーザーか、実行中のプロセスにかかる、フィードバックがないため、長さが (たとえば、ファイルをダウンロードするバイト数) がわかっている場合、進行状況バーを常に使用します。
- **保持インジケーター アニメーション**のためには、変更をアニメーション化するインジケーターを常にする必要があります、ユーザーが停止しているアプリに静止しているアクティビティのインジケーターを関連付けます。
- **処理中のタスクの説明**-自体によってアクティビティのインジケーターを表示するだけでは不十分で、ユーザーを待機しているプロセスに通知する必要があります。 タスクを明確に定義するわかりやすいラベル (通常は単一の完全な文) が含まれます。

### <a name="implementing-an-activity-indicator"></a>アクティビティのインジケーターを実装します。

アクティビティのインジケーターは、によって実装、 [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/)ことを示す、`UIActivity`が行わします。

### <a name="activity-indicators-and-storyboards"></a>アクティビティのインジケーターとストーリー ボード

IOS Designer の UI を作成するを使用している場合、ツールボックスから、アクティビティのインジケーターをレイアウトに追加できます。 プロパティ パッドから、次のプロパティを調整することができます。

![Properties Pad](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>アクティビティのインジケーターの動作の管理

使用して、`StartAnimating()`と`StopAnimating()`メソッドを起動し、アクティビティのインジケーターのアニメーションを停止します。

設定、`HidesWhenStopped`プロパティを`true`するアクティビティのインジケーターが消える`StopAnimating()`が呼び出されました。 設定されているこの`true`既定。 アクティビティのインジケーターにチェックして、回転アニメーションが実行されているかどうかを参照してください、任意の時点で、`IsAnimating`プロパティ。 


### <a name="managing-activity-indicator-appearances"></a>アクティビティのインジケーターの外観を管理します。

`UIActivityIndicatorViewStyle`列挙体は、アクティビティのインジケーターをインスタンス化中にパラメーターとして渡すことができます。 これを使用して、visual スタイルを設定することができます`Gray`、 `White`、または`WhiteLarge`など。

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

によって提供される色をオーバーライドする`UIActivityIndicatorViewStyle`を設定して、`Color`プロパティ。

## <a name="progress-bar"></a>進行状況バー

進行状況バーは、状態と時間のかかるタスクの長さを示す色で塗りつぶしますを線として表示します。 進行状況バーは、タスクの長さが認識またはを計算するときに常に使用する必要があります。

Apple では、進行状況バーを操作するための次の推奨事項があります。

- **正確に進行状況を報告**-進行状況バーがタスクの完了に必要な時間を正確に表したは常にします。 ビジー状態で表示されるアプリの作成にかかる時間を偽ることはありません。
- **Well-Defined 期間にわたって使用**-進行状況バーのみを表示しない時間のかかるタスクの時間が、配置しますが、ユーザーと、タスクの完了を示す値と、残りの期間の推定値です。

### <a name="implementing-an-progress-bar"></a>進行状況バーが実装します。

進行状況バーがインスタンス化によって作成された、 [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>進行状況バーとストーリー ボード

IOS Designer を使用する場合、UI に進行状況バーを追加できます。 検索**進行状況表示**で、**ツールボックス**ビューにドラッグします。

プロパティ パッドで、次のプロパティを調整することができます。

![Properties Pad](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>進行状況バーの動作の管理

使用して最初に、バーの進行状況を設定することができます、`Progress`プロパティ。

```csharp
ProgressBar.Progress = 0f;
```

使用して進行状況を調整することができます、`SetProgress`メソッドとかどうかをアニメーション化、変更するかどうかを宣言するブール値を渡します。

```csharp
ProgressBar.SetProgress(1.0f, true);
```

進行状況バーの使用に関する詳細についてを参照してください、[進行状況の報告](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress)レシピと[UICatalog tvOS サンプル](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/)します。

### <a name="managing-progress-bar-appearance"></a>進行状況バーの外観を管理します。

アクティビティのインジケーターに似ています、`UIProgressViewStyle`列挙体は、進行状況バーをインスタンス化するとき、パラメーターとして渡すことができます。

次のプロパティを使用して進行状況と追跡のイメージと濃淡の色を調整することができます。

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



