---
title: Xamarin での Haptic フィードバックの提供
description: このドキュメントでは、haptic のフィードバックを Xamarin iOS アプリに提供する方法について説明します。 ここでは、UIImpactFeedbackGenerator、UINotificationFeedbackGenerator、および Uiselectionフィード Backgenerator について説明します。
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9054135713837374dade958b3ccb35cc239bdb94
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655872"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Xamarin での Haptic フィードバックの提供

<a name="Overview" />

## <a name="overview"></a>概要

IPhone 7 と iPhone 7 に加えて、Apple には、ユーザーに物理的に参加するための追加の手段を提供する新しい haptic 応答が含まれています。 Haptic フィードバック (多くの場合、Haptics とも呼ばれます) は、ユーザーインターフェイスの設計でタッチの意味 (force、vibrations、motion を使用) を使用します。 これらの新しい tactile フィードバックオプションを使用して、ユーザーの注意を獲得し、行動を補強します。

次のトピックで、詳しく説明します。

- [Haptic のフィードバックについて](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Haptic のフィードバックについて

いくつかの組み込み UI 要素には、ピッカー、スイッチ、スライダーなどの haptic フィードバックが既に用意されています。 iOS 10 では、 `UIFeedbackGenerator`クラスの具象サブクラスを使用して、プログラムによって haptics をトリガーする機能が追加されました。

開発者は、次`UIFeedbackGenerator`のいずれかのサブクラスを使用して、プログラムで haptic フィードバックをトリガーできます。

- `UIImpactFeedbackGenerator`-このフィードバックジェネレーターを使用して、ビューが配置されたとき、または2つの画面上のオブジェクトが競合する場合に "thud" を表示するなどのアクションまたはタスクを補完します。
- `UINotificationFeedbackGenerator`-アクションの完了、失敗、またはその他の種類の警告などの通知には、このフィードバックジェネレーターを使用します。
- `UISelectionFeedbackGenerator`-リストから項目を選択するなど、アクティブに変更する選択には、このフィードバックジェネレーターを使用します。

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

このフィードバックジェネレーターを使用して、ビューが配置されたとき、または2つの画面オブジェクトが競合する場合に "thud" を表示するなどのアクションまたはタスクを補完します。

影響のフィードバックをトリガーするには、次のコードを使用します。

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

開発者が`UIImpactFeedbackGenerator`クラスの新しいインスタンスを作成すると、次`UIImpactFeedbackStyle`のように、フィードバックの強度を指定するが提供されます。

- `Heavy`
- `Medium`
- `Light`

`Prepare` のメソッドは、待機時間を最小限に抑えるためにhapticフィードバックが発生しようとしていることをシステムに通知するために`UIImpactFeedbackGenerator`呼び出されます。

その`ImpactOccurred`後、メソッドによって haptic フィードバックがトリガーされます。

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

アクションの完了、失敗、またはその他の種類の警告などの通知には、このフィードバックジェネレーターを使用します。

通知のフィードバックをトリガーするには、次のコードを使用します。

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

`UINotificationFeedbackGenerator`クラスの新しいインスタンスが作成され、その`Prepare`メソッドが呼び出されて、待機時間を最小限に抑えるために haptic フィードバックが発生しようとしていることをシステムに通知します。

を`NotificationOccurred`呼び出すと、指定さ`UINotificationFeedbackType`れたで haptic フィードバックがトリガーされます。

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>Uiselectionフィード Backgenerator

リストから項目を選択するなど、アクティブに変更する選択には、このフィードバックジェネレーターを使用します。

次のコードを使用して、選択に関するフィードバックをトリガーします。

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

`UISelectionFeedbackGenerator`クラスの新しいインスタンスが作成され、その`Prepare`メソッドが呼び出されて、待機時間を最小限に抑えるために haptic フィードバックが発生しようとしていることをシステムに通知します。

その`SelectionChanged`後、メソッドによって haptic フィードバックがトリガーされます。

## <a name="summary"></a>Summary

この記事では、iOS 10 で利用できる新しい種類の haptic フィードバックと、それらを Xamarin. iOS に実装する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
