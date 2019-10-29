---
title: Xamarin. Mac 開発者向け macOS Api
description: このドキュメントでは、目的 C のセレクターを読み取る方法と、それらにC#対応するメソッドを Xamarin. Mac アプリで検索する方法について説明します。
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/02/2017
ms.openlocfilehash: cd427d13bb79fd31e1e814726aaaf61788ae10ec
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030077"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>Xamarin. Mac 開発者向け macOS Api

## <a name="overview"></a>概要

Xamarin. Mac を使用した開発時間の大部分では、基礎となる目標 C C# api に関してあまり心配をすることなく、お客様が検討、読み取り、および書き込みを行うことができます。 ただし、場合によっては、Apple から API ドキュメントを読み取り、Stack Overflow からの回答を問題の解決策に変換するか、既存のサンプルと比較する必要があります。

## <a name="reading-enough-objective-c-to-be-dangerous"></a>目標を達成する-C を危険にする

場合によっては、目的の C の定義またはメソッドの呼び出しを読み取り、それをC#同等のメソッドに変換する必要があります。 それでは、C 言語の関数定義を見てみましょう。 このメソッド (目的-C の*セレクター* ) は `NSTableView`にあります。

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

宣言は左から右に読むことができます。

- `-` プレフィックスは、インスタンス (非静的) メソッドであることを意味します。 + は、クラス (静的) メソッドであることを意味します。
- `(BOOL)` は戻り値の型です ( C#bool in)
- 名前の最初の部分は `canDragRowsWithIndexes` です。
- `(NSIndexSet *)rowIndexes` は、最初のパラメーターであり、型はです。 最初のパラメーターの形式は次のとおりです: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` は、2番目のパラメーターとその型です。 最初のパラメーターの後にあるすべてのパラメーターは、次の形式になります: `selectorPart:(Type) pararmName`
- このメッセージセレクターの完全な名前は、`canDragRowsWithIndexes:atPoint:`です。 最後に `:` に注意する必要があります。
- 実際の Xamarin. Mac C#バインドは次のとおりです:`bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

このセレクター呼び出しは、同じ方法で読み取ることができます。

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- インスタンス `v` では、`set` と `point`の2つのパラメーターを使用して、`canDragRowsWithIndexes:atPoint` セレクターが呼び出されています。
- でC#は、メソッドの呼び出しは次のようになります。`x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>特定のC#セレクターのメンバーを検索する

これで、呼び出す必要がある目的の C セレクターが見つかりました。次の手順では、それをC#同等のメンバーにマップします。 次の4つの方法を試してみることができます (`NSTableView CanDragRows` の例をご覧ください)。

1. オートコンプリートの一覧を使用して、同じ名前の何かをすばやくスキャンします。 `NSTableView` のインスタンスであることがわかったので、次のように入力できます。

    - `NSTableView x;`
    - `x.` します。リストが表示されない場合は、ctrl + space キーを押します。
    - `CanDrag` [入力]
    - メソッドを右クリックし、[宣言] に移動して、アセンブリブラウザーを開きます。ここで、`Export` の属性を、問題のセレクターと比較できます。

2. クラスバインド全体を検索します。 `NSTableView` のインスタンスであることがわかったので、次のように入力できます。

    - `NSTableView x;`
    - `NSTableView`を右クリックし、[アセンブリブラウザーへの宣言] にアクセスします。
    - 問題のセレクターを検索します

3. [Xamarin. MAC API オンラインドキュメント](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0)を使用できます。

4. Miguel では、特定の API に対して検索できる、Xamarin. [Mac api の](https://tirania.org/tmp/rosetta.html)"ロゼッタストーン" ビューを提供しています。 API が AppKit または macOS 固有でない場合は、そこにあることがわかります。

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
