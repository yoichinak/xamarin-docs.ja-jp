---
title: Xamarin.Mac の内部で
description: このドキュメントには、Xamarin.Mac の内部動作を記述するさまざまなガイドへのリンクがします。 リンク先のドキュメントは、事前コンパイル、Xamarin.Mac アーキテクチャ、および Xamarin.Mac レジストラーについて説明します。
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792490"
---
# <a name="under-the-hood-in-xamarinmac"></a>Xamarin.Mac の内部で

## <a name="ahead-of-time-compilation-aotaotmd"></a>[コンパイル (AOT) の事前](aot.md)

事前時間 (AOT) のコンパイルでは、起動時のパフォーマンスを向上させるための強力な最適化の手法には。 ただしも影響ビルド時、アプリケーション サイズ、およびプログラムの実行、深刻な方法でそのしくみを理解する意義があるためです。

## <a name="mac-architecturearchitecturemd"></a>[Mac のアーキテクチャ](architecture.md)

Objective C をコンパイル、セレクター、レジストラー、アプリの起動、およびコード ジェネレーターなどの概念を含む Xamarin.Mac のリレーションシップです。

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac レジストラー](registrar.md)

Xamarin.Mac は、マネージ環境とアンマネージ Objective C のクラスを呼び出し、イベントが発生したときに呼び出されるマネージ クラスを許可する、Cocoa のランタイムの違いを仲介します。 この「マジック」を実行するために必要な作業は、レジストラーによって処理されますが、"内部"で何が起こってを理解することになることが役立ちます。
