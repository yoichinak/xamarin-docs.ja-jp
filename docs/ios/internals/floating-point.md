---
title: "浮動小数点"
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea29132ad4ac55f6fb151ac2125ab1add82c8518
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="floating-point"></a>浮動小数点

Xamarin.iOS 既定では、ARM 上の 64 ビットの精度を使用して 32 ビットおよび 64 ビットの浮動小数点演算です。  

この精度が高いほどは C# の場合、デスクトップ、モバイル、上での浮動小数点演算から開発者が期待どおりに近いパフォーマンスに与える影響が大きなあります。

32 ビット浮動小数点演算を使用する 32 ビット浮動ポイント コードをコンパイルすることができます。  これを行うには、少なくともを使用する必要があります。 Xamarin.iOS 8.10 とセットを、iOS でビルド オプションのパネル、"mtouch 余分な引数"エントリは、次の値を行。

     --aot-options=-O=float32

これは、静的なコンパイラに通知 (モノラルの組み込み静的コンパイラでは、またはある LLVM 電源 1 のいずれか) を 32 ビットの浮動小数点値を使用して浮動小数点演算を実行します。
