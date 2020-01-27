---
title: JNI と Xamarin Android の使用
description: Xamarin では、Java ではなくC# 、を使用して android アプリを作成できます。 いくつかのアセンブリが Xamarin. Android に付属しています。これは、GoogleMaps や Mono などの Java ライブラリのバインドを提供します。 ただし、バインドは、可能なすべての Java ライブラリに対して提供されていません。また、提供されているバインドでは、Java のすべての型とメンバーをバインドできない場合があります。 バインドされていない Java の型とメンバーを使用するには、Java ネイティブインターフェイス (JNI) を使用できます。 この記事では、JNI を使用して、Xamarin Android アプリケーションから Java の型とメンバーを操作する方法について説明します。
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 0fa717a775ff2f1ace9e248a8afde8d373e8a1f8
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724345"
---
# <a name="working-with-jni-and-xamarinandroid"></a>JNI と Xamarin Android の使用

_Xamarin では、Java ではなくC# 、を使用して android アプリを作成できます。いくつかのアセンブリが Xamarin. Android に付属しています。これは、GoogleMaps や Mono などの Java ライブラリのバインドを提供します。ただし、バインドは、可能なすべての Java ライブラリに対して提供されていません。また、提供されているバインドでは、Java のすべての型とメンバーをバインドできない場合があります。バインドされていない Java の型とメンバーを使用するには、Java ネイティブインターフェイス (JNI) を使用できます。この記事では、JNI を使用して、Xamarin Android アプリケーションから Java の型とメンバーを操作する方法について説明します。_

## <a name="overview"></a>の概要

Java コードを呼び出すためにマネージ呼び出し可能ラッパー (MCW) を作成する必要は必ずしも必要ではありません。 多くの場合、"inline" JNI は、バインドされていない Java メンバーの1回限りの使用に適しています。 多くの場合、JNI を使用すると、jar バインド全体を生成するよりも、Java クラスで1つのメソッドを呼び出すことが簡単になります。

Xamarin Android には、Android の `android.jar` ライブラリのバインドを提供する `Mono.Android.dll` アセンブリが用意されています。 `Mono.Android.dll` 内に存在しない型およびメンバーは、手動でバインドすることによって、`android.jar` 内に存在しない型およびメンバーを使用できます。 Java の型とメンバーをバインドするには、 **Java ネイティブインターフェイス**(**JNI**) を使用して、型の参照、フィールドの読み取りと書き込み、およびメソッドの呼び出しを行います。

Xamarin. Android の JNI API は、概念的には .NET の `System.Reflection` API と非常によく似ています。これにより、型とメンバーを名前で検索したり、フィールド値の読み取りと書き込みを行ったり、メソッドを呼び出したりすることができます。 JNI と `Android.Runtime.RegisterAttribute` カスタム属性を使用して、オーバーライドをサポートするためにバインドできる仮想メソッドを宣言できます。 でC#実装できるように、インターフェイスをバインドできます。

このドキュメントでは以下について説明します。

- JNI が型を参照する方法。
- フィールドの参照、読み取り、および書き込みを行う方法。
- 方法: メソッドを参照して呼び出す。
- マネージコードからのオーバーライドを可能にする仮想メソッドを公開する方法。
- インターフェイスを公開する方法。

## <a name="requirements"></a>要件

JNI[名前空間](xref:Android.Runtime.JNIEnv)を通じて公開されているように、Xamarin. Android のすべてのバージョンで使用できます。
Java の型とインターフェイスをバインドするには、Xamarin Android 4.0 以降を使用する必要があります。

## <a name="managed-callable-wrappers"></a>マネージ呼び出し可能ラッパー

**マネージ呼び出し可能ラッパー** (**mcw**) は、すべての JNI メカニズムをラップする Java クラスまたはインターフェイスの*バインド*で、クライアントC#コードは JNI の基礎となる複雑さについて心配する必要がありません。 ほとんどの `Mono.Android.dll` は、マネージ呼び出し可能ラッパーで構成されています。

マネージ呼び出し可能ラッパーは、次の2つの目的で機能します。

1. クライアントコードが基になる複雑さについて認識する必要がないように、JNI use をカプセル化します。
1. サブクラスの Java 型を使用して、Java インターフェイスを実装できるようにします。

1つ目の目的は、単純なマネージ型のクラスを使用できるように、複雑さを簡単にカプセル化することです。 このためには、この記事の後半で説明するように、さまざまな[jて env](xref:Android.Runtime.JNIEnv)メンバーを使用する必要があります。 マネージ呼び出し可能ラッパーは厳密には必要ないことに注意してください &ndash; "inline" JNI use は完全に許容され、バインドされていない Java メンバーの1回限りの使用に役立ちます。 サブクラスおよびインターフェイスの実装には、マネージ呼び出し可能ラッパーを使用する必要があります。

## <a name="android-callable-wrappers"></a>Android 呼び出し可能ラッパー

Android ランタイム (アート) がマネージコードを呼び出す必要がある場合は常に、android 呼び出し可能ラッパー (ACW) が必要です。実行時にアートにクラスを登録する方法がないため、これらのラッパーが必要です。
(具体的には[、JNI 関数は、Android](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986)ランタイムではサポートされていません。 そのため、Android 呼び出し可能ラッパーによって、ランタイム型の登録がサポートされないようになります)。

Android コードでは、マネージコードでオーバーライドまたは実装された仮想メソッドまたはインターフェイスメソッドを実行する必要がある場合、そのメソッドが適切なマネージ型にディスパッチされるように Java プロキシを提供する必要があります。 これらの Java プロキシ型は、マネージ型として "同じ" 基底クラスと Java インターフェイスリストを持つ Java コードです。同じコンストラクターを実装し、オーバーライドされた基底クラスとインターフェイスメソッドを宣言します。

Android 呼び出し可能ラッパーは、[ビルドプロセス](~/android/deploy-test/building-apps/build-process.md)中に、**モノの id .exe**プログラムによって生成され、(直接または間接的に) [Java](xref:Java.Lang.Object)を継承するすべての型に対して生成されます。

### <a name="implementing-interfaces"></a>インターフェイスの実装

Android インターフェイス ( [IComponentCallbacks](xref:Android.Content.IComponentCallbacks)など) を実装することが必要になる場合があります。

Android のクラスとインターフェイスはすべて、 [IJavaObject](xref:Android.Runtime.IJavaObject)インターフェイスを拡張します。そのため、すべての Android の種類で `IJavaObject`を実装する必要があります。
Xamarin Android は、この事実を活用します。これは、`IJavaObject` を使用して、指定されたマネージ型の Java プロキシ (Android 呼び出し可能ラッパー) を Android に提供する &ndash; です。 **モノの id**は `Java.Lang.Object` サブクラス (`IJavaObject`を実装する必要があります) のみを検索するため、`Java.Lang.Object` をサブクラス化して、マネージコードでインターフェイスを実装する方法を提供します。 例:

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

*この記事の残りの部分では、予告なしに変更される可能性のある実装の詳細に*ついて説明します (ここでは、開発者が内部で何が起こっているかを知りたい場合にのみここに記載されています)。

たとえば、次C#のソースがあるとします。

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

**Manid .exe**プログラムは、次の Android 呼び出し可能ラッパーを生成します。

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

基本クラスが保持され、マネージコード内でオーバーライドされるメソッドごとにネイティブメソッド宣言が提供されることに注意してください。

### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute

通常、ACW を構成する Java コードは、Xamarin Android によって自動的に生成されます。この生成は、クラスが Java クラスから派生し、既存の Java メソッドをオーバーライドするときに、クラス名とメソッド名に基づいています。 ただし、一部のシナリオでは、次に示すように、コード生成が適切ではありません。

- Android では、 [android: onClick](xref:Android.Views.View.IOnClickListener.OnClick*) xml 属性などのレイアウト XML 属性のアクション名がサポートされています。 指定されている場合、拡大されたビューインスタンスは Java メソッドの検索を試みます。

