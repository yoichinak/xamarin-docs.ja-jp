---
title: "API の設計"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 23aa944b88fe3e743b6b29810c29d1843f2efc29
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="api-design"></a>API の設計


## <a name="overview"></a>概要

コア モノラルの一部である基本クラス ライブラリ、に加えて Xamarin.Android モノラルでネイティブの Android アプリケーションを作成するためにさまざまな Android Api のバインドに付属しています。

中核 Xamarin.Android がありますが、相互運用機能のエンジン Java 世界中でそのブリッジ c# 世界し、(C#) または他の .NET 言語から、Java Api にアクセス権を持つ開発者を提供します。


## <a name="design-principles"></a>デザインの原則

これらは、デザインの原則 Xamarin.Android バインディングのためのいくつか

-  準拠している、 [.NET Framework デザイン ガイドライン](http://msdn.microsoft.com/en-us/library/ms229042.aspx)です。

-  Java クラスのサブクラスを開発者ができるようにします。

-  サブクラスには、c# の標準的な構成要素が動作します。

-  既存のクラスから派生します。

-  チェーンに基底コンス トラクターを呼び出します。

-  メソッドのオーバーライドは、# のオーバーライド システムで行う必要があります。

-  可能一般的な Java タスクが簡単に、およびハード Java タスク。

-  C# のプロパティとして JavaBean プロパティを公開します。

-  厳密に型指定された API を公開します。

    - タイプ セーフを向上します。

    - ランタイム エラーを最小限に抑えます。

    - 戻り値の型には、IDE の intellisense を取得します。

    - IDE のポップアップのドキュメントでは、します。

-  Api の IDE の調査をお勧めします。

    - フレームワークの代替手段を最小限に抑える Java Classlib 露出を利用します。

    - 適切なと該当する場合は、単一メソッド インターフェイスではなくデリゲート (C#) (ラムダ、匿名メソッドと System.Delegate) を公開します。

    - 任意の Java ライブラリを呼び出すための機構を提供 ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/))。



## <a name="assemblies"></a>アセンブリ

Xamarin.Android には形成するアセンブリの数が含まれています、 *MonoMobile プロファイル*です。 [アセンブリ](~/cross-platform/internals/available-assemblies.md)ページには詳細についてはします。

Android プラットフォームへのバインドに含まれる、`Mono.Android.dll`アセンブリ。 このアセンブリには、使用の Android api 全体のバインディングと Android ランタイム VM との通信が含まれています。


## <a name="binding-design"></a>バインディングのデザイン


### <a name="collections"></a>コレクション

Android Api では、リスト、セット、およびマップを提供する広範な java.util コレクションを使用します。 使用してこれらの要素を公開して、 [System.Collections.Generic](https://developer.xamarin.com/api/namespace/System.Collections.Generic/)バインディング内のインターフェイスです。 基本的なマッピングは次のとおりです。

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html)システム型にマップ[ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx)、ヘルパー クラス[Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/)です。

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html)システム型にマップ[IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx)、ヘルパー クラス[Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/)です。

