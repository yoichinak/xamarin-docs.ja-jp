---
title: JNI の使用
description: Xamarin.Android で Android アプリの作成を許可するC#Java の代わりにします。 いくつかのアセンブリは、Mono.Android.dll Mono.Android.GoogleMaps.dll などの Java ライブラリのバインドを提供する Xamarin.Android で提供されます。 ただし、すべての可能な Java ライブラリのバインドが指定されていない、用意されているバインドは、すべての Java 型およびメンバーはバインドできません。 バインドされていない Java 型およびメンバーを使用するには、Java ネイティブ インターフェイス (JNI) を使用できます。 この記事では、JNI を使用して、Java の型とメンバーの Xamarin.Android アプリケーションを操作する方法を説明します。
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: c674112f629f2054f81d72ee2b71268836e48b7a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106716"
---
# <a name="working-with-jni"></a>JNI の使用

_Xamarin.Android で Android アプリの作成を許可するC#Java の代わりにします。いくつかのアセンブリは、Mono.Android.dll Mono.Android.GoogleMaps.dll などの Java ライブラリのバインドを提供する Xamarin.Android で提供されます。ただし、すべての可能な Java ライブラリのバインドが指定されていない、用意されているバインドは、すべての Java 型およびメンバーはバインドできません。バインドされていない Java 型およびメンバーを使用するには、Java ネイティブ インターフェイス (JNI) を使用できます。この記事では、JNI を使用して、Java の型とメンバーの Xamarin.Android アプリケーションを操作する方法を説明します。_


## <a name="overview"></a>概要

これが常に不要または、管理呼び出し可能ラッパー (MCW) を呼び出す Java コードを作成することです。 多くの場合、「インライン」JNI が完全に許容可能および Java のバインドされていないメンバーの 1 回限り使用するために便利です。 多くの場合、全体の .jar のバインドを生成するよりも Java クラスで 1 つのメソッドを呼び出す JNI の使用が簡単です。

Xamarin.Android の提供、 `Mono.Android.dll` for Android のバインディングを提供するには、アセンブリ`android.jar`ライブラリ。 型とメンバー内で存在しない`Mono.Android.dll`内で型が存在しないと`android.jar`それらを手動でバインドされる場合があります。 使用する Java 型およびメンバーにバインドする、 **Java ネイティブ インターフェイス**(**JNI**) に型を検索、読み取りおよび書き込みフィールド、およびメソッドを呼び出します。

Xamarin.Android で JNI API は概念的に非常に似ています、 `System.Reflection` .NET での API: これにより、型を検索し、メソッド、および詳細を呼び出すメンバー名、フィールドの値を読み書き可能な。 JNI を使用して、`Android.Runtime.RegisterAttribute`オーバーライドをサポートするためにバインドできる仮想メソッドの宣言にカスタム属性。 インターフェイスをバインドするで実装できるようにC#します。

このドキュメントで説明します。

-  どの JNI は、型を参照します。
-  参照、読み取り、およびフィールドを記述する方法。
-  参照およびメソッドを呼び出す方法です。
-  マネージ コードからのオーバーライドを許可する仮想メソッドを公開する方法。
-  インターフェイスを公開する方法。



## <a name="requirements"></a>必要条件

JNI、を介して公開され、 [Android.Runtime.JNIEnv 名前空間](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)は Xamarin.Android のすべてのバージョンで使用できます。
Java 型およびインターフェイスをバインドするには、Xamarin.Android 4.0 またはそれ以降を使用する必要があります。


## <a name="managed-callable-wrappers"></a>マネージ呼び出し可能ラッパー

A**呼び出し可能ラッパーをマネージ**(**MCW**) は、*バインド*Java クラスまたはインターフェイスのクライアントがそのため、すべての JNI 機械をラップするC#コードする必要はありません基になる JNI の複雑さを心配します。 ほとんどの`Mono.Android.dll`マネージ呼び出し可能ラッパーで構成されます。

マネージ呼び出し可能ラッパーには、2 つの目的があります。

1.  JNI の使用をカプセル化できるように、クライアント コードを基になる複雑さについて知る必要はありません。
1.  サブクラスに Java 型と Java インターフェイスを実装することにします。

最初の目的は、コンシューマーがある使用するクラスのセットをシンプルで管理できるように、単に利便性と複雑さのカプセル化です。 これには、さまざまな使用が必要です[JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)メンバーがこの記事の後半で説明します。 呼び出し可能ラッパーを管理することに注意していないために必要な厳密に&ndash;「インライン」JNI の使用はまったくとはバインドされていない Java メンバーの 1 回限りの使用に適しています。 サブクラス化とインターフェイスの実装では、マネージ呼び出し可能ラッパーの使用が必要です。



## <a name="android-callable-wrappers"></a>Android 呼び出し可能ラッパー

Android ランタイム (アート) は、マネージ コードを呼び出す必要があるときは、android 呼び出し可能ラッパー (について) が必要です。アートの実行時にクラスを登録する方法がないために、これらのラッパーが必要です。
(具体的には、 [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI 関数は、Android ランタイムでサポートされていません。 Android 呼び出し可能ラッパーしたがってを構成するランタイム型の登録のサポートの不足している。)

Android のコードは、仮想実行またはインターフェイスのメソッドをオーバーライドするか、マネージ コードで実装する必要がある、ときに、Xamarin.Android では、このメソッドは、適切なマネージ型にディスパッチを取得できるように Java プロキシに提供します。 これらの Java プロキシ型は、同じコンス トラクターを実装して、オーバーライドされた基底クラスとインターフェイスのメソッドの宣言は、マネージ型として、「同じ」の基底クラスと Java インターフェイスの一覧を取得する Java コードです。

Android 呼び出し可能ラッパーがによって生成された、 **monodroid.exe**中のプログラム、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)、(直接または間接的に) を継承するすべての型に対して生成されると[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)します。



### <a name="implementing-interfaces"></a>インターフェイスの実装

Android のインターフェイスを実装する必要がある場合があります (など[Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/))。

すべての Android のクラスとインターフェイスを拡張して、 [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)インターフェイス。 したがって、すべての Android の種類を実装する必要があります`IJavaObject`します。
このファクトの Xamarin.Android を活用&ndash;を使用して`IJavaObject`を指定したマネージ型の Java プロキシ (、Android 呼び出し可能ラッパー) を使用した Android を提供します。 **Monodroid.exe**だけを検索`Java.Lang.Object`サブクラス (実装する必要があります`IJavaObject`)、サブクラス`Java.Lang.Object`マネージ コードでインターフェイスを実装する方法を示します。 例えば:

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

*この記事の残りの部分は、予告なく変更される可能性の実装の詳細を提供します。* (および、ここでは開発者が、内部で何が起こってについて興味がある可能性があるためにのみ)。

たとえば、次を指定C#ソース。

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

**Mandroid.exe**プログラムは、次の Android 呼び出し可能ラッパーを生成します。

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

基底クラスを保持すると、マネージ コード内でオーバーライドされる各メソッドのネイティブ メソッドの宣言が提供されていることを確認します。



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute

通常、Xamarin.Android を自動的に生成について; を構成する Java コードクラスは、Java クラスから派生し、既存の Java メソッドをオーバーライドするときに、この生成は、クラスとメソッドの名前に基づきます。 ただし、一部のシナリオでコード生成が十分でない、以下に示すとおりです。

-   Android のアクション名からレイアウトの XML 属性でのなどのサポート、 [android: onClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) XML 属性です。 指定すると、高めのインスタンスの表示は、Java メソッドを検索しようとします。

