---
title: ガベージ コレクション
ms.topic: article
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: e27e9577957229f347b217a8920eac239799da15
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="garbage-collection"></a>ガベージ コレクション

Xamarin.Android 使用モノラルの[世代単純なガベージ コレクター](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/)です。 これは、2 つの世代にマークおよび掃引ガベージ コレクターおよび*ラージ オブジェクト領域*、2 つの種類のコレクションで。 

-   マイナー コレクション (収集 Gen0 ヒープ) 
-   (Gen1 を収集し、ラージ オブジェクト スペース ヒープ) の主なコレクション。 

> [!NOTE]
> 使用して明示的なコレクションがない場合は、 [GC です。Collect()](xref:System.GC.Collect)コレクションは*オンデマンド*ヒープの割り当てに基づいて、します。 *これは参照カウントのシステムではありません*; オブジェクト*未解決の参照がないとすぐには収集されません*スコープが終了した場合またはします。 GC は、マイナーのヒープが新しい割り当て用のメモリを使い果たしたときに実行されます。 割り当てがない場合は実行されません。


マイナー コレクションは低コストな頻繁に行われると、最近割り当てられ、死んだ状態のオブジェクトを収集するために使用します。 マイナー コレクションは、割り当てられたオブジェクトの数 MB ごとの後に実行されます。 呼び出して、マイナー コレクションを手動で実行する可能性があります[GC です。(0) の収集します。](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

主要なコレクションは、コストがかかり、頻度が低いと、停止しているすべてのオブジェクトの再利用するために使用します。 メジャーのコレクションは、(の前に、ヒープのサイズを変更するには)、現在のヒープ サイズのメモリが不足したり後に実行されます。 呼び出して、メジャーのコレクションを手動で実行する可能性があります[GC です。() を収集](xref:System.GC.Collect)または呼び出すことによって[GC です。(Int) の収集](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_)引数と共に[GC です。MaxGeneration](xref:System.GC.MaxGeneration)です。 



## <a name="cross-vm-object-collections"></a>クロス VM オブジェクトのコレクション

オブジェクトの種類の 3 つのカテゴリがあります。

-   **管理されているオブジェクト**: 型は*いない*から継承[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)など、 [System.String](xref:System.String)です。 
    これらは、GC で通常収集されます。 

-   **Java オブジェクト**: Java の型が Android ランタイム VM 内で存在がモノラル VM に公開されません。 これらは、面倒であり、これ以上説明しません。 これらは、Android ランタイム VM によって通常収集されます。 

-   **オブジェクトをピア ツー ピア**: 実装する型[IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) 、例: すべて[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)と[Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/)サブクラスです。 これらの型のインスタンスがある 2 つの"halfs"、*管理されているピア*と*ネイティブ ピア*です。 管理されているピアは、c# のクラスのインスタンスです。 Android ランタイム、VM 内での Java クラスと、c# のインスタンスは、ネイティブのピア[IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/)プロパティには、ネイティブのピアへの参照グローバル JNI にはが含まれています。 


ネイティブのピアの 2 つの種類があります。

-   **フレームワークのピア**: 例: 既知の Xamarin.Android の何も"Normal"Java 型[android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)です。

-   **ユーザーのピア**: [Android 呼び出し可能ラッパー](~/android/platform/java-integration/working-with-jni.md)ビルド時に、アプリケーション内で各 Java.Lang.Object サブクラスが生成されます。


Xamarin.Android プロセス内で 2 つの Vm があるとは、ガベージ コレクションの 2 つの種類があります。

-   Android ランタイム コレクション 
-   モノラル コレクション 

Android ランタイム コレクション通常が、注意の動作: グローバルの参照を JNI が GC ルートとして扱われます。 グローバル、JNI がある場合を Android ランタイム上 VM オブジェクトをオブジェクトに保持するを参照するそのため、*できません*場合でも、コレクションの対象がそれ以外の場合は、収集します。

モノラル コレクションとは、楽しいがどうなります。 マネージ オブジェクトは、通常どおりに収集されます。 ピア オブジェクトは、次の手順を実行することによって収集されます。

1.  モノラル コレクションの対象となるすべてのピア オブジェクト JNI 弱いグローバル参照に置き換え、JNI グローバル参照であります。 

2.  Android ランタイム VM GC が呼び出されます。 ネイティブ ピア インスタンスを収集する可能性があります。 

3.  (1) で作成した、JNI の弱いのグローバル参照がチェックされます。 弱い参照を収集すると、ピア オブジェクトが収集されます。 弱い参照を持つ場合*いない*収集された、弱い参照は JNI グローバル参照に置き換えられ、ピア オブジェクトは収集されません。 注: API 14+ でつまりから返された値`IJavaObject.Handle`GC 後に変更することがあります。 

その結果、これは、いずれかで参照されている限り、ピア オブジェクトのインスタンスが存在するすべてのマネージ コード (例: に格納されている、`static`変数) または Java コードによって参照されます。 それ以外の場合は、何を超えるネイティブ ピアの有効期間を拡張するさらに、ネイティブのピアことはない収集可能なネイティブのピアと管理されているピアの両方が回収可能になるまでにライブでします。


## <a name="object-cycles"></a>オブジェクトのサイクル

ピア オブジェクトは、Android のランタイムおよびモノラル VM の内部で論理的に存在します。 たとえば、 [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)ピアのマネージ インスタンスが、対応する必要が[android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) framework ピア Java インスタンス。 継承するすべてのオブジェクト[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)両方の Vm 内での表現に生じる場合があります。 

両方の Vm での表現であるすべてのオブジェクトが 1 つの VM 内でのみ存在するオブジェクトと比較して、拡張の有効期間になります (など、 [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601))。 呼び出す[GC です。収集](xref:System.GC.Collect)Xamarin.Android GC がそれを収集する前にいずれかの VM でオブジェクトが参照されないことを確認する必要があると、これらのオブジェクトを収集必ずしもはありません。 

オブジェクトの有効期間を短縮する[Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)呼び出す必要があります。 これは、手動で"接続は切断されます"できるので、高速で収集されるオブジェクトにより、グローバルの参照を解放して、2 つの Vm 間でオブジェクトにします。 


## <a name="automatic-collections"></a>自動コレクション

以降で[リリース 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0)Xamarin.Android が自動的に gref しきい値を超えたときに、完全なガベージ コレクションが実行されます。 このしきい値は 90% のプラットフォームの既知の最大 grefs: エミュレーター (最大 2000) で 1800 grefs と 46800 grefs ハードウェア (最大 52000) にします。 *注:* Xamarin.Android だけがカウントによって作成された grefs [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)プロセスで作成した他の任意の grefs が認識していません。 これは、ヒューリスティック*のみ*です。 

自動のコレクションが実行されると、次のようなメッセージは、デバッグ ログに出力されます。

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

これは、非確定的と (例: の途中でグラフィックス レンダリング) で使用可能性があります。 このメッセージを表示する、他の場所で、明示的なコレクションを実行したい場合がありますまたはしようとする場合[ピア オブジェクトの有効期間を減らす](#Helping_the_GC)です。 

## <a name="gc-bridge-options"></a>GC ブリッジ オプション

Xamarin.Android は、Android、および Android のランタイムでの透過的なメモリ管理を提供します。 呼ばれるモノのガベージ コレクターの拡張機能として実装されて、 *GC ブリッジ*です。 

GC ブリッジは、Mono ガベージ コレクションおよびピアを「ライブ」の Android ランタイム ヒープと確認オブジェクトする必要があります。 図中に機能します。 GC ブリッジは、(順番に) 次の手順を実行して、この決定を加えます。

1.  到達できないピア オブジェクトの mono 参照グラフは、それらが表す Java オブジェクトに強制的に実行します。 

2.  Java GC を実行します。

3.  どのオブジェクトが実際に停止していることを確認します。 

この複雑な処理は、新機能により、サブクラスの`Java.Lang.Object`自由に参照するオブジェクトはどれも; Java で c# にオブジェクトをバインドできる制限が削除されます。 この複雑であるため、ブリッジ処理が非常に不経済があり、アプリケーションで顕著な一時停止が発生することができます。 アプリケーションには、重要な一時停止が発生する場合は、次の 3 つの GC ブリッジ実装のいずれかを調べてみる価値は。 

-   **Tarjan** -GC ブリッジのまったく新しい設計に基づいて[Robert Tarjan のアルゴリズムの伝達を逆参照と](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)です。
    最適なパフォーマンス、シミュレートされた負荷があるけれども、さらに、実験用のコードの大規模な共有 

-   **新しい**-元のコード、二次方程式の動作の 2 つのインスタンスの修正がコア アルゴリズムは保持の主要な見直す (に基づいて[Kosaraju のアルゴリズム](http://en.wikipedia.org/wiki/Kosaraju's_algorithm)コンポーネントが接続されている厳密を検索するため)。 

-   **古い**-元の実装 (3 つの最も安定したと見なされます)。 これは、アプリケーションを使用する必要がある、ブリッジ、`GC_BRIDGE`の一時停止が許容されます。 


最適な GC のブリッジ動作を確認する唯一の方法は、アプリケーションで実験して出力を分析することです。 ベンチマーク データを収集する 2 つの方法があります。 

-   **ログ記録を有効にする**-ログ記録を有効にする (説明として、[構成](~/android/internals/garbage-collection.md)セクション) GC ブリッジの各オプションをキャプチャして、各設定からログ出力を比較します。 検査、`GC`各オプションの以外の場合は特に、メッセージ、`GC_BRIDGE`メッセージ。 非対話型アプリケーション許容可能だが、非常に対話型などのアプリケーション (ゲーム) なって上の一時停止は、問題は、最大 150ms を一時停止します。 

-   **ブリッジのアカウンティングを有効にする**-ブリッジのアカウンティング、ブリッジ処理に関係する各オブジェクトが指すオブジェクトの平均コストが表示されます。 サイズがこの情報を並べ替えると、余分なオブジェクトの最大数を保持している新機能についてのヒントが提供されます。 


指定する`GC_BRIDGE`オプションのアプリケーションでは、us 必要があります、渡す`bridge-implementation=old`、`bridge-implementation=new`または`bridge-implementation=tarjan`を`MONO_GC_PARAMS`例については、環境変数。 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

既定の設定は**Tarjan**です。 見つかった場合は、回帰、する必要がありますには、このオプションを設定する**古い**です。 またより安定性を使用する情報を選択することがあります**古い**オプションの場合は**Tarjan**パフォーマンスの向上は生成されません。 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>GC を支援

メモリの使用およびコレクションの時間を短縮するには、GC を支援するさまざまな方法があります。



### <a name="disposing-of-peer-instances"></a>ピアのインスタンスの破棄

GC が不完全なプロセスと実行されない可能性がありますのときにメモリが少ないため、GC は、そのメモリを認識しませんが不足します。 

インスタンスなど、 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)型または派生型は、少なくとも 20 バイト サイズ (通知などは、せず変更される可能性がなどです。)。 
[呼び出し可能ラッパーをマネージ](~/android/internals/architecture.md)ので、追加のインスタンス メンバーを追加できませんがある場合、 [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/)メモリの 10 MB の blob を参照するインスタンスを Xamarin.Android の GC がわかりません&ndash;GC20 バイトのオブジェクトが表示され、10 MB のメモリをアライブに保つは Android のランタイムに割り当てられたオブジェクトにリンクされていることを確認することはできません。 

これは GC のために頻繁に必要です。 残念ながら、 *GC です。AddMemoryPressure()*と*GC です。RemoveMemoryPressure()*はサポートされていませんのでする*知る*だけを手動で呼び出す必要があります、大規模な Java に割り当てられたオブジェクト グラフを解放する[GC です。Collect()](xref:System.GC.Collect)プロンプト Java 側を解放する GC にメモリ、または明示的に破棄*Java.Lang.Object*サブクラス、マネージ呼び出し可能ラッパーと Java インスタンス間のマッピングを解除します。 たとえばを参照してください[バグ 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6)です。 


> [!NOTE]
> 必要があります*非常に高く*を破棄するときは注意が必要`Java.Lang.Object`サブクラス インスタンス。

メモリの破損の可能性を最小限に抑えるためには、次のガイドラインを呼び出すときに守って`Dispose()`です。


#### <a name="sharing-between-multiple-threads"></a>複数のスレッド間の共有

場合、 *Java またはマネージ*インスタンスは、複数のスレッド間で共有することが*することはできません`Dispose()`d*、**れた**です。 たとえば、 [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle))返す可能性があります、*キャッシュされたインスタンス*です。 複数のスレッドは、同じ引数を提供する場合は、取得、*同じ*インスタンス。 その結果、`Dispose()`の演算を行い、 `Typeface` 1 つのスレッドからのインスタンスになり、他のスレッドが無効になる`ArgumentException`から`JNIEnv.CallVoidMethod()`(など)、インスタンスが別のスレッドから破棄されたためです。 


#### <a name="disposing-bound-java-types"></a>バインドされた Java の型を破棄しています。

インスタンスを破棄することができますのインスタンスがバインドされている Java の型である場合、*限り*インスタンスは、マネージ コードから再利用されない*と*(参照前のスレッド間でJavaインスタンスを共有することはできません`Typeface.Create()`ディスカッション)。 (この決定を行うにくい場合があります。)マネージ コードを次回 Java インスタンスを入力、*新しい*ラッパーが作成されます。 

これはドロウアブルと他のリソースの量が多いインスタンスする際に役に立ちます。

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

上記の項目が安全であるため、ピアを[Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/)返しますはフレームワークのピアを参照してください*されません*ユーザー ピアです。 `Dispose()` 、最後の呼び出し、`using`ブロックには、管理対象の間のリレーションシップを壊す[ディスプレイ](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/)および framework[ディスプレイ](http://developer.android.com/reference/android/graphics/drawable/Drawable.html)Java インスタンスをインスタンスAndroid のランタイムをする必要があると、すぐに収集されます。 この*いない*ユーザー ピアにピア インスタンスが参照される場合は安全ですここで"external"の情報を使用している*知る*を、`Drawable`できないユーザーのピアでは、を参照してくださいであるため、`Dispose()`を呼び出す。安全です。 


#### <a name="disposing-other-types"></a>その他の種類を破棄しています。 

インスタンスは、Java の型のバインディングのない型を参照する場合 (カスタムなど`Activity`)、**しないで**呼び出す`Dispose()`しない限りする*知る*ことの Java コードはありませんオーバーライドのメソッドを呼び出すをインスタンス。 これを行うに障害が発生で[ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls)です。 

たとえばがある場合、カスタム リスナーをクリックします。

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

*しないで*Java は、将来にメソッドを呼び出すしようと、このインスタンスを破棄します。

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>明示的なチェックを使用して例外を回避するには

実装している場合、 [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/)メソッドをオーバー ロード、JNI が関係するオブジェクトを変更することを回避します。 これが生じる、*倍 dispose*可能にするようにコード (致命的) 状況はガベージ コレクションを既にされている基になる Java オブジェクトにアクセスしようとしています。 これには、次のような例外が生成されます。 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

オブジェクトの最初の dispose 原因になる、null のメンバーと、スローされる例外が発生したこの null のメンバーで後でアクセスしようとし、多くの場合、この状況が発生します。 具体的には、オブジェクトの`Handle`最初の破棄するときに (をマネージ インスタンスにリンクの基になる Java インスタンス) は無効になりますが、でも、マネージ コードは、使用可能な (参照ではなくなった場合でもこの基になるJavaインスタンスへのアクセスを試みます。[呼び出し可能ラッパーをマネージ](~/android/internals/architecture.md#Managed_Callable_Wrappers)の詳細については、Java インスタンスおよびマネージ インスタンスの間のマッピング)。 

この例外を防止することをお勧めはで明示的に確認すること、`Dispose`メソッドに、マネージ インスタンスと、基になる Java インスタンス間のマッピングは引き続き無効ですつまり、かどうかをチェック オブジェクトの`Handle`が null (`IntPtr.Zero`)。前に、そのメンバーにアクセスします。 たとえば、次`Dispose`メソッドへのアクセス、`childViews`オブジェクト。 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

初期 dispose が原因を渡す場合`childViews`に無効な`Handle`、`for`ループ アクセスがスローされます、`ArgumentException`です。 明示的なを追加して`Handle`前に チェックを null `childViews` 、次に、アクセス`Dispose`メソッドは、例外の発生を防止。 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>参照先のインスタンスを減らす

インスタンスのたびに、`Java.Lang.Object`型またはサブクラスがスキャンされている場合、GC では、全体の中に*オブジェクト グラフ*インスタンスを指すことはスキャンもする必要があります。 「ルート インスタンス」が参照するオブジェクトのインスタンスのセットは、オブジェクト グラフ*plus*ルート インスタンスによって参照されるすべてを指す、再帰的にします。 

次のクラスを考えます。

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

ときに`BadActivity`が構築すると、オブジェクト グラフ インスタンスを含む 10004 (1 x `BadActivity`、1 x `strings`、1 x`string[]`によって保持されている`strings`、文字列インスタンス x 10000)、*すべて*のする必要がありますスキャンされるたびに、`BadActivity`インスタンスをスキャンします。 

これは悪影響を及ぼす影響があります、収集時間での結果として得られる GC 一時停止時間を増加します。 

GC をいただくこと*削減*ユーザー ピア インスタンスをルートとするオブジェクト グラフのサイズ。 上記の例では、これ行う移動することによって`BadActivity.strings`Java.Lang.Object から継承しない別のクラス。 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>マイナー コレクション

呼び出して、マイナー コレクションを手動で実行する可能性があります[GC です。Collect(0)](xref:System.GC.Collect)です。 マイナー コレクションは、安価なは、(主なコレクションと比較して) 場合、が重大な問題、コストを修正できるように多くの場合、それらをトリガーしたくないあり必要がありますが、いくつかのミリ秒単位の停止時間。 

アプリケーションを同じものは繰り返しの「デューティ サイクル」にある場合がありますマイナーのコレクションを手動で実行することをお勧めデューティ サイクルが終了した後。 デューティ サイクルの例は次のとおりです。 

-  1 つのゲーム フレームのレンダリング サイクルです。
-  (開く、閉じる、入力)、指定したアプリのダイアログ ボックスと全体の相互作用 
-  ネットワークのグループは、アプリのデータの更新/同期を要求します。



## <a name="major-collections"></a>主要なコレクション

呼び出して、メジャーのコレクションを手動で実行する可能性があります[GC です。Collect()](xref:System.GC.Collect)または`GC.Collect(GC.MaxGeneration)`です。 

まれに、実行する必要があります、512 MB のヒープを収集するときに、Android スタイルのデバイスでの秒部分の停止時間を必要があります。 

メジャーのコレクションだけ手動で呼び出してくださいをもし。 

-   時間のかかる負荷の最後にサイクルと長整数型が一時停止はユーザーに問題を表示されません。 

-   内でオーバーライドされた[Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/)メソッドです。 



## <a name="diagnostics"></a>診断

グローバル参照の作成および破棄される場合を追跡するために設定することができます、 [debug.mono.log](~/android/troubleshooting/index.md)システム プロパティを含む[ *gref* ](~/android/troubleshooting/index.md)や[ *gc*](~/android/troubleshooting/index.md)です。 



## <a name="configuration"></a>構成

設定して Xamarin.Android ガベージ コレクターを構成することができます、`MONO_GC_PARAMS`環境変数。 ビルド アクションで環境変数を設定することがあります[AndroidEnvironment](~/android/deploy-test/environment.md)です。

`MONO_GC_PARAMS`環境変数は、次のパラメーターのコンマ区切りの一覧。 

-   `nursery-size` = *サイズ*: 収集家のサイズを設定します。 サイズは、(バイト単位) が指定され、2 の累乗にする必要があります。 サフィックス`k`、`m`と`g`キロ、メガとギガバイトでそれぞれ指定するために使用できます。 収集家は、(2) の第 1 世代です。 大規模な収集家は、通常、プログラムを高速化より多くのメモリを明らかに使用されます。 既定の収集家サイズは 512 kb です。 

-   `soft-heap-limit` = *サイズ*: ターゲットの最大値が、アプリのメモリ消費量を管理します。 メモリ使用量は、指定した値を下回っている、実行時間 (少ないコレクション) の GC が最適化されます。 
    この制限を超えた GC は、メモリ使用量 (複数のコレクション) に最適です。 

-   `evacuation-threshold` = *しきい値*: 退避のしきい値をパーセント単位で設定します。 値は、0 ~ 100 の範囲の整数にする必要があります。 既定値は、66 です。 コレクションのスイープのフェーズでは、特定のヒープ ブロック型の占有率は、この割合に達しなかった検出されると、そのブロックの型の次の主要なコレクションで復元できるため、100% に近くする占有率コピー コレクション実行されるか。 値 0 は、退避をオフにします。 

-   `bridge-implementation` = *実装をブリッジ*: GC ブリッジのためのオプションがパフォーマンスの問題を解決 GC 設定されます。 3 つの値がある:*古い*、*新しい*、 *tarjan*です。

-   `bridge-require-precise-merge`ブリッジが引き起こす可能性があります、まれに、オブジェクトを最適化が含まれています: Tarjan では、ガベージが最初にその後に 1 つの GC が収集されます。 このオプションを含む可能性のある遅くなりますより予測可能な Gc を行う、その最適化を無効にします。

たとえば、128 MB のヒープ サイズ制限に GC を構成するを使用してプロジェクトに新しいファイルを追加、**ビルド アクション**の`AndroidEnvironment`内容。 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
