---
title: Xamarin.iOS API の設計
description: Xamarin.iOS API の設計に使用された指導原理とそれの Objective-C との関連性。
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: a2435b30b7d5b468fca6c55d295c87b9a0d20652
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303727"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS API の設計

[Xamarin.iOS](~/ios/index.yml) には、Mono に含まれる中心的基底クラス ライブラリに加え、開発者が Mono でネイティブ iOS アプリケーションを作成するためのさまざまな iOS API 向けバインディングが付属しています。

Xamarin.iOS の中心となるのは、C# の領域と Objective-C の領域の橋渡しとなる相互運用エンジンと、CoreGraphics や [OpenGL ES](#opengles) のような、iOS C ベースの API 向けのバインディングです。

Objective-C コードと通信するための下位ランタイムが [MonoTouch.ObjCRuntime](#objcruntime) です。 この上にさらに、[Foundation](#foundation)、CoreFoundation、[UIKit](#uikit) 向けのバインディングが与えられます。

## <a name="design-principles"></a>設計原則

Xamarin.iOS バインディングには設計原則がいくつかあります (macOS の Objective-C 用 Mono バインディングである Xamarin.Mac にも適用されます)。

- [フレームワーク デザインのガイドライン](https://docs.microsoft.com/dotnet/standard/design-guidelines)に従う
- Objective-C クラスのサブクラス化を開発者に許可する

  - 既存クラスから派生させる
  - 連鎖する基本コンストラクターを呼び出す
  - オーバーライド メソッドは C# のオーバーライド システムで行う
  - サブクラス化は C# 標準コンストラクトで動作しなければならない

- Objective-C セレクターに触れさせない
- 任意の Objective-C ライブラリを呼び出すメカニズムを与える
- 一般的な Objective-C タスクを簡単にし、難しい Objective-C タスクを可能にする
- Objective-C プロパティを C# プロパティとして公開する
- 厳密に型指定された API を公開する

  - タイプ セーフ (型の安全性) の程度を大きくする
  - ランタイム エラーを最小限に抑える
  - 戻り値の型で IDE IntelliSense を取得する
  - IDE ポップアップ ドキュメントの準備をする

- API の IDE 内でいろいろ試すことを奨励する

  - たとえば、次のような弱く型指定された配列を公開する代わりに

    ```objc
    NSArray *getViews
    ```

    このように厳密な型を公開します

    ```csharp
    NSView [] Views { get; set; }
    ```

    これによって Visual Studio for Mac は、API の閲覧中に、オートコンプリート機能が利用可能になります。また、戻り値であらゆる `System.Array` 操作が可能になり、戻り値で、LINQ が利用できます。

- ネイティブ C# 型:

  - [`NSString` は `string`](~/ios/internals/api-design/nsstring.md) になります
  - 列挙型にすれば良かった `int` パラメーターと `uint` パラメーターを C# 列挙型と `[Flags]` 属性を含む C# 列挙型に変えます
  - 型に依存しない `NSArray` の代わりに、厳密に型指定された配列として配列を公開します。
  - イベントと通知については、ユーザーに次から選択してもらいます。

    - 既定で、厳密に型指定されたバージョン
    - 上級ユース ケースのための緩やかに型指定されたバージョン

- Objective-C デリゲート パターンのサポート:

  - C# イベント システム
  - C# デリゲート (ラムダ、匿名メソッド、`System.Delegate`) を Objective-C API にブロックとして公開します

### <a name="assemblies"></a>アセンブリ

Xamarin.iOS には、*Xamarin.iOS Profile* を構成するアセンブリがたくさん含まれています。 詳細については、[アセンブリ](~/cross-platform/internals/available-assemblies.md)に関するページを参照してください。

### <a name="major-namespaces"></a>主要な名前空間

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](xref:ObjCRuntime) 名前空間で開発者は C# の領域と Objective-C の領域をつなぐことができます。
これは、Cocoa# と Gtk# での経験に基づき、iOS のために特別に設計された新しいバインディングです。

#### <a name="foundation"></a>Foundation

[Foundation](xref:Foundation) 名前空間からは、iOS に含まれる Objective-C Foundation フレームワークと相互運用するために設計された基本的なデータ型が与えられます。また、これは Objective-C のオブジェクト指向プログラミングの基本です。

Xamarin.iOS によって C# で Objective-C のクラス階層が忠実に再現されます。 たとえば、Objective-C 基底クラス NSObject は [Foundation.NSObject](xref:Foundation.NSObject) 経由で C# から利用できます。

この名前空間からは基礎となる Objective-C Foundation 型のバインディングが与えられますが、基礎となる型を .NET 型にマッピングしたケースもありました。 次に例を示します。

- NSString と [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html) を扱う代わりに、このランタイムでは、API 全体でこの 2 つがそれぞれ C# [文字列](xref:System.String)と厳密に型指定された[配列](xref:System.Array)として公開されます。

- 開発者がサードパーティの Objective-C API、その他の iOS API、または Xamarin.iOS で現在バインディングされていない API をバインディングできるよう、さまざまなヘルパー API がここで公開されます。

API のバインディングに関する詳細については、[Xamarin.iOS のバインド生成機能](~/cross-platform/macios/binding/binding-types-reference.md)に関するセクションを参照してください。

##### <a name="nsobject"></a>NSObject

[NSObject](xref:Foundation.NSObject) 型は、すべての Objective-C バインドの基礎となります。 Xamarin.iOS 型は、iOS CocoaTouch API の 2 つの型クラス、C 型 (通常、CoreFoundation 型と呼ばれる) と Objective-C 型 (すべて NSObject クラスから派生) を忠実に再現するものです。

アンマネージド型を忠実に再現する型については、[Handle](xref:Foundation.NSObject.Handle) プロパティからネイティブ オブジェクトを与えることができます。

Mono では、あらゆるオブジェクトがガベージ コレクションされますが、`Foundation.NSObject` では、[System.IDisposable](xref:System.IDisposable) インターフェイスが実装されます。 つまり、あらゆる NSObject のリソースを直接解放できます。ガベージ コレクターが動きだすのを待つ必要がありません。 これは、重い NSObjects を、たとえば、大きなデータ ブロックをポイントしている UIImage を使用しているときに重要です。

自分で定義した型で確実にファイナライズを行う必要がある場合、[NSObject.Dispose(bool) メソッド](xref:Foundation.NSObject.Dispose(System.Boolean))をオーバーライドします。Dispose のパラメーターは "bool disposing" です。true に設定されている場合、Dispose メソッドが呼び出されています。ユーザーがオブジェクトで Dispose () を明示的に呼び出したからです。 値が false の場合、ファイナライザー スレッドのファイナライザーから Dispose(bool disposing) が呼び出されています。

##### <a name="categories"></a>カテゴリ

Xamarin.iOS 8.10 以降では、C# から Objective-C カテゴリを作成できます。

これは `Category` 属性を使用して行います。属性の引数として適用範囲を拡張する型を指定します。 たとえば、次の例では、NSString の適用範囲が拡張されます。

```csharp
[Category (typeof (NSString))]
```

各カテゴリ メソッドで、`Export` 属性で Objective-C にメソッドをエクスポートする通常のメカニズムが使用されています。

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

マネージド拡張メソッドはすべて静的にする必要がありますが、C# の拡張メソッドの標準構文を使用し、Objective-C インスタンス メソッドを作成できます。

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

また、拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスになります。

コード例全体:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

この例では、Objective-C から呼び出せる NSString クラスにネイティブ toUpper インスタンス メソッドが追加されます。

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

これが役に立つシナリオの 1 つは、コードベースのクラス セット全体にメソッドを追加することです。たとえば、これによりすべての `UIViewController` インスタンスでそれがローテーションできることが報告されます。

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```

##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute は、Xamarin.iOS 展開ツールである mtouch に、アプリケーションがそのサイズを減らすように処理されているとき、型または型のメンバーを保存するように指示する目的で使用されるカスタム属性です。

アプリケーションで静的にリンクされないメンバーはすべて、削除の対象となります。 そのため、この属性は、静的に参照されないが、アプリケーションで必要となるメンバーに印を付ける目的で使用されます。

たとえば、型を動的にインスタンス化する場合、型の既定のコンストラクターを保存することもできます。 XML シリアル化を利用する場合、型のプロパティを保持することもできます。

この属性は、ある型のすべてのメンバーに適用するか、型自体に適用できます。 型全体を保持する場合、その型で構文 [Preserve (AllMembers = true)] を利用できます。

#### <a name="uikit"></a>UIKit

[UIKit](xref:UIKit) 名前空間には、C# クラス形式で CocoaTouch を構成するあらゆる UI コンポーネントに対する一対一のマッピングが含まれます。 API は、C# 言語で使用されている規則に従うように変更されています。

C# デリゲートが一般的な操作のために用意されています。 詳細については、「[デリゲート](#delegates)」セクションを参照してください。

#### <a name="opengles"></a>OpenGLES

OpenGLES については、[OpenTK](https://opentk.net/) API の[改良版](xref:OpenTK)を配布しています。この API は、CoreGraphics のデータ型と構造体を使用するように変更されている OpenGL にオブジェクト指向でバインディングするものであり、また、iOS で利用できる機能のみ公開します。

OpenGLES 1.1 機能は [ES11.GL 型](xref:OpenTK.Graphics.ES11.GL)から利用できます。

OpenGLES 2.0 機能は [ES20.GL 型](xref:OpenTK.Graphics.ES20.GL)から利用できます。

OpenGLES 3.0 機能は [ES30.GL 型](xref:OpenTK.Graphics.ES30.GL)から利用できます。

### <a name="binding-design"></a>バインディング デザイン

Xamarin.iOS では、基礎となる Objective-C プラットフォームにバインディングするだけではありません。 .NET 型システムとディスパッチ システムを拡張するものであり、C# と Objective-C を一層効果的に混合します。

Windows と Linux でネイティブ ライブラリを呼び出すツールとして P/Invoke が便利なように、あるいは Windows での COM 相互運用に IJW サポートを使用できるように、Xamarin.iOS によって、C# オブジェクトと Objective-C オブジェクトをバインドできるようにランタイムが拡張されます。

後続のセクションで説明する内容は、Xamarin.iOS アプリケーションを作成するユーザーにとって必要ではありませんが、開発者がしくみを理解したり、もっと複雑なアプリケーションを作成したりするときに役立ちます。

#### <a name="types"></a>型

道理にかなうのなら、下位の Foundation 型ではなく C# 型が C# の領域に公開されます。  つまり、[API では NSString ではなく C# の sring 型が使用され](~/ios/internals/api-design/nsstring.md)、NSArray を公開せず、厳密に型指定された C# 配列が使用されます。

一般に、Xamarin.iOS と Xamarin.Mac の設計では、基礎になる `NSArray` オブジェクトは公開されません。 代わりに、ランタイムによって `NSObject` クラスの厳密に型指定された配列に `NSArray` が自動的に変換されます。 そのため、Xamarin.iOS では、NSArray を返す目的で GetViews のような緩やかに型指定されたメソッドを公開することはありません。

```csharp
NSArray GetViews ();
```

代わりに、次のように、バインディングによって、厳密に型指定された戻り値が公開されます。

```csharp
UIView [] GetViews ();
```

`NSArray` を直接使用するような不健全な状況では `NSArray` でいくつかのメソッドが公開されることがありますが、API バインディングではその使用は推奨されていません。

また、**Classic API** では、CoreGraphics API から `CGRect`、`CGPoint`、`CGSize` を公開せず、`System.Drawing` 実装の `RectangleF`、`PointF`、`SizeF` に取って代えました。OpenTK が使用される既存の OpenGL コードを開発者が残せるからです。 新しい 64 ビット **Unified API** を使用する場合、CoreGraphics API を使用してください。

#### <a name="inheritance"></a>継承

Xamarin.iOS API デザインでは、派生クラスにキーワード "override" を使用し、また、C# キーワード "base" を使用して基礎実装に連結することで、C# 型を拡張する場合と同様に、開発者はネイティブ Objective-C 型を拡張できます。

このデザインでは、開発者は開発プロセスの一環として Objective-C セレクターを扱わずに済みます。Objective-C システム全体が既に Xamarin.iOS ライブラリ内でラップされているためです。

#### <a name="types-and-interface-builder"></a>型と Interface Builder

作成した .NET クラスが Interface Builder で作成された型のインスタンスになるとき、`IntPtr` パラメーターを 1 つ受け取るコンストラクターを指定する必要があります。
これは、マネージド オブジェクト インスタンスをアンマネージド オブジェクトにバインドするために必要です。
コードは次のような 1 行で構成されます。

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

#### <a name="delegates"></a>デリゲート

Objective-C と C# では、それぞれの言語においてデリゲートという言葉の意味が異なります。

Objective-C の世界と、CocoaTouch に関してネットで見つかる文書では、デリゲートは通常、一連のメソッドに応答するクラスのインスタンスです。 これは C# インターフェイスに非常に似ています。メソッドが常に必須になるとは限らないところが異なります。

このようなデリゲートは、UIKit とその他の CocoaTouch API で重要な役割を果たします。 次のようなさまざまな作業をこなすために使用されます。

- 通知をコードに与える (C# や Gtk+ のイベント配信に似ている)。
- データ可視化制御のモデルを実装する。
- 制御動作を動かす。

プログラミング パターンは、制御動作を変更する派生クラスの作成を最小限に抑えるように設計されました。 このソリューションは、Gtk のシグナル、Qt スロット、Winforms イベント、WPF/Silverlight など、他の GUI ツールキットが長年にわたり行ってきたことに、性質面で似ています。 何百ものインターフェイスを用意する (アクションごとに 1 つのインターフェイス) ことや不要なメソッドを過度に実装することを避けるために、Objective-C では、オプションのメソッド定義がサポートされています。 これは、すべてのメソッドを実装する必要がある C# インターフェイスとは異なります。

Objective-C クラスでは、このプログラミング パターンが使用されるクラスでプロパティが公開されることがわかります。そのプロパティは通常、`delegate` という名称ですが、インターフェイスの必須および任意部分を実装するために必要です。

Xamarin.iOS からは、これらのデリゲートにバインドする互いに排他的なメカニズムが 3 つ与えられます。

1. [イベント経由](#via-events)
2. [`Delegate` プロパティ経由で厳密に型指定される](#strongly-typed-via-a-delegate-property)
3. [`WeakDelegate` プロパティ経由で緩やかに型指定される](#loosely-typed-via-the-weakdelegate-property)

たとえば、UIWebView クラスについて考えてみます。 これは delegate プロパティに割り当てられる UIWebViewDelegate インスタンスにディスパッチされます。

##### <a name="via-events"></a>イベント経由

さまざまな型に対して、Xamarin.iOS によって適切なデリゲートが自動的に作成され、そのデリゲートによって `UIWebViewDelegate` 呼び出しが C# イベントに転送されます。 `UIWebView`の場合:

- webViewDidStartLoad メソッドが [UIWebView.LoadStarted](xref:UIKit.UIWebView.LoadStarted) イベントにマッピングされます。
- webViewDidFinishLoad メソッドが [UIWebView.LoadFinished](xref:UIKit.UIWebView.LoadFinished) イベントにマッピングされます。
- webView:didFailLoadWithError メソッドが [UIWebView.LoadError](xref:UIKit.UIWebView.LoadError) イベントにマッピングされます。

たとえば、この単純なプログラムでは、Web ビューの読み込み時、開始時刻と終了時刻が記録されます。

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

##### <a name="via-properties"></a>プロパティ経由

イベントは、イベントのサブスクライバーが複数存在するような場合に便利です。 また、イベントは、コードからの戻り値がない場合に限られます。

値を返すことがコードに求められる場合、代わりにプロパティを選択しました。 つまり、オブジェクト内の特定の時点にメソッドを 1 つだけ設定できます。

たとえば、このメカニズムを利用し、`UITextField` のハンドラーでキーボードを画面から消すことができます。

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

この場合の `UITextField` の `ShouldReturn` プロパティはブール値を返すデリゲートを引数として受け取り、[Return] ボタンが押されたとき、TextField で何かすべきかどうかを判断します。 今回のメソッドでは、呼び出し元に "*true*" を返しますが、画面からキーボードを削除することもします (textfield で `ResignFirstResponder` が呼び出されるとこれが発生します)。

##### <a name="strongly-typed-via-a-delegate-property"></a>Delegate プロパティ経由で厳密に型指定される

イベントを使用しないことを望む場合、独自の [UIWebViewDelegate](xref:UIKit.UIWebViewDelegate) サブクラスを指定し、それを [UIWebView.Delegate](xref:UIKit.UIWebView.Delegate) プロパティに割り当てることができます。 UIWebView.Delegate が割り当てられると、UIWebView ビューのディスパッチ メカニズムが機能しなくなります。それに対応するイベントが発生すると、UIWebViewDelegate メソッドが呼び出されます。

たとえば、この単純な型では、Web ビューを読み込むために必要な時間が記録されます。

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

上の例はこのようなコードで使用されます。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

上の例では、UIWebViewer が作成され、メッセージに応答するものとして今回作成したクラス、Notifier のインスタンスにメッセージを送信するように UIWebViewer に指示します。

このパターンは、特定のコントロールの動作を制御する目的でも使用されます。たとえば、UIWebView の場合、[UIWebView.ShouldStartLoad](xref:UIKit.UIWebView.ShouldStartLoad) プロパティによって、`UIWebView` インスタンスは `UIWebView` でページを読み込むかどうかを制御できます。

このパターンは、いくつかのコントロールで必要なときにデータを与える目的でも使用されます。 たとえば、[UITableView](xref:UIKit.UITableView) コントロールは強力なテーブルレンダリング コントロールです。外見も中身も [UITableViewDataSource](xref:UIKit.UITableViewDataSource) のインスタンスによって動きます。

### <a name="loosely-typed-via-the-weakdelegate-property"></a>WeakDelegate プロパティ経由の弱い型付け

厳密に型指定されたプロパティに加え、弱く型付けされたデリゲートもあります。弱い型付けをすることで、開発者が必要に応じて違ったやり方でバインドできます。
Xamarin.iOS のバインディングで厳密に型指定された `Delegate` プロパティが公開されている場合、それに対応する `WeakDelegate` プロパティも必ず公開されています。

`WeakDelegate` を使用するとき、[Export](xref:Foundation.ExportAttribute) 属性でクラスを正しく装飾し、セレクターを指定する必要があります。 次に例を示します。

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

`WeakDelegate` プロパティが割り当てられると、`Delegate` プロパティが使用されないことにご注意ください。 また、継承された基底クラスをエクスポートすることを望んでいるとすれば、その基底クラスでメソッドを実装する場合、それをパブリック メソッドにする必要があります。

## <a name="mapping-of-the-objective-c-delegate-pattern-to-c"></a>Objective-C デリゲート パターンを C\# にマッピングする

次のような Objective-C サンプルがあります。

```objc
foo.delegate = [[SomethingDelegate] alloc] init]
```

このサンプルでは、クラス "SomethingDelegate" のインスタンスを作成して構築し、foo 変数の delegate プロパティに値を割り当てるよう、言語が指示されます。 このメカニズムは Xamarin.iOS と C# でサポートされています。構文は次のようになります。

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS では、Objective-C デリゲート クラスにマッピングされる、厳密に型指定されたクラスを指定しました。 それを使用するために、Xamarin.iOS の実装で定義されたメソッドをサブクラス化し、オーバーライドします。 しくみについて詳しくは、下の「モデル」セクションを参照してください。

### <a name="mapping-delegates-to-c"></a>デリゲートを C\# にマッピングする

一般的な UIKit では、Objective-C デリゲートが 2 つの形式で使用されます。

最初の形式では、コンポーネントのモデルにインターフェイスが与えられます。 たとえば、ビューに必要に応じてデータを提供するメカニズムとしてです。List ビューのデータ ストレージ機能などです。  このような場合、常に適切なクラスのインスタンスを作成し、変数を割り当ててください。

次の例では、文字列を使用するモデルの実装で `UIPickerView` を指定します。

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

2 番目の形式は、イベントの通知を提供することです。 そのような場合、上記のような形式で API を公開しますが、C# イベントも指定します。それにより、簡単な操作で簡単に使用できます。また、C# で匿名のデリゲートやラムダ式と統合されます。

たとえば、`UIAccelerometer` イベントをサブスクライブできます。

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

道理にかなっていれば 2 つのオプションを利用できますが、プログラマーはいずれかを選択する必要があります。 厳密に型指定されたレスポンダー/デリゲートのインスタンスを自分で作成し、それを割り当てると、C# イベントは機能しません。 C# イベントを使用する場合、レスポンダー/デリゲート クラスのメソッドは決して呼び出されません。

`UIWebView` を使用した前の例は、次のように C# 3.0 ラムダを利用して記述できます。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```

