---
title: JNI と Xamarin.Android を使用する
description: Xamarin.Android では、Java ではなく C# を使用して Android アプリを作成できます。 Xamarin.Android に付属する複数のアセンブリでは、Mono.Android.dll や Mono.Android.GoogleMaps.dll などの Java ライブラリのバインドが提供されています。 ただし、使用可能なすべての Java ライブラリに対するバインドが提供されているわけではなく、提供されているバインドでも、Java のすべての型とメンバーがバインドされてはいない場合があります。 バインドされていない Java の型とメンバーを使用するには、Java ネイティブ インターフェイス (JNI) を使用できます。 この記事では、JNI を使用して、Xamarin.Android アプリケーションから Java の型とメンバーを操作する方法について説明します。
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 0fa717a775ff2f1ace9e248a8afde8d373e8a1f8
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724345"
---
# <a name="working-with-jni-and-xamarinandroid"></a>JNI と Xamarin.Android を使用する

_Xamarin.Android では、Java ではなく C# を使用して Android アプリを作成できます。Xamarin.Android に付属する複数のアセンブリでは、Mono.Android.dll や Mono.Android.GoogleMaps.dll などの Java ライブラリのバインドが提供されています。ただし、使用可能なすべての Java ライブラリに対するバインドが提供されているわけではなく、提供されているバインドでも、Java のすべての型とメンバーがバインドされてはいない場合があります。バインドされていない Java の型とメンバーを使用するには、Java ネイティブ インターフェイス (JNI) を使用できます。この記事では、JNI を使用して、Xamarin.Android アプリケーションから Java の型とメンバーを操作する方法について説明します。_

## <a name="overview"></a>概要

Java コードを呼び出すためのマネージド呼び出し可能ラッパー (MCW) の作成は、常に必要または可能なわけではありません。 多くの場合は、"インライン" JNI でまったく問題なく、バインドされていない Java メンバーを 1 回だけ使用するには便利です。 完全な .jar バインドを生成するより、JNI を使用して Java クラスの 1 つのメソッドを呼び出す方が簡単なことがよくあります。

Xamarin.Android の `Mono.Android.dll` アセンブリでは、Android の `android.jar` ライブラリに対するバインドが提供されます。 `Mono.Android.dll` に存在しない型とメンバー、および `android.jar` に存在しない型を、手動でバインドすることによって使用できます。 Java の型とメンバーをバインドするには、**Java ネイティブ インターフェイス** (**JNI**) を使用して、型の参照、フィールドの読み取りと書き込み、メソッドの呼び出しを行います。

Xamarin.Android の JNI API は、概念的には .NET の `System.Reflection` API と非常によく似ています。これを使用すると、型とメンバーを名前で検索したり、フィールド値の読み取りと書き込みを行ったり、メソッドを呼び出したりすることができます。 JNI と `Android.Runtime.RegisterAttribute` カスタム属性を使用して、オーバーライドをサポートするためにバインドできる仮想メソッドを宣言できます。 C# で実装できるように、インターフェイスをバインドできます。

このドキュメントでは、以下のことについて説明します。

- JNI が型を参照する方法。
- フィールドの参照、読み取り、書き込みを行う方法。
- メソッドを参照して呼び出す方法。
- マネージド コードからオーバーライドできるように仮想メソッドを公開する方法。
- インターフェイスを公開する方法。

## <a name="requirements"></a>必要条件

JNI は、[Android.Runtime.JNIEnv 名前空間](xref:Android.Runtime.JNIEnv)を通して公開されると、すべてのバージョンの Xamarin.Android で使用できます。
Java の型とインターフェイスをバインドするには、Xamarin.Android 4.0 以降を使用する必要があります。

## <a name="managed-callable-wrappers"></a>マネージド呼び出し可能ラッパー

**マネージド呼び出し可能ラッパー** (**MCW**) は、Java のクラスまたはインターフェイス用の "*バインド*" であり、クライアントの C# コードで JNI の基盤の複雑さについて心配する必要がないように、JNI のすべての機構をラップします。 ほとんどの `Mono.Android.dll` は、マネージド呼び出し可能ラッパーで構成されています。

マネージド呼び出し可能ラッパーには、次の 2 つの目的があります。

1. クライアントのコードで基盤の複雑さを知る必要がないように、JNI の使用をカプセル化します。
1. Java の型をサブクラス化でき、Java のインターフェイスを実装できるようにします。

1 つ目の目的は、純粋に便利さのためであり、コンシューマーがシンプルで管理されたクラスのセットを使用できるように複雑さをカプセル化することです。 これには、後で説明するように、さまざまな [JNIEnv](xref:Android.Runtime.JNIEnv) メンバーを使用する必要があります。 マネージド呼び出し可能ラッパーは、絶対に必要なわけではいことに注意してください。"インライン" JNI を使用するのでもまったく問題はなく、バインドされていない Java メンバーを 1 回だけ使用する場合は便利です。 サブクラス化とインターフェイスの実装には、マネージド呼び出し可能ラッパーを使用する必要があります。

## <a name="android-callable-wrappers"></a>Android 呼び出し可能ラッパー

Android ランタイム (ART) でマネージド コードを呼び出す必要がある場合は常に、Android 呼び出し可能ラッパー (ACW) が必要です。実行時に ART にクラスを登録する方法がないため、これらのラッパーが必要になります。
(具体的には、JNI 関数の [DefineClass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) は、Android ランタイムではサポートされていません。 そのため、Android 呼び出し可能ラッパーによって、ランタイム型登録サポートの欠如を補います。)

マネージド コードでオーバーライドまたは実装されている仮想メソッドまたはインターフェイス メソッドを、Android のコードで実行する必要があるときは常に、このメソッドが適切なマネージド型にディスパッチされるように、Xamarin.Android で Java プロキシを提供する必要があります。 これらの Java プロキシ型は、マネージ型と "同じ" 基底クラスと Java インターフェイス リストを持つ Java コードであり、同じコンストラクターが実装され、オーバーライドされた基底クラスとインターフェイス メソッドが宣言されています。

Android 呼び出し可能ラッパーは、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)の間に **monodroid.exe** プログラムによって生成され、(直接的または間接的に) [Java.Lang.Object](xref:Java.Lang.Object) を継承するすべての型に対して生成されます。

### <a name="implementing-interfaces"></a>インターフェイスの実装

Android インターフェイス ([Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks) など) を実装することが必要になる場合があります。

