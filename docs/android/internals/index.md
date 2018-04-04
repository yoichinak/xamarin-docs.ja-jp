---
title: 高度な概念と内部構造
description: Xamarin.Android と API の設計の背後にあるアーキテクチャを基になります。
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/13/2017
ms.openlocfilehash: 517e21f2decd0dabbd03d752f13831a891ad7138
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-concepts-and-internals"></a>高度な概念と内部構造


##  <a name="architectureandroidinternalsarchitecturemd"></a>[アーキテクチャ](~/android/internals/architecture.md)

この記事では、Xamarin.Android アプリケーションの背後にある、基になるアーキテクチャについて説明します。 Xamarin.Android アプリケーションが Android ランタイム仮想マシンとと共にモノラル実行環境内で実行する方法について説明し、呼び出し可能ラッパーの Android および呼び出し可能ラッパーの管理対象としてこのような主要な概念について説明します。 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API の設計](~/android/internals/api-design.md)

コア モノラルの一部である基本クラス ライブラリ、に加えて Xamarin.Android モノラルでネイティブの Android アプリケーションを作成するためにさまざまな Android Api のバインドに付属しています。

中核 Xamarin.Android がありますが、相互運用機能のエンジン Java 世界中でそのブリッジ c# 世界し、(C#) または他の .NET 言語から、Java Api にアクセス権を持つ開発者を提供します。



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[アセンブリ](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android は、いくつかのアセンブリに付属します。 Silverlight は、デスクトップの .NET アセンブリの拡張のサブセットは、同様 Xamarin.Android もいくつかの Silverlight およびデスクトップの .NET アセンブリの拡張のサブセットです。 

