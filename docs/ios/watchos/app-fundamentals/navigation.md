---
title: Xamarin での watchOS ナビゲーションの使用
description: このドキュメントでは、watchOS アプリケーションでナビゲーションを操作する方法について説明します。 モーダルインターフェイス、階層ナビゲーション、およびページベースのインターフェイスについて説明します。
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: d3565e359ccbad9f7b779969f4273a8cbae4d438
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021743"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>Xamarin での watchOS ナビゲーションの使用

ウォッチで使用できる最も簡単なナビゲーションオプションは、現在のシーンの上に表示される単純な[モーダルポップアップ](#modal)です。

マルチシーンウォッチアプリの場合、次の2つのナビゲーションパラダイムを利用できます。

- [階層ナビゲーション](#Hierarchical_Navigation)
- [ページベースのインターフェイス](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>モーダルインターフェイス

インターフェイスコントローラーをモーダルとして開くには、`PresentController` メソッドを使用します。 インターフェイスコントローラーは、既に**インターフェイスの storyboard**で定義されている必要があります。

```csharp
PresentController ("pageController","some context info");
```

モーダルで表示されるコントローラーは、画面全体を使用します (前のシーンをカバーします)。 既定では、タイトルは **[キャンセル]** に設定され、タップするとコントローラーが破棄されます。

モーダルで表示されたコントローラーをプログラムで閉じるには、`DismissController`を呼び出します。

```csharp
DismissController();
```

モーダル画面には、単一のシーンを使用することも、ページベースのレイアウトを使用することもできます。

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>階層ナビゲーション

は、iOS での `UINavigationController` と同様に、移動可能なスタックのようなシーンを提供します。 シーンは、ナビゲーションスタックにプッシュしてポップすることができます (プログラムによって、またはユーザー選択によって)。

![](navigation-images/hierarchy-1.png "シーンをナビゲーションスタックにプッシュすることができます") ![](navigation-images/hierarchy-2.png "シーンをナビゲーションスタックからポップすることができます")

IOS の場合と同様に、左の端のスワイプは階層型のナビゲーションスタックで親コントローラーに戻ります。

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)と[WatchTables](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)の両方のサンプルには、階層ナビゲーションが含まれています。

### <a name="pushing-and-popping-in-code"></a>コードでのプッシュとポップ

Watch Kit では、iOS と同様に arching "ナビゲーションコントローラー" を作成する必要はありません。 `PushController` メソッドを使用してコントローラーをプッシュするだけで、ナビゲーションスタックが自動的に作成されます。

```csharp
PushController("secondPageController","some context info");
```

ウォッチの画面には左上に **[戻る]** ボタンが表示されますが、`PopController`を使用してナビゲーションスタックからシーンをプログラムで削除することもできます。

```csharp
PopController();
```

IOS と同様に、`PopToRootController`を使用してナビゲーションスタックのルートに戻ることもできます。

```csharp
PopToRootController();
```

### <a name="using-segues"></a>セグエの使用

セグエは、ストーリーボードのシーンの間に作成して、階層ナビゲーションを定義できます。 ターゲットシーンのコンテキストを取得するために、オペレーティングシステムは `GetContextForSegue` を呼び出して新しいインターフェイスコントローラーを初期化します。

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```

<a name="Page-Based_Interfaces"/>

## <a name="page-based-interfaces"></a>ページベースのインターフェイス

ページベースのインターフェイスは、iOS での `UIPageViewController` のように左から右にスワイプされます。 インジケーターのドットが画面の下部に表示され、現在表示されているページが表示されます。

![](navigation-images/paged-1.png "最初のページのサンプル")![](navigation-images/paged-2.png "サンプルの2ページ目")![](navigation-images/paged-5.png "5ページ目のサンプル")

ページベースのインターフェイスを watch アプリのメイン UI にするには、インターフェイスコントローラーとコンテキストの配列を使用して `ReloadRootControllers` を使用します。

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

アプリ内の他のいずれかのシーンから `PresentController` を使用して、ルートではないページベースのコントローラーを提示することもできます。

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WatchTables (サンプル)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchTables/)
