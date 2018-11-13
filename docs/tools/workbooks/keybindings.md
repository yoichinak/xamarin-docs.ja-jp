---
title: Xamarin ブック エディターのキーボード ショートカット
description: このドキュメントでは、Xamarin Workbooks エディターで使用するために使用できるキーボード ショートカットについて説明します。 具体的には、戻り値のキーが使用されるさまざまな方法で検索します。
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 87af9f824117b20250c02a3e070652607626de44
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526144"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin ブック エディターのキーボード ショートカット

## <a name="the-return-key-and-its-nuances"></a>返すキー、およびその微妙な差異

次の表では、コードを実行して、markdown を作成するさまざまなショートカット キーについて説明します。 使い慣れたと流動性の両方が賢明かつ一貫性のあるキー バインドを選択する十分な注意を思い出させています。

|キー バインド|コード セル|Markdown のセル|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>キャレットのセルのバッファーの末尾にあるセルが正常に解析された場合は実行のバッファーを下に結果が表示され、新しいコード セルが挿入され、セルに実行されたセルの後に重点を置いています。</p><p>解析が成功しなかった場合は、バッファーに新しい行が挿入されます。 解析が成功しなかった場合、コンパイラの診断は生成されません。</p>|<p><kbd>返す</kbd>は、キャレット位置 Markdown コンテキストに応じて異なる動作を示します。</p><ul><li>キャレットは、マークダウン コード ブロックでは、リテラルの新しい行が挿入されます。</li><li>キャレットが Markdown 一覧ブロック内にある場合は、新しいリスト アイテムを作成または現在のリスト項目を分割します。</li><li>キャレットが Markdown ブロックの他の任意の型である場合は、新しい段落ブロックを作成または現在のブロックを分割します。</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>常に、解析し、セルの内容の実行を試行します。 バッファーを下に (実行例外を含む) の結果が表示されますコンパイルが成功した場合と後続のセルがない場合、新しいものが作成され重点を置いています。</p><p>コンパイル エラーがある場合、診断が表示され、バッファーにキャレットの位置を変更せずにフォーカスがある残ります。</p>|挿入し、新しいコード セルをマークダウンの現在のセルの後に重点を置いています。|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|挿入し、現在のセルの後に新しい markdown セル重点を置いています。|同じ動作<kbd>を返す</kbd>|
|<kbd>Shift‑Return</kbd>|カレットの場所やコンテンツに関係なく、新しい行を常に挿入します。|現在の Markdown ブロック内で、ハード改行を挿入します。|
