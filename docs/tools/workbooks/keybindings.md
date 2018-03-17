---
title: "Xamarin ブック エディターのキーボード ショートカット"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 65424f8c213c17b58f0ce1ad4acc9f6dcdaa192c
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin ブック エディターのキーボード ショートカット

## <a name="the-return-key-and-its-nuances"></a>戻るキー、およびその微妙

次の表では、コードを実行すると、マークダウンを作成、さまざまなショートカット キーについて説明します。 使い慣れたと流体の両方のキー バインドが適切で一貫性のあるを選択する十分な注意を移動しました。

|キーの組み合わせ|コードのセル|マークダウン セル|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>キャレットのセルのバッファーの末尾にあるセルが正常に解析された場合は、アクティビティが実行されます結果は、バッファーを下に表示され、新しいコード セルが挿入され、実行されたセルの後にセルの重点を置きます。</p><p>解析が正常でない場合は、バッファーに新しい行が挿入されます。 解析が成功しなかった場合、コンパイラの診断は生成されません。</p>|<p><kbd>返す</kbd>はキャレット位置マークダウン コンテキストに応じて異なる動作を示します。</p><ul><li>キャレットは、マークダウン コード ブロックには、リテラルの新しい行が挿入されます。</li><li>カレットがマークダウン一覧ブロック内にある場合は、新しい一覧項目の作成や、現在のリスト項目を分割します。</li><li>カレットがマークダウン ブロックの他の任意の型である場合は、新しい段落ブロックを作成または、現在のブロックを分割します。</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>常にしようと解析し、セルの内容を実行します。 バッファーを下 (実行例外を含む) の結果に表示されますコンパイルが成功した場合は、および後続のセルが存在しない場合、新しいものが作成され重点を置きます。</p><p>すべてのコンパイル エラーがある場合は、診断が表示されますされ、変更されていない、カーソル位置でのバッファーがフォーカスのある残ります。</p>|挿入し、新しいコード セル マークダウンの現在のセルの後に焦点を当てています。|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|挿入し、現在のセルの後に新しいマークダウン セル焦点を当てています。|同じ動作<kbd>を返す</kbd>|
|<kbd>Shift‑Return</kbd>|常にカレットの場所またはコンテンツに関係なく、新しい行を挿入します。|現在のマークダウン ブロック内でハード改行を挿入します。|