Android のすべてのクラスとインターフェイスでは、[Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject) インターフェイスが拡張されています。そのため、Android のすべての型で、`IJavaObject` を実装する必要があります。
Xamarin.Android では、この事実が利用されており、`IJavaObject` を使用して、特定のマネージド型に対する Java プロキシ (Android 呼び出し可能ラッパー) を Android に提供しています。 **monodroid.exe** では `Java.Lang.Object` サブクラス (`IJavaObject` を実装している必要があります) のみが検索されるため、`Java.Lang.Object` をサブクラス化することにより、マネージド コードでインターフェイスを実装できます。 次に例を示します。

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```

### <a name="implementation-details"></a>実装の詳細

"*この記事の残りの部分で説明する実装の詳細は、予告なしに変更される可能性があります*" (ここでは、開発者が内部の処理に関心があるかもしれないという理由でのみ説明します)。

たとえば、次のような C# ソースについて考えます。

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**mandroid.exe** プログラムでは、次のような Android 呼び出し可能ラッパーが生成されます。

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

基底クラスが保持されており、マネージド コード内でオーバーライドされるメソッドごとにネイティブ メソッド宣言が提供されていることに注意してください。

### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute

通常、Xamarin.Android では、ACW を含む Java コードが自動的に生成されます。この生成は、クラスが Java クラスから派生し、既存の Java メソッドをオーバーライドするときの、クラス名とメソッド名に基づいています。 ただし、一部のシナリオでは、次に示すように、コード生成が適切ではありません。

- Android では、レイアウト XML 属性 ([android:onClick](xref:Android.Views.View.IOnClickListener.OnClick*) XML 属性など) でのアクション名がサポートされています。 それが指定されていると、拡張されたビュー インスタンスで Java メソッドの検索が試みられます。

- [java.io.Serializable](https://developer.android.com/reference/java/io/Serializable.html) インターフェイスでは、`readObject` メソッドと `writeObject` メソッドが必要です。 それらはこのインターフェイスのメンバーではないため、対応するマネージド実装ではこれらのメソッドが Java コードに公開されません。

- [android.os.Parcelable](xref:Android.OS.Parcelable) インターフェイスでは、実装クラスに `Parcelable.Creator` 型の静的フィールド `CREATOR` があることが想定されています。 生成される Java コードには、明示的なフィールドがいくつか必要です。 標準的なシナリオでは、マネージド コードから Java コードにフィールドを出力することはできません。

コード生成では、任意の名前で任意の Java メソッドを生成するためのソリューションが提供されていないため、Xamarin.Android 4.2 以降では、上のシナリオに対するソリューションを提供するため、[ExportAttribute](xref:Java.Interop.ExportAttribute) と [ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute) が導入されました。 どちらの属性も、`Java.Interop` 名前空間に存在します。

- `ExportAttribute` &ndash; メソッド名と予期される例外の種類を指定します (Java で明示的な "スロー" を提供するため)。 メソッドでそれを使用すると、メソッドによって、マネージド メソッドへの対応する JNI の呼び出しに対するディスパッチ コードを生成する Java メソッドが "エクスポート" されます。 これは、`android:onClick` および `java.io.Serializable` で使用できます。

- `ExportFieldAttribute` &ndash; フィールド名を指定します。 フィールド初期化子として機能するメソッドに存在します。 これは、`android.os.Parcelable` で使用できます。

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute のトラブルシューティング

- **Mono.Android.Export.dll** がないことによるパッケージ化の失敗 &ndash; コードの一部のメソッドまたは依存ライブラリで `ExportAttribute` または `ExportFieldAttribute` を使用する場合は、**Mono.Android.Export.dll** を追加する必要があります。 このアセンブリは、Java からのコールバック コードをサポートするために分離されています。 アプリケーションのサイズが増えるため、**Mono.Android.dll** とは別になっています。

- リリース ビルドでの、エクスポート メソッドに対する `MissingMethodException` の発生 &ndash; リリース ビルドではエクスポート メソッドに対して `MissingMethodException` が発生します。 (この問題は、最新バージョンの Xamarin.Android で修正されています)。

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` と `ExportFieldAttribute` では、Java ランタイム コードで使用できる機能が提供されます。 このランタイム コードでは、それらの属性によって生成される JNI メソッドを通してマネージド コードにアクセスします。 その結果、マネージド メソッドによってバインドされる既存の Java メソッドはありません。したがって、Java メソッドはマネージド メソッド シグネチャから生成されます。

ただし、この場合は完全に決定的ではありません。 特に注意すべきは、次のようなマネージド型と Java 型の間の高度なマッピングではこれが成り立つことです。

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

エクスポートされたメソッドでこのような型が必要な場合は、`ExportParameterAttribute` を使用して、対応するパラメーターまたは戻り値の型を明示的に指定する必要があります。

### <a name="annotation-attribute"></a>Annotation 属性

Xamarin.Android 4.2 では、`IAnnotation` の実装型が属性 (System.Attribute) に変換され、Java ラッパーでの注釈生成のサポートが追加されました。

これは、次のように方向が変化することを意味します。

- バインド ジェネレーターでは、`java.Lang.Deprecated` から `Java.Lang.DeprecatedAttribute` が生成されます (マネージド コードでは `[Obsolete]` である必要があります)。

- これは、既存の `Java.Lang.Deprecated` クラスがなくなるという意味ではありません。 これらの Java ベースのオブジェクトも、通常の Java オブジェクトとしてまだ使用できます (そのような使用方法が存在する場合)。 `Deprecated` クラスと `DeprecatedAttribute` クラスが存在します。

- `Java.Lang.DeprecatedAttribute` クラスは `[Annotation]` としてマークされます。 この `[Annotation]` 属性から継承されるカスタム属性がある場合、MSBuild タスクによって、Android 呼び出し可能ラッパー (ACW) でそのカスタム属性に対する Java 注釈が生成されます (@Deprecated)。

- 注釈は、クラス、メソッド、およびエクスポートされたフィールド (マネージド コード内のメソッド) に対して生成できます。

含む側のクラス (注釈付きクラス自体、または注釈付きメンバーを含むクラス) が登録されていない場合は、注釈を含めて、Java クラス ソース全体がまったく生成されません。 メソッドの場合、`ExportAttribute` を指定することで、メソッドを明示的に生成して注釈を付けることができます。 また、それは Java 注釈クラス定義を "生成" する機能ではありません。 つまり、特定の注釈に対してカスタム マネージド属性を定義する場合は、対応する Java 注釈クラスを含む別の .jar ライブラリを追加する必要があります。 注釈型が定義されている Java ソース ファイルを追加するだけでは不十分です。 Java コンパイラは、**apt** と同じようには機能しません。

さらに、次の制限が適用されます。

- この変換プロセスでは、これまでは注釈型で `@Target` 注釈は考慮されません。

- プロパティに対する属性は機能しません。 代わりに、プロパティ ゲッターまたはセッターの属性を使用します。

## <a name="class-binding"></a>クラスのバインド

クラスをバインドするということは、基になる Java 型の呼び出しを簡略化するためにマネージド呼び出し可能ラッパーを作成することを意味します。

C# からオーバーライドできるように仮想メソッドと抽象メソッドをバインドするには、Xamarin.Android 4.0 が必要です。 ただし、非仮想メソッド、静的メソッド、またはオーバーライドをサポートしていない仮想メソッドをバインドすることは、すべてのバージョンの Xamarin.Android でできます。

通常、バインドには次の項目が含まれます。

