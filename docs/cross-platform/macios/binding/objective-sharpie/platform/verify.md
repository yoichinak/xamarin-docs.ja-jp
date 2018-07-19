---
title: 目標油性属性を確認します。
description: このドキュメントでは、目的の油性によって生成された [確認] の属性について説明します。 [確認] 属性は、開発者が目的油性の出力を確認するは手動で場所を強調表示されます。
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855030"
---
# <a name="objective-sharpie-verify-attributes"></a>目標油性属性を確認します。

目的の油性によって生成されたバインドの注釈を使用を多くの場合、検索、`[Verify]`属性。 これらの属性を示す必要がある_確認_目標油性が (これが提供する、バインド宣言の上にコメントで) 元の C/Objective C 宣言とのバインドを比較することによって、正しいことをでした。

検証をお勧め_すべて_宣言、バインドされているが、可能性が最も高い_必要_の宣言の注釈が付けられた、`[Verify]`属性。 これは、多くの状況でがないため十分なメタデータで最適バインディングを作成する方法を推測する元のネイティブなソース コード。 ドキュメントまたは最高のバインドを決定するヘッダー ファイル内のコードのコメントを参照する必要があります。

修正するかが修正されました。 これが正しい場合、バインドがあることを確認する_削除_、`[Verify]`バインディングからの属性。

> [!IMPORTANT]
> `[Verify]` 属性では、バインドを確認する必要があるように、c# のコンパイル エラーが意図的に発生します。 削除する必要があります、`[Verify]`レビュー (し、修正された可能性があります)、コードの属性します。

## <a name="verify-hints-reference"></a>ヒントの参照を検証します。

属性に指定されたヒントの引数は、次のドキュメントで参照されている、クロスで指定できます。 生成されたいずれかをマニュアル`[Verify]`属性は、バインドが完了した後も、コンソールで提供されます。

|`[Verify]` ヒント|説明|
|---|---|
|InferredFromPreceedingTypedef|この宣言の名前は、一般的な慣例から推定された、すぐに上記`typedef`で元のネイティブなソース コード。 推測される名前がこの規則があいまいなために正しいことを確認します。|
|ConstantsInterfaceAssociation|Objective C インターフェイスと extern の変数宣言は関連付けることを確認する確実な方法はありません。 これらのインスタンスとしてバインドされている`[Field]`可能性があります 'Constants' を排除しながらより直感的な API を生成するためにほぼの具体的なインターフェイスに部分的なインターフェイスでプロパティが完全にインターフェイスします。|
|MethodToProperty|Objective C、メソッドは、c# プロパティをパラメーターを受け取らないと、(非 void の戻り値) の値を返すなどの規則が原因としてバインドされました。 上記のような多くの場合、メソッドより良い API を表示するプロパティとしてバインドする必要がありますが、偽陽性が発生することがあり、メソッドを実際には、バインドします。|
|StronglyTypedNSArray|ネイティブ`NSArray*`としてバインドされました`NSObject[]`します。 API のドキュメント (例: ヘッダー ファイル内のコメント) を設定する予測に基づいて、バインディングに配列をより厳密に入力できる可能性がありますまたはテストによって、配列の内容を確認します。 たとえば、NSArray * のみ NSNumber * を含む instancescan としてバインドする`NSNumber[]`の代わりに`NSObject[]`します。|

ヒントを使用して、ドキュメントを受け取ることができますもすばやく、`sharpie verify-docs`ツール、たとえば。

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
