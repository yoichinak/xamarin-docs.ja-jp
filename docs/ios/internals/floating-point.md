---
title: Xamarin. iOS の浮動小数点演算
description: このドキュメントでは、Xamarin. iOS が32ビットおよび64ビットの精度浮動小数点演算を処理し、関連するパフォーマンスへの影響について説明します。
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: af0e37b41a7bbe831bc629b1fd916f62819b3711
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022339"
---
# <a name="floating-point-operations-in-xamarinios"></a>Xamarin. iOS の浮動小数点演算

Xamarin.iOS では、ARM で 64 ビット精度を使用して、32 ビットおよび 64 ビットの浮動小数点演算が既定で実行されます。  

この高い精度は、開発者がデスクトップアプリにおいて C# の浮動小数点演算に期待するものに近いですが、モバイルアプリでは、パフォーマンスに大きな影響が及ぶ可能性があります。

32ビットの浮動小数点演算を使用するには、32ビットの浮動小数点コードをコンパイルすることができます。  これを行うには、少なくとも Xamarin. iOS 8.10 を使用して、iOS ビルドオプションのパネルで、次の値を示す "mtouch extra arguments" エントリ行を設定する必要があります。

```
--aot-options=-O=float32
```

これにより、静的コンパイラ (Mono の組み込み静的コンパイラ、または LLVM を使用したもの) に対して、32 ビット浮動小数点を使用して浮動小数点演算を実行するように通知されます。
