---
title: Xamarin.Android の API の設計原則
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: e762a286069d5ef1db90f3c45808eee0a7a04a7f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60954286"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin.Android の API の設計原則


## <a name="overview"></a>概要

Mono の一部である基本クラス ライブラリのコアだけでなく Xamarin.Android Mono とネイティブ Android アプリケーションを作成するためにさまざまな Android API のバインドに付属します。

Xamarin.Android のコアがありますが、相互運用機能のエンジン、Java の世界中でそのブリッジ、C# の世界と、C# または他の .NET 言語からの Java API にアクセス権を持つ開発者。


## <a name="design-principles"></a>設計の原則

Xamarin.Android バインドのデザイン原則をいくつか

-  準拠している、 [.NET Framework デザイン ガイドライン](https://docs.microsoft.com/dotnet/standard/design-guidelines/)します。

-  開発者は Java クラスのサブクラスを使用できます。

-  サブクラスは、C# の標準的なコンストラクトで動作します。

-  既存のクラスから派生します。

-  チェーンに基底コンス トラクターを呼び出します。

-  メソッドのオーバーライドは、C# の上書きのシステムで行う必要があります。

-  簡単に、共通の Java タスクとハードの Java タスクできます。

-  C# のプロパティとして JavaBean プロパティを公開します。

-  厳密に型指定された API を公開します。

    - タイプ セーフを向上します。

    - 実行時エラーを最小限に抑えます。

    - 戻り値の型には、IDE の intellisense を取得します。

    - IDE のポップアップのドキュメントではできます。

-  API の IDE で探索をお勧めします。

    - Java Classlib を最小限に抑える露出するフレームワークの代替手段を利用します。

    - 適切なと該当する場合は、単一メソッドのインターフェイスではなく C# のデリゲート (ラムダ、匿名メソッドと System.Delegate) を公開します。

    - 任意の Java ライブラリを呼び出すメカニズムを提供 ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/))。


## <a name="assemblies"></a>アセンブリ

Xamarin.Android には形成するアセンブリの数値が含まれています、 *MonoMobile プロファイル*します。 [アセンブリ](~/cross-platform/internals/available-assemblies.md)ページには詳細についてはします。

Android のプラットフォームへのバインドに含まれる、`Mono.Android.dll`アセンブリ。 このアセンブリには、使用の Android API 全体のバインドと Android ランタイム VM との通信が含まれています。


## <a name="binding-design"></a>バインドのデザイン


### <a name="collections"></a>コレクション

Android の API は、リスト、セット、およびマップを提供する広範な java.util コレクションを使用します。 使用してこれらの要素を公開しています、 [System.Collections.Generic](xref:System.Collections.Generic)バインディング内のインターフェイス。 基本的なマッピングは次のとおりです。