-   [Java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html)インターフェイス必要があります`readObject`と`writeObject`メソッド。 このインターフェイスのメンバーではないため、対応するマネージ実装は、Java コードをこれらのメソッドを公開しません。

-   [Android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/)インターフェイスは実装クラスは静的フィールドである必要があります`CREATOR`型の`Parcelable.Creator`します。 生成された Java コードでは、いくつかの明示的なフィールドが必要です。 ここでは、標準では、マネージ コードからの Java コードの出力フィールドに方法はありません。


Xamarin.Android の 4.2 以降のコード生成が任意の名前を持つ任意の Java メソッドを生成するソリューションを提供しないため、 [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)と[ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/)されました上記のシナリオにソリューションを提供するが導入されました。 両方の属性が内に存在、`Java.Interop`名前空間。

-   `ExportAttribute` &ndash; メソッド名とその予期される例外の型 (Java で明示的な「スロー」を提供) を指定します。 メソッドで使用されて、メソッドは「エクスポート」Java メソッドをマネージ メソッドに対応する JNI の呼び出しをディスパッチ コードを生成します。 これで使用できる`android:onClick`と`java.io.Serializable`します。

-   `ExportFieldAttribute` &ndash; フィールド名を指定します。 これは、フィールド初期化子として機能するメソッドに存在します。 これで使用できる`android.os.Parcelable`します。

[ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/)サンプル プロジェクトは、これらの属性を使用する方法を示しています。


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute のトラブルシューティング

-   パッケージ化の失敗がないため**Mono.Android.Export.dll** &ndash;を使用した場合`ExportAttribute`または`ExportFieldAttribute`追加しなければ、コードまたは依存するライブラリの一部のメソッドでは、 **Mono.Android.Export.dll**します。 このアセンブリは、Java からのコールバック コードをサポートするために分離されます。 別**Mono.Android.dll**ように、アプリケーションにその他のサイズを追加します。