#### <a name="responding-to-events"></a>イベントへの応答

Objective-C コードでは、複数のコントロールのイベント ハンドラーと複数のコントロールの情報プロバイダーが同じクラスでホストされることがあります。 これは、クラスがメッセージに応答するために可能です。また、クラスがメッセージに応答する限り、オブジェクトを連結できます。

前に説明したように、Xamarin.iOS は C# イベントベースのプログラミング モデルに対応しており、かつ、デリゲートを実装し、望むメソッドをオーバーライドする新しいクラスを作成できる Objective-C デリゲート パターンに対応しています。

また、複数の異なる操作のレスポンダーがすべて、あるクラスの同じインスタンスにホストされる Objective-C パターンに対応しています。 ただし、これを行うには、Xamarin.iOS バインディングの下位機能を使用する必要があります。

たとえば、あるクラスの同じインスタンスで `UITextFieldDelegate.textFieldShouldClear`: メッセージと `UIWebViewDelegate.webViewDidStartLoad`: の両方に自分のクラスを応答させる場合、[Export] 属性宣言を使用する必要がありました。

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

メソッドの C# 名は重要ではありません。重要なのは [Export] 属性に渡される文字列です。

このスタイルのプログラミングを使用するとき、ランタイム エンジンから渡される実際の型と C# プログラミングが一致するようにします。

