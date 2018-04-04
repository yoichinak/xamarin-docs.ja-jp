---
title: Java の統合の概要
description: Java のエコシステムには、コンポーネントの多様なと非常に多くのコレクションが含まれています。 これらのコンポーネントの多くを Android アプリケーションの開発にかかる時間を減らすために使用できます。 このドキュメントは紹介し、開発者がその Xamarin.Android アプリケーションの開発エクスペリエンスを向上させるためにこれら既存の Java コンポーネントを使用する方法のいくつかの概要を説明します。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: dbaf17479ae077fced425df5ac31bdbbc4e06b64
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="java-integration-overview"></a>Java の統合の概要

_Java のエコシステムには、コンポーネントの多様なと非常に多くのコレクションが含まれています。これらのコンポーネントの多くを Android アプリケーションの開発にかかる時間を減らすために使用できます。このドキュメントは紹介し、開発者がその Xamarin.Android アプリケーションの開発エクスペリエンスを向上させるためにこれら既存の Java コンポーネントを使用する方法のいくつかの概要を説明します。_


## <a name="overview"></a>概要

Java エコシステムの程度を指定するが Java で既に Xamarin.Android アプリケーションに必要な特定の機能をすべてをコーディングされている可能性があります。 このため、Xamarin.Android アプリケーションを作成するときに、これらの既存のライブラリを再利用する魅力的です。 

Xamarin.Android アプリケーションで Java ライブラリを再利用する 3 つの方法があります。 

-   **Java バインド ライブラリを作成する** &ndash; Xamarin.Android プロジェクトの使用を c#、Java 型ラッパーを作成するこの手法を使用します。 Xamarin.Android アプリケーションはこのプロジェクトで作成された c# ラッパーを参照しを使用して、`.jar`ファイル。 

-   **Java ネイティブ インターフェイス** &ndash; 、 *Java ネイティブ**インターフェイス*(JNI) を呼び出すか、JVM 内で実行中の Java コードによって呼び出される非 Java コード (C++、c# など) を可能にするフレームワークは、. 

-   **コードの移植** &ndash; Java ソース コードを取得し、c# に変換すること、このメソッドが含まれます。 これは、手動、またはシャープなど、自動化ツールを使用して実行できます。 

最初の 2 つの手法の中核は、 *Java ネイティブ インターフェイス*(JNI)。 JNI は、Java で記述されないアプリケーションが、Java 仮想マシンで実行されている Java コードと対話できるようにするフレームワークです。 Xamarin.Android を使用して作成 JNI*バインド*の c# コードです。 

最初の手法は、バインドの Java ライブラリより自動化された宣言型方法です。 Visual Studio またはを使用して Mac Xamarin.Android によって提供される、Visual Studio プロジェクトの種類に関係する&ndash;Java バインド ライブラリです。 これらのバインディングを正常に作成するには、Java バインド ライブラリ必要がありますもいくつか手動の変更が、同様のほど多くないは純粋な JNI アプローチ。 参照してください[バインド Java ライブラリ](~/android/platform/binding-java-library/index.md)ライブラリの Java バインドの詳細についてはします。 

JNI を使用して、2 番目の手法は、低レベルで機能しますが、さらに細かく制御を提供および通常できない Java バインドのライブラリを使用してアクセスを Java メソッドにアクセスすることがします。 

3 番目の手法は、前の 2 つの大きく異なる: Java からコードを c# に移植します。 1 つの言語から別のコードを移植するプロセスは非常に手間がかかりがそのツールを利用して労力が呼び出されるを削減することが*シャープ*です。 シャープは Java オープン ソース ツール-を-c# コンバーター。 



## <a name="summary"></a>まとめ

このドキュメントでは、Xamarin.Android アプリケーションで Java からライブラリを再利用できる、さまざまな方法のいくつかの概要が用意されています。 バインドの概念を導入および呼び出し可能ラッパーをマネージし、Java コードを c# に移植するためのオプションを説明します。 


## <a name="related-links"></a>関連リンク

- [アーキテクチャ](~/android/internals/architecture.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [シャープにする](https://github.com/slluis/sharpen)
- [Java ネイティブ インターフェイス](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
