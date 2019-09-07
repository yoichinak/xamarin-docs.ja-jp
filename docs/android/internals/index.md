---
title: 高度な概念と内部構造
description: Xamarin Android の背後にある基になるアーキテクチャと API の設計。
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/21/2018
ms.openlocfilehash: 4f860d493c5709e2f6c7f89e6f3a50981cf62dc3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757822"
---
# <a name="advanced-concepts-and-internals"></a>高度な概念と内部構造

_このセクションには、Xamarin Android のアーキテクチャ、API 設計、および制限事項について説明するトピックが含まれています。また、そのガベージコレクションの実装と、Xamarin で使用できるアセンブリについて説明するトピックも含まれています。Xamarin Android は[オープンソース](https://github.com/xamarin/xamarin-android)であるため、ソースコードを調べて、xamarin android の内部動作を理解することもできます。_

## <a name="architectureandroidinternalsarchitecturemd"></a>[アーキテクチャ](~/android/internals/architecture.md)

この記事では、Xamarin Android アプリケーションの背後にある基になるアーキテクチャについて説明します。 Android ランタイム仮想マシンと共に Mono 実行環境内で Xamarin アプリケーションを実行する方法について説明し、Android 呼び出し可能ラッパーやマネージ呼び出し可能ラッパーとしての主要な概念について説明します。 

## <a name="api-designandroidinternalsapi-designmd"></a>[API の設計](~/android/internals/api-design.md)

Mono の一部である基本クラス ライブラリのコアだけでなく Xamarin.Android Mono とネイティブ Android アプリケーションを作成するためにさまざまな Android API のバインドに付属します。

Xamarin.Android のコアがありますが、相互運用機能のエンジン、Java の世界中でそのブリッジ、C# の世界と、C# または他の .NET 言語からの Java API にアクセス権を持つ開発者。

## <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[アセンブリ](~/cross-platform/internals/available-assemblies.md)

Xamarin. Android には、いくつかのアセンブリが付属しています。 Silverlight がデスクトップ .NET アセンブリの拡張サブセットであるのと同様に、Xamarin Android も、いくつかの Silverlight およびデスクトップ .NET アセンブリの拡張されたサブセットになります。 