-   リリースのビルドで`MissingMethodException`エクスポート メソッドの発生&ndash;でリリース ビルドで`MissingMethodException`エクスポート メソッドに対して発生します。 (この問題は、Xamarin.Android の最新バージョンで修正されます)。



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` `ExportFieldAttribute`その Java の実行時のコードで使用できる機能を提供します。 この実行時のコードでは、これらの属性によって生成された JNI メソッドからマネージ コードにアクセスします。 その結果、管理対象のメソッドにバインドします。 既存の Java メソッドはありません。そのため、Java のメソッドは、マネージ メソッドのシグネチャから生成されます。

ただし、この場合は、完全に決定ではありません。 最も顕著なは、これはマネージ型となどの Java 型間のいくつかの高度なマッピングに当てはまります。

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

など、これらの型がエクスポートされたメソッドは、必要な場合に、`ExportParameterAttribute`対応するパラメーターまたは戻り値の型を明示的に使用する必要があります。



### <a name="annotation-attribute"></a>Annotation 属性

Xamarin.Android の 4.2 に変換した`IAnnotation`実装の種類に属性 (System.Attribute) と Java のラッパーでの注釈の生成のサポートが追加されました。

これは、次の方向性のある変更を意味します。

-   バインディング ジェネレーターは、生成`Java.Lang.DeprecatedAttribute`から`java.Lang.Deprecated`(必要があるときに`[Obsolete]`マネージ コードで)。

-   既存わけではない`Java.Lang.Deprecated`クラスが表示されなくなります。 これらの Java ベース オブジェクトされる可能性がありますも通常の Java オブジェクトとして (このような使用状況が存在する) 場合。 ある`Deprecated`と`DeprecatedAttribute`クラス。

-   `Java.Lang.DeprecatedAttribute`クラスをマーク`[Annotation]`します。 これから継承されるカスタム属性がある場合に`[Annotation]`属性、そのカスタム属性の Java の注釈を msbuild タスクが生成されます (@Deprecated) Android 呼び出し可能ラッパー (について) にします。

-   注釈は、クラス、メソッドを生成できませんでしたし、フィールド (これメソッドは、マネージ コードでは) をエクスポートします。

(自体には、注釈設定済みのクラスまたはクラスの注釈付きのメンバーを含む) は、外側のクラスが登録されていない場合、全体の Java クラスのソースは生成されません、注釈を含みます。 方法については、指定することができます、`ExportAttribute`を明示的に生成され、注釈が付けられたメソッドを取得します。 また、Java の注釈のクラス定義を「生成」する機能ではありません。 つまり、特定の注釈の場合は、カスタム マネージ属性を定義する場合は、対応する Java 注釈クラスを含む別の .jar ライブラリを追加する必要があります。 注釈の種類を定義する Java ソース ファイルを追加することは十分ではありません。 同じ方法では機能しません、Java コンパイラ**apt**します。

さらに、次の制限が適用されます。

-   この変換プロセスを考慮しません`@Target`これまでに、注釈の種類の注釈。

-   属性をプロパティには機能しません。 代わりに、プロパティ get アクセス操作子または set アクセス操作子の属性を使用します。



## <a name="class-binding"></a>クラスのバインド

基になる Java 型の呼び出しを簡略化する場合は、マネージ呼び出し可能ラッパーを記述するというクラスをバインドします。

オーバーライドを許可するように、仮想および抽象メソッドをバインドC#Xamarin.Android 4.0 が必要です。 ただし、任意のバージョンの Xamarin.Android バインドできます非仮想メソッド、静的メソッド、または仮想メソッドのオーバーライドをサポートしません。

通常、バインディングには、次のものが含まれます。

-  A [JNI の処理にバインドされている Java 型](#_Looking_up_Java_Types)します。

-  [JNI フィールド Id とバインドされた各フィールドのプロパティを](#_Instance_Fields)します。

-  [JNI メソッド Id と各メソッドにメソッドがバインドされている](#_Instance_Methods)します。

-  型が必要をサブクラス化する必要がある場合、 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)カスタム属性を使用して型宣言で[RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/)に設定`true`。



### <a name="declaring-type-handle"></a>型ハンドルを宣言します。

フィールドとメソッドの参照方法には、その宣言する型を参照するオブジェクト参照が必要です。 慣例により、これに保持されている、`class_ref`フィールド。

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

参照してください、 [JNI 型参照](#_JNI_Type_References)に関するセクションの詳細については、`CLASS`トークンです。


### <a name="binding-fields"></a>フィールドをバインド

Java のフィールドとして公開されるC#Java フィールドなどのプロパティ、 [java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in)としてバインド、C#プロパティ[Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/)します。
さらに、JNI 静的フィールドおよびインスタンス フィールドを区別、ためさまざまな方法はプロパティを実装する場合を使用します。

フィールドのバインドには、3 つのメソッドのセットが含まれます。

1.  *フィールド id を取得する*メソッド。 *フィールド id を取得する*フィールド ハンドルを返すため、メソッドは、*フィールドの値を取得*と*フィールド値を設定*メソッドで使用します。 フィールド id を取得するには、フィールドの名前を入力します。 宣言を知ることが必要です、 [JNI 型シグネチャ](#_JNI_Type_Signatures)フィールド。

1.  *フィールドの値を取得*メソッド。 これらのメソッドは、フィールド ハンドルを必要とし、フィールドの値を Java から読み取りをでしょう。
    使用する方法は、フィールドの型に依存します。

1.  *フィールド値を設定*メソッド。 これらのメソッドは、フィールド ハンドルを必要とし、Java 内のフィールドの値の書き込みを担当します。 使用する方法は、フィールドの型に依存します。


 [静的フィールド](#_Static_Fields)を使用して、 [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)、 `JNIEnv.GetStatic*Field`、および[JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/)メソッド。

 [インスタンス フィールド](#_Instance_Fields)を使用して、 [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/)、 `JNIEnv.Get*Field`、および[JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/)メソッド。

たとえば、静的プロパティ`JavaSystem.In`として実装することができます。

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

メモ: 使用して[InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership))に JNI の参照を変換する、`System.IO.Stream`インスタンス、およびを使用している`JniHandleOwnership.TransferLocalRef`ため[JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/)ローカルの参照を返します。

多くは、 [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/)型が`FromJniHandle`目的の型には、変換、JNI のメソッドを参照します。



### <a name="method-binding"></a>メソッドのバインド

Java のメソッドとして公開C#メソッドとしてC#プロパティ。 たとえば、Java メソッド[java.lang.Runtime.runFinalizersOnExit](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean))メソッドとしてバインドされている、 [Java.Lang.Runtime.RunFinalizersOnExit](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/)メソッドと[java.lang.Object.getClass](http://developer.android.com/reference/java/lang/Object.html#getClass)メソッドとしてバインドされている、 [Java.Lang.Object.Class](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/)プロパティ。

メソッドの呼び出しでは、2 段階のプロセスを示します。

1.  *メソッド id を取得する*を呼び出すメソッド。 *メソッド id を取得する*メソッドがメソッドの呼び出しメソッドを使用するメソッドのハンドルが返されます。 メソッド id を取得するには、宣言、メソッドの名前を入力します。 を知ることが必要です、 [JNI 型シグネチャ](#_JNI_Type_Signatures)メソッドの。

1.  メソッドを呼び出します。

フィールドと同様には、メソッド id を取得し、メソッドの呼び出しに使用するメソッドは静的メソッドとインスタンス メソッドとは異なります。

[静的メソッド](#_Static_Methods_1)を使用して、 [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)メソッド id を検索して使用する、`JNIEnv.CallStatic*Method`メソッドの呼び出しのファミリです。

[インスタンス メソッド](#_Instance_Methods)使用[JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)メソッド id を検索して使用する、`JNIEnv.Call*Method`と`JNIEnv.CallNonvirtual*Method`メソッドの呼び出しのファミリです。

メソッドのバインドは、メソッドの呼び出しでは複数の可能性があるだけです。 メソッドのバインド。 も許可 (抽象と非 final メソッド) をオーバーライドするメソッドが含まれています。 または (インターフェイスのメソッド) の実装します。 [インターフェイスの継承のサポート、](#_Supporting_Inheritance,_Interfaces_1)仮想メソッドとインターフェイスのメソッドのサポートの複雑さについて説明します。

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>静的メソッド

使用して静的メソッドをバインディングでは、`JNIEnv.GetStaticMethodID`し、適切なを使用してメソッド ハンドルを取得する`JNIEnv.CallStatic*Method`メソッド、メソッドの戻り値の型によって異なります。 バインドの例を次に、 [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime())メソッド。

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

静的フィールド、メソッドのハンドルを格納しました注`id_getRuntime`します。 これは、メソッド ハンドルをすべての呼び出しで検索する必要があるように、パフォーマンスの最適化です。 この方法でメソッドのハンドルをキャッシュする必要はありません。 メソッドのハンドルを取得した後[JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/)メソッドを呼び出すために使用します。 `JNIEnv.CallStaticObjectMethod` 返します、`IntPtr`返された Java インスタンスのハンドルを格納します。
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr、JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java ハンドルを厳密に型指定されたオブジェクトのインスタンスに変換するために使用します。



#### <a name="non-virtual-instance-method-binding"></a>非仮想インスタンス メソッドのバインド

バインドを`final`インスタンス メソッド、またはインスタンス メソッドをオーバーライドするを必要としないのを使用して`JNIEnv.GetMethodID`し、適切なを使用してメソッド ハンドルを取得する`JNIEnv.Call*Method`メソッド、メソッドの戻り値の型によって異なります。 バインドの例を次に、`Object.Class`プロパティ。

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

静的フィールド、メソッドのハンドルを格納しました注`id_getClass`します。
これは、メソッド ハンドルをすべての呼び出しで検索する必要があるように、パフォーマンスの最適化です。 この方法でメソッドのハンドルをキャッシュする必要はありません。 メソッドのハンドルを取得した後[JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/)メソッドを呼び出すために使用します。 `JNIEnv.CallStaticObjectMethod` 返します、`IntPtr`返された Java インスタンスのハンドルを格納します。
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr、JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java ハンドルを厳密に型指定されたオブジェクトのインスタンスに変換するために使用します。


### <a name="binding-constructors"></a>バインディングのコンス トラクター

コンス トラクターは、名前の Java メソッド`"<init>"`します。 Java インスタンスのメソッドと同様に、`JNIEnv.GetMethodID`コンス トラクターのハンドルの参照に使用されます。 Java のメソッドとは異なり、 [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/)メソッドを使用して、コンス トラクターのメソッド ハンドルを起動します。 戻り値`JNIEnv.NewObject`JNI ローカル参照には。


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

クラスのバインドのサブクラスは通常[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)します。
サブクラス化する`Java.Lang.Object`、プレイに追加のセマンティックが:`Java.Lang.Object`インスタンス管理での Java インスタンスへの参照をグローバル、`Java.Lang.Object.Handle`プロパティ。

1.  `Java.Lang.Object`既定のコンス トラクターは、Java インスタンスを割り当てます。

1.  型がある場合、 `RegisterAttribute` 、および`RegisterAttribute.DoNotGenerateAcw`は`true`のインスタンス、`RegisterAttribute.Name`型は、既定のコンス トラクターによって作成されます。

1.  それ以外の場合、 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (について) に対応する`this.GetType`既定のコンス トラクターでインスタンス化されます。 Android 呼び出し可能ラッパーは、のパッケージの作成中に生成されるすべて`Java.Lang.Object`をサブクラス化`RegisterAttribute.DoNotGenerateAcw`に設定されていない`true`します。

これは、予期される型では、バインドいないクラス、セマンティック: インスタンス化する、 `Mono.Samples.HelloWorld.HelloAndroid` C#インスタンスは、Java で構築する必要があります`mono.samples.helloworld.HelloAndroid`インスタンス生成された Android 呼び出し可能ラッパーであります。

クラスのバインドのことが考えられます正しい動作場合は、Java 型には、既定のコンス トラクターが含まれています。 または、他のコンス トラクターを呼び出す必要はありません。 それ以外の場合、次の操作を実行するコンス トラクターを指定する必要があります。

1.  呼び出す、 [Java.Lang.Object (IntPtr、JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 、既定ではなく`Java.Lang.Object`コンス トラクター。 これは、新しい Java インスタンスの作成を回避するために必要です。

1.  値をチェック[Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) Java インスタンスを作成する前にします。 `Object.Handle`プロパティ以外の値を持ちます`IntPtr.Zero`Java コードで構築された、Android 呼び出し可能ラッパーと、Android 呼び出し可能ラッパーの作成のインスタンスを格納するクラスのバインドが構築されるかどうか。 Android を作成する場合など、`mono.samples.helloworld.HelloAndroid`インスタンス、最初、および Java、Android 呼び出し可能ラッパーが作成される`HelloAndroid`コンス トラクターでは、対応するインスタンスを作成します`Mono.Samples.HelloWorld.HelloAndroid`型で、`Object.Handle`されるプロパティ。コンス トラクターの実行前に Java インスタンスに設定します。

1.  型、対応する Android 呼び出し可能ラッパーのインスタンスを作成しを使用して、現在のランタイム型でない、宣言と同じ場合[Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership))によって返されるハンドルを格納する[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/)します。

1.  現在のランタイム型が宣言する型と同じ場合は、Java コンス トラクターを呼び出すし、使用[Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership))によって返されるハンドルを格納する`JNIEnv.NewInstance`します。


たとえば、 [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int))コンス トラクター。 これとしてをバインドされます。

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

[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/)メソッドを実行するヘルパーを`JNIEnv.FindClass`、 `JNIEnv.GetMethodID`、 `JNIEnv.NewObject`、および`JNIEnv.DeleteGlobalReference`から返される値で`JNIEnv.FindClass`します。 詳細については、次のセクションを参照してください。

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>インターフェイスの継承をサポートするには、

Java 型をサブクラス化や Java インターフェイスを実装する必要の生成[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) を生成したすべて`Java.Lang.Object`パッケージ化プロセス中にサブクラスです。 についての生成はによって制御されます、 [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)カスタム属性。

C#の種類、`[Register]`カスタム属性のコンス トラクターが 1 つの引数が必要です。 [JNI 型参照を簡略化](#_Simplified_Type_References_1)Java の対応する入力します。 これにより、Java の間で異なる名前を提供することとC#します。

Xamarin.Android 4.0 では、前に、`[Register]`カスタム属性が"alias"既存の Java 型で使用できません。 これは、ACWs について生成プロセスを生成するためすべて`Java.Lang.Object`サブクラスが発生しました。

Xamarin.Android 4.0 で導入された、 [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/)プロパティ。 このプロパティについて生成処理を指示する*スキップ*新しいマネージ呼び出し可能ラッパーのない ACWs パッケージの作成時に生成される原因となる宣言を許可する、注釈付きの型。 これにより、既存の Java 型をバインドできます。 たとえば、次の単純な Java クラス`Adder`、1 つのメソッドを含む`add`を整数に追加し、結果を返します。

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder`として型をバインドする可能性があります。

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

