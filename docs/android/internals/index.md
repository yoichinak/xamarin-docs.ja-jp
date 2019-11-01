---
title: 高度な概念と内部構造
description: Xamarin Android の背後にある基になるアーキテクチャと API の設計。
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/21/2018
ms.openlocfilehash: 97382243ac5f767d94a782b895401c1f2f8ae554
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027853"
---
# <a name="advanced-concepts-and-internals"></a>高度な概念と内部構造

_このセクションには、Xamarin Android のアーキテクチャ、API 設計、および制限事項について説明するトピックが含まれています。また、そのガベージコレクションの実装と、Xamarin で使用できるアセンブリについて説明するトピックも含まれています。Xamarin Android は[オープンソース](https://github.com/xamarin/xamarin-android)であるため、ソースコードを調べて、xamarin android の内部動作を理解することもできます。_

## <a name="architectureandroidinternalsarchitecturemd"></a>[アーキテクチャ](~/android/internals/architecture.md)

この記事では、Xamarin Android アプリケーションの背後にある基になるアーキテクチャについて説明します。 Android ランタイム仮想マシンと共に Mono 実行環境内で Xamarin アプリケーションを実行する方法について説明し、Android 呼び出し可能ラッパーやマネージ呼び出し可能ラッパーとしての主要な概念について説明します。 

## <a name="api-designandroidinternalsapi-designmd"></a>[API の設計](~/android/internals/api-design.md)

Mono に含まれるコア基本クラスライブラリに加えて、Xamarin にはさまざまな Android Api のバインドが付属しており、開発者は Mono を使用してネイティブの Android アプリケーションを作成できます。

Xamarin Android の中核にある相互運用エンジンにより、Java 環境でC#世界を橋渡しし、開発者はまたはその他の .net 言語C#から java api にアクセスできるようになります。

## <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[アセンブリ](~/cross-platform/internals/available-assemblies.md)

Xamarin. Android には、いくつかのアセンブリが付属しています。 Silverlight がデスクトップ .NET アセンブリの拡張サブセットであるのと同様に、Xamarin Android も、いくつかの Silverlight およびデスクトップ .NET アセンブリの拡張されたサブセットになります。 
