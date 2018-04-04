---
title: 進行状況インジケーターの操作
description: この記事では、設計と Xamarin.tvOS アプリ内での進行状況インジケーターの操作について説明します。
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 96fc3ea0aa802f62bd697b34f7bd504eb445a4f6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-progress-indicators"></a>進行状況インジケーターの操作

_この記事では、設計と Xamarin.tvOS アプリ内での進行状況インジケーターの操作について説明します。_


Xamarin.tvOS アプリが新しいコンテンツの読み込みまたは時間のかかる処理操作を実行する必要がある場合があります。 その時間帯に、アクティビティのインジケーターまたは進行状況バー ユーザーにアプリがまだ実行されていることを通知し、実行中のタスクの長さに関するいくつかを示す値を与えるを提示する必要があります。

[![](progress-indicators-images/intro01.png "サンプルの進行状況インジケーター")](progress-indicators-images/intro01.png#lightbox)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>アクティビティのインジケーターについて

アクティビティのインジケーターは、視覚的にはスピン用の歯車アイコンとして表示し、何らかの長さのタスクを表すために使用します。 タスクを起動し、タスクが完了したときに表示されなくなります、インジケーターが表示されます。

Apple では、アクティビティのインジケーターの操作の次の方法があります。

- **可能な限り、代わりに進行状況バー** - アクティビティのインジケーターは、ユーザー実行中のプロセスにかかる、に関するフィードバックがないため、長さが (たとえば、ファイルをダウンロードするバイト数) がわかっている場合、進行状況バーを常に使用します。
- **インジケーター アニメーション保持**-ユーザーに関連して定常化アクティビティのインジケーター アプリを停止しているため、常に、インジケーターは表示されているときのアニメーションが必要です。
- **処理中のタスクについて説明する**-だけを単独で、アクティビティのインジケーターを表示するには、ユーザーを待機しているプロセスに通知する必要があります。 タスクを明確に定義される意味のあるラベル (通常は単一の完全な文) が含まれます。

<a name="Summary" />

## <a name="about-progress-bars"></a>進行状況バーの概要

進行状況バーは、状態と時間のかかるタスクの長さを示す色で塗りつぶしますを線として表示します。 進行状況バーは、タスクの長さ認識はまたはを計算するときに常に使用する必要があります。

Apple では、進行状況バーを操作するための以下の推奨事項があります。

- **正確に進行状況をレポート**-進行状況バーには、タスクの完了に必要な時間の正確に表した必要があります。 ビジー状態で表示されるアプリの作成にかかる時間を偽ることことはありません。
- **Well-Defined 期間にわたって使用**-進行状況バーのみを表示しない時間のかかるタスクを要している配置が、ユーザーとどの程度タスクの完了を示す値と残り時間の推定値を指定します。

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>進行状況インジケーターとストーリー ボード

Xamarin.tvOS アプリでは、プログレス インジケーターを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**アクティビティのインジケーター**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](progress-indicators-images/activity01.png "アクティビティのインジケーター")](progress-indicators-images/activity01.png#lightbox)
1. **ウィジェット タブ**の**プロパティ パッド**など、アクティビティのインジケーターのいくつかのプロパティを調整することができます、**スタイル**と**動作**: 

    [![](progress-indicators-images/activity02.png "ウィジェット タブ ")](progress-indicators-images/activity02.png#lightbox)
1. ドラッグ、**進行状況のビュー**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](progress-indicators-images/activity03.png "進行状況の表示")](progress-indicators-images/activity03.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**など、進行状況のビューのいくつかのプロパティを調整することができます、**スタイル**と**の進行状況**(% 完了)。 

    [![](progress-indicators-images/activity04.png "ウィジェット タブ")](progress-indicators-images/activity04.png#lightbox)
1. 最後に、割り当てる**名**コントロールに c# コードでそれらに応答できるようにします。 例えば: 

    [![](progress-indicators-images/activity05.png "名前を割り当てる")](progress-indicators-images/activity05.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**アクティビティのインジケーター**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](progress-indicators-images/activity01-vs.png "アクティビティのインジケーター")](progress-indicators-images/activity01-vs.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**など、アクティビティのインジケーターのいくつかのプロパティを調整することができます、**スタイル**と**の動作**: 

    [![](progress-indicators-images/activity02-vs.png "ウィジェット タブ")](progress-indicators-images/activity02-vs.png#lightbox)
1. ドラッグ、**進行状況のビュー**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](progress-indicators-images/activity03-vs.png "進行状況の表示")](progress-indicators-images/activity03-vs.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**など、進行状況のビューのいくつかのプロパティを調整することができます、**スタイル**と**の進行状況**(% 完了)。 

    [![](progress-indicators-images/activity04-vs.png "ウィジェット タブ")](progress-indicators-images/activity04-vs.png#lightbox)
1. 最後に、割り当てる**名**コントロールに c# コードでそれらに応答できるようにします。 例えば: 

    [![](progress-indicators-images/activity05-vs.png "名前を割り当てる")](progress-indicators-images/activity05-vs.png#lightbox)
1. 変更内容を保存します。

-----

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>アクティビティのインジケーターの操作

前述のように、アプリには、長いプロセスが実行されているが、正確な時間の長さが必要なタスクがわからない場合にアクティビティのインジケーターは表示されます。

任意の時点、アクティビティのインジケーターにチェックして、回転アニメーションが実行されているかどうかを参照してください、`IsAnimating`プロパティです。 場合、`HidesWhenStopped`プロパティは`true`アクティビティのインジケーターは自動的に非表示にする、アニメーションが停止したときにします。

次のコードを使用して、アニメーションを開始することができます。 

```csharp
ActivityIndicator.StartAnimating();
```

次は、アニメーションを停止します。

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>進行状況バーの操作

ここでも、進行状況バーにアプリがな期間の実行時間の長いタスクを実行して任意の時間を使用してください。 

`Progress` 0% から 100% (0.0 ~ 1.0) を完了した作業の量を設定するプロパティを使用します。 使用して、`ProgressTintColor`完了容量バーの色を設定するプロパティと`TrackTintColor`(未完了量) の背景色を設定するプロパティです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内での進行状況インジケーターの操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
