---
title: 高度な概念と内部構造
description: このガイドでは、Xamarin.Forms の高度な概念と内部構造について説明します。 現在、高速レンダラーと .NET Standard についての記事が含まれています。
ms.prod: xamarin
ms.assetid: 2273a31c-4022-42ba-befe-0d23ce2ff3b5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2017
ms.openlocfilehash: 292b0814cba446c97042ba1fe52ad9414ba74760
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203060"
---
# <a name="advanced-concepts--internals"></a>高度な概念と内部構造

## <a name="fast-renderersfast-renderersmd"></a>[高速レンダラー](fast-renderers.md)

この記事では、高速レンダラーは、結果として得られるネイティブ コントロール階層をフラット化して、増加し、android、Xamarin.Forms コントロールのレンダリング コストを削減について説明します。

## <a name="net-standardnet-standardmd"></a>[.NET Standard](net-standard.md)

この記事では、.NET Standard 2.0 を使用して、Xamarin.Forms アプリケーションに変換する方法について説明します。

## <a name="dependency-resolutiondependency-resolutionmd"></a>[依存関係の解決](dependency-resolution.md)

アプリケーションの依存関係注入コンテナーがある構築およびカスタム レンダラーでは、効果の有効期間を制御できるように、Xamarin.Forms に依存関係の解決方法を挿入する方法について説明し、`DependencyService`実装します。