ここでは、 `Adder` C#型*エイリアス*、 `Adder` Java の型。 `[Register]`の JNI の名前を指定する属性を使用、 `mono.android.test.Adder` Java 型、および`DoNotGenerateAcw`について生成を阻止するためにプロパティを使用します。 これは、結果、用、についての生成、`ManagedAdder`型が正しくサブクラス、`mono.android.test.Adder`型。 場合、`RegisterAttribute.DoNotGenerateAcw`プロパティを使用していない、し、Xamarin.Android ビルド プロセスに生成される新しい`mono.android.test.Adder`Java の型。 として結果、コンパイル エラーになり、`mono.android.test.Adder`型は 2 回、2 つの個別のファイルで見られます。



### <a name="binding-virtual-methods"></a>仮想メソッドのバインド

`ManagedAdder` サブクラス Java`Adder`型であり、特に興味深いはありません: C# `Adder`型では、すべての仮想メソッドをように定義されていない`ManagedAdder`何もオーバーライドすることはできません。

バインド`virtual`サブクラスによってオーバーライドを許可する方法には、次の 2 つのカテゴリに分類を実行する必要があるいくつかの処理が必要があります。

1.  **メソッドのバインド**

1.  **メソッドの登録**


#### <a name="method-binding"></a>メソッドのバインド

メソッドのバインドを 2 つのサポート メンバーの追加が必要です、 C# `Adder`定義: `ThresholdType`、および`ThresholdClass`します。

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType`プロパティのバインドの現在の型を返します。

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` 仮想および非仮想メソッド ディスパッチを実行するかを判断するメソッドのバインドで使用されます。 常に返すことは、`System.Type`宣言に対応するインスタンスC#型。

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass`プロパティは、バインドされた型の JNI クラスの参照を返します。

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` 非仮想メソッドを呼び出すときに、メソッドのバインドに使用されます。

#### <a name="binding-implementation"></a>バインディングの実装

バインディングのメソッドの実装では、Java のメソッドの実行時の呼び出しを担当します。 含まれています、`[Register]`メソッドの登録の一部であるし、は、メソッドの登録のセクションで説明するカスタム属性宣言。

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

`id_add`フィールドには呼び出す Java メソッドのメソッドの ID が含まれています。 `id_add`値から取得されます`JNIEnv.GetMethodID`、クラスを宣言する必要があります (`class_ref`)、Java のメソッド名 (`"add"`)、およびメソッドの JNI シグネチャ (`"(II)I"`)。

メソッド ID を入手すると、`GetType`と比較する`ThresholdType`仮想または非仮想ディスパッチが必要なかどうかを判断します。 仮想ディスパッチが必要なときに`GetType`と一致する`ThresholdType`、として`Handle`メソッドをオーバーライドする Java に割り当てられたサブクラスを参照できます。

ときに`GetType`と一致しません`ThresholdType`、`Adder`サブクラス化されています (などによって`ManagedAdder`)、および`Adder.Add`実装は、サブクラスが呼び出された場合のみ呼び出すこと`base.Add`。 これは、非仮想ディスパッチ場合も、where`ThresholdClass`が用意されています。 `ThresholdClass` Java クラスは、呼び出すメソッドの実装を指定します。



#### <a name="method-registration"></a>メソッドの登録

更新があると仮定`ManagedAdder`オーバーライドの定義、`Adder.Add`メソッド。

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

