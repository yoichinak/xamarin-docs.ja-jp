---
title: Xamarin での Haptic フィードバックの提供
description: このドキュメントでは、haptic のフィードバックを Xamarin iOS アプリに提供する方法について説明します。 ここでは、UIImpactFeedbackGenerator、UINotificationFeedbackGenerator、および Uiselectionフィード Backgenerator について説明します。
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 156af7a5336ac091c0202e38a3a59a32846e281a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003343"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Xamarin での Haptic フィードバックの提供

<a name="Overview" />

## <a name="overview"></a>概要

IPhone 7 と iPhone 7 に加えて、Apple には、ユーザーに物理的に参加するための追加の手段を提供する新しい haptic 応答が含まれています。 Haptic フィードバック (多くの場合、Haptics とも呼ばれます) は、ユーザーインターフェイスの設計でタッチの意味 (force、vibrations、motion を使用) を使用します。 これらの新しい tactile フィードバックオプションを使用して、ユーザーの注意を獲得し、行動を補強します。

次のトピックで、詳しく説明します。

- [Haptic のフィードバックについて](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [Uiselectionフィード Backgenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Haptic のフィードバックについて

いくつかの組み込み UI 要素には、ピッカー、スイッチ、スライダーなどの haptic フィードバックが既に用意されています。 iOS 10 では、`UIFeedbackGenerator` クラスの具象サブクラスを使用して、プログラムによって haptics をトリガーする機能が追加されました。

開発者は、次のいずれかの `UIFeedbackGenerator` サブクラスを使用して、プログラムで haptic のフィードバックをトリガーできます。

- `UIImpactFeedbackGenerator`-このフィードバックジェネレーターを使用して、ビューが配置されたとき、または2つの画面上のオブジェクトが競合する場合に "thud" を表示するなどのアクションまたはタスクを補完します。
- `UINotificationFeedbackGenerator`-このフィードバックジェネレーターを使用して、アクションの完了、失敗、またはその他の種類の警告などの通知を行います。
- `UISelectionFeedbackGenerator`-このフィードバックジェネレーターを使用して、リストから項目を選択するなどのアクティブな変更を行います。

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

開発者が `UIImpactFeedbackGenerator` クラスの新しいインスタンスを作成すると、次のようにフィードバックの強度を指定する `UIImpactFeedbackStyle` が提供されます。

- `Heavy`
- `Medium`
- `Light`

`UIImpactFeedbackGenerator` の `Prepare` メソッドを呼び出して、待機時間を最小限に抑えるために haptic のフィードバックが発生しようとしていることをシステムに通知します。

`ImpactOccurred` メソッドは、haptic フィードバックをトリガーします。

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

`UINotificationFeedbackGenerator` クラスの新しいインスタンスが作成され、待機時間を最小限に抑えるために haptic フィードバックが発生しようとしていることをシステムに通知するために、その `Prepare` メソッドが呼び出されます。

特定の `UINotificationFeedbackType` のを使用して haptic フィードバックをトリガーするために `NotificationOccurred` が呼び出されます。

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

`UISelectionFeedbackGenerator` クラスの新しいインスタンスが作成され、待機時間を最小限に抑えるために haptic フィードバックが発生しようとしていることをシステムに通知するために、その `Prepare` メソッドが呼び出されます。

`SelectionChanged` メソッドは、haptic フィードバックをトリガーします。

## <a name="summary"></a>まとめ

この記事では、iOS 10 で利用できる新しい種類の haptic フィードバックと、それらを Xamarin. iOS に実装する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
