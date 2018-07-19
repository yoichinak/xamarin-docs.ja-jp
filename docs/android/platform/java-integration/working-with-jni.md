---
title: JNI の操作
description: Xamarin.Android では、Java ではなく c# 内で Android アプリの作成を許可します。 いくつかのアセンブリが付属して Xamarin.Android Mono.Android.dll や Mono.Android.GoogleMaps.dll など、Java、ライブラリのバインドを提供します。 ただし、考えられるあらゆる Java ライブラリのバインドが指定されていないと、すべての Java 型およびメンバーが提供するバインディングはバインドできません。 バインドされていない Java 型およびメンバーを使用して、Java ネイティブ インターフェイス (JNI) を使用することがあります。 この記事は、JNI を使用して、Java の型および Xamarin.Android アプリケーションからのメンバーと対話する方法を示しています。
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 4b5874a0f0e4289201f68299e2e37660cabc9ecf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774175"
---
# <a name="working-with-jni"></a>JNI の操作

_Xamarin.Android では、Java ではなく c# 内で Android アプリの作成を許可します。いくつかのアセンブリが付属して Xamarin.Android Mono.Android.dll や Mono.Android.GoogleMaps.dll など、Java、ライブラリのバインドを提供します。ただし、考えられるあらゆる Java ライブラリのバインドが指定されていないと、すべての Java 型およびメンバーが提供するバインディングはバインドできません。バインドされていない Java 型およびメンバーを使用して、Java ネイティブ インターフェイス (JNI) を使用することがあります。この記事は、JNI を使用して、Java の型および Xamarin.Android アプリケーションからのメンバーと対話する方法を示しています。_


## <a name="overview"></a>概要

これが常に不要または、マネージ呼び出し可能ラッパー (MCW) Java コードを呼び出すために作成することです。 多くの場合、「インライン」JNI が完全に許容されますが、バインドされていない Java メンバーの 1 回限り使用するために役立ちます。 簡単 JNI を使用して生成全体の .jar バインドよりも Java クラスの 1 つのメソッドを呼び出すには多くの場合です。

Xamarin.Android 提供、 `Mono.Android.dll` for Android のバインドのバインドを提供するアセンブリ`android.jar`ライブラリです。 型およびメンバーではない内`Mono.Android.dll`内で存在しない型と`android.jar`それらを手動でバインドすることによって使用される可能性があります。 使用する Java 型およびメンバーをバインドする、 **Java ネイティブ インターフェイス**(**JNI**) 型を検索、読み取りおよび書き込み、フィールドおよびメソッドを呼び出すことにします。

Xamarin.Android で JNI API は、概念的に非常に似ています、 `System.Reflection` .net API: これにより、可能な型を検索し、メンバー、名前で読み書き可能なフィールドの値、呼び出すメソッド、および詳細。 JNI を使用して、`Android.Runtime.RegisterAttribute`のオーバーライドをサポートするためにバインドできる仮想メソッドの宣言にカスタム属性です。 C# で実装できるように、インターフェイスをバインドすることができます。

このドキュメントについて説明します。

-  どの JNI は、型を参照します。
-  検索、読み取り、およびフィールドを記述する方法。
-  検索し、メソッドを呼び出す方法です。
-  マネージ コードからのオーバーライドを許可する仮想メソッドを公開する方法です。
-  インターフェイスを公開する方法です。



## <a name="requirements"></a>要件

JNI、を通じて公開されていると、 [Android.Runtime.JNIEnv 名前空間](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)は Xamarin.Android のすべてのバージョンで使用できます。
Java 型およびインターフェイスをバインドするには、Xamarin.Android 4.0 またはそれ以降を使用する必要があります。


## <a name="managed-callable-wrappers"></a>マネージ呼び出し可能ラッパー

A**呼び出し可能ラッパーをマネージ**(**MCW**) は、*バインディング*Java クラスまたは c# クライアント コードがについて心配する必要があるように、すべての JNI 機械がラップされるインターフェイスJNI の基になる複雑度。 ほとんどの`Mono.Android.dll`マネージ呼び出し可能ラッパーから成ります。

マネージ呼び出し可能ラッパーには、次の 2 つの目的があります。

1.  クライアント コードが基になる複雑さを把握する必要があるように、JNI 使用をカプセル化します。
1.  サブクラスに持つ Java の型と Java インターフェイスを実装を可能にします。

最初の目的は、コンシューマーがあるシンプルで管理されている一連のクラスを使用するように、単に利便性と複雑さをカプセル化です。 さまざまな使用する必要があります[JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)メンバーこの記事の後半で説明します。 呼び出し可能ラッパーをマネージことに注意してくださいは必ずしも必要&ndash;JNI を使用して「インライン」が完全に許容されると、バインドされていない Java メンバーの 1 回限り使用するために便利です。 サブクラスとインターフェイスの実装には、マネージ呼び出し可能ラッパーの使用が必要です。



## <a name="android-callable-wrappers"></a>Android の呼び出し可能ラッパー

Android の実行時 (アート) は、マネージ コードを呼び出す必要があるときは、android 呼び出し可能ラッパー (について) が必要アートの実行時にクラスを登録する方法がないために、これらのラッパーが必要です。
(具体的には、 [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI 関数は、Android のランタイムによってサポートされていません。 Android 呼び出し可能ラッパーしたがってを占めるランタイム型の登録のサポートの不足。)

Android のコードは、バーチャル マシンを実行またはインターフェイスのメソッドをオーバーライドするか、マネージ コードで実装する必要があります、限り Xamarin.Android は、このメソッドは、適切なマネージ型にディスパッチを取得できるように Java プロキシを提供する必要があります。 これらの Java プロキシ型は、同じコンス トラクターを実装して、オーバーライドされた基底クラスとインターフェイスのメソッドの宣言は、マネージ型として、「同じ」基底クラスと Java インターフェイスのリストを持つの Java コードです。

Android の呼び出し可能ラッパーがによって生成される、 **monodroid.exe**中のプログラム、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)、(直接または間接的に) を継承するすべての型に対して生成されると[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)です。



### <a name="implementing-interfaces"></a>インターフェイスの実装

Android のインターフェイスを実装する必要がある場合があります (など[Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/))。

すべての Android のクラスとインターフェイスを拡張、 [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)インターフェイスです。 したがって、すべての Android の種類を実装する必要があります`IJavaObject`です。
このファクトの Xamarin.Android を活用&ndash;を使用して`IJavaObject`Java プロキシ (、Android 呼び出し可能ラッパー) で指定したマネージ型の Android を提供します。 **Monodroid.exe**だけを検索`Java.Lang.Object`サブクラス (実装する必要があります`IJavaObject`)、サブクラス`Java.Lang.Object`マネージ コードでインターフェイスを実装する方法を提供します。 例えば:

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

*この記事の残りの部分は、将来予告なしに変更される可能性が実装の詳細を提供*(およびここでは開発者が、内部で何が起こって興味がある可能性があるためにのみ)。

たとえば、次の c# のソースを指定します。

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

基本クラスを保持し、ネイティブ メソッドの宣言はマネージ コード内でオーバーライドされるメソッドごとに提供されることに注意してください。



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute

通常、Xamarin.Android を自動的に生成について; を構成する Java コードこの生成は、クラスは、Java クラスから派生され、Java の既存のメソッドをオーバーライドするときに、クラスとメソッドの名前に基づいています。 ただし、一部のシナリオで下記の手順に従って コード生成が適切なものでないです。

-   Android でアクション名レイアウト XML 属性では、たとえば、 [android: onClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) XML 属性です。 指定すると、高めのインスタンスの表示は、Java メソッドを検索しようとします。

-   [Java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html)インターフェイス必要があります`readObject`と`writeObject`メソッドです。 このインターフェイスのメンバーではないため、対応するマネージ実装にはこれらのメソッドの Java コードを公開しません。

-   [Android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/)インターフェイスでは、実装クラスは、静的フィールドである必要がありますが必要ですが`CREATOR`型の`Parcelable.Creator`します。 生成された Java コードでは、いくつかの明示的なフィールドが必要です。 この標準シナリオでは、マネージ コードから Java コードで出力フィールドにする方法はありません。


Xamarin.Android 4.2 以降コード生成が任意の名前を持つ任意の Java メソッドを生成するソリューションを提供しないので、 [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)と[ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/)されました上記のシナリオにソリューションを提供するが導入されました。 両方の属性が存在する、`Java.Interop`名前空間。

-   `ExportAttribute` &ndash; メソッド名と、予期される例外の種類 (Java で明示的な「スロー」を付けます) を指定します。 メソッドでこれを使用するときに、メソッドは「エクスポート」マネージ メソッドに対応する JNI 呼び出しをディスパッチ コードを生成する Java メソッド。 これで使用できる`android:onClick`と`java.io.Serializable`です。

-   `ExportFieldAttribute` &ndash; フィールド名を指定します。 これは、フィールド初期化子として機能するメソッドに常駐します。 これで使用できる`android.os.Parcelable`です。

[ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/)サンプル プロジェクトは、これらの属性を使用する方法を示しています。


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute と ExportFieldAttribute のトラブルシューティング

-   ないため、パッケージが失敗**Mono.Android.Export.dll** &ndash;を使用した場合`ExportAttribute`または`ExportFieldAttribute`コードまたは依存するライブラリの一部のメソッドでは、上には、追加する必要が**Mono.Android.Export.dll**です。 このアセンブリは、Java からのコールバック コードをサポートするために分離されます。 別**Mono.Android.dll**アプリケーションにその他のサイズが追加されるとします。