いることを思い出してください`Adder.Add`いた、`[Register]`カスタム属性。

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]`カスタム属性のコンス トラクターは、3 つの値を受け取ります。

1.  Java メソッドの名前`"add"`ここでします。

1.  メソッドの JNI 型シグネチャ`"(II)I"`ここでします。

1.  *コネクタ メソッド*、`GetAddHandler`ここでします。
    コネクタの方法については、後ほど説明します。


最初の 2 つのパラメーターは、メソッドをオーバーライドするメソッドの宣言を生成するについて生成処理を許可します。 結果として得られるについては、次のコードの一部が含まれます。

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

なお、`@Override`メソッドにデリゲートが宣言されて、 `n_`-同じ名前のメソッドの先頭します。 確認の Java コードを呼び出すとき`ManagedAdder.add`、`ManagedAdder.n_add`が起動され、オーバーライドするようにするC#`ManagedAdder.Add`実行するメソッド。

したがって、最も重要な質問: どのは`ManagedAdder.n_add`フック`ManagedAdder.Add`でしょうか。

Java`native`を通じて (Android ランタイム) の Java ランタイムとメソッドが登録されている、 [JNI RegisterNatives 関数](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)します。
`RegisterNatives` これに続くを呼び出すには、Java のメソッド名、JNI の型シグネチャ、および関数ポインターを含む構造体の配列を受け取る[JNI の呼び出し規約](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)します。
関数ポインターは、メソッドのパラメーターを続けて 2 つのポインター引数を受け取る関数である必要があります。 Java`ManagedAdder.n_add`を次の C のプロトタイプを持つ関数でメソッドを実装する必要があります。

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android は公開しません、`RegisterNatives`メソッド。 代わりに、についてと、MCW まとめて情報提供を呼び出すために必要な`RegisterNatives`: についてには、メソッド名と JNI の型シグネチャが含まれていますにフックするための関数ポインターは、不足しているだけです。

これは、ような場合、*コネクタ メソッド*が用意されています。 3 番目`[Register]`カスタム属性のパラメーターは、登録済みの型またはパラメーターを受け取らずを返す登録済みの型の基底クラスで定義されたメソッドの名前、`System.Delegate`します。 返された`System.Delegate`さらに、正しい JNI 関数のシグネチャを持つメソッドを参照します。 最後に、コネクタ メソッドから返されるデリゲート*する必要があります*Java にデリゲートを提供すると、GC は、収集しないようにルートします。

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

`GetAddHandler`メソッドを作成、`Func<IntPtr, IntPtr, int, int,
int>`を参照するデリゲート、`n_Add`メソッドが呼び出され、 [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/)します。
`JNINativeWrapper.CreateDelegate` ハンドルされない例外が処理され、発生させるができるように、try/catch ブロックで指定されたメソッドをラップします、 [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/)イベント。 そのため、デリゲートが静的に格納されている`cb_add`GC は、デリゲートを解放しないように変数。

最後に、`n_Add`委任メソッドを呼び出すメソッドが、対応するマネージ型 JNI パラメーターをマーシャ リングを担当します。

注: を使用して、常に`JniHandleOwnership.DoNotTransfer`Java インスタンスをより細かく、MCW を取得するときにします。 ローカルの参照として扱うこと (したがってを呼び出すと`JNIEnv.DeleteLocalRef`) が中断されます管理 -&gt; Java -&gt;スタックの遷移を管理します。



### <a name="complete-adder-binding"></a>追加する操作子のバインドを完了します。

完全な管理のバインド、`mono.android.tests.Adder`型は。

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

次の条件に一致する型を記述する場合。

1.  サブクラス `Java.Lang.Object`

1.  `[Register]`カスタム属性

1.  `RegisterAttribute.DoNotGenerateAcw` は `true` です


GC の対話型の*は許可されません*を参照するすべてのフィールドを持つ、`Java.Lang.Object`または`Java.Lang.Object`時サブクラスです。 型のフィールドなど`System.Object`任意のインターフェイス型が許可されていません。 参照できない型`Java.Lang.Object`インスタンスが許可されるよう`System.String`と`List<int>`します。 この制限は、GC によって途中のオブジェクトのコレクションを防止します。

かどうか、型を参照できるインスタンス フィールドを含める必要があります、`Java.Lang.Object`インスタンス フィールドの型である必要があります`System.WeakReference`または`GCHandle`します。



## <a name="binding-abstract-methods"></a>抽象メソッドのバインド

バインド`abstract`メソッドは仮想メソッドのバインドとほぼ同じです。 2 つだけ違いがあります。

1.  抽象メソッドは抽象です。 これも引き続き、`[Register]`属性と関連付けられているメソッド登録では、メソッドのバインドだけに移動、`Invoker`型。

1.  以外の`abstract``Invoker`型のサブクラスが作成される抽象型。 `Invoker`型は、基底クラスで宣言されているすべての抽象メソッドをオーバーライドする必要がありますされ、オーバーライドされた実装がメソッドにバインド実装で、非仮想ディスパッチ大文字と小文字は無視できます。


たとえば、ある上記`mono.android.test.Adder.add`メソッドが`abstract`します。 C#バインドを変更できるように`Adder.Add`が抽象型、および新しい`AdderInvoker`実装されている型が定義されます`Adder.Add`:

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

`Invoker`型は、Java が作成したインスタンスに JNI の参照を取得する場合にのみ必要です。


## <a name="binding-interfaces"></a>バインド インターフェイス

バインド インターフェイスは仮想メソッドを含むクラスをバインドに概念的に似ていますが、微妙な (とそれほど小さくない) 方法の詳細の多くが異なります。 次を考慮[Java インターフェイス宣言](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

インターフェイスのバインドがある 2 つの部分:C#とインターフェイスの呼び出し元定義のインターフェイスの定義。



### <a name="interface-definition"></a>インターフェイス定義

C#インターフェイス定義は、次の要件を満たす必要があります。

-   インターフェイス定義する必要がありますが、`[Register]`カスタム属性。

-   インターフェイス定義を拡張する必要があります、`IJavaObject interface`します。
    これに失敗すると、ACWs が Java インターフェイスから継承できなくなります。

-   各インターフェイス メソッドを含める必要があります、`[Register]`属性に対応する Java のメソッド名、JNI の署名、およびコネクタ メソッドを指定します。

-   コネクタのメソッドでは、コネクタ メソッドにあることができます種類も指定する必要があります。

バインドするときに`abstract`と`virtual`コネクタ メソッドのメソッドを登録されている型の継承階層内で検索します。 インターフェイスを持たないため、これができないため、要件、本文を含むメソッド コネクタ メソッドの場所を示す、型を指定します。 コロンの後に、コネクタのメソッド文字列内で型が指定されて`':'`、呼び出し元を含む型のアセンブリ修飾型名を指定する必要があります。

インターフェイス メソッド宣言は、対応するメソッドを使用して Java の翻訳を*互換性のある*型。 Java 組み込み型では、互換性のある型は、対応するC#型は、Java など`int`はC#`int`します。 参照型では、互換性のある型は、適切な Java の型の JNI のハンドルを提供する型です。

Java でインターフェイスのメンバーが直接呼び出されません&ndash;呼び出しは、呼び出し元の型によって媒介は&ndash;ある程度の柔軟性が許可されます。

Java の進行状況インターフェイスは、[で宣言されているC#として](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Java を割り当てることで、上記の通知`int[]`パラメーターを[JavaArray&lt;int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/)します。
これは必要ありません: でしたバインドしたことをC# `int[]`、または`IList<int>`、またはその他全体。 どのような型を選択すると、 `Invoker` Java に変換できる必要があります`int[]`呼び出しの種類。


### <a name="invoker-definition"></a>呼び出し元の定義

`Invoker`型定義を継承する必要があります`Java.Lang.Object`、適切なインターフェイスを実装し、インターフェイス定義で参照されるすべての接続方法を提供します。 クラスのバインドとは異なる複数の候補を 1 つがある:`class_ref`フィールドとメソッド Id が静的でないメンバーのインスタンス メンバーにする必要があります。

インスタンス メンバーを優先するのには、その理由と`JNIEnv.GetMethodID`Android ランタイムで動作します。 (Java の動作も可能性があります。 これはテストされて)。`JNIEnv.GetMethodID`実装されたインターフェイスと宣言されたインターフェイスではなくに由来するメソッドを検索する場合は null を返します。 検討してください、 [java.util.SortedMap&lt;K, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) Java インターフェイスを実装する、 [java.util.Map&lt;K, V&gt; ](http://developer.android.com/reference/java/util/Map.html)インターフェイス。 マップを提供します、[オフ](http://developer.android.com/reference/java/util/Map.html#clear())メソッド、つまり、一見合理的な`Invoker`SortedMap の定義になります。

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

上記ために失敗します`JNIEnv.GetMethodID`戻ります`null`の検索時に、`Map.clear`メソッドによって、`SortedMap`クラスのインスタンス。

この 2 つのソリューション: どのインターフェイスがすべてのメソッドの取得元を追跡して、`class_ref`ごとにインターフェイス、またはすべてのインスタンス メンバーとしておよびインターフェイス型ではなく、最も多く派生されたクラス タイプで、メソッド ルックアップを実行します。 後者が行われる**Mono.Android.dll**します。

呼び出し元の定義が 6 つのセクション: コンス トラクター、`Dispose`メソッド、`ThresholdType`と`ThresholdClass`、メンバー、`GetObject`メソッド、インターフェイス メソッドの実装、およびコネクタ メソッドの実装。



#### <a name="constructor"></a>コンストラクター

呼び出したインスタンスのランタイム クラスを参照し、インスタンスにランタイム クラスを保存する必要があるコンス トラクター`class_ref`フィールド。

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

注:`Handle`プロパティは、コンス トラクターの本文内で使用する必要がありますいないと、`handle`パラメーター、Android v4.0 で、`handle`基底コンス トラクターは、実行が完了した後にパラメーターが無効になる可能性があります。


#### <a name="dispose-method"></a>Dispose メソッド

`Dispose`メソッドがコンス トラクターで割り当てられたグローバル参照を解放する必要があります。

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


#### <a name="thresholdtype-and-thresholdclass"></a>開始と ThresholdClass

`ThresholdType`と`ThresholdClass`メンバーはクラスのバインドで確認された内容と同じです。

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

静的な`GetObject`メソッドがサポートするために必要な[Extensions.JavaCast&lt;T&gt;()](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>インターフェイス メソッド

インターフェイスのすべてのメソッドは、JNI を対応する Java メソッドを呼び出しますが、実装する必要があります。

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

コネクタのメソッドとサポート インフラストラクチャが適切な JNI のパラメーターをマーシャ リングを担当C#型。 Java`int[]`パラメーターは、JNI として渡される`jintArray`、これは、`IntPtr`内C#。 `IntPtr`にマーシャ リングする必要があります、`JavaArray<int>`呼び出しをサポートするために、C#インターフェイス。

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

場合`int[]`優先なります`JavaList<int>`、し[JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type))代わりに使用される可能性があります。

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

ただし、注意を`JNIEnv.GetArray`のため、大きな配列をこの結果、多数の追加の GC の負荷の Vm 間での配列全体をコピーします。


### <a name="complete-invoker-definition"></a>呼び出し元の定義が完了します。

[IAdderProgressInvoker 定義が完了](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

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

多くの JNIEnv メソッドが返す*JNI* *オブジェクト参照*に似ている`GCHandle`s。 JNI はオブジェクト参照の 3 つの種類を提供します。 ローカルの参照、グローバル参照、およびグローバルの弱い参照。 3 つすべてとして表される`System.IntPtr`、*が*(JNI 関数型セクション) に従ってすべて`IntPtr`から返される`JNIEnv`メソッドは、参照します。 たとえば、 [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)を返します、 `IntPtr`、オブジェクト参照が返されないが返されますが、`jmethodID`します。 参照してください、 [JNI 関数のドキュメント](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)詳細についてはします。

ローカル参照がによって作成された*ほとんど*メソッドの参照を作成します。
Android は、通常は 512 任意の特定時点で存在するローカルの参照の数に制限を許可するだけです。 使用してローカルの参照を削除することができます[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/)します。
戻り値のオブジェクト参照がローカル参照を返す JNIEnv メソッドを参照する JNI とは異なりすべて[JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/)を返します、*グローバル*参照。 場合によって構築することによってすばやくローカル参照を削除することを強くお勧め、 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)オブジェクトの周囲を指定して`JniHandleOwnership.TransferLocalRef`を[(IntPtr Java.Lang.ObjectJniHandleOwnership の転送を処理)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)コンス トラクター。

グローバル参照がによって作成された[JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/)と[JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/)します。
破棄できるよう、 [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/)します。
エミュレーターでは、ハードウェア デバイスが約 52,000 グローバル参照の制限に 2,000 の未解決のグローバル参照の制限があります。

グローバルの弱い参照は、Android バージョン 2.2 (Froyo) 以降にのみ使用できます。 グローバルの弱い参照を削除できる[JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr))します。


### <a name="dealing-with-jni-local-references"></a>JNI ローカル参照を処理します。

[JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/)、 [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/)、 [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/)、 [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)と[JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/)メソッドを返す、 `IntPtr` 、Java オブジェクトへの JNI ローカル参照が含まれていますまたは`IntPtr.Zero`Java が返された場合`null`します。 制限 (512 エントリ) が参照することを確認することが望ましいで未処理になる可能性のあるローカル参照の数によっては、適切なタイミングで削除されます。 ローカル参照に対処することが 3 つの方法があります。 明示的に削除するには、からの作成、`Java.Lang.Object`インスタンスを使用すると、それらを保持するために`Java.Lang.Object.GetObject<T>()`周囲にマネージ呼び出し可能ラッパーを作成します。



### <a name="explicitly-deleting-local-references"></a>ローカル参照を明示的に削除します。

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/)ローカル参照を削除するために使用します。 ローカルの参照が削除されると、使用できませんなくなった場合に、注意が必要ことを確認するように`JNIEnv.DeleteLocalRef`が最後に参照をローカルで実行します。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```



