---
title: Xamarin の watchOS Menu コントロール (Force Touch)
description: このドキュメントでは、Xamarin で watchOS force touch ジェスチャを使用する方法について説明します。 また、強制タッチに応答する方法、メニューを追加する方法、およびメニュー項目を変更する方法についても説明します。
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 497ac23ae6fe094b8049ac1b3460d327716e4ece
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291678"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>Xamarin の watchOS Menu コントロール (Force Touch)

Watch Kit は、ウォッチアプリ画面に実装されたときにメニューをトリガーする Force Touch ジェスチャを提供します。

![](menu-images/menu.png "メニューを表示 Apple Watch には")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Force Touch への応答

`Menu`がインターフェイスコントローラーに対して実装されている場合、ユーザーが Force Touch を実行すると、メニューが表示されます。 メニューが実装されていない場合は、他の操作が発生しないように、画面が短時間でアニメーション化されます。

強制タッチは、画面上の特定の要素に関連付けられていません。インターフェイスコントローラーにアタッチできるメニューは1つだけです。このメニューは、画面上での Force Touch の押し方に関係なく表示されます。

1 ~ 4 つのメニューオプションを表示できます。


## <a name="adding-a-menu"></a>メニューの追加

は`Menu` 、デザイン時にストーリー `InterfaceController`ボードのに追加する必要があります。 メニューコントロールをインターフェイスコントローラー上にドラッグすると、ストーリーボードのプレビューに視覚的な表示はありませんが、**ドキュメントアウトライン**パッドには**メニュー**が表示されます。

![](menu-images/menu-action.png "デザイン時のメニューの編集")

メニューコントロールには、最大4つのメニュー項目を追加できます。 これらは、 **[プロパティ]** パッドで構成できます。 次の属性を設定できます。

- タイトル、および
- カスタムイメージ、または
- システムイメージ:Accept、Add、Block、辞退、Info、More、More、ミュート、Pause、Play、Repeat、Resume、Share、シャッフル、スピーカー、ごみ箱。

[プロパティ`Action` ] パッドの **[イベント]** セクションを選択し、アクションメソッドの名前を入力して、を作成します。 部分メソッドは、次のようにインターフェイスコントローラークラスで実装できるコードで作成されます。

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>カスタムイメージ

IOS のタブ画像と同様に、メニュー項目イメージには、背景を表示するためのアルファチャネルを持つ不透明なパターンが必要です。

最適なパフォーマンスを得るには、メニューに使用するイメージを watch アプリのプロジェクト (watch app extension プロジェクトではなく) に追加する必要があります。


## <a name="changing-the-menu-items"></a>メニュー項目の変更

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>追加 (実行時に)

を`Menu`実行時にインターフェイスコントローラーに追加することはできませんが、の`MenuItem`コレクションはプログラムによって変更*でき*ます。
次に`AddMenuItem`示すように、メソッドを使用します。

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

現在、Xamarin. iOS Watch Kit API では`selector` 、 `AdMenuItem`メソッドのが必要です。これは次のように宣言する必要があります。

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>削除 (実行時に)

メソッド`ClearAllMenuItems`を呼び出して、*プログラムによって追加された*すべてのメニュー項目を削除できます。

ストーリーボードで構成されているメニュー項目を消去することはできません。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のメニュードキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
