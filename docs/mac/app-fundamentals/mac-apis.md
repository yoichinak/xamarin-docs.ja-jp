---
title: macOS Xamarin.Mac 開発者向けの Api
description: このドキュメントでは、OBJECTIVE-C セレクターを読み取る方法と、Xamarin.Mac アプリで c# の対応するメソッドを検索する方法について説明します。
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/02/2017
ms.openlocfilehash: c387bbead1ac56d7f4c4c05a79c430302e50aec1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085282"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS Xamarin.Mac 開発者向けの Api

## <a name="overview"></a>概要

Xamarin.Mac を使って開発時間の大部分を考えて、読み取り、および基になる Objective C Api を使用した量に関係なく、c# で記述することができます。 ただし、場合があります必要がありますを apple の API のドキュメントを読み取る、問題のソリューションには、Stack Overflow から回答を変換したりする既存のサンプルと比較します。

## <a name="reading-enough-objective-c-to-be-dangerous"></a>危険であるための十分な OBJECTIVE-C の読み取り

Objective C の定義を参照する必要する場合があります。 またはメソッドの呼び出しし、同等の c# メソッドに変換します。 Objective C 関数の定義を参照してください、部分を分割してみましょう。 このメソッド (、*セレクター* Objective C で) にある  `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

宣言は、右側に左に読み取ることができます。

- `-`プレフィックス (静的ではない) インスタンス メソッドであることを意味します。 +、その値がクラス (静的) メソッド
- `(BOOL)` 戻り値の型 (c# でのブール値) には
- `canDragRowsWithIndexes` 名前の最初の部分です。
- `(NSIndexSet *)rowIndexes` 最初のパラメーターの型します。 最初のパラメーターは、形式です。 `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` 2 番目のパラメーターとその型です。 1 つ目の後にすべてのパラメーターは、形式を示します。 `selectorPart:(Type) pararmName`
- このメッセージ セレクタの完全な名前が:`canDragRowsWithIndexes:atPoint:`します。 注、 `:` - 最後にあることが重要です。
- 実際の Xamarin.Mac c# バインドは次のとおりです。 `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

このセレクターの呼び出しは、同じように読むことができます。

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- インスタンス`v`があるその`canDragRowsWithIndexes:atPoint`セレクターの 2 つのパラメーターと呼ばれる`set`と`point`、渡されました。
- C# では、メソッドの呼び出しはようになります。 `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>指定されたセレクターの c# メンバーを検索

呼び出す必要があります、OBJECTIVE-C セレクターが見つかり、これで、次の手順へのマッピングを同等の c# のメンバー。 4 つの方法を試みることができますが (進める、`NSTableView CanDragRows`例)。

1. 同じ名前のものを簡単にスキャンする、自動補完の一覧を使用します。 インスタンスであることがわかっているため`NSTableView`入力することができます。

    - `NSTableView x;`
    - `x.` [ctrl + スペースの一覧が表示されない場合)。
    - `CanDrag` [入力]
    - メソッドを右クリックし、比較できるアセンブリ ブラウザーを開くための宣言に移動、`Export`属性セレクターの問題を

2. クラス全体のバインディングを検索します。 インスタンスであることがわかっているため`NSTableView`入力することができます。

    - `NSTableView x;`
    - 右クリックして`NSTableView`、アセンブリ ブラウザーに宣言へ移動
    - 問題のセレクターを検索します。

3. 使用することができます、 [Xamarin.Mac API のオンライン ドキュメント](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0)します。

4. Miguel Xamarin.Mac Api の「ロゼッタ ストーン」ビューを提供する[ここ](https://tirania.org/tmp/rosetta.html)を指定の API を検索することができます。 API が AppKit または macOS 固有でない場合があります見つけることがあります。

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
