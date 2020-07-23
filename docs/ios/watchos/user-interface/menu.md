---
title: Xamarin の watchOS Menu コントロール (Force Touch)
description: このドキュメントでは、Xamarin で watchOS force touch ジェスチャを使用する方法について説明します。 また、強制タッチに応答する方法、メニューを追加する方法、およびメニュー項目を変更する方法についても説明します。
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 1a9013c8f3adad0f52c264ea1216cab784d6b58f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939660"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>Xamarin の watchOS Menu コントロール (Force Touch)

Watch Kit は、ウォッチアプリ画面に実装されたときにメニューをトリガーする Force Touch ジェスチャを提供します。

![メニューを表示 Apple Watch には](menu-images/menu.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Force Touch への応答

`Menu`がインターフェイスコントローラーに対して実装されている場合、ユーザーが Force Touch を実行すると、メニューが表示されます。 メニューが実装されていない場合は、他の操作が発生しないように、画面が短時間でアニメーション化されます。

強制タッチは、画面上の特定の要素に関連付けられていません。インターフェイスコントローラーにアタッチできるメニューは1つだけです。このメニューは、画面上での Force Touch の押し方に関係なく表示されます。

1 ~ 4 つのメニューオプションを表示できます。

## <a name="adding-a-menu"></a>メニューの追加

は、 `Menu` デザイン時にストーリーボードのに追加する必要があり `InterfaceController` ます。 メニューコントロールをインターフェイスコントローラー上にドラッグすると、ストーリーボードのプレビューに視覚的な表示はありませんが、**ドキュメントアウトライン**パッドには**メニュー**が表示されます。

![デザイン時のメニューの編集](menu-images/menu-action.png)

メニューコントロールには、最大4つのメニュー項目を追加できます。 これらは、[**プロパティ**] パッドで構成できます。 次の属性を設定できます。

- タイトル、および
- カスタムイメージ、または
- システムイメージ: Accept、Add、Block、辞退、Info、More、More、ミュート、Pause、Play、Repeat、Resume、Share、シャッフル、スピーカー、ごみ箱。

`Action`[**プロパティ**] パッドの [**イベント**] セクションを選択し、アクションメソッドの名前を入力して、を作成します。 部分メソッドは、次のようにインターフェイスコントローラークラスで実装できるコードで作成されます。

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>カスタム イメージ

IOS のタブ画像と同様に、メニュー項目イメージには、背景を表示するためのアルファチャネルを持つ不透明なパターンが必要です。

最適なパフォーマンスを得るには、メニューに使用するイメージを watch アプリのプロジェクト (watch app extension プロジェクトではなく) に追加する必要があります。

## <a name="changing-the-menu-items"></a>メニュー項目の変更

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>追加 (実行時に)

を実行時にインターフェイスコントローラーに追加することはできませんが、 `Menu` のコレクションは `MenuItem` プログラムによって変更*でき*ます。
`AddMenuItem`次に示すように、メソッドを使用します。

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

現在、Xamarin. iOS Watch Kit API では、メソッドのが必要です `selector` 。これは次の `AdMenuItem` ように宣言する必要があります。

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>削除 (実行時に)

メソッドを呼び出して、 `ClearAllMenuItems` *プログラムによって追加された*すべてのメニュー項目を削除できます。

ストーリーボードで構成されているメニュー項目を消去することはできません。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のメニュードキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
