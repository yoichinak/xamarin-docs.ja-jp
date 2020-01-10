---
title: Xamarin. iOS API の設計
description: Xamarin の iOS Api を設計するために使用された基本原則と、それらが目的の C にどのように関連しているかを説明します。
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: ab56332617fece8e80429f82000880012bf85b41
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022402"
---
# <a name="xamarinios-api-design"></a>Xamarin. iOS API の設計

[Xamarin.iOS](~/ios/index.yml) には、Mono の一部であるコアの基本クラス ライブラリだけでなく、開発者が Mono を利用してネイティブの iOS アプリケーションの作成を可能にするために、さまざまな iOS API へのバインドが付属しています。

Xamarin.iOS のコアには、C# の世界と Objective-C の世界をブリッジする相互運用機能および、CoreGraphics や [OpenGL ES](#opengles) などの iOS C ベースの API のバインドがあります。

Objective-C のコードと通信する低レベルのランタイムは、[MonoTouch.ObjCRuntime](#objcruntime) にあります。 この上には、 [Foundation](#foundation)、CoreFoundation、および[UiKit](#uikit) のバインドが用意されています。

## <a name="design-principles"></a>設計原則

次に、Xamarin の iOS のバインドに関する設計原則をいくつか示します。これらは、macOS での Objective-C の Mono バインドである Xamarin.Mac にも適用されます。

- フレームワークの[デザインガイドライン](https://docs.microsoft.com/dotnet/standard/design-guidelines)に従う
- 開発者が Objective-C クラスをサブクラス化できるようにします。

  - 既存のクラスからの派生
  - チェーンする基本コンストラクターの呼び出し
  - メソッドのオーバーライドは、C# のオーバーライドシステムを使用して行う必要があります。
  - サブクラス化は C# 標準のコンストラクトで動作する

- 開発者を Objective-C セレクターに公開しない
- 任意の Objective-C ライブラリを呼び出すメカニズムを提供する
- 一般的な Objective-C のタスクを簡単に、困難な Objective-C タスクを可能にする
- Objective-C のプロパティを C# のプロパティとして公開する
- 厳密に型指定された API を公開します。

  - タイプセーフの向上
  - 実行時エラーの最小化
  - 戻り値の型で IDE IntelliSense を取得する
  - IDE ポップアップドキュメントを許可します

- API の IDE で探索をお勧めします。

  - たとえば、次のように弱く型指定された配列を公開するのではなく、

    ```objc
    NSArray *getViews
    ```

    次のように、厳密な型を公開します。

    ```csharp
    NSView [] Views { get; set; }
    ```

    これにより、API の参照中にオートコンプリートを実行できるようになり、戻り値ですべての `System.Array` 操作を使用できるようになり、戻り値が LINQ に参加できるように Visual Studio for Mac ます。

- ネイティブ C# 型:

  - [`NSString` が `string`になります](~/ios/internals/api-design/nsstring.md)
  - `int` を有効にし、`[Flags]`属性を持つ列挙C#型とC#列挙型に列挙されている必要があるパラメーターを `uint` します。
  - 型に依存しない `NSArray` オブジェクトではなく、厳密に型指定された配列として配列を公開します。
  - イベントと通知の場合は、次のいずれかの選択肢をユーザーに提供します。

    - 既定では、厳密に型指定されたバージョン
    - 高度なユースケースの弱い型指定のバージョン

- Objective-C のデリゲートパターンをサポートします。

  - C#イベントシステム
  - デリゲートC# (ラムダ、匿名メソッド、および`System.Delegate`) をブロックとして Objectie-C の API に公開する

### <a name="assemblies"></a>アセンブリ

Xamarin.iOS には、 *Xamarin.iOS Profile* を構成する多数のアセンブリが含まれています。 [[アセンブリ](~/cross-platform/internals/available-assemblies.md)] ページに詳細情報があります。

### <a name="major-namespaces"></a>主要な名前空間

#### <a name="objcruntime"></a>ObjCRuntime

[Objcruntime](xref:ObjCRuntime) 名前空間を使用すると、開発者は C# と Objective-C との間で世界を橋渡しできます。
これは、Cocoa # と Gtk # のエクスペリエンスに基づいて、iOS 専用に設計された新しいバインドです。

#### <a name="foundation"></a>Foundation

[Foundation](xref:Foundation)名前空間は、iOS の一部である Objective-C Foundation フレームワークと相互運用できるように設計された基本データ型を提供します。これは、Objective-C でのオブジェクト指向プログラミングの基本となります。

Xamarin.iOS は、C# で Objective-C のクラスの階層をミラーリングしています。 たとえば、Objective-C の基本クラスである  [NSObject](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html)は[Foundation.NSObject](xref:Foundation.NSObject)を介して C# から使用できます。

この名前空間には、基になる Objective-C からの型のバインドが用意されていますが、いくつかのケースでは、基になる型を .NET 型にマップしています。 例えば:

- [NSString](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) と [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html) を扱う代わりに、このランタイムでは、これらを C# の [string](xref:System.String) および厳密に型指定された [Array](xref:System.Array) として API 全体に公開しています。

- ここでは、開発者がサードパーティの Objective-C API、他の iOS API、Xamarin.iOS によって現在バインドされていない API をバインドできるように、さまざまなヘルパー API が公開されています。

バインディング API の詳細については、「[Xamarin.iOS のバインドジェネレーター](~/cross-platform/macios/binding/binding-types-reference.md)」を参照してください。

##### <a name="nsobject"></a>NSObject

[NSObject](xref:Foundation.NSObject)型は、すべての目的 C バインドの基礎となります。 Xamarin iOS の種類は、iOS CocoaTouch Api からの型の2つのクラスをミラー化します。 C 型 (通常は CoreFoundation 型と呼ばれます) と目的 C 型 (これらはすべて NSObject クラスから派生) です。

アンマネージ型をミラー化する型ごとに、 [Handle](xref:Foundation.NSObject.Handle)プロパティを使用してネイティブオブジェクトを取得できます。

Mono はすべてのオブジェクトに対してガベージコレクションを提供しますが、`Foundation.NSObject` は、 [IDisposable](xref:System.IDisposable)インターフェイスを実装します。 これは、ガベージコレクターが開始されるのを待たずに、特定の NSObject のリソースを明示的に解放できることを意味します。 これは、大量のデータブロックへのポインターを保持している可能性のある UIImages など、ヘビー NSObjects を使用している場合に重要です。

型で決定的な終了処理を実行する必要がある場合は、 [NSObject (bool) メソッド](xref:Foundation.NSObject.Dispose(System.Boolean))をオーバーライドして dispose のパラメーターを "bool disposing" にします。 true に設定すると、ユーザーが明示的に呼び出されたため、dispose メソッドが呼び出されることを意味します。オブジェクトに対して Dispose () を行います。 値が false の場合、これは Dispose (bool disposing) メソッドがファイナライザースレッドでファイナライザーから呼び出されることを意味します。

##### <a name="categories"></a>カテゴリ

Xamarin. iOS 8.10 以降では、C# から Objective-C のカテゴリを作成できます。

これを行うには、属性の引数として拡張する型を指定して、`Category` 属性を使用します。 次の例では、インスタンスを拡張 NSString です。

```csharp
[Category (typeof (NSString))]
```

各カテゴリメソッドは、 `Export`属性を使用して Objective-C にメソッドをエクスポートするための通常のメカニズムを使用します。

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

マネージ拡張メソッドはすべて静的である必要がありますが、C# の拡張メソッドの標準構文を使用して、Objective-C インスタンスメソッドを作成することができます。

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

また、拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスになります。

完全な例:

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

この例では、native toUpper instance メソッドを NSString クラスに追加します。これは、Objective-C から呼び出すことができます。

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

これが役立つ1つのシナリオは、コードベースのクラスのセット全体にメソッドを追加することです。たとえば、すべての `UIViewController` インスタンスがローテーションできることをレポートします。

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

PreserveAttribute は、アプリケーションのサイズを小さくするために、アプリケーションの処理中に型または型のメンバーを保持するために、mtouch を指定するために使用されるカスタム属性です。

アプリケーションで静的にリンクされないメンバーはすべて、削除の対象となります。 したがって、この属性は、静的に参照されていないが、アプリケーションで依然として必要なメンバーをマークするために使用されます。

たとえば、型を動的にインスタンス化する場合、型の既定のコンストラクターを保存することもできます。 XML シリアル化を利用する場合、型のプロパティを保持することもできます。

この属性は、ある型のすべてのメンバーに適用するか、型自体に適用できます。 型全体を保持する場合は、型の [Preserve (AllMembers = true)] という構文を使用できます。

#### <a name="uikit"></a>UIKit

[Uikit](xref:UIKit)名前空間には、クラスのC#形式で CocoaTouch を構成するすべての UI コンポーネントに対する一対一のマッピングが含まれています。 API は、 C#言語で使用されている規則に従って変更されています。

C#デリゲートは、一般的な操作のために用意されています。 詳細については、「[デリゲート](#delegates)」セクションを参照してください。

#### <a name="opengles"></a>OpenGLES

OpenGLES については、[変更](xref:OpenTK)されたバージョンの[OpenTK](http://www.opentk.com/) API を配布します。これは、coregraphics データ型と構造体を使用するように変更された OpenGL へのオブジェクト指向のバインドであり、iOS で使用可能な機能のみを公開します。

OpenGLES 1.1 機能は、 [ES11.GL 型](xref:OpenTK.Graphics.ES11.GL)を介して使用できます。

OpenGLES 2.0 機能は、 [ES20.GL 型](xref:OpenTK.Graphics.ES20.GL)を介して使用できます。

OpenGLES 3.0 機能は、 [ES30.GL 型](xref:OpenTK.Graphics.ES30.GL)を介して使用できます。

### <a name="binding-design"></a>バインディングのデザイン

Xamarin.iOS は、基礎となる Objective-C プラットフォームへのバインディングにすぎません。 C# と Objective-C を上手く混在させるために、.net 型システムとディスパッチシステムを拡張しています。

P/Invoke が、Windows および Linux でネイティブライブラリを呼び出すための便利なツールであるように、また、Windows の COM 相互運用機能に対して IJW サポートを使用できるように、Xamarin はランタイムを拡張して、C# オブジェクトの Objective-C オブジェクトへのバインドオブジェクトをサポートします。

次のいくつかのセクションでは、Xamarin アプリケーションを作成しているユーザーには必要ありませんが、複雑なアプリケーションを作成するときに、開発者がどのような作業を行っているかを理解するのに役立ちます。

#### <a name="types"></a>種類

意味があるところでC# 、型は低レベルの Foundation 型ではなく、 C#宇宙に公開されます。  これは[`NSString` は `string` となります](~/ios/internals/api-design/nsstring.md)し、nsstring C#を公開するのではなく、厳密に型指定された配列を使用することを意味します。

一般に、Xamarin と Xamarin の設計では、基になる `NSArray` オブジェクトは公開されません。 代わりに、ランタイムは `NSArray`s を、一部の `NSObject` クラスの厳密に型指定された配列に自動的に変換します。 そのため、Xamarin では、NSArray を返す GetViews のように弱く型指定されたメソッドを公開していません。

```csharp
NSArray GetViews ();
```

代わりに、次のように、バインディングは厳密に型指定された戻り値を公開します。

```csharp
UIView [] GetViews ();
```

`NSArray`で公開されているメソッドの中には、`NSArray` を直接使用する必要があるものの、API バインドでは使用しないことをお勧めします。

また、CoreGraphics API から `CGRect`、`CGPoint` および `CGSize` を公開するのではなく、 **Classic API**では、開発者が保持するのと同様に `System.Drawing`、`RectangleF`、`PointF` の実装に置き換えました。OpenTK を使用する既存の OpenGL コード。 新しい64ビット**Unified API**を使用する場合は、COREGRAPHICS API を使用する必要があります。

#### <a name="inheritance"></a>継承

Xamarin. iOS API の設計により、開発者は派生クラスで "override" キーワードを使用してC# 型を拡張することも、 "base" C# キーワードを使用して基本実装にチェーンするのと同じ方法でネイティブの Objective-C の型を拡張することもできます。

このように設計することで、開発プロセスの一部としての Objective-C のセレクターの処理を避けることができます。これは、Objective-C システム全体が既に Xamarin.iOS ライブラリ内にラップされているためです。

#### <a name="types-and-interface-builder"></a>型と Interface Builder

Interface Builder によって作成された型のインスタンスである .NET クラスを作成する場合は、1つの `IntPtr` パラメーターを受け取るコンストラクターを指定する必要があります。
これは、マネージオブジェクトインスタンスをアンマネージオブジェクトにバインドするために必要です。
このコードは、次のような1つの行で構成されます。

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

#### <a name="delegates"></a>デリゲート

Objective-C と C# の "デリゲート" の意味は、各言語によって異なります。

Objective-C の世界では、CocoaTouch についてのオンラインのドキュメントで、デリゲートは通常、一連のメソッドに応答するクラスのインスタンスになります。 これは C# のインターフェイスによく似ていますが、メソッドが必ずしも必須ではないという点が異なります。

これらのデリゲートは、UIKit およびその他の CocoaTouch API で重要な役割を果たします。 これらは、さまざまなタスクを実行するために使用されます。

- コードに通知を提供します (C# または Gtk+ でのイベント配信に似ています)。
- データ視覚化コントロールのモデルを実装します。
- コントロールの動作を制御します。

プログラミングパターンは、コントロールの動作を変更するための派生クラスの作成を最小限にするように設計されています。 このソリューションは、長年にわたって他の GUI ツールキットによって実行されていたものと似ています。Gtk のシグナル、Qt スロット、Winforms イベント、WPF/Silverlight イベントなど。 数百のインターフェイス (アクションごとに1つ) を使用したり、不要なメソッドを実装したりする必要がないようにするには、省略可能なメソッドの定義をサポートします。 これは、すべてC#のメソッドを実装する必要があるインターフェイスとは異なります。

Objective-C クラスでは、このプログラミングパターンを使用するクラスは、通常`delegate`と呼ばれるプロパティを公開しています。このプロパティは、インターフェイスの必須の部分および 0 個以上のオプションの部分を実装するために必要です。

Xamarin では、これらのデリゲートにバインドする相互排他的な3つのメカニズムが用意されています。

1. [Via イベント](#via-events)。
2. [`Delegate` プロパティを使用した厳密な型指定](#strongly-typed-via-a-delegate-property)
3. [`WeakDelegate` プロパティを使用して緩やかに型指定された](#loosely-typed-via-the-weakdelegate-property)

たとえば、 [Uiwebview](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html)クラスを考えてみます。 これにより、 [delegate](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate)プロパティに割り当てられている[uiwebviewdelegate](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html)インスタンスにディスパッチされます。

##### <a name="via-events"></a>Via イベント

多くの種類の場合、Xamarin では、`UIWebViewDelegate` の呼び出しをイベントにC#転送する適切なデリゲートが自動的に作成されます。 `UIWebView`の場合:

- [WebViewDidStartLoad](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:)メソッドは、 [Uiwebview. loadstarted](xref:UIKit.UIWebView.LoadStarted)イベントにマップされます。
- [WebViewDidFinishLoad](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:)メソッドは、 [Uiwebview. loadfinished](xref:UIKit.UIWebView.LoadFinished)イベントにマップされます。
- [WebView: didFailLoadWithError](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:)メソッドは、 [Uiwebview. loaderror](xref:UIKit.UIWebView.LoadError)イベントにマップされます。

たとえば、次の単純なプログラムは、web ビューを読み込むときの開始時刻と終了時刻を記録します。

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

##### <a name="via-properties"></a>Via プロパティ

イベントは、イベントに複数のサブスクライバーが存在する可能性がある場合に便利です。 また、イベントは、コードからの戻り値がない場合にのみ制限されます。

コードが値を返すことが予想される場合は、プロパティに対して代わりにを選択します。 これは、オブジェクト内の特定の時刻に設定できるメソッドが1つだけであることを意味します。

たとえば、このメカニズムを使用して、`UITextField`のハンドラーの画面でキーボードを閉じることができます。

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

この場合の `UITextField`の `ShouldReturn` プロパティは、ブール値を返すデリゲートを引数として受け取り、返されるボタンが押されたときにテキストフィールドが何かを実行する必要があるかどうかを判断します。 このメソッドでは、呼び出し元に*true*を返しますが、画面からもキーボードを削除します (これは、textfield が `ResignFirstResponder`を呼び出すと発生します)。

##### <a name="strongly-typed-via-a-delegate-property"></a>デリゲートプロパティを使用して厳密に型指定された

イベントを使用しない場合は、独自の[Uiwebviewdelegate](xref:UIKit.UIWebViewDelegate)サブクラスを指定し、それを[uiwebview. delegate](xref:UIKit.UIWebView.Delegate)プロパティに割り当てることができます。 UIWebView. Delegate が割り当てられると、UIWebView イベントディスパッチ機構は機能しなくなり、対応するイベントが発生すると、UIWebViewDelegate メソッドが呼び出されます。

たとえば、次の単純型は、web ビューの読み込みにかかる時間を記録します。

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

上記のコードでは、次のようなコードが使用されています。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

上の例では、UIWebViewer を作成し、メッセージに応答するために作成したクラスである Notifier のインスタンスにメッセージを送信するように指示します。

このパターンは、特定のコントロールの動作を制御するためにも使用されます。たとえば、UIWebView の場合は、 [uiwebview.](xref:UIKit.UIWebView.ShouldStartLoad)を指定すると、`UIWebView` インスタンスは `UIWebView` がページを読み込むかどうかを制御できます。

このパターンは、いくつかのコントロールに対して必要に応じてデータを提供するためにも使用されます。 たとえば、 [Uitableview](xref:UIKit.UITableView)コントロールは強力なテーブルレンダリングコントロールで、ルックアンドコンテンツは、 [Uitableviewdatasource](xref:UIKit.UITableViewDataSource)のインスタンスによって駆動されます。

### <a name="loosely-typed-via-the-weakdelegate-property"></a>弱デリゲートプロパティを使用して緩やかに型指定

厳密に型指定されたプロパティに加えて、開発者が必要に応じて異なる方法でバインドできる、弱い型指定のあるデリゲートもあります。
厳密に型指定された `Delegate` プロパティが Xamarin で公開されているすべての場所で、対応する `WeakDelegate` プロパティも公開されます。

`WeakDelegate`を使用する場合は、[エクスポート](xref:Foundation.ExportAttribute)属性を使用してセレクターを指定することにより、クラスを適切に修飾する必要があります。 例えば:

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

`WeakDelegate` プロパティが割り当てられると、`Delegate` プロパティは使用されないことに注意してください。 さらに、継承された基本クラスで [Export] を使用する場合は、メソッドをパブリックメソッドにする必要があります。

## <a name="mapping-of-the-objective-c-delegate-pattern-to-c"></a>Objective-C デリゲートパターンの C\# へのマッピング

次のような Objective-C サンプルが表示されます。

```objc
foo.delegate = [[SomethingDelegate] alloc] init]
```

これにより、言語に対して "すべてのデリゲート" クラスのインスタンスを作成して構築し、値を foo 変数の delegate プロパティに割り当てるように指示します。 このメカニズムは、Xamarin.iOS C# でサポートされています。構文は次のとおりです。

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin では、Objective-C デリゲートクラスにマップされる厳密に型指定されたクラスを提供しています。 これらを使用するには、Xamarin の実装で定義されているメソッドをサブクラス化し、オーバーライドします。 動作の詳細については、以下の「モデル」を参照してください。

### <a name="mapping-delegates-to-c"></a>デリゲートの C\# へのマッピング

通常、UIKit では、2 つの形式で Objective-C デリゲートを使用します。

最初のフォームは、コンポーネントのモデルへのインターフェイスを提供します。 たとえば、リストビューのデータストレージ機能など、ビューに必要なデータを提供するメカニズムとして使用できます。  このような場合は、常に適切なクラスのインスタンスを作成し、変数を割り当てる必要があります。

次の例では、文字列を使用するモデルの実装に `UIPickerView` を提供しています。

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

2番目の形式は、イベントの通知を提供することです。 このような場合でも、前に説明した形式で API を公開しますC#が、イベントも提供します。これは、の簡単な操作と、のC#匿名デリゲートおよびラムダ式との統合により簡単に使用できるようにする必要があります。

たとえば、`UIAccelerometer` イベントにサブスクライブできます。

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

2つのオプションは意味がありますが、プログラマにとってはどちらか一方を選択する必要があります。 厳密に型指定された応答側/デリゲートの独自のインスタンスを作成しC#て割り当てると、イベントは機能しなくなります。 C#イベントを使用する場合、応答側/デリゲートクラスのメソッドは呼び出されません。

`UIWebView` 使用した前の例は、次C#のように3.0 ラムダを使用して記述できます。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```

#### <a name="responding-to-events"></a>イベントへの応答

目的 C のコードでは、複数のコントロールの複数のコントロールとプロバイダーのイベントハンドラーが、同じクラスでホストされることがあります。 クラスはメッセージに応答し、クラスがメッセージに応答する限り、オブジェクトをリンクすることができます。

これまでに説明したように、Xamarin C#は、イベントベースのプログラミングモデルと、デリゲートを実装する新しいクラスを作成して目的のメソッドをオーバーライドする目的 C のデリゲートパターンの両方をサポートしています。

また、複数の異なる操作のレスポンダーがすべて同じクラスのインスタンスでホストされている、Objective-C のパターンをサポートすることもできます。 ただし、これを行うには、Xamarin. iOS バインドの低レベルの機能を使用する必要があります。

たとえば、クラスが `UITextFieldDelegate.textFieldShouldClear`: message と `UIWebViewDelegate.webViewDidStartLoad`の両方に応答するようにする場合は、クラスの同じインスタンスで [Export] 属性宣言を使用する必要があります。

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

メソッドC#の名前は重要ではありません。重要なのは、[Export] 属性に渡される文字列だけです。

このスタイルのプログラミングを使用する場合は、 C#パラメーターが、ランタイムエンジンが渡す実際の型と一致していることを確認してください。

#### <a name="models"></a>モデル

UIKit ストレージ機能、またはヘルパークラスを使用して実装されたレスポンダーでは、これらは通常、ターゲット C コードではデリゲートとして参照され、プロトコルとして実装されます。

Objective-C プロトコルはインターフェイスに似ていますが、オプションのメソッドをサポートしています。つまり、プロトコルを機能させるためにすべてのメソッドを実装する必要はありません。

モデルを実装する方法は2つあります。 これは、手動で実装することも、既存の厳密に型指定された定義を使用することもできます。

手動のメカニズムは、Xamarin. iOS によってバインドされていないクラスを実装する場合に必要です。 非常に簡単です。

- ランタイムに登録するクラスにフラグを付ける
- オーバーライドする各メソッドに対して、実際のセレクター名を持つ [Export] 属性を適用します。
- クラスをインスタンス化し、それを渡します。

たとえば、次の例では、UIApplicationDelegate プロトコル定義にオプションのメソッドを1つだけ実装しています。

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

目標-C セレクター名 ("applicationDidFinishLaunching:") は Export 属性で宣言され、クラスは `[Register]` 属性で登録されます。

Xamarin iOS は、手動バインドを必要としない、厳密に型指定された宣言を提供します。この宣言はすぐに使用できます。 このプログラミングモデルをサポートするために、Xamarin の iOS ランタイムはクラス宣言で [Model] 属性をサポートしています。 これは、メソッドが明示的に実装されている場合を除き、クラス内のすべてのメソッドを接続しないことをランタイムに通知します。

これは、UIKit で、オプションのメソッドを使用してプロトコルを表すクラスを次のように記述することを意味します。

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

一部のメソッドのみを実装するモデルを実装する場合は、目的のメソッドをオーバーライドし、他のメソッドを無視するだけで済みます。 ランタイムは、元のメソッドではなく、上書きされたメソッドのみを目的の C 環境にフックします。

前の手動のサンプルに相当するものは次のとおりです。

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

その利点は、セレクター、引数の型、またはへC#のマッピングを検索するために、目的の C ヘッダーファイルを掘り下げ、厳密な型と共に Visual Studio for Mac から intellisense を取得する必要がないことです。

#### <a name="xib-outlets-and-c"></a>XIB アウトレットと C\#

> [!IMPORTANT]
> このセクションでは、XIB ファイルを使用するときの、IDE とアウトレットとの統合について説明します。 Xamarin Designer for iOS を使用する場合、次に示すように、IDE の プロパティ セクションの  **id > 名** に名前を入力することで、これがすべて置き換えられます。
>
> [![](images/designeroutlet.png "Entering an item Name in the iOS Designer")](images/designeroutlet.png#lightbox)
>
>IOS Designer の詳細については、 [Ios designer の概要](~/ios/user-interface/designer/introduction.md#how-it-works)に関するドキュメントを参照してください。

これは、アウトレットをと統合C#する方法についての低レベルの説明であり、Xamarin の上級ユーザー向けに提供されています。 Visual Studio for Mac を使用すると、フライトで生成されたコードを使用して、バックグラウンドで自動的にマッピングが行われます。

Interface Builder を使用してユーザーインターフェイスを設計すると、アプリケーションの外観をデザインするだけで、いくつかの既定の接続が確立されます。 プログラムによって情報を取得したり、実行時にコントロールの動作を変更したり、実行時にコントロールを変更したりする場合は、一部のコントロールをマネージコードにバインドする必要があります。

これを行うには、いくつかの手順を実行します。

1. **ファイルの所有者**に**アウトレット宣言**を追加します。
1. コントロールを**ファイルの所有者**に接続します。
1. UI と接続を XIB/NIB ファイルに格納します。
1. 実行時に NIB ファイルを読み込みます。
1. アウトレット変数にアクセスします。

手順 (1) ~ (3) については、Interface Builder を使用したインターフェイスの構築に関する Apple のドキュメントを参照してください。

Xamarin を使用する場合は、アプリケーションで UIViewController から派生したクラスを作成する必要があります。 次のように実装されています。

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

次に、NIB ファイルから ViewController を読み込むには、次の手順を実行します。

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

これにより、NIB からユーザーインターフェイスが読み込まれます。 ここで、アウトレットにアクセスするには、アクセスする必要があることをランタイムに通知する必要があります。 これを行うには、`UIViewController` サブクラスでプロパティを宣言し、[Connect] 属性を使用して注釈を設定する必要があります。 以下に例を示します。

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

プロパティの実装は、実際のネイティブ型の値を実際にフェッチして格納するものです。

Visual Studio for Mac と InterfaceBuilder を使用する場合は、このことについて心配する必要はありません。 Visual Studio for Mac は、宣言されたすべてのアウトレットを、プロジェクトの一部としてコンパイルされる部分クラスのコードと共に自動的にミラー化します。

#### <a name="selectors"></a>セレクター

Objective-C プログラミングの中核となる概念はセレクターです。 多くの場合、セレクターを渡す必要がある API や、コードがセレクターに応答することを期待する API が使用されます。

でC#新しいセレクターを作成するのは非常に簡単です。`ObjCRuntime.Selector`クラスの新しいインスタンスを作成し、それを必要とする API 内の任意の場所で結果を使用するだけです。 例:

```csharp
var selector_add = new Selector ("add:plus:");
```

メソッドがC#セレクター呼び出しに応答するには、`NSObject`型から継承する必要がありC# 、メソッドは、`[Export]`属性を使用してセレクター名で修飾する必要があります。 例えば:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

セレクター名が完全に一致している**必要があり**ます。これには、すべての中間コロンと末尾のコロン (":") が含まれます (存在する場合)。

#### <a name="nsobject-constructors"></a>NSObject コンストラクター

`NSObject` から派生した Xamarin のほとんどのクラスは、オブジェクトの機能に固有のコンストラクターを公開しますが、すぐには明らかではないさまざまなコンストラクターも公開します。

コンストラクターは次のように使用されます。

```csharp
public Foo (IntPtr handle)
```

このコンストラクターは、ランタイムがクラスをアンマネージクラスにマップする必要がある場合に、クラスをインスタンス化するために使用されます。 これは、XIB/NIB ファイルを読み込むときに発生します。  この時点で、目的 C ランタイムによってアンマネージ環境にオブジェクトが作成され、このコンストラクターが呼び出されてマネージ側が初期化されます。

通常は、ハンドルパラメーターを使用して基本コンストラクターを呼び出し、本文で必要な初期化を実行するだけです。

```csharp
public Foo ()
```

これは、クラスの既定のコンストラクターであり、NSObject クラスと、その間のすべてのクラスと、その間にあるすべてのクラスを初期化し、最後に、これをクラスのメソッド `init` にチェーンします。

```csharp
public Foo (NSObjectFlag x)
```

このコンストラクターは、インスタンスを初期化するために使用されますが、最後に Objective-C の "init" メソッドをコードが呼び出さないようにします。 通常、この値は、(コンストラクターで`[Export]`を使用するときに) 初期化を既に登録している場合、または別の平均を通じて初期化を実行した場合に使用します。

```csharp
public Foo (NSCoder coder)
```

このコンストラクターは、オブジェクトが NSCoding インスタンスから初期化されている場合に提供されます。 詳細については、「Apple の[アーカイブとシリアル化のプログラミングガイド](https://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)」を参照してください。

#### <a name="exceptions"></a>例外

Xamarin. iOS API の設計では、例外としてC# C の例外は発生しません。 この設計では、最初に目的の C にガベージを送信することはなく、生成する必要がある例外は、無効なデータを目的の C に渡す前に、バインディング自体によって生成されることになります。

#### <a name="notifications"></a>通知

IOS と OS X の両方で、開発者は、基になるプラットフォームによってブロードキャストされた通知をサブスクライブできます。 これを行うには、`NSNotificationCenter.DefaultCenter.AddObserver` メソッドを使用します。 `AddObserver` メソッドは2つのパラメーターを受け取ります。1つは、サブスクライブする通知です。もう1つは、通知が発生したときに呼び出されるメソッドです。

Xamarin. iOS と Xamarin. Mac の両方で、さまざまな通知のキーが、通知をトリガーするクラスでホストされます。 たとえば、`UIMenuController` によって発生した通知は、"Notification" という名前で終わる `UIMenuController` クラスの `static NSString` プロパティとしてホストされます。

### <a name="memory-management"></a>メモリ管理

Xamarin. iOS には、使用されなくなったときにリソースの解放を処理するガベージコレクターが用意されています。 ガベージコレクターに加えて、から派生したすべてのオブジェクトは、`System.IDisposable` インターフェイスを実装 `NSObject` ます。

#### <a name="nsobject-and-idisposable"></a>NSObject および IDisposable

`IDisposable` インターフェイスを公開することは、大きなメモリブロックをカプセル化する可能性があるオブジェクト (たとえば、`UIImage` が無害なポインターのように見えても、2メガバイトの画像を指している可能性があります) を解放する開発者を支援するための便利な方法です。重要なリソースと有限のリソース (ビデオデコードバッファーなど)。

NSObject は IDisposable インターフェイスを実装し、 [.Net Dispose パターン](https://msdn.microsoft.com/library/fs2xkftw.aspx)も実装します。 これにより、NSObject をサブクラス化する開発者は、Dispose の動作をオーバーライドし、必要に応じて独自のリソースを解放できます。 たとえば、次のようなビューコントローラーを考えてみます。

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

マネージオブジェクトが破棄されると、それは役に立たなくなります。 オブジェクトへの参照がまだ存在する可能性がありますが、この時点では、オブジェクトはすべてのインテントと目的に対して無効です。 一部の .NET Api では、破棄されたオブジェクトのメソッドにアクセスしようとすると、ObjectDisposedException がスローされます。たとえば、次のようになります。

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

変数 "image" にアクセスできる場合でも、実際には無効な参照であり、イメージを保持していた Objective-C オブジェクトを指していません。

ただし、でオブジェクトをC#破棄することは、必ずしもオブジェクトが破棄されるという意味ではありません。 オブジェクトへの参照C#を解放するだけです。 Cocoa 環境が独自に使用するための参照を保持している可能性があります。 たとえば、UIImageView の Image プロパティをイメージに設定してから、イメージを破棄した場合、基になる UIImageView は独自の参照を取得し、このオブジェクトへの参照をそのまま使用するようになります。

#### <a name="when-to-call-dispose"></a>Dispose を呼び出すタイミング

オブジェクトを削除するときに Mono が必要な場合は、Dispose を呼び出す必要があります。 考えられるユースケースは、NSObject が実際にメモリや情報プールなどの重要なリソースへの参照を保持していることを Mono が認識していない場合です。 そのような場合は、Mono がガベージコレクションサイクルを実行するのを待つのではなく、Dispose を呼び出して、メモリへの参照を直ちに解放する必要があります。

内部的には、Mono が[文字列からC# nsstring 参照](~/ios/internals/api-design/nsstring.md)を作成するときに、ガベージコレクターが実行する作業量を減らすために、それらをすぐに破棄します。 処理するオブジェクトの量が減るほど、GC の実行速度が速くなります。

#### <a name="when-to-keep-references-to-objects"></a>オブジェクトへの参照を保持する場合

自動メモリ管理には、参照がない限り、使用されていないオブジェクトが GC によって削除されるという副作用があります。 このような副作用が生じることもあります。たとえば、最上位レベルのビューコントローラーを保持するローカル変数を作成する場合や、最上位のウィンドウを作成し、それらのウィンドウを背面に置いた場合などです。

静的変数またはインスタンス変数内の参照をオブジェクトに対して保持しない場合、Mono はそのオブジェクトに対して Dispose () メソッドを呼び出し、オブジェクトへの参照を解放します。 これは未解決の参照である可能性があるため、Objective-C ランタイムがオブジェクトを破棄します。

## <a name="related-links"></a>関連リンク

- [バインド (フィールドを)](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