### <a name="wrapping-with-javalangobject"></a>Java.Lang.Object での折り返し

`Java.Lang.Object` 提供、 [Java.Lang.Object (IntPtr ハンドル、JniHandleOwnership 転送)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)コンス トラクターを既存の JNI の参照をラップするために使用できます。 [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/)パラメーターを指定する方法、`IntPtr`パラメーターを扱う必要があります。

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; 、作成した`Java.Lang.Object`から新しいグローバル参照を作成するインスタンス、`handle`パラメーター、および`handle`は変更されません。
    呼び出し元が解放する役割を担います`handle`、必要な場合。

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; 、作成した`Java.Lang.Object`インスタンスから新しいグローバル参照が作成されます、`handle`パラメーター、および`handle`で削除が[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . 呼び出し元を解放する必要があります`handle`、および使用する必要がありますいない`handle`コンス トラクターは、実行が終了したら。

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; 、作成した`Java.Lang.Object`インスタンスの所有権を引き継ぎ、`handle`パラメーター。 呼び出し元を解放する必要があります`handle`します。


JNI メソッドの呼び出しメソッドがローカルの refs を返すため`JniHandleOwnership.TransferLocalRef`は、通常使用します。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

作成したグローバル参照はまで解放されませんが、`Java.Lang.Object`インスタンスがガベージ コレクションします。 できない場合は、ガベージ コレクションの高速化、グローバルの参照を解放インスタンスの破棄します。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Java.Lang.Object.GetObject を使用して&lt;T&gt;)

`Java.Lang.Object` 提供、 [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr ハンドル、JniHandleOwnership 転送)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)メソッドを指定した型のマネージ呼び出し可能ラッパーを作成するために使用できます。

型`T`次の要件を満たす必要があります。

1.  `T` 参照型である必要があります。

1.  `T` 実装する必要があります、`IJavaObject`インターフェイス。

1.  場合`T`は抽象クラスまたはインターフェイスを`T`パラメーターの型でコンス トラクターを提供する必要があります`(IntPtr,
    JniHandleOwnership)`します。

1.  場合`T`は抽象クラスまたはインターフェイスである*する必要があります*する、*呼び出し元*の使用可能な`T`します。 呼び出し元が継承した非抽象型`T`実装または`T`、として、同じ名前を持つ`T`呼び出し元のサフィックスを持つ。 たとえば、インターフェイス、T は`Java.Lang.IRunnable`、型`Java.Lang.IRunnableInvoker`が存在し、必要なを含める必要があります`(IntPtr,
    JniHandleOwnership)`コンス トラクター。


JNI メソッドの呼び出しメソッドがローカルの refs を返すため`JniHandleOwnership.TransferLocalRef`は、通常使用します。

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java 型を調べる

