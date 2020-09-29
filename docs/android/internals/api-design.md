---
title: Xamarin. Android API 設計の原則
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: bb9dadd388a514f18abdaf6e6db6d76f0be99f86
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454248"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin. Android API 設計の原則

Mono に含まれるコア基本クラスライブラリに加えて、Xamarin にはさまざまな Android Api のバインドが付属しており、開発者は Mono を使用してネイティブの Android アプリケーションを作成できます。

Xamarin Android の中核にある相互運用エンジンは、C# の世界を Java の世界に橋渡しし、開発者が C# またはその他の .NET 言語から Java Api にアクセスできるようにします。

## <a name="design-principles"></a>設計原則

これらは、Xamarin Android のバインドの設計原則の一部です。

- [.NET Framework の設計ガイドライン](/dotnet/standard/design-guidelines/)に準拠します。

- 開発者が Java クラスをサブクラス化できるようにします。

- サブクラスは C# の標準コンストラクトを使用する必要があります。

- 既存のクラスから派生します。

- チェーンする基本コンストラクターを呼び出します。

- メソッドのオーバーライドは、C# のオーバーライドシステムを使用して行う必要があります。

- 一般的な Java タスクを簡単にし、困難な Java タスクにすることができます。

- JavaBean プロパティを C# プロパティとして公開します。

- 厳密に型指定された API を公開します。

  - タイプセーフを増やします。

  - 実行時エラーを最小化します。

  - 戻り値の型に対して IDE intellisense を取得します。

  - IDE ポップアップドキュメントを使用できるようにします。

- API の IDE 内でいろいろ試すことを奨励する

  - フレームワークの代替手段を利用して、Java Classlib の露出を最小限に抑えます。

  - 必要に応じて、単一メソッドインターフェイスではなく、C# デリゲート (ラムダ、匿名メソッド、および system.object) を公開します。

  - 任意の Java ライブラリ ( [Android. Runtime. jの](xref:Android.Runtime.JNIEnv)) を呼び出すための機構を提供します。

## <a name="assemblies"></a>アセンブリ

Xamarin Android には、 *モノモバイルプロファイル*を構成するさまざまなアセンブリが含まれています。 詳細については、[アセンブリ](~/cross-platform/internals/available-assemblies.md)に関するページを参照してください。

Android プラットフォームへのバインドは、アセンブリに含まれてい `Mono.Android.dll` ます。 このアセンブリには、Android Api を使用し、Android ランタイム VM と通信するためのバインド全体が含まれています。

## <a name="binding-design"></a>バインディング デザイン

### <a name="collections"></a>コレクション

Android Api は、java の util コレクションを広範囲にわたって使用して、リスト、セット、およびマップを提供します。 これらの要素は、バインディングの [system.object インターフェイスを](xref:System.Collections.Generic) 使用して公開されています。 基本的なマッピングは次のとおりです。

