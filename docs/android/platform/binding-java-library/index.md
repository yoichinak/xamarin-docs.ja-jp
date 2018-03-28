---
title: Java ライブラリのバインド
description: Android のコミュニティには多くの Java ライブラリ; アプリで使用することがあります。このガイドでは、バインドのライブラリを作成して、Java ライブラリを Xamarin.Android アプリケーションに組み込む方法を説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 47e32a68b7b10a2d02ee41a9abf234be6f002f7b
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="binding-a-java-library"></a>Java ライブラリのバインド

_Android のコミュニティには多くの Java ライブラリ; アプリで使用することがあります。このガイドでは、バインドのライブラリを作成して、Java ライブラリを Xamarin.Android アプリケーションに組み込む方法を説明します。_

## <a name="overview"></a>概要

Android 用のサードパーティ ライブラリ エコシステムは、大規模なです。 このため、頻繁に理にかなって、新しいものを作成するよりも既存の Android ライブラリを使用します。 Xamarin.Android には、これらのライブラリを使用する 2 つの方法が用意されています。

-   作成、*バインド ライブラリ*を自動的に折り返されてを c# ラッパー ライブラリのため、c# の呼び出しを使用して Java コードを呼び出すことができます。

-   使用して、 *Java ネイティブ インターフェイス*(*JNI*) ライブラリの Java コードの呼び出しを直接起動します。 JNI はプログラミング フレームワークであり、Java コードを呼び出し、ネイティブ アプリケーションやライブラリを呼び出すことができます。

このガイドには、最初のオプションがについて説明します: を作成する方法、*バインド ライブラリ*をアプリケーションにリンクするアセンブリに 1 つまたは複数の既存の Java ライブラリをラップします。 JNI の使用に関する詳細については、次を参照してください。 [JNI 扱う](~/android/platform/java-integration/working-with-jni.md)です。

Xamarin.Android を使用してバインディングを実装する*呼び出し可能ラッパーをマネージ*(*MCW*)。 MCW は、マネージ コードは、Java コードを呼び出す必要がある場合に使用される JNI ブリッジです。 マネージ呼び出し可能ラッパーでは、Java の型をサブクラス化および Java 型上の仮想メソッドのオーバーライドのサポートも提供します。 同様に、Android ランタイム (アート) コードがマネージ コードを呼び出すしようとすると、ときにこれは Android 呼び出し可能ラッパー (について) と呼ばれる別の JNI ブリッジを介してします。 これは、[アーキテクチャ](~/android/internals/architecture.md)次の図に示します。

