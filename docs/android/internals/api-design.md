---
title: Xamarin. Android API 設計の原則
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2958e456aeb25ba39697ad82500d574907e963e4
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510759"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin. Android API 設計の原則

Mono の一部である基本クラス ライブラリのコアだけでなく Xamarin.Android Mono とネイティブ Android アプリケーションを作成するためにさまざまな Android API のバインドに付属します。

Xamarin.Android のコアがありますが、相互運用機能のエンジン、Java の世界中でそのブリッジ、C# の世界と、C# または他の .NET 言語からの Java API にアクセス権を持つ開発者。


## <a name="design-principles"></a>設計原則

これらは、Xamarin Android のバインドの設計原則の一部です。

-  [.NET Framework の設計ガイドライン](https://docs.microsoft.com/dotnet/standard/design-guidelines/)に準拠します。

-  開発者が Java クラスをサブクラス化できるようにします。

-  サブクラスは、C# の標準的なコンストラクトで動作します。

-  既存のクラスから派生します。

-  チェーンする基本コンストラクターを呼び出します。

-  メソッドのオーバーライドは、C# の上書きのシステムで行う必要があります。

-  一般的な Java タスクを簡単にし、困難な Java タスクにすることができます。

-  JavaBean プロパティをプロパティC#として公開します。

-  厳密に型指定された API を公開します。

    - タイプセーフを増やします。

    - 実行時エラーを最小化します。

    - 戻り値の型に対して IDE intellisense を取得します。

    - IDE ポップアップドキュメントを使用できるようにします。

-  API の IDE で探索をお勧めします。

    - フレームワークの代替手段を利用して、Java Classlib の露出を最小限に抑えます。

    - 適切なと該当する場合は、単一メソッドのインターフェイスではなく C# のデリゲート (ラムダ、匿名メソッドと System.Delegate) を公開します。

    - 任意の Java ライブラリ ( [Android. Runtime. jの](xref:Android.Runtime.JNIEnv)) を呼び出すための機構を提供します。


## <a name="assemblies"></a>アセンブリ

Xamarin Android には、*モノモバイルプロファイル*を構成するさまざまなアセンブリが含まれています。 [[アセンブリ](~/cross-platform/internals/available-assemblies.md)] ページに詳細情報があります。

Android プラットフォームへのバインドは、 `Mono.Android.dll`アセンブリに含まれています。 このアセンブリには、使用の Android API 全体のバインドと Android ランタイム VM との通信が含まれています。


## <a name="binding-design"></a>バインディングのデザイン


### <a name="collections"></a>コレクション

Android の API は、リスト、セット、およびマップを提供する広範な java.util コレクションを使用します。 これらの要素は、バインディングの[system.object インターフェイスを](xref:System.Collections.Generic)使用して公開されています。 基本的なマッピングは次のとおりです。

-   [java. util. マップ<E> ](https://developer.android.com/reference/java/util/Set.html)をシステム型[ICollection<T>](xref:System.Collections.Generic.ICollection`1), helper クラス[JavaSet<T>](xref:Android.Runtime.JavaSet`1)に設定します。

-   JavaList は、system 型[IList<T>](xref:System.Collections.Generic.IList`1)、ヘルパークラス、およびにマップ[されます。<T>](xref:Android.Runtime.JavaList`1) [<E> ](https://developer.android.com/reference/java/util/List.html)

-   TValue [< K, V >](https://developer.android.com/reference/java/util/Map.html)マップをシステム型[IDictionary < TKey, >](xref:System.Collections.Generic.IDictionary`2), Helper クラス[JavaDictionary < K, v >](xref:Android.Runtime.JavaDictionary`2)にマップします。

-   JavaCollection は、システム型[ICollection<T>](xref:System.Collections.Generic.ICollection`1), helper クラスにマップされます。 [<E> ](https://developer.android.com/reference/java/util/Collection.html) [<T>](xref:Android.Runtime.JavaCollection`1)

これらの型のより高速なコピーを容易にするヘルパークラスが用意されています。 可能であれば、またはのような[`List<T>`](xref:System.Collections.Generic.List`1)フレームワークの実装では[`Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2)なく、これらの提供されたコレクションを使用することをお勧めします。 [Android のランタイム](xref:Android.Runtime)実装はネイティブ Java コレクションを内部的に使用するため、android API メンバーに渡すときにネイティブコレクションとの間でコピーを行う必要はありません。

インターフェイスの実装は、そのインターフェイスを受け入れる Android メソッドに渡すことができます。 `List<int>`たとえば、を[&lt;arrayadapter&gt;int (Context, int,&lt;IList&gt;int)](xref:Android.Widget.ArrayAdapter`1)コンストラクターに渡します。 *ただし*、Android のランタイム実装を*除く*すべての実装については、Mono VM から android ランタイム VM にリストを*コピー*する必要があります。 リストが Android ランタイム内で後で変更された場合 (たとえば、 [arrayadapter&lt;T&gt;を呼び出した場合)。Add (T)](xref:Android.Widget.ArrayAdapter`1.Add*)メソッド)。これらの変更はマネージコードでは表示され*ません*。 を`JavaList<int>`使用した場合は、それらの変更が表示されます。

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

-  Java メソッドのペア`T getFoo()`と`void setFoo(T)`は、 `Foo`プロパティに変換されます。 例:[アクティビティ。インテント](xref:Android.App.Activity.Intent)。

-  Java メソッド`getFoo()`は、読み取り専用の Foo プロパティに変換されます。 例:[PackageName](xref:Android.Content.Context.PackageName)。

-  セットのみのプロパティは生成されません。

-  プロパティの型が配列の場合、プロパティは生成され*ません*。



### <a name="events-and-listeners"></a>イベントとリスナー

Android API は、Java の上に構築されており、そのコンポーネントがイベント リスナーをフックするための Java のパターンに従います。 このパターンは、ユーザーが匿名クラスを作成し、オーバーライドするメソッドを宣言する必要があるため、煩雑になる傾向があります。たとえば、Android を使用した Android では、次のような処理が行われます。

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

これらのメカニズムはどちらも、Xamarin Android で使用できることに注意してください。 リスナー インターフェイスを実装し、View.SetOnClickListener でアタッチまたは Click イベントに通常の C# パラダイムのいずれかを使用して作成されたデリゲートをアタッチすることができます。

リスナーのコールバックメソッドに void が返された場合は、 [&lt;EventHandler teventargs&gt; ](xref:System.EventHandler`1)デリゲートに基づいて API 要素を作成します。 これらのリスナー型について、上記の例のようなイベントを生成します。 ただし、リスナーのコールバックが void 以外の値と非**ブール**値を返す場合、Events と EventHandlers は使用されません。 代わりに、コールバックのシグネチャに対して特定のデリゲートを生成し、イベントの代わりにプロパティを追加します。 その理由は、デリゲート呼び出し順序と戻り処理を処理することです。 このアプローチは、Xamarin. iOS API で行われる処理を反映しています。

C#イベントまたはプロパティは、Android イベント登録方法の場合にのみ自動的に生成されます。

1. には `set` プレフィックスがあります。たとえば、[*set*OnClickListener](xref:Android.Views.View.SetOnClickListener*)。

1. に戻り`void`値の型があります。

1. は、パラメーターを1つだけ受け取ります。パラメーターの型はインターフェイスで、インターフェイスにはメソッドが1つだけ`Listener`あり、インターフェイス名はのようになります (例: [OnClick *Listener*](xref:Android.Views.View.IOnClickListener))。


さらに、リスナーインターフェイスメソッドの戻り値の型が**void**ではなく**ブール**型である場合、生成された*EventArgs*サブクラスには、*処理*されたプロパティが含まれます。 *処理*されたプロパティの値は、*リスナー*メソッドの戻り値として使用され、 `true`既定でになります。

たとえば、Android ビューの[setonkeylistener ()](xref:Android.Views.View.SetOnKeyListener*)メソッドは、[ビュー. onkeylistener](xref:Android.Views.View.IOnKeyListener)インターフェイスを受け取り、 [onKey (View, int, keyevent)](xref:Android.Views.View.IOnKeyListener.OnKey*)メソッドにはブール型の戻り値の型があります。 Xamarin では[、対応する](xref:Android.Views.View.KeyPress)system.windows.forms.keyeventargs.handled [&lt;&gt;](xref:Android.Views.View.KeyEventArgs)イベントが生成されます。これは、EventHandler ビューです。
さらに、*KeyEventArgs* クラスには、*View.OnKeyListener.onKey()* メソッドの戻り値として使用される [View.KeyEventArgs.Handled](xref:Android.Views.View.KeyEventArgs.Handled) プロパティが含まれています。

デリゲートベースの接続を公開するために、他のメソッドのオーバーロードと ctor を追加する予定です。 また、複数のコールバックを持つリスナーは、個々のコールバックの実装が妥当であるかどうかを判断するために追加の検査を必要とするので、識別されたとおりに変換します。 対応するイベントがない場合は、リスナーは、C# で使用する必要がありますが、注目するデリゲートの使用量ができたと考えられるすべてを表示してください。 また、"リスナー" サフィックスが付いていないインターフェイスは、デリゲートの代わりに使用できるという点で、何らかの変換も行われています。

すべてのリスナーインターフェイスは、[`Android.Runtime.IJavaObject`](xref:Android.Runtime.IJavaObject)
インターフェイス。バインディングの実装の詳細があるため、リスナークラスはこのインターフェイスを実装する必要があります。 これを行うには、 [java](xref:Java.Lang.Object)のサブクラスまたはその他のラップされた java オブジェクト (Android アクティビティなど) にリスナーインターフェイスを実装します。


### <a name="runnables"></a>Runnables

Java では、 [java](xref:Java.Lang.Runnable)の実行可能インターフェイスを利用して委任メカニズムを提供します。 [.Java](xref:Java.Lang.Thread)クラスは、このインターフェイスの注目すべきコンシューマーです。 Android は API でもインターフェイスを採用しています。
[Activity. runOnUiThread ()](xref:Android.App.Activity.RunOnUiThread*)と[View.post ()](xref:Android.Views.View.Post*)は、注目すべき例です。

この`Runnable`インターフェイスには、1つの void メソッド、 [run ()](xref:Java.Lang.Runnable.Run)が含まれています。 そのために適していると C# でのバインディングを[System.Action](xref:System.Action)を委任します。 ネイティブ api `Action`でを`Runnable`使用するすべての api メンバー (たとえば、 [Activity. runonuithread ()](xref:Android.App.Activity.RunOnUiThread*)および[View.Post ())](xref:Android.Views.View.Post*)のパラメーターを受け取るオーバーロードをバインドに提供しています。

いくつかの型がインターフェイスを実装しているため、runnables として直接渡すことができるため、[irunnable](xref:Java.Lang.IRunnable) 可能なオーバーロードを代わりに使用したままにしました。


### <a name="inner-classes"></a>内部クラス

Java には、静的な入れ子になったクラスと非静的クラスの2種類の[入れ子になっ](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html)たクラスがあります。

Java の静的な入れ子になったクラスは、C# の入れ子になった型と同じです。

静的でない入れ子になったクラス (*内部クラス*とも呼ばれます) は、大幅に異なります。 外側の型のインスタンスへの暗黙的な参照が含まれており、静的メンバーを含むことはできません (この概要の範囲外の違いがあります)。

バインドと C# を使用する際に、静的な入れ子になったクラスが入れ子になった、通常の型として扱われます。 内部クラスには、次の2つの大きな違いがあります。

1. 含んでいる型への暗黙的な参照は、コンストラクターのパラメーターとして明示的に指定する必要があります。

1. 内側のクラスと内部クラスから継承する場合*する必要があります*型の中で入れ子にする基本の内部クラスを含む型から継承して、派生型は、C# と同じ型のコンストラクターを指定する必要がありますの種類を格納しています。

たとえば、 [WallpaperService](xref:Android.Service.Wallpaper.WallpaperService.Engine)の内部クラスを考えてみます。 これは内部クラスであるため、 [WallpaperService () コンストラクター](xref:Android.Service.Wallpaper.WallpaperService.Engine#ctor)は[WallpaperService](xref:Android.Service.Wallpaper.WallpaperService)インスタンスへの参照を受け取ります (比較対照は、パラメーターを取らない Java WallpaperService () コンストラクターと比較してください)。

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

`CubeWallpaper.CubeEngine` `WallpaperService.Engine` `CubeWallpaper` `CubeWallpaper`が内で`CubeWallpaper`入れ子になっていること、の外側のクラスから継承されていること、および宣言型を受け取るコンストラクターがあることに注意してください (この例では、上記で指定したすべて)。 `CubeWallpaper.CubeEngine`

### <a name="interfaces"></a>インターフェイス

Java インターフェイスは、C# から問題が発生する 2 つのメンバーの 3 つのセットを含めることができます。

1. メソッド

1. 種類

1. フィールド

Java インターフェイスは、次の2つの型に変換されます。

1. メソッド宣言を含む (省略可能な) インターフェイス。 このインターフェイスには、Java インターフェイスと同じ名前が付けられます。*ただし*、' *I* ' プレフィックスも含まれます。

1. Java インターフェイス内で宣言された任意のフィールドを含む (省略可能な) 静的クラス。

入れ子になった型は、入れ子になった型ではなく、外側のインターフェイスの兄弟となるように "再配置" されます。外側のインターフェイス名はプレフィックスとして使用されます。

たとえば、 [Parcelable](xref:Android.OS.Parcelable)インターフェイスを考えてみます。
*Parcelable*インターフェイスには、メソッド、入れ子にされた型、および定数が含まれています。 *Parcelable*インターフェイスメソッドは、 [IParcelable](xref:Android.OS.IParcelable)インターフェイスに配置されます。
*Parcelable*インターフェイス定数は[ParcelableConsts](xref:Android.OS.ParcelableConsts)型に配置されます。 入れ子になった[ClassLoaderCreator&lt;t >](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html)と[>&lt;Parcelable](https://developer.android.com/reference/android/os/Parcelable.Creator.html)の型は現在、ジェネリックサポートの制限のためにバインドされていません。サポートされている場合は、は、 *IParcelableClassLoaderCreator*および*IParcelableCreator*インターフェイスとして表示されます。 たとえば、入れ子になった[DeathRecipient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html)インターフェイスは、 [IBinderDeathRecipient](xref:Android.OS.IBinderDeathRecipient)インターフェイスとしてバインドされます。

> [!NOTE]
> Xamarin Android 1.9 以降では、java コードの移植を簡素化するために Java インターフェイス定数が_複製_されています。 これは、 [android プロバイダー](https://developer.android.com/reference/android/provider/package-summary.html)のインターフェイス定数に依存する Java コードの移植性を向上させるのに役立ちます。

上記の型に加えて、次の4つの変更点があります。

1. Java インターフェイスと同じ名前を持つ型は、定数を含むように生成されます。

1. インターフェイス定数を含む型には、実装された Java インターフェイスから取得されるすべての定数も含まれます。

1. 定数を含む Java インターフェイスを実装するすべてのクラスは、実装されているすべてのインターフェイスの定数を含む新しい入れ子になった InterfaceConsts 型を取得します。

1. *料金*型は互換性のために残されています。


*Parcelable*インターフェイスの場合、これは定数を格納するための[*Parcelable*](xref:Android.OS.Parcelable)型となることを意味します。 たとえば、 [Parcelable](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR)定数は*ParcelableConsts*定数としてではなく、 [*ContentsFileDescriptor*](xref:Android.OS.Parcelable.ContentsFileDescriptor)定数としてバインドされます。

さらに多くの定数を含む他のインターフェイスを実装する定数を含むインターフェイスの場合は、すべての定数の和集合が生成されるようになりました。 たとえば、 [MediaColumns](xref:Android.Provider.MediaStore.MediaColumns)インターフェイスには、android............... [videocolumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) . ただし、1.9 より前では、 [VideoColumnsConsts](xref:Android.Provider.MediaStore.Video.VideoColumnsConsts)型には、 [MediaColumnsConsts](xref:Android.Provider.MediaStore.MediaColumnsConsts)で宣言された定数にアクセスする方法がありませんでした。
その結果、Java 式*MediaStore.Video.VideoColumns.TITLE* C# の式にバインドする必要がある*MediaStore.Video.MediaColumnsConsts.Title*は読まず見つけにくい多数の Java のドキュメント。 1\.9 では、同等C#の式は[Mediastore. Video columns. Title](xref:Android.Provider.MediaStore.Video.VideoColumns.Title)です。

さらに、Java *Parcelable*インターフェイスを実装する[Android. os バンドル](xref:Android.OS.Bundle)の種類を考えてみましょう。 インターフェイスが実装されているため、そのインターフェイスのすべての定数は、バンドルの種類を介して "アクセス可能" になります。たとえば、 *CONTENTS_FILE_DESCRIPTOR*は完全に有効な Java 式です。
以前は、この式をにC#移植するには、実装されているすべてのインターフェイスを調べて、 *CONTENTS_FILE_DESCRIPTOR*の送信元の型を確認する必要がありました。 Xamarin Android 1.9 以降では、定数を含む Java インターフェイスを実装するクラスには、継承されたすべてのインターフェイス定数を含む*InterfaceConsts*型が入れ子になっています。 これにより、 *CONTENTS_FILE_DESCRIPTOR*を[*InterfaceConsts. ContentsFileDescriptor*](xref:Android.OS.Bundle.InterfaceConsts.ContentsFileDescriptor)に変換することができます。

最後に、 *ParcelableConsts*などの*料金*サフィックスが付いている型は、新しく導入された InterfaceConsts 入れ子になった型以外は、互換性のために残されています。 これらは、Xamarin Android 3.0 で削除されます。


## <a name="resources"></a>リソース

イメージ、レイアウトの説明、バイナリ blob、および文字列ディクショナリは、[リソースファイル](https://developer.android.com/guide/topics/resources/providing-resources.html)としてアプリケーションに含めることができます。
さまざまな Android API の設計において[リソース Id に対して](https://developer.android.com/guide/topics/resources/accessing-resources.html)イメージを処理する、代わりに文字列またはバイナリ blob 直接します。

たとえば、ユーザーインターフェイスレイアウト ( `main.axml`)、国際化テーブル文字列 ( `strings.xml`)、およびいくつかのアイコン ( `drawable-*/icon.png`) を含むサンプル Android アプリでは、リソースはアプリケーションの "resources" ディレクトリに保持されます。

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

ネイティブ Android API は、ファイル名を直接操作しないが、リソース Id の代わりに動作します。 リソースを使用する Android アプリケーションをコンパイルすると、ビルドシステムによって配布用のリソースがパッケージ化さ`Resource`れ、に含まれるリソースの1つに対応するトークンを含むというクラスが生成されます。 たとえば、上記のリソースレイアウトの場合、R クラスで公開されるものは次のようになります。

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

次に、を`Resource.Drawable.icon`使用して`drawable/icon.png`ファイルを参照`Resource.Layout.main`するか、 `layout/main.xml`ファイルを参照`Resource.String.first_string`するか、またはディクショナリファイル`values/strings.xml`の最初の文字列を参照します。


## <a name="constants-and-enumerations"></a>定数と列挙体

ネイティブ Android API がある多くのメソッドを返したり、int、int の意味を決定する定数フィールドにマップする必要があります。 これらのメソッドを使用するには、ユーザーはドキュメントを参照して、適切な値である定数を確認する必要がありますが、これは最適ではありません。

たとえば、 [Activity. requestWindowFeature (Int featureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))を考えてみます。

このような場合は、関連する定数を .NET 列挙型にグループ化し、代わりに列挙体を取得するようにメソッドを再マップします。
これにより、候補となる値を IntelliSense で選択できるようになります。

上記の例は次のようになります。[Activity. RequestWindowFeature (WindowFeatures featureId)](xref:Android.App.Activity.RequestWindowFeature*)。

これは、定数が属するを把握する非常に手動のプロセス、API の定数を使用することに注意してください。 列挙型としてより適切に表現される API で使用される定数については、バグをファイルに記述してください。
