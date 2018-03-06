---
title: "アーキテクチャ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9579acc6c070bf692b0db1bd444a31c9ea4aa7ca
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="architecture"></a>アーキテクチャ

Xamarin.Android アプリケーションは、Mono 実行環境内で実行されます。
この実行環境の実行によってサイドで Android ランタイム (アート) 仮想マシンとします。 両方のランタイム環境では、linux 上で実行し、により、開発者は基になるシステムにアクセスするユーザー コードにさまざまな Api を公開します。 モノラル ランタイムは、C 言語で書き込まれます。

使用することができます、[システム](http://msdn.microsoft.com/en-us/library/system.aspx)、 [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx)、 [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx)および .NET の残りの部分クラス ライブラリを基になる Linux オペレーティング システムの機能にアクセスします。

Android でのオーディオ、画像、OpenGL およびテレフォニーのようなシステム機能のほとんどはネイティブ アプリケーションに直接は使用できませんのいずれかに存在している Android のランタイム Java Api を介してのみ公開されている、 [Java](https://developer.xamarin.com/api/namespace/Java.Lang/)*。名前空間または[Android](https://developer.xamarin.com/api/namespace/Android/)。 * 名前空間。 アーキテクチャは、次のようにほぼです。

[![カーネル上と下 .NET と Java + バインド モノラルとアートのダイアグラム](architecture-images/architecture1.png)](architecture-images/architecture1.png)

Xamarin.Android 開発者 (下位アクセス用) が認識される .NET Api への呼び出しまたはによって公開されている Java Api への仲介役を果たしますが Android 名前空間で公開されているクラスを使用して、オペレーティング システムのさまざまな機能にアクセスします。Android のランタイム。

Android のクラスが、Android のランタイム クラスと通信する方法の詳細については、次を参照してください。、 [API の設計](~/android/internals/api-design.md)ドキュメント。

<a name="Application_Packages" />

## <a name="application-packages"></a>アプリケーション パッケージ

Android アプリケーション パッケージは ZIP を使用したコンテナー、 *.apk*ファイル拡張子。 Xamarin.Android アプリケーション パッケージと同じである構造体レイアウトとして、次の項目を追加、標準の Android パッケージ。

-   (IL を含む)、アプリケーション アセンブリが*格納*内で圧縮されていない、*アセンブリ*フォルダーです。 リリースでのスタートアップがビルド プロセス中に、 *.apk*は*mmap()*プロセスと、アセンブリに ed はメモリから読み込まれます。 これにより、アプリの起動時間の短縮を実行する前に抽出する必要はありませんアセンブリとして。 - *注:*アセンブリの場所情報など、 [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/)と[Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *は適して*リリースでは構築します。 個別の filesystem のエントリとして存在しないされ、使用可能な場所がありません。


-   モノのランタイムを含むネイティブ ライブラリに内に存在は、 *.apk*です。 Xamarin.Android アプリケーションなどで、必要に応じて対象と Android のアーキテクチャ、ネイティブ ライブラリを含める必要があります*armeabi* 、 *armeabi v7a* 、 *x86*です。 Xamarin.Android アプリケーションは、適切なランタイム ライブラリが含まれている場合を除き、プラットフォームで実行できません。


Xamarin.Android アプリケーションにも含まれて*Android 呼び出し可能ラッパー* Android マネージ コードへの呼び出しを使用できるようにします。


<a name="Android_Callable_Wrappers" />

## <a name="android-callable-wrappers"></a>Android の呼び出し可能ラッパー

- **Android の呼び出し可能ラッパー**は、 [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface)いつでも Android ランタイムがマネージ コードを呼び出すために必要なために使用するブリッジです。 Android の呼び出し可能ラッパーは、仮想メソッドを優先し、Java インターフェイスを実装することができます。 参照してください、 [Java の統合の概要](~/android/platform/java-integration/index.md)詳細ドキュメント。


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>マネージ呼び出し可能ラッパー

マネージ呼び出し可能ラッパーは、マネージ コードは、Android のコードを呼び出すし、仮想メソッドのオーバーライドおよびインターフェイスの Java インターフェイスを実装するためのサポートを提供する必要があるたびに使用される JNI ブリッジです。 全体[Android](https://developer.xamarin.com/api/namespace/Android/). * 関連する名前空間は、マネージ呼び出し可能ラッパーを経由して生成されると[.jar バインディング](~/android/platform/binding-java-library/index.md)です。
マネージ呼び出し可能ラッパーはマネージ コードと Android の型の間で変換して、基になる Android プラットフォームのメソッドを呼び出す JNI を介してを担当します。

作成された各マネージ呼び出し可能ラッパーを保持経由でアクセスできるは、Java グローバル参照、 [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/)プロパティです。 グローバル参照を使用して、Java インスタンスおよびマネージ インスタンスの間のマッピングを提供します。 グローバル参照が制限されているリソース: エミュレーターは、ほとんどのハードウェアにより、時に存在するグローバル 52,000 以上の参照中に、時に存在するのみ 2000 のグローバル参照を許可します。

グローバル参照の作成および破棄される場合を追跡するために設定することができます、 [debug.mono.log](~/android/troubleshooting/index.md)システム プロパティを含む[gref](~/android/troubleshooting/index.md)です。

グローバル参照を明示的に呼び出すことによって解放される[Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)マネージ呼び出し可能ラッパーにします。 これは、Java インスタンスと、マネージ インスタンスの間のマッピングを削除し、Java インスタンスを収集するを許可します。 場合は、Java インスタンスは、マネージ コードからへの再アクセス、その新しいマネージ呼び出し可能ラッパーが作成されます。

インスタンスを誤って破棄インスタンスとして、スレッド間で共有する場合は、マネージ呼び出し可能ラッパーの破棄と、その他のスレッドからの参照が影響する場合は、注意を実行する必要があります。 最大安全のため、のみ`Dispose()`経由で割り当てられているインスタンスの`new`*または*メソッドから先*知る*新しいインスタンスがキャッシュされたインスタンスを常に割り当てます。スレッド間で共有偶発的なインスタンスが発生します。


<a name="Managed_Callable_Wrapper_Subclasses" />

## <a name="managed-callable-wrapper-subclasses"></a>呼び出し可能ラッパー サブクラスの管理

マネージ呼び出し可能ラッパー サブクラスは、「興味深い」すべてのアプリケーション固有のロジックが live 可能性があります。 ユーザー設定が含まれます[Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)サブクラス (など、 [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13)既定のプロジェクト テンプレートの種類)。 (具体的には、これらはいずれかの*Java.Lang.Object*サブクラスは*いない*を含む、 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)カスタム属性または[RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/)は*false*、既定値です)。

同様に管理されている、呼び出し可能ラッパーをマネージ呼び出し可能ラッパー サブクラスが経由でアクセスできるグローバルの参照を含めることも、 [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/)プロパティです。 マネージ呼び出し可能ラッパーでグローバル参照明示的に解放できるを呼び出して同様[Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)です。
マネージ呼び出し可能ラッパーとは異なり*細心*としてこのようなインスタンスの破棄される前に実行する必要があります*Dispose()*インスタンスの演算を行い、Java インスタンス間のマッピングが中断されます (のインスタンス、Android の呼び出し可能ラッパー) およびマネージ インスタンスです。

<a name="Java_Activation" />

### <a name="java-activation"></a>Java のアクティブ化

ときに、 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) Java から (について) が作成されると、についてコンス トラクターは、対応する c# 呼び出されるコンス トラクターになります。 についてなど*MainActivity*呼び出しは、既定のコンス トラクターが含まれます*MainActivity*の既定のコンス トラクターです。 (これには、 *TypeManager.Activate()*についてコンス トラクター内で呼び出します)。

結果の他の 1 つのコンス トラクター シグネチャがある: *(IntPtr、JniHandleOwnership)*コンス トラクターです。 *(IntPtr、JniHandleOwnership)* Java オブジェクトはマネージ コードに公開され、マネージ呼び出し可能ラッパーを JNI ハンドルを管理するために作成する必要があるたびに、コンス トラクターが呼び出されます。 通常これは自動的にします。

これで 2 つのシナリオがある、 *(IntPtr、JniHandleOwnership)*コンス トラクターを呼び出し可能ラッパーをマネージ サブクラスに手動で指定する必要があります。

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/)がサブクラス化されています。 *アプリケーション*は特殊です既定値*アプリケーション*コンス トラクターは*決して*呼び出されると[(IntPtr、JniHandleOwnership) コンス トラクターを指定する代わりにする必要があります](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105)。

2. 基本クラスのコンス トラクターから仮想メソッドの呼び出しです。


注意してください (2) は、リークを起こして抽象化します。 同様に、c#、Java でコンス トラクターから仮想メソッドへの呼び出しは常に、最派生メソッドの実装を呼び出します。 たとえば、 [TextView (コンテキスト、です、int) コンス トラクター](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/)仮想メソッドを呼び出す[TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod())としてバインドされる、 [TextView.DefaultMovementMethod プロパティ](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/)です。
したがって、型の場合[LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1) に[サブクラス TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26)、(2) [TextView.DefaultMovementMethod をオーバーライド](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)、および (3)[のインスタンスをアクティブ化XML を使用してクラス](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29)、オーバーライドされた*DefaultMovementMethod*についてコンス トラクターを実行する可能性がありは発生前に、c# のコンス トラクターを実行する前に呼び出されるプロパティです。

LogTextBox インスタンスをインスタンス化でサポートされるを通じて、 [LogTextView (IntPtr、JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28)コンス トラクターについて LogTextBox インスタンスが最初に入るとマネージ コード、および起動し、 [LogTextBox (コンテキスト、IAttributeSet、int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41)コンス トラクター *、同じインスタンスで*についてコンス トラクターが実行される場合。

イベントの順序:

1.  XML のレイアウトに読み込まれる、 [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)です。

2.  Android レイアウト オブジェクト グラフをインスタンス化しのインスタンスをインスタンス化*monodroid.apidemo.LogTextBox*のについて*LogTextBox*です。

3.  *Monodroid.apidemo.LogTextBox*コンス トラクターを実行、 [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29)コンス トラクターです。

4.  *TextView*コンス トラクターを呼び出す*monodroid.apidemo.LogTextBox.getDefaultMovementMethod()*です。

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* checks to see if there is already a corresponding C# instance for *handle* . ある場合が返されます。 このシナリオではありません、ため*Object.GetObject&lt;T&gt;()*いずれかを作成する必要があります。

7.  *Object.GetObject&lt;T&gt;()*は検索、 *LogTextBox (IntPtr、JniHandleOwneship)*コンス トラクターは、これを呼び出して、マッピングを作成*処理*と作成されたインスタンスで、作成されたインスタンスを返します。

8.  *TextView.n_GetDefaultMovementMethod()* invokes the *LogTextBox.DefaultMovementMethod* property getter.

9.  制御が戻る、 *android.widget.TextView*コンス トラクターは、実行を終了します。

10. *Monodroid.apidemo.LogTextBox*コンス トラクターを実行する呼び出し*TypeManager.Activate()*です。

11. *LogTextBox (コンテキスト、IAttributeSet、int)*コンス トラクターを実行*(7) で作成された、同じインスタンスで*です。

12. ...


場合 (IntPtr、JniHandleOwnership) コンス トラクターが見つからない場合、 [System.MissingMethodException](https://developer.xamarin.com/api/type/System.MissingMethodException/)がスローされます。


<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>不完全な Dispose() 呼び出し

JNI ハンドルと C# の場合、対応するインスタンスの間のマッピングがあります。 Java.Lang.Object.Dispose() は、このマッピングを解除します。 Java のアクティブ化のようになります JNI ハンドルは、マッピングが切断された後にマネージ コードを入力する場合、 *(IntPtr、JniHandleOwnership)*コンス トラクターを確認し、呼び出されます。 コンス トラクターが存在しない場合は、例外がスローされます。

たとえば、次のマネージ呼び出し可能 Wraper サブクラスを考えてみます。

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

インスタンスの場合は、Dispose() を作成すると、発生する、マネージ呼び出し可能ラッパーを再作成します。

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

プログラムが終了します。

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

サブクラスは含まれている場合、 *(IntPtr、JniHandleOwnership)*コンス トラクター、*新しい*型のインスタンスが作成されます。 その結果、インスタンスは、新しいインスタンスは、""すべてのインスタンスのデータが失わに表示されます。 (値が null であることに注意してください)。

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

のみ*Dispose()*の Java オブジェクトは、使用されませんまたはサブクラスにインスタンス データが含まれていないことがわかっている場合に、呼び出し可能ラッパーのサブクラスをマネージ*(IntPtr、JniHandleOwnership)*コンス トラクターが用意されています。


<a name="Application_Startup" />

## <a name="application-startup"></a>アプリケーションの起動

アクティビティをサービスなどが起動、Android が最初に確認アクティビティ、サービスなどをホストする実行中のプロセスがないか確認します。このようなプロセスが存在しない場合、新しいプロセスが作成されます、 [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html)読み取りとで指定された型には、 [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm)属性は読み込まれ、インスタンス化します。 次に、によって指定されたすべての型、 [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm)属性値がインスタンス化して、 [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/)メソッドが呼び出されました。 追加することによってこの Xamarin.Android フック、*モノラルです。MonoRuntimeProvider* *ContentProvider* AndroidManifest.xml ビルド処理中にします。 *モノラルです。MonoRuntimeProvider.attachInfo()*メソッドはの Mono ランタイムをプロセスに読み込みを管理します。
モノラルを使用して、この時点より前に試みたすべての操作は失敗します。 (*注*その理由はどのサブクラスの種類[Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/)を提供する必要があります、 [(IntPtr、JniHandleOwnership) コンス トラクター](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)アプリケーション インスタンスが、。前に作成モノラルを初期化することができます。)

プロセスの初期化が完了すると、`AndroidManifest.xml`アクティビティ/サービスなどを起動するのクラス名を検索する参照されます。 たとえば、 [ /manifest/application/activity/@android:name属性](http://developer.android.com/guide/topics/manifest/activity-element.html#nm)をロードするためのアクティビティの名前を決定するために使用します。 活動では、この型を継承する必要があります[android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)です。
指定された型が読み込まれた[Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (型は、Java である必要がありますが、したがって Android 呼び出し可能ラッパーを入力)、インスタンス化されます。 Android の呼び出し可能ラッパー インスタンスの作成は、C# の場合、対応する型のインスタンスの作成をトリガーします。 Android が起動し、 [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) 、対応するは[Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)呼び出されることも、競合とします。
