---
title: Java 開発者向け Xamarin
description: Java 開発者は、C# のコードを再利用できる利点を活かしながら、Xamarin プラットフォームでの自分のスキルと既存の Java コードの活用方法について学びます。 C# の構文が Java の構文とよく似ていることと、両方の言語でよく似た機能が提供されていることがわかります。 さらに、開発が容易になる C# 固有の機能についても説明します。
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: eb1d08c5dee6c7944fa42e7446b34a5dbbb45ad3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120412"
---
# <a name="xamarin-for-java-developers"></a>Java 開発者向け Xamarin

_Java 開発者は、C# のコードを再利用できる利点を活かしながら、Xamarin プラットフォームでの自分のスキルと既存の Java コードの活用方法について学びます。C# の構文が Java の構文とよく似ていることと、両方の言語でよく似た機能が提供されていることがわかります。さらに、開発が容易になる C# 固有の機能についても説明します。_


## <a name="overview"></a>概要

この記事では Java 開発者向けの C# プログラミングの概要を提供し、主として Xamarin.Android アプリケーションの開発中に目にする C# 言語の機能に重点を置いています。 また、この記事では、Java の同等機能との違いについて説明し、Java では利用できない (Xamarin.Android に関連する) 重要な C# の機能を紹介します。 他の参考資料へのリンクが含まれているので、ここからさらに C# と .NET についての学習を進めることができます。

Java に慣れていれば、C# の構文にもすぐに慣れることができます。 C# の構文は Java の構文とよく似ており、C# は Java、C、C++ のような "中かっこ" 言語です。 多くの点で、C# の構文は Java の構文のスーパーセットのように見えますが、名前の変更やキーワードの追加がいくつか行われています。

Java の重要な特徴の多くは C# にもあります。

-   クラスに基づくオブジェクト指向プログラミング

-   厳密な型指定

-   インターフェイスのサポート

-   ジェネリック

-   ガベージ コレクション

-   実行時のコンパイル

Java と C# はどちらも、マネージド実行環境で実行される中間言語にコンパイルされます。 C# と Java はどちらも、静的に型指定され、文字列を変更不可能な型として処理します。
どちらの言語も、単一ルートのクラス階層を使います。 Java と同様に、C# は単一継承のみをサポートし、グローバル メソッドには対応していません。
どちらの言語においても、オブジェクトは `new` キーワードを使ってヒープ上に作成され、使われなくなるとガベージ コレクションが行われます。 どちらも、`try`/`catch` セマンティクスで正式な例外処理のサポートを提供します。 そして、スレッド管理と同期のサポートを提供します。

ただし、Java と C# には違う点も多くあります。 例:

