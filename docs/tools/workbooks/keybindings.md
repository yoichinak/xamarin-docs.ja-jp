---
title: "Xamarin ブック エディターのキーボード ショートカット"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: cbbf070a9c94221d98dedcd1df884a897ff7df3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin ブック エディターのキーボード ショートカット

## <a name="the-return-key-and-its-nuances"></a>戻るキー、およびその微妙

次の表では、コードを実行すると、マークダウンを作成、さまざまなショートカット キーについて説明します。 使い慣れたと流体の両方のキー バインドが適切で一貫性のあるを選択する十分な注意を移動しました。

<table>
  <tr>
    <th>キーの組み合わせ</th>
    <th>コードのセル</th>
    <th>マークダウン セル</th>
  </tr>
  <tr>
    <td><kbd>Return</kbd></td>
    <td>
      <p>キャレットのセルのバッファーの末尾にあるセルが正常に解析された場合は、アクティビティが実行されます結果は、バッファーを下に表示され、新しいコード セルが挿入され、実行されたセルの後にセルの重点を置きます。</p>
      
      <p>解析が正常でない場合は、バッファーに新しい行が挿入されます。 解析が成功しなかった場合、コンパイラの診断は生成されません。</p>
    </td>
    <td>
      <p>
        <kbd>返す</kbd>はキャレット位置マークダウン コンテキストに応じて異なる動作を示します。
      </p>
      <ul>
        <li>
キャレットは、マークダウン コード ブロックには、リテラルの新しい行が挿入されます。
        </li>
        <li>
カレットがマークダウン一覧ブロック内にある場合は、新しい一覧項目の作成や、現在のリスト項目を分割します。
        </li>
        <li>
カレットがマークダウン ブロックの他の任意の型である場合は、新しい段落ブロックを作成または、現在のブロックを分割します。
        </li>
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Return</kbd></dd>
      </dl>
    </td>
    <td>
      <p>常にしようと解析し、セルの内容を実行します。 バッファーを下 (実行例外を含む) の結果に表示されますコンパイルが成功した場合は、および後続のセルが存在しない場合、新しいものが作成され重点を置きます。</p>
      
      <p>すべてのコンパイル エラーがある場合は、診断が表示されますされ、変更されていない、カーソル位置でのバッファーがフォーカスのある残ります。</p>
    </td>
    <td>
挿入し、新しいコード セル マークダウンの現在のセルの後に焦点を当てています。
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Shift‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Shift‑Return</kbd></dd>
      </dl>
    </td>
    <td>
挿入し、現在のセルの後に新しいマークダウン セル焦点を当てています。
    </td>
    <td>
同じ動作<kbd>を返す</kbd>
    </td>
  </tr>
  <tr>
    <td><kbd>Shift‑Return</kbd></td>
    <td>
常にカレットの場所またはコンテンツに関係なく、新しい行を挿入します。
    </td>
    <td>
現在のマークダウン ブロック内でハード改行を挿入します。
    </td>
  </tr>
</table>
