---
title: Java ライブラリのバインド
description: Android コミュニティには、アプリで使用できる Java ライブラリが多数あります。このガイドでは、バインドライブラリを作成して、Xamarin Android アプリケーションに Java ライブラリを組み込む方法について説明します。
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 4c01022e01c5ba6a9099b88e99558bd7d7ce728d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524548"
---
# <a name="binding-a-java-library"></a>Java ライブラリのバインド

_Android コミュニティには、アプリで使用できる Java ライブラリが多数あります。このガイドでは、バインドライブラリを作成して、Xamarin Android アプリケーションに Java ライブラリを組み込む方法について説明します。_

## <a name="overview"></a>概要

Android 用のサードパーティ製ライブラリエコシステムは大規模です。 このため、新しい Android ライブラリを作成するよりも、既存の Android ライブラリを使用することがよくあります。 Xamarin には、これらのライブラリを使用する2つの方法が用意されています。

- 呼び出しを使用C#して Java コードを呼び出すC#ことができるように、ラッパーを使用してライブラリを自動的にラップする*バインドライブラリ*を作成します。

- Java*ネイティブインターフェイス*(*JNI*) を使用して、java ライブラリコード内の呼び出しを直接呼び出します。 JNI は、Java コードがを呼び出し、ネイティブアプリケーションまたはライブラリから呼び出すことができるプログラミングフレームワークです。

このガイドでは、最初のオプションについて説明します。1つ以上の既存の Java ライブラリをアプリケーションのリンク先のアセンブリにラップする*バインドライブラリ*を作成する方法について説明します。 JNI の使用方法の詳細については、「 [JNI を使用した作業](~/android/platform/java-integration/working-with-jni.md)」を参照してください。

Xamarin は*マネージ呼び出し可能ラッパー* (*mcw*) を使用してバインディングを実装します。 MCW は、マネージコードが Java コードを呼び出す必要がある場合に使用される JNI ブリッジです。 マネージ呼び出し可能ラッパーは、java 型のサブクラス化と Java 型の仮想メソッドのオーバーライドもサポートしています。 同様に、Android ランタイム (アート) コードでマネージコードを呼び出す必要がある場合は、Android の呼び出し可能ラッパー (ACW) と呼ばれる別の JNI ブリッジによって実行されます。 この[アーキテクチャ](~/android/internals/architecture.md)を次の図に示します。

[![Android JNI ブリッジのアーキテクチャ](images/architecture.png)](images/architecture.png#lightbox)

バインドライブラリは、Java 型のマネージ呼び出し可能ラッパーを含むアセンブリです。 たとえば、次の Java 型`MyClass`は、バインドライブラリでラップします。

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

を含む`MyClass` **.jar**のバインドライブラリを生成した後、それをインスタンス化し、からC#メソッドを呼び出すことができます。

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

このバインドライブラリを作成するには、Xamarin *Java Bindings ライブラリ*テンプレートを使用します。 結果のバインドプロジェクトは、MCW クラス、 **.jar**ファイル、およびその中に埋め込まれている Android ライブラリプロジェクトのリソースを含む .net アセンブリを作成します。 Android アーカイブ用のバインドライブラリ () を作成することもできます。AAR) ファイルと Eclipse Android ライブラリプロジェクト。 結果のバインドライブラリ DLL アセンブリを参照することにより、既存の Java ライブラリを Xamarin. Android プロジェクトで再利用できます。

バインドライブラリ内の型を参照する場合は、バインドライブラリの名前空間を使用する必要があります。 通常、 C#ソースファイルの`using`先頭に、Java パッケージ名の .net 名前空間バージョンであるディレクティブを追加します。 たとえば、バインドさ**れた .jar**の Java パッケージ名が次のようになっているとします。

```csharp
com.company.package
```

次に、 C#ソースファイルの`using`先頭に次のステートメントを記述して、バインドされた **.jar**ファイル内の型にアクセスします。

```csharp
using Com.Company.Package;
```


既存の Android ライブラリをバインドする場合は、次の点に注意する必要があります。

