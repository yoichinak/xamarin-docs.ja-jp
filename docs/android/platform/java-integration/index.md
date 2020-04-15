---
title: Java と Xamarin.Android の統合
description: Java エコシステムには、膨大な数の多様なコンポーネントが含まれています。 これらのコンポーネントの多くを使用して、Android アプリケーションの開発にかかる時間を短縮できます。 このドキュメントでは、開発者がこれらの既存の Java コンポーネントを使用して Xamarin.Android アプリケーションの開発エクスペリエンスを向上させる方法の概要について説明します。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: ecaa02e036c74074b4fa922ea079355b72ff02e2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020096"
---
# <a name="java-integration-with-xamarinandroid"></a>Java と Xamarin.Android の統合

"_Java エコシステムには、膨大な数の多様なコンポーネントが含まれています。これらのコンポーネントの多くを使用して、Android アプリケーションの開発にかかる時間を短縮できます。このドキュメントでは、開発者がこれらの既存の Java コンポーネントを使用して Xamarin.Android アプリケーションの開発エクスペリエンスを向上させる方法の概要について説明します。_ "

## <a name="overview"></a>概要

Java エコシステムの広がりを考えると、Xamarin.Android アプリケーションに必要な特定の機能は、Java で既にコーディングされている可能性が非常に高くなります。 このため、Xamarin.Android アプリケーションを作成するときは、これらの既存のライブラリを試して再利用することが推奨されます。

Xamarin.Android アプリケーションで Java ライブラリを再利用するには、次の 3 つの方法があります。 

- **Java バインド ライブラリを作成する** &ndash; この手法では、Xamarin.Android プロジェクトを使用して、java 型の C# ラッパーを作成します。 その後、このプロジェクトによって作成された C# ラッパーを Xamarin.Android アプリケーションで参照し、`.jar` ファイルを使用できます。 

- **Java Native Interface** &ndash; *Java Native* *Interface* (JNI) は、JVM 内で実行中の Java コードを Java 以外のコード (C++ や C# など) で呼び出すか、Java コードから Java 以外のコードを呼び出すことを可能にするフレームワークです。 

- **コードを移植する** &ndash; この手法では、Java ソース コードを取得した後、それを C# に変換する作業が含まれます。 これは、手動で行うことも、Sharpen などの自動ツールを使用して行うこともできます。 

最初の 2 つの手法の中心にあるのは、*Java Native Interface* (JNI) です。 JNI は、Java で記述されていないアプリケーションによって、Java 仮想マシンで実行されている Java コードを対話処理できるようにするフレームワークです。 Xamarin.Android では、JNI を使用して C# コードの "*バインド*" が作成されます。 

最初の手法は、Java ライブラリをバインドするためのより自動化された叙述手法です。 これには、Visual Studio for Mac または Xamarin.Android によって提供される Visual Studio プロジェクトの種類 (Java バインド ライブラリ) のいずれかの使用が含まれます。 これらのバインドを正常に作成するには、Java バインド ライブラリの手動による変更が必要になることがありますが、JNI を使用する方法ほど作業量は多くありません。 Java バインド ライブラリの詳細については、「[Java ライブラリ のバインド](~/android/platform/binding-java-library/index.md)」を参照してください。 

JNI を使用する 2 番目の手法は、はるかに低いレベルで機能しますが、より細かな調整と Java バインド ライブラリの使用では通常はアクセスできない Java メソッドへのアクセスを行うことができます。 

3 番目の手法は、前の 2 つの手法とは大きく異なり、Java から C# にコードを移植します。 ある言語から別の言語へのコードの移植は非常に手間のかかる処理になる可能性がありますが、*Sharpen* いう名前のツールを使用することで、労力を減らすことができます。 Sharpen は、Java から C# へのコンバーターであるオープン ソース ツールです。 

## <a name="summary"></a>まとめ

このドキュメントでは、Xamarin.Android アプリケーションで Java のライブラリを再利用できるさまざまな方法について、その概要を説明しました。 バインドと呼び出し可能なマネージド ラッパーの概念を紹介し、Java コードを C# に移植するためのオプションについて説明しました。 

## <a name="related-links"></a>関連リンク

- [アーキテクチャ](~/android/internals/architecture.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [シャープにする](https://github.com/slluis/sharpen)
- [Java Native Interface](https://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