-   リリース ビルドで`MissingMethodException`エクスポート メソッドの発生&ndash;でリリース ビルド`MissingMethodException`エクスポート メソッドに対して発生します。 (この問題は、Xamarin.Android の最新のバージョンで修正します)。



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` および`ExportFieldAttribute`実行時のコードを使用して、Java の機能を提供します。 この実行時のコードでは、これらの属性によって生成された JNI メソッドからマネージ コードにアクセスします。 その結果、管理対象のメソッドは、次のバインドです。 既存の Java メソッドはありません。そのため、Java メソッドは、マネージ メソッドのシグネチャから生成されます。

ただし、この場合は、完全に決定ではありません。 特に、これは、マネージ型と Java 型など、一部の高度なマッピングでは true。

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

エクスポートされたメソッドの場合など、これらの型が必要な場合に、`ExportParameterAttribute`対応するパラメーターまたは戻り値の型を明示的に使用する必要があります。



### <a name="annotation-attribute"></a>注釈の属性

変換した Xamarin.Android 4.2 で`IAnnotation`実装の種類に属性 (System.Attribute)、および Java ラッパーで注釈の生成のサポートを追加します。

これは、次の方向性のある変更を意味します。

-   バインディング ジェネレーターが生成されます`Java.Lang.DeprecatedAttribute`から`java.Lang.Deprecated`(べき中`[Obsolete]`マネージ コードで)。

-   既存わけではない`Java.Lang.Deprecated`クラスが表示されなくなります。 これら Java ベースのオブジェクトも使用できます通常の Java オブジェクトとしてこのような使用法が存在する場合)。 ある`Deprecated`と`DeprecatedAttribute`クラスです。

-   `Java.Lang.DeprecatedAttribute`クラス マークが付いている`[Annotation]`です。 これから継承されるカスタム属性がある場合に`[Annotation]`属性に、そのカスタム属性の Java 注釈を実行する msbuild タスクが生成されます (@Deprecated) Android 呼び出し可能ラッパー (について) にします。

-   注釈は、クラス、メソッドを生成できませんでしたし、フィールド (マネージ コードにメソッドが) をエクスポートします。

(自体には、注釈付きのクラスまたは注釈付きのメンバーを含むクラス) は、外側のクラスが登録されていない場合、Java クラス ソース全体は生成されません、注釈を含むです。 メソッドを指定できます、`ExportAttribute`を明示的に生成され、注釈が付けられたメソッドを取得します。 また、Java 注釈クラス定義を「生成」機能ではありません。 つまり、特定の注釈のカスタム マネージ属性を定義する場合は、対応する Java 注釈クラスを含む別の .jar ライブラリを追加する必要があります。 注釈の種類を定義する Java ソース ファイルを追加することはできません。 同じ方法では機能しません、Java コンパイラ**apt**です。

さらに、次の制限が適用されます。

-   この変換プロセスは考慮されません`@Target`注釈注釈の種類にはこれまでです。

-   属性プロパティには機能しません。 代わりに、プロパティ get アクセス操作子または set アクセス操作子の属性を使用します。



## <a name="class-binding"></a>クラスのバインド

クラスをバインドすると、基になる Java 型の呼び出しを簡略化するマネージ呼び出し可能ラッパーの書き込みを意味します。

C# からのオーバーライドを許可するように仮想および抽象メソッドのバインディングには、Xamarin.Android 4.0 が必要です。 ただし、Xamarin.Android の任意のバージョンまたはバインドできます非仮想メソッド、静的メソッドは、仮想メソッドのオーバーライドをサポートしません。

通常、バインディングには、次の項目が含まれます。

-  A [JNI がバインドされている Java の型に対するハンドル](#_Looking_up_Java_Types)です。

-  [JNI フィールド Id およびバインドされた各フィールドのプロパティを](#_Instance_Fields)です。

-  [JNI メソッド Id と各メソッドにメソッドがバインドされている](#_Instance_Methods)です。

-  のに型で必要なサブクラスが必要な場合、 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)カスタム属性を使用して型宣言で[RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) 'éý'`true`です。



### <a name="declaring-type-handle"></a>型ハンドルを宣言します。

フィールドとメソッドのルックアップ メソッドでは、その宣言する型を参照するオブジェクト参照が必要です。 規則では、これに保持されている、`class_ref`フィールド。

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

参照してください、 [JNI 型参照](#_JNI_Type_References)に関する詳細については、「、`CLASS`トークンです。


### <a name="binding-fields"></a>フィールドのバインド

Java フィールドは、c# プロパティ、Java フィールドなどとして公開される[java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in)は c# プロパティとしてバインド[Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/)です。
さらに、ため JNI 静的フィールドおよびインスタンス フィールドを区別、さまざまな方法使用プロパティを実装する場合。

フィールドのバインドには、3 つのメソッドのセットが含まれます。

1.  *フィールド id を取得する*メソッドです。 *フィールド id を取得する*フィールド ハンドルを返すため、メソッドは、*フィールドの値を取得*と*フィールド値を設定*メソッドを使用します。 フィールド id を取得するには、フィールドの名前を入力します。 宣言を知ることが必要です、 [JNI 型シグネチャ](#_JNI_Type_Signatures)フィールドのです。

1.  *フィールドの値を取得*メソッドです。 これらのメソッドは、フィールド ハンドルを必要とし、フィールドの値を Java から読み取りをでしょう。
    使用する方法は、フィールドの型によって異なります。

1.  *フィールド値を設定*メソッドです。 これらのメソッドは、フィールド ハンドルを必要とし、Java 内のフィールドの値の書き込みを担当します。 使用する方法は、フィールドの型によって異なります。


 [静的フィールド](#_Static_Fields)を使用して、 [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)、 `JNIEnv.GetStatic*Field`、および[JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/)メソッドです。

 [インスタンス フィールド](#_Instance_Fields)を使用して、 [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/)、 `JNIEnv.Get*Field`、および[JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/)メソッドです。

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

注: を使用している[InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership))に JNI 参照を変換、`System.IO.Stream`インスタンス、およびお使いの`JniHandleOwnership.TransferLocalRef`ため[JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/)ローカルの参照を返します。

多くは、 [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/)型が`FromJniHandle`目的の型に、JNI を変換するメソッドを参照します。



### <a name="method-binding"></a>メソッドのバインディング

Java メソッドは、c# のメソッドおよび c# のプロパティとして公開されます。 たとえば、Java メソッド[java.lang.Runtime.runFinalizersOnExit](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean))メソッドとしてバインドされている、 [Java.Lang.Runtime.RunFinalizersOnExit](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/)メソッド、および[java.lang.Object.getClass](http://developer.android.com/reference/java/lang/Object.html#getClass)メソッドとしてバインドされている、 [Java.Lang.Object.Class](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/)プロパティです。

メソッドの呼び出しは、2 段階に分かれたプロセスです。

1.  *メソッド id を取得する*メソッドを呼び出します。 *メソッド id を取得する*メソッドは、メソッドの呼び出しメソッドを使用するメソッドのハンドルを返すことを担当します。 メソッドの id を取得するには、宣言、メソッドの名前を入力します。 を知ることが必要です、 [JNI 型シグネチャ](#_JNI_Type_Signatures)メソッドの。

1.  メソッドを呼び出します。

フィールドと同様には、メソッドの id を取得し、メソッドの呼び出しに使用する方法は、静的メソッドとインスタンス メソッドの間で異なります。

[静的メソッド](#_Static_Methods_1)使用[JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)メソッド id を参照して、`JNIEnv.CallStatic*Method`メソッドの呼び出しのファミリです。

[インスタンス メソッド](#_Instance_Methods)使用[JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)メソッド id を参照して、`JNIEnv.Call*Method`と`JNIEnv.CallNonvirtual*Method`メソッドの呼び出しのファミリです。

メソッドのバインディングとは、メソッドの呼び出しでは複数の可能性があるだけです。 メソッドのバインディングも許可する (abstract および非 final メソッド) をオーバーライドするメソッドが含まれていますか、(インターフェイスのメソッド) の実装します。 [インターフェイスの継承のサポート、](#_Supporting_Inheritance,_Interfaces_1)仮想メソッドとインターフェイスのメソッドをサポートするの複雑な作業について説明します。

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>静的メソッド

使用して静的メソッドのバインディングでは、`JNIEnv.GetStaticMethodID`し、適切なを使用して、メソッドのハンドルを取得する`JNIEnv.CallStatic*Method`メソッド、メソッドの戻り値の型によって異なります。 バインディングの例を次に示します、 [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime())メソッド。

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

静的フィールドにメソッドのハンドルを格納する注`id_getRuntime`です。 これは、メソッドのハンドルがすべての呼び出しでは検索する必要があるように、パフォーマンスの最適化です。 この方法でメソッドのハンドルをキャッシュする必要はありません。 メソッドのハンドルを取得すると、一度[JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/)メソッドを呼び出すために使用します。 `JNIEnv.CallStaticObjectMethod` 返します、`IntPtr`返された Java インスタンスのハンドルが含まれています。
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr、JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java ハンドルを厳密に型指定されたオブジェクトのインスタンスに変換するために使用します。



#### <a name="non-virtual-instance-method-binding"></a>非仮想インスタンス メソッドのバインディング

バインディング、`final`インスタンス メソッド、またはインスタンス メソッドをオーバーライドすると、必要ないを使用して`JNIEnv.GetMethodID`し、適切なを使用して、メソッドのハンドルを取得する`JNIEnv.Call*Method`メソッド、メソッドの戻り値の型によって異なります。 バインディングの例を次に示します、`Object.Class`プロパティ。

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

静的フィールドにメソッドのハンドルを格納する注`id_getClass`です。
これは、メソッドのハンドルがすべての呼び出しでは検索する必要があるように、パフォーマンスの最適化です。 この方法でメソッドのハンドルをキャッシュする必要はありません。 メソッドのハンドルを取得すると、一度[JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/)メソッドを呼び出すために使用します。 `JNIEnv.CallStaticObjectMethod` 返します、`IntPtr`返された Java インスタンスのハンドルが含まれています。
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr、JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java ハンドルを厳密に型指定されたオブジェクトのインスタンスに変換するために使用します。


### <a name="binding-constructors"></a>バインディングのコンス トラクター

コンス トラクターは、名前を持つ Java メソッド`"<init>"`です。 Java インスタンス メソッドと同様に、`JNIEnv.GetMethodID`コンス トラクターのハンドルを照合するために使用します。 Java メソッドとは異なり、 [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/)メソッド呼び出しでコンス トラクター メソッドのハンドルを使用します。 戻り値`JNIEnv.NewObject`JNI ローカル参照には。


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

クラスのバインドのサブクラスは通常[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)です。
サブクラス化する`Java.Lang.Object`、追加セマンティック活躍:`Java.Lang.Object`インスタンス管理で Java インスタンスへの参照をグローバル、`Java.Lang.Object.Handle`プロパティです。

1.  `Java.Lang.Object`既定のコンス トラクターは、Java インスタンスを割り当てます。

1.  型がある場合、 `RegisterAttribute` 、および`RegisterAttribute.DoNotGenerateAcw`は`true`のインスタンス、`RegisterAttribute.Name`型が、既定のコンス トラクターによって作成されます。

1.  それ以外の場合、 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (について) に対応する`this.GetType`が既定のコンス トラクターでインスタンス化します。 Android の呼び出し可能ラッパーは、のパッケージの作成中に生成された各`Java.Lang.Object`をサブクラス`RegisterAttribute.DoNotGenerateAcw`に設定されていない`true`です。

ある型では、バインドされませんクラス、これは、予期セマンティック: インスタンス化する、 `Mono.Samples.HelloWorld.HelloAndroid` c# インスタンスは、Java を構築する必要があります`mono.samples.helloworld.HelloAndroid`生成された Android 呼び出し可能ラッパーであるインスタンスです。

クラスのバインディングがあります、正しい動作 Java の型には、既定のコンス トラクターが含まれています。 ユーザーやその他のコンス トラクターを呼び出す必要がない場合。 それ以外の場合、次の操作を実行するコンス トラクターを指定する必要があります。

1.  呼び出す、 [Java.Lang.Object (IntPtr、JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 、既定ではなく`Java.Lang.Object`コンス トラクターです。 これにより、新しい Java インスタンスを作成しないように注意が必要です。

1.  値を確認[Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) Java インスタンスを作成する前にします。 `Object.Handle`プロパティが値以外の`IntPtr.Zero`Java コードで構築された Android 呼び出し可能ラッパーと、作成された Android 呼び出し可能ラッパー インスタンスを格納するクラスのバインディングが構築されるかどうか。 たとえば、Android を作成すると、`mono.samples.helloworld.HelloAndroid`インスタンス、最初、および Java Android 呼び出し可能ラッパーが作成されます`HelloAndroid`コンス トラクターには、対応するインスタンスを作成`Mono.Samples.HelloWorld.HelloAndroid`の種類と、`Object.Handle`プロパティコンス トラクター実行前に、Java インスタンスに設定されています。

1.  型、対応する Android 呼び出し可能ラッパーのインスタンスを作成しを使用して現在のランタイム型が、宣言と同じでない場合[Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership))によって返されるハンドルを格納する[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/)です。

1.  現在のランタイム型が宣言する型と同じ場合は、Java コンス トラクターを呼び出すし、使用[Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership))によって返されるハンドルを格納する`JNIEnv.NewInstance`です。


たとえば、 [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int))コンス トラクターです。 これとしてをバインドします。

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

[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/)メソッドを実行するヘルパー、 `JNIEnv.FindClass`、 `JNIEnv.GetMethodID`、 `JNIEnv.NewObject`、および`JNIEnv.DeleteGlobalReference`から返される値で`JNIEnv.FindClass`です。 詳細については、次のセクションを参照してください。

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>インターフェイスの継承をサポートするには、

Java の型をサブクラス化や Java インターフェイスを実装するの生成が必要です[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) に対して生成されるすべて`Java.Lang.Object`パッケージ化プロセス中にサブクラスです。 使用して制御されますについて生成、 [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)カスタム属性です。

C# 型の場合、`[Register]`カスタム属性のコンス トラクターに 1 つの引数が必要です。 [JNI 型参照を簡略化](#_Simplified_Type_References_1)Java の対応する入力します。 これにより、Java や c# の間で別の名前を提供することができます。

Xamarin.Android 4.0 より前、`[Register]`カスタム属性が「エイリアス」既存の Java 型で使用できません。 これは、ACWs について生成プロセスを生成するためすべて`Java.Lang.Object`サブクラスが発生しました。

Xamarin.Android 4.0 で導入された、 [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/)プロパティです。 このプロパティについて生成処理を指示する*スキップ*新しいマネージ呼び出し可能ラッパー パッケージの作成時に生成される ACWs は発生しませんの宣言を許可する、注釈付きの型。 これには、既存 Java 型のバインディングが使用できます。 たとえば、次の簡単な Java クラス`Adder`、1 つのメソッドが含まれています`add`を整数に追加し、結果を返します。

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder`型としてバインドする可能性があります。

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

