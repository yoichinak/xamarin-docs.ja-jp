---
title: アーキテクチャ
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: ea66cda0e2a1935a430c064c9cebd4134d295729
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60954265"
---
# <a name="architecture"></a>アーキテクチャ

Xamarin.Android アプリケーションは、Mono 実行環境内で実行されます。
この実行環境の実行のサイド Android ランタイム (アート) 仮想マシンとします。 両方のランタイム環境では、Linux カーネル上で実行し、開発者が基になるシステムにアクセスできるユーザー コードにさまざまな API を公開します。 Mono ランタイムは、C 言語で記述されます。

使用すること、[システム](xref:System)、 [System.IO](xref:System.IO)、 [System.Net](xref:System.Net)および .NET の残りの部分クラス ライブラリを基になる Linux オペレーティング システムの機能にアクセスします。

Android では、オーディオ、画像、OpenGL、およびテレフォニーのようなシステム機能のほとんどはネイティブ アプリケーションを直接使用できませんのいずれかに存在するランタイムの Android Java API を介してのみ公開されている、 [Java](https://developer.xamarin.com/api/namespace/Java.Lang/)*。名前空間または[Android](https://developer.xamarin.com/api/namespace/Android/)。 * 名前空間。 アーキテクチャは、このような約です。

[![Mono とアートのカーネル上と下、.NET と Java + バインディング ダイアグラム](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android 開発者アクセス (の低レベルのアクセス許可) がわかっている .NET API を呼び出すかによって公開されている Java API へのブリッジを提供する Android 名前空間で公開されているクラスを使用して、オペレーティング システムのさまざまな機能Android ランタイム。

Android のクラスが Android ランタイム クラスを通信する方法の詳細については、次を参照してください。、 [API の設計](~/android/internals/api-design.md)ドキュメント。


## <a name="application-packages"></a>アプリケーション パッケージ

Android アプリケーション パッケージは ZIP コンテナーで、 *.apk*ファイル拡張子。 Xamarin.Android アプリケーション パッケージに加え、次の標準の Android パッケージの構造とレイアウトが同じであります。

-   (IL を含む)、アプリケーション アセンブリが*格納*内で圧縮されていない、*アセンブリ*フォルダー。 リリースでのスタートアップがビルド プロセス中に、 *.apk*は*mmap()* プロセスと、アセンブリに ed はメモリから読み込まれます。 これにより、アセンブリは、実行前に抽出する必要はありません、アプリの起動時間の短縮、します。  
-   *注:* アセンブリの場所情報など、 [Assembly.Location](xref:System.Reflection.Assembly.Location)と[Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *に依存できません*リリース ビルドにします。 個別のファイル システムのエントリとして存在していないされ、使用可能な場所がありません。


-   Mono ランタイムを含むネイティブ ライブラリが存在する、 *.apk*します。 Xamarin.Android アプリケーションなどで必要な対象となる Android アーキテクチャのネイティブ ライブラリを含める必要があります*armeabi* 、 *armeabi v7a* 、 *x86*します。 Xamarin.Android アプリケーションは、適切なランタイム ライブラリが含まれている場合を除き、プラットフォームで実行できません。


Xamarin.Android アプリケーションを含めることも*Android 呼び出し可能ラッパー*マネージ コードを呼び出す Android を許可します。



## <a name="android-callable-wrappers"></a>Android 呼び出し可能ラッパー

- **Android 呼び出し可能ラッパー**は、 [JNI](https://en.wikipedia.org/wiki/Java_Native_Interface)ブリッジが Android ランタイムがマネージ コードを呼び出す必要があるたびに使用されます。 Android 呼び出し可能ラッパーは仮想メソッドを優先し、Java インターフェイスを実装することができます。 参照してください、 [Java 統合の概要](~/android/platform/java-integration/index.md)詳細ドキュメント。


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>マネージ呼び出し可能ラッパー

マネージ呼び出し可能ラッパーは、マネージ コードは、Android のコードを呼び出すし、仮想メソッドのオーバーライドおよび Java インターフェイスを実装するためのサポートを提供する必要があるたびに使用されている JNI ブリッジです。 全体[Android](https://developer.xamarin.com/api/namespace/Android/). * 関連する名前空間は、管理対象の呼び出し可能ラッパーを使用して生成[.jar のバインド](~/android/platform/binding-java-library/index.md)します。
マネージ呼び出し可能ラッパーはマネージ コードと Android の種類間の変換と JNI を使用して基になる Android プラットフォームのメソッドの呼び出しを担当します。

各管理対象の呼び出し可能ラッパーの保留 Java グローバルの参照を作成からアクセスできる、 [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/)プロパティ。 グローバル参照を使用して、Java インスタンスとマネージ インスタンス間のマッピングを提供します。 グローバル参照が制限されているリソース: エミュレーターは、ほとんどのハードウェアでは、時に存在するグローバル参照を超える 52,000 できますが、時に、存在するグローバル 2000 のみの参照を許可します。

グローバル参照を作成および破棄されるときに、追跡するには設定、 [debug.mono.log](~/android/troubleshooting/index.md)システム プロパティを格納する[gref](~/android/troubleshooting/index.md)します。

グローバル参照を明示的に呼び出すことによって解放される[Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)マネージ呼び出し可能ラッパー。 これは、Java インスタンスとマネージ インスタンス間のマッピングを削除し、Java インスタンスを収集することを許可します。 Java インスタンスはマネージ コードから再アクセス、する場合はその新しいマネージ呼び出し可能ラッパーが作成されます。

他のスレッドからの参照に影響は、インスタンスを誤ってインスタンスを破棄として、スレッド間で共有する場合は、管理されている呼び出し可能ラッパーの破棄時に、注意を実行する必要があります。 のみ最大安全のため`Dispose()`経由で割り当てられているインスタンスの`new`*または*メソッドからが*知る*と可能性がありますいないキャッシュされたインスタンスの新しいインスタンスを常に割り当てる偶発的なインスタンスがスレッド間で共有が発生します。



## <a name="managed-callable-wrapper-subclasses"></a>呼び出し可能ラッパーのサブクラスの管理

呼び出し可能ラッパーをマネージ サブクラスは、「興味深い」すべてのアプリケーション固有のロジックが live 可能性があります。 以下のカスタム[Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)サブクラス (など、 [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13)で既定のプロジェクト テンプレートの種類)。 (具体的には、これらはいずれかの*Java.Lang.Object*サブクラスは*いない*を含む、 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)カスタム属性または[RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/)は*false*、既定値です)。

ような管理、呼び出し可能ラッパーをマネージ呼び出し可能ラッパーのサブクラスをからアクセスできるグローバルな参照を含めることも、 [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/)プロパティ。 管理対象の呼び出し可能ラッパーとグローバル参照に明示的に解放できる呼び出して同様[Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)します。
管理対象の呼び出し可能ラッパーとは異なり*細心*として、このようなインスタンスの破棄される前に実行する必要があります*Dispose()* Java インスタンスの間のマッピングをインスタンスの処理が中断されます (のインスタンス、Android 呼び出し可能ラッパー) とマネージ インスタンス。


### <a name="java-activation"></a>Java のアクティブ化

ときに、 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) Java から (について) が作成されると、対応する c# コンス トラクターを呼び出すと、についてコンス トラクター。 についてなど*MainActivity*呼び出しは、既定のコンス トラクターを含む*MainActivity*の既定のコンス トラクター。 (これを行う、 *TypeManager.Activate()* についてコンス トラクター内で呼び出します)。

結果の他の 1 つのコンス トラクター シグネチャがある: *(IntPtr、JniHandleOwnership)* コンス トラクター。 *(IntPtr、JniHandleOwnership)* Java オブジェクトはマネージ コードに公開および管理されている呼び出し可能ラッパーを JNI のハンドルを管理するために作成する必要があるたびに、コンス トラクターが呼び出されます。 これは、通常は自動的にします。

これで 2 つのシナリオがある、 *(IntPtr、JniHandleOwnership)* コンス トラクターは、呼び出し可能ラッパーをマネージ サブクラスに手動で指定する必要があります。

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/)サブクラスです。 *アプリケーション*は特別な; 既定*アプリケーション*コンス トラクターは*ことはありません*呼び出されると、 [(IntPtr、JniHandleOwnership) コンス トラクターを指定する代わりにする必要があります](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. 基底クラスのコンス トラクターから仮想メソッドの呼び出し。


注意してください (2) はリークの抽象化です。 Java、c# の場合は、ように、コンス トラクターから仮想メソッドへの呼び出しは常に最派生メソッドの実装を呼び出します。 たとえば、 [TextView (コンテキスト、です、int) コンス トラクター](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/)仮想メソッドを呼び出す[TextView.getDefaultMovementMethod()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod())としてバインドされている、 [TextView.DefaultMovementMethod プロパティ](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/)します。
そのため、型の場合[LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1) を[サブクラス TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26)(2)、 [TextView.DefaultMovementMethod をオーバーライド](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)、および (3)[のインスタンスをアクティブ化XML を使用してクラス](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29)、オーバーライドされた*DefaultMovementMethod*プロパティについてコンス トラクターには機会を実行して、これが発生する前に前に呼び出される、C#コンス トラクターが発言するには実行します。

LogTextBox インスタンスをインスタンス化でサポートされるを通じて、 [LogTextView (IntPtr、JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28)コンス トラクターについて LogTextBox インスタンスが最初に入るときにマネージ コード、および起動し、 [LogTextBox (コンテキスト、IAttributeSet、int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41)コンス トラクター*同じインスタンスで*についてコンス トラクターが実行される場合。

イベントの順序:

1.  XML のレイアウトに読み込まれる、 [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)します。

2.  Android レイアウト オブジェクト グラフをインスタンス化およびのインスタンスをインスタンス化*monodroid.apidemo.LogTextBox*のについて*LogTextBox*します。

3.  *Monodroid.apidemo.LogTextBox*コンス トラクターが実行、 [android.widget.TextView](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29)コンス トラクター。

4.  *TextView*コンス トラクターを呼び出す*monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* します。

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* checks to see if there is already a corresponding C# instance for *handle* . ある場合が返されます。 このシナリオではありません、ため*Object.GetObject&lt;T&gt;()* 1 つを作成する必要があります。

7.  *Object.GetObject&lt;T&gt;()* 探し、 *LogTextBox (IntPtr、JniHandleOwneship)* コンス トラクターを呼び出すの間のマッピングを作成します*処理*と作成されたインスタンスを作成したインスタンスを返します。

8.  *TextView.n_GetDefaultMovementMethod()* 呼び出す、 *LogTextBox.DefaultMovementMethod*プロパティ get アクセス操作子。

9.  制御が戻ります、 *android.widget.TextView*コンス トラクターは、実行を終了します。

10. *Monodroid.apidemo.LogTextBox*コンス トラクターが実行を呼び出す*TypeManager.Activate()* します。

11. *LogTextBox (コンテキスト、IAttributeSet、int)* コンス トラクターが実行 *(7) で作成したのと同じインスタンスで*します。

12. 場合 (IntPtr、JniHandleOwnership) コンス トラクターが見つかりません、その後、System.MissingMethodException](xref:System.MissingMethodException) がスローされます。

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>途中の Dispose() の呼び出し

JNI のハンドルと C# の場合、対応するインスタンスの間のマッピングがあります。 Java.Lang.Object.Dispose() では、このマッピングを解除します。 Java のアクティブ化のように見えます JNI ハンドルでは、マッピングが切断された後、マネージ コードが入ると場合、 *(IntPtr、JniHandleOwnership)* コンス トラクターを確認し、呼び出されます。 コンス トラクターが存在しない場合は、例外がスローされます。

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

場合は、Dispose()、インスタンスを作成し、再作成するマネージ呼び出し可能ラッパーが発生します。

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

プログラムが終了されます。

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

サブクラスにが含まれている場合、 *(IntPtr、JniHandleOwnership)* コンス トラクター、*新しい*型のインスタンスが作成されます。 その結果、インスタンスは、新しいインスタンスがあるために、「すべてのインスタンス データにとどめる」に表示されます。 (値が null であることに注意してください)。

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

のみ*Dispose()* の Java オブジェクトがなくなった場合に、使用しないか、サブクラスにインスタンス データが含まれていないことがわかっている場合に、呼び出し可能ラッパーのサブクラスをマネージ *(IntPtr、JniHandleOwnership)* コンス トラクターが用意されています。



## <a name="application-startup"></a>アプリケーションの起動

ときに、アクティビティ、サービスの場合などが起動され、Android が最初に確認アクティビティ、サービスなどをホストする実行中のプロセスがないか確認します。このようなプロセスが存在しない場合、新しいプロセスが作成、 [AndroidManifest.xml](https://developer.android.com/guide/topics/manifest/manifest-intro.html)読み取りとで指定された型には、 [ /manifest/application/@android:name ](https://developer.android.com/guide/topics/manifest/application-element.html#nm)属性が読み込まれ、インスタンス化します。 次に、によって指定されたすべての型、 [ /manifest/application/provider/@android:name ](https://developer.android.com/guide/topics/manifest/provider-element.html#nm)属性値は、インスタンス化してその[ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/)メソッドが呼び出されます。 追加することで、これへの Xamarin.Android フック、 *mono します。MonoRuntimeProvider* *ContentProvider*ビルド プロセス中に AndroidManifest.xml にします。 *Mono します。MonoRuntimeProvider.attachInfo()* メソッドは、Mono ランタイムをプロセスに読み込みを担当します。
このポイントの前に、Mono を使用するすべての試行は失敗します。 (*注*:これは、ためのサブクラスの種類[Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/)を提供する必要があります、 [(IntPtr、JniHandleOwnership) コンス トラクター](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)Mono を初期化する前に、アプリケーション インスタンスが作成されると、します)。

プロセスの初期化が完了すると、`AndroidManifest.xml`アクティビティ/サービスなどを起動するは、クラス名を検索する参照されます。 たとえば、 [ /manifest/application/activity/@android:name属性](https://developer.android.com/guide/topics/manifest/activity-element.html#nm)を読み込むアクティビティの名前を特定するために使用します。 この型を継承する必要があります、アクティビティ[android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)します。
使用して、指定した型が読み込まれる[Class.forName()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (型は、Java である必要がありますが、そのため、Android 呼び出し可能ラッパーを入力)、インスタンス化されます。 Android 呼び出し可能ラッパー インスタンスの作成では、対応する c# の型のインスタンスの作成をトリガーします。 Android を呼び出して[Activity.onCreate(Bundle)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) 、対応するは[Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)呼び出される、競合オフしているとします。
