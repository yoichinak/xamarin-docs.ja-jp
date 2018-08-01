---
title: watchOS Xamarin 内のメニュー コントロール (Force タッチ)
description: このドキュメントでは、Xamarin で watchOS force タッチ ジェスチャを使用する方法について説明します。 Force タッチに応答する方法について説明しますが、メニューとメニュー項目の変更を追加する方法です。
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4b973b925b99189416087224644c376864c56871
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791346"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchOS Xamarin 内のメニュー コントロール (Force タッチ)

ウォッチ キットは、メニュー watch アプリの画面で実装された場合にトリガーを Force タッチ ジェスチャを提供します。

![](menu-images/menu.png "Apple Watch、メニューを表示")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Force タッチへの応答

場合、 `Menu` Force がタッチ、メニューが表示される、ユーザーを実行するときに、インターフェイス、コント ローラーに対して実装されています。 画面が一時的にアニメーション化メニューが実装されていない場合、いないその他のアクションが発生します。

Force 仕上げが画面上の特定の要素に関連付けられていません。1 つだけのメニューは、インターフェイスのコント ローラーに関連付けることし、Force タッチ キーを押しての画面で発生しているに関係なく表示されます。

1 つおよび 4 つのメニュー間でのオプションを表示できます。


## <a name="adding-a-menu"></a>メニューを追加します。

A`Menu`に追加する必要があります、`InterfaceController`デザイン時に、ストーリー ボード上。 メニュー コントロールがインターフェイスのコント ローラーにドラッグされたときに、ストーリー ボード プレビューの表示はありませんが、**メニュー**に表示されます、 **ドキュメント アウトライン**パッド。

![](menu-images/menu-action.png "デザイン時にメニューの編集")

最大で 4 つのメニュー項目は、メニュー コントロールを追加できます。 構成することができます、**プロパティ**パッドです。 次の属性を設定できます。

- タイトル、および
- カスタム イメージ、または
- システム イメージ: 受け入れるには、追加、ブロック、拒否、については、おそらく、さらに、ミュート、一時停止、再生、繰り返すには、再開、共有、ランダム再生、スピーカー、ゴミ箱です。

作成、`Action`を選択して、**イベント**のセクションで、**プロパティ**パッド、アクション メソッドの名前を入力します。 部分メソッドは、コードでは、次のように、インターフェイス コント ローラー クラスで実装することができますに作成されます。

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>カスタム イメージ

IOS のタブの画像と同様に、メニュー項目のイメージには、背景が透けてを許可するアルファ チャネルを持つ非透過のパターンが必要があります。

最適なパフォーマンスをウォッチ アプリ プロジェクト (watch アプリ拡張機能プロジェクトではない) には、メニューを使用するイメージを追加する必要があります。


## <a name="changing-the-menu-items"></a>メニュー項目を変更します。

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>実行時に追加します。

発生することはできません、`Menu`ですが、実行時にインターフェイス コント ローラーに追加する一連の`MenuItem`s*できます*プログラムで変更します。
使用して、`AddMenuItem`ように、メソッド。

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Xamarin.iOS ウォッチ キット API は現在必要があります、`selector`の`AdMenuItem`メソッドで、次のように宣言する必要があります。

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>実行時に削除します。

`ClearAllMenuItems`メソッドをすべて削除するのに呼び出せる*プログラムによって追加された*メニュー項目。

ストーリー ボードで構成されているメニュー項目をクリアできません。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のメニューのドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