ここでは、 `Adder` c# 型*エイリアス*、 `Adder` Java の型。 `[Register]`の JNI 名を指定する属性を使用、 `mono.android.test.Adder` Java の型、および`DoNotGenerateAcw`について生成を抑制するプロパティを使用します。 これは、結果、についての生成時に、`ManagedAdder`型が正しくサブクラス、`mono.android.test.Adder`型です。 場合、`RegisterAttribute.DoNotGenerateAcw`プロパティが使用されておらず、Xamarin.Android ビルド プロセスに生成される新しい`mono.android.test.Adder`Java の型。 これは、結果として、コンパイル エラーとして、 `mono.android.test.Adder` 2 つの独立したファイルに 2 回、型があります。



### <a name="binding-virtual-methods"></a>仮想メソッドのバインディング

`ManagedAdder` サブクラス Java`Adder`型であり、特に興味深いはありません: c#`Adder`型では、すべての仮想メソッドをように定義されていない`ManagedAdder`何も、オーバーライドできません。

バインド`virtual`サブクラスによってオーバーライドを許可する方法には、次の 2 つのカテゴリに分類を実行する必要があるいくつかの処理が必要があります。

1.  **メソッドのバインディング**

1.  **メソッドの登録**


#### <a name="method-binding"></a>メソッドのバインディング

メソッドのバインディングには、c# の 2 つのサポート メンバーの追加が必要です`Adder`定義: `ThresholdType`、および`ThresholdClass`です。

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType`プロパティは、バインディングの現在の型を返します。

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` 仮想および非仮想メソッド ディスパッチを実行する場合を判断するメソッドのバインドに使用されます。 常に返すか、 `System.Type` C# の場合、宣言する型に対応するインスタンス。

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass`プロパティは、バインドの型の JNI クラス参照を返します。

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` 非仮想メソッドを呼び出すときに、メソッドのバインディングで使用されます。

#### <a name="binding-implementation"></a>バインディングの実装

メソッドのバインディングの実装では、Java メソッドの実行時の呼び出しを担当します。 含まれています、`[Register]`メソッド登録の一部であるし、は、メソッドの登録のセクションで説明するカスタム属性の宣言。

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

`id_add`フィールド、メソッドの ID を表す Java メソッドを呼び出します。 `id_add`から値を取得`JNIEnv.GetMethodID`、宣言するクラスを作成する必要があります (`class_ref`)、Java メソッドの名前 (`"add"`)、およびメソッドの JNI シグネチャ (`"(II)I"`)。

メソッドの ID を取得した後`GetType`と比較`ThresholdType`仮想または非仮想のディスパッチが必要かを判断します。 仮想のディスパッチが必要なときに`GetType`と一致する`ThresholdType`、として`Handle`可能性があります、メソッドをオーバーライドして、Java に割り当てられたサブクラスを参照してください。

ときに`GetType`と一致しません`ThresholdType`、`Adder`サブクラス化されている (など`ManagedAdder`)、および`Adder.Add`実装は、サブクラスが呼び出された場合のみ呼び出される`base.Add`です。 これは、ような非仮想ディスパッチが`ThresholdClass`で提供します。 `ThresholdClass` Java クラスを呼び出すメソッドの実装を指定します。



#### <a name="method-registration"></a>メソッドの登録

更新があると仮定`ManagedAdder`定義を上書きする、`Adder.Add`メソッド。

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

