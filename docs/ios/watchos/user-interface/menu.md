---
title: watchOS で Xamarin (Force Touch) メニュー コントロール
description: このドキュメントでは、Xamarin で watchOS force タッチ ジェスチャを使用する方法について説明します。 Force タッチに応答する方法について説明しますが、メニューとメニュー項目を変更する追加する方法。
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7696c820ab6fdf19bdef46db31061fb5914e6cf4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109758"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchOS で Xamarin (Force Touch) メニュー コントロール

キットには、watch アプリの画面で実装された場合、メニューをトリガーする強制タッチ ジェスチャをご覧ください。

![](menu-images/menu.png "Apple Watch のメニューを表示")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Force Touch への応答

場合、`Menu`ユーザーは、Force Touch、メニューが表示されますが実行すると、インターフェイス コント ローラーに対して実装されています。 画面が簡単にアニメーション化メニューが実装されていない場合、いないその他のアクションが発生します。

Force タッチは画面で、特定の要素に関連付けられていません。インターフェイス コント ローラーに接続できる 1 つだけのメニューと、画面上の Force Touch キーを押しての発生に関係なく表示されます。

1 つまたは 4 つのメニュー間オプションを表示できます。


## <a name="adding-a-menu"></a>メニューの追加

A`Menu`に追加する必要があります、`InterfaceController`デザイン時にストーリー ボード上。 メニュー コントロールが、インターフェイス コント ローラーにドラッグしたときは、ストーリー ボードのプレビューで表示はありませんが、**メニュー**に表示されます、**ドキュメント アウトライン**パッド。

![](menu-images/menu-action.png "デザイン時にメニューの編集")

最大で 4 つのメニュー項目は、メニュー コントロールを追加できます。 構成することができますが、**プロパティ**パッド。 次の属性を設定できます。

- タイトル、および
- カスタム イメージ、または
- システム イメージ: 受け入れるには、追加、ブロック、拒否、情報、さらに、ミュート、一時停止、再生、再開、共有、シャッフル、スピーカー、ごみ箱を繰り返します。

作成、`Action`を選択して、**イベント**のセクション、**プロパティ**パッド、アクション メソッドの名前を入力します。 部分メソッドが、コードでは、次のように、インターフェイス コント ローラー クラスで実装できますで作成されます。

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>カスタム イメージ

IOS でのタブの画像と同様に、メニュー項目のイメージは、背景を表示できるようにするアルファ チャネルを持つ非透過のパターンを必要とします。

最適なパフォーマンス watch アプリ プロジェクト (watch アプリ拡張機能プロジェクトではなく) にメニューを使用するイメージを追加する必要があります。


## <a name="changing-the-menu-items"></a>メニュー項目を変更します。

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>実行時に追加します。

発生することはできません、`Menu`ですが、実行時に、インターフェイス コント ローラーに追加するのコレクション`MenuItem`s*できます*プログラムで変更します。
使用して、`AddMenuItem`メソッド。

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

`ClearAllMenuItems`すべてを削除するメソッドを呼び出すことが*プログラムによって追加された*メニュー項目。

ストーリー ボードで構成されているメニュー項目をクリアすることはできません。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のメニューのドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
