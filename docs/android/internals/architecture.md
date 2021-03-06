---
title: Architecture
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 8cbcc0cf99dd82dc9a943c63d9e9544482581896
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571416"
---
# <a name="architecture"></a>Architecture

Xamarin Android アプリケーションは、Mono 実行環境内で実行されます。
この実行環境は、Android Runtime (アート) 仮想マシンとサイドバイサイドで実行されます。 どちらのランタイム環境も、Linux カーネル上で実行され、開発者が基になるシステムにアクセスできるようにするためのさまざまな Api をユーザーコードに公開します。 Mono ランタイムは C 言語で記述されています。

[System](xref:System)、 [System.IO](xref:System.IO)、 [System.Net](xref:System.Net) 、およびその他の .net クラスライブラリを使用して、基盤となる Linux オペレーティングシステムの機能にアクセスできます。

Android では、オーディオ、グラフィックス、OpenGL、テレフォニーなどのほとんどのシステム機能をネイティブアプリケーションに直接使用することはできません。これらの機能は、 [java](xref:Java.Lang). * 名前空間または[android](xref:Android). * 名前空間のいずれかに存在する Android ランタイム Java api を介してのみ公開されます。 アーキテクチャは次のようになります。

[![カーネルと .NET/Java + バインドの下にある Mono と ART の図](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin Android 開発者は、ユーザーが認識している .NET Api (低レベルのアクセス用) を呼び出すか、android ランタイムによって公開されている Java Api への橋渡しを行う Android 名前空間で公開されているクラスを使用して、オペレーティングシステムのさまざまな機能にアクセスします。

Android クラスが Android ランタイムクラスと通信する方法の詳細については、 [API の設計](~/android/internals/api-design.md)に関するドキュメントを参照してください。

## <a name="application-packages"></a>アプリケーション パッケージ

Android アプリケーションパッケージは、ファイル拡張子が*apk*の ZIP コンテナーです。 Xamarin Android アプリケーションパッケージの構造とレイアウトは、通常の Android パッケージと同じですが、次の点が追加されています。

- アプリケーションアセンブリ (IL を含む) は、[*アセンブリ*] フォルダー内に圧縮されずに*格納*されます。 リリースでのプロセスの開始時に、がビルドされ*ます。 apk*はプロセスに対して*mmap ()* され、アセンブリはメモリから読み込まれます。 これにより、アセンブリを実行前に抽出する必要がないため、アプリの起動時間が短縮されます。  
- *注:* アセンブリの場所情報 ([アセンブリの場所](xref:System.Reflection.Assembly.Location)や[アセンブリ](xref:System.Reflection.Assembly.CodeBase)など) をリリースビルドで使用*することはできません*。 これらは、個別のファイルシステムエントリとして存在せず、使用できる場所がありません。

- Mono ランタイムを含むネイティブライブラリは、 *apk*内に存在します。 Xamarin Android アプリケーションには、必要な Android アーキテクチャ ( *armeabi* 、 *armeabi-armeabi-v7a* 、 *x86*など) 用のネイティブライブラリが含まれている必要があります。 適切なランタイムライブラリが含まれていない場合、Xamarin Android アプリケーションをプラットフォームで実行することはできません。

Xamarin android アプリケーションには、android からマネージコードを呼び出せるようにするための*Android 呼び出し可能ラッパー*も含まれています。

## <a name="android-callable-wrappers"></a>Android 呼び出し可能ラッパー

- **Android 呼び出し可能ラッパー**は、android ランタイムがマネージコードを呼び出す必要があるときに常に使用される[JNI](https://en.wikipedia.org/wiki/Java_Native_Interface) bridge です。 Android 呼び出し可能ラッパーは、仮想メソッドをオーバーライドし、Java インターフェイスを実装する方法を示します。 詳細については、 [Java 統合の概要](~/android/platform/java-integration/index.md)に関するドキュメントを参照してください。

<a name="Managed_Callable_Wrappers"></a>

## <a name="managed-callable-wrappers"></a>マネージ呼び出し可能ラッパー

マネージ呼び出し可能ラッパーは、マネージコードが Android コードを呼び出す必要があるときに常に使用される JNI bridge で、仮想メソッドのオーバーライドと Java インターフェイスの実装をサポートします。 [Android](xref:Android). * と関連する名前空間全体は、 [.jar バインド](~/android/platform/binding-java-library/index.md)を介して生成されるマネージ呼び出し可能ラッパーです。
マネージ呼び出し可能ラッパーは、JNI を介して、マネージ型と Android 型の間の変換と、基になる Android プラットフォームメソッドの呼び出しを行います。

作成されたマネージ呼び出し可能ラッパーはそれぞれ、Java グローバル参照を保持します。これには、 [IJavaObject](xref:Android.Runtime.IJavaObject.Handle)プロパティを使用してアクセスできます。 グローバル参照は、Java インスタンスとマネージインスタンスの間のマッピングを提供するために使用されます。 グローバル参照は限られたリソースです。エミュレーターでは、2000のグローバル参照だけを同時に存在させることができます。一方、ほとんどのハードウェアでは、52000のグローバル参照を一度に存在させることができます。

グローバル参照がいつ作成[され、](~/android/troubleshooting/index.md)破棄されるかを追跡するに[は、"システムの](~/android/troubleshooting/index.md)プロパティ" プロパティを設定します。

グローバル参照を明示的に解放するには、マネージ呼び出し可能ラッパーで[()](xref:Java.Lang.Object.Dispose)を呼び出します。 これにより、Java インスタンスとマネージインスタンスの間のマッピングが削除され、Java インスタンスを収集できるようになります。 マネージコードから Java インスタンスに再度アクセスする場合は、新しいマネージ呼び出し可能ラッパーが作成されます。

インスタンスが他のスレッドからの参照に影響を与えるように、インスタンスが誤ってスレッド間で共有される可能性がある場合は、マネージ呼び出し可能ラッパーを破棄するときに注意が必要です。 安全性を最大にするために、 `Dispose()` またはメソッドを介して割り当てられたインスタンスのうち、 `new` 常に新しいインスタンスを割り当て、キャッシュされたインスタンスで*は*ない場合にのみ、スレッド間での誤っ*た*インスタンスの共有が発生する可能性があります。

## <a name="managed-callable-wrapper-subclasses"></a>マネージド呼び出し可能ラッパーサブクラス

マネージ呼び出し可能なラッパーサブクラスは、"興味深い" アプリケーション固有のロジックがすべて存在する可能性があります。 これには、カスタムの[Android](xref:Android.App.Activity) Activity1 サブクラス (既定のプロジェクトテンプレートの[Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13)型など) が含まれます。 (具体的には、 [registerattribute](xref:Android.Runtime.RegisterAttribute)カスタム属性または registerattribute を*含まない*すべての*Java. Lang. Object*サブクラスです[。 DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw)は*false*で、これが既定値です)。

マネージ呼び出し可能ラッパーと同様に、マネージ呼び出し可能ラッパーサブクラスにもグローバル参照が含まれています。これは、 [Java... Handle](xref:Java.Lang.Object.Handle)プロパティを通じてアクセスできます。 マネージ呼び出し可能ラッパーと同様に、グローバル参照は、 [Java.. Object. Dispose ()](xref:Java.Lang.Object.Dispose)を呼び出すことによって明示的に解放できます。
マネージ呼び出し可能ラッパーとは異なり、インスタンスを破棄する前に、このようなインスタンスを破棄する前に十分な*注意*を払う必要があります。これは、インスタンスの*Dispose ()* によって、Java インスタンス (Android 呼び出し可能ラッパーのインスタンス) とマネージインスタンスの間のマッピングが解除されるためです。

### <a name="java-activation"></a>Java のアクティブ化

[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) が Java から作成されると、ACW コンストラクターによって、対応する C# コンストラクターが呼び出されます。 たとえば、 *mainactivity*の ACW には、 *mainactivity*の既定のコンストラクターを呼び出す既定のコンストラクターが含まれます。 (これは ACW コンストラクター内の*Typemanager. Activate ()* 呼び出しによって行われます)。

その他のコンストラクターシグネチャとして、 *(IntPtr, jに Handleオーナーシップ)* コンストラクターがあります。 *(IntPtr, JJNI Handleオーナーシップ)* コンストラクターは、Java オブジェクトがマネージコードに公開されるたびに呼び出され、マネージ呼び出し可能ラッパーを構築して、ハンドルを管理する必要があります。 これは通常、自動的に行われます。

次の2つのシナリオでは、マネージ呼び出し可能ラッパーサブクラスで *(IntPtr, jの所有権)* コンストラクターを手動で指定する必要があります。

1. [Android. アプリケーション](xref:Android.App.Application)はサブクラス化されています。 *アプリケーション*は特別です。*既定の*[このコンストラクター] は呼び出され*ません*。[代わりに、(IntPtr, jの所有権) コンストラクターを指定する必要があり](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105)ます。

2. 基底クラスのコンストラクターからの仮想メソッドの呼び出し。

(2) はリーク抽象化であることに注意してください。 Java では、コンストラクターから仮想メソッドを呼び出すと、常に最も派生したメソッドの実装が呼び出されます。 たとえば、 [textview (Context, AttributeSet, int) コンストラクター](xref:Android.Widget.TextView#ctor*)は、 [getDefaultMovementMethod ()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod())仮想メソッドを呼び出します。これは、 [Textview. defaultmovementmethod プロパティ](xref:Android.Widget.TextView.DefaultMovementMethod)としてバインドされます。
したがって、型[Logtextbox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs)が (1)[サブクラス textview](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26)に設定されている場合、(2) は[Textview. defaultmovementmethod をオーバーライド](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)し、(3) [XML を使用してそのクラスのインスタンスをアクティブ化](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29)する前に、オーバーライドされた*defaultmovementmethod*プロパティが呼び出されます。このプロパティは、C# コンストラクターが実行される前に

これは、 [Logtextbox (IntPtr, JACW Handleオーナーシップ)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28)コンストラクターを通じてインスタンス logtextbox をインスタンス化することによってサポートされます。このコンストラクターは、最初にマネージコードを入力した後、ACW コンストラクターの実行時に*同じインスタンスで* [Logtextbox (Context、iattributeset、int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41)コンストラクターを呼び出します。

イベントの順序:

1. レイアウト XML は、 [Contentview](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)に読み込まれます。

2. Android はレイアウトオブジェクトグラフをインスタンス化し、ACW の*インスタンスをインスタンス*化します。これは*logtextbox*の場合に使用します。

3. このようにして、"*モノの id. apide* " という[コンストラクターが実行](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29)されます。

4. *TextView*コンストラクターでは、 *getDefaultMovementMethod ()* を呼び出しています。

5. *getDefaultMovementMethod (n_getDefaultMovementMethod)* は、() を呼び出します。この () は、() を呼び出します。この *(* ) は、() を呼び出します。これにより、 *n_GetDefaultMovementMethod ()* が呼び出されます。 [ &lt; &gt; ](xref:Java.Lang.Object.GetObject*)

6. *Java. オブジェクト. GetObject &lt;TextView &gt; ()* は、対応する C# インスタンスが*ハンドル*に対して既に存在するかどうかを確認します。 存在する場合は、それが返されます。 このシナリオでは、オブジェクトがないため、 *GetObject &lt; t &gt; ()* で作成する必要があります。

7. *オブジェクト. GetObject &lt;T &gt; ()* は*Logtextbox (IntPtr, JniHandleOwneship)* コンストラクターを検索し、それを呼び出して、*ハンドル*と作成されたインスタンスの間のマッピングを作成し、作成されたインスタンスを返します。

8. *TextView. n_GetDefaultMovementMethod ()* は、 *Logtextbox. DefaultMovementMethod*プロパティ getter を呼び出します。

9. コントロールは、実行を終了する、 *android. TextView. TextView*コンストラクターに戻ります。

10. *Typemanager. ap$ mo. LogTextBox*コンストラクターが実行され、 *Typemanager. Activate ()* が呼び出されます。

11. *Logtextbox (Context、IAttributeSet、int)* コンストラクターは、 *(7) で作成された同じインスタンス上で実行さ*れます。

12. (IntPtr, JMissingMethodException Handleの所有権) コンストラクターが見つからない場合は、MissingMethodException (xref:) がスローされます。

<a name="Premature_Dispose_Calls"></a>

### <a name="premature-dispose-calls"></a>破棄 () 呼び出しの途中

JNI ハンドルとそれに対応する C# インスタンスの間にマッピングがあります。 Java. オブジェクト. Dispose () は、このマッピングを中断します。 マッピングが解除された後で JNI ハンドルがマネージコードに入ると、Java のアクティブ化のように表示され、 *(IntPtr, j Handleオーナーシップ)* コンストラクターが確認され、呼び出されます。 コンストラクターが存在しない場合は、例外がスローされます。

たとえば、次のマネージ呼び出し可能 Wraper サブクラスがあるとします。

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

インスタンスを作成し、そのインスタンスを破棄 () して、マネージ呼び出し可能ラッパーを再作成すると、次のようになります。

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

プログラムは次のように死んでしまいます。

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

サブクラスに *(IntPtr, jの所有権)* コンストラクターが含まれている場合、型の*新しい*インスタンスが作成されます。 その結果、インスタンスは新しいインスタンスであるため、インスタンスデータがすべて失われます。 (値が null であることに注意してください)。

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Java オブジェクトが使用されなくなったことがわかっている場合、またはサブクラスにインスタンスデータが含まれておらず、 *(IntPtr, j、所有権)* コンストラクターが指定されている場合は、マネージ呼び出し可能ラッパーサブクラスの*Dispose ()* のみを実行します。

## <a name="application-startup"></a>アプリケーションの起動

アクティビティやサービスなどが起動されると、Android はまず、アクティビティやサービスなどをホストするために実行されているプロセスがあるかどうかを確認します。このようなプロセスが存在しない場合は、新しいプロセスが作成され、 [Androidmanifest .xml](https://developer.android.com/guide/topics/manifest/manifest-intro.html)が読み取られ、属性に指定された型 [/manifest/application/@android:name](https://developer.android.com/guide/topics/manifest/application-element.html#nm) が読み込まれてインスタンス化されます。 次に、属性値によって指定されたすべての型 [/manifest/application/provider/@android:name](https://developer.android.com/guide/topics/manifest/provider-element.html#nm) がインスタンス化され、その[contentprovider. attachinfo %28)](xref:Android.Content.ContentProvider.AttachInfo*)メソッドが呼び出されます。 Xamarin Android は、mono を追加することによってこれにフックします。 *ビルド処理中の MonoRuntimeProvider* *Contentprovider*から AndroidManifest .xml。 *Mono。MonoRuntimeProvider. attachInfo ()* メソッドは、Mono ランタイムをプロセスに読み込みます。
この時点より前に Mono を使用しようとすると、失敗します。 (*注*: これは、Mono が初期化される前にアプリケーションインスタンスが作成されるため、 [Android. App. アプリケーション](xref:Android.App.Application)では、 [(IntPtr, jに handleた所有権) コンストラクター](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)を提供する必要があるためです)。

プロセスの初期化が完了すると、 `AndroidManifest.xml` は、起動するアクティビティ/サービスなどのクラス名を検索するように求められます。 たとえば、 [ /manifest/application/activity/@android:name 属性](https://developer.android.com/guide/topics/manifest/activity-element.html#nm)は、読み込むアクティビティの名前を決定するために使用されます。 アクティビティの場合、この型は[android. app. Activity](xref:Android.App.Activity)を継承する必要があります。
指定された型は、[クラス. forName ()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String))によって読み込まれます。この場合、型は Java 型である必要があります。したがって、Android 呼び出し可能ラッパーがインスタンス化されます。 Android 呼び出し可能ラッパーインスタンスを作成すると、対応する C# 型のインスタンスの作成がトリガーされます。 その後、Android は[onCreate (バンドル)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle))を起動します。これにより、対応する[onCreate (バンドル)](xref:Android.App.Activity.OnCreate*)が呼び出され、競合が発生しなくなります。