フィールドまたは JNI のメソッドを検索するには、フィールドまたはメソッドの宣言型は、最初に参照する必要があります。 [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) Java 型を検索するメソッドを使用します。 文字列パラメーターが、*型参照を簡略化*または*完全な型参照*の Java の型。 参照してください、 [JNI 型の参照セクション](#_JNI_Type_References)詳細については、簡素化され、完全な型参照。

注: 異なり他のすべての`JNIEnv`オブジェクトのインスタンスを返すメソッド`FindClass`ローカル参照ではなく、グローバルの参照を返します。

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>インスタンス フィールド

フィールドを操作する*フィールド Id*します。 フィールド Id が経由で取得した[JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/)、クラスが必要ですが、フィールドは、フィールドの名前で定義されている、 [JNI 型シグネチャ](#JNI_Type_Signatures)フィールドの。

フィールド Id は、解放する必要はありませんが、対応する Java 型が読み込まれる限り有効とします。 (Android は現在サポートしていませんクラスをアンロードします。)

インスタンス フィールドを操作するメソッドの 2 つのセットがあります: 1 つのインスタンス フィールドと 1 つのインスタンス フィールドを記述するための読み取り。 すべてのメソッドのセットには、読み取りまたは書き込みフィールドの値をフィールド ID が必要です。


### <a name="reading-instance-field-values"></a>インスタンス フィールドの値の読み取り

インスタンス フィールドの値を読み取るためのメソッドのセットでは、名前付けパターンに従います。

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
場所`*`フィールドの種類です。

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash;など、組み込み型のない任意のインスタンス フィールドの値を読み取る`java.lang.Object`配列、およびインターフェイスの型。 返される値は、JNI ローカル参照です。

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash;の値を読み取る`bool`インスタンス フィールドです。

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash;の値を読み取る`sbyte`インスタンス フィールドです。

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash;の値を読み取る`char`インスタンス フィールドです。

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash;の値を読み取る`short`インスタンス フィールドです。

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash;の値を読み取る`int`インスタンス フィールドです。

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash;の値を読み取る`long`インスタンス フィールドです。

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash;の値を読み取る`float`インスタンス フィールドです。

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash;の値を読み取る`double`インスタンス フィールドです。





### <a name="writing-instance-field-values"></a>インスタンス フィールドの値を書き込んでいます

インスタンス フィールドの値を書き込むためのメソッドのセットでは、名前付けパターンに従います。

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

場所*型*フィールドの種類です。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash;など、組み込み型のない任意のフィールドの値を書き込む`java.lang.Object`配列、およびインターフェイスの型。 `IntPtr` JNI ローカル参照、JNI グローバル参照、JNI の弱いグローバル参照、値がありますまたは`IntPtr.Zero`(の`null`)。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash;の値を書き込む`bool`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash;の値を書き込む`sbyte`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash;の値を書き込む`char`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash;の値を書き込む`short`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash;の値を書き込む`int`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash;の値を書き込む`long`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash;の値を書き込む`float`インスタンス フィールドです。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash;の値を書き込む`double`インスタンス フィールドです。


<a name="_Static_Fields" />

## <a name="static-fields"></a>静的フィールド

静的フィールドを操作する*フィールド Id*します。 フィールド Id が経由で取得した[JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/)、クラスが必要ですが、フィールドは、フィールドの名前で定義されている、 [JNI 型シグネチャ](#JNI_Type_Signatures)フィールドの。

フィールド Id は、解放する必要はありませんが、対応する Java 型が読み込まれる限り有効とします。 (Android は現在サポートしていませんクラスをアンロードします。)

静的フィールドを操作するメソッドの 2 つのセットがあります: 1 つのインスタンス フィールドと 1 つのインスタンス フィールドを記述するための読み取り。 すべてのメソッドのセットには、読み取りまたは書き込みフィールドの値をフィールド ID が必要です。


### <a name="reading-static-field-values"></a>静的フィールドの値の読み取り

静的フィールドの値を読み取るためのメソッドのセットでは、名前付けパターンに従います。

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

場所`*`フィールドの種類です。

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash;など、組み込み型のない任意の静的フィールドの値を読み取る`java.lang.Object`配列、およびインターフェイスの型。 返される値は、JNI ローカル参照です。

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash;の値を読み取る`bool`静的フィールド。

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash;の値を読み取る`sbyte`静的フィールド。

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash;の値を読み取る`char`静的フィールド。

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash;の値を読み取る`short`静的フィールド。

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash;の値を読み取る`long`静的フィールド。

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash;の値を読み取る`float`静的フィールド。

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash;の値を読み取る`double`静的フィールド。



### <a name="writing-static-field-values"></a>静的フィールド値を書き込んでいます

静的フィールドの値を書き込むためのメソッドのセットでは、名前付けパターンに従います。

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

場所*型*フィールドの種類です。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash;など、組み込み型のない任意の静的フィールドの値を書き込む`java.lang.Object`配列、およびインターフェイスの型。 `IntPtr` JNI ローカル参照、JNI グローバル参照、JNI の弱いグローバル参照、値がありますまたは`IntPtr.Zero`(の`null`)。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash;の値を書き込む`bool`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash;の値を書き込む`sbyte`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash;の値を書き込む`char`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash;の値を書き込む`short`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash;の値を書き込む`int`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash;の値を書き込む`long`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash;の値を書き込む`float`静的フィールド。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash;の値を書き込む`double`静的フィールド。


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>インスタンス メソッド

を介してインスタンス メソッドが呼び出される*メソッド Id*します。 メソッドの Id が経由で取得した[JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)、種類がありますが、メソッドは、メソッドの名前で定義されていると、 [JNI 型シグネチャ](#JNI_Type_Signatures)メソッドの。

メソッドの Id は、解放する必要はありませんが、対応する Java 型が読み込まれる限り有効とします。 (Android は現在サポートしていませんクラスをアンロードします。)

メソッドを呼び出すためのメソッドの 2 つのセットがあります。 事実上、メソッドの呼び出しに 1 つと非仮想的メソッドを呼び出すための 1 つ。 メソッドの両方のセットが、メソッドの呼び出しにメソッド ID を必要とし、また非仮想呼び出しではどのクラスの実装を呼び出す必要がありますを指定することが必要です。

インターフェイスのメソッドのみ検索できますで宣言する型。拡張または継承されたインターフェイスに由来するメソッドを検索することはできません。 後でバインド インターフェイスを参照してください]、[呼び出し元の実装は、詳細セクションします。

任意のメソッドは、クラスで宣言または任意の基底クラスまたはインターフェイスの実装検索できます。


### <a name="virtual-method-invocation"></a>仮想メソッドの呼び出し

メソッドを呼び出してメソッドのセットには、名前付けのパターンが実質的に従います。

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

場所`*`はメソッドの戻り値の型。

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash;など、非組み込み型を返すメソッドを呼び出す`java.lang.Object`、配列、およびインターフェイスします。 返される値は、JNI ローカル参照です。

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash;を返すメソッドを呼び出す、`bool`値。

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash;を返すメソッドを呼び出す、`sbyte`値。

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash;を返すメソッドを呼び出す、`char`値。

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash;を返すメソッドを呼び出す、`short`値。

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash;を返すメソッドを呼び出す、`long`値。

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash;を返すメソッドを呼び出す、`float`値。

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash;を返すメソッドを呼び出す、`double`値。



### <a name="non-virtual-method-invocation"></a>非仮想メソッドの呼び出し

メソッドを呼び出してメソッドのセットには、名前付けパターン非仮想的に従います。

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

場所`*`はメソッドの戻り値の型。 非仮想メソッドの呼び出しは通常、仮想メソッドの基本メソッドの呼び出しに使用されます。

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash;非仮想的など、非組み込み型を返すメソッドを呼び出す`java.lang.Object`、配列、およびインターフェイスします。 返される値は、JNI ローカル参照です。

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`bool`値。

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`sbyte`値。

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`char`値。

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`short`値。

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`long`値。

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`float`値。

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash;非仮想的を返すメソッドを呼び出し、`double`値。


<a name="_Static_Methods" />

## <a name="static-methods"></a>静的メソッド

を介して静的メソッドが呼び出される*メソッド Id*します。 メソッドの Id が経由で取得した[JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)、種類がありますが、メソッドは、メソッドの名前で定義されていると、 [JNI 型シグネチャ](#JNI_Type_Signatures)メソッドの。

メソッドの Id は、解放する必要はありませんが、対応する Java 型が読み込まれる限り有効とします。 (Android は現在サポートしていませんクラスをアンロードします。)



### <a name="static-method-invocation"></a>静的メソッドの呼び出し

メソッドを呼び出してメソッドのセットには、名前付けのパターンが実質的に従います。

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

場所`*`はメソッドの戻り値の型。

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash;など、非組み込み型を返す静的メソッドを呼び出す`java.lang.Object`、配列、およびインターフェイスします。 返される値は、JNI ローカル参照です。

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash;を返す静的メソッドを呼び出す、`bool`値。

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash;を返す静的メソッドを呼び出す、`sbyte`値。

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash;を返す静的メソッドを呼び出す、`char`値。

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash;を返す静的メソッドを呼び出す、`short`値。

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash;を返す静的メソッドを呼び出す、`long`値。

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash;を返す静的メソッドを呼び出す、`float`値。

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash;を返す静的メソッドを呼び出す、`double`値。


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI 型シグネチャ

[JNI 型シグネチャ](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432)は[JNI 型参照](#_JNI_Type_References)(型がない単純な参照)、メソッドは除きます。 メソッド、JNI の型シグネチャが始めかっこ`'('`すべて終わりかっこの入力後に連結して 1 つ (区切りコンマまたはその他) で、型パラメーターの型の参照と、その後`')'`、メソッドの戻り値の型の JNI 型参照を続きます。

たとえば、Java メソッドを考えてみます。

```java
long f(int n, String s, int[] array);
```

JNI の型のシグネチャは次のようになります。

```csharp
(ILjava/lang/String;[I)J
```

一般に、*強く*を使用することが推奨、 `javap` JNI の署名を確認するコマンド。 JNI 型シグネチャなど、 [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String))メソッドは、"(Ljava/lang/文字列;) Ljava/lang/スレッド$ 状態"、JNI のシグネチャを入力するときに、 [java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values)メソッドは、"() [Ljava/lang/スレッド$ 状態;"します。 末尾セミコロン; の注意します。これら*は*JNI の型シグネチャの一部です。

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI 型の参照

JNI 型の参照は Java 型参照です。 などの完全修飾型名の Java を使用できません`java.lang.String`JNI、JNI のバリエーションを代わりに使用する必要があります`"java/lang/String"`または`"Ljava/lang/String;"`、コンテキストに応じてには詳細については後述します。
JNI 型参照の 4 つの種類があります。

-  **built-in**
-  **simplified**
-  **type**
-  **array**


### <a name="built-in-type-references"></a>組み込み型の参照

組み込み型の参照は、単一の文字、組み込みの値の型を参照するために使用します。 マッピングは次のとおりです。

-  `"B"` `sbyte`します。
-  `"S"` `short`します。
-  `"I"` `int`します。
-  `"J"` `long`します。
-  `"F"` `float`します。
-  `"D"` `double`します。
-  `"C"` `char`します。
-  `"Z"` `bool`します。
-  `"V"` `void`メソッドが型を返します。


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>簡略化された型の参照

簡略化された型の参照でのみ使用できます[JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String))します。
参照を簡略化された型を派生する 2 つの方法はあります。

1.  Java を完全修飾名に置き換えますすべて`'.'`パッケージ名と型名を前に`'/'`、毎回`'.'`内での型名を`'$'`します。

1.  出力を読み取る`'unzip -l android.jar | grep JavaName'`します。


Java の型と、2 つのいずれかが[java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html)簡略化された型参照にマップされる`java/lang/Thread$State`します。


### <a name="type-references"></a>型の参照

型参照が組み込み型参照または簡略化された型参照を`'L'`プレフィックスと`';'`サフィックス。 Java の型の[java.lang.String](http://developer.android.com/reference/java/lang/String.html)、簡略化された型の参照が`"java/lang/String"`は、型参照`"Ljava/lang/String;"`します。

型の参照は、配列型の参照と JNI の署名に使用されます。

出力を読み取ることによって、補助的な方法は、型参照を取得するは`'javap -s -classpath android.jar fully.qualified.Java.Name'`します。
種類に応じて、関連することができますを使用するコンス トラクターの宣言またはメソッド JNI の名前を決定する型を返します。 例えば:

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

`Thread.State` Java の列挙型では、シグネチャを使ってできるように、`valueOf`型参照が $ Ljava/lang/スレッドの状態を調べます。



### <a name="array-type-references"></a>配列型の参照

配列型の参照が`'['`JNI の型参照にプレフィックスとして付加されます。
配列を指定するときに、簡略化された型の参照を使用できません。

たとえば、`int[]`は`"[I"`、`int[][]`は`"[[I"`と`java.lang.Object[]`は`"[Ljava/lang/Object;"`します。



## <a name="java-generics-and-type-erasure"></a>Java ジェネリックと型の消去

*ほとんど*JNI、Java のジェネリックを使用して表示される、時間の*存在しない*します。
いくつか「しわ、」があるが、これらしわは Java が JNI が方法を検索し、呼び出すジェネリック メンバーではなく、ジェネリックをやり取りする方法。

JNI を操作するときに、ジェネリック型またはメンバーと非ジェネリック型またはメンバーの間の違いはありません。 たとえば、ジェネリック型[java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) 「生」のジェネリック型ではまた`java.lang.Class`、どちらも同じの型は単純な参照がある`"java/lang/Class"`します。


## <a name="java-native-interface-support"></a>Java ネイティブ インターフェイスのサポート

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) Jave ネイティブ インターフェイス (JNI) のマネージ ラッパーは、します。 JNI 関数内で宣言、 [Java ネイティブ インターフェイス仕様](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)メソッドは、明示的な削除に変更されている場合、`JNIEnv*`パラメーターと`IntPtr`の代わりに使用が`jobject`、 `jclass`、`jmethodID`など。たとえば、 [JNI NewObject 関数](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

公開されると、 [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/)メソッド。

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

2 つの呼び出しの間の変換は、かなり簡単です。 C では、次の必要があります。

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C#同等になります。

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr に保持されている Java オブジェクト インスタンスを作成したら、おそらくたい操作を行います。 など JNIEnv メソッドを使用することができます[ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/)表示するには、類似性は既に場合C#してラッパーが JNI の参照にラッパーを作成する必要あります。 を介してその行うことができます、 [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/)拡張メソッド。

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

使用することも、 [Java.Lang.Object.GetObject <t>()</t> ](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)メソッド。

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

さらに、すべての JNI 関数によって変更されているを削除する、`JNIEnv*`パラメーター JNI のすべての関数内に存在します。


## <a name="summary"></a>まとめ

JNI を直接扱うは、いかなる代償に避ける必要がある悲惨なエクスペリエンスです。 残念ながら、これは常に回避できません。うまくいけばこのガイドは、Android の Mono とバインドされていない Java ケースに達した場合サポートが提供されます。


## <a name="related-links"></a>関連リンク

- [SanityTests (サンプル)](https://developer.xamarin.com/samples/SanityTests/)
- [Java ネイティブ インターフェイスの仕様](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java ネイティブ インターフェイス関数](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
