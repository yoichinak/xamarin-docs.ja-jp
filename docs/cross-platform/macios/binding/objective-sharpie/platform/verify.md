---
title: 目標マジックペン検証属性
description: このドキュメントでは、目標マジックペンによって生成される [Verify] 属性について説明します。 [Verify] 属性は、目標マジックペンの出力を手動で確認する必要がある開発者に対して強調表示されます。
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: d951952103a94dfc60a8083a75998611b635cda9
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940788"
---
# <a name="objective-sharpie-verify-attributes"></a>目標マジックペン検証属性

多くの場合、目標マジックペンによって生成されるバインディングには、`[Verify]` 属性で注釈が付けられていることがわかります。 これらの属性は、バインディングを元の C/マジックペン宣言と比較することによって、目的が正しいことを_確認_する必要があることを示しています (これは、バインドされた宣言の上にあるコメントで指定されます)。

バインドされている_すべて_の宣言に対して検証を使用することをお勧めしますが、`[Verify]` 属性で注釈が付けられている宣言では、このことが_必要_ これは、多くの場合、バインドを最適に生成する方法を推測するために、元のネイティブソースコードに十分なメタデータがないためです。 最適なバインドを決定するには、ヘッダーファイル内のドキュメントまたはコードコメントを参照することが必要になる場合があります。

バインドが正しいことを確認した後、または正しく修正されたことを確認したら、`[Verify]` 属性をバインドから_削除_します。

> [!IMPORTANT]
> `[Verify]` の属性にC#よってコンパイルエラーが意図的に発生し、バインディングの検証が強制されます。 コードを確認して (修正する可能性があります)、`[Verify]` 属性を削除する必要があります。

## <a name="verify-hints-reference"></a>検証ヒントのリファレンス

属性に指定されたヒント引数は、以下のドキュメントで相互参照できます。 生成された `[Verify]` 属性のドキュメントは、バインディングが完了した後もコンソールに表示されます。

|`[Verify]` ヒント|説明|
|---|---|
|InferredFromPreceedingTypedef|この宣言の名前は、元のネイティブソースコードの直前の `typedef` から、一般的な規則によって推論されました。 この規則があいまいであるため、推論された名前が正しいことを確認します。|
|ConstantsInterfaceAssociation|Extern 変数宣言が関連付けられている可能性がある目標 C インターフェイスを判別する方法はありません。 これらのインスタンスは、部分インターフェイスの `[Field]` プロパティとして、近い具象インターフェイスにバインドされます。これにより、直感的な API が生成されるため、' 定数 ' インターフェイスが完全に削除される可能性があります。|
|MethodToProperty|目標 C メソッドは、パラメーターを取らずC#に値 (void 以外の戻り値) を返すなどの規則により、プロパティとしてバインドされました。 多くの場合、このようなメソッドは、より良い API を示すためにプロパティとしてバインドされる必要がありますが、偽陽性が発生する可能性があり、バインディングは実際にはメソッドである必要があります。|
|StronglyTypedNSArray|ネイティブ `NSArray*` が `NSObject[]`としてバインドされました。 API ドキュメント (ヘッダーファイル内のコメントなど) によって設定された予測に基づいて、またはテストによって配列の内容を調べることにより、バインディング内の配列をより厳密に型指定することができます。 たとえば、Nsarray * instancescan だけを含む NSArray * は、`NSObject[]`ではなく `NSNumber[]` としてバインドされます。|

また、`sharpie verify-docs` ツールを使用して、ヒントのドキュメントをすぐに受け取ることもできます。次に例を示します。

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```
