---
title: Xamarin.Mac で内部的には
description: このドキュメントは、Xamarin.Mac の内部動作を記述するさまざまなガイドにリンクしています。 リンク先のドキュメントでは、事前にコンパイル、Xamarin.Mac アーキテクチャ、および Xamarin.Mac レジストラーについて説明します。
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 872f26febf3abbe4d659773d2bf2d27348c64513
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118774"
---
# <a name="under-the-hood-in-xamarinmac"></a>Xamarin.Mac で内部的には

## <a name="ahead-of-time-compilation-aotaotmd"></a>[事前 (AOT) コンパイルの](aot.md)

事前の time (AOT) コンパイルが起動時のパフォーマンスを向上させるための強力な最適化手法です。 ただし、これも影響、ビルド時、アプリケーションのサイズ、およびプログラムの実行、深刻な方法でそのしくみを理解するにはします。

## <a name="mac-architecturearchitecturemd"></a>[Mac のアーキテクチャ](architecture.md)

Objective-c、コンパイル、セレクター、レジストラー、アプリの起動、およびコード ジェネレーターなどの概念を含む Xamarin.Mac のリレーションシップです。

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac レジストラー](registrar.md)

Xamarin.Mac は、マネージ環境とアンマネージの OBJECTIVE-C クラスを呼び出し、イベントが発生したときに呼び出されるマネージ クラスを許可する、Cocoa のランタイム間のギャップを橋渡しします。 この「マジック」作業の実行に必要な作業は、レジストラーによって処理されますが、"内部で"何が起こってを理解することもできます。
