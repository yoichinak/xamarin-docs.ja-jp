---
title: Java ライブラリのバインド
description: Android コミュニティには、アプリで使用できる Java ライブラリが多数あります。このガイドでは、バインド ライブラリを作成して、Xamarin.Android アプリケーションに Java ライブラリを組み込む方法について説明します。
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: d40a23076ec8f405e57ec40de47ec9ad2261d85d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027607"
---
# <a name="binding-a-java-library"></a>Java ライブラリのバインド

_Android コミュニティには、アプリで使用できる Java ライブラリが多数あります。このガイドでは、バインド ライブラリを作成して、Xamarin.Android アプリケーションに Java ライブラリを組み込む方法について説明します。_

## <a name="overview"></a>概要

Android 用のサードパーティ製ライブラリ エコシステムは巨大です。 このため、新しい Android ライブラリを作成するよりも、既存の Android ライブラリを使用することがよくあります。 Xamarin.Android には、これらのライブラリを使用する 2 つの方法が用意されています。

- C# 呼び出しを使用して Java コードを呼び出すことができるように、ライブラリを C# ラッパーで自動的にラップする "*バインド ライブラリ*" を作成する。

- "*Java ネイティブ インターフェイス* (*JNI*)" を使用して Java ライブラリ コード内の呼び出しを直接呼び出す。 JNI は、Java コードが、ネイティブ アプリケーションまたはライブラリを呼び出す、またはそれらから呼び出されることができるようにするプログラミング フレームワークです。

このガイドでは、最初のオプションである、1 つ以上の既存の Java ライブラリをアプリケーション内でリンクできるアセンブリにラップする "*バインド ライブラリ*" を作成する方法について説明します。 JNI の使用の詳細については、「[JNI の使用](~/android/platform/java-integration/working-with-jni.md)」をご覧ください。

Xamarin.Android では、"*マネージド呼び出し可能ラッパー* (*MCW*)" を使用してバインドを実装します。 MCW は、マネージド コードで Java コードを呼び出す必要がある場合に使用される JNI ブリッジです。 マネージド呼び出し可能ラッパーは、Java 型のサブクラス化と Java 型の仮想メソッドのオーバーライドもサポートしています。 同様に、Android ランタイム (ART) コードでマネージド コードを呼び出す必要がある場合は、Android 呼び出し可能ラッパー (ACW) と呼ばれる別の JNI ブリッジ経由で実行されます。 この[アーキテクチャ](~/android/internals/architecture.md)を次の図に示します。

