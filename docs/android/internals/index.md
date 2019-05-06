---
title: 高度な概念と内部構造
description: Xamarin.Android とその API の設計の背後にあるアーキテクチャを基になります。
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/21/2018
ms.openlocfilehash: f5844dd4340afa0596219a33ed1e479a0dbcfa76
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953384"
---
# <a name="advanced-concepts-and-internals"></a>高度な概念と内部構造

_このセクションには、アーキテクチャ、API の設計、および Xamarin.Android の制限事項を説明するトピックが含まれています。さらに、そのガベージ コレクションの実装と Xamarin.Android で使用可能なアセンブリを説明するトピックが含まれています。Xamarin.Android のため[オープン ソース](https://github.com/xamarin/xamarin-android)、そのソース コードを調べることで、Xamarin.Android の内部動作を理解することもできます。_


##  <a name="architectureandroidinternalsarchitecturemd"></a>[アーキテクチャ](~/android/internals/architecture.md)

この記事では、Xamarin.Android アプリケーションの背後にある基になるアーキテクチャについて説明します。 Android ランタイムを仮想マシンと共に、Mono 実行環境内で Xamarin.Android アプリケーションを実行する方法について説明し、Android 呼び出し可能ラッパーと呼び出し可能ラッパーの管理対象としてこのような主要な概念について説明します。 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API の設計](~/android/internals/api-design.md)

Mono の一部である基本クラス ライブラリのコアだけでなく Xamarin.Android Mono とネイティブ Android アプリケーションを作成するためにさまざまな Android API のバインドに付属します。

Xamarin.Android のコアがありますが、相互運用機能のエンジン、Java の世界中でそのブリッジ、C# の世界と、C# または他の .NET 言語からの Java API にアクセス権を持つ開発者。



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[アセンブリ](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android は、いくつかのアセンブリで出荷されます。 デスクトップの .NET アセンブリの拡張のサブセットを Silverlight には、同じよう Xamarin.Android もいくつかの Silverlight とデスクトップの .NET アセンブリの拡張のサブセットです。 

