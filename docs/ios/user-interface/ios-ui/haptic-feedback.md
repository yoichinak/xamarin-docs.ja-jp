---
title: Xamarin.iOS で Haptic フィードバックを提供します。
description: このドキュメントでは、Xamarin.iOS アプリで haptic フィードバックを提供する方法について説明します。 UIImpactFeedbackGenerator、UINotificationFeedbackGenerator、および UISelectionFeedbackGenerator についても説明します。
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d0dae6d6f50423474fbfebad5d630000e2160f6a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790189"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Xamarin.iOS で Haptic フィードバックを提供します。

<a name="Overview" />

## <a name="overview"></a>概要

IPhone 7 と iPhone で 7 さらに、Apple が含まれる、ユーザーが物理的に参加するその他の方法を提供する新しい haptic 応答します。 Haptic フィードバックが (単に Haptics とも呼ばれる) は、ユーザー インターフェイスの設計でタッチ (またはを使用して強制、振動モーション) の意味を使用します。 ユーザーの注意を取得し、それらのアクションを強化するには、これらの新しい触るフィードバック オプションを使用します。

次のトピックで、詳しく説明します。

- [Haptic フィードバックについて](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Haptic フィードバックについて

いくつかの組み込み UI 要素は、既にピッカー、スイッチおよびスライダーなど haptic フィードバックを提供します。 iOS 10 には、今すぐプログラムでの具体的なサブクラスを使用して haptics をトリガーする機能が追加されて、`UIFeedbackGenerator`クラスです。

開発者は、次のいずれかを使用して`UIFeedbackGenerator`サブクラスはトリガー haptic フィードバック プログラムで。

- `UIImpactFeedbackGenerator` -アクションまたはビューが所定の位置にスライドまたは 2 つの画面に表示されるオブジェクトが競合する場合は、"thud"を表示するなどのタスクを補完するために、このフィードバック ジェネレーターを使用します。
- `UINotificationFeedbackGenerator` -警告のアクションの完了、失敗またはその他の種類などの通知をこのフィードバック ジェネレーターを使用します。
- `UISelectionFeedbackGenerator` -このフィードバック ジェネレーターを一覧から項目をピッキングなどアクティブに変更を選択するために使用します。

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

アクションまたはビューが所定の位置にスライドまたは 2 つの画面に表示されるオブジェクトが競合する場合は、"thud"を表示するなどのタスクを補完するのにには、このフィードバック ジェネレーターを使用します。

トリガーの影響からのフィードバックには、次のコードを使用します。

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

開発者がの新しいインスタンスを作成するときに、`UIImpactFeedbackGenerator`クラスを提供する、`UIImpactFeedbackStyle`としてフィードバックの強度を指定します。

- `Heavy`
- `Medium`
- `Light`

`Prepare`のメソッド、 `UIImpactFeedbackGenerator` haptic フィードバックがしている待機時間を最小限に抑えることができるように、システムに通知するために呼び出されます。

`ImpactOccurred`メソッド haptic フィードバックをトリガーします。

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

警告のアクションの完了、失敗またはその他の種類などの通知をこのフィードバック ジェネレーターを使用します。

トリガーの通知からのフィードバックには、次のコードを使用します。

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

新しいインスタンス、`UINotificationFeedbackGenerator`クラスが作成されると、その`Prepare`haptic フィードバックがしている待機時間を最小限に抑えることができるようにシステムに通知するメソッドが呼び出されます。

`NotificationOccurred` Haptic フィードバックをトリガーするために呼び出される、指定された`UINotificationFeedbackType`の。

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

一覧から項目をピッキングなどアクティブに変更を選択するためには、このフィードバック ジェネレーターを使用します。

トリガーの選択範囲からのフィードバックには、次のコードを使用します。

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

新しいインスタンス、`UISelectionFeedbackGenerator`クラスが作成されると、その`Prepare`haptic フィードバックがしている待機時間を最小限に抑えることができるようにシステムに通知するメソッドが呼び出されます。

`SelectionChanged`メソッド haptic フィードバックをトリガーします。

## <a name="summary"></a>まとめ

この記事は新しい種類 haptic フィードバック Xamarin.iOS でそれらを実装する方法と iOS 10 で使用できるをについて説明します。

## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
