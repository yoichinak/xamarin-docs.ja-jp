---
title: Java ライブラリのバインド
description: Android のコミュニティがアプリで使用することがあります多くの Java ライブラリこのガイドでは、バインド ライブラリを作成して、Xamarin.Android アプリケーションに Java ライブラリを組み込む方法について説明します。
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 0f4ec3cfd7c154e43db9f8e123259317c0d17e21
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670533"
---
# <a name="binding-a-java-library"></a>Java ライブラリのバインド

_Android のコミュニティがアプリで使用することがあります多くの Java ライブラリこのガイドでは、バインド ライブラリを作成して、Xamarin.Android アプリケーションに Java ライブラリを組み込む方法について説明します。_

## <a name="overview"></a>概要

Android 用のサード パーティ製のライブラリ エコシステム膨大になります。 このため、頻繁に理にかなって、新しいものを作成するよりも既存の Android ライブラリを使用します。 Xamarin.Android には、これらのライブラリを使用する 2 つの方法が用意されています。

-   作成、*バインド ライブラリ*でライブラリを自動的にラップするC#経由での Java を起動するためのラッパー コードC#呼び出し。

-   使用して、 *Java ネイティブ インターフェイス*(*JNI*) Java ライブラリ コードで呼び出しを直接起動します。 JNI は、Java コードを呼び出し、ネイティブ アプリケーションやライブラリによって呼び出されるようにするプログラミング フレームワークです。

このガイドでは、最初のオプション: を作成する方法、*バインド ライブラリ*1 つまたは複数の既存の Java ライブラリをラップするアセンブリ、アプリケーションでリンクすることができます。 JNI の使用に関する詳細については、[JNI の](~/android/platform/java-integration/working-with-jni.md)を参照してください。

Xamarin.Android を使用してバインドを実装する*呼び出し可能ラッパーをマネージ*(*MCW*)。 MCW は、マネージ コードは、Java のコードを呼び出す必要がある場合に使用される JNI のブリッジです。 呼び出し可能ラッパーをマネージ Java 型をサブクラス化と Java の型の仮想メソッドのオーバーライドもサポートしています。 同様に、Android ランタイム (アート) コードがマネージ コードを呼び出すことが希望されるたびにこれは Android 呼び出し可能ラッパー (について) と呼ばれる別の JNI ブリッジを使用しています。 これは、[アーキテクチャ](~/android/internals/architecture.md)次の図に示します。