注意してください`Adder.Add`いた、`[Register]`カスタム属性。

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]`カスタム属性のコンス トラクターは、3 つの値を受け取ります。

1.  Java メソッドの名前`"add"`ここでします。

1.  メソッドの JNI 型シグネチャ`"(II)I"`ここでします。

1.  *コネクタ メソッド*、`GetAddHandler`ここでします。
    コネクタの方法については、後ほど説明します。


最初の 2 つのパラメーターを使用すると、メソッドをオーバーライドするメソッドの宣言を生成するについて生成処理します。 結果として得られるについてには、次のコードの一部が含まれます。

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

注意してください、`@Override`メソッドを宣言すると、これを委任する場合、 `n_`-同じ名前のメソッドの先頭を指定します。 確認の Java コードを呼び出すときに`ManagedAdder.add`、`ManagedAdder.n_add`呼び出される、これにより、オーバーライドする c#`ManagedAdder.Add`に実行されるメソッド。

したがって、最も重要な質問: どのは`ManagedAdder.n_add`フック`ManagedAdder.Add`しますか?

Java`native`を通じて (Android のランタイム) の Java ランタイムとメソッドが登録されている、 [JNI RegisterNatives 関数](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)です。
`RegisterNatives` これに続くを呼び出すには、Java メソッド名、JNI 型シグネチャ、および関数ポインターを含む構造体の配列を受け取る[呼び出し規約 JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)です。
関数ポインターは、メソッドのパラメーターを続けて 2 つのポインター引数を受け取る関数である必要があります。 Java`ManagedAdder.n_add`を次の C のプロトタイプを持つ関数を使ってメソッドを実装する必要があります。

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android を公開しません、`RegisterNatives`メソッドです。 代わりに、についてと、MCW 一緒に提供を呼び出すために必要な情報`RegisterNatives`: についてには、メソッド名と JNI 型のシグネチャが含まれています、関数ポインターをフックするのには、欠落しているだけです。

これは、ような場合、*コネクタ メソッド*で提供します。 3 番目`[Register]`カスタム属性のパラメーターは、登録済みの型またはパラメーターを受け取らずして返す登録済みの型の基本クラスで定義されたメソッドの名前、`System.Delegate`です。 返された`System.Delegate`順番を正しい JNI 関数のシグネチャを持つメソッドを参照します。 最後に、コネクタ メソッドで返されるデリゲート*必要があります*ルートを指定して、GC は、収集しないように、デリゲートは Java に提供されているとします。

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
int>`を参照するデリゲート、`n_Add`メソッドを呼び出します[JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/)です。
`JNINativeWrapper.CreateDelegate` ハンドルされない例外が処理され、発生させるができるように、try ブロックと catch ブロックで指定されたメソッドをラップ、 [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/)イベント。 得られたデリゲートは、静的に格納されている`cb_add`変数の GC は、デリゲートを解放しないようにします。

最後に、`n_Add`メソッドは、呼び出すメソッドを委任し、対応するマネージ型では、JNI パラメーターをマーシャ リングを担当します。

注: 常に使用`JniHandleOwnership.DoNotTransfer`Java インスタンスに対して、MCW を取得するときにします。 ローカルな参照として扱う (したがってを呼び出すと`JNIEnv.DeleteLocalRef`) が中断されます管理 -&gt; Java -&gt;スタック遷移を管理します。



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


GC の対話型のため、*いない必要があります*を参照するすべてのフィールドを持つ、`Java.Lang.Object`または`Java.Lang.Object`実行時にサブクラスです。 たとえば、型のフィールドは`System.Object`任意のインターフェイス型は使用できません。 参照できない型`Java.Lang.Object`インスタンスが許可されるよう`System.String`と`List<int>`です。 この制限は、GC が早すぎる段階にオブジェクトのコレクションを防止します。

かどうか、型を参照できるインスタンス フィールドを含める必要があります、`Java.Lang.Object`インスタンス、フィールドの型である必要があります`System.WeakReference`または`GCHandle`です。



## <a name="binding-abstract-methods"></a>抽象メソッドのバインディング

バインド`abstract`メソッドは仮想メソッドのバインディングとほぼ同じです。 2 つの違いもあります。

1.  抽象メソッドは抽象クラスです。 これも引き続き、`[Register]`属性と関連付けられたメソッド登録では、バインディングは、メソッドは単に移動、`Invoker`型です。

1.  以外`abstract``Invoker`型には、どのサブクラスが作成される抽象型です。 `Invoker`型は、基底クラスで宣言されているすべての抽象メソッドをオーバーライドする必要がありますされ、オーバーライドされた実装が、メソッドがバインディングの実装では、表示、非仮想ディスパッチ大文字と小文字を区別することができます。


たとえば、あると想定上記`mono.android.test.Adder.add`メソッドが`abstract`です。 C# バインドの変更ができるように`Adder.Add`が抽象型、および新しい`AdderInvoker`に実装されている型が定義されます`Adder.Add`:

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

`Invoker`型は、Java で作成されたインスタンスに JNI 参照を取得する場合にのみ必要です。


## <a name="binding-interfaces"></a>バインド インターフェイス

