---
title: Xamarin.iOS での浮動小数点演算
description: このドキュメントでは、Xamarin.iOS が 32 ビットおよび 64 ビットの精度の浮動小数点演算を処理する方法について説明し、パフォーマンスへの関連付けの影響について説明します。
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: c5ee1b833e309c78c7338298cbe5c8800afb1ba1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116239"
---
# <a name="floating-point-operations-in-xamarinios"></a>Xamarin.iOS での浮動小数点演算

Xamarin.iOS は既定では実行 ARM 上の 64 ビットの有効桁数を使用して 32 ビットおよび 64 ビットの浮動小数点演算します。  

この高い有効桁数は浮動小数点演算でから開発者の期待に近いC#モバイル、デスクトップで、パフォーマンスに与える影響が大きく影響します。

32 ビット浮動小数点演算を使用する 32 ビット浮動ポイント コードをコンパイルすることになります。  これを行うには、少なくともを使用する必要があります。 Xamarin.iOS 8.10 と iOS の設定は、パネルの オプションのをビルド、"mtouch 余分な引数"エントリは、次の値を行。

     --aot-options=-O=float32

これは、静的コンパイラに通知されます (Mono の組み込み静的コンパイラ、または LLVM を利用した 1 つのいずれか) を 32 ビットの浮動小数点値を使用して、浮動小数点演算を実行します。