-   [java.util.Map<K,V>](http://developer.android.com/reference/java/util/Map.html) maps to system type [IDictionary<TKey,TValue>](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), helper class [Android.Runtime.JavaDictionary<K,V>](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html)システム型にマップ[ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx)、ヘルパー クラス[Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/)です。

高速 copyless これらの型のマーシャ リングするためのヘルパー クラスが用意されています。 可能であれば、提供されているフレームワークの実装ではなくコレクションを提供されるこれらを使用することをお勧め、like [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/)または[ `Dictionary<TKey, TValue>`](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/)です。 [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/)実装ネイティブ Java コレクションを内部的に使用され、したがって必要ありません Android API メンバーを渡すときに、ネイティブ コレクションとの間のコピーします。

任意のインターフェイスの実装をそのインターフェイスを受け付ける Android のメソッドに渡すなどを渡すことができます、`List<int>`を[ArrayAdapter&lt;int&gt;(コンテキスト、int、IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)コンス トラクターです。 *ただし*、すべての実装の*を除く*Android.Runtime 実装では、このためには*コピー* Android ランタイム VM にモノラル VM からのリスト。 リストが以降の場合は、Android のランタイム内で変更されました (例: を呼び出して、 [ArrayAdapter&lt;T&gt;です。Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/)メソッド)、それらの変更*されません*マネージ コードに表示されます。 場合、`JavaList<int>`が使用すると、それらの変更の場合表示されます。

Rephrased、コレクション インターフェイスの実装は*されません*上記のいずれかが表示**ヘルパー クラス**es の [In] のみマーシャ リングします。

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```


### <a name="properties"></a>プロパティ

Java メソッドは、該当する場合のプロパティに変換されます。

-  Java メソッド ペア`T getFoo()`と`void setFoo(T)`に変換されて、`Foo`プロパティです。 例: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/)です。

-  Java メソッド`getFoo()`は読み取り専用 Foo プロパティに変換します。 例: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/)です。

-  設定専用のプロパティは生成されません。

-  プロパティは、*いない*プロパティの型が配列の場合に生成します。



### <a name="events-and-listeners"></a>イベントとリスナー

Android Api は Java の上に構築され、そのコンポーネントのイベント リスナーをフックするため Java パターンに従います。 このパターンする匿名クラスを作成し、オーバーライドするメソッドを宣言するユーザーが必要な作業は複雑がちですが、たとえば、これは、Java と Android に処理を完了できるとどのように。

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

イベントを使用して、c# のコードは同等のコードは次のようになります。

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

上記のメカニズムの両方が Xamarin.Android を使用できることに注意してください。 リスナー インターフェイスを実装して View.SetOnClickListener に接続するもクリック イベントに通常の c# パラダイムのいずれかを使用して作成したデリゲートをアタッチすることができます。

基づく API 要素を作成、リスナー コールバック メソッドが void の戻り値を持つ、 [EventHandler&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/)を委任します。 これらのリスナーの種類の上記の例のようにイベントが生成しました。 ただし、void ではないと非リスナー コールバックが返されます-**ブール**値、イベントおよび EventHandlers は使用されません。 お代わりに、特定のデリゲートのコールバックの署名を生成し、イベントの代わりにプロパティを追加します。 理由は、デリゲートの呼び出しの順序を処理し、処理を返すにです。 この方法は、Xamarin.iOS API で何がミラー化します。

C# の場合は、イベントまたはプロパティのみ自動的に生成される場合は、Android のイベント登録メソッド。

1. `set`プレフィックスなど[*設定*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/)です。

1. `void`型を返します。

1. 1 つだけのパラメーターを受け入れるパラメーターの型がインターフェイス、インターフェイスが 1 つのメソッドとインターフェイスの名前で終わる`Listener`など、 [View.OnClick*リスナー*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/)です。


さらに場合は、リスナー インターフェイス メソッドの戻り値の型を持つ**ブール**の代わりに**void**、生成された*EventArgs*サブクラスには、 *Handled*プロパティです。 値、 *Handled*プロパティの戻り値として使用、*リスナー*メソッド、およびその既定値は`true`します。

たとえば、Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/)メソッドを受け取ります、 [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener)インターフェイス、および[View.OnKeyListener.onKey (ビュー、int、KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/)メソッドには、ブール型の戻り値の型があります。 Xamarin.Android 生成、対応する[View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/)イベントは、これは、 [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/)です。
*KeyEventArgs*クラスにはさらに、 [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/)の戻り値として使用されるプロパティ、 *View.OnKeyListener.onKey()*メソッドです。

他のメソッドとデリゲート ベースの接続を公開する ctors のオーバー ロードを追加する予定があること。 また、複数のコールバックを持つリスナーには、変換しているこれら特定されるので、個々 のコールバックを実装する、妥当なはかどうかを決定するいくつか追加の検査が必要です。 対応するイベントがない場合は、リスナー、C# の場合は、使用する必要がありますがください注目するデリゲートの使用法を持つ可能性がありますと思われるいずれか。 したら、「リスナー」サフィックスが付いていないインターフェイスの一部の変換が、デリゲートの代替恩恵が受けはクリアします。

すべてのリスナー インターフェイスを実装、 [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)リスナー クラスは、このインターフェイスを実装する必要がありますので、バインディングの実装の詳細のためのインターフェイスです。 サブクラスでリスナー インターフェイスを実装することによってこれ行う[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)またはその他の Android アクティビティなどの Java オブジェクトをラップします。


### <a name="runnables"></a>実行可能オブジェクト

Java を利用、 [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/)委任メカニズムを提供するインターフェイスです。 [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/)クラスは、このインターフェイスの重要なコンシューマーです。 Android には、同様の API では、インターフェイスが採用されています。
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/)と[View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable)注目に値する例を示します。

`Runnable`インターフェイスには、1 つの void メソッドが含まれています。 [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/)です。 そのために適しているとして c# でのバインディング、 [System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx)を委任します。 受け入れるオーバー ロードのバインドに提供されており、`Action`消費するすべての API メンバーのパラメーター、`Runnable`では、ネイティブの API など[Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action))と[View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

め、 [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/)いくつかの型がインターフェイスを実装し、そのため、それらを置換ではなく位置でのオーバー ロードが実行可能コードとして直接渡されます。


### <a name="inner-classes"></a>内部クラス

Java は 2 種類の異なる[クラスを入れ子になった](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): 静的クラスと非静的クラスを入れ子にします。

Java の静的な入れ子になったクラスは、C# での入れ子にされた型と同じです。

非静的とも呼ばれるクラスが入れ子になった*内部クラス*が大幅に異なります。 それを囲む型のインスタンスへの暗黙的な参照が含まれてし、(その他の違いのこの概要のスコープ外) の間での静的メンバーを含めることはできません。

バインドと c# を使用する際に、静的な入れ子になったクラスは、通常の入れ子にされた型として扱われます。 内部クラスには、その一方で、次の 2 つの大きな違いがあります。

1. コンス トラクターのパラメーターとしては、外側の型に暗黙の参照を明示的に指定する必要があります。

1. 内側のクラスと内部クラスから継承する場合*必要があります*型の中で入れ子にする基本の内部クラスを含む型から継承して、派生型は、C# の場合と同じ型のコンス トラクターを提供する必要がありますの種類を含むです。


たとえば、 [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/)内部クラス。 内部のクラスであるため、 [WallpaperService.Engine() コンス トラクター](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/)への参照を受け取り、 [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/)インスタンス (比較および Java を対比[WallpaperService.Engine () コンス トラクター](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/)するパラメーターを受け取らない)。

内部クラス、例から派生では、CubeWallpaper.CubeEngine です。

```csharp
class CubeWallpaper : WallpaperService {
        public override WallpaperService.Engine OnCreateEngine ()
        {
                return new CubeEngine (this);
        }

        class CubeEngine : WallpaperService.Engine {
                public CubeEngine (CubeWallpaper s)
                        : base (s)
                {
                }
        }
}
```

注方法`CubeWallpaper.CubeEngine`内で入れ子に`CubeWallpaper`、`CubeWallpaper`の外側のクラスから継承`WallpaperService.Engine`、および`CubeWallpaper.CubeEngine`を宣言する型--を受け取るコンス トラクターを持つ`CubeWallpaper`上記としてすべての場合。


### <a name="interfaces"></a>インターフェイス

Java インターフェイスは、c# から問題が発生する 2 つのメンバーの 3 つのセットを含めることができます。

1. メソッド

1. 種類

1. フィールド


Java インターフェイスは、2 つの型に変換されます。

1. (省略可能) インターフェイス メソッド宣言を含むです。 このインターフェイスでは、同じ名前を Java インターフェイスと*を除く*さらに、'*すれば*' プレフィックス。

1. すべてのフィールドを含む (オプション) の静的クラスは、Java インターフェイス内で宣言されています。

入れ子にされた型は、プレフィックスとしてそれを囲むインターフェイスの名前で、入れ子になった型ではなく、それを囲むインターフェイスの兄弟である「移動」されています。

たとえば、 [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/)インターフェイスです。
*Parcelable*インターフェイスには、メソッド、入れ子にされた型、および定数が含まれています。 *Parcelable*にインターフェイスのメソッドを配置している、 [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/)インターフェイスです。
*Parcelable*インターフェイス定数に配置している、 [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/)型です。 入れ子になった[android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html)と[android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html)型は現在、ジェネリックのサポートの制限のためのバインドこれらがサポートされている場合、対応するとして、 *Android.OS.IParcelableClassLoaderCreator*と*Android.OS.IParcelableCreator*インターフェイスです。 たとえば、入れ子になった[android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html)インターフェイスとしてバインドされている、 [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/)インターフェイスです。


> [!NOTE]
> Xamarin.Android 1.9 から始まり、Java インターフェイス定数は、<em>複製</em>Java の移植を簡略化するためにコードします。 これにより、依存している Java コードの移植を向上させるために[android プロバイダー](http://developer.android.com/reference/android/provider/package-summary.html)定数のインターフェイスです。

上記の種類に加えて、さらに 4 つの変更があります。

1. 定数を格納するには、Java インターフェイスと同じ名前を持つ型が生成されます。

1. またインターフェイス定数を含む型には、実装済みの Java インターフェイスからのすべての定数が含まれています。

1. 定数を含む Java インターフェイスを実装するすべてのクラスは、定数をすべて実装されたインターフェイスを格納する新しい入れ子になった InterfaceConsts 型を取得します。

1. *定数*型は廃止されました。


*Android.os.Parcelable*インターフェイス、つまり、あるようになりましたこと、 [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/)定数を格納する型。 たとえば、 [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR)定数としてバインドしなければ、 [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/)定数の代わりに、として*ParcelableConsts.ContentsFileDescriptor*定数。

インターフェイスを含むその他のインターフェイスを実装している定数をまだ複数の定数を含む、すべての定数の和集合は今すぐ生成されます。 たとえば、 [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html)インターフェイスを実装して、 [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/)インターフェイスです。 ただし、1.9、前に、 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/)型で宣言された定数にアクセスする方法を持たない[Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/)です。
その結果、Java 式*MediaStore.Video.VideoColumns.TITLE* c# の式にバインドする必要があります*MediaStore.Video.MediaColumnsConsts.Title*は読み取らず見つけにくい多数の Java のドキュメントです。 C# の式を同等になります、1.9 で[ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/)です。

さらに、検討、 [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/)を Java を実装する型*Parcelable*インターフェイスです。 インターフェイスを実装することからそのインターフェイス上のすべての定数は"を通してアクセス"バンドル型など*Bundle.CONTENTS_FILE_DESCRIPTOR*完全に有効な Java 式です。
以前は、この式を c# に移植する必要になりますを見ると、どの型を実装するすべてのインターフェイスを見て、 *CONTENTS_FILE_DESCRIPTOR*から付属します。 Xamarin.Android 1.9 以降、定数が含まれている Java インターフェイスを実装するクラスが入れ子になった*InterfaceConsts*継承されたインターフェイスのすべての定数を含んでいる型。 これにより、変換*Bundle.CONTENTS_FILE_DESCRIPTOR*に[ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/)です。

最後に、型は、*定数*などサフィックス*Android.OS.ParcelableConsts* Obsolete、新しく導入された InterfaceConsts 以外の入れ子になった型ようになりました。 Xamarin.Android 3.0 では削除されます。


## <a name="resources"></a>リソース

イメージ、レイアウトの説明、バイナリの blob および文字列ディクショナリとして、アプリケーションに含めることができます[リソース ファイル](http://developer.android.com/guide/topics/resources/providing-resources.html)です。
さまざまな Android Api の設計において[リソース Id で動作](http://developer.android.com/guide/topics/resources/accessing-resources.html)イメージ扱うのではなく文字列またはバイナリの blob の直接です。

たとえば、サンプル Android アプリをユーザー インターフェイスのレイアウトを含む ( `main.axml`)、国際化テーブル文字列 ( `strings.xml`) といくつかのアイコン ( `drawable-*/icon.png`)、「リソース」アプリケーションのディレクトリに、そのリソースを保持するが。

    Resources/
        drawable-hdpi/
            icon.png

        drawable-ldpi/
            icon.png

        drawable-mdpi/
            icon.png

        layout/
            main.axml

        values/
            strings.xml

ネイティブ Android Api は、ファイル名を直接操作しないでが、リソース Id の代わりに動作します。 リソースを使用する Android アプリケーションをコンパイルするときに、ビルド システムは配布用のリソースをパッケージ化しと呼ばれるクラスを生成`Resource`ずつ含まれるリソースのために、トークンを格納しています。 たとえば、上のリソース レイアウトでは、これはどのような R クラスが公開されます。

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

使用して`Resource.Drawable.icon`参照に、`drawable/icon.png`ファイル、または`Resource.Layout.main`参照に、`layout/main.xml`ファイル、または`Resource.String.first_string`ディクショナリ ファイル内の最初の文字列を参照する`values/strings.xml`です。


## <a name="constants-and-enumerations"></a>定数と列挙体

ネイティブ Android Api を実行したり、int、int の意味を決定する定数フィールドにマップする必要がありますを返す多くの方法があります。 これらのメソッドを使用するのには、ユーザーは、定数が適切な値。 これは理想的な未満を表示するドキュメントを参照して必要があります。

たとえば、 [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))です。

このような場合にで .NET の列挙体に関連する定数をグループ化し、代わりに、列挙する方法を再マップするよう努めてです。
これにより、潜在的な値の IntelliSense の選択範囲を提供することはできます。

上記の例になります: [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/))。

これは、定数が互いに非常に手動での処理、どの Api の定数を使用することに注意してください。 列挙体としてより適切になる API の定数を使用するためのバグを送信してください。