バインド インターフェイスが含まれている仮想メソッド、クラスをバインドする概念的に似ていますが、微妙な (かつあまり軽度でない) 方法の詳細の多くが異なります。 次の検討[Java インターフェイス宣言](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

インターフェイス バインディング部分があります: C# の場合、インターフェイスの定義とインターフェイスの呼び出し元定義します。



### <a name="interface-definition"></a>インターフェイス定義

C# のインターフェイス定義には、次の要件を満たす必要があります。

-   インターフェイス定義する必要がありますが、`[Register]`カスタム属性です。

-   インターフェイス定義を拡張する必要があります、`IJavaObject interface`です。
    これに失敗すると、ACWs が Java インターフェイスから継承するできなくなります。

-   各インターフェイスのメソッドを含める必要があります、`[Register]`属性に対応する Java メソッド名、JNI 署名、およびコネクタ メソッドを指定します。

-   コネクタ メソッドには、コネクタ メソッドは上にあることができます種類も指定する必要があります。

バインドするときに`abstract`と`virtual`コネクタ メソッドのメソッドを登録されている型の継承階層内で検索します。 インターフェイスしなくてもよいため、これができないため、要件、本文、および本文を含むメソッド コネクタ メソッドが配置されていることを示す型を指定します。 コロンの後ろにコネクタ メソッドの文字列内で指定された型`':'`、呼び出し元を含む型のアセンブリ修飾型名を指定する必要があります。

インターフェイス メソッド宣言は、対応する Java メソッドを使用して、翻訳*互換性*型です。 Java 組み込み型の互換性のある型型では、対応する c#、Java など`int`は c#`int`です。 参照型で、適切な Java の型の JNI ハンドルを提供できる型は互換性のある型です。

Java でインターフェイスのメンバーが直接呼び出されません&ndash;呼び出しは、呼び出し元の型によって媒介は&ndash;程度の柔軟性が許可されるようにします。

Java の進行状況インターフェイスを指定できます[として c# で宣言されている](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Java をマップして、上記で予告`int[]`パラメーターを[JavaArray&lt;int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/)です。
これは必要ありません。 おでしたにバインドされていること、c# `int[]`、または`IList<int>`、または、それ以外完全です。 すべての型を選択した場合、 `Invoker` Java に変換できる必要があります`int[]`呼び出しの種類。


### <a name="invoker-definition"></a>呼び出し元の定義

`Invoker`の種類の定義を継承する必要があります`Java.Lang.Object`適切なインターフェイスを実装して、インターフェイス定義で参照されるすべての接続方法を入力します。 クラスのバインディングとは異なる複数の候補を 1 つがある:`class_ref`フィールドとメソッドの Id がインスタンスのメンバー、静的でないメンバーにする必要があります。

インスタンス メンバーを使い続ける理由の理由を行うには`JNIEnv.GetMethodID`Android ランタイムで動作します。 (Java 動作も可能性があります。 テストされていません。)`JNIEnv.GetMethodID`実装されたインターフェイスと宣言されたインターフェイスではなくに由来するメソッドを検索する場合は null を返します。 検討してください、 [java.util.SortedMap&lt;K, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) Java インターフェイスを実装する、 [java.util.Map&lt;K, V&gt; ](http://developer.android.com/reference/java/util/Map.html)インターフェイスです。 マップを提供、[オフ](http://developer.android.com/reference/java/util/Map.html#clear())メソッド、つまり、一見合理的な`Invoker`SortedMap の定義になります。

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

上記のため失敗します`JNIEnv.GetMethodID`が返されます`null`の検索時に、`Map.clear`メソッドによって、`SortedMap`クラスのインスタンス。

この 2 つのソリューション: すべてのメソッドを起源、どのインターフェイスを追跡して、`class_ref`ごとインターフェイス、またはすべてのインスタンス メンバーとしてしの最も多く派生クラス型、インターフェイス型ではなくメソッドの参照を実行します。 後者が行われる**Mono.Android.dll**です。

呼び出し元の定義が 6 つのセクションでは: コンス トラクター、`Dispose`メソッド、`ThresholdType`と`ThresholdClass`、メンバー、`GetObject`メソッド、インターフェイス メソッドの実装、およびコネクタ メソッドの実装です。



#### <a name="constructor"></a>コンストラクター

コンス トラクターが呼び出されているインスタンスのランタイム クラスを参照し、インスタンスのランタイム クラスを格納する必要がある`class_ref`フィールド。

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

注:`Handle`コンス トラクター本体にあるプロパティを使用する必要がありますおよび not、`handle`パラメーター、Android v4.0 と同様、`handle`基底コンス トラクターは、実行が終了した後にパラメーターが無効になる可能性があります。


#### <a name="dispose-method"></a>Dispose メソッド

`Dispose`メソッドは、コンス トラクターで割り当てられているグローバルの参照を解放する必要があります。

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


#### <a name="thresholdtype-and-thresholdclass"></a>開始および ThresholdClass

`ThresholdType`と`ThresholdClass`メンバーがクラス バインドで見つかったものと同じです。

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

インターフェイスのすべてのメソッドは、JNI から対応する Java メソッドを呼び出すが実装されている必要があります。

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

コネクタ メソッドとサポートするインフラストラクチャが、JNI パラメーターを適切な c# 型をマーシャ リングを担当します。 Java`int[]`パラメーターが、JNI として渡されます`jintArray`、これは、 `IntPtr` c# 内で。 `IntPtr`にマーシャ リングする必要があります、 `JavaArray<int>` c# インターフェイスの呼び出しをサポートするためにします。

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

ただしを`JNIEnv.GetArray`のため、大きな配列をこの結果、追加の GC 負荷の多くの Vm 間で全体の配列をコピーします。


### <a name="complete-invoker-definition"></a>呼び出し元の定義を完了します。

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



## <a name="jni-object-references"></a>JNI オブジェクト参照

多くの JNIEnv メソッドを返します*JNI* *オブジェクトが参照*はのような`GCHandle`s。 JNI が 3 つのオブジェクト参照の種類を提供します。 ローカルの参照、グローバル参照、およびグローバルの弱い参照します。 3 つすべてとして表される`System.IntPtr`、*が*(JNI 関数の型 セクション) に従ってすべて`IntPtr`から返される`JNIEnv`メソッドは、参照します。 たとえば、 [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)を返します、 `IntPtr`、オブジェクト参照が返されないが返されますが、`jmethodID`です。 参照してください、 [JNI 関数ドキュメント](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)詳細についてはします。

ローカルの参照がによって作成される*ほとんど*メソッドの参照を作成します。
Android では、どの時点でも、通常 512 が存在するローカル参照の限られた数だけ許可します。 使用してローカルの参照を削除することができます[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/)です。
戻り値のオブジェクト参照がローカル参照を返す JNIEnv メソッドを参照できませんすべて JNI とは異なり[JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/)を返します、*グローバル*参照します。 できる限り、場合によって構築することによってすばやくローカル参照を削除することを強くお勧め、 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)オブジェクトの周りを指定して`JniHandleOwnership.TransferLocalRef`を[Java.Lang.Object (IntPtrJniHandleOwnership の転送を処理)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)コンス トラクターです。

グローバル参照がによって作成された[JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/)と[JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/)です。
に破棄できます[JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/)です。
エミュレーターでは、ハードウェア デバイスが約 52,000 グローバル参照の制限に 2,000 の未解決のグローバル参照の制限があります。

グローバルの弱い参照は、Android バージョン 2.2 (Froyo) 以降にのみ使用できます。 グローバルの弱い参照を削除することができます[JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr))です。


### <a name="dealing-with-jni-local-references"></a>JNI ローカル参照を処理します。

[JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/)、 [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/)、 [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/)、 [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)と[JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/)を返し、 `IntPtr` JNI ローカルな参照、Java オブジェクトにはが含まれていますまたは`IntPtr.Zero`Java が返される場合は`null`します。 により参照されることを確認する必要がある (512 エントリ) 後に保留にできるローカル参照の数を制限は、適切なタイミングで削除されます。 ローカル参照を処理することがある 3 つの方法: 明示的に削除すると、それらを作成する、`Java.Lang.Object`を使用して、それらを保持するためにインスタンス`Java.Lang.Object.GetObject<T>()`を中心にマネージ呼び出し可能ラッパーを作成します。



### <a name="explicitly-deleting-local-references"></a>ローカルの参照を明示的に削除します。

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/)ローカル参照を削除するために使用します。 ローカルの参照を削除するは使用できません、注意が必要ことを確認するように`JNIEnv.DeleteLocalRef`は最後の参照をローカルで実行します。

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

