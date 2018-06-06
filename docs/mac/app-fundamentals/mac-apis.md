---
title: macOS Xamarin.Mac 開発者向けの Api
description: このドキュメントでは、OBJECTIVE-C セレクターを読み取る方法および Xamarin.Mac アプリでは、対応する c# のメソッドを検索する方法について説明します。
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: cceaa2f6e89b712be5929f7e978663d8c47f18c5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791551"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS Xamarin.Mac 開発者向けの Api

## <a name="overview"></a>概要

Xamarin.Mac による開発時間の大部分と思われる、読み取り、および基になる Objective C Api と程度を気にせず、c# で記述することができます。 ただし、場合がありますを必要があります、Apple から API のドキュメントを読み取る、問題のソリューションにスタック オーバーフローからの応答を変換または比較する既存のサンプルにします。

## <a name="reading-enough-objective-c-to-be-dangerous"></a>危険であることのための十分な Objective C の読み取り

Objective C 定義の読み取りに必要なすることがあります。 またはメソッドを呼び出すし、同等の c# のメソッドに変換します。 Objective C 関数定義を見てし、断片に分割してみましょう。 このメソッド (、*セレクター* Objective c) にある  `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

宣言は、右側に左読み取ることができます。

- `-`プレフィックスは、(静的ではない) インスタンス メソッドであることを意味します。 +、その値が、クラス (静的) メソッド
- `(BOOL)` 戻り値の型 (c# でのブール値)
- `canDragRowsWithIndexes` 名前の最初の部分です。
- `(NSIndexSet *)rowIndexes` 最初のパラメーターは、し、それには入力します。 最初のパラメーターは、形式です。 `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` 2 番目のパラメーターとその型です。 形式を最初より後のすべてのパラメーターには。 `selectorPart:(Type) pararmName`
- このメッセージ セレクタの完全な名前が:`canDragRowsWithIndexes:atPoint:`です。 注、`:`最後のことが重要です。
- 実際の Xamarin.Mac c# バインド。 `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

このセレクター呼び出しには、同じ方法を読み取ることができます。

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- インスタンス`v`があるその`canDragRowsWithIndexes:atPoint`セレクターの 2 つのパラメーターと呼ばれる`set`と`point`、渡されました。
- C# の場合は、メソッドの呼び出しは次のようになります。 `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>指定されたセレクターの c# メンバーを検索します。

呼び出す必要があります、OBJECTIVE-C セレクターが見つかったので、これで、次の手順へのマッピングを同等の c# のメンバーです。 4 つのアプローチを試みることができますが (続行、`NSTableView CanDragRows`例)。

1. 同じ名前のものを簡単にスキャンする、自動補完の一覧を使用します。 インスタンスであることがわかっているため`NSTableView`入力することができます。

    - `NSTableView x;`
    - `x.` [ctrl + 領域一覧が表示されない場合)。
    - `CanDrag` [入力]
    - メソッドを右クリックし、比較できるアセンブリ ブラウザーを開くための宣言へ移動、`Export`セレクターの対象の属性

2. クラス全体のバインドを検索します。 インスタンスであることがわかっているため`NSTableView`入力することができます。

    - `NSTableView x;`
    - 右クリック`NSTableView`アセンブリのブラウザーに宣言へ移動
    - 問題のセレクターを検索します。

3. 使用することができます、 [Xamarin.Mac API のオンライン ドキュメント](https://developer.xamarin.com/api/root/monomac-lib/)です。

4. ある Miguel Xamarin.Mac Api の「ラシード駒」ビューを提供する[ここ](http://tirania.org/tmp/rosetta.html)を指定された API を探すことができます。 API が AppKit または macOS 固有でない場合があります見つけることがあります。

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