#### <a name="models"></a>モデル

UIKit ストレージ機能や、ヘルパー クラスを使用して実装されたレスポンダーでは、通常、Objective-C コードでデリゲートとして参照され、プロトコルとして実装されます。

Objective-C プロトコルはインターフェイスのようなものですが、メソッドは自由に選択できます。つまり、全部のメソッドを実装しなくてもプロトコルは動作します。

モデルは 2 とおりの方法で実装されます。 手動で実装するか、既存の厳密に型指定された定義を使用できます。

手動メカニズムは、Xamarin.iOS でバインドされていないクラスを実装するときに必要です。 次のことが簡単にできます。

- ランタイムで登録するためのフラグをクラスに付ける
- オーバーライドするメソッドごとに実際のセレクター名を付け、[エクスポート] 属性を適用する
- クラスをインスタンス化し、渡す

たとえば、次の場合、UIApplicationDelegate プロトコル定義で任意のメソッドのうち、1 つだけが実装されます。

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective-C セレクター名 ("applicationDidFinishLaunching:") は Export 属性で宣言され、クラスは `[Register]` 属性で登録されます。

Xamarin.iOS からは厳密に型指定された宣言が与えられます。これはすぐに使用できるもので、手動でバインドする必要がありません。 このプログラミング モデルをサポートするために、Xamarin.iOS ランタイムでは、クラス宣言で [Model] 属性がサポートされます。 これで、メソッドが明示的に実装されていない限り、クラスのすべてのメソッドを接続するよう、ランタイムに通知します。

