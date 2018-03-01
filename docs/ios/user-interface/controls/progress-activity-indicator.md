---
title: "進行状況とアクティビティのインジケーター"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: b91c0d7504b391630beded7a52e240a0d043152f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="progress-and-activity-indicators"></a>進行状況とアクティビティのインジケーター

アプリが時間の長いを実行する必要がある可能性が読み込み中、またはデータと、この遅延が、UI を更新中に遅延が発生する処理などのタスクを実行します。 この期間中には、常をシステムがビジー状態の作業を行っているユーザーを組み込みますプログレス インジケーターを使用する必要があります。 これにより、ユーザー コントロールがその入力を待機していないこと、その要求で、アプリが機能するを待たずに、正確にどのくらいの期間の詳細を示す手段を提供できます。

iOS には、アプリのこの進行状況を示す値を提供する 2 つの主な方法が用意されています: アクティビティのインジケーター (など、特定_ネットワーク_アクティビティのインジケーター) と進行状況バーです。

## <a name="activity-indicator"></a>アクティビティのインジケーター

アプリには、長いプロセスが実行されているが、正確な時間の長さが必要なタスクがわからない場合に、アクティビティのインジケーターを表示するか。

Apple では、アクティビティのインジケーターの操作の次の方法があります。

- **可能な限り、代わりに進行状況バー** - アクティビティのインジケーターは、ユーザー実行中のプロセスにかかる、に関するフィードバックがないため、長さが (たとえば、ファイルをダウンロードするバイト数) がわかっている場合、進行状況バーを常に使用します。
- **インジケーター アニメーション保持**-ユーザーに関連して定常化アクティビティのインジケーター アプリを停止しているため、常に、インジケーターは表示されているときのアニメーションが必要です。
- **処理中のタスクについて説明する**-だけを単独で、アクティビティのインジケーターを表示するには、ユーザーを待機しているプロセスに通知する必要があります。 タスクを明確に定義される意味のあるラベル (通常は単一の完全な文) が含まれます。

### <a name="implementing-an-activity-indicator"></a>アクティビティのインジケーターを実装します。

アクティビティのインジケーターはによって実装され、 [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/)ことを示す、`UIActivity`が行われています。

### <a name="activity-indicators-and-storyboards"></a>アクティビティのインジケーターとストーリー ボード

UI を作成する、iOS デザイナーを使用している場合、アクティビティのインジケーターをツールボックスから、レイアウトに追加できます。 次のプロパティは、プロパティ パッドから調整できます。

![プロパティの埋め込み](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>アクティビティのインジケーターの動作の管理

使用して、`StartAnimating()`と`StopAnimating()`メソッドを開始およびアクティビティのインジケーターのアニメーションを停止します。

設定、`HidesWhenStopped`プロパティを`true`が経過後すると、アクティビティのインジケーターを`StopAnimating()`が呼び出されています。 これは、設定は`true`既定です。 任意の時点、アクティビティのインジケーターにチェックして、回転アニメーションが実行されているかどうかを参照してください、`IsAnimating`プロパティです。 


### <a name="managing-activity-indicator-appearances"></a>アクティビティのインジケーターの外観を管理します。

`UIActivityIndicatorViewStyle`列挙体は、アクティビティのインジケーターをインスタンス化するとき、パラメーターとして渡すことができます。 これを使用して、visual スタイルを設定することができます`Gray`、 `White`、または`WhiteLarge`、例を示します。

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

によって提供される色をオーバーライドする`UIActivityIndicatorViewStyle`を設定して、`Color`プロパティです。

## <a name="progress-bar"></a>進行状況バー

進行状況バーは、状態と時間のかかるタスクの長さを示す色で塗りつぶしますを線として表示します。 進行状況バーは、タスクの長さ認識はまたはを計算するときに常に使用する必要があります。

Apple では、進行状況バーを操作するための以下の推奨事項があります。

- **正確に進行状況をレポート**-進行状況バーには、タスクの完了に必要な時間の正確に表した必要があります。 ビジー状態で表示されるアプリの作成にかかる時間を偽ることことはありません。
- **Well-Defined 期間にわたって使用**-進行状況バーのみを表示しない時間のかかるタスクを要している配置が、ユーザーとどの程度タスクの完了を示す値と残り時間の推定値を指定します。

### <a name="implementing-an-progress-bar"></a>進行状況バーを実装します。

インスタンス化して、進行状況バーを作成します。 [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>進行状況バーとストーリー ボード

IOS デザイナーを使用する場合を UI に進行状況バーを追加することもできます。 検索**進行状況表示**で、**ツールボックス**し、ビューにドラッグします。

プロパティ パッド上、次のプロパティを調整できます。

![プロパティの埋め込み](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>進行状況バーの動作の管理

使用して最初に、バーの進行状況を設定することができます、`Progress`プロパティ。

```csharp
ProgressBar.Progress = 0f;
```

使用して進行状況を調整することができます、`SetProgress`メソッドを宣言するアニメーションか変更するかどうかはブール値を渡すことです。

```csharp
ProgressBar.SetProgress(1.0f, true);
```

進行状況バーを使用する方法についてを参照してください、[進行状況の報告](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS)レシピ、および[UICatalog tvOS サンプル](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/)です。

### <a name="managing-progress-bar-appearance"></a>進行状況バーの外観を管理します。

アクティビティのインジケーターに似ています、`UIProgressViewStyle`列挙体は、進行状況バーをインスタンス化中にパラメーターとして渡すことができます。

次のプロパティを使用して、進行状況とトラック イメージと色の濃淡を調整できます。

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



