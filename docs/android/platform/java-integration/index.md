---
title: Java と Xamarin Android の統合
description: Java エコシステムには、多様で膨大な数のコンポーネントが含まれています。 これらのコンポーネントの多くは、Android アプリケーションの開発にかかる時間を短縮するために使用できます。 このドキュメントでは、開発者がこれらの既存の Java コンポーネントを使用して Xamarin Android アプリケーションの開発エクスペリエンスを向上させる方法の概要について概要を説明します。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: a9d239140cee9eb600414a1bfb0733c9af6488bc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761424"
---
# <a name="java-integration-with-xamarinandroid"></a>Java と Xamarin Android の統合

_Java エコシステムには、多様で膨大な数のコンポーネントが含まれています。これらのコンポーネントの多くは、Android アプリケーションの開発にかかる時間を短縮するために使用できます。このドキュメントでは、開発者がこれらの既存の Java コンポーネントを使用して Xamarin Android アプリケーションの開発エクスペリエンスを向上させる方法の概要について概要を説明します。_

## <a name="overview"></a>概要

Java エコシステムの範囲を考えると、Xamarin. Android アプリケーションに必要な特定の機能が Java で既にコーディングされている可能性が非常に高くなります。 このため、Xamarin Android アプリケーションを作成するときは、これらの既存のライブラリを試して再利用することが魅力的です。

Xamarin Android アプリケーションで Java ライブラリを再利用するには、次の3つの方法があります。 

- **Java バインドライブラリを作成する**この手法では、Java 型のラッパーを作成C#するために Xamarin Android プロジェクトが使用されます。 &ndash; その後、このプロジェクトで作成されC#たラッパーを Xamarin Android アプリケーションで参照し、 `.jar`そのファイルを使用できます。 

- **Java ネイティブインターフェイス**C++ C# Java ネイティブインターフェイス (JNI) は、java 以外のコード (やなど) が JVM 内で実行されている java コードによる呼び出しまたは呼び出しを行うことを可能にするフレームワークです。 &ndash; 

- **コードを移植する**このメソッドは、Java ソースコードを取得し、それをにC#変換します。 &ndash; これは手動で行うことも、シャープなどの自動ツールを使用して行うこともできます。 

最初の2つの手法の中核となるのは、 *Java ネイティブインターフェイス*(JNI) です。 JNI は、Java で記述されていないアプリケーションが Java 仮想マシンで実行されている Java コードと対話できるようにするフレームワークです。 JNI を使用してコードC#の*バインド*を作成します。 

最初の手法は、Java ライブラリをバインドするための、より自動化された宣言型の方法です。 これには、Visual Studio for Mac または Xamarin. Android &ndash;によって提供される Visual Studio プロジェクトの種類のいずれかを使用する Java バインドライブラリが必要です。 これらのバインドを正常に作成するには、Java バインドライブラリに何らかの手動変更が必要になることがありますが、純粋な JNI アプローチほど多くの変更は必要ありません。 Java バインドライブラリの詳細については[、「Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)」を参照してください。 

JNI を使用する2番目の手法は、はるかに低いレベルで動作しますが、java バインドライブラリを使用して通常はアクセスできない Java メソッドに対してより詳細な制御とアクセスを行うことができます。 

3番目の手法は、前の2つの手法 (Java からへC#のコードの移植) とは大きく異なります。 ある言語から別の言語へのコードの移植は非常に手間のかかるプロセスですが、*シャープ*と呼ばれるツールを使用すると、その作業を減らすことができます。 シャープは、Java からC#コンバーターのオープンソースツールです。 

## <a name="summary"></a>Summary

このドキュメントでは、Java からのライブラリを Xamarin Android アプリケーションで再利用するさまざまな方法の概要を説明しました。 ここでは、バインディングとマネージ呼び出し可能ラッパーの概念と、Java コードをに移植C#するためのオプションについて説明しました。 

## <a name="related-links"></a>関連リンク

- [アーキテクチャ](~/android/internals/architecture.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [シャープにする](https://github.com/slluis/sharpen)
- [Java ネイティブインターフェイス](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