* **ライブラリの外部依存関係はありますか。** &ndash;Android ライブラリに必要な Java 依存関係は、 **Referencejar**または**EmbeddedReferenceJar**として Xamarin プロジェクトに含める必要があります。 すべてのネイティブアセンブリは、 **EmbeddedNativeLibrary**としてバインドプロジェクトに追加する必要があります。  

* **Android ライブラリの対象となる Android API のバージョンを教えてください。** &ndash;Android API レベルを "ダウングレード" することはできません。Xamarin. Android バインドプロジェクトが、Android ライブラリと同じ API レベル (またはそれ以降) を対象としていることを確認します。

* **ライブラリをコンパイルするために使用された JDK のバージョン** &ndash;Android ライブラリが、Xamarin Android で使用されているものとは異なるバージョンの JDK でビルドされている場合、バインドエラーが発生することがあります。 可能であれば、Xamarin Android のインストールで使用されているものと同じバージョンの JDK を使用して、Android ライブラリを再コンパイルします。


## <a name="build-actions"></a>ビルド アクション

バインドライブラリを作成する場合は、.jar またはに*ビルドアクション*を設定し**ます**。AAR ファイルをバインドライブラリプロジェクト&ndash;に組み込むと、各ビルドアクションによって**jar**またはの方法が決まります。AAR ファイルは、バインドライブラリに埋め込まれる (または参照される) ことになります。 これらのビルドアクションの概要を次に示します。

* `EmbeddedJar`作成されたバインディングライブラリ DLL に .jar を埋め込みリソースとして埋め込み**ます。** &ndash; これは最も単純で一般的に使用されるビルドアクションです。 このオプションは、 **jar**を自動的にバイトコードにコンパイルし、バインドライブラリにパッケージ化する場合に使用します。

* `InputJar`結果として得られるバインドライブラリには .jar が埋め込まれません。 &ndash;DLL. バインドライブラリ。DLL は、実行時にこの **.jar**に依存します。 このオプションは、(たとえば、ライセンスの理由で) **.jar**をバインドライブラリに含めない場合に使用します。 このオプションを使用する場合は、アプリを実行するデバイスで入力**jar**が使用可能であることを確認する必要があります。

* `LibraryProjectZip`&ndash;を埋め込みます。AAR ファイルを結果のバインドライブラリに挿入します。DLL. これは、バインドされたのリソース (およびコード) にアクセスできる点を除いて、EmbeddedJar に似ています。AAR ファイル。 を埋め込む場合は、このオプションを使用します。AAR をバインドライブラリに挿入します。

* `ReferenceJar`参照を指定します。 jar: 参照。 jar は、バインドされた .jar またはのいずれかの .jar です。 &ndash;AAR ファイルはに依存します。 この参照は、コンパイル時の依存関係を満たすためにのみ使用されます。 このビルドアクションを使用するとC# 、バインドは参照**jar**に対して作成されず、結果のバインドライブラリには埋め込まれません。DLL. このオプションは、参照**jar**のバインドライブラリを作成する場合に使用しますが、まだ実行していない場合は、このオプションを使用します。 このビルドアクションは、複数の **.jar**s (やなど) をパッケージ化する場合に便利です。複数の相互に依存するバインディングライブラリにします。

