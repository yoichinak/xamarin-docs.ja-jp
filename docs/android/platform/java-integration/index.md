---
title: Java と Xamarin Android の統合
description: Java エコシステムには、多様で膨大な数のコンポーネントが含まれています。 これらのコンポーネントの多くは、Android アプリケーションの開発にかかる時間を短縮するために使用できます。 このドキュメントでは、開発者がこれらの既存の Java コンポーネントを使用して Xamarin Android アプリケーションの開発エクスペリエンスを向上させる方法の概要について概要を説明します。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: ecaa02e036c74074b4fa922ea079355b72ff02e2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020096"
---
# <a name="java-integration-with-xamarinandroid"></a>Java と Xamarin Android の統合

_Java エコシステムには、多様で膨大な数のコンポーネントが含まれています。これらのコンポーネントの多くは、Android アプリケーションの開発にかかる時間を短縮するために使用できます。このドキュメントでは、開発者がこれらの既存の Java コンポーネントを使用して Xamarin Android アプリケーションの開発エクスペリエンスを向上させる方法の概要について概要を説明します。_

## <a name="overview"></a>概要

Java エコシステムの範囲を考えると、Xamarin. Android アプリケーションに必要な特定の機能が Java で既にコーディングされている可能性が非常に高くなります。 このため、Xamarin Android アプリケーションを作成するときは、これらの既存のライブラリを試して再利用することが魅力的です。

Xamarin Android アプリケーションで Java ライブラリを再利用するには、次の3つの方法があります。 

- **Java バインドライブラリを作成**するこの方法では、Xamarin. Android プロジェクトを使用してC# 、java 型のラッパーを作成します。 &ndash; その後、Xamarin アプリケーションは、このプロジェクトC#で作成されたラッパーを参照し、`.jar`ファイルを使用できます。 

- Java**ネイティブ**インターフェイス &ndash; java*ネイティブインターフェイス (* JNI) は、java 以外のコード ( C++やC#など) が JVM 内で実行されている java コードを呼び出すか呼び出すことを可能にするフレームワークです。 

- このメソッド &ndash;**コードを移植**するには、Java ソースコードを取得し、それC#をに変換する必要があります。 これは手動で行うことも、シャープなどの自動ツールを使用して行うこともできます。 

最初の2つの手法の中核となるのは、 *Java ネイティブインターフェイス*(JNI) です。 JNI は、Java で記述されていないアプリケーションが Java 仮想マシンで実行されている Java コードと対話できるようにするフレームワークです。 JNI を使用してコードC#の*バインド*を作成します。 

最初の手法は、Java ライブラリをバインドするための、より自動化された宣言型の方法です。 これには、Visual Studio for Mac または Xamarin Android によって提供される Visual Studio プロジェクトの種類 &ndash; Java バインドライブラリを使用する必要があります。 これらのバインドを正常に作成するには、Java バインドライブラリに何らかの手動変更が必要になることがありますが、純粋な JNI アプローチほど多くの変更は必要ありません。 Java バインドライブラリの詳細については[、「Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)」を参照してください。 

JNI を使用する2番目の手法は、はるかに低いレベルで動作しますが、java バインドライブラリを使用して通常はアクセスできない Java メソッドに対してより詳細な制御とアクセスを行うことができます。 

3番目の手法は、前の2つの手法 (Java からへC#のコードの移植) とは大きく異なります。 ある言語から別の言語へのコードの移植は非常に手間のかかるプロセスですが、*シャープ*と呼ばれるツールを使用すると、その作業を減らすことができます。 シャープは、Java からC#コンバーターのオープンソースツールです。 

## <a name="summary"></a>まとめ

このドキュメントでは、Java からのライブラリを Xamarin Android アプリケーションで再利用するさまざまな方法の概要を説明しました。 ここでは、バインディングとマネージ呼び出し可能ラッパーの概念と、Java コードをに移植C#するためのオプションについて説明しました。 

## <a name="related-links"></a>関連リンク

- [アーキテクチャ](~/android/internals/architecture.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [シャープにする](https://github.com/slluis/sharpen)
- [Java ネイティブインターフェイス](https://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