[![Android JNI のブリッジのアーキテクチャ](images/architecture.png)](images/architecture.png#lightbox)

バインディング ライブラリは、Java の型のマネージ呼び出し可能ラッパーを含むアセンブリです。 たとえば、Java の型をここでは`MyClass`、バインド ライブラリをラップする必要があります。

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

バインド ライブラリを生成した後、 **.jar**を格納している`MyClass`、インスタンス化してからのメソッドを呼び出すC#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

このバインド ライブラリを作成するには、Xamarin.Android を使用*Java バインド ライブラリ*テンプレート。 結果のバインド プロジェクトでは、MCW クラスでは、.NET アセンブリを作成します。 **.jar**ファイル、および Android ライブラリ プロジェクトがそこに埋め込まれたリソースです。 Android アーカイブのバインド ライブラリを作成することもできます (します。AAR) ファイルと Eclipse Android ライブラリ プロジェクト。 によって生成されたバインド ライブラリ DLL アセンブリを参照するは、Xamarin.Android プロジェクトで既存の Java ライブラリを再利用できます。

バインディング ライブラリ内の型を参照するときに、バインド ライブラリの名前空間を使用する必要があります。 通常、追加、`using`の上部にあるディレクティブ、C#ソース ファイル、つまり、Java のパッケージ名の .NET 名前空間のバージョン。 たとえば、Java のパッケージの名前、バインドの **.jar**以下します。

```csharp
com.company.package
```

次を記述し、`using`の上部にあるステートメント、C#ソース ファイル、バインドされた型にアクセスする **.jar**ファイル。

```csharp
using Com.Company.Package;
```


既存の Android ライブラリをバインドするときに、次の点に留意する必要があります。

* **ライブラリの外部の依存関係はありますか。** &ndash; Xamarin.Android プロジェクトでは、Android ライブラリで必要な Java 依存関係を含める必要がある、 **ReferenceJar**か、または、 **EmbeddedReferenceJar**します。 ネイティブ アセンブリとしてバインド プロジェクトに追加する必要があります、 **EmbeddedNativeLibrary**します。  

* **Android ライブラリのターゲットにはサポートされている Android API のバージョンですか。** &ndash; Android API レベルを「ダウン グレード」することはできません。Xamarin.Android バインド プロジェクトも、レベル (またはそれ以上) には、同じ API をターゲットはことを確認、Android ライブラリとして。

* **JDK のバージョンは、ライブラリをコンパイルに使用されたか。** &ndash; Xamarin.Android で Android ライブラリの使用よりも JDK の異なるバージョンで作成した場合、バインド エラーが発生します。 可能であれば、同じバージョンの Xamarin.Android のインストールで使用される JDK を使用して、Android ライブラリを再コンパイルします。


## <a name="build-actions"></a>ビルド アクション

バインド ライブラリを作成するときに設定する*ビルド アクション*上、 **.jar**またはします。バインド ライブラリ プロジェクトに組み込むこと AAR ファイル&ndash;各ビルド アクションを決定する方法、 **.jar**または。AAR ファイルがあるに埋め込まれた (またはによって参照される)、バインド ライブラリ。 これらを次に示しますビルド アクション。

* `EmbeddedJar` &ndash; 埋め込みます、 **.jar**埋め込みリソースとしては、結果のバインド ライブラリ DLL にします。 これは、最も簡単なほとんどの一般的に使用されるビルド アクションです。 このオプションを使用する場合、 **.jar**自動的にバイト コードにコンパイルされ、バインド ライブラリにパッケージ化します。

* `InputJar` &ndash; 埋め込まれません、 **.jar**結果バインド ライブラリにします。DLL です。 バインド ライブラリ。DLL はこの依存関係を持って **.jar**実行時にします。 含めるしたくない場合は、このオプションを使用、 **.jar** (たとえば、ライセンス上の理由)、バインド ライブラリ。 このオプションを使用する場合、入力を確保 **.jar**デバイス、アプリを実行するに収録されています。

* `LibraryProjectZip` &ndash; 埋め込みます、します。結果として得られるバインド ライブラリに AAR ファイルです。DLL です。 バインドされているリソース (およびそのコード) にアクセスできる点を除いて EmbeddedJar に似ています。AAR ファイルです。 このオプションを埋め込む場合に使用します。バインド ライブラリ AAR します。

* `ReferenceJar` &ndash; 参照を指定 **.jar**: 参照 **.jar**は、 **.jar** 、バインドの 1 つは **.jar**または。AAR ファイルによって異なります。 この参照 **.jar**コンパイル時の依存関係を満たすためにのみ使用されます。 このビルド アクションを使用するとC#参照のバインドは作成されません **.jar**結果バインド ライブラリが埋め込まれていないとします。DLL です。 参照のバインディング ライブラリを行うと、このオプションを使用 **.jar**が実行されていないが、します。 このビルド アクションは複数のパッケージ化に適しています。 **.jar**s (やします。AARs) に複数のバインド ライブラリを相互に依存します。

* `EmbeddedReferenceJar` &ndash; 参照を埋め込みます **.jar**結果バインド ライブラリにします。DLL です。 作成する場合は、このビルド アクションを使用してC#両方の入力をバインド **.jar** (またはします。AAR) とすべての参照の **.jar**(s)、バインド ライブラリ。

* `EmbeddedNativeLibrary` &ndash; ネイティブを埋め込みます **.so**バインディングにします。 このビルド アクションのため **.so**によって必要なファイルを **.jar**バインドされているファイルします。 手動で読み込む必要があります、 **.so** Java ライブラリからコードを実行する前にライブラリ。 次に示します。

これらのビルド アクションは、次のガイドで詳しく説明します。

Java API のドキュメントをインポートするためにそれらを変換して、次のビルド アクションを使用するさらに、 C# XML ドキュメント。

* `JavaDocJar` Maven パッケージのスタイルに準拠している Java ライブラリの Javadoc アーカイブ Jar をポイントするために使用されます (通常は`FOOBAR-javadoc**.jar**`)。
* `JavaDocIndex` をポイントするために使用`index.html`API リファレンス ドキュメント HTML 内のファイル。
* `JavaSourceJar` 補完するために使用`JavaDocJar`を最初のソースから JavaDoc を生成し、として結果を扱う`JavaDocIndex`、Maven に準拠している Java ライブラリのスタイルをパッケージ化 (通常は`FOOBAR-sources**.jar**`)。

API のドキュメントが Java8、Java7 または (すべて別の形式では)、Java6 SDK から既定 doclet をする必要がありますまたは DroidDoc スタイル。

## <a name="including-a-native-library-in-a-binding"></a>バインディングのネイティブ ライブラリを含む

含める必要があります、 **.so** Java ライブラリのバインドの一部として Xamarin.Android バインド プロジェクトのライブラリです。 Xamarin.Android は JNI の呼び出しと、エラー メッセージを失敗して、ラップされた Java コードの実行時に_java.lang.UnsatisfiedLinkError:ネイティブ メソッドが見つかりません:_ アプリケーションのアウト logcat に表示されます。

この修正プログラムは、手動で読み込む、 **.so**ライブラリへの呼び出しで`Java.Lang.JavaSystem.LoadLibrary`します。 たとえば、Xamarin.Android プロジェクトに共有ライブラリがあると仮定すると**libpocketsphinx_jni.so**のビルド アクションを使用してプロジェクトをバインドに含まれる**EmbeddedNativeLibrary**、次のスニペット(共有ライブラリを使用する前に実行) が読み込まれます、 **.so**ライブラリ。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>C、Java Api の調整&eparsl;

Xamarin.Android バインド ジェネレーターは、いくつかの Java の表現方法と .NET のパターンに対応するパターンに変更されます。 次の一覧は、Java にマップする方法について説明しますC#/.NET:

-   _Setter と Getter メソッド_Java では_プロパティ_.NET でします。

-   _フィールド_Java では_プロパティ_.NET でします。

-   _リスナーとリスナー インターフェイス_Java では_イベント_.NET でします。 コールバック インターフェイスのメソッドのパラメーターによって表される、`EventArgs`サブクラスです。

-   A_クラスの静的な入れ子になった_Java では、_入れ子になったクラス_.NET でします。

-   _内部クラス_Java では、_入れ子になったクラス_にインスタンス コンス トラクターでC#します。



## <a name="binding-scenarios"></a>バインディングのシナリオ

次のバインディング シナリオのガイドでは、アプリに組み込むこと Java ライブラリ (またはライブラリ) をバインドできます。

-   [バインドします。JAR](~/android/platform/binding-java-library/binding-a-jar.md)バインド ライブラリを作成するためのチュートリアルは、 **.jar**ファイル。

-   [バインドします。AAR](~/android/platform/binding-java-library/binding-an-aar.md)バインド ライブラリを作成するためのチュートリアルです。AAR ファイル。 Android Studio ライブラリをバインドする方法については、このチュートリアルを参照してください。

-   [Eclipse ライブラリ プロジェクトをバインド](~/android/platform/binding-java-library/binding-a-library-project.md)Android ライブラリ プロジェクトからバインド ライブラリを作成するためのチュートリアルです。 Eclipse の Android ライブラリ プロジェクトをバインドする方法については、このチュートリアルを参照してください。

-   [バインドのカスタマイズ](~/android/platform/binding-java-library/customizing-bindings/index.md)ビルド エラーを解決し、結果として得られる API を図形には、バインドを手動に変更する方法について説明します"C#のような"。

-   [バインドのトラブルシューティング](~/android/platform/binding-java-library/troubleshooting-bindings.md)バインド エラーの一般的なシナリオを一覧表示、考えられる原因、について説明します、およびこれらのエラーを解決するための推奨事項します。


## <a name="related-links"></a>関連リンク

- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [ネイティブ ライブラリの使用](~/android/platform/native-libraries.md)