- `readObject` と `writeObject` メソッドが必要[です。](https://developer.android.com/reference/java/io/Serializable.html) このインターフェイスのメンバーではないため、対応するマネージ実装ではこれらのメソッドが Java コードに公開されません。

- [Parcelable](xref:Android.OS.Parcelable)インターフェイスは、実装クラスが `Parcelable.Creator`型の静的フィールド `CREATOR` を持つ必要があることを想定しています。 生成された Java コードには、明示的なフィールドが必要です。 標準的なシナリオでは、マネージコードから Java コードのフィールドを出力することはできません。

コード生成には、任意の名前を持つ任意の Java メソッドを生成するためのソリューションが用意されていません。これは、Xamarin Android 4.2 以降では、上記のシナリオに対するソリューションを提供するために、 [Exportattribute](xref:Java.Interop.ExportAttribute)と[ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute)が導入されました。 両方の属性が `Java.Interop` 名前空間に存在します。

- `ExportAttribute` &ndash; は、メソッド名と予期される例外の種類 (Java で明示的に "スロー" を与えるため) を指定します。 メソッドで使用されている場合、メソッドは、ディスパッチコードを生成する Java メソッドをマネージメソッドに対応する JNI 呼び出しに "エクスポート" します。 これは、`android:onClick` と `java.io.Serializable`で使用できます。

- `ExportFieldAttribute` &ndash; フィールド名を指定します。 これは、フィールド初期化子として機能するメソッドに存在します。 これは、`android.os.Parcelable`と共に使用できます。

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute のトラブルシューティング

- パッケージが不足しているため、パッケージ化が失敗しまし**た。コード**または依存ライブラリの一部のメソッドで `ExportAttribute` または `ExportFieldAttribute` を使用した場合は、 **mono**. &ndash; を追加する必要があります。 このアセンブリは、Java からのコールバックコードをサポートするために分離されています。 これは、アプリケーションにサイズを追加するため、 **Mono. Android .dll**とは別です。

- リリースビルドでは、リリースビルドで &ndash; エクスポートメソッドに対して `MissingMethodException` が実行され、エクスポートメソッドに対して `MissingMethodException` が発生します。 (この問題は、最新バージョンの Xamarin Android で修正されています)。

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` と `ExportFieldAttribute` は、Java ランタイムコードが使用できる機能を提供します。 このランタイムコードは、これらの属性によって駆動される、生成された JNI メソッドを使用してマネージコードにアクセスします。 そのため、マネージメソッドがバインドする既存の Java メソッドはありません。したがって、Java メソッドはマネージメソッドシグネチャから生成されます。

ただし、この場合は、完全な式ではありません。 特に、マネージ型と Java 型の間の高度なマッピングには次のようなものがあります。

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

エクスポートされたメソッドにこのような型が必要な場合は、`ExportParameterAttribute` を使用して、対応するパラメーターまたは戻り値の型を明示的に指定する必要があります。

### <a name="annotation-attribute"></a>Annotation 属性

Xamarin Android 4.2 では `IAnnotation` 実装型を属性 (system.string) に変換し、Java ラッパーで注釈を生成するためのサポートを追加しました。

これは、次のように方向が変化することを意味します。

- バインディングジェネレーターは、`java.Lang.Deprecated` から `Java.Lang.DeprecatedAttribute` を生成します (マネージコードで `[Obsolete]` する必要があります)。

- これは、既存の `Java.Lang.Deprecated` クラスが非表示になるという意味ではありません。 これらの Java ベースのオブジェクトは、通常の Java オブジェクトと同様に使用される場合があります (このような使用方法が存在する場合)。 `Deprecated` クラスと `DeprecatedAttribute` クラスがあります。

- `Java.Lang.DeprecatedAttribute` クラスは `[Annotation]` としてマークされます。 この `[Annotation]` 属性から継承されたカスタム属性がある場合、msbuild タスクによって、Android 呼び出し可能ラッパー (ACW) でそのカスタム属性 (@Deprecated) の Java 注釈が生成されます。

- 注釈は、クラス、メソッド、およびエクスポートされたフィールド (マネージコード内のメソッド) に生成できます。

含んでいるクラス (注釈付きクラス自体、または注釈付きメンバーを含むクラス) が登録されていない場合は、注釈を含め、Java クラスソース全体がまったく生成されません。 メソッドについては、`ExportAttribute` を指定して、メソッドを明示的に生成して注釈を付けることができます。 また、Java annotation クラス定義を "生成" する機能でもありません。 言い換えると、特定の注釈に対してカスタムマネージ属性を定義する場合は、対応する Java annotation クラスを含む別の .jar ライブラリを追加する必要があります。 注釈の種類を定義する Java ソースファイルを追加するだけでは不十分です。 Java コンパイラは、 **apt**と同じようには機能しません。

さらに、次の制限事項が適用されます。

- この変換プロセスでは、これまでは注釈の種類の `@Target` 注釈は考慮されません。

- プロパティに対する属性が機能しません。 代わりに、プロパティ getter または setter の属性を使用してください。

## <a name="class-binding"></a>クラスのバインド

クラスをバインドすることは、基になる Java 型の呼び出しを簡略化するためにマネージ呼び出し可能ラッパーを作成することを意味します。

からのC#オーバーライドを許可するように仮想メソッドと抽象メソッドをバインドするには、Xamarin. Android 4.0 が必要です。 ただし、すべてのバージョンの Xamarin. Android では、非仮想メソッド、静的メソッド、または仮想メソッドをバインドして、上書きをサポートすることはできません。

通常、バインディングには次の項目が含まれます。

- [バインドされている Java 型への JNI ハンドル](#_Looking_up_Java_Types)。

- [JNI フィールド id とバインドされた各フィールドのプロパティ](#_Instance_Fields)。

- [JNI メソッドの id とメソッドをバインド](#_Instance_Methods)します。

- サブクラスが必要な場合、型は、 [registerattribute. DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw)が `true`に設定された型宣言に対して[registerattribute](xref:Android.Runtime.RegisterAttribute)カスタム属性を持つ必要があります。

### <a name="declaring-type-handle"></a>宣言 (型ハンドルを)

フィールドおよびメソッドの参照メソッドには、宣言する型を参照するオブジェクト参照が必要です。 慣例により、これは `class_ref` のフィールドに保持されます。

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

`CLASS` トークンの詳細については、「 [JNI 型の参照](#_JNI_Type_References)」を参照してください。

### <a name="binding-fields"></a>バインド (フィールドを)

Java フィールドはプロパティとC#して公開されます。たとえば、java フィールド[java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in)はC#プロパティ[Java.Lang.JavaSystem.In](xref:Java.Lang.JavaSystem.In)としてバインドされます。
さらに、JNI は静的フィールドとインスタンスフィールドを区別するため、プロパティを実装するときに異なるメソッドを使用します。

フィールドバインドには、次の3つのメソッドのセットが含まれます。

1. *Get field id*メソッド。 *Get field id*メソッドは、 *get field 値*および*set field value*メソッドが使用するフィールドハンドルを返す役割を担います。 フィールド id を取得するには、宣言する型、フィールドの名前、およびフィールドの[JNI type シグネチャ](#JNI_Type_Signatures)を知っている必要があります。

1. *フィールド値の取得*メソッド。 これらのメソッドはフィールドハンドルを必要とし、Java からフィールドの値を読み取る必要があります。
    使用するメソッドは、フィールドの型によって異なります。

1. *フィールド値の設定*メソッド。 これらのメソッドはフィールドハンドルを必要とし、Java 内でフィールドの値を記述します。 使用するメソッドは、フィールドの型によって異なります。

[静的フィールド](#_Static_Fields)では、 [jnienv](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)、`JNIEnv.GetStatic*Field`、および[jnienv field](xref:Android.Runtime.JNIEnv.SetStaticField*)の各メソッドが使用されます。

 [インスタンスフィールド](#_Instance_Fields)は、 [jnienv](xref:Android.Runtime.JNIEnv.GetFieldID*)、`JNIEnv.Get*Field`、および[Jnienv の各 setfield](xref:Android.Runtime.JNIEnv.SetField*)メソッドを使用します。

たとえば、静的なプロパティ `JavaSystem.In` は次のように実装できます。

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

メモ: [Inputstreaminvoker](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*)を使用して、参照を `System.IO.Stream` インスタンスに変換しています。また、`JniHandleOwnership.TransferLocalRef` を使用しています。これは、 [j objectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*)がローカル参照を返すためです。

多くの[Android ランタイム](xref:Android.Runtime)型には、JNI 参照を目的の型に変換するメソッドが `FromJniHandle` あります。

### <a name="method-binding"></a>メソッドのバインド

Java メソッドは、 C#メソッドとしてC# 、プロパティとして公開されます。 たとえば、java メソッドの[java. lang. runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) メソッドは、[Java.Lang.Runtime.RunFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*) メソッドとしてバインドされています。また、[.javaクラス](https://developer.android.com/reference/java/lang/Object.html#getClass)のメソッドは、[java. オブジェクト. クラス](xref:Java.Lang.Object.Class) プロパティとしてバインドされています。

メソッドの呼び出しは、次の2つの手順からなるプロセスです。

1. 呼び出すメソッドの*get メソッド id* 。 *Get メソッド id*メソッドは、メソッドの呼び出しメソッドが使用するメソッドハンドルを返す役割を担います。 メソッド id を取得するには、宣言する型、メソッドの名前、およびメソッドの[JNI type シグネチャ](#JNI_Type_Signatures)を知っている必要があります。

1. メソッドを呼び出します。

フィールドの場合と同様に、メソッド id を取得し、メソッドを呼び出すために使用するメソッドは、静的メソッドとインスタンスメソッドで異なります。

[静的メソッド](#_Static_Methods_1)では、 [GetStaticMethodID ()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)を使用してメソッド id を参照し、メソッドの `JNIEnv.CallStatic*Method` ファミリを使用して呼び出しを行います。

[インスタンスメソッド](#_Instance_Methods)は[GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)を使用してメソッド id を参照し、`JNIEnv.Call*Method` および `JNIEnv.CallNonvirtual*Method` のメソッドのファミリを使用して呼び出しを行います。

メソッドのバインドは、単なるメソッドの呼び出しよりも多くの可能性があります。 メソッドのバインドには、メソッドをオーバーライドする (抽象メソッドまたは final 以外のメソッドの場合) か、実装する (インターフェイスメソッドの場合) ことも含まれます。 「[継承のサポート](#_Supporting_Inheritance,_Interfaces_1)」のセクションでは、仮想メソッドとインターフェイスメソッドをサポートする複雑さについて説明します。

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>静的メソッド

静的メソッドをバインドするには、メソッドの戻り値の型に応じて、`JNIEnv.GetStaticMethodID` を使用してメソッドハンドルを取得し、適切な `JNIEnv.CallStatic*Method` メソッドを使用する必要があります。 [GetRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime())メソッドのバインドの例を次に示します。

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

メソッドハンドルは静的フィールド `id_getRuntime`に格納されていることに注意してください。 これはパフォーマンスの最適化であるため、呼び出しのたびにメソッドハンドルを検索する必要はありません。 メソッドハンドルをこの方法でキャッシュする必要はありません。 メソッドハンドルを取得した後は、メソッドを呼び出すために[Jnienv メソッド](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)が使用されます。 `JNIEnv.CallStaticObjectMethod` は、返された Java インスタンスのハンドルを含む `IntPtr` を返します。
Java [. lang.ini&lt;t&gt;(IntPtr、jの Handlehandleオーナーシップ)](xref:Java.Lang.Object.GetObject*)を使用して、java ハンドルを厳密に型指定されたオブジェクトインスタンスに変換します。

#### <a name="non-virtual-instance-method-binding"></a>非仮想インスタンスメソッドのバインド

`final` インスタンスメソッド、またはオーバーライドを必要としないインスタンスメソッドをバインドするには、メソッドの戻り値の型に応じて、`JNIEnv.GetMethodID` を使用してメソッドハンドルを取得し、適切な `JNIEnv.Call*Method` メソッドを使用する必要があります。 `Object.Class` プロパティのバインドの例を次に示します。

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

メソッドハンドルは静的フィールド `id_getClass`に格納されていることに注意してください。
これはパフォーマンスの最適化であるため、呼び出しのたびにメソッドハンドルを検索する必要はありません。 メソッドハンドルをこの方法でキャッシュする必要はありません。 メソッドハンドルを取得した後は、メソッドを呼び出すために[Jnienv メソッド](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)が使用されます。 `JNIEnv.CallStaticObjectMethod` は、返された Java インスタンスのハンドルを含む `IntPtr` を返します。
Java [. lang.ini&lt;t&gt;(IntPtr、jの Handlehandleオーナーシップ)](xref:Java.Lang.Object.GetObject*)を使用して、java ハンドルを厳密に型指定されたオブジェクトインスタンスに変換します。

### <a name="binding-constructors"></a>バインディングコンストラクター

コンストラクターは、`"<init>"`という名前の Java メソッドです。 Java インスタンスメソッドの場合と同様に、コンストラクターハンドルを参照するために `JNIEnv.GetMethodID` が使用されます。 Java メソッドとは異なり、コンストラクターメソッドハンドルを呼び出すために[Jnienv](xref:Android.Runtime.JNIEnv.NewObject*)メソッドが使用されます。 `JNIEnv.NewObject` の戻り値は、JNI ローカル参照です。

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

通常、クラスのバインドは、 [Java. Lang. オブジェクト](xref:Java.Lang.Object)をサブクラス化します。
`Java.Lang.Object`をサブクラス化すると、追加のセマンティックが得られます。 `Java.Lang.Object` インスタンスは、`Java.Lang.Object.Handle` プロパティを通じて Java インスタンスへのグローバル参照を保持します。

1. `Java.Lang.Object` の既定のコンストラクターは、Java インスタンスを割り当てます。

1. 型に `RegisterAttribute` があり、`RegisterAttribute.DoNotGenerateAcw` が `true` 場合は、既定のコンストラクターを使用して `RegisterAttribute.Name` 型のインスタンスが作成されます。

1. それ以外の場合、`this.GetType` に対応する[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) は、既定のコンストラクターを使用してインスタンス化されます。 Android 呼び出し可能ラッパーは、`RegisterAttribute.DoNotGenerateAcw` が `true`に設定されていない `Java.Lang.Object` サブクラスごとに、パッケージの作成時に生成されます。

クラスバインドではない型の場合、これは想定されるセマンティクスです。 C# `Mono.Samples.HelloWorld.HelloAndroid` インスタンスのインスタンス化では、生成された Android 呼び出し可能ラッパーである Java `mono.samples.helloworld.HelloAndroid` インスタンスを構築する必要があります。

クラスバインディングでは、Java 型に既定のコンストラクターが含まれている場合、または他のコンストラクターを呼び出す必要がない場合、これは正しい動作である可能性があります。 それ以外の場合は、次のアクションを実行するコンストラクターを指定する必要があります。

1. 既定の `Java.Lang.Object` コンストラクターの代わりに、 [Java. Lang. オブジェクト (IntPtr、jを Handlehandleオーナーシップ)](xref:Java.Lang.Object#ctor*)を呼び出します。 これは、新しい Java インスタンスを作成しないようにするために必要です。

1. Java インスタンスを作成する前に、 [.java](xref:Java.Lang.Object.Handle)の値を確認してください。 Android 呼び出し可能ラッパーが Java コードで構築されていて、作成された Android 呼び出し可能ラッパーインスタンスを格納するようにクラスバインディングが構築されている場合、`Object.Handle` プロパティは `IntPtr.Zero` 以外の値を持ちます。 たとえば、Android が `mono.samples.helloworld.HelloAndroid` インスタンスを作成する場合、最初に Android 呼び出し可能ラッパーが作成され、Java `HelloAndroid` コンストラクターによって、対応する `Mono.Samples.HelloWorld.HelloAndroid` 型のインスタンスが作成され、`Object.Handle` プロパティがコンストラクター実行前に Java インスタンスに設定されます。

1. 現在のランタイム型が宣言型と同じでない場合は、対応する Android 呼び出し可能ラッパーのインスタンスを作成し、 [SetHandle](xref:Java.Lang.Object.SetHandle*)を使用して、 [j](xref:Android.Runtime.JNIEnv.CreateInstance*)によって返されるハンドルを格納する必要があります。

1. 現在のランタイム型が宣言する型と同じである場合は、Java コンストラクターを呼び出し、`JNIEnv.NewInstance` によって返されるハンドルを格納するために [Object.SetHandle](xref:Java.Lang.Object.SetHandle*) を使用します。

たとえば、 [java. Integer (int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int))コンストラクターを考えてみます。 これは次のようにバインドされます。

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

[Jare env](xref:Android.Runtime.JNIEnv.CreateInstance*)メソッドは、`JNIEnv.FindClass`から返された値に対して `JNIEnv.FindClass`、`JNIEnv.GetMethodID`、`JNIEnv.NewObject`、および `JNIEnv.DeleteGlobalReference` を実行するためのヘルパーです。 詳細については、次のセクションを参照してください。

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>継承のサポート, インターフェイス

Java 型をサブクラス化するか、Java インターフェイスを実装するには、パッケージ化プロセス中にすべての `Java.Lang.Object` サブクラスに対して生成される[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (acws) を生成する必要があります。 ACW generation は、[カスタム属性](xref:Android.Runtime.RegisterAttribute)によって制御されます。

型C#の場合、`[Register]` カスタム属性コンストラクターには、対応する Java 型の[JNI 単純化型参照](#_Simplified_Type_References_1)の1つの引数が必要です。 これにより、Java との間C#で異なる名前を指定できます。

Xamarin Android 4.0 より前の `[Register]` カスタム属性は、既存の Java の種類 "エイリアス" では使用できませんでした。 これは、ACW の生成プロセスによって、検出された `Java.Lang.Object` サブクラスごとに ACWs が生成されるためです。

Xamarin. Android 4.0 では、 [DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw)プロパティが導入されました。 このプロパティは、注釈付きの型を*スキップ*するように ACW generation プロセスに指示します。これにより、パッケージの作成時に acws が生成されない新しいマネージ呼び出し可能ラッパーを宣言できます。 これにより、既存の Java 型をバインドできます。 たとえば、次の単純な Java クラスを考えてみます。このクラスには、整数にを加算して結果を返す1つのメソッド `add`である `Adder`が含まれています。

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

ここでは、 C# `Adder` 型の*エイリアス*`Adder` Java 型になります。 `[Register]` 属性は、`mono.android.test.Adder` Java 型の JNI 名を指定するために使用されます。 `DoNotGenerateAcw` プロパティは、ACW の生成を禁止するために使用されます。 これにより、`ManagedAdder` 型の ACW が生成され、`mono.android.test.Adder` 型が適切にサブクラス化されます。 `RegisterAttribute.DoNotGenerateAcw` プロパティが使用されていない場合は、Xamarin のビルドプロセスによって新しい `mono.android.test.Adder` Java 型が生成されます。 この場合、`mono.android.test.Adder` の型は2つの異なるファイルに2回存在するので、コンパイルエラーが発生します。

### <a name="binding-virtual-methods"></a>仮想メソッドのバインド

`ManagedAdder` サブクラスは Java `Adder` の種類ですが、特に重要C#ではありません。 `Adder` の種類では仮想メソッドが定義されていないため、`ManagedAdder` では何もオーバーライドできません。

サブクラスによるオーバーライドを許可する `virtual` メソッドをバインドするには、次の2つのカテゴリに分類されるいくつかの処理を行う必要があります。

1. **メソッドのバインド**

1. **メソッドの登録**

#### <a name="method-binding"></a>メソッドのバインド

メソッドのバインドでは、 C# `Adder` 定義に2つのサポートメンバーを追加する必要があります。 `ThresholdType`、`ThresholdClass`です。

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` プロパティは、バインディングの現在の型を返します。

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` は、仮想と非仮想メソッドのディスパッチを実行するタイミングを決定するために、メソッドバインドで使用されます。 常に、宣言C#する型に対応する `System.Type` インスタンスを返す必要があります。

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` プロパティは、バインドされた型の JNI クラス参照を返します。

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` は、非仮想メソッドを呼び出すときに、メソッドバインドで使用されます。

#### <a name="binding-implementation"></a>バインディングの実装

メソッドのバインディング実装は、Java メソッドのランタイム呼び出しを行います。 また、メソッドの登録の一部である `[Register]` カスタム属性の宣言も含まれており、メソッドの登録に関するセクションで説明します。

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

`id_add` フィールドには、呼び出す Java メソッドのメソッド ID が含まれています。 `id_add` 値は `JNIEnv.GetMethodID`から取得されます。これには、宣言するクラス (`class_ref`)、Java メソッド名 (`"add"`)、およびメソッドの JNI シグネチャ (`"(II)I"`) が必要です。

メソッド ID が取得されると、`GetType` が `ThresholdType` と比較され、仮想または非仮想ディスパッチが必要かどうかが判断されます。 `GetType` が `ThresholdType`と一致する場合は、メソッドをオーバーライドする Java によって割り当てられたサブクラスが `Handle` によって参照される場合があるため、仮想ディスパッチが必要です。

`GetType` が `ThresholdType`と一致しない場合、`Adder` はサブクラス化されています (例: `ManagedAdder`)。また、サブクラスが `Adder.Add` を呼び出した場合にのみ、`base.Add`実装が呼び出されます。 これは、非仮想ディスパッチケースです。これは `ThresholdClass` が存在する場所です。 `ThresholdClass` は、呼び出すメソッドの実装を提供する Java クラスを指定します。

#### <a name="method-registration"></a>メソッドの登録

`Adder.Add` メソッドをオーバーライドする更新された `ManagedAdder` 定義があるとします。

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

`Adder.Add` に `[Register]` カスタム属性があることを思い出してください。

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` カスタム属性コンストラクターは、次の3つの値を受け取ります。

1. Java メソッドの名前。この場合は `"add"`。

1. メソッドの JNI Type シグネチャ。この場合は `"(II)I"`。

1. *コネクタメソッド*。この場合は `GetAddHandler`。
    コネクタの方法については後で説明します。

最初の2つのパラメーターを使用すると、ACW generation プロセスでメソッド宣言を生成し、メソッドをオーバーライドできます。 結果として得られる ACW には、次のコードの一部が含まれます。

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

`@Override` メソッドが宣言されていることに注意してください。これは、同じ名前の `n_`プレフィックスのメソッドにデリゲートします。 これにより、Java コードが `ManagedAdder.add`を呼び出すと、`ManagedAdder.n_add` が呼び出され、オーバーライドC#する `ManagedAdder.Add` メソッドを実行できるようになります。

このように、最も重要な質問は、`ManagedAdder.Add`にどのように `ManagedAdder.n_add` フックされているかということです。

Java `native` メソッドは、 [JNI RegisterNatives 関数](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)を介して Java (Android ランタイム) ランタイムに登録されます。
`RegisterNatives` は、Java メソッド名、JNI 型シグネチャ、および[JNI 呼び出し規約](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)に従って呼び出す関数ポインターを含む構造体の配列を受け取ります。
関数ポインターは、2つのポインター引数の後にメソッドパラメーターを受け取る関数である必要があります。 Java `ManagedAdder.n_add` メソッドは、次の C プロトタイプを持つ関数を使用して実装する必要があります。

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin Android は、`RegisterNatives` メソッドを公開していません。 代わりに、ACW と MCW は `RegisterNatives`を呼び出すために必要な情報を提供します。 ACW にはメソッド名と JNI 型の署名が含まれていますが、不足しているのは、フックする関数ポインターだけです。

ここで、*コネクタのメソッド*が登場します。 3番目の `[Register]` カスタム属性パラメーターは、登録された型で定義されたメソッドの名前か、パラメーターを受け入れず `System.Delegate`を返す登録された型の基本クラスです。 返された `System.Delegate` は、正しい JNI 関数シグネチャを持つメソッドを参照します。 最後に、デリゲートが Java に提供されるため、コネクタメソッドが返すデリゲートは、GC によって収集されないように、ルートする*必要があり*ます。

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

`GetAddHandler` メソッドは、`n_Add` メソッドを参照する `Func<IntPtr, IntPtr, int, int,
int>` デリゲートを作成し、 [JNINativeWrapper](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*)を呼び出します。
`JNINativeWrapper.CreateDelegate` は、指定されたメソッドを try/catch ブロックにラップします。これにより、未処理の例外が処理され、 [UnhandledExceptionRaiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser)イベントが発生します。 結果として得られるデリゲートは、GC がデリゲートを解放しないように、静的な `cb_add` 変数に格納されます。

最後に、`n_Add` メソッドは、対応するマネージ型への JNI パラメーターのマーシャリング、およびメソッド呼び出しの委任を行います。

注: Java インスタンスで MCW を取得する場合は、常に `JniHandleOwnership.DoNotTransfer` を使用します。 これらをローカル参照として扱う (したがって `JNIEnv.DeleteLocalRef`を呼び出す) と、マネージ&gt; の Java&gt; マネージスタックの遷移が中断されます。

### <a name="complete-adder-binding"></a>補完バインドの完了

`mono.android.tests.Adder` 型の完全なマネージバインドは次のとおりです。

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

次の条件に一致する型を書き込む場合:

1. サブクラスの `Java.Lang.Object`

1. `[Register]` カスタム属性がある

1. `RegisterAttribute.DoNotGenerateAcw` は `true` です

次に、GC の相互作用の場合、型は、実行時に `Java.Lang.Object` または `Java.Lang.Object` サブクラスを参照するフィールドを持つことができ*ません*。 たとえば、型 `System.Object` のフィールドとインターフェイス型は使用できません。 `System.String` や `List<int>`など、`Java.Lang.Object` インスタンスを参照できない型も許可されます。 この制限は、GC によってオブジェクトが早期に収集されないようにするためです。

型に `Java.Lang.Object` インスタンスを参照できるインスタンスフィールドが含まれている必要がある場合は、フィールド型を `System.WeakReference` または `GCHandle`する必要があります。

## <a name="binding-abstract-methods"></a>抽象メソッドのバインド

バインド `abstract` メソッドは、仮想メソッドのバインドとほぼ同じです。 2つの違いがあります。

1. 抽象メソッドは abstract です。 `[Register]` 属性と関連付けられたメソッドの登録を保持します。メソッドのバインドは、単純に `Invoker` 型に移動されます。

1. 抽象型をサブクラス化する非 `abstract` `Invoker` 型が作成されます。 `Invoker` 型は、基本クラスで宣言されているすべての抽象メソッドをオーバーライドする必要があり、オーバーライドされた実装はメソッドのバインディング実装ですが、非仮想ディスパッチケースは無視できます。

たとえば、上記の `mono.android.test.Adder.add` メソッドが `abstract`であるとします。 バインドC#が変更されて `Adder.Add` が抽象的になり、`Adder.Add`を実装する新しい `AdderInvoker` 型が定義されるようになります。

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

`Invoker` 型は、Java で作成されたインスタンスへの JNI 参照を取得する場合にのみ必要です。

## <a name="binding-interfaces"></a>バインディングインターフェイス

バインディングインターフェイスは、概念的には仮想メソッドを含むバインドクラスに似ていますが、細かい点の多くは微妙な違いがあります。 次の[Java インターフェイス宣言](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14)について考えてみます。

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

インターフェイスバインディングには、インターフェイス定義C#とインターフェイスの呼び出し元定義の2つの部分があります。

### <a name="interface-definition"></a>インターフェイス定義

インターフェイスC#定義は、次の要件を満たしている必要があります。

- インターフェイス定義には `[Register]` カスタム属性が必要です。

- インターフェイス定義は `IJavaObject interface`を拡張する必要があります。
    そうしないと、ACWs が Java インターフェイスから継承できなくなります。

- 各インターフェイスメソッドには、対応する Java メソッド名、JNI シグネチャ、およびコネクタメソッドを指定する `[Register]` 属性が含まれている必要があります。

- コネクタメソッドでは、コネクタメソッドを配置できる型も指定する必要があります。

`abstract` メソッドと `virtual` メソッドをバインドすると、登録されている型の継承階層内でコネクタメソッドが検索されます。 インターフェイスには本体を含むメソッドを含めることができないため、これは動作しないため、コネクタメソッドが配置されている場所を示す型を指定する必要があります。 この型は、コネクタメソッド文字列内でコロン `':'`の後に指定され、呼び出し元を含む型のアセンブリ修飾型名である必要があります。

インターフェイスメソッドの宣言は、*互換性のある*型を使用して、対応する Java メソッドを変換したものです。 Java の組み込み型の場合、互換性のある型C#は対応する型です。たとえばC# 、java `int` は `int`です。 参照型の場合、互換性のある型は、適切な Java 型の JNI ハンドルを提供できる型です。

インターフェイスのメンバーは、Java によって直接呼び出されることはありません &ndash; 呼び出しは、呼び出し元の &ndash; 型によって仲介されるので、ある程度の柔軟性が許可されます。

Java Progress インターフェイスは、次[のようC#にで宣言](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83)できます。

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

上記の「Java `int[]` パラメーターを[JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1)にマップすることに注意してください。
これは必要ありません。 C# `int[]`、`IList<int>`、またはその他のものにバインドした可能性があります。 どのような種類を選択した場合でも、`Invoker` は、それを Java `int[]` 型に変換して呼び出しを行うことができなければなりません。

### <a name="invoker-definition"></a>呼び出し元の定義

`Invoker` 型定義は、`Java.Lang.Object`を継承し、適切なインターフェイスを実装し、インターフェイス定義で参照されるすべての接続メソッドを提供する必要があります。 クラスバインドとは異なる候補がもう1つあります。 `class_ref` フィールドとメソッド Id は、静的メンバーではなく、インスタンスメンバーである必要があります。

インスタンスメンバーを優先する理由は、Android ランタイムでの `JNIEnv.GetMethodID` の動作と同じです。 (これは Java の動作でもあり、テストされていません)。`JNIEnv.GetMethodID` は、宣言されたインターフェイスではなく、実装されたインターフェイスからのメソッドを検索するときに null を返します。 [&lt;k, v&gt;](https://developer.android.com/reference/java/util/Map.html)インターフェイスを実装する、 [&lt;k, v&gt;](https://developer.android.com/reference/java/util/SortedMap.html) java インターフェイスを使用することを検討してください。 Map には[明確](https://developer.android.com/reference/java/util/Map.html#clear())な方法が用意されているため、SortedMap の `Invoker` 定義は次のようになります。

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

`JNIEnv.GetMethodID` は `SortedMap` クラスのインスタンスを介して `Map.clear` メソッドを検索するときに `null` を返すため、上記のエラーは発生しません。

これには、次の2つの解決策があります。どのインターフェイスをどのメソッドから取得するかを追跡し、各インターフェイスの `class_ref` を保持するか、すべてをインスタンスメンバーとして保持し、インターフェイス型ではなく、最も派生されたクラス型に対してメソッド参照を実行します。 後者は、Mono. **Android .dll**で実行されます。

呼び出し元の定義には、コンストラクター、`Dispose` メソッド、`ThresholdType` と `ThresholdClass` のメンバー、`GetObject` メソッド、インターフェイスメソッドの実装、およびコネクタメソッドの実装の6つのセクションがあります。

#### <a name="constructor"></a>コンストラクター

コンストラクターは、呼び出されるインスタンスのランタイムクラスを参照し、ランタイムクラスを [インスタンス `class_ref`] フィールドに格納する必要があります。

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

注: `Handle` プロパティは、`handle` パラメーターではなく、コンストラクター本体内で使用する必要があります。 Android v4.0 の場合と同様に、基本コンストラクターの実行が完了した後、`handle` パラメーターは無効になる可能性があります。

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

`ThresholdType` メンバーと `ThresholdClass` メンバーは、クラスバインディングで見つかったものと同じです。

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

[JavaCast&lt;t&gt;()](xref:Android.Runtime.Extensions.JavaCast*)をサポートするには、静的 `GetObject` メソッドが必要です。

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>インターフェイス メソッド

インターフェイスのすべてのメソッドには、JNI を通じて対応する Java メソッドを呼び出す実装が必要です。

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

#### <a name="connector-methods"></a>コネクタのメソッド

コネクタのメソッドとサポートインフラストラクチャは、JNI パラメーターを適切なC#型にマーシャリングする役割を担います。 Java `int[]` パラメーターは、内C#の `IntPtr` である JNI `jintArray`として渡されます。 インターフェイスのC#呼び出しをサポートするために、`IntPtr` を `JavaArray<int>` にマーシャリングする必要があります。

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

`int[]` が `JavaList<int>`よりも優先される場合は、代わりに[GetArray ()](xref:Android.Runtime.JNIEnv.GetArray*)を使用できます。

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

ただし、`JNIEnv.GetArray` は Vm 間で配列全体をコピーするので、大きな配列の場合、これによって多くの GC の負荷が大きくなる可能性があります。

### <a name="complete-invoker-definition"></a>呼び出し元の定義の完了

[完全な Iadder進捗呼び出し](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88)元の定義:

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

## <a name="jni-object-references"></a>JNI オブジェクト参照

多くの JJNI Env メソッドは、`GCHandle`s に似た、 *オブジェクト参照*を返します。 JNI には、ローカル参照、グローバル参照、および弱いグローバル参照の3種類のオブジェクト参照が用意されています。 3つすべてが `System.IntPtr`として表されます*が*、`JNIEnv` メソッドから返されたすべての `IntPtr`が参照であるとは限りません。 たとえば、GetMethodID は `IntPtr`を返し[ます](xref:Android.Runtime.JNIEnv.GetMethodID*)が、オブジェクト参照を返しません。 `jmethodID`を返します。 詳細については、 [JNI 関数のドキュメント](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)を参照してください。

ローカル参照は、*ほとんど*の参照作成メソッドによって作成されます。
Android では、特定の時点 (通常は 512) に限られた数のローカル参照しか存在できません。 ローカル参照は、 [DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)を使用して削除できます。
JNI とは異なり、オブジェクト参照を返すすべての参照 j Env メソッドがローカル参照を返すわけではありません。[FindClass](xref:Android.Runtime.JNIEnv.FindClass*)は*グローバル*参照を返します。 ローカル参照は、可能な限り早く削除することを強くお勧めします。これは、オブジェクトの周囲に[java. lang. オブジェクト](xref:Java.Lang.Object)を作成し、そのオブジェクトに `JniHandleOwnership.TransferLocalRef` を指定することによって、[Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) コンストラクター。

グローバル参照は、 [Jare env. NewGlobalRef](xref:Android.Runtime.JNIEnv.NewGlobalRef*)と[Jの Env. findclass](xref:Android.Runtime.JNIEnv.FindClass*)によって作成されます。
これらは、 [DeleteGlobalRef](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*)で破棄できます。
エミュレーターには、2000の未処理のグローバル参照の制限がありますが、ハードウェアデバイスでは、52000のグローバル参照が制限されています。

弱いグローバル参照は、Android v1.0 (Froyo) 以降でのみ使用できます。 弱いグローバル参照を削除するには、 [jを削除します。](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*)

### <a name="dealing-with-jni-local-references"></a>JNI ローカル参照の処理

[JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*)、[JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*)、[JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*)、[JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*)、および [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) メソッドは `IntPtr` を返します。java オブジェクトへの JNI ローカル参照、または `IntPtr.Zero` java が返された場合は `null` を格納します。 ローカル参照の数が制限されているため (512 エントリ)、参照が適時に削除されることを確認することをお勧めします。 ローカル参照の処理方法には、明示的に削除する方法、それを保持するための `Java.Lang.Object` インスタンスを作成する方法、および `Java.Lang.Object.GetObject<T>()` を使用してマネージ呼び出し可能ラッパーを作成する方法の3つがあります。

### <a name="explicitly-deleting-local-references"></a>ローカル参照の明示的な削除

ローカル参照を削除するには、 [DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)を使用します。 ローカル参照を削除した後は使用できなくなります。そのため、ローカル参照で最後に実行された `JNIEnv.DeleteLocalRef` であることを確認する必要があります。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Java... オブジェクトを使用した折り返し

`Java.Lang.Object` には、終了している JNI 参照をラップするために使用できる、 [Java. オブジェクト (IntPtr ハンドル、jの Handleオーナーシップの転送)](xref:Java.Lang.Object#ctor*)コンストラクターが用意されています。 [Jの Handleオーナーシップ](xref:Android.Runtime.JniHandleOwnership)パラメーターは、`IntPtr` パラメーターの処理方法を決定します。

- 作成された `Java.Lang.Object`[インスタンス &ndash; 作成](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer)された `handle` パラメーターから新しいグローバル参照を作成し、`handle` を変更しません。
    呼び出し元は、必要に応じて `handle` を解放する役割を担います。

- [JniHandleOwnership.TransferLocalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; 生成された `Java.Lang.Object` インスタンスは、[JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) で削除された `handle` の所有権の譲渡によって新しいグローバル参照を作成し、`handle` パラメーターから新しいグローバル参照を作成します。 呼び出し元は `handle` を解放する必要がなく、コンストラクターの実行が完了した後で `handle` を使用することはできません。

- 作成された `Java.Lang.Object` インスタンスは、`handle` パラメーター[の所有権を引き継ぎ &ndash;。](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) 呼び出し元は `handle` を解放しないでください。

JNI メソッドの呼び出しメソッドはローカル refs を返すため、通常は `JniHandleOwnership.TransferLocalRef` が使用されます。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

作成されたグローバル参照は、`Java.Lang.Object` インスタンスがガベージコレクションされるまで解放されません。 可能であれば、インスタンスを破棄するとグローバル参照が解放され、ガベージコレクションが高速化されます。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>&lt;T&gt;() を使用して、

`Java.Lang.Object` には、指定された型のマネージ呼び出し可能ラッパーを作成するために使用できる、 [.java&lt;t&gt;(IntPtr ハンドル、jの Handleオーナーシップの転送)](xref:Java.Lang.Object.GetObject*)メソッドが用意されています。

`T` 型は、次の要件を満たしている必要があります。

1. `T` は参照型である必要があります。

1. `T` は `IJavaObject` インターフェイスを実装する必要があります。

1. `T` が抽象クラスまたはインターフェイスでない場合、`T` は `(IntPtr,
    JniHandleOwnership)` パラメーター型を持つコンストラクターを提供する必要があります。

1. `T` が抽象クラスまたはインターフェイスの場合は、`T` に使用できる*呼び出し*元が存在している*必要*があります。 呼び出し元は、`T` を継承するか `T` を実装する非抽象型であり、呼び出し元のサフィックスと同じ `T` と同じ名前です。 たとえば、T が `Java.Lang.IRunnable` インターフェイスである場合、型 `Java.Lang.IRunnableInvoker` が存在し、必要な `(IntPtr,
    JniHandleOwnership)` コンストラクターが含まれている必要があります。

JNI メソッドの呼び出しメソッドはローカル refs を返すため、通常は `JniHandleOwnership.TransferLocalRef` が使用されます。

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java の種類の検索

JNI でフィールドまたはメソッドを参照するには、フィールドまたはメソッドの宣言する型を最初に検索する必要があります。 Java 型を参照するには、 [Android. Runtime. FindClass (string)](xref:Android.Runtime.JNIEnv.FindClass*)メソッドを使用します。 文字列パラメーターは、簡略化された*型参照*、または Java 型の*完全な型参照*です。 単純型と完全型の参照の詳細については、「 [JNI 型の参照」](#_JNI_Type_References)を参照してください。

注: オブジェクトインスタンスを返す他の `JNIEnv` メソッドとは異なり、`FindClass` はローカル参照ではなく、グローバル参照を返します。

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>インスタンスフィールド

フィールドは*フィールド id*を通じて操作されます。 フィールド Id は、フィールドが定義されているクラス、フィールドの名前、およびフィールドの[JNI Type 署名](#JNI_Type_Signatures)を必要とする、j によって取得され[ます。](xref:Android.Runtime.JNIEnv.GetFieldID*)

フィールド Id は解放する必要はなく、対応する Java 型が読み込まれる限り有効です。 (Android では、現在、クラスのアンロードはサポートされていません)。

インスタンスフィールドを操作するためのメソッドには、インスタンスフィールドを読み取るメソッドとインスタンスフィールドを書き込むメソッドの2つのセットがあります。 メソッドのすべてのセットでは、フィールド値の読み取りまたは書き込みを行うためにフィールド ID が必要です。

### <a name="reading-instance-field-values"></a>インスタンスフィールド値の読み取り

インスタンスフィールド値を読み取るためのメソッドのセットは、名前付けパターンに従います。

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

ここで `*` はフィールドの型です。

- `java.lang.Object`、配列、インターフェイスの種類などの組み込み型ではないインスタンスフィールドの値を読み取るには、 [jの &ndash; GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*)を指定します。 返される値は、JNI ローカル参照です。

- [GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; `bool` インスタンスフィールドの値を読み取ります。

- [GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; `sbyte` インスタンスフィールドの値を読み取ります。

- `char` インスタンスフィールドの値 &ndash; 読み取るには、 [GetCharField](xref:Android.Runtime.JNIEnv.GetCharField*)を呼び出します。

- [GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; `short` インスタンスフィールドの値を読み取ります。

- `int` インスタンスフィールドの値 &ndash; 読み取るための[GetIntField](xref:Android.Runtime.JNIEnv.GetIntField*)です。

- [GetLongField](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; `long` インスタンスフィールドの値を読み取ります。

- [GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; `float` インスタンスフィールドの値を読み取ります。

- [GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; `double` インスタンスフィールドの値を読み取ります。

### <a name="writing-instance-field-values"></a>インスタンスフィールド値の書き込み

インスタンスフィールド値を書き込むためのメソッドのセットは、次の名前付けパターンに従います。

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

ここで、 *type*はフィールドの型です。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash;、`java.lang.Object`、配列、インターフェイス型などの組み込み型ではないフィールドの値を書き込むことができます。 `IntPtr` 値には、JNI ローカル参照、JNI グローバル参照、JNI weak グローバル参照、または `IntPtr.Zero` (`null`) を指定できます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `bool` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `sbyte` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `char` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `short` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `int` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `long` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `float` インスタンスフィールドの値を書き込むことができます。

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `double` インスタンスフィールドの値を書き込むことができます。

<a name="_Static_Fields" />

## <a name="static-fields"></a>静的フィールド

静的フィールドは、*フィールド id*を通じて操作されます。 フィールド Id は、フィールドが定義されているクラス、フィールドの名前、およびフィールドの[JNI 型の署名](#JNI_Type_Signatures)を必要とする、j によって取得され[ます。](xref:Android.Runtime.JNIEnv.GetStaticFieldID*)

フィールド Id は解放する必要はなく、対応する Java 型が読み込まれる限り有効です。 (Android では、現在、クラスのアンロードはサポートされていません)。

静的フィールドを操作するためのメソッドには、インスタンスフィールドを読み取るメソッドとインスタンスフィールドを書き込むメソッドの2つのセットがあります。 メソッドのすべてのセットでは、フィールド値の読み取りまたは書き込みを行うためにフィールド ID が必要です。

### <a name="reading-static-field-values"></a>静的フィールド値の読み取り

静的フィールド値を読み取るためのメソッドのセットは、名前付けパターンに従います。

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

ここで `*` はフィールドの型です。

- [GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash;、`java.lang.Object`、配列、インターフェイス型などの組み込み型ではない静的フィールドの値を読み取ります。 返される値は、JNI ローカル参照です。

- [GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; `bool` の静的フィールドの値を読み取ります。

- [GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; `sbyte` の静的フィールドの値を読み取ります。

- [GetStaticCharField](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; `char` の静的フィールドの値を読み取ります。

- [GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; `short` の静的フィールドの値を読み取ります。

- [GetStaticLongField](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; `long` の静的フィールドの値を読み取ります。

- [GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; `float` の静的フィールドの値を読み取ります。

- [GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; `double` の静的フィールドの値を読み取ります。

### <a name="writing-static-field-values"></a>静的フィールド値の書き込み

静的フィールド値を書き込むためのメソッドのセットは、次の名前付けパターンに従います。

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

ここで、 *type*はフィールドの型です。

- `java.lang.Object`、配列、インターフェイス型などの組み込み型ではない静的フィールドの値は、 [Jnienv フィールド](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; 書き込みます。 `IntPtr` 値には、JNI ローカル参照、JNI グローバル参照、JNI weak グローバル参照、または `IntPtr.Zero` (`null`) を指定できます。

- `bool` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `sbyte` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `char` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `short` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `int` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `long` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `float` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

- `double` 静的なフィールド) の値 &ndash; 書き込むことが[でき](xref:Android.Runtime.JNIEnv.SetStaticField*)ます。

<a name="_Instance_Methods" />

## <a name="instance-methods"></a>インスタンスメソッド

インスタンスメソッドは、*メソッド id*を通じて呼び出されます。 メソッド Id を取得するには、GetMethodID を使用します[。](xref:Android.Runtime.JNIEnv.GetMethodID*)これには、メソッドが定義されている型、メソッドの名前、およびメソッドの[JNI 型シグネチャ](#JNI_Type_Signatures)が必要です。

メソッド Id は解放する必要はなく、対応する Java 型が読み込まれる限り有効です。 (Android では、現在、クラスのアンロードはサポートされていません)。

メソッドを呼び出すメソッドには2つのセットがあります。1つはメソッドを仮想的に呼び出すメソッドで、もう1つはメソッドを呼び出すためのメソッドです。 どちらのメソッドのセットもメソッドを呼び出すためにメソッド ID を必要とし、非仮想呼び出しでは、呼び出すクラス実装を指定する必要があります。

インターフェイスメソッドは、宣言する型内でのみ検索できます。拡張/継承されたインターフェイスからのメソッドを参照することはできません。 詳細については、後の「バインドインターフェイス/呼び出し元の実装」を参照してください。

クラス、または基底クラスまたは実装されているインターフェイスで宣言されているすべてのメソッドを検索できます。

### <a name="virtual-method-invocation"></a>仮想メソッドの呼び出し

メソッドを呼び出すメソッドのセットは、実質的に名前付けパターンに従います。

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

ここで `*` はメソッドの戻り値の型です。

- [CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; は、`java.lang.Object`、配列、インターフェイスなどの非 builtin 型を返すメソッドを呼び出します。 返される値は、JNI ローカル参照です。

- [CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; `bool` 値を返すメソッドを呼び出します。

- [JNIEnv.CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; `sbyte` 値を返すメソッドを呼び出します。

- &ndash; `char` 値を返すメソッドを呼び出すメソッドを呼び出し[ます。](xref:Android.Runtime.JNIEnv.CallCharMethod*)

- &ndash; `short` 値を返すメソッドを呼び出すメソッドを呼び出すことができます[。](xref:Android.Runtime.JNIEnv.CallShortMethod*)

- &ndash; `long` 値を返すメソッドを呼び出すメソッドを呼び出し[ます。](xref:Android.Runtime.JNIEnv.CallLongMethod*)

- [CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; `float` 値を返すメソッドを呼び出します。

- [CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; `double` 値を返すメソッドを呼び出します。

### <a name="non-virtual-method-invocation"></a>非仮想メソッドの呼び出し

メソッドを呼び出すためのメソッドのセットは、次のような名前付けパターンに従います。

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

ここで `*` はメソッドの戻り値の型です。 非仮想メソッドの呼び出しは、通常、仮想メソッドの基本メソッドを呼び出すために使用されます。

- `java.lang.Object`、配列、インターフェイスなど、非組み込み型を返すメソッドを仮想的に呼び出すことはできません。このメソッドは、&ndash; 実質的には呼び出さ[れません。](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) 返される値は、JNI ローカル参照です。

- [CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash;、`bool` 値を返すメソッドを仮想的に呼び出すことはできません。

- [CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash;、`sbyte` 値を返すメソッドを仮想的に呼び出すことはできません。

- &ndash;、`char` 値を返すメソッドを仮想的に呼び出すことは[できません。](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*)

- &ndash;、`short` 値を返すメソッドを仮想的に呼び出すことは[できません。](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*)

- [CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash;、`long` 値を返すメソッドを仮想的に呼び出すことはできません。

- [CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash;、`float` 値を返すメソッドを仮想的に呼び出すことはできません。

- [CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash;、`double` 値を返すメソッドを仮想的に呼び出すことはできません。

<a name="_Static_Methods" />

## <a name="static-methods"></a>静的メソッド

静的メソッドは、*メソッド id*を通じて呼び出されます。 メソッド Id を取得するには、GetStaticMethodID を使用します[。](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)これには、メソッドが定義されている型、メソッドの名前、およびメソッドの[JNI 型シグネチャ](#JNI_Type_Signatures)が必要です。

メソッド Id は解放する必要はなく、対応する Java 型が読み込まれる限り有効です。 (Android では、現在、クラスのアンロードはサポートされていません)。

### <a name="static-method-invocation"></a>静的メソッドの呼び出し

メソッドを呼び出すメソッドのセットは、実質的に名前付けパターンに従います。

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

ここで `*` はメソッドの戻り値の型です。

- `java.lang.Object`、配列、インターフェイスなどの非 builtin 型を返す静的メソッドを呼び出す &ndash;、 [Jnienv メソッド](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)を呼び出します。 返される値は、JNI ローカル参照です。

- [CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; `bool` 値を返す静的メソッドを呼び出します。

- `sbyte` 値を返す静的メソッドを呼び出す &ndash; は、 [Jnienv を呼び出します。](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*)

- `char` 値を返す静的メソッドを呼び出すには、 [Jnienv メソッド &ndash; 呼び出します。](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*)

- `short` 値を返す静的メソッドを呼び出す &ndash;、このメソッドを呼び出し[ます。](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*)

- `long` 値を返す静的メソッドを呼び出す &ndash;、このメソッドを呼び出し[ます。](xref:Android.Runtime.JNIEnv.CallLongMethod*)

- [CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; `float` 値を返す静的メソッドを呼び出します。

- [CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; `double` 値を返す静的メソッドを呼び出します。

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI 型シグネチャ

[JNI 型シグネチャ](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432)は、メソッドを除き、 [JNI 型参照](#_JNI_Type_References)です (ただし、簡略化された型参照ではありません)。 メソッドを使用する場合、JNI 型シグネチャは、始めかっこ `'('`、その後に連結されたすべてのパラメーター型の型参照 (コンマや他の文字を区別しない)、続けて終わりかっこ `')'`、メソッドの戻り値の型の JNI 型参照を続けたものです。

たとえば、Java メソッドの場合は、次のようになります。

```java
long f(int n, String s, int[] array);
```

JNI 型シグネチャは次のようになります。

```csharp
(ILjava/lang/String;[I)J
```

一般に、JNI 署名を決定するには、`javap` コマンドを使用することを*強く*お勧めします。 たとえば、JNI [(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String))メソッドの JNI 型シグネチャは、"(ljava/Lang/string;) ljava/Lang/Thread $ state;" ですが、 [java.](https://developer.android.com/reference/java/lang/Thread.State.html#values)メソッドの type シグネチャは "() [Ljava/Lang/thread $ state;" になっているとします。 末尾のセミコロンをご覧ください。これら*は*JNI 型シグネチャの一部です。

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI 型参照

JNI 型参照は、Java 型参照とは異なります。 JNI で `java.lang.String` などの完全修飾 Java 型名を使用することはできません。コンテキストに応じて、JNI のバリエーション `"java/lang/String"` または `"Ljava/lang/String;"`を使用する必要があります。詳細については、以下を参照してください。
JNI 型参照には、次の4種類があります。

- **built-in**
- **simplified**
- **type**
- **array**

### <a name="built-in-type-references"></a>組み込み型参照

組み込み型参照は、組み込みの値型を参照するために使用される1つの文字です。 マッピングは次のとおりです。

- `sbyte` の `"B"`。
- `short` の `"S"`。
- `int` の `"I"`。
- `long` の `"J"`。
- `float` の `"F"`。
- `double` の `"D"`。
- `char` の `"C"`。
- `bool` の `"Z"`。
- `void` メソッドの戻り値の型の `"V"`。

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>単純型参照

簡略化された型参照は、 [jの env でのみ使用できます。 FindClass (string)](xref:Android.Runtime.JNIEnv.FindClass*))。
単純型参照を派生させるには、次の2つの方法があります。

1. 完全修飾 Java 名から、パッケージ名の前と型名の前にあるすべての `'.'` を `'/'` に置き換え、型名内のすべての `'.'` を `'$'` で置き換えます。

1. `'unzip -l android.jar | grep JavaName'` の出力を読み取ります。

どちらの場合も、Java 型 [java.lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html) は、簡略化された型参照 `java/lang/Thread$State` にマップされます。

### <a name="type-references"></a>型参照

型参照は、組み込み型参照、または `'L'` プレフィックスと `';'` サフィックスを持つ単純型参照です。 Java 型の場合[は、単純](https://developer.android.com/reference/java/lang/String.html)型参照が `"java/lang/String"`ますが、型参照は `"Ljava/lang/String;"`ます。

型参照は、配列型参照および JNI シグネチャと共に使用されます。

型参照を取得するための追加の方法として、`'javap -s -classpath android.jar fully.qualified.Java.Name'`の出力を読み取る方法があります。
関連する型に応じて、コンストラクターの宣言またはメソッドの戻り値の型を使用して、JNI 名を決定できます。 例:

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

`Thread.State` は Java 列挙型であるため、`valueOf` メソッドのシグネチャを使用して、型参照が Ljava/lang/Thread $ State; であることを確認できます。

### <a name="array-type-references"></a>配列型の参照

配列型参照は、JNI 型参照の先頭に `'['` ます。
配列を指定するときに、簡略化された型参照を使用することはできません。

たとえば、`int[]` が `"[I"`、`int[][]` が `"[[I"`、`java.lang.Object[]` が `"[Ljava/lang/Object;"`ているとします。

## <a name="java-generics-and-type-erasure"></a>Java ジェネリックと型消去

*ほとんど*の場合、JNI で見られるように、Java ジェネリック*は存在しません*。
"しわ" にはいくつかの "しわ" がありますが、JNI の検索方法やジェネリックメンバーの呼び出し方法ではなく、Java がジェネリックと対話する方法があります。

JNI を介して対話する場合、ジェネリック型またはメンバーと非ジェネリック型またはメンバーの間に違いはありません。 たとえば、ジェネリック型の[&lt;t&gt;](https://developer.android.com/reference/java/lang/Class.html)も、"生の" ジェネリック型 `java.lang.Class`であり、どちらも単純型参照が同じであり、`"java/lang/Class"`です。

## <a name="java-native-interface-support"></a>Java ネイティブインターフェイスのサポート

[Android. Runtime. JJNI env](xref:Android.Runtime.JNIEnv)は、Jave ネイティブインターフェイス () のマネージラッパーです。 JNI 関数は[Java ネイティブインターフェイスの仕様](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)内で宣言されていますが、明示的な `JNIEnv*` パラメーターを削除するようにメソッドが変更されており、`jobject`、`jclass`、`jmethodID`などではなく `IntPtr` が使用されています。たとえば、 [JNI NewObject 関数](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517)を考えてみます。

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

これは、 [J/NewObject](xref:Android.Runtime.JNIEnv.NewObject*)メソッドとして公開されています。

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

2つの呼び出しの間の変換は、合理的に簡単です。 C では、次のことを行います。

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

次C#のような結果が得られます。

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr に Java オブジェクトインスタンスを保持している場合は、そのインスタンスで何らかの処理を実行することをお勧めします。 JJNI [() などの jの](xref:Android.Runtime.JNIEnv.CallVoidMethod*)env メソッドを使用してこれを行うことができますが、既にアナログC#ラッパーがある場合は、ラッパーを構築して参照を作成する必要があります。 これを行うには、 [JavaCast\<t >](xref:Android.Runtime.Extensions.JavaCast*)拡張メソッドを使用します。

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

また、 [\<t >](xref:Java.Lang.Object.GetObject*)メソッドを使用することもできます。

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

さらに、すべての JNI 関数に存在する `JNIEnv*` パラメーターを削除することによって、すべての JNI 関数が変更されています。

## <a name="summary"></a>要約

JNI を直接処理することは、すべてのコストで回避する必要がある、非常に手間のかかるエクスペリエンスです。 残念ながら、これは常にあればではありません。このガイドでは、Mono for Android でバインドされていない Java ケースにヒットしたときに、いくつかのサポートを提供します。

## <a name="related-links"></a>関連リンク

- [Java ネイティブインターフェイスの仕様](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java ネイティブインターフェイス関数](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
