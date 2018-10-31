---
title: ガベージ コレクション
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 814e975f57023424618c5ea403126f36f87467a7
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235013"
---
# <a name="garbage-collection"></a>ガベージ コレクション

Xamarin.Android で Mono を使用して[単純な世代別ガベージ コレクター](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/)します。 これは、2 つの世代にマーク アンド スイープ ガベージ コレクターと*ラージ オブジェクト スペース*、2 つの種類のコレクションで。 

-   マイナー コレクション (収集 Gen0 ヒープ) 
-   (収集 Gen1 およびラージ オブジェクト スペース ヒープ) の主要なコレクション。 

> [!NOTE]
> 使用して明示的なコレクションがない場合は、 [GC。Collect()](xref:System.GC.Collect)コレクションは*オンデマンド*ヒープの割り当てに基づいて、します。 *これは、参照カウント システムではありません*; オブジェクト*未解決の参照がないとすぐには収集されません*、か、スコープが終了しました。 GC は、マイナーのヒープが新しい割り当てのためのメモリを使い果たしたときに実行されます。 割り当てがない場合は実行されません。


マイナー コレクションは低コストな頻度の高いと、最近割り当てられた、配信不能のオブジェクトを収集するために使用します。 マイナー コレクションは、割り当てられたオブジェクトのいくつかの MB ごとの後に実行されます。 軽微なコレクションを呼び出すことによって手動で実行できます[GC。(0) の収集します。](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

メジャーのコレクションでは、コストがかかり、あまり頻繁には、すべて終了したオブジェクトを再利用する使用されます。 現在のヒープ サイズ (ヒープのサイズ変更) する前にメモリが使い果たされると、メジャーのコレクションが実行されます。 メジャーのコレクションを呼び出すことによって手動で実行できます[GC。() を収集](xref:System.GC.Collect)または呼び出すことによって[GC。(Int) の収集](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_)引数で[GC。MaxGeneration](xref:System.GC.MaxGeneration)します。 



## <a name="cross-vm-object-collections"></a>クロス VM オブジェクトのコレクション

オブジェクトの種類の 3 つのカテゴリがあります。

-   **マネージ オブジェクト**: 型は*いない*から継承[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 、例: [System.String](xref:System.String)します。 
    これらは、GC によって通常収集されます。 

-   **Java オブジェクト**: Java の型が Android ランタイム VM 内に存在するが、Mono の VM には公開されません。 これらは、退屈であり、これ以上説明しません。 これらは、Android ランタイム VM によって通常収集されます。 

-   **ピア オブジェクト**: 実装する型[IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) 、例: すべて[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)と[Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/)サブクラスです。 これらの型のインスタンスがある 2 つの"halfs"、*管理されているピア*と*ネイティブ ピア*します。 管理対象のピアのインスタンスである、C#クラス。 ネイティブのピアが Android ランタイム VM 内での Java クラスのインスタンスとC# [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/)プロパティには、ネイティブのピアに JNI グローバル参照が含まれています。 


ネイティブのピアの 2 種類あります。

-   **フレームワークのピア**: 例: xamarin.android を何もがわかっている"Normal"Java 型[android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)します。

-   **ユーザーのピア**: [Android 呼び出し可能ラッパー](~/android/platform/java-integration/working-with-jni.md)ビルド時に、アプリケーション内に存在する各 Java.Lang.Object サブクラスが生成されます。


Xamarin.Android プロセス内で 2 つの Vm があるとは、ガベージ コレクションの 2 つの種類があります。

-   Android ランタイム コレクション 
-   Mono のコレクション 

Android ランタイム コレクションの動作、注意点がありますが、通常、: JNI グローバル参照は、GC ルートとして扱われます。 JNI がグローバルな場合、Android ランタイム VM オブジェクトはオブジェクトを保持してを参照するその結果、*できません*場合でも、コレクションの対象ではそれ以外の場合は、収集します。

Mono のコレクションとは、楽しい作業が行われる場所です。 通常、マネージ オブジェクトが収集されます。 ピア オブジェクトを収集するには、次のプロセスを実行します。

1.  Mono のコレクションの対象となるすべてのピア オブジェクトでは、弱い JNI グローバル参照に置き換え、JNI グローバル参照があります。 

2.  Android ランタイム VM GC が呼び出されます。 任意のネイティブのピア インスタンスは収集可能性があります。 

3.  (1) で作成した、JNI の弱いのグローバルな参照がチェックされます。 弱い参照を収集すると、ピア オブジェクトが収集されます。 弱い参照を持つ場合*いない*収集された、弱い参照は JNI グローバル参照に置き換えられ、ピア オブジェクトは収集されません。 注: API 14 以降でつまりから返される値`IJavaObject.Handle`GC 後に変更する可能性があります。 

これは、いずれかによって参照されている限り、ピア オブジェクトのインスタンスが存在するすべての最終的な結果がマネージ コード (例: に格納されている、`static`変数) または Java コードによって参照されます。 それ以外の場合は、何を超えるネイティブ ピアの有効期間を拡張するさらに、live、ネイティブのピアと管理されているピアの両方が収集可能になるまで、ネイティブのピアが収集可能なできないようです。


## <a name="object-cycles"></a>オブジェクトのサイクル

ピア オブジェクトは、Android ランタイムおよび Mono VM の内部で論理的に存在します。 たとえば、 [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)ピアのマネージ インスタンスは、対応する必要が[android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) framework ピア Java インスタンス。 すべてのオブジェクトから継承する[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)ことが期待される両方の Vm 内の表現があります。 

両方の Vm で表現するすべてのオブジェクトが 1 つの VM 内でのみ存在するオブジェクトと比較して、拡張の有効期間になります (など、 [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601))。 呼び出す[GC。収集](xref:System.GC.Collect)Xamarin.Android GC が収集する前にいずれかの VM で、オブジェクトが参照されていないことを確認する必要が必ずしも、これらのオブジェクトを収集しません。 

オブジェクトの有効期間を短縮する[Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)呼び出す必要があります。 これは手動で""の接続が切断できます。 つまり、オブジェクトを高速で収集するグローバルの参照を解放して、2 つの Vm 間でオブジェクトにします。 


## <a name="automatic-collections"></a>自動コレクション

以降で[リリース 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0)、gref しきい値を超えたときに Xamarin.Android がフル GC に自動的に実行します。 このしきい値は 90% のプラットフォームの既知の最大 grefs: 1800 grefs エミュレーター (最大 2000) でと 46800 grefs ハードウェア (最大 52000)。 *注:* Xamarin.Android によって作成された grefs だけがカウント[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)は、その他のプロセスで作成した grefs は認識していないとします。 これは、ヒューリスティック*のみ*します。 

自動のコレクションが実行されると、次のようなメッセージは、デバッグ ログを印刷します。

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

内で見つかったは、これは、非確定的ないもの時刻 (例: グラフィックス レンダリング) 中に発生する可能性があります。 このメッセージを表示する、他の場所で、明示的なコレクションを実行したい場合がありますまたはしようとする場合[ピア オブジェクトの有効期間を短縮](#Helping_the_GC)します。 

## <a name="gc-bridge-options"></a>GC ブリッジ オプション

Xamarin.Android では、Android と Android ランタイムによる透過的なメモリ管理を提供します。 Mono ガベージ コレクターと呼ばれる拡張機能として実装されます、 *GC ブリッジ*します。 

GC ブリッジは、Mono のガベージ コレクションとアウトされるピアの「存続性」Android ランタイム ヒープを使用して検証オブジェクトする必要があります。 数値の中に機能します。 順序で次の手順を実行して、この判断に GC ブリッジします。

1.  Java のオブジェクトに到達できないピア オブジェクトの mono 参照グラフを誘発します。 

2.  Java の GC を実行します。

3.  どのオブジェクトが実際に実行されないことを確認します。 

この複雑な処理が実現のサブクラス`Java.Lang.Object`任意のオブジェクトを自由に参照以外に Java でのオブジェクトをバインドできるすべての制限を削除しますC#します。 このような複雑さのためには、アプリケーションで顕著な一時停止する可能性が、ブリッジ処理を非常に高価なことができます。 アプリケーションで大きな一時停止が発生している場合は、次の 3 つの GC ブリッジ実装のいずれかを調べてみる価値は。 

-   **Tarjan** -GC ブリッジのまったく新しいデザインに基づいて[Robert Tarjan のアルゴリズムの伝達を逆参照と](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)します。
    最適なパフォーマンス、シミュレートされたワークロードでは、は、実験用のコードの大きな共有もあります。 

-   **新しい**-二次動作の 2 つのインスタンスの修正がコア アルゴリズムを保持する、元のコードの主要な改訂 (に基づいて[Kosaraju のアルゴリズム](http://en.wikipedia.org/wiki/Kosaraju's_algorithm)コンポーネントが接続されている厳密に検索用)。 

-   **古い**-元の実装 (3 つの最も安定したと見なされます)。 これは、アプリケーションで使用する場合のブリッジ、`GC_BRIDGE`一時停止が許容されます。 


最適などの GC のブリッジの動作を確認する唯一の方法は、アプリケーションで実験して出力を分析することです。 ベンチマークのデータを収集する 2 つの方法はあります。 

-   **ログ記録を有効にする**-ログ記録を有効にする (で説明、[構成](~/android/internals/garbage-collection.md)セクション) の GC のブリッジの各オプションでは、キャプチャして、各設定のログ出力を比較します。 検査、`GC`メッセージの各オプションは、特に、`GC_BRIDGE`メッセージ。 非対話型アプリケーションは、許容量が 60 ミリ秒 (ゲーム) などの非常に対話型アプリケーション用の上には、一時停止しますが、問題は、最大 150ms を一時停止します。 

-   **ブリッジのアカウンティングを有効にする**-ブリッジのアカウンティング ブリッジ プロセスに関係する各オブジェクトによって指されるオブジェクトの平均コストが表示されます。 サイズがこの情報を並べ替えると、余分なオブジェクトの最大数を保持している内容についての手掛かりが提供されます。 


指定する`GC_BRIDGE`オプションをアプリケーションには、私たちが必要があります、渡す`bridge-implementation=old`、`bridge-implementation=new`または`bridge-implementation=tarjan`を`MONO_GC_PARAMS`例については、環境変数。 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

既定の設定は**Tarjan**します。 回帰の場合は、する必要がありますこのオプションを設定する**古い**します。 またより安定してを使用する情報を選択することがあります**古い**オプション場合**Tarjan**パフォーマンスの向上は生成されません。 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>GC を支援

GC メモリの使用およびコレクションの時間を短縮するための複数の方法はあります。



### <a name="disposing-of-peer-instances"></a>ピアのインスタンスの破棄

GC に不完全なプロセスが実行されないのは低はメモリが低いときに、GC はそのメモリを認識しないためです。 

インスタンスなど、 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)型または派生型は、少なくとも 20 バイト サイズ (通知などせず変更される可能性など。)。 
[呼び出し可能ラッパーをマネージ](~/android/internals/architecture.md)ので、追加のインスタンス メンバーを追加できませんがある場合、 [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/)インスタンスのメモリ、10 MB の blob を参照する Xamarin.Android の GC をわかりません&ndash;GC20 バイトのオブジェクトが表示されます、10 MB のメモリをアライブに保つは Android の実行時に割り当てられたオブジェクトにリンクされていることを確認することはできません。 

GC を頻繁に必要があります。 残念ながら、 *GC。AddMemoryPressure()* と*GC。RemoveMemoryPressure()* はサポートされていませんのでする*知る*だけラージ Java に割り当てられたオブジェクト グラフを手動で呼び出す必要がありますを解放する[GC。Collect()](xref:System.GC.Collect)プロンプト Java 側を解放する GC にメモリ、またはすることができますの明示的な破棄*Java.Lang.Object*マネージ呼び出し可能ラッパーと Java のインスタンス間のマッピングの重大なサブクラスです。 たとえばを参照してください[バグ 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6)します。 


> [!NOTE]
> 必要があります*非常に*を破棄する場合は注意`Java.Lang.Object`サブクラスのインスタンス。

メモリの破損の可能性を最小限に抑えるには、次のガイドラインを呼び出すときに観察`Dispose()`します。


#### <a name="sharing-between-multiple-threads"></a>複数のスレッド間の共有

場合、 *Java またはマネージ*インスタンス、複数のスレッド間で共有できる*することはできません`Dispose()`d*、**これまで**します。 例えば [`Typeface.Create()`](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) 
返す可能性があります、*キャッシュされたインスタンス*します。 複数のスレッドは、同じ引数を提供する場合は、取得、*同じ*インスタンス。 その結果、`Dispose()`の演算を行い、 `Typeface` 1 つのスレッドからインスタンスが他のスレッドで発生する可能性が無効になる`ArgumentException`から`JNIEnv.CallVoidMethod()`(特に) インスタンスが別のスレッドから破棄されたため、します。 


#### <a name="disposing-bound-java-types"></a>バインドされた Java 型を破棄

インスタンスを破棄することができますのインスタンスがバインドされている Java の型である場合、*限り*マネージ コードからインスタンスを再利用されない*と*Java インスタンスを (参照前のスレッド間で共有することはできません`Typeface.Create()`ディスカッション)。 (この決定を行うにくい場合があります。)マネージ コードを次回 Java インスタンスの入力を*新しい*ラッパーが作成されます。 

これは、機能は、ドローアブルと他のリソースの量が多いインスタンスに関しては多くの場合に便利です。

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

上記は安全なので、ピアを[Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/)返しますはフレームワークのピアを参照してください*いない*User ピア。 `Dispose()`呼び出しの最後に、`using`ブロックは、管理対象の間のリレーションシップを解除[ディスプレイ](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/)および framework[ディスプレイ](http://developer.android.com/reference/android/graphics/drawable/Drawable.html)Java インスタンスをインスタンスAndroid ランタイムをする必要があるとすぐに収集されます。 これが*いない*User ピアにピア インスタンスと呼ばれる場合は、安全では、ここに"external"の情報を使用して*知る*を`Drawable`User ピアを参照できませんので、`Dispose()`を呼び出す安全です。 


#### <a name="disposing-other-types"></a>その他の種類の破棄 

Java 型のバインドのない型のインスタンスを指す場合 (など、カスタム`Activity`)、**しないで**呼び出し`Dispose()`しない限りする*知る*こと Java コードはありませんオーバーライドされたメソッドを呼び出すインスタンス。 そのために[ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls)します。 

たとえばがある場合、カスタム リスナーをクリックします。

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

*しないで*Java は、将来のメソッドを呼び出すしようと、このインスタンスを破棄します。

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>明示的なチェックを使用して例外を回避するには

実装している場合、 [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/)メソッドをオーバー ロード、JNI に関連するオブジェクトに触れないでください。 これを作成、*倍 dispose* (致命的) で、コードをできるようにする場合はガベージ コレクションが既にを基になる Java オブジェクトにアクセスしようとしています。 これには、次のような例外が生成されます。 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

多くの場合、この状況では、オブジェクトの最初の dispose の原因になる null の場合、メンバーと、この null のメンバーで、その後のアクセス試行がスローされる例外が発生し、ときに発生します。 具体的には、オブジェクトの`Handle`で最初の dispose (をマネージ インスタンスにリンクの基になる Java インスタンス) は無効になりますが、でも、マネージ コードは、使用可能な (参照では不要になった場合でもこの基になるJavaインスタンスへのアクセスを試みます。[呼び出し可能ラッパーをマネージ](~/android/internals/architecture.md#Managed_Callable_Wrappers)Java インスタンスとマネージ インスタンス間のマッピングの詳細について)。 

この例外を防止する優れた方法がで明示的に確認するには、`Dispose`と基になる Java インスタンスの管理対象のインスタンス間のマッピングは引き続き無効な場合はメソッドは、確認する場合、オブジェクトの`Handle`が null (`IntPtr.Zero`)前に、そのメンバーにアクセスします。 たとえば、次`Dispose`メソッドへのアクセス、`childViews`オブジェクト。 

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

初期の dispose がエラーの原因を渡す場合`childViews`に無効な`Handle`、`for`ループへのアクセスがスローされます、`ArgumentException`します。 明示的に追加することで`Handle`null チェックを 1 つ目の前に`childViews`、次に、アクセス`Dispose`メソッドは、例外の発生を防ぎます。 

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

インスタンスのたびに、`Java.Lang.Object`型またはサブクラスがスキャンされている場合、GC では、全体の中に*オブジェクト グラフ*インスタンスを指すことはスキャンもする必要があります。 オブジェクト グラフは、一連の「インスタンスのルート」を参照する、オブジェクト インスタンス*plus*ルート インスタンスによって参照されているすべての参照先で再帰的にします。 

次のクラスを検討してください。

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

ときに`BadActivity`が構築されると、オブジェクト グラフの 10004 インスタンスを含む、(1 x `BadActivity`、1 x `strings`、1 x`string[]`によって保持されている`strings`、文字列インスタンス x 10000)、*すべて*のある必要がありますスキャンされるたびに、`BadActivity`インスタンスをスキャンします。 

これは好ましくない影響があります、コレクションの時間の GC の一時停止時間を増加します。 

によって、GC が役立つことができます*削減*ユーザー ピア インスタンスによってルートとするオブジェクト グラフのサイズ。 上記の例では、これを行う移動`BadActivity.strings`Java.Lang.Object から継承しない別のクラス。 

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

軽微なコレクションを呼び出すことによって手動で実行できます[GC。Collect(0)](xref:System.GC.Collect)します。 マイナー コレクションは安価なは、(メジャーのコレクションと比較して) 場合が必要は重大なコスト、固定されている多くの場合、それらをトリガーするため、わずか数ミリ秒の一時停止時間にしてください。 

アプリケーションで同じことは繰り返し実行の「デューティ サイクル」にある場合は、デューティ サイクルが終了した後、マイナーのコレクションを手動で実行することをお勧め場合があります。 デューティ サイクルの例は次のとおりです。 

-  1 つのゲームのフレームのレンダリング サイクルです。
-  特定のアプリのダイアログ (開く、閉じる、入力) と全体の相互作用 
-  ネットワークのグループは、アプリのデータの更新/同期を要求します。



## <a name="major-collections"></a>メジャーのコレクション

メジャーのコレクションを呼び出すことによって手動で実行できます[GC。Collect()](xref:System.GC.Collect)または`GC.Collect(GC.MaxGeneration)`します。 

まれには、実行する必要があります、512 MB のヒープを収集するときに、スタイルの Android デバイスでの秒部分の一時停止時間を必要があります。 

主要なコレクション呼び出すことができる手動で、これまでの場合。 

-   時間のかかる関税の最後にサイクルと、時間の長いが一時停止は、ユーザーに問題を提示しません。 

-   内でオーバーライドされた[Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/)メソッド。 



## <a name="diagnostics"></a>診断

グローバル参照を作成および破棄されるときに、追跡するには設定、 [debug.mono.log](~/android/troubleshooting/index.md)システム プロパティを格納する[ *gref* ](~/android/troubleshooting/index.md)や[ *gc*](~/android/troubleshooting/index.md)します。 



## <a name="configuration"></a>構成

設定して、Xamarin.Android のガベージ コレクターを構成することができます、`MONO_GC_PARAMS`環境変数。 ビルド アクションで環境変数を設定することがあります[AndroidEnvironment](~/android/deploy-test/environment.md)します。

`MONO_GC_PARAMS`環境変数は、次のパラメーターのコンマ区切りの一覧。 

-   `nursery-size` = *サイズ*: 新世代のサイズを設定します。 サイズはバイト単位で、2 の累乗である必要があります。 サフィックス`k`、`m`と`g`キロ、200万とギガバイトでそれぞれ指定するために使用できます。 新世代は、(2) の第 1 世代です。 大きい新世代では、プログラムの速度が通常より多くのメモリを明らかに使用されます。 既定の新世代のサイズを 512 kb です。 

-   `soft-heap-limit` = *サイズ*: ターゲットの最大は、アプリのメモリ使用量を管理します。 メモリ使用量が指定の値を下回ると、実行時間 (少ないコレクション) の GC が最適化されています。 
    メモリ使用量 (複数のコレクション) は、GC が、この上限を超えた最適化されています。 

-   `evacuation-threshold` = *しきい値*: % で退避のしきい値を設定します。 値は 0 ~ 100 の範囲の整数である必要があります。 既定値は 66 です。 コレクションのスイープのフェーズでは、特定のヒープ ブロック型の占有率がこの割合に達しなかったことが検出されるは、コピーする占有率を 100% に近くなってを復元できるため、次の主要なコレクションでそのブロック型のコレクションを行います。 値 0 は、退避をオフにします。 

-   `bridge-implementation` = *実装をブリッジ*: GC ブリッジ アドレス GC パフォーマンスの問題のヘルプが設定されます。 3 つの値がある:*古い*、*新しい*、 *tarjan*します。

-   `bridge-require-precise-merge`ブリッジには引き起こす可能性があります、まれに、オブジェクトを最適化が含まれています: Tarjan では、まずガベージになった後に 1 つの GC が収集されます。 このオプションを含むより遅い可能性がありますより予測可能な Gc を行う、その最適化を無効にします。

たとえば、128 MB のヒープ サイズの制限を GC で構成するを使用してプロジェクトを新しいファイルを追加、**ビルド アクション**の`AndroidEnvironment`内容。 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
