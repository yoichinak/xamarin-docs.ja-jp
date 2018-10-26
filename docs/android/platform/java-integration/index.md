---
title: Java 統合の概要
description: Java のエコシステムには、コンポーネントの多様性と膨大なコレクションが含まれています。 これらのコンポーネントの多くは、Android アプリケーションの開発にかかる時間を短縮するができます。 このドキュメントは紹介し、開発者がこれらの既存の Java コンポーネントを Xamarin.Android アプリケーションの開発エクスペリエンスを向上させるために使用できる方法のいくつかの概要を説明します。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: 3ab31fb7cac97fbae3315f51daf3dd4b1edbcc1d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112398"
---
# <a name="java-integration-overview"></a>Java 統合の概要

_Java のエコシステムには、コンポーネントの多様性と膨大なコレクションが含まれています。これらのコンポーネントの多くは、Android アプリケーションの開発にかかる時間を短縮するができます。このドキュメントは紹介し、開発者がこれらの既存の Java コンポーネントを Xamarin.Android アプリケーションの開発エクスペリエンスを向上させるために使用できる方法のいくつかの概要を説明します。_


## <a name="overview"></a>概要

Java エコシステムの範囲を指定するには、これは Java で、Xamarin.Android アプリケーションに必要な特定の機能をコーディングされている可能性があります。 このため、Xamarin.Android アプリケーションを作成するときに、これらの既存のライブラリを再利用する魅力的ななります。 

Xamarin.Android アプリケーションでの Java ライブラリを再利用可能な 3 つの方法はあります。 

-   **Java バインド ライブラリを作成する**&ndash;を作成するこの手法では、Xamarin.Android プロジェクトが使用されるC#Java 型を囲むラッパー。 Xamarin.Android アプリケーションを参照できます、C#ラッパーによって、このプロジェクトを作成し、使用、`.jar`ファイル。 

-   **Java ネイティブ インターフェイス** &ndash; 、 *Java ネイティブ**インターフェイス*(JNI) は非 Java コードを可能にするフレームワーク (C++ などまたはC#) を呼び出すか、実行中の Java コードによって呼び出されるJVM の内部。 

-   **コードの移植**&ndash;この方法では、Java ソース コードに変換し、C#します。 これは、手動、またはシャープなどの自動化されたツールを使用して実行できます。 

最初の 2 つの手法の中核には、 *Java ネイティブ インターフェイス*(JNI)。 JNI は、Java 仮想マシンで実行する Java コードとの対話には、Java で記述されていないアプリケーションを使用するフレームワークです。 Xamarin.Android を使用して作成 JNI*バインド*のC#コード。 

最初の手法は、Java ライブラリのバインドをより自動化された、宣言的なアプローチです。 Mac または Visual Studio プロジェクトの種類は Xamarin.Android によって提供されるため、Visual Studio を使用します&ndash;Java バインド ライブラリ。 これらのバインドを正常に作成するには、Java バインド ライブラリ必要がありますも一部手動の変更が、同様のほど多くないのは純粋な JNI アプローチ。 参照してください[Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)Java バインド ライブラリの詳細について。 

JNI を使用して、2 番目の手法の多くを低いレベルで動作しますが、さらに細かく制御を提供および通常アクセスできない、Java バインド ライブラリを使用する Java メソッドにアクセスすることが。 

3 番目の手法は、前の 2 つのまったく異なる: Java をからコードを移植C#します。 1 つの言語から別のコードを移植できる非常に困難な作業が、ツールのヘルプでの作業が呼び出されることを削減することは*シャープ*します。 シャープは Java であるオープン ソース ツールです-に-C#コンバーター。 



## <a name="summary"></a>まとめ

このドキュメントでは、Xamarin.Android アプリケーションで Java からのライブラリを再利用できる、さまざまな方法のいくつかの高度な概要が用意されています。 バインドの概念を導入し呼び出し可能ラッパーは、管理されているし、Java コードを移植するためのオプションを説明したC#します。 


## <a name="related-links"></a>関連リンク

- [アーキテクチャ](~/android/internals/architecture.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [シャープにする](https://github.com/slluis/sharpen)
- [Java ネイティブ インターフェイス](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
