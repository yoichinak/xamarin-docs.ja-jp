---
title: Xamarin. Mac の内部で
description: このドキュメントでは、Xamarin. Mac の内部動作について説明するさまざまなガイドにリンクしています。 リンクされたドキュメントでは、事前にコンパイル、Xamarin、Mac のアーキテクチャ、および Xamarin. Mac レジストラーについて説明します。
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: 51cf479ba07a769f5d7a875bb3f1203caef2ad0b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290108"
---
# <a name="under-the-hood-in-xamarinmac"></a>Xamarin. Mac の内部で

## <a name="ahead-of-time-compilation-aotaotmd"></a>[事前コンパイル (AOT)](aot.md)

事前に (AOT) コンパイルは、起動時のパフォーマンスを向上させるための強力な最適化手法です。 ただし、ビルド時間、アプリケーションサイズ、およびプログラムの実行に大きな影響があるため、動作のしくみを理解していることがわかります。

## <a name="mac-architecturearchitecturemd"></a>[Mac のアーキテクチャ](architecture.md)

Xamarin は、コンパイル、セレクター、レジストラー、アプリの起動、ジェネレーターなどの概念を含む、目標 C との関係を持ちます。

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin. Mac レジストラー](registrar.md)

Xamarin. Mac は、マネージ環境と Cocoa のランタイムとの間のギャップを橋渡しします。これにより、マネージクラスはアンマネージドの C クラスを呼び出し、イベントが発生したときにコールバックすることができます。 この "マジック" を検査するために必要な作業はレジストラーによって処理されますが、"内部" について理解しておくと役立つ場合があります。
