---
title: Xamarin.iOS API の設計
description: このドキュメントでは、いくつかの適用、Xamarin.iOS Api と OBJECTIVE-C にこれらを関連付ける方法を設計するために使用される基本原則について説明します
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 56f9cbdae565f0d89463742377ec2311d8e375ac
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235052"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS API の設計

Core、Mono の一部である基本クラス ライブラリだけでなく[Xamarin.iOS](http://www.xamarin.com/iOS)さまざまな iOS 開発者は Mono とネイティブの iOS アプリケーションの作成を許可する Api のバインドが付属しています。

Xamarin.iOS の中核は、OBJECTIVE-C の世界と iOS CoreGraphics などの C ベースの Api のバインドでの c# 世界をブリッジするエンジンと相互運用機能および[OpenGL ES](#OpenGLES)します。

Objective C コードとの通信に低レベルのランタイムがで[MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime)します。 上のこのバインドに[Foundation](#MonoTouch.Foundation)、CoreFoundation、および[UIKit](#MonoTouch.UIKit)提供されます。

## <a name="design-principles"></a>設計の原則

これらは、(これらにも適用、Xamarin.Mac、OBJECTIVE-C での macOS での Mono バインド) の Xamarin.iOS バインディングの設計原則の一部を示します。

- に従って、 [Framework デザイン ガイドライン](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- 開発者は Objective C をサブクラス化クラスを許可します。

  - 既存のクラスから派生します。
  - チェーンに、基底コンス トラクターを呼び出す
  - # の上書きのシステムでメソッドのオーバーライドを行う必要があります。
  - サブクラス化は、c# の標準的な構成要素を使用する必要があります。

- 開発者は OBJECTIVE-C セレクターを公開しません
- 任意の Objective C ライブラリを呼び出すメカニズムを提供します。
- Objective C の一般的なタスクを簡単かつハード Objective C のタスクの可能なをこと
- C# のプロパティとして Objective C のプロパティを公開します。
- 厳密に型指定された API を公開します。

  - タイプ セーフを向上します。
  - 実行時エラーを最小限に抑える
  - 戻り値の型に対する IDE の IntelliSense を取得します。
  - IDE のポップアップのドキュメントでは、します。

- Api の IDE で探索をお勧めします。

  - たとえば、次のように厳密に型指定の配列を公開するには: の代わりに
    
    ```objc
    NSArray *getViews
    ```
    このような厳密な型を公開します。
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    Visual Studio for Mac API の閲覧中にオート コンプリートを実行する機能は、これには、すべての`System.Array`戻り値で使用できる操作でき、LINQ に参加する戻り値。

- ネイティブの c# 型:

  - [`NSString` なります `string`](~/ios/internals/api-design/nsstring.md)
  - 有効にする`int`と`uint`パラメーター (C#) 列挙型と c# の列挙体に列挙されています`[Flags]`属性
  - 型に依存しないのではなく`NSArray`オブジェクトとして厳密に型指定された配列の配列を公開します。
  - イベントと通知は、ユーザーの間で選択できるように。

    - 既定では厳密に型指定されたバージョン
    - 高度なユース ケースで厳密に型指定のバージョン

- Objective C のデリゲートのパターンをサポートしてください。

    - イベントのシステム (C#)
    - C# のデリゲートを公開 (ラムダ、匿名メソッド、および`System.Delegate`) をブロックとして Objective C Api

### <a name="assemblies"></a>アセンブリ

Xamarin.iOS には構成するアセンブリの数値が含まれています、 *Xamarin.iOS プロファイル*します。 [アセンブリ](~/cross-platform/internals/available-assemblies.md)ページには詳細についてはします。

### <a name="major-namespaces"></a>メジャーの名前空間 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/)名前空間により、開発者の c# と OBJECTIVE-C の世界の橋渡しを
これは、新しいバインディング、Cocoa # と Gtk # の経験を基に、iOS 向けに設計されています。

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) Objective C Foundation フレームワークは、iOS の一部であると相互運用するように設計、基本データ型とオブジェクト指向の OBJECTIVE-C でのプログラミングの基本クラスが名前空間には

C# から OBJECTIVE-C のクラスの階層にミラー化 Xamarin.iOS Objective C の基本クラスなど、 [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html)を使用して c# から使用できるように[Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)します。

この名前空間には、基になる Objective C Foundation 型のバインドが用意されていますが、いくつかのケースでにマップされています基になる型の .NET 型です。 例えば:

- 処理するのではなく[NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)と[NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html)、ランタイムはこれらとして c# が公開[文字列](xref:System.String)s の厳密に型指定された[配列](xref:System.Array)全体で sAPI です。

- さまざまなヘルパー Api は、開発者は Objective C Api、Api または Api Xamarin.iOS で現在バインドされていないその他の iOS のサード パーティのバインドを許可するのには、ここに公開されます。

Api のバインドの詳細については、次を参照してください。、 [Xamarin.iOS バインディング ジェネレーター](~/cross-platform/macios/binding/binding-types-reference.md)セクション。


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)型はすべて、OBJECTIVE-C のバインドの基盤です。 Xamarin.iOS 型 iOS CocoaTouch Api からの型の 2 つのクラスをミラー化: (CoreFoundation 型と通常呼ばれる) C の型と OBJECTIVE-C の種類 (これらの NSObject クラスから派生しています)。

アンマネージ型をミラー化される型ごとを使用してネイティブ オブジェクトを取得することは、[処理](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)プロパティ。

Mono は、オブジェクトのすべてのガベージ コレクションを提供中に、`Foundation.NSObject`実装、 [System.IDisposable](xref:System.IDisposable)インターフェイス。 これは、ガベージ コレクターでは、開始を待機することがなく任意指定 NSObject のリソースを明示的に解放できることを意味します。 これは、機能は、負荷の高い NSObjects、大量のデータ ブロックへのポインターを保持する可能性があります UIImages などを使用しているときに重要です。

を、型が確定的なファイナライズを実行する必要がある場合は、オーバーライド、 [NSObject.Dispose(bool) メソッド](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose)が Dispose にパラメーターが"bool disposing"、設定が true に場合ため、Dispose メソッドが呼び出されることと、ユーザーオブジェクトで明示的に呼び出す Dispose ()。 値が false の場合は、Dispose (bool disposing) メソッドがファイナライザーから、ファイナライザー スレッドで呼び出されることを意味します。 []()


##### <a name="categories"></a>カテゴリ

以降では、Xamarin.iOS 8.10 は、c# から OBJECTIVE-C のカテゴリを作成することです。

これを使用して、`Category`属性、属性に引数として拡張する型を指定します。 次の例は、NSString インスタンスの拡張します。

    [Category (typeof (NSString))]

各カテゴリのメソッドは Objective C を使用するメソッドをエクスポートするため、通常のメカニズムを使用して、`Export`属性。

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

すべてのマネージ拡張メソッドは静的である必要がありますが、c# での拡張メソッドの標準構文を使用して OBJECTIVE-C でインスタンス メソッドを作成することはできます。

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスになります。

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

この例は、NSString クラスは、OBJECTIVE-C から呼び出すことができますにネイティブ toUpper インスタンス メソッドが追加されます。

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

これは便利なシナリオの 1 つが、コードベース内のクラスのセット全体にメソッドを追加すること、たとえば、これは、すべて`UIViewController`インスタンス レポート回転できます。

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

PreserveAttribute は、そのサイズを小さくアプリケーションを処理するときにフェーズ中に、型または型のメンバーを保持するために、mtouch – Xamarin.iOS 展開ツール-を指示するために使用するカスタム属性です。

アプリケーションで静的にリンクされないメンバーはすべて、削除の対象となります。 そのため、この属性は、メンバーは静的に参照されていないが、これは、アプリケーションで必要にまだマークに使用されます。

たとえば、型を動的にインスタンス化する場合、型の既定のコンストラクターを保存することもできます。 XML シリアル化を利用する場合、型のプロパティを保持することもできます。

この属性は、ある型のすべてのメンバーに適用するか、型自体に適用できます。 型全体を保持する場合は、構文を使用することができます [保持する (AllMembers = true)] 型にします。

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/)名前空間には、すべての c# クラスの形式で CocoaTouch を構成する UI コンポーネントに一対一のマッピングが含まれています。 C# 言語で使用される規則に従う API が変更されました。

C# のデリゲートは、一般的な操作向けに提供されます。 参照してください、[デリゲート](#Delegates)詳細についてはします。

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

OpenGLES を配布、[バージョン変更](https://developer.xamarin.com/api/namespace/OpenTK/)の[OpenTK](http://www.opentk.com/) CoreGraphics データ型と構造体を使用するように変更されている OpenGL するためのオブジェクト指向のバインドとしてのみ公開している API、iOS で使用できる機能。

OpenGLES 1.1 の機能が記載されている、ES11.GL 型を介して使用可能な[ここ](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/)型。

OpenGLES 2.0 の機能が記載されている、ES20.GL 型を介して使用可能な[ここ](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/)型。

OpenGLES 3.0 の機能が記載されている、ES30.GL 型を介して使用可能な[ここ](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/)型。


### <a name="binding-design"></a>バインドのデザイン

Xamarin.iOS は Objective C の基盤となるプラットフォームへのバインドだけではありません。 .NET 型システムとより良い blend は c# と OBJECTIVE-C にディスパッチ システムを拡張します。

P/invoke は Windows および Linux 上のネイティブ ライブラリを呼び出す便利なツールまたは IJW としての Windows で COM 相互運用機能のサポートを使用できますと同様、Xamarin.iOS は OBJECTIVE-C オブジェクトへのバインド c# オブジェクトをサポートするためにランタイムを拡張します。

ユーザーが、Xamarin.iOS アプリケーションを作成する開発者を支援するために必要ないくつかのセクションでは、次の説明を理解すると、モ ノの完了し、複雑なアプリケーションを作成するときに役立ちます。



#### <a name="types"></a>種類

C# の型は、適切と思われる場所 c# universe に低レベルの Foundation 型ではなく公開されます。  つまり、 [API NSString ではなく、c# の"string"型を使用して](~/ios/internals/api-design/nsstring.md)NSArray を公開する代わりに厳密に型指定された c# の配列を使用するとします。

一般に、設計では、Xamarin.iOS および Xamarin.Mac、基になる`NSArray`オブジェクトは公開されません。 ランタイムが自動的に変換する代わりに、`NSArray`いくつかの厳密に型指定された配列に`NSObject`クラス。 そのため、Xamarin.iOS を返す、NSArray GetViews のような厳密に型指定されたメソッドを公開しません。

```csharp
NSArray GetViews ();
```

代わりに、バインディングは、このような厳密に型指定の戻り値を公開します。

```csharp
UIView [] GetViews ();
```

公開されるメソッドの数が少ない`NSArray`のコーナー ケースを使用する場合があります、`NSArray`を直接 API バインドでの使用はお勧めできませんが、します。

さらに、**クラシック API**を公開するのではなく`CGRect`、`CGPoint`と`CGSize`置き換えを持つ、CoreGraphics API から、`System.Drawing`実装`RectangleF`、 `PointF`と`SizeF`開発者が OpenTK を使用する既存の OpenGL コードを保持するのに役立つようにします。 新しい 64 ビットを使用する場合**Unified API**、CoreGraphics API を使用する必要があります。

<a name="Inheritance" />

#### <a name="inheritance"></a>継承

Xamarin.iOS API の設計では、c# 型を"override"キーワードを使用して、派生クラスで、"base"の c# のキーワードを使用して基底の実装チェーンが延長される同じ方法でネイティブの OBJECTIVE-C で型を拡張できます。

この設計は、OBJECTIVE-C でシステム全体が既に Xamarin.iOS ライブラリ内でラップするために、開発プロセスの一部として、OBJECTIVE-C セレクターと処理を避けるために開発者にできます。


#### <a name="types-and-interface-builder"></a>型とインターフェイス ビルダー

インターフェイス ビルダーによって作成された型のインスタンスである .NET クラスを作成するときに、1 つを受け取るコンス トラクターを提供する必要があります。`IntPtr`パラメーター。
これは、アンマネージ オブジェクトを使用して、管理対象のオブジェクト インスタンスをバインドに必要です。
コードは、次のように、1 つの行で構成されます。

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>デリゲート

OBJECTIVE-C と c# は、各言語で word デリゲート用の異なる意味を持ちます。

Objective C の世界と CocoaTouch に関するオンライン表示されているドキュメントでは、デリゲート、通常は、一連のメソッドに応答するクラスのインスタンスです。 これは、機能は、メソッドは常に必須にしないこと、相違点があります (C#) インターフェイス、非常に似ています。

これらのデリゲートは、UIKit およびその他の CocoaTouch Api で重要な役割を果たします。 さまざまな作業に使用されます。

-  コード (c# または Gtk + でのイベントの配信に似ています) への通知を提供します。
-  データ視覚化コントロールのモデルを実装します。
-  コントロールの動作を制御します。


コントロールの動作を変更する派生クラスの作成を最小限に抑えるには、プログラミングのパターンは設計されています。 このソリューションは、長年にわたって行うが他のツールキットに似た: Gtk の信号を Qt スロット、イベントの Winforms、WPF と Silverlight のイベント、という具合です。 (アクションごとに 1 つ) インターフェイスの数が多く、または必要はありませんが多すぎるメソッドを実装する開発者を避けるため、OBJECTIVE-C では省略可能なメソッドの定義をサポートしています。 これは、c# インターフェイスを実装するすべてのメソッドを必要とするよりも異なります。

OBJECTIVE-C のクラスでこのプログラミングのパターンを使用するクラスが、通常、という名前のプロパティを公開することを確認は`delegate`、これは、インターフェイスの必須部分と 0 個、または省略可能な複数の部分を実装するために必要です。

Xamarin.iOS でこれらのデリゲートにバインドする 3 つの相互に排他的なメカニズムが提供されています。

1.  [イベントを介して](#Via_Events)します。
2.  [使用して厳密に型指定された、`Delegate`プロパティ](#StrongDelegate)
3.  [使用して緩く型指定された、`WeakDelegate`プロパティ](#WeakDelegate)

たとえば、 [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html)クラス。 これをディスパッチする、 [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html)インスタンスに割り当てられる、[委任](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate)プロパティ。

<a name="Via_Events" />

##### <a name="via-events"></a>イベントの場合

多くの種類では、Xamarin.iOS は自動的に作成、転送する適切なデリゲート、 `UIWebViewDelegate` c# イベントへの呼び出し。 `UIWebView`の場合:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:)をメソッドにマップされて、 [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/)イベント。
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:)をメソッドにマップされて、 [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/)イベント。
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:)をメソッドにマップされて、 [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/)イベント。

たとえば、この単純なプログラムは、web の読み込みを表示するときに開始および終了時刻を記録します。

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>プロパティを使用して

イベントは、イベントには、複数のサブスクライバーがある可能性がある場合に便利です。 また、イベントの場合のみに制限は、コードからの戻り値が存在しません。

場合は、コードの戻り値が必要な場合を選択しました代わりにプロパティ。 これはオブジェクトで特定の時点で 1 つのメソッドを設定できることを意味します。

たとえば、画面のハンドラーで、キーボードを消去するこのメカニズムを使用できます、 `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`の`ShouldReturn`プロパティここでデリゲートを受け取り、引数としてブール値を返し、かどうか、テキスト フィールドがあるでしょうか、戻り値のボタンが押されると判断します。 このメソッドで返されます*true* 、呼び出し元に、キーボード、画面からも削除されていますが、(このエラーは、テキスト フィールドを呼び出す場合`ResignFirstResponder`)。

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>デリゲート プロパティを使用して厳密に型指定

イベントを使用していない場合は、行うことができます独自[UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/)サブクラスに割り当てると、 [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/)プロパティ。 UIWebView.Delegate、割り当てられているし、UIWebView イベントのディスパッチ メカニズムが機能しなく UIWebViewDelegate メソッドは、対応するイベントが発生したときに呼び出されます。

たとえば、この単純型は、web ビューの読み込みにかかる時間を記録します。

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

上記は、このようなコードで使用されます。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

上記は、UIWebViewer を作成し、通知、メッセージに応答を作成したクラスのインスタンスにメッセージを送信するように指示します。

UIWebView 場合は、たとえば特定のコントロールの動作を制御するこのパターンを使用しても、 [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/)プロパティを使用、`UIWebView`コントロールのインスタンスかどうか、`UIWebView`ロードは、かどうかをページします。

パターンは、いくつかのコントロールのオンデマンドでデータを提供するも使用されます。 たとえば、 [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/)コントロールは、強力なテーブルの描画コントロール – と外観と内容の両方のインスタンスによって駆動、 [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>疎 WeakDelegate プロパティを使用して型指定されました。

厳密に型指定のプロパティだけでなく、開発者に必要な場合は、モ ノを異なる方法でバインドを許可する弱い型指定されたデリゲートもあります。
厳密に型指定されたすべての場所で`Delegate`での Xamarin.iOS のバインド、対応するプロパティが公開される`WeakDelegate`プロパティも公開されています。

使用する場合、 `WeakDelegate`、正しくクラスを使用して、修飾することを担当、[エクスポート](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)セレクターを指定する属性。 例えば:

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

2 回に注意してください、`WeakDelegate`プロパティが割り当てられて、`Delegate`プロパティは使用されません。 さらに、[エクスポート] する継承された基本クラスでメソッドを実装する場合は、そのパブリック メソッド。


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Objective C デリゲート パターンから C へのマッピング&#35;

ときにこのような Objective C のサンプルを参照してください。

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

これには、言語を作成、"SomethingDelegate"クラスのインスタンスを構築、foo 変数にデリゲート プロパティに値を代入するように指示します。 このメカニズムが Xamarin.iOS でサポートされており、c# の構文は。

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS で Objective C にマップする厳密に型指定されたクラスはデリゲート クラスの指定があります。 これらを使用するには、をサブクラス化してする Xamarin.iOS の実装によって定義されているメソッドをオーバーライドします。 そのしくみの詳細については、セクション「モデル」以下を参照してください。


##### <a name="mapping-delegates-to-c35"></a>C へのデリゲートのマッピング&#35;

UIKit は一般に、2 つの形式で Objective C のデリゲートを使用します。

最初のフォームは、コンポーネントのモデルへのインターフェイスを提供します。 リスト ビューのデータ ストレージ設備などを表示、オンデマンドでデータを提供するメカニズムたとえば、としてです。  このような場合は、常に、適切なクラスのインスタンスを作成し、変数を割り当てる必要があります。

次の例では説明、`UIPickerView`文字列を使用するモデルの実装を使用します。

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

2 番目の形式は、イベントの通知を提供します。 ような場合、引き続き、上記で説明した形式で API を公開していますがも提供 c# イベント、クイック操作に使用する簡素化され、匿名デリゲートとラムダ式 (C#) と統合されたとして使用します。

たとえばにサブスクライブすることができます`UIAccelerometer`イベント。

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

ある意味、それらがプログラマとしては、どちらか一方を選択する必要がありますが 2 つのオプションがあります。 厳密に型指定された応答側/デリゲートのインスタンスを作成して割り当てる場合は、c# イベントは機能できません。 C# でイベントを使用する場合、応答側/デリゲート クラスのメソッドは呼び出されません。

前の例を使用する`UIWebView`このような c# 3.0 のラムダを使用して記述できます。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>イベントへの応答

Objective C コードでも複数のコントロールと複数のコントロールの情報のプロバイダーのイベント ハンドラーでホストされる同じクラス。 これは、クラスは、メッセージに応答するため、クラスがメッセージに応答している限り、オブジェクトを相互にリンクすることはできます。

として以前に詳細な Xamarin.iOS 両方 c#-イベント ベースのプログラミング モデルをサポート、および、OBJECTIVE-C でデリゲート パターンを作成できます、新しいクラスをデリゲートを実装して、必要なメソッドをオーバーライドします。

レスポンダーを複数の異なる操作がすべてホストされる場所、クラスの同じインスタンスでは OBJECTIVE-C でのパターンをサポートすることはも。 ただしこれは、バインドする Xamarin.iOS の低レベルの機能を使用する必要があります。

たとえば、両方に対応するクラスの場合、 `UITextFieldDelegate.textFieldShouldClear`: メッセージと`UIWebViewDelegate.webViewDidStartLoad`: クラスの同じインスタンスでは、[エクスポート] の属性の宣言を使用する必要が。

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

C#名前、メソッドは重要です。重要なは、[エクスポート] 属性に渡される文字列です。

このスタイルのプログラミングを使用する場合は、c# パラメーター、ランタイム エンジンに渡される実際の型と一致することを確認します。

<a name="Models" />

#### <a name="models"></a>モデル

UIKit ストレージ設備、またはヘルパー クラスを使用して実装されているレスポンダーでは、これらは通常と呼ばれて Objective C コードでデリゲートをプロトコルとして実装されます。

OBJECTIVE-C プロトコルは、インターフェイスに似ていますが、省略可能なメソッドをサポートする – は、すべてのメソッド、プロトコルが機能するために実装する必要があります。

モデルを実装する 2 つの方法はあります。 手動で実装するか、既存の厳密に型指定された定義を使用します。


Xamarin.iOS がバインドされていないクラスを実装しようとすると、手動の機構は必要があります。 これは非常に簡単に実行できます。

-  ランタイムと登録のため、クラスをフラグします。
-  各メソッドをオーバーライドする実際のセレクターの名前を持つ [エクスポート] 属性を適用します。
-  クラスのインスタンスを作成し、渡します。

たとえば、次省略可能なメソッドの 1 つだけで実装 UIApplicationDelegate プロトコルの定義。

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

OBJECTIVE-C セレクターの名前 ("applicationDidFinishLaunching:") は、エクスポート属性で宣言されてとクラスが登録されていると、`[Register]`属性。

Xamarin.iOS では、厳密に型指定された宣言、すぐに使用すると、手動のバインドを必要としないを提供します。 このプログラミング モデルをサポートするためには、Xamarin.iOS ランタイムは、[モデル] 属性をクラス宣言をサポートします。 これには、メソッドがない場合、クラス内のすべてのメソッドをする必要があります接続しないランタイムを明示的に実装が通知されます。

つまり、UIKit で、省略可能な方法でプロトコルを表すクラスは次のように書き込まれます。

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

だけ一部のメソッドを実装するモデルを実装するときに行う必要があるすべてが、興味のあるし、その他のメソッドを無視することは、メソッドをオーバーライドします。 ランタイムは、上書きのメソッドでは、OBJECTIVE-C で世界中に元のメソッドいないのみにフックします。

前の手動のサンプルと同じですが。

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

利点は、引数、または c# の場合へのマッピングの種類、セレクターを検索する Objective C ヘッダー ファイルを詳しく調査する必要がないから返された intellisense Visual Studio for Mac では、厳密な型と共に


#### <a name="xib-outlets-and-c35"></a>XIB Outlet と C&#35;

> [!IMPORTANT]
> XIB ファイルを使用する場合は、outlet と IDE の統合を説明します。 IOS 用の Xamarin のデザイナーを使用する場合これはすべて置き換えられますの下の名前を入力して**Identity > 名**次に示すように IDE の [プロパティ] セクション。
>
> [![](images/designeroutlet.png "IOS Designer で項目名を入力します。")](images/designeroutlet.png#lightbox)
>
>IOS Designer の詳細についてを参照してください、 [iOS Designer の概要](~/ios/user-interface/designer/introduction.md#how-it-works)ドキュメント。

これは c# を使用したアウトレットを統合する方法の低レベルの説明であり、Xamarin.iOS の上級ユーザー向けに提供されます。 Visual Studio for Mac のマッピング使用が終了すると自動的にバック グラウンドでを使用して、フライトでコードが生成されます。

インターフェイス ビルダーでは、ユーザー インターフェイスを設計するときは、アプリケーションの外観の設計にのみと、いくつかの既定の接続を確立します。 プログラムで情報を取得、実行時にコントロールの動作を変更、または実行時にコントロールを変更する場合、マネージ コードに一部のコントロールをバインドするために必要ななります。

これは、いくつかの手順で行います。

1.  追加、**アウトレット宣言**を**ファイルの所有者**します。
1.  コントロールを接続、**ファイルの所有者**します。
1.  UI に加えて、XIB/NIB ファイルへの接続を格納します。
1.  実行時に、NIB ファイルを読み込みます。
1.  アウトレット変数にアクセスします。


(3) までの手順 (1) は、Interface Builder でインターフェイスを構築するための Apple のドキュメントで説明します。

Xamarin.iOS を使用する場合、アプリケーションは UIViewController から派生したクラスを作成する必要があります。 実装されているこのようなこと。

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

読み込むには、ViewController NIB ファイルから、これを行います。

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

これにより、ユーザー インターフェイスは、NIB から読み込みます。 ここで、アウトレットにアクセスする、ランタイムにアクセスすることを通知する必要があります。 これを行う、`UIViewController`サブクラスがプロパティを宣言し、[Connect] 属性で注釈を付けることを必要があります。 以下に例を示します。

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

プロパティの実装は、実際にフェッチし、実際のネイティブ型の値を格納する 1 つです。

Mac および InterfaceBuilder 用 Visual Studio を使用する場合、これについて心配する必要はありません。 Visual Studio for Mac は、プロジェクトの一部としてコンパイルされた部分クラス内のコードで宣言されたすべてのアウトレットを自動的にミラー化します。

#### <a name="selectors"></a>セレクター

Objective C プログラミングの中核となる概念は、セレクターです。 セレクターを渡す必要がしたり、セレクターに応答するコードが必要ですが Api を遭遇する、多くの場合。

C# での新しいセレクターを作成することは非常に簡単 – だけの新しいインスタンスを作成する、`ObjCRuntime.Selector`クラスを必要とする API での任意の場所で結果を使用します。 例えば:

```csharp
var selector_add = new Selector ("add:plus:");
```

用の c# メソッドに応答セレクターの呼び出しから継承する必要があります、`NSObject`セレクターの名前を使用して型と c# のメソッドを装飾する必要があります、`[Export]`属性。 例えば:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

そのセレクターに注意してください名**する必要があります**すべて中間から末尾のコロンを含む完全に一致 (":")、存在する場合。

#### <a name="nsobject-constructors"></a>NSObject のコンス トラクター

ほとんどのクラスから派生する Xamarin.iOS`NSObject`オブジェクトの機能に固有のコンス トラクターが公開されますが、でもすぐに明らかではないさまざまなコンス トラクターが公開されます。

コンス トラクターは、次のように使用されます。

```csharp
public Foo (IntPtr handle)
```

このコンス トラクターは、ランタイムは、クラスをアンマネージ クラスにマップする必要がある場合に、クラスをインスタンス化に使用されます。 これは、XIB/NIB ファイルを読み込むときに発生します。  この時点では、OBJECTIVE-C ランタイムがアンマネージ環境で、オブジェクトを作成したし、マネージ側を初期化するためにこのコンス トラクターが呼び出されます。

通常、行う必要があるすべては、handle パラメーターと、本文には、基底コンス トラクターを呼び出し、必要な初期化を行います。

```csharp
public Foo ()
```

これは、クラスの既定のコンス トラクターとで Xamarin.iOS クラスを提供するの 間に、Foundation.NSObject クラスとすべてのクラスを初期化し、最後に、OBJECTIVE-C にこれをチェーンこれ`init`クラスのメソッド。

```csharp
public Foo (NSObjectFlag x)
```

このコンス トラクターはのインスタンスを初期化が最後に、OBJECTIVE-C で"init"メソッドを呼び出すことから、コードを防ぐために使用されます。 既に登録を初期化した場合に通常使用する (使用すると`[Export]`コンス トラクターで) ときに別の平均値で初期化完了済みまたはします。

```csharp
public Foo (NSCoder coder)
```

NSCoding インスタンスから、オブジェクトは初期化されている場所の場合、このコンス トラクターが提供されます。 詳細については、Apple を参照してください。[アーカイブおよびシリアル化プログラミング ガイド。](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>例外

Xamarin.iOS API の設計では、c# の例外として Objective C の例外は発生しません。 デザイン ガベージに送信されません、OBJECTIVE-C で世界最初の段階では、無効なデータがこれまでは前に、例外を生成する必要がありますがバインド自体によって生成されることは、OBJECTIVE-C で世界中に渡されます。

#### <a name="notifications"></a>通知

IOS および OS X の両方では、開発者は、基になるプラットフォームによっては、ブロードキャスト通知をサブスクライブできます。 使用してこれは、`NSNotificationCenter.DefaultCenter.AddObserver`メソッド。 `AddObserver` 1 つは、サブスクライブすることを示す通知; 通知が発生したときに呼び出されるメソッドは、他のメソッドは 2 つのパラメーターを受け取ります。

Xamarin.iOS および Xamarin.Mac の両方では、さまざまな通知のキーは、通知をトリガーするクラスにホストされます。 によって、通知が発生したなど、`UIMenuController`としてホストされる`static NSString`プロパティで、`UIMenuController`名「通知」で終わるクラス。

### <a name="memory-management"></a>メモリ管理

Xamarin.iOS には、ガベージ コレクターが使用されている不要になったときのリソースを解放する処理にがあります。 ガベージ コレクターだけでなくすべてのオブジェクトから派生した`NSObject`実装、`System.IDisposable`インターフェイス。

#### <a name="nsobject-and-idisposable"></a>NSObject および IDisposable

公開する、`IDisposable`インターフェイスが大きなメモリ ブロックをカプセル化するオブジェクトを解放するときに開発者を支援の便利な方法は、(たとえば、`UIImage`無害なポインターでは同じようになりますが、2 つメガバイトのイメージを指している可能性があります) とその他の重要であり、有限のリソース (ビデオのデコード バッファー) のようにします。

NSObject が IDisposable インターフェイスを実装しても、 [.NET Dispose パターン](http://msdn.microsoft.com/library/fs2xkftw.aspx)します。 これにより、開発者は、サブクラスである NSObject を Dispose の動作をオーバーライドし、オンデマンドで独自のリソースを解放します。 たとえば、多数のイメージを保持するようにこのビュー コント ローラーがあるとします。

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

マネージ オブジェクトが破棄されたときに役に立たなくなった。 オブジェクトへの参照がまだありますが、オブジェクトが正しくありません消しての時点でします。 一部の .NET Api は、たとえば破棄されたオブジェクトでメソッドにアクセスしようとすると ObjectDisposedException をスローすることによって、これを確認します。

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

場合でも、「イメージ」変数にアクセスすることができますが、それが本当に参照が無効ですになり、イメージが保持されている OBJECTIVE-C オブジェクトへのポインター。

ただし、c# でオブジェクトを破棄しないわけでは、オブジェクトが破棄とは限りません。 C# がオブジェクトへの参照を解除するだけです。 周囲の参照を使用して独自の可能性があります Cocoa 環境を維持していることができます。 たとえば、イメージ、UIImageView のイメージのプロパティを設定すると、イメージを破棄し、基になる UIImageView 独自の参照を実行したし、が完了するまでにこのオブジェクトへの参照を保持それを使用します。

#### <a name="when-to-call-dispose"></a>Dispose を呼び出すときに

オブジェクトの除去で Mono 必要がある場合は、Dispose を呼び出す必要があります。 可能なユース ケースは、Mono は、NSObject が実際に、メモリや、情報のプールなどの重要なリソースへの参照を保持していることに関する知識を持たない場合です。 その場合、Mono、ガベージ コレクション サイクルを実行するを待つのではなく、メモリへの参照を直ちに解放するために Dispose を呼び出す必要があります。

Mono が作成時に内部的には、 [NSString を c# の文字列から参照](~/ios/internals/api-design/nsstring.md)、ガベージ コレクターが実行するには作業量を削減するには、すぐに破棄されます。 GC が早く使用すると、処理するには、約少数のオブジェクトが実行されます。

#### <a name="when-to-keep-references-to-objects"></a>オブジェクトへの参照を保持する場合

自動メモリ管理には、1 つの副作用は、それらへの参照がない限りに GC 使用されていないオブジェクトの rid を取得します。 これは、場合もあります副作用がある驚くべきことなど、最上位レベルのビュー コント ローラーを保持するローカル変数を作成することも、最上位レベルのウィンドウとそれらを持つが背後消える場合。

オブジェクトへの参照を静的またはインスタンス変数を格納しないで、Mono には、Dispose() メソッドを呼び出してさいわいし、オブジェクトへの参照を解放します。 のみの未解決の参照などがありますので、OBJECTIVE-C ランタイムはオブジェクトを破棄します。

## <a name="related-links"></a>関連リンク

- [フィールドをバインド](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
