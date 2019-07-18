---
title: Xamarin.iOS で Haptic フィードバックの提供
description: このドキュメントでは、Xamarin.iOS アプリでのハプティクス フィードバックを提供する方法について説明します。 UIImpactFeedbackGenerator、UINotificationFeedbackGenerator、および UISelectionFeedbackGenerator がについて説明します。
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b2c381c59ba1574e80babc2c7e68535a3deffe35
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384620"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Xamarin.iOS で Haptic フィードバックの提供

<a name="Overview" />

## <a name="overview"></a>概要

IPhone 7 および iPhone では、7 Plus、Apple が物理的にユーザーと関わるための追加方法を提供する新しいハプティクス応答を含めるがします。 (単に Haptics とも呼ばれます) のハプティクス フィードバックは、ユーザー インターフェイスのデザインでタッチ (force、振動モーション経由) の意味を使用します。 これらの新しい触るフィードバック オプションを使用して、ユーザーの注意を引くし、そのアクションを補強します。

次のトピックで、詳しく説明します。

- [Haptic フィードバックについて](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Haptic フィードバックについて

いくつかの組み込み UI 要素は、既にピッカー、スイッチ、スライダーなどのハプティクス フィードバックを提供します。 iOS 10 では、プログラムでの具体的なサブクラスを使用して haptics をトリガする機能を追加、`UIFeedbackGenerator`クラス。

開発者は、次のいずれかを使用できます`UIFeedbackGenerator`サブクラスはトリガーのハプティクス フィードバック プログラムを使用します。

- `UIImpactFeedbackGenerator` -アクションまたはビューの場所にスライドしながら、または 2 つの画面に表示されるオブジェクトが衝突する場合は、"thud"を表示するなどのタスクを補完するのに、このフィードバック ジェネレーターを使用します。
- `UINotificationFeedbackGenerator` -通知の警告の処理完了、失敗またはその他の種類など、このフィードバック ジェネレーターを使用します。
- `UISelectionFeedbackGenerator` -このフィードバック ジェネレーターを一覧から項目の選択などを変更する積極的に選択に使用します。

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

このフィードバック ジェネレーターを使用して、アクションまたはビューの場所にスライドしながら、または 2 つの画面に表示されるオブジェクトが衝突する場合は、"thud"を表示するなどのタスクを補完します。

トリガーへの影響からのフィードバックには、次のコードを使用します。

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

`Prepare`のメソッド、`UIImpactFeedbackGenerator`ハプティクス フィードバックは、待機時間を最小限に抑えることができるように実行されるときに、システムに通知すると呼びます。

`ImpactOccurred`メソッドをハプティクス フィードバックをトリガーします。

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

このフィードバック ジェネレーターを使用して、操作完了、失敗またはその他の種類の警告などの通知。

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

新しいインスタンス、`UINotificationFeedbackGenerator`クラスを作成し、その`Prepare`ハプティクス フィードバックは、待機時間を最小限に抑えることができるように実行されるときに、システムに通知するメソッドが呼び出されます。

`NotificationOccurred`のハプティクス フィードバックをトリガーするために呼び出される、指定された`UINotificationFeedbackType`の。

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

このフィードバック ジェネレーターを使用して、一覧から項目の選択などを変更する積極的に選択します。

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

新しいインスタンス、`UISelectionFeedbackGenerator`クラスを作成し、その`Prepare`ハプティクス フィードバックは、待機時間を最小限に抑えることができるように実行されるときに、システムに通知するメソッドが呼び出されます。

`SelectionChanged`メソッドをハプティクス フィードバックをトリガーします。

## <a name="summary"></a>まとめ

この記事では iOS 10 と Xamarin.iOS でそれらを実装する方法で使用できる haptic フィードバックの新しい型をについて説明します。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
