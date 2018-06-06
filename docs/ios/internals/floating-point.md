---
title: Xamarin.iOS で浮動小数点演算
description: このドキュメントでは、Xamarin.iOS が 32 ビットおよび 64 ビットの単精度浮動小数点演算を処理する方法について説明し、パフォーマンスへの関連付けの影響について説明します。
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea5d69b52cbd4c76abb236bd1a272633dde440b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786162"
---
# <a name="floating-point-operations-in-xamarinios"></a>Xamarin.iOS で浮動小数点演算

Xamarin.iOS 既定では、ARM 上の 64 ビットの精度を使用して 32 ビットおよび 64 ビットの浮動小数点演算です。  

この精度が高いほどは C# の場合、デスクトップ、モバイル、上での浮動小数点演算から開発者が期待どおりに近いパフォーマンスに与える影響が大きなあります。

32 ビット浮動小数点演算を使用する 32 ビット浮動ポイント コードをコンパイルすることができます。  これを行うには、少なくともを使用する必要があります。 Xamarin.iOS 8.10 とセットを、iOS でビルド オプションのパネル、"mtouch 余分な引数"エントリは、次の値を行。

     --aot-options=-O=float32

これは、静的なコンパイラに通知 (モノラルの組み込み静的コンパイラでは、またはある LLVM 電源 1 のいずれか) を 32 ビットの浮動小数点値を使用して浮動小数点演算を実行します。