[![Android の JNI ブリッジ アーキテクチャ](images/architecture.png)](images/architecture.png#lightbox)

バインドのライブラリは、Java の型のマネージ呼び出し可能ラッパーを含むアセンブリです。 たとえば、Java の型を次に示します`MyClass`バインドのライブラリをラップします。

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

用のバインドのライブラリを生成した後、 **.jar**を格納している`MyClass`、それをインスタンス化したりで c# からメソッドを呼び出したりできます。

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

このバインドのライブラリを作成するには Xamarin.Android を使用する*Java バインド ライブラリ*テンプレート。 結果のバインド プロジェクト MCW クラスでは、.NET アセンブリが作成**.jar**ファイル、およびそれに埋め込まれている Android ライブラリ プロジェクトのリソース。 Android のアーカイブのバインドのライブラリを作成することもできます (です。[Aar]) ファイル、Eclipse の Android ライブラリ プロジェクト。 結果のバインドのライブラリ DLL アセンブリを参照しては Xamarin.Android プロジェクトの既存の Java ライブラリを再利用できます。

バインディング ライブラリ内の型を参照するときに、バインディング ライブラリの名前空間を使用する必要があります。 通常、追加、`using`ディレクティブ (C#) ソースの上部にあるファイル、つまり、.NET 名前空間のバージョン、Java のパッケージ名。 たとえば、Java のパッケージの名前をバインドの場合**.jar**です。

```csharp
com.company.package
```

以下にする場合は、 `using` 、バインドの種類と、c# ソース ファイルへのアクセスの上部にあるステートメント**.jar**ファイル。

```csharp
using Com.Company.Package;
```


既存の Android ライブラリをバインドするときに、次の点に注意してに必要があります。

* **ライブラリの外部依存関係があるか。** &ndash; Xamarin.Android プロジェクトでは、Android ライブラリで必要な Java の依存関係を含める必要がある、 **ReferenceJar**か、または、 **EmbeddedReferenceJar**です。 ネイティブ アセンブリとしてバインド プロジェクトに追加する必要があります、 **EmbeddedNativeLibrary**です。  

* **Android ライブラリのターゲットは Android API のバージョンは?** &ndash; Android API レベルを「ダウン グレード」することはできません。Xamarin.Android バインディング プロジェクトが対象としている同じ API レベル (またはそれ以上) を確認してください。 Android ライブラリです。

* **JDK のバージョンが、ライブラリをコンパイルするために使用しますか。** &ndash; バインド エラー Xamarin.Android によって Android ライブラリが使用中でより JDK の別のバージョンでビルドされた場合に発生します。 可能であれば、Xamarin.Android のインストールで使用される JDK の同じバージョンを使用して Android ライブラリを再コンパイルします。


## <a name="build-actions"></a>ビルド アクション

バインドのライブラリを作成するときに設定する*ビルド アクション*上、 **.jar**またはします。AAR ファイル、バインドのライブラリ プロジェクトに組み込むこと&ndash;各ビルド アクションを決定する方法、 **.jar**またはします。[Aar] ファイルに埋め込まれている (またはでによって参照される)、バインドのライブラリです。 これらを次に示しますアクションを構築します。

* `EmbeddedJar` &ndash; 埋め込みます、 **.jar**埋め込みリソースとしての結果として得られるバインド ライブラリ DLL にします。 これは、最も簡単なビルドのための一般的なアクションがほとんどです。 このオプションを使用する場合、 **.jar**自動的にバイトのコードにコンパイルされ、バインドのライブラリにパッケージ化します。

* `InputJar` &ndash; 埋め込まれません、 **.jar**結果バインドのライブラリにします。DLL です。 バインド ライブラリです。DLL はこの依存関係を持つ**.jar**実行時にします。 含めるしたくない場合に、このオプションを使用して、 **.jar**バインド ライブラリで (たとえば、理由をライセンス)。 このオプションを使用する場合、ことを確認入力**.jar**はアプリを実行しているデバイスで使用できます。

* `LibraryProjectZip` &ndash; 埋め込みます、します。結果のバインドのライブラリに AAR ファイルです。DLL です。 これは、機能は、リソース (およびそのコード) をアクセスするには、バインドされた点を除いて EmbeddedJar に似ています。[Aar] ファイルです。 このオプションを埋め込む場合に使用します。バインド ライブラリ AAR です。

* `ReferenceJar` &ndash; 参照を指定**.jar**: 参照**.jar**は、 **.jar** 、バインドの 1 つ**.jar**またはします。AAR ファイルによって異なります。 この参照**.jar**コンパイル時の依存関係を満たすためにのみ使用されます。 このビルド アクションを使用する場合、c# のバインド参照は作成されません**.jar**とは、結果バインドのライブラリには埋め込まれません。DLL です。 このオプションを使用して、参照のバインドのライブラリが作成されます**.jar**が実行されていないが、します。 このビルド アクションは複数のパッケージ化に役立つ**.jar**s (またはします。AARs) 複数の相互依存しているバインド ライブラリにします。

* `EmbeddedReferenceJar` &ndash; 参照を埋め込みます**.jar**結果バインドのライブラリにします。DLL です。 C# の場合、両方の入力のバインドを作成するときに、このビルド アクションを使用して**.jar** (またはします。[Aar]) とそのすべての参照の**.jar**(s) バインド ライブラリにします。

* `EmbeddedNativeLibrary` &ndash; ネイティブの埋め込みます**.so**バインディングにします。 このビルド アクションが使用**.so**で必要なファイル、 **.jar**バインドされているファイルします。 手動で読み込む必要があります、 **.so** Java ライブラリからコードを実行する前にライブラリです。 これは、以下について説明します。

これらのビルド アクションは、次のガイドで詳しく説明します。

さらに、次のビルド アクションは、Java API のドキュメントをインポートするために使用し、c# XML ドキュメントに変換します。

* `JavaDocJar` Javadoc Jar のためのアーカイブ Maven パッケージ スタイルに準拠している Java ライブラリ をポイントするために使用 (通常`FOOBAR-javadoc**.jar**`)。
* `JavaDocIndex` 指すために使用`index.html`API リファレンス ドキュメント HTML 内のファイルです。
* `JavaSourceJar` 補完するために使用される`JavaDocJar`を最初のソースから JavaDoc を生成し、として結果を扱う`JavaDocIndex`、Maven に準拠している Java ライブラリ用パッケージのスタイル (通常`FOOBAR-sources**.jar**`)。

API のドキュメントが既定 doclet Java8、Java7 または Java6 SDK (すべて別の形式は) からにする必要がありますまたは DroidDoc スタイル。

## <a name="including-a-native-library-in-a-binding"></a>バインドでネイティブ ライブラリを含む

含める必要があります、 **.so**バインド Java ライブラリの一部として Xamarin.Android バインディング プロジェクト内のライブラリです。 Xamarin.Android は JNI 呼び出しと、エラー メッセージを失敗ラップの Java コードが実行されると、 _java.lang.UnsatisfiedLinkError: ネイティブ メソッドが見つかりません:_アプリケーションをアウト logcat に表示されます。

この修正プログラムは、手動で読み込む、 **.so**ライブラリへの呼び出しが`Java.Lang.JavaSystem.LoadLibrary`です。 たとえば Xamarin.Android プロジェクトがライブラリを共有すると仮定して**libpocketsphinx_jni.so**ビルド アクションが使用してプロジェクトをバインドに含まれる**EmbeddedNativeLibrary**では、以下スニペット (共有ライブラリを使用する前に実行) が読み込まれます、 **.so**ライブラリ。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Java Api C を適応させる&eparsl;

Xamarin.Android バインディング ジェネレーターは、いくつかの Java の表現方法と .NET のパターンに対応するパターンに変更されます。 次の一覧は、c#、Java をマップする方法について説明します/.NET:

-   _Setter または Getter メソッド_Java は_プロパティ_.NET でします。

-   _フィールド_Java は_プロパティ_.NET でします。

-   _リスナー/リスナー インターフェイス_Java は_イベント_.NET でします。 コールバック インターフェイスでメソッドのパラメーターとして表現される、`EventArgs`サブクラスです。

-   A_クラスの静的な入れ子になった_Java では、_入れ子になったクラス_.NET でします。

-   _内部クラス_Java では、_入れ子になったクラス_C# の場合、インスタンス コンス トラクターを使用します。



## <a name="binding-scenarios"></a>バインディングのシナリオ

次のバインディングのシナリオのガイドは、アプリに組み込むこと Java ライブラリ (またはライブラリ) をバインドするのに役立ちます。

-   [バインドします。JAR](~/android/platform/binding-java-library/binding-a-jar.md)のバインドのライブラリを作成するためのチュートリアルは、 **.jar**ファイル。

-   [バインドにします。[Aar]](~/android/platform/binding-java-library/binding-an-aar.md)用のバインドのライブラリを作成するためのチュートリアルは、します。[Aar] ファイル。 Android Studio ライブラリをバインドする方法については、このチュートリアルを参照してください。

-   [Eclipse ライブラリ プロジェクトをバインド](~/android/platform/binding-java-library/binding-a-library-project.md)Android ライブラリ プロジェクトからバインドのライブラリを作成するためのチュートリアルは、します。 Eclipse の Android ライブラリ プロジェクトをバインドする方法については、このチュートリアルを参照してください。

-   [バインディングをカスタマイズする](~/android/platform/binding-java-library/customizing-bindings/index.md)ビルド エラーを解決して、図形になるよう複数の結果として得られる API へのバインドを手動で変更する方法について説明します"c# のように"。

-   [バインディングのトラブルシューティング](~/android/platform/binding-java-library/troubleshooting-bindings.md)バインディング エラーの一般的なシナリオを一覧表示、考えられる原因、について説明します、およびこれらのエラーを解決するための推奨事項を提供します。


## <a name="related-links"></a>関連リンク

- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [GAPI メタデータ](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [ネイティブ ライブラリの使用](~/android/platform/native-libraries.md)