-   [java.util.Set<E> ](https://developer.android.com/reference/java/util/Set.html)システム型にマップされます[ICollection<T>](xref:System.Collections.Generic.ICollection`1)、ヘルパー クラス[Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/)します。

-   [java.util.List<E> ](https://developer.android.com/reference/java/util/List.html)システム型にマップされます[IList<T>](xref:System.Collections.Generic.IList`1)、ヘルパー クラス[Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/)します。

-   [< K, V > java.util.Map](https://developer.android.com/reference/java/util/Map.html)システム型にマップされます[IDictionary < TKey, TValue >](xref:System.Collections.Generic.IDictionary`2)、ヘルパー クラス[Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/)します。

-   [java.util.Collection<E> ](https://developer.android.com/reference/java/util/Collection.html)システム型にマップされます[ICollection<T>](xref:System.Collections.Generic.ICollection`1)、ヘルパー クラス[Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/)します。

これらの型のマーシャ リングが高速 copyless を容易にするヘルパー クラスを用意しました。 などの可能な限り、これらのフレームワークが提供する実装ではなくコレクションを指定された使用をお勧め、 [ `List<T>` ](xref:System.Collections.Generic.List`1)または[ `Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2)します。 [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/)実装がネイティブ Java コレクションを内部的に使用し、ので不要とネイティブのコレクションからの Android API のメンバーに渡すときにコピーします。

任意のインターフェイスの実装をそのインターフェイスを受け入れる Android メソッドに渡すなどを渡すことができます、`List<int>`を[ArrayAdapter&lt;int&gt;(コンテキスト、int、IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)コンス トラクター。 *ただし*、すべての実装の*を除く*Android.Runtime 実装では、このためには*コピー* Android ランタイム VM に Mono VM からのリスト。 リストが以降の場合は、Android ランタイム内で変更 (例: を呼び出して、 [ArrayAdapter&lt;T&gt;します。Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/)メソッド)、それらの変更*されません*マネージ コードに表示されます。 場合、`JavaList<int>`が使用すると、それらの変更が表示されます。

もじって、コレクション インターフェイスの実装では、*いない*上記のいずれかにある**ヘルパー クラス**[In] es がマーシャ リングのみ。

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

Java のメソッドは、該当する場合のプロパティに変換されます。

-  Java のメソッド ペア`T getFoo()`と`void setFoo(T)`に変換されます、`Foo`プロパティ。 例:[Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/)します。

-  Java メソッド`getFoo()`読み取り専用 Foo プロパティに変換されます。 例:[Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/)します。

-  設定専用のプロパティは生成されません。

-  プロパティは、*いない*配列プロパティの型である場合に生成します。



### <a name="events-and-listeners"></a>リスナーとイベント

Android API は、Java の上に構築されており、そのコンポーネントがイベント リスナーをフックするための Java のパターンに従います。 このパターンは、ユーザーが匿名のクラスを作成し、オーバーライドするメソッドを宣言する必要がある面倒になる傾向があります、たとえば、これは Java と Android で処理を行うことが方法。

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

イベントを使用して C# での同等のコードは次のようになります。

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

上記のメカニズムの両方が Xamarin.Android で使用可能なことに注意してください。 リスナー インターフェイスを実装し、View.SetOnClickListener でアタッチまたは Click イベントに通常の C# パラダイムのいずれかを使用して作成されたデリゲートをアタッチすることができます。

基づく API 要素を作成しますリスナー コールバック メソッドが void の戻り値の場合、 [EventHandler&lt;TEventArgs&gt; ](xref:System.EventHandler`1)を委任します。 これらのリスナーの種類は、上記の例のようなイベントが生成されます。 ただし、非 void a と非リスナー コールバックが返されます-**ブール**値、イベント、EventHandlers は使用されません。 私たちは代わりに、コールバックのシグネチャの特定のデリゲートを生成し、イベントの代わりにプロパティを追加します。 理由は、処理をデリゲートの呼び出しの順序で処理です。 このアプローチでは、Xamarin.iOS API をミラー化します。

イベント (C#) またはプロパティのみ自動的に生成される場合は、Android のイベント登録メソッド。

1. `set`プレフィックス、例: [*設定*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/)します。

1. `void`型を返します。

1. 1 つだけのパラメーターを受け入れるパラメーターの型がインターフェイス、インターフェイスが 1 つのメソッドおよびインターフェイスの名前で終わる`Listener`、例: [View.OnClick*リスナー*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/)します。


さらに場合は、リスナー インターフェイス メソッドの戻り値の型を持つ**ブール**の代わりに**void**、し、生成された*EventArgs*サブクラスには、 *Handled*プロパティ。 値、 *Handled*プロパティの戻り値として使用されます、*リスナー*メソッド、およびその既定値は`true`します。

Android ではたとえば、 [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/)メソッドは、 [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener)インターフェイス、および[View.OnKeyListener.onKey (ビュー、int、KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/)メソッドでは、ブール型の戻り値の型があります。 Xamarin.Android の生成、対応する[View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/)は、そのイベントを[EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/)します。
*KeyEventArgs*クラスにはさらに、 [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/)の戻り値として使用されるプロパティ、 *View.OnKeyListener.onKey()* メソッド。

他のメソッドとデリゲート ベースの接続を公開する ctors のオーバー ロードを追加する予定です。 また、複数のコールバックを持つリスナーには、いくつか追加の検査変換これら特定されるため、個々 のコールバックを実装する、妥当ながあるか判断が必要です。 対応するイベントがない場合は、リスナーは、C# で使用する必要がありますが、注目するデリゲートの使用量ができたと考えられるすべてを表示してください。 行った「リスナー」サフィックスが付いていないインターフェイスの一部の変換と、それらが、デリゲートの代替のメリットは明らかでした。

すべてのリスナー インターフェイスを実装します [`Android.Runtime.IJavaObject`](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)
リスナーのクラスは、このインターフェイスを実装する必要がありますので、バインドの実装の詳細のためのインターフェイスです。 これを行うのサブクラスで、リスナー インターフェイスを実装する[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)またはその他の Android アクティビティなどの Java オブジェクトをラップします。


### <a name="runnables"></a>実行可能オブジェクト

Java を使用して、 [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/)委任メカニズムを提供するインターフェイス。 [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/)クラスは、このインターフェイスの注目すべきコンシューマー。 Android は、API にも同様のインターフェイスが使用されます。
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/)と[View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable)注目すべき例があります。

`Runnable`インターフェイスには、1 つの void メソッドが含まれています。 [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/)します。 そのために適していると C# でのバインディングを[System.Action](xref:System.Action)を委任します。 バインディングを受け入れるオーバー ロードが用意されています、`Action`パラメーターを使用するすべての API メンバーを`Runnable`ネイティブの API の例: [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action))と[View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

まま、 [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/)いくつかの種類は、インターフェイスを実装し、そのため、それらを置き換えるのではなく位置でのオーバー ロードが直接の実行可能オブジェクトとして渡されます。


### <a name="inner-classes"></a>内部クラス

Java が 2 つのさまざまな種類の[クラスを入れ子になった](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): 静的クラスと非静的クラスを入れ子にします。

Java の静的な入れ子になったクラスは、C# の入れ子になった型と同じです。

非静的とも呼ばれるクラスが入れ子になった*内部クラス*、大幅に異なります。 その外側の型のインスタンスへの暗黙的な参照が含まれていて (この概要の範囲外の相違点) の間での静的メンバーを含めることはできません。

バインドと C# を使用する際に、静的な入れ子になったクラスが入れ子になった、通常の型として扱われます。 内部のクラスには、2 つの大きな違いがある一方で。

1. コンテナーの型に暗黙の参照は、コンス トラクター パラメーターとして明示的に指定する必要があります。

1. 内側のクラスと内部クラスから継承する場合*する必要があります*型の中で入れ子にする基本の内部クラスを含む型から継承して、派生型は、C# と同じ型のコンス トラクターを指定する必要がありますの種類を格納しています。


たとえば、 [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/)内部クラス。 内部のクラスであるため、 [WallpaperService.Engine() コンス トラクター](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/)への参照を受け取り、 [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/)インスタンス (比較して、java [WallpaperService.Engine () コンス トラクター](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/)するパラメーターを受け取らない)。

内部クラスから派生した、例では、CubeWallpaper.CubeEngine です。

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

注方法`CubeWallpaper.CubeEngine`内で入れ子になって`CubeWallpaper`、`CubeWallpaper`の外側のクラスから継承`WallpaperService.Engine`、および`CubeWallpaper.CubeEngine`--宣言する型を受け取るコンス トラクターを持つ`CubeWallpaper`としてすべて上記の場合。


### <a name="interfaces"></a>インターフェイス

Java インターフェイスは、C# から問題が発生する 2 つのメンバーの 3 つのセットを含めることができます。

1. メソッド

1. 種類

1. フィールド


Java インターフェイスは、2 つの型に変換されます。

1. メソッド宣言を含む (省略可能) のインターフェイス。 このインターフェイスでは、同じ名前を Java のインターフェイスと*を除く*もが、'*は*' プレフィックス。

1. (省略可能) の静的クラスが任意のフィールドを含む Java インターフェイス内で宣言されています。

入れ子にされた型は「再配置」をプレフィックスとしてそれを囲むインターフェイス名と、入れ子にされた型ではなく、それを囲むインターフェイスの兄弟にします。

たとえば、 [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/)インターフェイス。
*Parcelable*インターフェイスには、メソッド、入れ子にされた型、および定数が含まれています。 *Parcelable*インターフェイス メソッドに格納され、 [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/)インターフェイス。
*Parcelable*インターフェイス定数がまとめて、 [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/)型。 入れ子になった[android.os.Parcelable.ClassLoaderCreator <t> </t> ](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html)と[android.os.Parcelable.Creator <t> </t> ](https://developer.android.com/reference/android/os/Parcelable.Creator.html)型では現在ありませんは、ジェネリックのサポートの制限のためのバインドとして記述すると、それらがサポートされている場合、 *Android.OS.IParcelableClassLoaderCreator*と*Android.OS.IParcelableCreator*インターフェイス。 たとえば、入れ子になった[android.os.IBinder.DeathRecpient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html)インターフェイスとしてバインドされている、 [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/)インターフェイス。


> [!NOTE]
> Java インターフェイスの定数は、Xamarin.Android 1.9 以降、<em>複製</em>Java の移植を簡略化するためにコードします。 これにより、依存している移植の Java コードを改良する[android プロバイダー](https://developer.android.com/reference/android/provider/package-summary.html)インターフェイスの定数。

上記の種類に加えて、さらに 4 つの変更があります。

1. 定数を含む Java インターフェイスと同じ名前を持つ型が生成されます。

1. また、インターフェイス、定数を含む型には、実装済みの Java インターフェイスから取得したすべての定数が含まれます。

1. 定数を含む Java インターフェイスを実装するすべてのクラスは、実装されたインターフェイスのすべての定数を含む新しい入れ子になった InterfaceConsts 型を取得します。

1. *Consts*タイプは廃止されました。


*Android.os.Parcelable*インターフェイス、つまりがここであること、 [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/)定数を格納する型。 たとえば、 [Parcelable.CONTENTS_FILE_DESCRIPTOR](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR)定数としてバインドされる、 [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) としての代わりに定数*ParcelableConsts.ContentsFileDescriptor*定数。

定数を格納している他のインターフェイスを実装しているまだは定数を格納しているインターフェイスでは、すべての定数の和集合が生成されるようになりました。 たとえば、 [android.provider.MediaStore.Video.VideoColumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html)インターフェイスの実装、 [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/)インターフェイス。 1.9 より前、ただし、 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/)型で宣言された定数にアクセスする方法を持たない[Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/)します。
その結果、Java 式*MediaStore.Video.VideoColumns.TITLE* C# の式にバインドする必要がある*MediaStore.Video.MediaColumnsConsts.Title*は読まず見つけにくい多数の Java のドキュメント。 同等の C# 式がある、1.9 で[ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/)します。

さらに、検討してください、 [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) 、Java の実装の種類*Parcelable*インターフェイス。 そのインターフェイスのすべての定数は例:「から」バンドルの種類、アクセスできる、インターフェイスを実装しているため*Bundle.CONTENTS_FILE_DESCRIPTOR* Java 式を完全に有効です。
以前は、この式に移植するC#種類に実装されているすべてのインターフェイスを確認する必要があります、 *CONTENTS_FILE_DESCRIPTOR*から。 Xamarin.Android 1.9 以降、定数が含まれている Java インターフェイスを実装するクラスが入れ子になった*InterfaceConsts*型、継承されたインターフェイスのすべての定数が含まれます。 変換することにより、この*Bundle.CONTENTS_FILE_DESCRIPTOR*に[ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/)します。

最後に、型、 *Consts*などサフィックス*Android.OS.ParcelableConsts*は古い形式には、新しく導入された InterfaceConsts 以外の入れ子にされた型。 Xamarin.Android 3.0 では削除されます。


## <a name="resources"></a>リソース

として、アプリケーションでイメージ、レイアウトの説明、バイナリの blob および文字列の辞書を含めることが[リソース ファイル](https://developer.android.com/guide/topics/resources/providing-resources.html)します。
さまざまな Android API の設計において[リソース Id に対して](https://developer.android.com/guide/topics/resources/accessing-resources.html)イメージを処理する、代わりに文字列またはバイナリ blob 直接します。

たとえば、サンプルする Android アプリケーションをユーザー インターフェイスのレイアウトが含まれています ( `main.axml`)、国際化テーブル文字列 ( `strings.xml`) と一部のアイコン ( `drawable-*/icon.png`) は、アプリケーションの"Resources"ディレクトリにそのリソースを保持します。

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

ネイティブ Android API は、ファイル名を直接操作しないが、リソース Id の代わりに動作します。 リソースを使用する Android アプリケーションをコンパイルするときに、ビルド システムは配布用のリソースをパッケージ化しと呼ばれるクラスを生成`Resource`それぞれに含まれるリソースのトークンを格納しています。 たとえば、上記のリソース レイアウトのこれは、R クラスが公開されます。

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

使用して`Resource.Drawable.icon`参照に、`drawable/icon.png`ファイル、または`Resource.Layout.main`参照に、`layout/main.xml`ファイル、または`Resource.String.first_string`ディクショナリ ファイル内の最初の文字列を参照する`values/strings.xml`します。


## <a name="constants-and-enumerations"></a>定数と列挙体

ネイティブ Android API がある多くのメソッドを返したり、int、int の意味を決定する定数フィールドにマップする必要があります。 これらのメソッドを使用するには、ユーザーは、定数が適切な値より小さいに最適なドキュメントを参照して必要があります。

たとえば、 [Activity.requestWindowFeature (int featureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))します。

このような場合に、.NET 列挙に関連する定数をグループ化し、代わりに、列挙する方法を再マップするよう努めています。
これにより、潜在的な値の IntelliSense の選択を提供できなくなっています。

上記の例になります。[Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)します。

これは、定数が属するを把握する非常に手動のプロセス、API の定数を使用することに注意してください。 列挙体として表されたより適切になる API で使用される任意の定数のバグを報告してください。