[![Android JNI ブリッジのアーキテクチャ](images/architecture.png)](images/architecture.png#lightbox)

バインド ライブラリは、Java 型用のマネージド呼び出し可能ラッパーを含むアセンブリです。 たとえば、次のような Java 型 `MyClass` をバインド ライブラリでラップするとします。

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

`MyClass` を含む **.jar** のバインド ライブラリを生成した後、それをインスタンス化し、C# からそのメソッドを呼び出すことができます。

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

このバインド ライブラリを作成するには、Xamarin.Android の "*Java バインド ライブラリ*" テンプレートを使用します。 生成されるバインド プロジェクトでは、Android ライブラリ プロジェクト用の MCW クラス、 **.jar** ファイル、およびリソースが埋め込まれた .NET アセンブリが作成されます。 Android アーカイブ (.AAR) ファイルおよび Eclipse Android ライブラリ プロジェクト用のバインド ライブラリも作成できます。 作成されたバインド ライブラリの DLL アセンブリを参照することにより、既存の Java ライブラリを Xamarin.Android プロジェクトで再利用できます。

バインド ライブラリ内の型を参照する場合は、バインド ライブラリの名前空間を使用する必要があります。 通常は、C# ソース ファイルの先頭に、Java パッケージ名の .NET 名前空間バージョンである `using` ディレクティブを追加します。 たとえば、バインドされた **.jar** の Java パッケージ名が次のようになっているとします。

```csharp
com.company.package
```

その場合、C# ソース ファイルの先頭に次の `using` ステートメントを記述して、バインドされた **.jar** ファイル内の型にアクセスします。

```csharp
using Com.Company.Package;
```

既存の Android ライブラリをバインドする場合は、次の点に注意する必要があります。

- **ライブラリに外部依存関係はあるか** &ndash; Android ライブラリに必要なすべての Java 依存関係は、**ReferenceJar** または **EmbeddedReferenceJar** として Xamarin.Android プロジェクトに含める必要があります。 すべてのネイティブ アセンブリを **EmbeddedNativeLibrary**としてバインド プロジェクトに追加する必要があります。  

- **Android ライブラリのターゲットとなる Android API のバージョンは何か** &ndash; Android API レベルを "ダウングレード" することはできません。Xamarin.Android バインド プロジェクトが、Android ライブラリと同じ API レベル (またはそれ以降) をターゲットとしていることを確認してください。

- **ライブラリをコンパイルするために使用された JDK のバージョン** &ndash; Android ライブラリが、Xamarin.Android で使用されているものとは異なるバージョンの JDK でビルドされている場合、バインド エラーが発生することがあります。 可能であれば、Xamarin.Android のインストールで使用されているものと同じバージョンの JDK を使用して、Android ライブラリを再コンパイルしてください。

## <a name="build-actions"></a>ビルド アクション

バインド ライブラリを作成するときに、バインド ライブラリ プロジェクトに埋め込む **.jar** または .AAR ファイルに "*ビルド アクション*" を設定します。各ビルド アクションによって、 **.jar** または .AAR ファイルが、バインド ライブラリに埋め込まれる (または参照される) 方法が決まります。 次の一覧にこのビルド アクションの概要を示します。

- `EmbeddedJar` &ndash; **.jar** を、作成されるバインド ライブラリ .DLL に埋め込みリソースとして埋め込みます。 これは最も単純で一般的に使用されるビルド アクションです。 このオプションは、 **.jar** を自動的にバイト コードにコンパイルし、バインド ライブラリにパッケージ化する場合に使用します。

- `InputJar` &ndash; **.jar** を、作成されるバインド ライブラリ .DLL に埋め込みません。 バインド ライブラリ の .DLL は、実行時にこの **.jar** に依存します。 このオプションは、 **.jar** をバインド ライブラリに含めたくない場合に使用します (たとえばライセンスの理由など)。 このオプションを使用する場合は、アプリを実行するデバイスで入力の **.jar** が使用可能であることを確認する必要があります。

- `LibraryProjectZip` &ndash; 作成されるバインド ライブラリ .DLL に .AAR ファイルを埋め込みます。 これは、EmbeddedJar に似ていますが、バインドされる .AAR ファイル内のリソース (およびコード) にアクセスできる点が異なります。 .AAR をバインド ライブラリに埋め込む場合はこのオプションを使用します。

- `ReferenceJar` &ndash; 参照 **.jar** を指定します。参照 **.jar** は、バインドされている **.jar** または .AAR ファイルのいずれかが依存している **.jar** です。 この参照 **.jar** は、コンパイル時の依存関係を満たすためにのみ使用されます。 このビルド アクションを使用すると、参照 **.jar** の C# バインドは作成されず、作成されるバインド ライブラリの .DLL には埋め込まれません。 このオプションは、参照 **.jar** のバインド ライブラリを作成する場合に、まだ実行していなければ使用してください。 このビルド アクションは、複数の **.jar** (および/または .AAR) を、複数の相互に依存するバインド ライブラリにパッケージ化する場合に便利です。

- `EmbeddedReferenceJar` &ndash; 参照 **.jar** を、作成されるバインド ライブラリ .DLL に埋め込みます。 このオプションは、入力 **.jar** (または .AAR) およびそのすべての参照 **.jar** の両方の C# バインドをバインド ライブラリ内に作成する場合に使用します。

- `EmbeddedNativeLibrary` &ndash; ネイティブ **.so** をバインドに埋め込みます。 このビルド アクションは、バインドされる **.jar** ファイルに必要な **.so** ファイルのために使用されます。 Java ライブラリからコードを実行する前に、 **.so** を手動で読み込む必要がある場合があります。 これについては、以下で説明します。

これらのビルド アクションについては、以降のガイドで詳しく説明されています。

さらに、次のビルド アクションを使用して、Java API ドキュメントをインポートし、C# XML ドキュメントに変換することができます。

- `JavaDocJar` は、Maven パッケージ スタイル (通常は `FOOBAR-javadoc**.jar**`) に準拠する Java ライブラリの Javadoc アーカイブ Jar を指すために使用されます。
- `JavaDocIndex` は、API リファレンス ドキュメント HTML 内の `index.html` ファイルを指すために使用されます。
- `JavaSourceJar` は、`JavaDocJar` を補完するために使用されます。最初にソースから JavaDoc を生成し、Maven パッケージスタイル (通常は `FOOBAR-sources**.jar**`) に準拠した Java ライブラリの `JavaDocIndex` として扱います。

API ドキュメントは、Java8、Java7、または Java6 SDK の既定の doclet (すべて異なる形式) であるか、または DroidDoc スタイルである必要があります。

## <a name="including-a-native-library-in-a-binding"></a>バインドにネイティブ ライブラリを含める

Java ライブラリのバインドの一部として、Xamarin.Android バインド プロジェクトに **.so** ライブラリを含める必要がある場合があります。 ラップされた Java コードを実行すると、Xamarin.Android は JNI の呼び出しに失敗し、エラーメッセージ _java.lang.UnsatisfiedLinkErr または Native method not found:_ がアプリケーションの logcat 出力に表示されます。

これを解決するには、`Java.Lang.JavaSystem.LoadLibrary` を呼び出して **.so** ライブラリを手動で読み込みます。 たとえば、Xamarin.Android プロジェクトに、ビルド アクションが **EmbeddedNativeLibrary** のバインド プロジェクトに含まれる共有ライブラリ **libpocketsphinx_jni.so** があるとします。次のスニペット (共有ライブラリを使用する前に実行) によって **.so** ライブラリが読み込まれます。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Java API を C&eparsl; に適合させる

Xamarin.Android のバインド ジェネレーターは、一部の Java 表現とパターンを .NET パターンに対応するように変更します。 次の一覧では、Java を C#/.NET にマップする方法について説明します。

- Java の "_セッター/ゲッター メソッド_" は、.NET の "_プロパティ_" です。

- Java の "_フィールド_" は、.NET の "_プロパティ_" です。

- Java の "_リスナー/リスナー インターフェイス_" は、.NET の "_イベント_" です。 コールバック インターフェイスのメソッドのパラメーターは、`EventArgs` サブクラスによって表されます。

- Java の "_静的入れ子クラス_" は、.NET の "_入れ子クラス_" です。

- Java の "_内部クラス_" は、C# の インスタンス コンストラクターを持つ "_入れ子クラス_" です。

## <a name="binding-scenarios"></a>バインドのシナリオ

次のバインド シナリオ ガイドは、アプリに組み込むために Java ライブラリをバインドするのに役立ちます。

- 「[.JAR のバインド](~/android/platform/binding-java-library/binding-a-jar.md)」は、 **.jar** ファイル用のバインド ライブラリを作成するためのチュートリアルです。

- 「[.AAR のバインド](~/android/platform/binding-java-library/binding-an-aar.md)」は、.AAR ファイル用のバインド ライブラリを作成するためのチュートリアルです。 Android Studio ライブラリをバインドする方法については、このチュートリアルを参照してください。

- 「[Eclipse ライブラリ プロジェクトのバインド](~/android/platform/binding-java-library/binding-a-library-project.md)」は、Android ライブラリ プロジェクトからバインド ライブラリを作成するためのチュートリアルです。 Eclipse の Android ライブラリ プロジェクトをバインドする方法については、このチュートリアルを参照してください。

- 「[バインディングのカスタマイズ](~/android/platform/binding-java-library/customizing-bindings/index.md)」では、ビルド エラーを解決し、生成される API の構造をより "C# らしく" するように、バインドに手動で変更を加える方法について説明します。

- 「[バインドのトラブルシューティング](~/android/platform/binding-java-library/troubleshooting-bindings.md)」では、一般的なバインド エラーのシナリオ、考えられる原因、およびこれらのエラーを解決するための推奨事項について説明します。

## <a name="related-links"></a>関連リンク

- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [ネイティブ ライブラリの使用](~/android/platform/native-libraries.md)
