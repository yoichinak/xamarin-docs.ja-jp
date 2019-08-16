---
title: Xamarin. iOS の浮動小数点演算
description: このドキュメントでは、Xamarin. iOS が32ビットおよび64ビットの精度浮動小数点演算を処理し、関連するパフォーマンスへの影響について説明します。
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: cd1bd0507f89f7b29bfcd3ef1ba0a3b1215632ce
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527377"
---
# <a name="floating-point-operations-in-xamarinios"></a>Xamarin. iOS の浮動小数点演算

Xamarin では、ARM で64ビット精度を使用して、32ビットおよび64ビットの浮動小数点演算が既定で実行されます。  

この精度は、デスクトップC#上の浮動小数点演算による開発者の期待に近いものですが、モバイルでは、パフォーマンスに大きな影響が及ぶ可能性があります。

32ビットの浮動小数点演算を使用するには、32ビットの浮動小数点コードをコンパイルすることができます。  これを行うには、少なくとも Xamarin. iOS 8.10 を使用して、iOS ビルドオプションのパネルで、次の値を示す "mtouch extra arguments" エントリ行を設定する必要があります。

```
--aot-options=-O=float32
```

これにより、静的コンパイラ (Mono の組み込み静的コンパイラ、または LLVM を使用したもの) に対して、32ビットの浮動小数点演算を実行するように通知されます。