* `EmbeddedReferenceJar`結果として得られるバインディングライブラリに参照 .jar を埋め込みます。 &ndash;DLL. このビルドアクションは、入力 **.jar** (またC#はの両方のバインドを作成する場合に使用します。AAR) とそのすべての参照 **.jar**をバインドライブラリに作成します。

* `EmbeddedNativeLibrary`ネイティブなをバインドに埋め込み**ます。** &ndash; このビルドアクションは、バインドされている **.jar**ファイルに必要なファイルに対して使用され**ます。** Java ライブラリからコードを実行する前に、ライブラリを手動で読み込む必要がある場合があります。 詳細については、以下を参照してください。

これらのビルドアクションの詳細については、次のガイドで説明します。

さらに、次のビルドアクションを使用して、Java API ドキュメントをインポートしC# 、XML ドキュメントに変換することができます。

* `JavaDocJar`は、Maven パッケージスタイル (通常は`FOOBAR-javadoc**.jar**`) に準拠する Java ライブラリの Javadoc アーカイブ Jar を指すために使用されます。
* `JavaDocIndex`は、API リファレンスドキュメント`index.html` HTML 内のファイルを指すために使用されます。
* `JavaSourceJar`を補完`JavaDocJar`するために使用されます。最初にソースから JavaDoc を生成`JavaDocIndex`し、Maven パッケージスタイル (通常は`FOOBAR-sources**.jar**`) に準拠する Java ライブラリに対して結果をとして扱います。

API ドキュメントは、Java8、Java7、または Java6 SDK の既定の doclet (すべて異なる形式) であるか、または Iddoc スタイルである必要があります。

## <a name="including-a-native-library-in-a-binding"></a>バインドにネイティブライブラリを含める

Java ライブラリのバインドの一部として、Xamarin Android プロジェクトに **. so**ライブラリを含めることが必要になる場合があります。 ラップされた Java コードを実行すると、JNI の呼び出しに失敗し、エラーメッセージ_UnsatisfiedLinkError が発生します。ネイティブメソッドが見つかりませ_ん: アプリケーションの logcat out に表示されます。

この問題を解決するには、の呼び出し`Java.Lang.JavaSystem.LoadLibrary`を使用して、... ライブラリを手動で読み込みます。 たとえば、Xamarin Android プロジェクトに共有ライブラリ libpocketsphinx_jni が含まれていると**します。そのため**、 **EmbeddedNativeLibrary**のビルドアクションを使用してバインドプロジェクトにインクルードします。次のスニペット (共有ライブラリを使用する前に実行)は、 **...** ライブラリを読み込みます。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Java Api を C に適合させる&eparsl;

Xamarin Android のバインドジェネレーターは、一部の Java 表現とパターンを .NET パターンに対応するように変更します。 次の一覧では、Java を/.NET C#にマップする方法について説明します。

- Java の_Setter/Getter メソッド_は、.Net の_プロパティ_です。

- Java の_フィールド_は .Net の_プロパティ_です。

- Java の_リスナー/リスナーインターフェイス_は、.Net の_イベント_です。 コールバックインターフェイスのメソッドのパラメーターは、 `EventArgs`サブクラスによって表されます。

- Java の_静的入れ子クラス_は、.Net の_入れ子になったクラス_です。

- Java の_内部クラス_は、の C#インスタンスコンストラクターを持つ入れ子になったクラスです。



## <a name="binding-scenarios"></a>バインドのシナリオ

次のバインドシナリオガイドは、アプリに組み込むために Java ライブラリ (またはライブラリ) をバインドするのに役立ちます。

- [をバインドします。JAR](~/android/platform/binding-java-library/binding-a-jar.md)は、 **.jar**ファイルのバインドライブラリを作成するためのチュートリアルです。

- [をバインドします。AAR](~/android/platform/binding-java-library/binding-an-aar.md)は、のバインドライブラリを作成するためのチュートリアルです。AAR ファイル。 Android Studio ライブラリをバインドする方法については、このチュートリアルを参照してください。

- [Eclipse ライブラリプロジェクトのバインド](~/android/platform/binding-java-library/binding-a-library-project.md)は、Android ライブラリプロジェクトからバインドライブラリを作成するためのチュートリアルです。 Eclipse Android ライブラリプロジェクトをバインドする方法については、このチュートリアルを参照してください。

- [バインディングのカスタマイズ](~/android/platform/binding-java-library/customizing-bindings/index.md)では、ビルドエラーを解決し、結果として得られる API の構造を ""C#のようなものにするために、バインディングに手動で変更を加える方法について説明します。

- [バインディングのトラブルシューティング](~/android/platform/binding-java-library/troubleshooting-bindings.md)では、一般的なバインドエラーのシナリオ、考えられる原因、およびこれらのエラーを解決するための提案を示します。


## <a name="related-links"></a>関連リンク

- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [ネイティブ ライブラリの使用](~/android/platform/native-libraries.md)