- [java. util. マップ \<E> ](https://developer.android.com/reference/java/util/Set.html)をシステム型[ICollection \<T> ](xref:System.Collections.Generic.ICollection`1), helper クラス[JavaSet \<T> ](xref:Android.Runtime.JavaSet`1)に設定します。

- JavaList は、system 型[IList \<T> ](xref:System.Collections.Generic.IList`1)、ヘルパークラス、およびにマップ[されます。 \<T> ](xref:Android.Runtime.JavaList`1) [ \<E> ](https://developer.android.com/reference/java/util/List.html)

- TValue [<K, V>](https://developer.android.com/reference/java/util/Map.html)マップをシステム型[IDictionary<TKey,>](xref:System.Collections.Generic.IDictionary`2), Helper クラス[JavaDictionary<K, v>](xref:Android.Runtime.JavaDictionary`2)にマップします。

- JavaCollection は、システム型[ICollection \<T> ](xref:System.Collections.Generic.ICollection`1), helper クラスにマップされ[ます。 \<T> ](xref:Android.Runtime.JavaCollection`1) [ \<E> ](https://developer.android.com/reference/java/util/Collection.html)

これらの型のより高速なコピーを容易にするヘルパークラスが用意されています。 可能であれば、またはのようなフレームワークの実装ではなく、これらの提供されたコレクションを使用することをお勧めし [`List<T>`](xref:System.Collections.Generic.List`1) [`Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2) ます。 [Android のランタイム](xref:Android.Runtime)実装はネイティブ Java コレクションを内部的に使用するため、android API メンバーに渡すときにネイティブコレクションとの間でコピーを行う必要はありません。

インターフェイスの実装は、そのインターフェイスを受け入れる Android メソッドに渡すことができます。たとえば、を `List<int>` [arrayadapter &lt; int &gt; (Context, int, IList &lt; int &gt; )](xref:Android.Widget.ArrayAdapter`1) コンストラクターに渡します。 *ただし*、Android のランタイム実装を *除く* すべての実装については、Mono VM から android ランタイム VM にリストを *コピー* する必要があります。 リストが Android ランタイム内で後で変更された場合 (たとえば、 [Arrayadapter T を呼び出した場合)。 &lt; &gt;Add (T)](xref:Android.Widget.ArrayAdapter`1.Add*) メソッド)。これらの変更はマネージコードでは表示され *ません* 。 を `JavaList<int>` 使用した場合は、それらの変更が表示されます。

選択肢では、上記の**ヘルパークラス**のいずれでも*ない*コレクションインターフェイスの実装では、[in] のみがマーシャリングされます。

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

Java メソッドは、必要に応じてプロパティに変換されます。

- Java メソッドのペア `T getFoo()` と `void setFoo(T)` は、プロパティに変換され `Foo` ます。 例: [Activity. インテント](xref:Android.App.Activity.Intent)。

- Java メソッド `getFoo()` は、読み取り専用の Foo プロパティに変換されます。 例: [PackageName](xref:Android.Content.Context.PackageName)。

- セットのみのプロパティは生成されません。

- プロパティの型が配列の場合、プロパティは生成され *ません* 。

### <a name="events-and-listeners"></a>イベントとリスナー

Android Api は Java 上に構築され、そのコンポーネントは、イベントリスナーをフックするための Java パターンに従います。 このパターンは、ユーザーが匿名クラスを作成し、オーバーライドするメソッドを宣言する必要があるため、煩雑になる傾向があります。たとえば、Android を使用した Android では、次のような処理が行われます。

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

イベントを使用した C# の同等のコードは、次のようになります。

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

これらのメカニズムはどちらも、Xamarin Android で使用できることに注意してください。 リスナーインターフェイスを実装し、ビューを SetOnClickListener でアタッチできます。また、通常の C# パラダイムを使用して作成されたデリゲートをクリックイベントにアタッチすることもできます。

リスナーのコールバックメソッドに void が返された場合は、 [EventHandler &lt; teventargs &gt; ](xref:System.EventHandler`1)デリゲートに基づいて API 要素を作成します。 これらのリスナー型について、上記の例のようなイベントを生成します。 ただし、リスナーのコールバックが void 以外の値と非 **ブール** 値を返す場合、Events と EventHandlers は使用されません。 代わりに、コールバックのシグネチャに対して特定のデリゲートを生成し、イベントの代わりにプロパティを追加します。 その理由は、デリゲート呼び出し順序と戻り処理を処理することです。 このアプローチは、Xamarin. iOS API で行われる処理を反映しています。

C# のイベントまたはプロパティは、Android イベント登録メソッドの場合にのみ自動的に生成されます。

1. には `set` プレフィックスがあります。たとえば、 [onclicklistener を*設定*](xref:Android.Views.View.SetOnClickListener*)します。

1. に `void` 戻り値の型があります。

1. は、パラメーターを1つだけ受け取ります。パラメーターの型はインターフェイスで、インターフェイスにはメソッドが1つだけあり、インターフェイス名はのようになります `Listener` (例: [OnClick *Listener*](xref:Android.Views.View.IOnClickListener))。

さらに、リスナーインターフェイスメソッドの戻り値の型が**void**ではなく**ブール**型である場合、生成された*EventArgs*サブクラスには、*処理*されたプロパティが含まれます。 *処理*されたプロパティの値は、*リスナー*メソッドの戻り値として使用され、既定でに `true` なります。

たとえば、Android ビューの [setonkeylistener ()](xref:Android.Views.View.SetOnKeyListener*) メソッドは、 [ビュー. onkeylistener](xref:Android.Views.View.IOnKeyListener) インターフェイスを受け取り、 [onKey (View, int, keyevent)](xref:Android.Views.View.IOnKeyListener.OnKey*) メソッドにはブール型の戻り値の型があります。 Xamarin では[、対応する](xref:Android.Views.View.KeyPress)system.windows.forms.keyeventargs.handled [ &lt; &gt; ](xref:Android.Views.View.KeyEventArgs)イベントが生成されます。これは、EventHandler ビューです。
さらに、 *system.windows.forms.keyeventargs.handled*クラスには、system.windows.forms.keyeventargs.handled *()* メソッドの戻り値として使用される[View.KeyEventArgs.Handled](xref:Android.Views.View.KeyEventArgs.Handled)プロパティが含まれています。

デリゲートベースの接続を公開するために、他のメソッドのオーバーロードと ctor を追加する予定です。 また、複数のコールバックを持つリスナーは、個々のコールバックの実装が妥当であるかどうかを判断するために追加の検査を必要とするので、識別されたとおりに変換します。 対応するイベントがない場合は、リスナーを C# で使用する必要がありますが、注意を払ってデリゲートを使用できると考えてください。 また、"リスナー" サフィックスが付いていないインターフェイスは、デリゲートの代わりに使用できるという点で、何らかの変換も行われています。

すべてのリスナーインターフェイスは、 [`Android.Runtime.IJavaObject`](xref:Android.Runtime.IJavaObject)
インターフェイス。バインディングの実装の詳細があるため、リスナークラスはこのインターフェイスを実装する必要があります。 これを行うには、 [java](xref:Java.Lang.Object) のサブクラスまたはその他のラップされた java オブジェクト (Android アクティビティなど) にリスナーインターフェイスを実装します。

### <a name="runnables"></a>Runnables

Java では、 [java](xref:Java.Lang.Runnable) の実行可能インターフェイスを利用して委任メカニズムを提供します。 [.Java](xref:Java.Lang.Thread)クラスは、このインターフェイスの注目すべきコンシューマーです。 Android は API でもインターフェイスを採用しています。
[Activity. runOnUiThread ()](xref:Android.App.Activity.RunOnUiThread*) と [View.post ()](xref:Android.Views.View.Post*) は、注目すべき例です。

この `Runnable` インターフェイスには、1つの void メソッド、 [run ()](xref:Java.Lang.Runnable.Run)が含まれています。 このため、C# では、 [System. Action](xref:System.Action) デリゲートとしてバインドすることができます。 `Action`ネイティブ api でを使用するすべての api メンバー (たとえば、 `Runnable` [Activity. RunOnUiThread ()](xref:Android.App.Activity.RunOnUiThread*)および[View.Post ())](xref:Android.Views.View.Post*)のパラメーターを受け取るオーバーロードをバインドに提供しています。

いくつかの型がインターフェイスを実装しているため、runnables として直接渡すことができるため、 [irunnable](xref:Java.Lang.IRunnable) 可能なオーバーロードを代わりに使用したままにしました。

### <a name="inner-classes"></a>内部クラス

Java には、静的な入れ子になったクラスと非静的クラスの2種類の [入れ子になっ](https://download.oracle.com/javase/tutorial/java/javaOO/nested.html)たクラスがあります。

Java の静的入れ子になったクラスは、C# の入れ子になった型と同じです。

静的でない入れ子になったクラス ( *内部クラス*とも呼ばれます) は、大幅に異なります。 外側の型のインスタンスへの暗黙的な参照が含まれており、静的メンバーを含むことはできません (この概要の範囲外の違いがあります)。

バインディングと C# を使用する場合、入れ子になった静的クラスは通常の入れ子になった型として扱われます。 内部クラスには、次の2つの大きな違いがあります。

1. 含んでいる型への暗黙的な参照は、コンストラクターのパラメーターとして明示的に指定する必要があります。

1. 内部クラスから継承する場合、内部クラスは、基本内部クラスの包含型から継承する型の中で入れ子にする *必要があり* ます。また、派生型は、型を含む C# と同じ型のコンストラクターを提供する必要があります。

たとえば、 [WallpaperService](xref:Android.Service.Wallpaper.WallpaperService.Engine) の内部クラスを考えてみます。 これは内部クラスであるため、 [WallpaperService () コンストラクター](xref:Android.Service.Wallpaper.WallpaperService.Engine#ctor) は [WallpaperService](xref:Android.Service.Wallpaper.WallpaperService) インスタンスへの参照を受け取ります (比較対照は、パラメーターを取らない Java WallpaperService () コンストラクターと比較してください)。

内部クラスの派生の例として、CubeWallpaper があります。 Cubewallpaper:

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

`CubeWallpaper.CubeEngine`が内で入れ子になっていること `CubeWallpaper` 、 `CubeWallpaper` の外側のクラスから継承されていること、 `WallpaperService.Engine` および `CubeWallpaper.CubeEngine` 宣言型を受け取るコンストラクターがあることに注意し `CubeWallpaper` てください (この例では、上記で指定したすべて)。

### <a name="interfaces"></a>インターフェイス

Java インターフェイスには3つのメンバーのセットを含めることができ、そのうちの2つは C# の問題を引き起こします。

1. メソッド

1. 型

1. フィールド

Java インターフェイスは、次の2つの型に変換されます。

1. メソッド宣言を含む (省略可能な) インターフェイス。 このインターフェイスには、Java インターフェイスと同じ名前が付けられます。 *ただし* 、' *I* ' プレフィックスも含まれます。

1. Java インターフェイス内で宣言された任意のフィールドを含む (省略可能な) 静的クラス。

入れ子になった型は、入れ子になった型ではなく、外側のインターフェイスの兄弟となるように "再配置" されます。外側のインターフェイス名はプレフィックスとして使用されます。

たとえば、 [Parcelable](xref:Android.OS.Parcelable) インターフェイスを考えてみます。
*Parcelable*インターフェイスには、メソッド、入れ子にされた型、および定数が含まれています。 *Parcelable*インターフェイスメソッドは、 [IParcelable](xref:Android.OS.IParcelable)インターフェイスに配置されます。
*Parcelable*インターフェイス定数は[ParcelableConsts](xref:Android.OS.ParcelableConsts)型に配置されます。 入れ子になった[Parcelable と ClassLoaderCreator \<T> ](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html)の種類は、現在、ジェネリックのサポートの制限のためにバインドされていません。サポートされている場合は、 *IParcelableClassLoaderCreator*および[ \<T> Parcelable](https://developer.android.com/reference/android/os/Parcelable.Creator.html)インターフェイスとして表示され*ます。* たとえば、入れ子になった [DeathRecipient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) インターフェイスは、 [IBinderDeathRecipient](xref:Android.OS.IBinderDeathRecipient) インターフェイスとしてバインドされます。

> [!NOTE]
> Xamarin Android 1.9 以降では、java コードの移植を簡素化するために Java インターフェイス定数が _複製_ されています。 これは、 [android プロバイダー](https://developer.android.com/reference/android/provider/package-summary.html) のインターフェイス定数に依存する Java コードの移植性を向上させるのに役立ちます。

上記の型に加えて、次の4つの変更点があります。

1. Java インターフェイスと同じ名前を持つ型は、定数を含むように生成されます。

1. インターフェイス定数を含む型には、実装された Java インターフェイスから取得されるすべての定数も含まれます。

1. 定数を含む Java インターフェイスを実装するすべてのクラスは、実装されているすべてのインターフェイスの定数を含む新しい入れ子になった InterfaceConsts 型を取得します。

1. *料金*型は互換性のために残されています。

*Parcelable*インターフェイスの場合、これは定数を格納するための[*Parcelable*](xref:Android.OS.Parcelable)型となることを意味します。 たとえば、 [CONTENTS_FILE_DESCRIPTOR Parcelable](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR)定数は、 *ParcelableConsts*定数としてではなく、 [*ContentsFileDescriptor*](xref:Android.OS.Parcelable.ContentsFileDescriptor)定数としてバインドされます。

さらに多くの定数を含む他のインターフェイスを実装する定数を含むインターフェイスの場合は、すべての定数の和集合が生成されるようになりました。 たとえば、 [MediaColumns](xref:Android.Provider.MediaStore.MediaColumns)インターフェイスには、android............... [videocolumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) . ただし、1.9 より前では、 [VideoColumnsConsts](xref:Android.Provider.MediaStore.Video.VideoColumnsConsts) 型には、 [MediaColumnsConsts](xref:Android.Provider.MediaStore.MediaColumnsConsts)で宣言された定数にアクセスする方法がありませんでした。
その結果、java の式*Mediastore*にバインドする必要があります。この式は、多くの java ドキュメント*MediaStore.Video.MediaColumnsConsts.Title*を読み取らずに検出するのは困難ですが、このようにしてください。 1.9 では、同等の C# 式は [*Mediastore. Video columns. Title*](xref:Android.Provider.MediaStore.Video.VideoColumns.Title)です。

さらに、Java *Parcelable*インターフェイスを実装する[Android. os バンドル](xref:Android.OS.Bundle)の種類を考えてみましょう。 インターフェイスを実装するため、そのインターフェイスのすべての定数は、バンドルの種類 (バンドルなど) を介して "アクセス可能" になり *ます。 CONTENTS_FILE_DESCRIPTOR* は完全に有効な Java 式です。
以前は、この式を C# に移植するには、実装されているすべてのインターフェイスを調べて、 *CONTENTS_FILE_DESCRIPTOR* の元の型を確認する必要がありました。 Xamarin Android 1.9 以降では、定数を含む Java インターフェイスを実装するクラスには、継承されたすべてのインターフェイス定数を含む *InterfaceConsts* 型が入れ子になっています。 これにより、 *バンドルの* 変換が可能になります。 [*InterfaceConsts. ContentsFileDescriptor*](xref:Android.OS.Bundle.InterfaceConsts.ContentsFileDescriptor)に CONTENTS_FILE_DESCRIPTOR。

最後に、 *ParcelableConsts*などの*料金*サフィックスが付いている型は、新しく導入された InterfaceConsts 入れ子になった型以外は、互換性のために残されています。 これらは、Xamarin Android 3.0 で削除されます。

## <a name="resources"></a>リソース

イメージ、レイアウトの説明、バイナリ blob、および文字列ディクショナリは、 [リソースファイル](https://developer.android.com/guide/topics/resources/providing-resources.html)としてアプリケーションに含めることができます。
さまざまな Android Api は、イメージ、文字列、またはバイナリ blob を直接処理するのではなく [、リソース id に対して動作](https://developer.android.com/guide/topics/resources/accessing-resources.html) するように設計されています。

たとえば、ユーザーインターフェイスレイアウト ( `main.axml` )、国際化テーブル文字列 ()、およびいくつかのアイコン () を含むサンプル Android アプリでは、 `strings.xml` `drawable-*/icon.png` リソースはアプリケーションの "resources" ディレクトリに保持されます。

```
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
```

ネイティブ Android Api は、ファイル名を直接操作するのではなく、リソース Id を操作します。 リソースを使用する Android アプリケーションをコンパイルすると、ビルドシステムによって配布用のリソースがパッケージ化され、に `Resource` 含まれるリソースの1つに対応するトークンを含むというクラスが生成されます。 たとえば、上記のリソースレイアウトの場合、R クラスで公開されるものは次のようになります。

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

次に、を使用し `Resource.Drawable.icon` てファイルを参照する `drawable/icon.png` か、ファイルを参照する `Resource.Layout.main` か、 `layout/main.xml` または `Resource.String.first_string` ディクショナリファイルの最初の文字列を参照し `values/strings.xml` ます。

## <a name="constants-and-enumerations"></a>定数と列挙体

ネイティブ Android Api には、int を受け取るまたは返すメソッドが多数あります。これは、int の意味を判断するために定数フィールドにマップする必要があります。 これらのメソッドを使用するには、ユーザーはドキュメントを参照して、適切な値である定数を確認する必要がありますが、これは最適ではありません。

たとえば、 [Activity. requestWindowFeature (Int featureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))を考えてみます。

このような場合は、関連する定数を .NET 列挙型にグループ化し、代わりに列挙体を取得するようにメソッドを再マップします。
これにより、候補となる値を IntelliSense で選択できるようになります。

上の例は、次のようになります: [Activity. RequestWindowFeature (WindowFeatures featureId)](xref:Android.App.Activity.RequestWindowFeature*)。

これは、どの定数が一緒に所属し、どの Api がこれらの定数を使用するかを判断するための、非常に手動のプロセスです。 列挙型としてより適切に表現される API で使用される定数については、バグをファイルに記述してください。