`Java.Lang.Object` 提供、 [Java.Lang.Object (IntPtr ハンドル、JniHandleOwnership 転送)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)コンス トラクターを既存の JNI 参照をラップするために使用できます。 [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/)パラメーターを指定する方法、`IntPtr`パラメーターを扱う必要があります。

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; 、作成した`Java.Lang.Object`インスタンスからの新しいグローバル参照の作成は、`handle`パラメーター、および`handle`は変更されません。
    呼び出し元が担当する解放`handle`必要に応じて、します。

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; 、作成した`Java.Lang.Object`インスタンスからの新しいグローバル参照の作成は、`handle`パラメーター、および`handle`が一緒に削除[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . 呼び出し元を解放する必要があります`handle`、および使用する必要がありますいない`handle`コンス トラクターは、実行が終了したらです。

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; 、作成した`Java.Lang.Object`インスタンスの所有権を引き継ぎます、`handle`パラメーター。 呼び出し元を解放する必要がありますいない`handle`です。


JNI メソッドの呼び出しを返し、ローカルの refs のため`JniHandleOwnership.TransferLocalRef`は、通常使用します。

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

作成したグローバル参照はまで解放されません、`Java.Lang.Object`インスタンスがガベージ コレクションします。 できない場合は、インスタンスの破棄は参照を解放しグローバル、ガベージ コレクションの高速化します。

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

1.  `T` 実装する必要があります、`IJavaObject`インターフェイスです。

1.  場合`T`は、抽象クラスまたはインターフェイスを`T`パラメーターの型でコンス トラクターを提供する必要があります`(IntPtr,
    JniHandleOwnership)`です。

1.  場合`T`は抽象クラスまたはインターフェイスである*必要があります*する、 *invoker*の使用可能な`T`します。 呼び出し元が継承する非抽象型`T`または実装する`T`、として、同じ名前を持つ`T`Invoker サフィックスが付いています。 たとえば、インターフェイス、T は`Java.Lang.IRunnable`、型、`Java.Lang.IRunnableInvoker`が存在し、必要なを含める必要があります`(IntPtr,
    JniHandleOwnership)`コンス トラクターです。


JNI メソッドの呼び出しを返し、ローカルの refs のため`JniHandleOwnership.TransferLocalRef`は、通常使用します。

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java 型の検索

フィールドまたは JNI でメソッドを検索するには、最初にフィールドまたはメソッドの宣言する型を検索する必要があります。 [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) Java の型を照合するメソッドを使用します。 文字列パラメーターが、*型参照を簡略化*または*完全な型参照*Java の型のです。 参照してください、 [JNI 型の References セクション](#_JNI_Type_References)簡素化と完全の種類の参照に関する詳細。

注: とは異なり他のすべて`JNIEnv`オブジェクト インスタンスを返すメソッド`FindClass`ローカル参照ではなく、グローバルの参照を返します。

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>インスタンス フィールド

フィールドを操作する*フィールド Id*です。 フィールド Id が経由で取得した[JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/)、クラスがありますが、フィールドは、フィールドの名前で定義されていると、 [JNI 型シグネチャ](#JNI_Type_Signatures)フィールドのです。

フィールド Id が解放する必要はありませんは有効では、対応する Java 型が読み込まれている限り、します。 (Android は現在サポートされませんクラスをアンロードします。)

インスタンス フィールドを操作するメソッドの 2 つのセットがあります: 1 つのインスタンス フィールドと 1 つのインスタンス フィールドを記述するための読み取りにします。 すべてのメソッドのセットでは、フィールドの値を読み取ったり書き込んだりするフィールド ID が必要です。


### <a name="reading-instance-field-values"></a>インスタンス フィールドの値の読み取り

インスタンス フィールドの値を読み取るためのメソッドのセットでは、名前付けパターンに従います。

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
ここで`*`フィールドの種類です。

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash;が組み込み型など、任意のインスタンス フィールドの値を読み取る`java.lang.Object`配列、およびインターフェイスの型。 返される値は、JNI ローカル参照です。

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

ここで*型*フィールドの種類です。

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash;が組み込み型など、任意のフィールドの値を書き込む`java.lang.Object`配列、およびインターフェイスの型。 `IntPtr` JNI ローカル参照、JNI グローバル リファレンス、JNI 弱いグローバル参照を値として使用することがありますまたは`IntPtr.Zero`(の`null`)。

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

静的フィールドを操作する*フィールド Id*です。 フィールド Id が経由で取得した[JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/)、クラスがありますが、フィールドは、フィールドの名前で定義されていると、 [JNI 型シグネチャ](#JNI_Type_Signatures)フィールドのです。

フィールド Id が解放する必要はありませんは有効では、対応する Java 型が読み込まれている限り、します。 (Android は現在サポートされませんクラスをアンロードします。)

静的フィールドを操作するメソッドの 2 つのセットがあります: 1 つのインスタンス フィールドと 1 つのインスタンス フィールドを記述するための読み取りにします。 すべてのメソッドのセットでは、フィールドの値を読み取ったり書き込んだりするフィールド ID が必要です。


### <a name="reading-static-field-values"></a>静的フィールドの値の読み取り

静的フィールドの値を読み取るためのメソッドのセットでは、名前付けパターンに従います。

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

ここで`*`フィールドの種類です。

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash;など、組み込み型のない静的フィールドの値を読み取る`java.lang.Object`配列、およびインターフェイスの型。 返される値は、JNI ローカル参照です。

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash;の値を読み取る`bool`静的フィールドです。

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash;の値を読み取る`sbyte`静的フィールドです。

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash;の値を読み取る`char`静的フィールドです。

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash;の値を読み取る`short`静的フィールドです。

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash;の値を読み取る`long`静的フィールドです。

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash;の値を読み取る`float`静的フィールドです。

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash;の値を読み取る`double`静的フィールドです。



### <a name="writing-static-field-values"></a>静的フィールドの値を書き込んでいます

静的フィールドの値を書き込むためのメソッドのセットでは、名前付けパターンに従います。

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

ここで*型*フィールドの種類です。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash;など、組み込み型のない静的フィールドの値を書き込む`java.lang.Object`配列、およびインターフェイスの型。 `IntPtr` JNI ローカル参照、JNI グローバル リファレンス、JNI 弱いグローバル参照を値として使用することがありますまたは`IntPtr.Zero`(の`null`)。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash;の値を書き込む`bool`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash;の値を書き込む`sbyte`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash;の値を書き込む`char`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash;の値を書き込む`short`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash;の値を書き込む`int`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash;の値を書き込む`long`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash;の値を書き込む`float`静的フィールドです。

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash;の値を書き込む`double`静的フィールドです。


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>インスタンス メソッド

を介してインスタンス メソッドが呼び出される*メソッド Id*です。 メソッド Id が経由で取得した[JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)、型が必要ですが、メソッドは、メソッドの名前で定義されていると[JNI 型シグネチャ](#JNI_Type_Signatures)メソッドの。

メソッド Id は、解放する必要はありませんは有効では、対応する Java 型が読み込まれている限り、します。 (Android は現在サポートされませんクラスをアンロードします。)

メソッドを呼び出すためのメソッドの 2 つのセットがあります: 事実上、メソッドを呼び出すための 1 つと非仮想的メソッドを呼び出すための 1 つです。 両方のメソッドのセットが、メソッドを呼び出すメソッドの ID を必要とし、また非仮想呼び出しではどのクラスの実装を呼び出す必要がありますを指定する必要があります。

インターフェイス メソッドのみを検索できます。 宣言する型の中で拡張継承されたインターフェイスに由来するメソッドは検索できません。 後でバインド インターフェイスを参照してください。 呼び出し元の実装詳細については「/。

クラスで宣言されている任意のメソッドまたはその基底クラスまたは実装されたインターフェイスを検索できます。


### <a name="virtual-method-invocation"></a>仮想メソッドの呼び出し

メソッドを呼び出してメソッドのセットには、事実上、名前付けパターンが次に示します。

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

ここで`*`はメソッドの戻り値の型。

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash;など、非組み込み型を返すメソッドを呼び出す`java.lang.Object`、配列、およびインターフェイスします。 返される値は、JNI ローカル参照です。

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash;を返すメソッドを呼び出し、`bool`値。

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash;を返すメソッドを呼び出し、`sbyte`値。

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash;を返すメソッドを呼び出し、`char`値。

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash;を返すメソッドを呼び出し、`short`値。

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash;を返すメソッドを呼び出し、`long`値。

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash;を返すメソッドを呼び出し、`float`値。

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash;を返すメソッドを呼び出し、`double`値。



### <a name="non-virtual-method-invocation"></a>非仮想メソッドの呼び出し

メソッドを呼び出してメソッドのセットには、名前付けパターン非仮想的次に示します。

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

ここで`*`はメソッドの戻り値の型。 非仮想メソッドの呼び出しは通常、仮想メソッドの基本メソッドの呼び出しに使用されます。

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash;など、非組み込み型を返すメソッドを呼び出す非仮想的`java.lang.Object`、配列、およびインターフェイスします。 返される値は、JNI ローカル参照です。

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`bool`値。

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`sbyte`値。

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`char`値。

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`short`値。

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`long`値。

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`float`値。

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash;非仮想的を返すメソッドを呼び出す、`double`値。


<a name="_Static_Methods" />

## <a name="static-methods"></a>静的メソッド

静的メソッドがを通じて呼び出された*メソッド Id*です。 メソッド Id が経由で取得した[JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)、型が必要ですが、メソッドは、メソッドの名前で定義されていると[JNI 型シグネチャ](#JNI_Type_Signatures)メソッドの。

メソッド Id は、解放する必要はありませんは有効では、対応する Java 型が読み込まれている限り、します。 (Android は現在サポートされませんクラスをアンロードします。)



### <a name="static-method-invocation"></a>静的メソッドの呼び出し

メソッドを呼び出してメソッドのセットには、事実上、名前付けパターンが次に示します。

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

ここで`*`はメソッドの戻り値の型。

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash;など、非組み込み型を返す静的メソッドを呼び出す`java.lang.Object`、配列、およびインターフェイスします。 返される値は、JNI ローカル参照です。

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash;を返す静的メソッドを呼び出し、`bool`値。

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash;を返す静的メソッドを呼び出し、`sbyte`値。

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash;を返す静的メソッドを呼び出し、`char`値。

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash;を返す静的メソッドを呼び出し、`short`値。

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash;を返す静的メソッドを呼び出し、`long`値。

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash;を返す静的メソッドを呼び出し、`float`値。

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash;を返す静的メソッドを呼び出し、`double`値。


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI 型シグネチャ

[JNI 型シグネチャ](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432)は[JNI 型参照](#_JNI_Type_References)(型がない単純な参照)、メソッドを除く。 メソッドは、JNI 型のシグネチャが始めかっこ`'('`すべて連結 (が区切るコンマなし、またはその他)、1 つの種類の終わりかっこの後に、パラメーターの型の参照と、その後`')'`、メソッドの戻り値の型の JNI 型参照を続きます。

たとえば、Java メソッドを指定します。

```java
long f(int n, String s, int[] array);
```

JNI 型のシグネチャは次のようになります。

```csharp
(ILjava/lang/String;[I)J
```

一般に、*強く*を使用することをお勧め、 `javap` JNI 署名を確認するコマンド。 JNI 型シグネチャなど、 [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String))メソッドは、"(Ljava/lang/String;) Ljava/lang/スレッド$ 状態"、JNI の署名の種類になる一方で、 [java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values)メソッドは、"() [Ljava/lang/スレッド$ 状態である"です。 末尾のセミコロンを注意します。これら*は*JNI 型のシグネチャの一部です。

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI 型の参照

JNI 型参照は、Java の型参照と異なります。 などの完全修飾型名の Java を使用できません`java.lang.String`JNI、JNI バリエーションを代わりに使用する必要があります`"java/lang/String"`または`"Ljava/lang/String;"`、コンテキストに応じてには詳細については後述します。
JNI 型参照の 4 つの種類があります。

-  **built-in**
-  **simplified**
-  **type**
-  **array**


### <a name="built-in-type-references"></a>組み込み型の参照

組み込み型の参照は、組み込みの値の型を参照するために使用、1 文字です。 マッピングは次のとおりです。

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

型は単純な参照でのみ使用できます[JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String))です。
2 つの方法を簡素化された型の参照を派生させるためにがあります。

1.  Java、完全修飾名に置き換えますすべて`'.'`パッケージ名に含まれると、型名にする前に`'/'`、および各`'.'`と型名に含まれる`'$'`です。

1.  出力を読み取る`'unzip -l android.jar | grep JavaName'`です。


Java の型と、2 つのいずれかが[java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html)型は単純な参照にマップされている`java/lang/Thread$State`です。


### <a name="type-references"></a>型の参照

型参照は組み込み型の参照または使用した型は単純な参照、`'L'`プレフィックスと`';'`サフィックス。 Java の型の[java.lang.String](http://developer.android.com/reference/java/lang/String.html)、簡略化された型の参照が`"java/lang/String"`、型参照は`"Ljava/lang/String;"`します。

型の参照は、配列型の参照と JNI 署名に使用されます。

型の参照を取得する追加の方法がの出力の読み取りを`'javap -s -classpath android.jar fully.qualified.Java.Name'`です。
種類に応じて、関連するコンス トラクターの宣言やも使用できますメソッドが JNI 名を特定の型を返します。 例えば:

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

`Thread.State` Java 列挙型では、署名を使用できるように、`valueOf`型参照が Ljava/lang/スレッド$ 状態であることを確認するメソッド。



### <a name="array-type-references"></a>配列型の参照

配列型の参照が`'['`先頭 JNI 型参照に指定します。
配列を指定するときに、簡略化された型の参照を使用できません。

たとえば、`int[]`は`"[I"`、`int[][]`は`"[[I"`、および`java.lang.Object[]`は`"[Ljava/lang/Object;"`します。



## <a name="java-generics-and-type-erasure"></a>Java ジェネリックと型の消去

*ほとんど*JNI、Java ジェネリックを使用して表示される、時間の*存在しない*です。
いくつか「しわ、」があるが、それらしわが、JNI がどのようにを検索し、汎用メンバーを呼び出しますが、ジェネリックでは、Java がやり取りする方法。

JNI を介してやり取りするときに、ジェネリック型またはメンバーと非ジェネリック型またはメンバーの間の違いはありません。 たとえば、ジェネリック型[java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) 「生」のジェネリック型ではも`java.lang.Class`、どちらも、同じの型は単純な参照がある`"java/lang/Class"`です。


## <a name="java-native-interface-support"></a>Java ネイティブ インターフェイスのサポート

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) Jave ネイティブ インターフェイス (JNI) のマネージ ラッパーがします。 内で JNI 関数が宣言されている、 [Java ネイティブ インターフェイス仕様](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)を明示的に削除する方法が変更されている場合、`JNIEnv*`パラメーターと`IntPtr`の代わりに使用`jobject`、 `jclass`、 `jmethodID`, などです。たとえば、 [JNI NewObject 関数](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

これは、として公開されます、 [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/)メソッド。

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

2 つの呼び出しの間の変換は、ある程度簡単です。 C では、次の必要があります。

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C# の場合と同じようになります。

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr に保持されている Java オブジェクト インスタンスがある場合は、何らかの処理を行う必要があります可能性があります。 など、JNIEnv メソッドを使用できる[ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/)のためが既に存在アナログ c# ラッパー JNI の参照にするラッパーを作成する必要ありますが、します。 これまで行うことができます、 [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/)拡張メソッド。

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

さらに、すべての JNI 関数が変更されて削除することで、`JNIEnv*`パラメーターすべて JNI 関数内に存在します。


## <a name="summary"></a>まとめ

JNI を直接処理する場合は、絶対に避ける必要がある大きな引っかきエクスペリエンスです。 残念ながら、これは常に回避できません。できれば for Android モノラルにバインドされていない Java 事例をヒットしたときに、このガイドはいくつかサポートを提供はします。


## <a name="related-links"></a>関連リンク

- [SanityTests (サンプル)](https://developer.xamarin.com/samples/SanityTests/)
- [Java ネイティブ インターフェイスの仕様](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java ネイティブ インターフェイス関数](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
