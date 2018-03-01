---
title: "属性を確認してください。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: cda523cd9d762c3a3c1570e2abd0acb8a264d5dd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="verify-attributes"></a>属性を確認してください。


目標ペンを使わずによって生成されたバインディングは注釈を付けることも珍しく、`[Verify]`属性。 これらの属性を示す必要がある_確認_目標ペンを使わずが元の C/Objective C 宣言 (にバインドされている宣言の上にコメントが表示されます) を使用してバインディングを比較することによって、正しいことをでした。

検証をお勧め_すべて_宣言、バインドされているが、最も可能性の高い_必要な_宣言注釈が付けられたため、`[Verify]`属性。 これは、多くの状況がないため十分なメタデータ最適バインディングを作成する方法を推測する元のネイティブのソース コードでです。 ドキュメントまたは最適なバインディング決定を行うヘッダー ファイル内のコードのコメントを参照する必要があります。

修正、バインドがあることを確認するか、正しいように修正が_削除_、`[Verify]`のバインドからの属性です。

> [!IMPORTANT]
> `[Verify]` 属性では、結合を確認する必要があるように、c# コンパイル エラーが意図的に発生します。 削除する必要があります、`[Verify]`確認 (し修正可能性があります)、コードとの属性です。

## <a name="verify-hints-reference"></a>ヒントの参照を検証します。

属性に指定されたヒント引数は、以下のドキュメントで参照されている、交差で使用できます。 生成される任意のドキュメント`[Verify]`属性は、バインディングが完了した後もコンソールに提供されます。

<table>
  <thead>
  <tr>
    <th>ヒントを確認してください。</th>
    <th>説明</th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td>InferredFromPreceedingTypedef</td>
    <td>この宣言の名前がから共通の規約によって推定された、すぐに先行<code>typedef</code>元のソースのネイティブ コードにします。 推論された名前がこの規則は、あいまいな正しいことを確認します。</td>
  </tr>
  <tr>
    <td>ConstantsInterfaceAssociation</td>
    <td>どの Objective C インターフェイスで、extern 変数宣言が、関連付けられている可能性がありますを決定する確実な方法はありません。 これらのインスタンスとしてバインド<code>[Field]</code>が近くの具体的なインターフェイスに '定数' をなくなる可能性がありますより直感的な API を生成するために部分的なインターフェイスでプロパティがまったくインターフェイスです。</td>
  </tr>
  <tr>
    <td>MethodToProperty</td>
    <td>Objective C メソッドは、パラメーターを使用していないと、値 (非 void の戻り値) を返すなどの規則のための c# プロパティとしてバインドされました。 上記のような多くの場合、メソッドより良い API を表示するプロパティとしてバインドする必要がありますが、偽陽性が行われたりとメソッド、実際には、バインドします。</td>
  </tr>
  <tr>
    <td>StronglyTypedNSArray</td>
    <td>ネイティブ<code>NSArray*</code>としてバインドされました<code>NSObject[]</code>です。 API のドキュメント (例: ヘッダー ファイル内のコメント) を使用して設定の期待に基づくバインドの配列をより厳密に型できる可能性がありますか、テストによって、配列の内容を確認します。 たとえば、NSArray * が含まれているのみ NSNumber * instancescan としてバインドしなければ<code>NSNumber[]</code>の代わりに<code>NSObject[]</code>です。</td>
  </tr>
  </tbody>
</table>

ヒントを使用するためのドキュメントを迅速に受け取ることができます、`sharpie verify-docs`ツール、たとえば。

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

