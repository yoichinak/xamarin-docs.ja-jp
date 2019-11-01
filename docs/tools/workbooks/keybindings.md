---
title: Xamarin Workbooks エディターのキーボードショートカット
description: このドキュメントでは、Xamarin Workbooks エディターで使用できるキーボードショートカットについて説明します。 特に、戻り値のキーが使用されるさまざまな方法を見ていきます。
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: f8c76f96ea16b9341d349ca110a3eb39207a4d30
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019103"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin Workbooks エディターのキーボードショートカット

## <a name="the-return-key-and-its-nuances"></a>Return キー、およびそのニュアンス

次の表では、コードを実行して markdown を作成するためのさまざまなキーバインドについて説明します。 なじみのある一貫性のあるキーのバインドを選択することは、よく知られています。

|キーバインド|コードセル|Markdown セル|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>カーソルがセルバッファーの最後にあり、セルを正常に解析できる場合は、そのセルが実行され、結果がバッファーの下に表示されます。新しいコードセルが挿入され、実行されたセルの後にフォーカスが挿入されます。</p><p>解析が成功しなかった場合は、バッファーに新しい行が挿入されます。 解析が失敗した場合、コンパイラ診断は生成されません。</p>|<p>カーソル位置の Markdown コンテキストによって異なる動作が<kbd>返さ</kbd>れます。</p><ul><li>キャレットが Markdown コードブロック内にある場合は、リテラルの新しい行が挿入されます。</li><li>キャレットが Markdown list ブロック内にある場合は、新しいリスト項目を作成するか、現在のリスト項目を分割します。</li><li>キャレットが他の種類の Markdown ブロックに含まれている場合は、新しい段落ブロックを作成するか、現在のブロックを分割します。</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>コマンド-Return</kbd></dd><dt>成立</dt><dd><kbd>制御-戻り値</kbd></dd></dl>|<p>は、常にセルの内容の解析と実行を試みます。 コンパイルが成功すると、結果 (実行例外を含む) がバッファーの下に表示されます。後続のセルがない場合は、新しいセルが作成され、フォーカスが設定されます。</p><p>コンパイルエラーが発生した場合は、診断が表示され、バッファーはカーソル位置を変更せずにフォーカスされたままになります。</p>|現在の markdown cell の後に新しいコードセルを挿入し、それに焦点を当てます。|
|<dl><dt>Mac</dt><dd><kbd>コマンドシフト-戻る</kbd><dd><dt>成立</dt><dd><kbd>制御-シフト-戻る</kbd></dd></dl>|現在のセルの後に新しい markdown セルを挿入してフォーカスを追加します。|<kbd>戻り値</kbd>と同じ動作|
|<kbd>Shift + Return</kbd>|キャレットの位置または内容に関係なく、常に新しい行を挿入します。|現在の Markdown ブロック内にハード改行を挿入します。|