- [バインドされている Java 型への JNI ハンドル](#_Looking_up_Java_Types)。

- [バインドされた各フィールドに対する JNI のフィールド ID とプロパティ](#_Instance_Fields)。

- [バインドされた各メソッドに対する JNI のメソッド ID とメソッド](#_Instance_Methods)。

- サブクラス化が必要な場合、型の宣言に [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) が `true` に設定された [RegisterAttribute](xref:Android.Runtime.RegisterAttribute) カスタム属性が必要です。

### <a name="declaring-type-handle"></a>型ハンドルの宣言

フィールドおよびメソッドの参照方法には、宣言する型を参照するオブジェクト参照が必要です。 慣例により、これは `class_ref` フィールドに保持されます。

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

`CLASS` トークンの詳細については、「[JNI の型参照](#_JNI_Type_References)」セクションを参照してください。

### <a name="binding-fields"></a>フィールドのバインド

Java のフィールドは C# のプロパティとして公開されます。たとえば、Java のフィールド [java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) は、C# のプロパティ [Java.Lang.JavaSystem.In](xref:Java.Lang.JavaSystem.In) としてバインドされます。
さらに、JNI では静的フィールドとインスタンス フィールドが区別されるため、プロパティを実装するときに異なるメソッドが使用されます。

フィールドのバインドには、メソッドの 3 つのセットが含まれます。

1. "*フィールド ID 取得*" メソッド。 "*フィールド ID 取得*" メソッドの役割は、"*フィールド値取得*" メソッドおよび "*フィールド値設定*" メソッドによって使用されるフィールド ハンドルを返すことです。 フィールド ID を取得するには、宣言する型、フィールドの名前、およびフィールドの [JNI 型シグネチャ](#JNI_Type_Signatures)がわかっている必要があります。

1. "*フィールド値取得*" メソッド。 これらのメソッドではフィールド ハンドルが必要であり、その役割は Java からフィールドの値を読み取ることです。
    使用するメソッドは、フィールドの型によって異なります。

1. "*フィールド値設定*" メソッド。 これらのメソッドではフィールド ハンドルが必要であり、その役割は Java にフィールドの値を書き込むことです。 使用するメソッドは、フィールドの型によって異なります。

[静的フィールド](#_Static_Fields)では、[JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)、`JNIEnv.GetStatic*Field`、[JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) の各メソッドが使用されます。

 [インスタンス フィールド](#_Instance_Fields)では、[JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*)、`JNIEnv.Get*Field`、[JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) の各メソッドが使用されます。

たとえば、静的プロパティ `JavaSystem.In` は次のように実装できます。

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

メモ:[InputStreamInvoker.FromJniHandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*) を使用して、JNI 参照を `System.IO.Stream` インスタンスに変換しています。また、[JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) からはローカル参照が返されるため、`JniHandleOwnership.TransferLocalRef` を使用しています。

多くの [Android.Runtime](xref:Android.Runtime) 型が備える `FromJniHandle` メソッドにより、JNI 参照が目的の型に変換されます。

### <a name="method-binding"></a>メソッドのバインド

Java のメソッドは、C# のメソッドおよび C# のプロパティとして公開されます。 たとえば、Java の [java.lang.Runtime.runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) メソッドは [Java.Lang.Runtime.RunFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*) メソッドとしてバインドされ、[java.lang.Object.getClass](https://developer.android.com/reference/java/lang/Object.html#getClass) メソッドは [Java.Lang.Object.Class](xref:Java.Lang.Object.Class) プロパティとしてバインドされます。

メソッドの呼び出しは、2 つのステップから成るプロセスです。

1. 呼び出すメソッドの "*メソッド ID を取得*" します。 "*メソッド ID 取得*" メソッドの役割は、メソッド呼び出しメソッドで使用されるメソッド ハンドルを返すことです。 メソッド ID を取得するには、宣言する型、メソッドの名前、およびメソッドの [JNI 型シグネチャ](#JNI_Type_Signatures)がわかっている必要があります。

1. メソッドを呼び出します。

フィールドの場合と同様に、メソッド ID を取得するメソッドとメソッドを呼び出すメソッドは、静的メソッドとインスタンス メソッドでは異なります。

[静的メソッド](#_Static_Methods_1)では、[JNIEnv.GetStaticMethodID()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) を使用してメソッド ID を参照し、`JNIEnv.CallStatic*Method` メソッド ファミリを使用して呼び出しを行います。

[インスタンス メソッド](#_Instance_Methods)では、[JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) を使用してメソッド ID を参照し、`JNIEnv.Call*Method` および `JNIEnv.CallNonvirtual*Method` メソッド ファミリを使用して呼び出しを行います。

メソッドのバインドは、単なるメソッドの呼び出しでは済まない可能性があります。 メソッドのバインドには、メソッドをオーバーライドすること (抽象メソッドまたは final 以外のメソッドの場合) または実装すること (インターフェイス メソッドの場合) の許可も含まれます。 「[継承のサポート、インターフェイス](#_Supporting_Inheritance,_Interfaces_1)」セクションでは、仮想メソッドとインターフェイス メソッドをサポートする複雑さについて説明します。

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>静的メソッド

静的メソッドのバインドでは、`JNIEnv.GetStaticMethodID` を使用してメソッド ハンドルを取得してから、メソッドの戻り値の型に応じて適切な `JNIEnv.CallStatic*Method` メソッドを使用する必要があります。 次に、[Runtime.getRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) メソッドのバインドの例を示します。

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

メソッド ハンドルを静的フィールド `id_getRuntime` に格納していることに注意してください。 これは、すべての呼び出しでメソッド ハンドルを検索する必要がないようにして、パフォーマンスを最適化するためです。 メソッド ハンドルをこの方法でキャッシュする必要はありません。 メソッド ハンドルを取得した後は、[JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) を使用してメソッドを呼び出します。 `JNIEnv.CallStaticObjectMethod` から返される `IntPtr` には、返された Java インスタンスのハンドルが含まれます。
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*) を使用して、Java のハンドルを厳密に型指定されたオブジェクト インスタンスに変換します。

#### <a name="non-virtual-instance-method-binding"></a>非仮想インスタンス メソッドのバインド

`final` インスタンス メソッドまたはオーバーライドを必要としないインスタンス メソッドのバインドでは、`JNIEnv.GetMethodID` を使用してメソッド ハンドルを取得してから、メソッドの戻り値の型に応じて適切な `JNIEnv.Call*Method` メソッドを使用する必要があります。 次に、`Object.Class` プロパティのバインドの例を示します。

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

メソッド ハンドルを静的フィールド `id_getClass` に格納していることに注意してください。
これは、すべての呼び出しでメソッド ハンドルを検索する必要がないようにして、パフォーマンスを最適化するためです。 メソッド ハンドルをこの方法でキャッシュする必要はありません。 メソッド ハンドルを取得した後は、[JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) を使用してメソッドを呼び出します。 `JNIEnv.CallStaticObjectMethod` から返される `IntPtr` には、返された Java インスタンスのハンドルが含まれます。
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*) を使用して、Java のハンドルを厳密に型指定されたオブジェクト インスタンスに変換します。

### <a name="binding-constructors"></a>コンストラクターのバインド

コンストラクターは、`"<init>"` という名前の Java のメソッドです。 Java のインスタンス メソッドと同様に、コンストラクター ハンドルの参照には `JNIEnv.GetMethodID` を使用します。 Java のメソッドとは異なり、コンストラクター メソッド ハンドルを呼び出すには、[JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*) メソッドを使用します。 `JNIEnv.NewObject` の戻り値は、JNI のローカル参照です。

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

通常、クラスのバインドでは、[Java.Lang.Object](xref:Java.Lang.Object) がサブクラス化されます。
`Java.Lang.Object` をサブクラス化すると、追加のセマンティックが機能するようになります。`Java.Lang.Object` インスタンスでは、`Java.Lang.Object.Handle` プロパティを通じて Java インスタンスへのグローバル参照が保持されます。

1. 既定の `Java.Lang.Object` コンストラクターでは、Java インスタンスが割り当てられます。

1. 型に `RegisterAttribute` があり、`RegisterAttribute.DoNotGenerateAcw` が `true` の場合は、既定のコンストラクターによって `RegisterAttribute.Name` 型のインスタンスが作成されます。

1. それ以外の場合は、`this.GetType` に対応する [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) が、既定のコンストラクターによってインスタンス化されます。 Android 呼び出し可能ラッパーは、`RegisterAttribute.DoNotGenerateAcw` が `true` に設定されていないすべての `Java.Lang.Object` サブクラスに対して、パッケージの作成時に生成されます。

クラス バインドではない型の場合、これは想定されるセマンティックです。C# の `Mono.Samples.HelloWorld.HelloAndroid` インスタンスをインスタンス化すると、生成された Android 呼び出し可能ラッパーである Java の `mono.samples.helloworld.HelloAndroid` インスタンスが構築される必要があります。

クラス バインドでは、Java の型に既定のコンストラクターが含まれている場合、または他のコンストラクターを呼び出す必要がない場合は、これが正しい動作である可能性があります。 それ以外の場合は、次のアクションを実行するコンストラクターを提供する必要があります。

1. 既定の `Java.Lang.Object` コンストラクターの代わりに、[Java.Lang.Object(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object#ctor*) を呼び出します。 これは、新しい Java インスタンスを作成しないようにするために必要です。

1. Java のインスタンスを作成する前に、[Java.Lang.Object.Handle](xref:Java.Lang.Object.Handle) の値を調べます。 Android 呼び出し可能ラッパーが Java のコードで構築されていて、作成された Android 呼び出し可能ラッパーのインスタンスを格納するためにクラス バインドが構築されている場合、`Object.Handle` プロパティは `IntPtr.Zero` 以外の値になります。 たとえば、Android で `mono.samples.helloworld.HelloAndroid` のインスタンスが作成されるときは、最初に Android 呼び出し可能ラッパーが作成され、Java の `HelloAndroid` コンストラクターによって対応する `Mono.Samples.HelloWorld.HelloAndroid` 型のインスタンスが作成されて、コンストラクターの実行前に `Object.Handle` プロパティが Java のインスタンスに設定されます。

1. 現在のランタイム型が宣言する型と同じでない場合は、対応する Android 呼び出し可能ラッパーのインスタンスが作成され、[Object.SetHandle](xref:Java.Lang.Object.SetHandle*) を使用して、[JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*) によって返されるハンドルを格納する必要があります。

1. 現在のランタイム型が宣言する型と同じである場合は、Java コンストラクターを呼び出し、[Object.SetHandle](xref:Java.Lang.Object.SetHandle*) を使用して、`JNIEnv.NewInstance` によって返されるハンドルを格納します。

たとえば、[java.lang.Integer(int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) コンストラクターについて考えます。 これは次のようにバインドされます。

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

[JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*) メソッドは、`JNIEnv.FindClass` から返された値に対して `JNIEnv.FindClass`、`JNIEnv.GetMethodID`、`JNIEnv.NewObject`、`JNIEnv.DeleteGlobalReference` を実行するためのヘルパーです。 詳細については、次のセクションを参照してください。

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>継承のサポート、インターフェイス

Java の型をサブクラス化するか、Java のインターフェイスを実装するには、パッケージ化プロセス中にすべての `Java.Lang.Object` サブクラスに対して生成される [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) を生成する必要があります。 ACW の生成は、[Android.Runtime.RegisterAttribute](xref:Android.Runtime.RegisterAttribute) カスタム属性を使用して制御されます。

C# の型の場合、`[Register]` カスタム属性コンストラクターには 1 つの引数が必要であり、これは対応する Java の型に対する [JNI の略式型参照](#_Simplified_Type_References_1)です。 これにより、Java と C# の間で異なる名前を指定できます。

Xamarin.Android 4.0 より前の `[Register]` カスタム属性は、既存の Java の型を "別名化する" ために使用できませんでした。 これは、ACW の生成プロセスによって、検出されたすべての `Java.Lang.Object` サブクラスに対して ACW が生成されるためです。

Xamarin.Android 4.0 では、[RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) プロパティが導入されました。 このプロパティは、注釈付きの型を "*スキップする*" よう ACW 生成プロセスに指示するものであり、これにより、パッケージの作成時に ACW が生成されない新しいマネージド呼び出し可能ラッパーを宣言できます。 これにより、既存の Java の型をバインドできます。 たとえば、次のような簡単な Java のクラス `Adder` を考えてみます。このクラスには、整数への加算を行って結果を返す 1 つのメソッド `add` が含まれています。

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` 型は次のようにバインドできます。

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

ここでは、C# の `Adder` 型によって Java の `Adder` 型が "*別名化*" されています。 `[Register]` 属性は Java の `mono.android.test.Adder` 型の JNI 名を指定するために使用されており、`DoNotGenerateAcw` プロパティは ACW の生成を禁止するために使用されています。 これにより、`ManagedAdder` 型に対する ACW が生成され、`mono.android.test.Adder` 型が適切にサブクラス化されます。 `RegisterAttribute.DoNotGenerateAcw` プロパティを使用しないと、Xamarin.Android のビルド プロセスによって新しい `mono.android.test.Adder` Java 型が生成されます。 それにより、`mono.android.test.Adder` 型が 2 つの異なるファイルの 2 箇所に存在するようになるので、コンパイル エラーが発生します。

### <a name="binding-virtual-methods"></a>仮想メソッドのバインド

`ManagedAdder` によって Java の `Adder` 型がサブクラス化されますが、特に重要なことではありません。C# の `Adder` 型では仮想メソッドが定義されていないので、`ManagedAdder` では何もオーバーライドできません。

サブクラスによるオーバーライドを許可するために `virtual` メソッドをバインドするには、次の 2 つのカテゴリに分類されるいくつかの処理を行う必要があります。

1. **メソッドのバインド**

1. **メソッドの登録**

#### <a name="method-binding"></a>メソッドのバインド

メソッドをバインドするには、C# の `Adder` の定義に 2 つのサポート メンバー `ThresholdType` と `ThresholdClass` を追加する必要があります。

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` プロパティは、バインドの現在の型を返します。

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` は、仮想メソッドと非仮想メソッドのディスパッチを実行する状況を決定するために、メソッドのバインドで使用されます。 常に、C# の宣言する型に対応する `System.Type` インスタンスを返す必要があります。

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` プロパティは、バインドされた型に対する JNI クラス参照を返します。

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` は、非仮想メソッドを呼び出すときに、メソッドのバインドで使用されます。

#### <a name="binding-implementation"></a>バインドの実装

メソッドのバインドの実装では、Java メソッドの実行時の呼び出しが行われます。 また、メソッドの登録の一部である `[Register]` カスタム属性の宣言も含まれます。これについては、メソッドの登録に関するセクションで説明します。

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

`id_add` フィールドには、呼び出す Java メソッドのメソッド ID が含まれています。 `id_add` の値は、`JNIEnv.GetMethodID` から取得されます。これには、宣言するクラス (`class_ref`)、Java メソッドの名前 (`"add"`)、およびメソッドの JNI シグネチャ (`"(II)I"`) が必要です。

メソッド ID が取得されると、`GetType` と `ThresholdType` が比較されて、仮想ディスパッチまたは非仮想ディスパッチが必要かどうかが判断されます。 `GetType` が `ThresholdType` と一致する場合は、メソッドをオーバーライドする Java によって割り当てられたサブクラスが `Handle` によって参照されている可能性があるため、仮想ディスパッチが必要です。

`GetType` が `ThresholdType` と一致しない場合は、`Adder` はサブクラス化されており (たとえば、`ManagedAdder` により)、サブクラスで `base.Add` が呼び出された場合にのみ、`Adder.Add` の実装が呼び出されます。 これは、非仮想ディスパッチのケースであり、これには `ThresholdClass` が関係します。 `ThresholdClass` では、呼び出すメソッドの実装を提供する Java クラスが指定されています。

#### <a name="method-registration"></a>メソッドの登録

`Adder.Add` メソッドをオーバーライドする更新された `ManagedAdder` の定義があるものとします。

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

`Adder.Add` には `[Register]` カスタム属性があったことを思い出してください。

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` カスタム属性のコンストラクターは、次の 3 つの値を受け取ります。

1. Java メソッドの名前。この場合は `"add"`。

1. メソッドの JNI 型シグネチャ。この場合は `"(II)I"`。

1. "*コネクタ メソッド*"。この場合は `GetAddHandler`。
    コネクタ メソッドについては後で説明します。

最初の 2 つのパラメーターにより、ACW 生成プロセスでメソッドの宣言を生成して、メソッドをオーバーライドできます。 結果の ACW には、次のコードの一部が含まれます。

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

`@Override` メソッドが宣言されていることに注意してください。これは、同じ名前の `n_` プレフィックス付きメソッドにデリゲートします。 これにより、Java コードで `ManagedAdder.add` を呼び出すと、`ManagedAdder.n_add` が呼び出され、C# の `ManagedAdder.Add` メソッドのオーバーライドを実行できるようになります。

そこで、最も重要な質問は、`ManagedAdder.n_add` は `ManagedAdder.Add` にどのようにフックされるかということです。

Java の `native` メソッドは、[JNI RegisterNatives 関数](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)によって Java ランタイム (Android ランタイム) に登録されます。
`RegisterNatives` は、Java のメソッド名、JNI の型シグネチャ、および呼び出す関数ポインターが含まれる構造体の配列を受け取ります。これは、[JNI 呼び出し規約](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)に従います。
関数ポインターは、後にメソッド パラメーターが付いた 2 つのポインター引数を受け取る関数である必要があります。 Java の `ManagedAdder.n_add` メソッドは、次の C プロトタイプを持つ関数によって実装される必要があります。

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android では `RegisterNatives` メソッドは公開されていません。 代わりに、ACW と MCW によって、`RegisterNatives` を呼び出すために必要な情報が提供されます。ACW には、メソッド名と JNI 型シグネチャが含まれており、足りないのはフックする関数ポインターだけです。

ここで、"*コネクタ メソッド*" が登場します。 3 番目の `[Register]` カスタム属性パラメーターは、登録される型または登録される型の基底クラスで定義されているメソッドの名前であり、このメソッドはパラメーターを受け取らず、`System.Delegate` を返します。 返される `System.Delegate` では、正しい JNI 関数シグネチャを持つメソッドが参照されています。 最後に、コネクタ メソッドによって返されるデリゲートは、デリゲートが Java に提供されるため、GC によって収集されないように、ルート化されている "*必要があります*"。

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

`GetAddHandler` メソッドによって作成される `Func<IntPtr, IntPtr, int, int,
int>` デリゲートでは、`n_Add` メソッドが参照されていて、[JNINativeWrapper.CreateDelegate](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*) が呼び出されます。
指定されたメソッドは `JNINativeWrapper.CreateDelegate` によって try/catch ブロックにラップされるため、ハンドルされない例外がハンドルされ、結果として [AndroidEvent.UnhandledExceptionRaiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser) イベントが発生します。 結果のデリゲートは、GC によってデリゲートが解放されないように、静的な `cb_add` 変数に格納されます。

最後に、`n_Add` メソッドでは、JNI パラメーターが対応するマネージド型にマーシャリングされてから、メソッドの呼び出しがデリゲートされます。

メモ:Java インスタンスで MCW を取得するときは、常に `JniHandleOwnership.DoNotTransfer` を使用します。 それらをローカル参照として扱う (したがって `JNIEnv.DeleteLocalRef` を呼び出す) と、マネージド -&gt; Java -&gt; マネージドのスタック遷移が壊れます。

### <a name="complete-adder-binding"></a>Adder の完全なバインド

`mono.android.tests.Adder` 型の完全なマネージド バインドは次のとおりです。

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```

### <a name="restrictions"></a>制約

次の条件に一致する型を作成する場合:

1. `Java.Lang.Object` をサブクラス化する

1. `[Register]` カスタム属性を持っている

1. `RegisterAttribute.DoNotGenerateAcw` が `true` である

この場合、GC との相互作用のため、型には、実行時に `Java.Lang.Object` または `Java.Lang.Object` サブクラスを参照する可能性のあるフィールドが "*存在しないようにする必要があります*"。 たとえば、`System.Object` 型およびすべてのインターフェイス型のフィールドは許可されません。 `System.String` や `List<int>` など、`Java.Lang.Object` インスタンスを参照できない型は許可されます。 この制限は、GC によってオブジェクトが早期に収集されないようにするためです。

`Java.Lang.Object` インスタンスを参照する可能性があるインスタンス フィールドを型に含める必要がある場合は、フィールドの型を `System.WeakReference` または `GCHandle` にする必要があります。

## <a name="binding-abstract-methods"></a>抽象メソッドのバインド

`abstract` メソッドのバインドは、仮想メソッドのバインドとほぼ同じです。 違うのは次の 2 点だけです。

1. 抽象メソッドは抽象です。 それでも `[Register]` 属性および関連付けられたメソッドの登録は保持されており、メソッドのバインドは `Invoker` 型に移動されるだけです。

1. 抽象型をサブクラス化する非 `abstract` の `Invoker` 型が作成されます。 `Invoker` 型では、基底クラスで宣言されているすべての抽象メソッドをオーバーライドする必要があり、オーバーライドされた実装はメソッドのバインドの実装ですが、非仮想ディスパッチのケースは無視できます。

たとえば、上記の `mono.android.test.Adder.add` メソッドが `abstract` であるものとします。 C# のバインドは `Adder.Add` が抽象であるように変更されて、新しい `AdderInvoker` 型は `Adder.Add` を実装するように定義されます。

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

`Invoker` 型は、Java で作成されたインスタンスに対する JNI 参照を取得する場合にのみ必要です。

## <a name="binding-interfaces"></a>インターフェイスのバインド

インターフェイスのバインドは、概念的には仮想メソッドを含むクラスのバインドに似ていますが、仕様の多くには微妙な (およびあまり微妙ではない) 違いがあります。 次のような [Java のインターフェイス宣言](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14)について考えます。

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

インターフェイスのバインドには、C# のインターフェイス定義と、インターフェイスに対する Invoker の定義の 2 つの部分があります。

### <a name="interface-definition"></a>インターフェイス定義

C# のインターフェイス定義では、次の要件が満たされている必要があります。

- インターフェイス定義には、`[Register]` カスタム属性が必要です。

- インターフェイス定義は、`IJavaObject interface` を拡張している必要があります。
    そうでないと、ACW は Java インターフェイスから継承できなくなります。

- 各インターフェイス メソッドには、対応する Java メソッド名、JNI シグネチャ、およびコネクタ メソッドを指定する `[Register]` 属性が含まれている必要があります。

- コネクタ メソッドでは、コネクタ メソッドを配置できる型も指定されている必要があります。

`abstract` メソッドと `virtual` メソッドをバインドするときは、登録されている型の継承階層内でコネクタ メソッドが検索されます。 インターフェイスは本体を含むメソッドを持つことができず、これは動作しないため、コネクタ メソッドが配置される場所を示す型を指定する必要があります。 型は、コネクタ メソッドの文字列内のコロン `':'` の後で指定され、呼び出し元を含む型のアセンブリ修飾型名である必要があります。

インターフェイス メソッドの宣言は、"*互換性のある*" 型を使用して、対応する Java メソッドを変換したものです。 Java の組み込み型の場合、互換性のある型は、対応する C# 型です。たとえば、Java の `int` は C# の `int` です。 参照型の場合、互換性のある型は、適切な Java 型の JNI ハンドルを提供できる型です。

インターフェイスのメンバーが Java によって直接呼び出されることはありません。呼び出しは、Invoker 型によって仲介されます。そのため、ある程度の柔軟性が許可されます。

Java の Progress インターフェイスは、[C# では次のように宣言する](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83)ことができます。

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

上の例では、Java の `int[]` パラメーターを [JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1) にマップしていることに注意してください。
これは必須ではありません。C# の `int[]`、`IList<int>`、またはその他のものにバインドすることもできます。 どのような型を選択する場合でも、`Invoker` では、呼び出しのために、それを Java の `int[]` 型に変換できる必要があります。

### <a name="invoker-definition"></a>Invoker の定義

`Invoker` 型の定義では、`Java.Lang.Object` を継承し、適切なインターフェイスを実装し、インターフェイス定義で参照されるすべての接続メソッドを提供する必要があります。 クラスのバインドとは異なる点がもう 1 つあります。`class_ref` フィールドとメソッド ID は、静的メンバーではなく、インスタンス メンバーである必要があります。

インスタンス メンバーにする理由は、Android ランタイムでの `JNIEnv.GetMethodID` の動作に関係があります。 (Java でもこのような動作になる可能性がありますが、テストされていません)。`JNIEnv.GetMethodID` では、宣言されたインターフェイスではなく、実装されたインターフェイスからのメソッドを検索すると、null が返されます。 [java.util.SortedMap&lt;K, V&gt;](https://developer.android.com/reference/java/util/SortedMap.html) という Java インターフェイスについて考えます。このインターフェイスでは、[java.util.Map&lt;K, V&gt;](https://developer.android.com/reference/java/util/Map.html) インターフェイスが実装されています。 Map では [clear](https://developer.android.com/reference/java/util/Map.html#clear()) メソッドが提供されているため、SortedMap に対して妥当であると思われる `Invoker` の定義は次のようになります。

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

`SortedMap` クラスのインスタンスを介して `Map.clear` メソッドを検索すると、`JNIEnv.GetMethodID` から `null` が返されるため、上記の定義は失敗します。

これには、2 つの解決策があります。1 つは、すべてのメソッドの元になっているインターフェイスを追跡し、各インターフェイスに対して `class_ref` を指定することです。もう 1 つは、すべてをインスタンス メンバーとして保持し、インターフェイス型ではなく、最も派生されたクラス型でメソッド参照を実行することです。 後者は **Mono.Android.dll** で行われます。

Invoker の定義には、コンストラクター、`Dispose` メソッド、`ThresholdType` メンバーと `ThresholdClass` メンバー、`GetObject` メソッド、インターフェイス メソッドの実装、およびコネクタ メソッドの実装の、6 つのセクションがあります。

#### <a name="constructor"></a>コンストラクター

コンストラクターでは、呼び出されるインスタンスのランタイム クラスを参照し、インスタンスの `class_ref` フィールドにランタイム クラスを格納する必要があります。

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

メモ:Android v4.0 では、基本コンストラクターの実行が終了した後では `handle` パラメーターが無効になる可能性があるため、`Handle` プロパティは、`handle` パラメーターではなく、コンストラクター本体内で使用する必要があります。

#### <a name="dispose-method"></a>Dispose メソッド

`Dispose` メソッドでは、コンストラクターで割り当てられたグローバル参照を解放する必要があります。

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```

#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType と ThresholdClass

`ThresholdType` メンバーと `ThresholdClass` メンバーは、クラスのバインドで使用されているものと同じです。

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

#### <a name="getobject-method"></a>GetObject メソッド

静的メソッド `GetObject` は、[Extensions.JavaCast&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*) をサポートするために必要です。

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>インターフェイス メソッド

インターフェイスのすべてのメソッドには実装が必要であり、実装では JNI を通じて対応する Java メソッドを呼び出します。

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```

#### <a name="connector-methods"></a>コネクタ メソッド

コネクタ メソッドとサポート インフラストラクチャの役割は、JNI パラメーターを適切な C# の型にマーシャリングすることです。 Java の `int[]` パラメーターは、JNI の `jintArray` として渡され、これは C# 内では `IntPtr` です。 C# インターフェイスの呼び出しをサポートするには、`IntPtr` を `JavaArray<int>` にマーシャリングする必要があります。

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

`int[]` が `JavaList<int>` より優先される場合は、代わりに [JNIEnv.GetArray()](xref:Android.Runtime.JNIEnv.GetArray*) を使用できます。

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

ただし、`JNIEnv.GetArray` を使うと VM 間で配列全体がコピーされるので、大きい配列の場合、これによって GC の負荷が大きくなる可能性があります。

### <a name="complete-invoker-definition"></a>Invoker の完全な定義

[完全な IAdderProgressInvoker の定義](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```

## <a name="jni-object-references"></a>JNI のオブジェクト参照

多くの JNIEnv メソッドでは、`GCHandle` に似た *JNI* "*オブジェクト参照*" が返されます。 JNI では、ローカル参照、グローバル参照、弱いグローバル参照という 3 種類のオブジェクト参照が提供されています。 3 つすべてが `System.IntPtr` と表されます。"*ただし*"、`JNIEnv` メソッドから返されるすべての `IntPtr` が参照であるとは限りません (JNI の関数型に関するセクションを参照)。 たとえば、[JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) からは `IntPtr` が返されますが、オブジェクト参照は返されず、`jmethodID` が返されます。 詳細については、[JNI 関数のドキュメント](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)を参照してください。

ローカル参照は、"*ほとんどの*" 参照作成メソッドによって作成されます。
Android では、特定の時点で存在できるローカル参照の数に制限があります (通常は 512)。 ローカル参照は [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) を使用して削除できます。
JNI とは異なり、オブジェクト参照を返すすべての参照 JNIEnv メソッドで、ローカル参照が返されるわけではありません。[JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*) では "*グローバル*" 参照が返されます。 可能な限り早くローカル参照を削除することを強くお勧めします。そのためには、通常、オブジェクトの周囲に [Java.Lang.Object](xref:Java.Lang.Object) を作成し、[Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) コンストラクターに対して `JniHandleOwnership.TransferLocalRef` を指定します。

グローバル参照は、[JNIEnv.NewGlobalRef](xref:Android.Runtime.JNIEnv.NewGlobalRef*) と [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*) によって作成されます。
それらは [JNIEnv.DeleteGlobalRef](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*) で破棄できます。
エミュレーターでは未処理のグローバル参照が 2,000 に制限されますが、ハードウェア デバイスでのグローバル参照の制限は約 52,000 です。

弱いグローバル参照は、Android v2.2 (Froyo) 以降でのみ使用できます。 弱いグローバル参照は、[JNIEnv.DeleteWeakGlobalRef](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*) で削除できます。

### <a name="dealing-with-jni-local-references"></a>JNI ローカル参照の処理

[JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*)、[JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*)、[JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*)、[JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*)、[JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) メソッドで返される `IntPtr` には、Java オブジェクトへの JNI ローカル参照が含まれるか、または Java で `null` が返される場合は `IntPtr.Zero` が含まれます。 同時に未処理にできるローカル参照の数は制限されているため (512 エントリ)、参照がタイムリーに削除されるようにすることをお勧めします。 ローカル参照の処理方法には、明示的に削除する、それを保持するための `Java.Lang.Object` インスタンスを作成する、`Java.Lang.Object.GetObject<T>()` を使用してマネージド呼び出し可能ラッパーを作成する、の 3 つがあります。

### <a name="explicitly-deleting-local-references"></a>ローカル参照の明示的な削除

ローカル参照を削除するには、[JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) を使用します。 削除されたローカル参照は使用できなくなるため、ローカル参照で最後に実行されるのが `JNIEnv.DeleteLocalRef` であるように注意する必要があります。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Java.Lang.Object でのラップ

`Java.Lang.Object` で提供されている [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) コンストラクターを使用すると、既存の JNI 参照をラップできます。 [JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership) パラメーターにより、`IntPtr` パラメーターの処理方法が決まります。

- [JniHandleOwnership.DoNotTransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; 作成された `Java.Lang.Object` インスタンスにより、`handle` パラメーターから新しいグローバル参照が作成され、`handle` は変更されません。
    呼び出し元は、必要に応じて `handle` を解放する必要があります。

- [JniHandleOwnership.TransferLocalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; 作成された `Java.Lang.Object` インスタンスにより、`handle` パラメーターから新しいグローバル参照が作成され、`handle` は [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) で削除されます。 呼び出し元は、`handle` を解放してはならず、コンストラクターの実行が完了した後で `handle` を使用することはできません。

- [JniHandleOwnership.TransferGlobalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; 作成された `Java.Lang.Object` インスタンスは、`handle` パラメーターの所有権を引き継ぎます。 呼び出し元は、`handle` を解放してはなりません。

JNI メソッドの呼び出しメソッドからはローカル参照が返されるため、通常は `JniHandleOwnership.TransferLocalRef` が使用されます。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

作成されたグローバル参照は、`Java.Lang.Object` インスタンスがガベージ コレクションされるまで解放されません。 可能であれば、インスタンスを破棄することでグローバル参照が解放され、ガベージ コレクションが高速化されます。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Using Java.Lang.Object.GetObject&lt;T&gt;()

`Java.Lang.Object` で提供される [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object.GetObject*) メソッドを使用すると、指定した型のマネージド呼び出し可能ラッパーを作成できます。

`T` 型は、次の要件を満たしている必要があります。

1. `T` は、参照型である必要があります。

1. `T` は、`IJavaObject` インターフェイスを実装している必要があります。

1. `T` が抽象クラスまたはインターフェイスでない場合、`T` ではパラメーターの型が `(IntPtr,
    JniHandleOwnership)` であるコンストラクターを提供する必要があります。

1. `T` が抽象クラスまたはインターフェイスの場合は、`T` で使用できる "*呼び出し元*" が存在する "*必要があります*"。 呼び出し元は、`T` を継承するか `T` を実装する非抽象型であり、`T` と同じ名前に Invoker サフィックスが付いたものです。 たとえば、T が `Java.Lang.IRunnable` インターフェイスである場合、型 `Java.Lang.IRunnableInvoker` が存在し、必要な `(IntPtr,
    JniHandleOwnership)` コンストラクターが含まれている必要があります。

JNI メソッドの呼び出しメソッドからはローカル参照が返されるため、通常は `JniHandleOwnership.TransferLocalRef` が使用されます。

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java の型の検索

JNI でフィールドまたはメソッドを検索するには、フィールドまたはメソッドの宣言する型を最初に検索する必要があります。 Java の型を検索するには、[Android.Runtime.JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*)) メソッドを使用します。 文字列パラメーターは、Java 型の "*略式型参照*" または "*完全な型参照*" です。 略式型参照と完全な型参照の詳細については、「[JNI の型参照](#_JNI_Type_References)」セクションを参照してください。

メモ:オブジェクト インスタンスを返す他のすべての `JNIEnv` メソッドとは異なり、`FindClass` ではローカル参照ではなくグローバル参照が返されます。

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>インスタンスのフィールド

フィールドは "*フィールド ID*" を使用して操作されます。 フィールド ID は、[JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*) によって取得されます。これには、フィールドが定義されているクラス、フィールドの名前、およびフィールドの [JNI 型シグネチャ](#JNI_Type_Signatures)が必要です。

フィールド ID は、解放する必要はなく、対応する Java 型が読み込まれている限り有効です。 (現在、Android ではクラスのアンロードはサポートされていません)。

インスタンス フィールドを操作するためのメソッドには、インスタンス フィールドを読み取るメソッドとインスタンス フィールドを書き込むメソッドの 2 つのセットがあります。 メソッドのすべてのセットで、フィールドの値を読み取るか書き込むためのフィールド ID が必要です。

### <a name="reading-instance-field-values"></a>インスタンス フィールドの値の読み取り

インスタンス フィールドの値を読み取るためのメソッドのセットは、次の名前付けパターンに従います。

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

`*` はフィールドの型です。

- [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; `java.lang.Object`、配列、インターフェイス型など、組み込み型ではない任意のインスタンス フィールドの値を読み取ります。 返される値は、JNI のローカル参照です。

- [JNIEnv.GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; `bool` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; `sbyte` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetCharField](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; `char` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; `short` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetIntField](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; `int` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetLongField](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; `long` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; `float` インスタンス フィールドの値を読み取ります。

- [JNIEnv.GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; `double` インスタンス フィールドの値を読み取ります。

### <a name="writing-instance-field-values"></a>インスタンス フィールドの値の書き込み

インスタンス フィールドの値を書き込むためのメソッドのセットは、次の名前付けパターンに従います。

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

*Type* はフィールドの型です。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `java.lang.Object`、配列、インターフェイス型など、組み込み型ではない任意のフィールドの値を書き込みます。 `IntPtr` の値には、JNI のローカル参照、JNI のグローバル参照、JNI の弱いグローバル参照、または `IntPtr.Zero` (`null` の場合) を使用できます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `bool` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `sbyte` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `char` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `short` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `int` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `long` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `float` インスタンス フィールドの値を書き込みます。

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; `double` インスタンス フィールドの値を書き込みます。

<a name="_Static_Fields" />

## <a name="static-fields"></a>静的フィールド

静的フィールドは "*フィールド ID*" を使用して操作されます。 フィールド ID は、[JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticFieldID*) によって取得されます。これには、フィールドが定義されているクラス、フィールドの名前、およびフィールドの [JNI 型シグネチャ](#JNI_Type_Signatures)が必要です。

フィールド ID は、解放する必要はなく、対応する Java 型が読み込まれている限り有効です。 (現在、Android ではクラスのアンロードはサポートされていません)。

静的フィールドを操作するためのメソッドには、静的フィールドを読み取るメソッドと静的フィールドを書き込むメソッドの 2 つのセットがあります。 メソッドのすべてのセットで、フィールドの値を読み取るか書き込むためのフィールド ID が必要です。

### <a name="reading-static-field-values"></a>静的フィールドの値の読み取り

静的フィールドの値を読み取るためのメソッドのセットは、次の名前付けパターンに従います。

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

`*` はフィールドの型です。

- [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; `java.lang.Object`、配列、インターフェイス型など、組み込み型ではない任意の静的フィールドの値を読み取ります。 返される値は、JNI のローカル参照です。

- [JNIEnv.GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; `bool` 静的フィールドの値を読み取ります。

- [JNIEnv.GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; `sbyte` 静的フィールドの値を読み取ります。

- [JNIEnv.GetStaticCharField](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; `char` 静的フィールドの値を読み取ります。

- [JNIEnv.GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; `short` 静的フィールドの値を読み取ります。

- [JNIEnv.GetStaticLongField](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; `long` 静的フィールドの値を読み取ります。

- [JNIEnv.GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; `float` 静的フィールドの値を読み取ります。

- [JNIEnv.GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; `double` 静的フィールドの値を読み取ります。

### <a name="writing-static-field-values"></a>静的フィールドの値の書き込み

静的フィールドの値を書き込むためのメソッドのセットは、次の名前付けパターンに従います。

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

*Type* はフィールドの型です。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `java.lang.Object`、配列、インターフェイス型など、組み込み型ではない任意の静的フィールドの値を書き込みます。 `IntPtr` の値には、JNI のローカル参照、JNI のグローバル参照、JNI の弱いグローバル参照、または `IntPtr.Zero` (`null` の場合) を使用できます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `bool` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `sbyte` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `char` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `short` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `int` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `long` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `float` 静的フィールドの値を書き込みます。

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; `double` 静的フィールドの値を書き込みます。

<a name="_Instance_Methods" />

## <a name="instance-methods"></a>インスタンス メソッド

インスタンス メソッドは "*メソッド ID*" によって呼び出されます。 メソッド ID は、[JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) によって取得されます。これには、メソッドが定義されているクラス、メソッドの名前、およびメソッドの [JNI 型シグネチャ](#JNI_Type_Signatures)が必要です。

メソッド ID は、解放する必要はなく、対応する Java 型が読み込まれている限り有効です。 (現在、Android ではクラスのアンロードはサポートされていません)。

メソッドを呼び出すためのメソッドには、仮想的に呼び出すメソッドと非仮想的に呼び出すメソッドの 2 つのセットがあります。 どちらのメソッドのセットにもメソッドを呼び出すためにメソッド ID が必要であり、非仮想的な呼び出しでは、呼び出すクラス実装も指定する必要があります。

インターフェイス メソッドは、宣言する型の中でのみ検索できます。拡張インターフェイスまたは継承インターフェイスからのメソッドを検索することはできません。 詳細については、インターフェイスのバインドおよび Invoker の実装に関するセクションを参照してください。

クラス、基底クラス、または実装インターフェイスで宣言されているすべてのメソッドを検索できます。

### <a name="virtual-method-invocation"></a>仮想メソッドの呼び出し

仮想的にメソッドを呼び出すメソッドのセットは、次の名前付けパターンに従います。

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

`*` はメソッドの戻り値の型です。

- [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; `java.lang.Object`、配列、インターフェイスなどの非組み込み型を返すメソッドを呼び出します。 返される値は、JNI のローカル参照です。

- [JNIEnv.CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; `bool` 値を返すメソッドを呼び出します。

- [JNIEnv.CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; `sbyte` 値を返すメソッドを呼び出します。

- [JNIEnv.CallCharMethod](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; `char` 値を返すメソッドを呼び出します。

- [JNIEnv.CallShortMethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; `short` 値を返すメソッドを呼び出します。

- [JNIEnv.CallLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; `long` 値を返すメソッドを呼び出します。

- [JNIEnv.CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; `float` 値を返すメソッドを呼び出します。

- [JNIEnv.CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; `double` 値を返すメソッドを呼び出します。

### <a name="non-virtual-method-invocation"></a>非仮想メソッドの呼び出し

非仮想的にメソッドを呼び出すメソッドのセットは、次の名前付けパターンに従います。

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

`*` はメソッドの戻り値の型です。 非仮想メソッドの呼び出しは、通常、仮想メソッドの基本メソッドを呼び出すために使用されます。

- [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; `java.lang.Object`、配列、インターフェイスなどの非組み込み型を返すメソッドを非仮想的に呼び出します。 返される値は、JNI のローカル参照です。

- [JNIEnv.CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; `bool` 値を返すメソッドを非仮想的に呼び出します。

- [JNIEnv.CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; `sbyte` 値を返すメソッドを非仮想的に呼び出します。

- [JNIEnv.CallNonvirtualCharMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; `char` 値を返すメソッドを非仮想的に呼び出します。

- [JNIEnv.CallNonvirtualShortMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; `short` 値を返すメソッドを非仮想的に呼び出します。

- [JNIEnv.CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; `long` 値を返すメソッドを非仮想的に呼び出します。

- [JNIEnv.CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; `float` 値を返すメソッドを非仮想的に呼び出します。

- [JNIEnv.CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; `double` 値を返すメソッドを非仮想的に呼び出します。

<a name="_Static_Methods" />

## <a name="static-methods"></a>静的メソッド

静的メソッドは "*メソッド ID*" によって呼び出されます。 メソッド ID は、[JNIEnv.GetStaticMethodID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) によって取得されます。これには、メソッドが定義されているクラス、メソッドの名前、およびメソッドの [JNI 型シグネチャ](#JNI_Type_Signatures)が必要です。

メソッド ID は、解放する必要はなく、対応する Java 型が読み込まれている限り有効です。 (現在、Android ではクラスのアンロードはサポートされていません)。

### <a name="static-method-invocation"></a>静的メソッドの呼び出し

仮想的にメソッドを呼び出すメソッドのセットは、次の名前付けパターンに従います。

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

`*` はメソッドの戻り値の型です。

- [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; `java.lang.Object`、配列、インターフェイスなどの非組み込み型を返す静的メソッドを呼び出します。 返される値は、JNI のローカル参照です。

- [JNIEnv.CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; `bool` 値を返す静的メソッドを呼び出します。

- [JNIEnv.CallStaticByteMethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; `sbyte` 値を返す静的メソッドを呼び出します。

- [JNIEnv.CallStaticCharMethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; `char` 値を返す静的メソッドを呼び出します。

- [JNIEnv.CallStaticShortMethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; `short` 値を返す静的メソッドを呼び出します。

- [JNIEnv.CallStaticLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; `long` 値を返す静的メソッドを呼び出します。

- [JNIEnv.CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; `float` 値を返す静的メソッドを呼び出します。

- [JNIEnv.CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; `double` 値を返す静的メソッドを呼び出します。

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI 型シグネチャ

[JNI 型シグネチャ](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) は、メソッドを除く [JNI 型参照](#_JNI_Type_References) です (ただし、簡略化されていない型参照)。 メソッドの場合の JNI 型シグネチャは、順番に、左かっこ `'('`、連結されたすべてのパラメーター型の型参照 (コンマや他の文字で区切りません)、右かっこ `')'`、メソッドの戻り値の型の JNI 型参照です。

たとえば、次のような Java メソッドがあるものとします。

```java
long f(int n, String s, int[] array);
```

JNI 型シグネチャは次のようになります。

```csharp
(ILjava/lang/String;[I)J
```

通常、JNI シグネチャを決定するには、`javap` コマンドを使用することを "*強く*" お勧めします。 たとえば、[java.lang.Thread.State.valueOf(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) メソッドの JNI 型シグネチャは "(Ljava/lang/String;)Ljava/lang/Thread$State;" ですが、[java.lang.Thread.State.values](https://developer.android.com/reference/java/lang/Thread.State.html#values) メソッドの JNI 型シグネチャは "()[Ljava/lang/Thread$State;" です。 末尾のセミコロンに注意してください。それらは JNI 型シグネチャの "*一部です*"。

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI 型参照

JNI 型参照は、Java 型参照とは異なります。 JNI では、`java.lang.String` などの完全修飾 Java 型名を使用することはできません。コンテキストに応じて、JNI のバリエーション `"java/lang/String"` または `"Ljava/lang/String;"` を使用する必要があります。詳細については、以下を参照してください。
4 種類の JNI 型参照があります。

- **組み込み**
- **略式**
- **type**
- **array**

### <a name="built-in-type-references"></a>組み込み型参照

組み込み型参照は、組み込みの値の型を参照するために使用される 1 つの文字です。 対応は次のとおりです。

- `"B"` は `sbyte`。
- `"S"` は `short`。
- `"I"` は `int`。
- `"J"` は `long`。
- `"F"` は `float`。
- `"D"` は `double`。
- `"C"` は `char`。
- `"Z"` は `bool`。
- `"V"` は `void` メソッドの戻り値の型。

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>略式型参照

略式型参照は、[JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*) においてのみ使用できます。
略式型参照を派生させるには、次の 2 つの方法があります。

1. 完全修飾 Java 名から、パッケージ名の内部および型名の前にあるすべての `'.'` を `'/'` に置き換え、型名の内部にあるすべての `'.'` を `'$'` に置き換えます。

1. `'unzip -l android.jar | grep JavaName'` の出力を読み取ります。

どちらの場合でも、Java 型 [java.lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html) は、略式型参照 `java/lang/Thread$State` にマップされます。

### <a name="type-references"></a>型参照

型参照は、組み込み型参照または略式型参照に `'L'` プレフィックスと `';'` サフィックスが付いたものです。 Java 型 [java.lang.String](https://developer.android.com/reference/java/lang/String.html) の場合、略式型参照は `"java/lang/String"` ですが、型参照は `"Ljava/lang/String;"` になります。

型参照は、配列型参照および JNI シグネチャで使用されます。

型参照を取得するもう 1 つの方法は、`'javap -s -classpath android.jar fully.qualified.Java.Name'` の出力を読み取ることです。
関係する型に応じて、コンストラクターの宣言またはメソッドの戻り値の型を使用して、JNI 名を決定できます。 次に例を示します。

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State` は Java 列挙型なので、`valueOf` メソッドのシグネチャを使用して、型参照が Ljava/lang/Thread$State; であることを決定できます。

### <a name="array-type-references"></a>配列型参照

配列型参照は、JNI 型参照にプレフィックス `'['` が付いたものです。
配列を指定するときは、略式型参照を使用できません。

たとえば、`int[]` は `"[I"`、`int[][]` は `"[[I"`、`java.lang.Object[]` は `"[Ljava/lang/Object;"` です。

## <a name="java-generics-and-type-erasure"></a>Java ジェネリックと型消去

"*ほとんど*" の場合、JNI で見られるように、Java ジェネリックは "*存在していません*"。
いくつかの "手段" がありますが、それらの手段は Java がジェネリックと対話する方法に関するものであり、JNI がジェネリック メンバーを検索して呼び出す方法ではありません。

JNI を介して対話する場合、ジェネリック型またはメンバーと非ジェネリック型またはメンバーの間に違いはありません。 たとえば、ジェネリック型 [java.lang.Class&lt;T&gt;](https://developer.android.com/reference/java/lang/Class.html) は "生の" ジェネリック型 `java.lang.Class` でもあり、どちらも略式型参照は同じ `"java/lang/Class"` です。

## <a name="java-native-interface-support"></a>Java ネイティブ インターフェイスのサポート

[Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv) は、Jave ネイティブ インターフェイス (JNI) 用のマネージド ラッパーです。 JNI 関数は [Java ネイティブ インターフェイス仕様](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)で宣言されていますが、メソッドは明示的な `JNIEnv*` パラメーターを削除するように変更されており、`jobject`、`jclass`、`jmethodID` などの代わりに `IntPtr` が使用されています。たとえば、次のような [JNI NewObject 関数](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517)について考えます。

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

これは [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*) メソッドとして公開されます。

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

2 つの呼び出しの間の変換は、かなり簡単です。 C では次のようになります。

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

同等の C# では次のようになります。

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr に Java オブジェクト インスタンスが格納されたら、おそらくそれで何かを行うでしょう。 [JNIEnv.CallVoidMethod()](xref:Android.Runtime.JNIEnv.CallVoidMethod*) などの JNIEnv メソッドを使用してそれを行うことができますが、既に類似の C# ラッパーがある場合は、JNI 参照に対してラッパーを構築することができます。 これは、[Extensions.JavaCast\<T>](xref:Android.Runtime.Extensions.JavaCast*) 拡張メソッドを使用して行うことができます。

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

また、[Java.Lang.Object.GetObject\<T>](xref:Java.Lang.Object.GetObject*) メソッドを使用することもできます。

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

さらに、すべての JNI 関数は、`JNIEnv*` パラメーターを削除することによって変更されています。

## <a name="summary"></a>まとめ

JNI を直接操作することは、何としてでも避ける必要がある面倒な経験です。 残念ながら、常に避けられるわけではありません。Mono for Android でバインドされていない Java に遭遇したとき、このガイドが何かのお役に立てれば幸いです。

## <a name="related-links"></a>関連リンク

- [Java ネイティブ インターフェイスの仕様](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java ネイティブ インターフェイスの関数](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