-   Java は、暗黙的に型指定されたローカル変数をサポートしていません (C# は `var` キーワードをサポートします)。

-   Java ではパラメーターは値によってのみ渡すことができますが、C# では値だけでなく参照で渡すこともきます (C# では、パラメーターの参照渡しに `ref` および `out` キーワードが提供されていますが、Java にはこれに相当する機能はありません)。

-   Java は、`#define` のようなプリプロセッサ ディレクティブをサポートしていません。

-   Java は符号なし整数型をサポートしていませんが、C# では `ulong`、`uint`、`ushort`、`byte` などの符号なし整数型がサポートされます。

-   Java は演算子のオーバーロードをサポートしていませんが、C# では演算子と変換をオーバーロードできます。

-   Java の `switch` ステートメントでは、コードは次の switch セクションに移行できますが、C# では、すべての `switch` セクションの最後で switch を終了する必要があります (各セクションの最後を `break` ステートメントで閉じる必要があります)。

-   Java ではメソッドによってスローされる例外を `throws` キーワードで指定しますが、C# にはチェックされた例外の概念がなく、`throws` キーワードは C# ではサポートされていません。

-   C# では統合言語クエリ (LINQ) がサポートされており、予約語 `from`、`select`、`where` を使って、データベース クエリと同じような方法でコレクションに対するクエリを記述できます。


もちろん、C# と Java の間にはこの記事で説明しきれないほど多くの違いがあります。 また、Java も C# も発展し続けているので (たとえば、まだ Android ツールチェーンに含まれない Java 8 では、C# スタイルのラムダ式がサポートされています)、これらの相違点は時間とともに変化します。 ここで示すのは、現時点で、Xamarin.Android を始めて使う Java 開発者が遭遇する最も重要な相違点だけです。

-   「[Java から C# 開発者への転身](#fundamentals)」では、C# と Java の基本的な違いについて説明します。

-   「[オブジェクト指向のプログラミング機能](#oopfeatures)」では、2 つの言語のオブジェクト指向機能の間で最も重要な違いの概要を説明します。

-   「[キーワードの相違点](#keywords)」では、同等のキーワード、C# のみのキーワード、C# のキーワードの定義へのリンクの便利な表を提供します。

現在、C# から Xamarin.Android に移植されている重要な機能の多くは、Android での Java 開発にはまだ利用できません。 以下の機能は、より優れたコードを短時間で記述するのに役立ちます。

-   [プロパティ](#properties) &ndash; C# のプロパティ システムでは、setter メソッドと getter メソッドを作成しなくても、メンバー変数に安全に直接アクセスできます。

-   [ラムダ式](#lambdas) &ndash; C# では、匿名メソッド ("*ラムダ*" とも呼ばれます) を使うことで、機能をより簡潔かつ効率的に表現できます。 一度しか使わないオブジェクトを記述するオーバーヘッドを回避することができ、パラメーターを追加することなくローカル状態をメソッドに渡すことができます。

-   [イベント処理](#events) &ndash; C# では、"*イベント ドリブン プログラミング*" が言語レベルでサポートされており、オブジェクトは目的のイベントが発生したときに通知を受け取るように登録できます。 `event` キーワードでは、パブリッシャー クラスがイベント サブスクライバーへの通知に使うことができるマルチキャスト ブロードキャスト メカニズムが定義されています。

-   [非同期プログラミング](#async) &ndash; C# の非同期プログラミング機能 (`async`/`await`) では、アプリの応答性が維持されます。
    この機能の言語レベルでのサポートにより、非同期プログラミングの実装が容易になり、エラーが減ります。


最後に、Xamarin では "*バインド*" と呼ばれる技術を使って、[既存の Java のアセットを利用する](#interop)こともできます。 Xamarin の自動バインド ジェネレーターを使うことで、C# から Java の既存のコード、フレームワーク、ライブラリを呼び出すことができます。 そのために必要なことは、Java でスタティック ライブラリを作成し、バインドを介して C# に公開するだけです。

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>Java から C# の開発へ

以下のセクションでは、C# と Java を "使い始める" ときの基本的な違いの概要を説明し、後のセクションでは、これらの言語でのオブジェクト指向の相違点について説明します。



### <a name="libraries-vs-assemblies"></a>ライブラリとアセンブリ

Java では、通常、関連するクラスを **.jar** ファイルにパッケージ化します。 一方、C# と .NET では、再利用可能なプリコンパイル済みのコードは "*アセンブリ*" にパッケージ化され、通常、アセンブリは *.dll* ファイルとしてパッケージ化されます。 アセンブリは C#/.NET コードの展開の単位であり、通常、各アセンブリは C# プロジェクトと関連付けられます。 アセンブリには、実行時に Just-In-Time コンパイルされる中間コード (IL) が含まれます。

アセンブリについて詳しくは、MSDN の「[Assemblies and the Global Assembly Cache](https://msdn.microsoft.com/en-us/library/ms173099.aspx)」 (アセンブリとグローバル アセンブリ キャッシュ (C# および Visual Basic)) のトピックをご覧ください。


### <a name="packages-vs-namespaces"></a>パッケージと名前空間

C# では、`namespace` キーワードを使って関連する型をグループ化します。これは、Java の `package` キーワードと似ています。 通常、Xamarin.Android アプリは、そのアプリ用に作成された名前空間に存在します。 たとえば、次の C# コードでは、天気予報アプリ用の `WeatherApp` 名前空間ラッパーが宣言されています。

```csharp
namespace WeatherApp
{
    ...
```


### <a name="importing-types"></a>型のインポート

外部の名前空間で定義されている型を使うときは、`using` ステートメントを使ってこれらの型をインポートします (これは、Java の `import` ステートメントとよく似ています)。 Java では、次のようなステートメントを使って 1 つの型をインポートします。

```java
import javax.swing.JButton
```

Java パッケージ全体をインポートするときは次のようなステートメントを使います。

```java
import javax.swing.*
```

C# の `using` ステートメントの動作もよく似ていますが、ワイルドカードを指定せずにパッケージ全体をインポートすることができます。 たとえば、次の例のように、Xamarin.Android のソース ファイルの先頭で一連の `using` ステートメントが使われていることがよくあります。

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

これらのステートメントは、`System`、`Android.App`、`Android.Content` などの名前空間から機能をインポートします。



### <a name="generics"></a>ジェネリック

Java と C# はどちらも "*ジェネリック*" をサポートします。ジェネリックとは、コンパイル時に異なる型を挿入できるプレースホルダーです。 ただし、ジェネリックの動作は C# では若干異なります。 Java では、[型消去](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)により、型の情報はコンパイル時にのみ使用でき、実行時には使用できません。 これに対し、.NET の共通言語ランタイム (CLR) では、ジェネリック型の明示的なサポートが提供されています。これは、C# は実行時に型情報にアクセスできることを意味します。 日常の Xamarin.Android 開発でこの違いの重要性に気付くことはあまりありませんが、[リフレクション](https://msdn.microsoft.com/en-us/library/ms173183.aspx)を使っている場合は、この機能に頼って実行時に型情報にアクセスしています。

Xamarin.Android では、レイアウト コントロールへの参照を取得するために使われているジェネリック メソッド `FindViewById` をよく目にします。 このメソッドは、参照するコントロールの種類を指定するジェネリック型パラメーターを受け取ります。 例:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

このコード例では、`FindViewById` はレイアウトで **Label** として定義されている `TextView` コントロールへの参照を取得し、`TextView` 型として返します。

ジェネリックについて詳しくは、MSDN の「[ジェネリック (C# プログラミング ガイド)](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx)」トピックをご覧ください。
Xamarin.Android でのジェネリック C# クラスのサポートにはいくつかの制限があることに注意してください。詳しくは、「[Limitations](~/android/internals/limitations.md)」(制限事項) をご覧ください。


<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>オブジェクト指向プログラミングの機能

Java と C# では、非常によく似たオブジェクト指向プログラミングの表現方法が使われています。

-   すべてのクラスは、最終的に 1 つのルート オブジェクトから派生します。すべての Java オブジェクトは `java.lang.Object` から派生し、すべての C# オブジェクトは `System.Object` から派生します。

-   クラスのインスタンスは参照型です。

-   インスタンスのプロパティやメソッドにアクセスするときは、"<code>.</code>" 演算子を使います。

-   クラスのすべてのインスタンスは、`new` 演算子を使ってヒープ上に作成されます。

-   どちらの言語もガベージ コレクションを使うので、使われていないオブジェクトを明示的に解放する手段があります (つまり、C++ のような `delete` キーワードはありません)。

-   継承を使ってクラスを拡張することができ、どちらの言語も基底クラスは型ごとに 1 つだけ許可されます。

-   インターフェイスを定義することができ、1 つのクラスで複数のインターフェイス定義から継承 (つまり実装) できます。

一方で、重要な違いもいくつかあります。

-   Java には、匿名クラスと内部クラスという、C# ではサポートされていない 2 つの強力な機能があります (ただし、C# ではクラス定義を入れ子にすることができ、C# の入れ子になったクラスは Java の静的な入れ子になったクラスに似ています)。

-   C# は C スタイルの構造体型 (`struct`) をサポートしますが、Java はサポートしません。

-   C# では、`partial` キーワードを使うことにより、異なるソース ファイルでクラス定義を実装することができます。

-   C# のインターフェイスではフィールドを宣言できません。

-   C# では、C++ スタイルのデストラクター構文を使ってファイナライザーを表現します。 構文は Java の `finalize` メソッドと異なりますが、セマンティクスはほぼ同じです (C# のデストラクターは基底クラスのデストラクターを自動的に呼び出すのに対し、Java では `super.finalize` の明示的な呼び出しを使うことに注意してください)。



### <a name="class-inheritance"></a>クラスの継承

Java でクラスを拡張するには、`extends` キーワードを使います。 C# でクラスを拡張するには、コロン (`:`) を使って派生を示します。 たとえば、Xamarin.Android アプリでは、次のコード フラグメントのようなクラスの派生をよく目にします。

```csharp
public class MainActivity : Activity
{
    ...
```

この例では、`MainActivity` は `Activity` クラスを継承します。

Java でインターフェイスのサポートを宣言するには、`implements` キーワードを使います。 一方、C# では、次のコードで示すように、継承するインターフェイスの名前をクラスのリストに追加するだけです。

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

この例では、`SensorsActivity` は `Activity` を継承し、`ISensorEventListener` インターフェイスで宣言されている機能を実装します。 インターフェイスのリストは基底クラスの後で指定する必要があることに注意してください (そうしないと、コンパイル時エラーになります)。 慣例として、C# のインターフェイス名の先頭には大文字の "I" が付いています。これにより、`implements` キーワードがなくても、どのクラスがインターフェイスか判断できます。

C# でクラスがそれ以上サブクラス化されないようにしたい場合は、クラス名の前に `sealed` を付けます。Java では、クラス名の前に `final` を付けます。

C# のクラス定義について詳しくは、MSDN の[クラスと構造体](https://msdn.microsoft.com/en-us/library/x9afc042.aspx)に関するトピックと[継承](https://msdn.microsoft.com/en-us/library/x9afc042.aspx)に関するトピックをご覧ください。


<a name="properties" />

### <a name="properties"></a>プロパティ

Java では、外部コードからクラスのメンバーを隠ぺいおよび保護しながら、メンバーに対する変更方法を制御するために、ミューテーター メソッド (setter) とインスペクター メソッド (getter) がよく使われます。 たとえば、Android の `TextView` クラスでは、`getText` メソッドと `setText` メソッドが提供されています。 C# では、"*プロパティ*" と呼ばれる、似ていますがさらに直接的なメカニズムが提供されています。
C# クラスのユーザーは、フィールドと似た方法でプロパティにアクセスできますが、各アクセスでは、実際には、呼び出し元に対して透過的なメソッド呼び出しが行われます。 この "外から見えない" メソッドでは、他の値の設定、変換の実行、オブジェクトの状態の変更などの副作用を実装できます。

プロパティは、UI (ユーザー インターフェイス) オブジェクトのメンバーに対するアクセスと変更によく使われます。 例:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

この例では、幅と高さの値が、`MeasuredWidth` および `MeasuredHeight` プロパティにアクセスすることによって `rulerView` オブジェクトから読み取られています。 これらのプロパティの読み取りでは、関連付けられている (ただし隠ぺいされた) フィールドの値が、バックグラウンドでフェッチされて、呼び出し元に返されます。 `rulerView` オブジェクトは、幅と高さの値を 1 つの測定単位 (ピクセルなど) で格納し、`MeasuredWidth` プロパティと `MeasuredHeight` プロパティがアクセスされたら、別の測定単位 (ミリメートルなど) にその場で値を変換できます。

また、`rulerView` オブジェクトには `DrawingCacheEnabled` という名前のプロパティもあります。コード例では、このプロパティを `true` に設定して、`rulerView` の描画キャッシュを有効にしています。 見えないところで、関連付けられている隠ぺいされたフィールドが新しい値で更新され、場合によっては `rulerView` 状態の他の部分が変更されます。 たとえば、`DrawingCacheEnabled` が `false` に設定されていると、`rulerView` はオブジェクトに既に蓄積されている描画キャッシュ情報を消去することもあります。

プロパティへのアクセスは、読み取り/書き込み、読み取り専用、または書き込み専用にすることができます。 また、読み取りと書き込みに異なるアクセス修飾子を使うこともできます。 たとえば、1 つのプロパティに public 読み取りアクセスと private 書き込みアクセスを定義することができます。

C# のプロパティについて詳しくは、MSDN の「[プロパティ (C# プログラミング ガイド)](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx)」トピックをご覧ください。



### <a name="calling-base-class-methods"></a>基底クラスのメソッドの呼び出し

C# で基底クラスのコンストラクターを呼び出すには、コロン (`:`) の後に `base` キーワードと初期化子のリストを記述します。この `base` コンストラクターの呼び出しは、派生コンストラクター パラメーター リストの直後に配置します。 基底クラスのコンストラクターは、派生コンストラクターに入ったときに呼び出されます。コンパイラは、メソッド本体の先頭に、基底コンストラクターへの呼び出しを挿入します。 次のコード フラグメントでは、Xamarin.Android アプリで派生コンストラクターから呼び出される基底コンストラクターを示します。

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public class PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

この例では、`PictureLayout` クラスは `ViewGroup` クラスから派生しています。 この例の `PictureLayout` コンストラクターは、`context` 引数を受け取り、`base(context)` の呼び出しでそれを `ViewGroup` コンストラクターに渡します。

C# で基底クラスのメソッドを呼び出すには、`base` キーワードを使います。 たとえば、Xamarin.Android アプリでは次に示すような基底メソッドの呼び出しをよく行います。

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

この場合、派生クラス (`MainActivity`) によって定義されている `OnCreate` メソッドは、基底クラス (`Activity`) の `OnCreate` メソッドを呼び出します。



### <a name="access-modifiers"></a>アクセス修飾子

Java と C# はどちらも、`public`、`private`、`protected` の各アクセス修飾子をサポートしています。 ただし、C# ではさらに 2 つのアクセス修飾子がサポートされています。

-   **`internal`** &ndash; クラスのメンバーは、現在のアセンブリ内でのみアクセスできます。

-   **`protected internal`** &ndash; クラスのメンバーは、定義アセンブリ、定義クラス、および派生クラス (アセンブリがアクセスできる内部と外部両方の派生クラス) 内でアクセスできます。

C# のアクセス修飾子について詳しくは、MSDN の「[アクセス修飾子 (C# プログラミング ガイド)](https://msdn.microsoft.com/en-us/library/ms173121.aspx)」トピックをご覧ください。



### <a name="virtual-and-override-methods"></a>仮想メソッドとオーバーライド メソッド

Java と C# はどちらも、同じ方法で関連するオブジェクトを処理する機能である "*ポリモーフィズム*" をサポートしています。 どちらの言語でも、基底クラスの参照を使って派生クラスのオブジェクトを参照でき、派生クラスのメソッドはその基底クラスのメソッドをオーバーライドできます。 どちらの言語にも、派生クラスのメソッドによって置き換えられるように設計された基底クラスのメソッドである "*仮想*" メソッドの概念があります。
Java と同様に、C# は `abstract` のクラスとメソッドをサポートします。

ただし、仮想メソッドの宣言方法と仮想メソッドをオーバーライドする方法に関して、Java と C# ではいくつか違いがあります。

-   C# の既定では、メソッドは仮想ではありません。 親クラスで `virtual` キーワードを使って、オーバーライドされるメソッドに明示的にラベルを付ける必要があります。 これに対し、Java ではすべてのメソッドが既定で仮想メソッドになります。

-   C# でメソッドがオーバーライドされないようにするには、単に `virtual` キーワードを省略するだけです。 一方、Java では、`final` キーワードを使ってメソッドを "オーバーライド不可" としてマークする必要があります。

-   C# の派生クラスでは、`override` キーワードを使って、仮想基底クラスのメソッドがオーバーライドされていることを明示的に示す必要があります。

C# によるポリモーフィズムのサポートについて詳しくは、MSDN の「[ポリモーフィズム (C# プログラミング ガイド)](https://msdn.microsoft.com/en-us/library/ms173152.aspx)」トピックをご覧ください。


<a name="lambdas" />

## <a name="lambda-expressions"></a>ラムダ式

C# では、自分を囲んでいるメソッドの状態にアクセスできるインラインの匿名メソッドである "*クロージャ*" を作成できます。
ラムダ式を使うと、Java で実装する場合より少ない行数で、同じ機能を実装できます。

ラムダ式では、Java で 1 回だけ使うクラスまたは匿名クラスの作成に伴う余分な手続きを省くことができます。代わりに、メソッド コードのビジネス ロジックをインラインに記述できます。 また、ラムダはそれを囲んでいるメソッドの変数にアクセスできるので、メソッドのコードに状態を渡すために長いパラメーター リストを作成する必要がありません。

C# では、次のように `=>` 演算子を使ってラムダ式を作成します。

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

Xamarin.Android では、イベント ハンドラーの定義にラムダ式がよく使われます。 例:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

この例のラムダ式のコード (中かっこ内のコード) は、クリック数をインクリメントし、クリック数を表示する `button` テキストを更新しています。 このラムダ式は、ボタンをタップするたびに呼び出されるクリック イベント ハンドラーとして、`button` オブジェクトに登録されます (イベント ハンドラーについては後で詳しく説明します)。この簡単な例の `sender` および `args` パラメーターは、ラムダ式のコードでは使われていませんが、イベント登録のメソッド シグネチャ要件を満たすためにラムダ式で必要です。 内部では、C# のコンパイラはボタン クリック イベントが発生するたびに呼び出される匿名メソッドに、ラムダ式を変換します。

C# とラムダ式について詳しくは、MSDN の「[ラムダ式 (C# プログラミング ガイド)](https://msdn.microsoft.com/en-us/library/bb397687.aspx)」トピックをご覧ください。


<a name="events" />

## <a name="event-handling"></a>イベント処理

"*イベント*" は、何か重要なことがオブジェクトに起きたときに、そのオブジェクトが登録されているサブスクライバーに通知するための手段です。 Java では、サブスクライバーはコールバック メソッドを含む `Listener` インターフェイスを通常実装しますが、C# では、"*デリゲート*" によって言語レベルでイベント処理のサポートが提供されます。 "*デリゲート*" は、オブジェクト指向でのタイプ セーフな関数ポインターのようなものであり、オブジェクト参照とメソッド トークンをカプセル化しています。 クライアント オブジェクトでイベントをサブスクライブする必要がある場合は、デリゲートを作成して、通知オブジェクトにデリゲートを渡します。
イベントが発生すると、通知オブジェクトはデリゲート オブジェクトによって表されるメソッドを呼び出し、サブスクライブしているクライアント オブジェクトにイベントを通知します。 C# では、基本的に、イベント ハンドラーはデリゲートを介して呼び出されるメソッドにすぎません。

デリゲートについて詳しくは、MSDN の「[デリゲート (C# プログラミング ガイド)](https://msdn.microsoft.com/en-us/library/ms173171.aspx)」トピックをご覧ください。

C# の場合、イベントは "*マルチキャスト*" です。つまり、イベント発生の通知を、複数のリスナーが受け取ることができます。 この違いは、Java と C# でのイベント登録の構文上の違い考えるとわかります。 Java では、`SetXXXListener` を呼び出してイベント通知に登録します。C# では、`+=` 演算子を使い、イベント リスナーのリストにデリゲートを "追加する" ことによって、イベント通知に登録します。
Java では `SetXXXListener` を呼び出して登録を解除しますが、C# では、`-=` を使ってリスナーのリストからデリゲートを "差し引き" ます。

Xamarin.Android では、ユーザーが UI コントロールに対して何かを行ったときにオブジェクトに通知するためにイベントがよく使われます。 通常、UI コントロールには `event` キーワードを使って定義されたメンバーがあります。その UI コントロールからのイベントをサブスクライブするには、これらのメンバーにデリゲートをアタッチします。

イベントをサブスクライブするには:

1.  イベント発生時に呼び出されるメソッドを参照するデリゲート オブジェクトを作成します。

2.  `+=` 演算子を使って、サブスクライブするイベントにデリゲートをアタッチします。

次の例では、ボタンのクリックをサブスクライブするデリゲートを (`delegate` キーワードを明示的に使って) 定義しています。
このボタン クリック ハンドラーは、新しいアクティビティを開始します。

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

ただし、ラムダ式を使ってイベントに登録し、`delegate` キーワード全体を省略することもできます。 例:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

この例の `startActivityButton` オブジェクトには、特定のメソッド シグネチャ (送信元とイベントの引数を受け取り、void を返す) を持つデリゲートを期待するイベントがあります。 しかし、そのようなデリゲートまたはそのメソッドをわざわざ明示的に定義したくないので、メソッドのシグネチャを `(sender, e)` と宣言し、ラムダ式を使ってイベント ハンドラーの本体を実装します。
`sender` および `e` パラメーターを使っていない場合でも、このパラメーター リストを宣言する必要があることに注意してください。

デリゲートのサブスクライブを解除することは (`-=` 演算子を使って) できますが、ラムダ式はサブスクライブ解除できないことに注意してください。サブスクライブ解除しようとすると、メモリ リークが発生する可能性があります。 ラムダ形式のイベント登録は、ハンドラーをイベントからサブスクライブ解除しない場合にのみ使ってください。

Xamarin.Android のコードでは、通常、イベント ハンドラーの宣言にラムダ式を使います。 この簡略化されたイベント ハンドラーの宣言方法は最初はわかりにくいかもしれませんが、コードを読み書きする時間が大幅に短縮されます。 このパターン (Xamarin.Android のコードでは頻繁に出現します) の認識に慣れてくれば、アプリケーションのビジネス ロジックについて考える時間が増え、構文に関することにかかる時間は減ります。


<a name="async" />

## <a name="asynchronous-programming"></a>非同期プログラミング

"*非同期プログラミング*" は、アプリケーションの全体的な応答性を向上させる方法です。 非同期プログラミング機能を使うと、アプリの一部が時間のかかる処理によってブロックされている間も、アプリ コードの残りの部分は実行し続けることができます。
非同期的に作成されていない場合に、アプリ全体が停止したように見える原因になる可能性のある操作の例としては、Web へのアクセス、画像の処理、ファイルの読み取り/書き込みなどがあります。

C# には、`async` および `await` キーワードによる言語レベルでの非同期プログラミングのサポートが含まれます。 これらの言語機能を使うと、アプリケーションのメイン スレッドをブロックすることなく、実行時間の長いタスクを実行するコードを、とても簡単に記述できます。 簡単に説明すると、メソッドで `async` キーワードを使うことにより、メソッドのコードが非同期に実行され、呼び出し元のスレッドをブロックしないことを示します。 `async` でマークされているメソッドを呼び出すときは、`await` キーワードを使います。 コンパイラは、`await` を、メソッドの実行がバックグラウンド スレッドに移動される (タスクが呼び出し元に返される) ポイントとして解釈します。 このタスクが完了すると、コードの `await` ポイントで呼び出し元のスレッドでのコード実行が再開されて、`async` の呼び出しの結果が返されます。 慣例として、非同期に実行するメソッドの名前にはサフィックス `Async` を付けます。

Xamarin.Android アプリケーションでは、実行時間の長い操作がバックグラウンド タスクで行われている間は、通常、`async` と `await` を使って、ユーザー入力 (**[キャンセル]** ボタンのタップなど) に応答できるように UI スレッドを解放します。

次の例では、ボタン クリック イベント ハンドラーにより、非同期操作が Web からイメージをダウンロードします。


```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

この例では、ユーザーが `downloadButton` コントロールをクリックすると、`downloadAsync` イベント ハンドラーは `WebClient` オブジェクトと `Uri` オブジェクトを作成して、指定された URL からイメージを取得します。 次に、この URL を使って `WebClient` オブジェクトの `DownloadDataTaskAsync` メソッドを呼び出し、イメージを取得します。

`downloadAsync` のメソッド宣言の前に `async` キーワードが付いていて、非同期に実行してタスクを返すことを示していることに注意してください。 また、`DownloadDataTaskAsync` の呼び出しの前に `await` キーワードが付いていることにも注意してください。 アプリは、イベント ハンドラーの実行 (`await` が出現した場所で開始します) を、`DownloadDataTaskAsync` が完了して戻るまでバックグラウンド スレッドに移動します。
その間も、アプリの UI スレッドはユーザー入力に応答して、他のコントロールに対するイベント ハンドラーを発動できます。 `DownloadDataTaskAsync` が完了すると (数秒かかることがあります)、`bytes` 変数に `DownloadDataTaskAsync` の呼び出しの結果が設定されて実行が再開され、イベント ハンドラーの残りのコードが呼び出し元の (UI) スレッドでダウンロードされたイメージを表示します。

C# での `async`/`await` の概要については、MSDN の「[Asynchronous Programming with Async and Await](https://msdn.microsoft.com/en-us/library/hh191443.aspx)」 (Async および Await を使用した非同期プログラミング (C# および Visual Basic)) のトピックをご覧ください。
Xamarin による非同期プログラミング機能のサポートについて詳しくは、「[非同期サポートの概要](~/cross-platform/platform/async.md)」をご覧ください。


<a name="keywords" />

## <a name="keyword-differences"></a>キーワードの違い

Java で使われている言語キーワードの多くは、C# でも使われます。 また、次の表で示すように、Java キーワードと同等の機能を持っていても C# では名前が異なるものもいくつかあります。

|Java|C#|説明|
|---|---|---|
|`boolean`|[bool](https://msdn.microsoft.com/en-us/library/c8f5xwh7.aspx)|ブール値 true および false を宣言するために使われます。|
|`extends`|`:`|継承するクラスとインターフェイスの前に付きます。|
|`implements`|`:`|継承するクラスとインターフェイスの前に付きます。|
|`import`|[using](https://msdn.microsoft.com/en-us/library/zhdeatwt.aspx)|名前空間から型をインポートし、名前空間の別名の作成にも使われます。|
|`final`|[sealed](https://msdn.microsoft.com/en-us/library/88c54tsw.aspx)|クラスの派生を防止します。派生クラスでメソッドとプロパティがオーバーライドされるのを防止します。|
|`instanceof`|[is](https://msdn.microsoft.com/en-us/library/scekt9xw.aspx)|オブジェクトに特定の型との互換性があるかどうかを評価します。|
|`native`|[extern](https://msdn.microsoft.com/en-us/library/e59b22c5.aspx)|外部で実装されているメソッドを宣言します。|
|`package`|[namespace](https://msdn.microsoft.com/en-us/library/z2kcy19k.aspx)|関連する一連のオブジェクトのスコープを宣言します。|
|`T...`|[params T](https://msdn.microsoft.com/en-us/library/w5zay9db.aspx)|可変数個の引数を受け取るメソッド パラメーターを指定します。|
|`super`|[base](https://msdn.microsoft.com/en-us/library/hfw7t1ce.aspx)|派生クラス内から親クラスのメンバーにアクセスするために使われます。|
|`synchronized`|[lock](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx)|ロックの取得と解放でコードの重要なセクションをラップします。|


また、C# に固有で、Java には対応するもののないキーワードも多くあります。 Xamarin.Android のコードでは、以下の C# キーワードがよく使われます (この表は、Xamarin.Android の[サンプル コード](https://developer.xamarin.com/samples/android/all/)を読むときに参照すると便利です)。

|C#|説明|
|---|---|
|[as](https://msdn.microsoft.com/en-us/library/cscsdfbt.aspx)|互換性のある参照型の間、または null 許容型の間の変換を実行します。|
|[async](https://msdn.microsoft.com/en-us/library/hh156513.aspx)|メソッドまたはラムダ式が非同期であることを指定します。|
|[await](https://msdn.microsoft.com/en-us/library/hh156528.aspx)|タスクが完了するまで、メソッドの実行を中断します。|
|[byte](https://msdn.microsoft.com/en-us/library/5bdb6693.aspx)|符号なしの 8 ビット整数型です。|
|[delegate](https://msdn.microsoft.com/en-us/library/900fyy8e.aspx)|メソッドまたは匿名メソッドをカプセル化するために使われます。|
|[enum](https://msdn.microsoft.com/en-us/library/sbbt4032.aspx)|列挙型つまり名前付き定数のセットを宣言します。|
|[event](https://msdn.microsoft.com/en-us/library/8627sbea.aspx)|パブリッシャー クラスのイベントを宣言します。|
|[fixed](https://msdn.microsoft.com/en-us/library/f58wzh21.aspx)|変数が再配置されないようにします。|
|`get`|プロパティの値を取得するアクセサー メソッドを定義します。|
|[in](https://msdn.microsoft.com/en-us/library/dd469484.aspx)|パラメーターがジェネリック インターフェイスの弱い派生型を受け付けられるようにします。|
|[object](https://msdn.microsoft.com/en-us/library/9kkx3h3c.aspx)|.NET Framework のオブジェクト型の別名です。|
|[out](https://msdn.microsoft.com/en-us/library/t3c3bfhx.aspx)|パラメーター修飾子またはジェネリック型パラメーターの宣言です。|
|[override](https://msdn.microsoft.com/en-us/library/ebca9ah3.aspx)|継承されたメンバーの実装を拡張または修正します。|
|[partial](https://msdn.microsoft.com/en-us/library/6b0scde8.aspx)|定義が複数のファイルに分割されることを宣言します。または、実装からのメソッド定義を分割します。|
|[readonly](https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx)|クラスのメンバーが、宣言時にのみ、またはクラス コンストラクターによってのみ、割り当てられることを宣言します。|
|[ref](https://msdn.microsoft.com/en-us/library/14akc2c7.aspx)|値ではなく、参照によって引数が渡されるようにします。|
|[set](https://msdn.microsoft.com/en-us/library/ms228368.aspx)|プロパティの値を設定するアクセサー メソッドを定義します。|
|[string](https://msdn.microsoft.com/en-us/library/362314fe.aspx)|.NET Framework の文字列型の別名です。|
|[struct](https://msdn.microsoft.com/en-us/library/ah19swz4.aspx)|関連する変数のグループをカプセル化する値の型です。|
|[typeof](https://msdn.microsoft.com/en-us/library/58918ffs.aspx)|オブジェクトの型を取得します。|
|[var](https://msdn.microsoft.com/en-us/library/bb383973.aspx)|暗黙的に型指定されたローカル変数を宣言します。|
|[値](https://msdn.microsoft.com/en-us/library/a1khb4f8.aspx)|クライアント コードがプロパティに代入する値を参照します。|
|[virtual](https://msdn.microsoft.com/en-us/library/9fkccyh4.aspx)|派生クラスでのメソッドのオーバーライドを許可します。|


<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>既存の Java コードとの相互運用

C# に変換したくない既存の Java 機能がある場合は、2 つの方法で、Xamarin.Android アプリケーションの既存の Java ライブラリを再利用できます。

-  **Java バインド ライブラリを作成する** &ndash; この方法では、Xamarin ツールを使って Java 型を囲む C# ラッパーを生成します。 このようなラッパーは "*バインド*" と呼ばれます。 結果として、Xamarin.Android アプリケーションはこれらのラッパーを呼び出すことによって *.jar* ファイルを使うことができます。

-  **Java ネイティブ インターフェイス** &ndash; *Java ネイティブ インターフェイス* (JNI) は、C# アプリによる Java コードの呼び出し、または Java コードによる C# アプリの呼び出しを可能にするフレームワークです。

これらの手法について詳しくは、「[Java Integration Overview](~/android/platform/java-integration/index.md)」(Java 統合の概要) をご覧ください。



## <a name="for-further-reading"></a>関連項目

MSDN の「[C# プログラミング ガイド](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)」は C# プログラミング言語の学習を始めるときに役立ちます。「[C# リファレンス](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)」を使うと C# 言語の特定の機能を検索できます。

Java の知識には、少なくとも Java 言語の知識と同程度の Java クラス ライブラリの知識が必要ですが、同様に C# の実用的な知識には、.NET Framework についての知識がある程度必要です。 Microsoft の「[Moving to C# and the .NET Framework, for Java Developers](https://www.microsoft.com/en-us/download/details.aspx?id=6073)」(Java 開発者のための、C# および .NET Framework への移行) 学習パケットは、Java の観点から (C# についてより深く理解しながら) .NET Framework を詳しく学習するのによい方法です。

C# で最初の Xamarin.Android プロジェクトに取り組む準備ができたら、Microsoft の「[Hello, Android](~/android/get-started/hello-android/index.md)」シリーズが、初めての Xamarin.Android アプリケーションの作成と、Xamarin での Android アプリケーション開発に関する基本事項の理解のさらなる前進に役立ちます。



## <a name="summary"></a>まとめ

この記事では、Java 開発者の観点から Xamarin.Android C# プログラミング環境の概要について説明しました。 C# と Java の類似点と、実用での相違点を指摘しました。 アセンブリと名前空間、外部の型をインポートする方法、およびアクセス修飾子、ジェネリック、クラスの派生、基底クラスのメソッドの呼び出し、メソッドのオーバーライド、イベント処理の相違点の概要について説明しました。 プロパティ、`async`/`await` 非同期プログラミング、ラムダ、C# のデリゲート、C# のイベント処理システムなど、Java では使用できない C# の機能について説明しました。 重要な C# のキーワードの表を示し、既存の Java ライブラリと相互運用する方法を説明し、さらに学習するための関連ドキュメントへのリンクを提供しました。


## <a name="related-links"></a>関連リンク

- [Java 統合の概要](~/android/platform/java-integration/index.md)
- [C# プログラミングガイド](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)
- [C# リファレンス](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)
- [Java 開発者のための、C# および .NET Framework への移行](https://www.microsoft.com/en-us/download/details.aspx?id=6073)