つまり、UIKit では、任意のメソッドを指定したプロトコルを表わすクラスは次のように記述されます。

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

一部のメソッドのみを実装するモデルを実装する場合、必要なことは、関心のあるメソッドをオーバーライドし、その他のメソッドを無視することです。 ランタイムでは、上書きされたメソッドのみが Objective-C の領域に接続され、元のメソッドは接続されません。

前の手動テンプルに相当するものが以下です。

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

この長所は、セレクター、引数の種類、C# へのマッピングを見つける目的で Objective-C ヘッダー ファイルを調べる必要がないことと、Visual Studio for Mac の IntelliSense と厳密な型を使用できることです。

#### <a name="xib-outlets-and-c"></a>XIB Outlets と C\#

> [!IMPORTANT]
> このセクションでは、XIB ファイルの使用時、アウトレットとの IDE 統合について説明します。 Xamarin Designer for iOS を使用するとき、下の画像のように、IDE の [プロパティ] セクションにある **[ID] の [名前]** に名前を入力することで、これが全部置換されます。
>
> [![](images/designeroutlet.png "Entering an item Name in the iOS Designer")](images/designeroutlet.png#lightbox)
>
>iOS デザイナーの詳細については、「[iOS デザイナーの概要](~/ios/user-interface/designer/introduction.md#how-it-works)」ドキュメントを参照してください。

これは Outlets が C# と統合するしくみを詳しく説明したものであり、Xamarin.iOS の上級ユーザー向けに提供されています。 Visual Studio for Mac を使用するときは、その場で自動的に生成されたコードを利用し、裏側で自動的にマッピングが行われます。

Interface Builder でユーザー インターフェイスを設計すると、アプリケーションの外見を設計し、既定の接続をいくつか確立するだけになります。 プログラムを利用して情報をフェッチする、実行時のコントロールの動作を変更する、あるいは実行時のコントロールを変更する場合、一部のコントロールをマネージド コードにバインドする必要があります。

これは数回のステップで行われます。

1. **アウトレット宣言**を**ファイルの所有者**に追加します。
1. コントロールを**ファイルの所有者**に接続します。
1. UI と接続を XIB/NIB ファイルに格納します。
1. 実行時に NIB ファイルを読み込みます。
1. アウトレット変数にアクセスします。

手順 (1) から (3) までは Apple の Interface Builder でインターフェイスを構築する方法に関するドキュメントで取り上げられています。

Xamarin.iOS の使用時、UIViewController から派生されるクラスをアプリケーションで作成する必要があります。 これは次のように実装されます。

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

それから、NIB ファイルから ViewController を読み込みために次を行います。

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

これにより、NIB からユーザー インターフェイスが読み込まれます。 次に、アウトレットにアクセスするために、アウトレットにアクセスすることをランタイムに知らせる必要があります。 これを行うために、`UIViewController` サブクラスでプロパティを宣言し、[Connect] 属性で注釈を付ける必要があります。 以下に例を示します。

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

プロパティ実装は、実際のネイティブ型の値を実際に取得し、格納する実装です。

Visual Studio for Mac と InterfaceBuilder を使用するとき、これに関して心配する必要はありません。 Visual Studio for Mac では、プロジェクトの一部としてコンパイルされた部分的クラスのコードによって、宣言済みのアウトレットがすべて自動的にミラー化されます。

#### <a name="selectors"></a>セレクター

Objective-C プログラミングの中心的概念はセレクターです。 セレクターを渡すこと要求する API やセレクターに応答することをコードに求める API には頻繁に遭遇します。

C# ではとても簡単に新しいセレクターを作成できます。`ObjCRuntime.Selector` クラスの新しいインスタンスを作成したら、それを必要とする API の場所で使用する。それだけです。 次に例を示します。

```csharp
var selector_add = new Selector ("add:plus:");
```

C# メソッドがセレクターの呼び出しに応答するには、それが `NSObject` 型から派生している必要があり、`[Export]` 属性を利用し、セレクター名で C# メソッドを修飾する必要があります。 次に例を示します。

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

セレクター名は、中間と末尾にコロン (":") があればそれも含め、厳密に**一致する必要がある**ことにご注意ください。

#### <a name="nsobject-constructors"></a>NSObject コンストラクター

`NSObject` から派生した Xamarin.iOS クラスのほとんどでオブジェクトの機能に固有のコンストラクターが公開されますが、すぐにはわからないさまざまなコンストラクターも公開されます。

コンストラクターは次のように使用されます。

```csharp
public Foo (IntPtr handle)
```

このコンストラクターは、ランタイムでクラスをアンマネージド クラスにマッピングする必要があるとき、クラスをインスタンス化する目的で使用されます。 これは XIB/NIB ファイルを読み込むときに行われます。  この時点で、Objective-C ランタイムによりアンマネージドの領域でオブジェクトが作成されているでしょう。また、このコンストラクターが呼び出され、マネージド側が初期化されます。

通常、必要な作業は、ハンドル パラメーターで基本コンストラクターを呼び出し、本文で必要な初期化を行うことだけです。

```csharp
public Foo ()
```

これはクラスの既定のコンストラクターです。Xamarin.iOS 付属のクラスでは、これにより Foundation.NSObject クラスと間にあるすべてのクラスが初期化され、最後に、クラスの Objective-C `init` メソッドにこれが連結されます。

```csharp
public Foo (NSObjectFlag x)
```

このコンストラクターはインスタンスを初期化する目的で使用されますが、コードが最後に Objective-C "init" メソッドを呼び出すことを禁止します。 初期化に既に登録しているとき (コンストラクターで `[Export]` を使用するとき)、または、別の手段で初期化を既に完了しているとき、通常、これを使用します。

```csharp
public Foo (NSCoder coder)
```

このコンストラクターは、オブジェクトが NSCoding インスタンスから初期化されている場合に与えられます。

#### <a name="exceptions"></a>例外

Xamarin.iOS API デザインでは、Objective-C 例外が C# 例外として発生しません。 このデザインでは、第一に Objective-C の領域にガベージが送信されません。そして、生成しなければならない例外があれば、無効なデータが Objective-C の領域に渡される前にそれが生成されます。

#### <a name="notifications"></a>通知

iOS と OS X の両方で、基礎となるプラットフォームでブロードキャストされる通知を開発者が定期購読できます。 これは `NSNotificationCenter.DefaultCenter.AddObserver` メソッドを使用して行われます。 `AddObserver` メソッドは 2 つのパラメーターを受け取ります。1 つは、定期購読する通知です。もう 1 つは、通知が発生したときに呼び出すメソッドです。

Xamarin.iOS と Xamarin.Mac の両方で、さまざまな通知のキーは、通知をトリガーするクラスでホストされます。 たとえば、`UIMenuController` で生成される通知は、"Notification" という名前で終わる `UIMenuController` クラスで `static NSString` プロパティとしてホストされます。

### <a name="memory-management"></a>メモリ管理

Xamarin.iOS に含まれるガベージ コレクターによって、不要になったリソースが解放されます。 ガベージ コレクターに加え、`NSObject` から派生されるオブジェクトはすべて `System.IDisposable` インターフェイスを実装します。

#### <a name="nsobject-and-idisposable"></a>NSObject と IDisposable

オブジェクトによってカプセル化が行われることがあります。大きなメモリ ブロック (たとえば、`UIImage` は無邪気なポインターのように見えますが、2 メガバイトの画像をポイントしていることがあります) やその他の重要な有限リソース (動画の復号バッファー) がカプセル化されます。そのようなオブジェクトを解放するとき、開発者を支援する便利な方法が `IDisposable` インターフェイスの公開です。

NSObject では IDisposable interface が実装され、さらに [.NET Dispose パターン](https://msdn.microsoft.com/library/fs2xkftw.aspx)も実装されます。 これにより、開発者は NSObject をサブクラス化し、Dispose 動作をオーバーライドして自分だけのリソースを必要なときに解放できます。 たとえば、画像の集まりを維持するこのビュー コントローラーについて考えてください。

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

破棄されたマネージド オブジェクトはもう役に立ちません。 オブジェクトの参照が残っているかもしれませんが、どの点から見ても、破棄後のオブジェクトはこの時点で無効です。 これを確実に行う .NET API もあります。たとえば、次のように、破棄されたオブジェクトでメソッドにアクセスしようとすると、ObjectDisposedException をスローします。

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

引き続き変数 "image" にアクセスできるとしても、それは実際、無効な参照であり、画像を保持していた Objective-C オブジェクトを今はポイントしていません。

ただし、C# でオブジェクトを破棄しても、そのオブジェクトは必ずしも完全に破壊されていません。 C# で与えられたオブジェクトの参照を解放しただけです。 Cocoa 環境がそこで使用するために参照を維持している可能性があります。 たとえば、UIImageView の Image プロパティをある画像に設定し、その画像を破棄した場合、基礎となる UIImageView では独自の参照を取得しており、このオブジェクトの参照を使い終わるまで維持します。

#### <a name="when-to-call-dispose"></a>Dispose を呼び出す状況

オブジェクトを削除するとき Mono が必要であれば、Dispose を呼び出してください。 考えられるユース ケースは、メモリや情報プールなど、重要なリソースの参照が NSObject で実際に維持されていることが Mono で認識されていないときです。 そのようなケースでは、Mono がガベージ コレクションを実行するのを待たず、Dispose を呼び出し、メモリの参照をすぐに解放してください。

内部的には、Mono によって [C# 文字列から NSString 参照](~/ios/internals/api-design/nsstring.md)が作成されるとき、参照をすぐに破棄し、ガベージ コレクターが関連する作業量を減らします。 扱うオブジェクトの数が少なければ、それだけ GC の実行時間が短くなります。

#### <a name="when-to-keep-references-to-objects"></a>オブジェクトの参照を維持する状況

自動メモリ管理の副作用の 1 つは、参照がない限り、使用されていないオブジェクトが GC によって削除されることです。 これは意外な副作用を引き起こすことがあります。たとえば、最上位ビュー コントローラーまたは最上位ウィンドウを維持するローカル変数を作成したのに、それがひっそりと消えます。

静的変数またはインスタンス変数でオブジェクトの参照を維持しないなら、Mono によって変数で Dispose() メソッドがしっかりと実行され、オブジェクトの参照が解放されます。 これが唯一の未処理の参照という場合もあり、Objective-C ランタイムによってオブジェクトが自動的に破壊されます。

## <a name="related-links"></a>関連リンク

- [フィールドのバインド](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
