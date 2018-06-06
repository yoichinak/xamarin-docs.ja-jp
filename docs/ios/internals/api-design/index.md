---
title: Xamarin.iOS API の設計
description: このドキュメントでは、Xamarin.iOS Api と目標 C になります。 この関係を構築するために使用される基本原則の一部について説明します
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: a7e508ddd086936a3ffea9d76cde7d896fe4d1f3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787355"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS API の設計

モノラルの一部である基本クラス ライブラリのコアだけでなく[Xamarin.iOS](http://www.xamarin.com/iOS)さまざまな iOS 開発者は、Mono でネイティブの iOS アプリケーションの作成を許可するための Api のバインドが付属しています。

結びます。 また、c# world Objective C の世界に加え、iOS CoreGraphics と同様に C ベースの Api のバインドと相互運用機能のエンジンがある、Xamarin.iOS の中核と[OpenGL ES](#OpenGLES)です。

Objective C コードとの通信に低レベルのランタイムは[MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime)です。 このため、バインドの上に[Foundation](#MonoTouch.Foundation)、CoreFoundation、および[UIKit](#MonoTouch.UIKit)提供されます。

## <a name="design-principles"></a>デザインの原則

(そのにも適用 Xamarin.Mac、macos Objective C の Mono バインド)、Xamarin.iOS バインド用の設計原則のいくつか次に示します。

- 以下の[Framework デザイン ガイドライン](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- 開発者はサブクラス Objective C のクラスを使用するには。

  - 既存のクラスから派生します。
  - チェーンに、基底コンス トラクターを呼び出す
  - # のオーバーライド システムとメソッドのオーバーライドを行う必要があります。
  - サブクラス化する必要があります (C#) 標準のコンストラクトを使用

- Objective C のセレクターを開発者に公開しないでください。
- Objective C の任意のライブラリを呼び出すための機構を提供します。
- Objective C の一般的なタスクを簡単かつハード Objective C のタスクの可能なを作成します。
- Objective C のプロパティは c# プロパティとして公開します。
- 厳密に型指定された API を公開します。

  - タイプ セーフを向上します。
  - ランタイム エラーを最小限に抑える
  - 戻り値の型の IDE の IntelliSense を取得します。
  - IDE のポップアップのドキュメントでは、します。

- Api の IDE の調査をお勧めします。

  - たとえば、次のように、弱い型指定の配列を公開するには: の代わりに
    
    ```objc
    NSArray *getViews
    ```
    次のように、厳密な型を公開します。
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    これにより、Visual Studio for Mac API の参照中に自動補完を行うには、により、すべて、`System.Array`返される値で使用できる操作でき、戻り値を LINQ に参加します。

- ネイティブの c# 型:

  - [`NSString` なります `string`](~/ios/internals/api-design/nsstring.md)
  - 有効にする`int`と`uint`c# 列挙型と C# の場合と列挙体に列挙されている必要があるパラメーター`[Flags]`属性
  - 型に依存しないのではなく`NSArray`オブジェクトが厳密に型指定された配列としての配列を公開します。
  - イベントと通知は、のいずれかをユーザーに付与します。

    - 既定では、厳密に型指定されたバージョン
    - 高度なユース ケースの弱い型指定のバージョン

- Objective C デリゲート パターンをサポートしてください。

    - C# イベント システム
    - デリゲート (C#) を公開 (ラムダ、匿名メソッド、および`System.Delegate`) ブロックと OBJECTIVE-C Api を

### <a name="assemblies"></a>アセンブリ

Xamarin.iOS には形成するアセンブリの数が含まれています、 *Xamarin.iOS プロファイル*です。 [アセンブリ](~/cross-platform/internals/available-assemblies.md)ページには詳細についてはします。

### <a name="major-namespaces"></a>メジャーの名前空間 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/)名前空間により、c# と目標 C. の長所をブリッジする開発者
これは、Cocoa # と Gtk # から経験に基づいて、iOS 向けに設計された、新しいバインディングです。

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/)名前空間には、基本データ型は、iOS の一部である OBJECTIVE-C Foundation フレームワークとの相互運用するように設計され、オブジェクト指向目標 C のプログラミングの基本クラスになります

Xamarin.iOS ミラーリング (C#) 目標 C からのクラス階層です。 Objective C の基本クラスなど、 [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html)経由での c# から使用できるように、 [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)です。

この名前空間には、基になる OBJECTIVE-C Foundation 型のバインドを提供しますが、いくつかのケースでお基になる型にマップの .NET 型です。 例えば:

- 処理するのではなく[NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)と[NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html)、ランタイムに公開するこれらとして c#[文字列](https://developer.xamarin.com/api/type/System.String/)s 厳密に型指定された[配列](https://developer.xamarin.com/api/type/System.Array/)全体にわたって sAPI です。

- さまざまなヘルパー Api は、開発者はサード パーティ Objective C Api では、Api または Api Xamarin.iOS によって現在バインドされていないその他の iOS のバインドを使用できるようにここで公開されます。

Api のバインドの詳細については、次を参照してください。、 [Xamarin.iOS バインディング ジェネレーター](~/cross-platform/macios/binding/binding-types-reference.md)セクションです。


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)型は Objective C のすべてのバインドの基礎です。 Xamarin.iOS 型ミラー iOS CocoaTouch Api からの型の 2 つのクラス: C 型 (通常呼ばれる CoreFoundation 型) と OBJECTIVE-C 型 (これらの NSObject クラスから派生すべて)。

アンマネージ型をミラー化の種類ごとに、可能であればを介してネイティブ オブジェクトを取得する、[処理](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)プロパティです。

モノラルはすべて、オブジェクトのガベージ コレクションを提供中に、`Foundation.NSObject`を実装する、 [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/)インターフェイスです。 これは、ガベージ コレクターには、開始を待機することがなく任意指定 NSObject のリソースを明示的に解放できることを意味します。 これは、機能は、重 NSObjects、大きいブロックのデータへのポインターを保持する可能性があります UIImages などを使用しているときに重要です。

場合は、型は、決定的な終了処理を実行する必要があります、オーバーライド、 [NSObject.Dispose(bool) メソッド](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose)Dispose のパラメーターが"bool disposing"かどうか、設定には、true に呼び出すことを意味、Dispose メソッドはされているため、ユーザーオブジェクトで明示的に呼び出す Dispose () です。 値が false の場合は、Dispose (bool disposing) メソッドがファイナライザーからファイナライザー スレッドで呼び出されることを意味します。 []()


##### <a name="categories"></a>カテゴリ

以降では Xamarin.iOS 8.10 は c# から Objective C のカテゴリを作成することです。

これを使用して、`Category`属性、属性の引数として拡張する型を指定します。 次の例は、NSString インスタンスの拡張します。

    [Category (typeof (NSString))]

各カテゴリのメソッドが Objective C を使用するメソッドをエクスポートするため、通常のメカニズムを使用して、`Export`属性。

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

すべてのマネージ拡張メソッドは静的である必要がありますが、c# での拡張メソッドの標準構文を使用して、OBJECTIVE-C インスタンス メソッドを作成すること。

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

され、拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスになります。

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

この例はネイティブ toUpper インスタンス メソッドに追加されます、NSString クラス、目標 C. から呼び出すことができます。

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

1 つのシナリオがこれは便利ですが、コードベース内のクラスのセット全体にメソッドを追加する、たとえば、これは、すべて`UIViewController`のインスタンスは、回転できることをレポートします。

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

PreserveAttribute は、そのサイズを小さく、アプリケーションが処理されるタイミング フェーズ中にへの通知 mtouch – Xamarin.iOS 配置ツール –、型、または、型のメンバーを保持するために使用されるカスタム属性です。

アプリケーションで静的にリンクされないメンバーはすべて、削除の対象となります。 そのため、この属性はメンバーを静的に参照されないが、アプリケーションで引き続き必要なことを示すために使用されます。

たとえば、型を動的にインスタンス化する場合、型の既定のコンストラクターを保存することもできます。 XML シリアル化を利用する場合、型のプロパティを保持することもできます。

この属性は、ある型のすべてのメンバーに適用するか、型自体に適用できます。 構文を使用するには型全体を保持する場合は、[を保持する (AllMembers = true)] の型にします。

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/)名前空間には、すべての c# クラスの形式で CocoaTouch を構成する UI コンポーネントに一対一のマッピングが含まれています。 この API は、c# 言語で使用される規則に従って変更されています。

C# のデリゲートは、一般的な操作に提供されます。 参照してください、[デリゲート](#Delegates)詳細についてはします。

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

OpenGLES、お配布、[バージョン変更](https://developer.xamarin.com/api/namespace/OpenTK/)の[OpenTK](http://www.opentk.com/) API、CoreGraphics データ型と構造体を使用するように変更した OpenGL をオブジェクト指向のバインドだけでなくだけを公開する、iOS で利用可能な機能です。

OpenGLES 1.1 の機能が記載されている、ES11.GL 型を介して使用できる[ここ](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/)型です。

OpenGLES 2.0 の機能が記載されている、ES20.GL 型を介して使用できる[ここ](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/)型です。

OpenGLES 3.0 の機能が記載されている、ES30.GL 型を介して使用できる[ここ](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/)型です。


### <a name="binding-design"></a>バインディングのデザイン

Xamarin.iOS は、基になる OBJECTIVE-C プラットフォームへのバインドだけではありません。 .NET 型システムと向上 blend は c# および目標 C にディスパッチ システムを拡張します。

P/invoke は Windows および Linux でのネイティブ ライブラリを呼び出すための便利なツールまたは IJW としてサポートを使用して COM 相互運用の Windows と同様、Xamarin.iOS は Objective C のオブジェクトへのバインド (C#) オブジェクトをサポートするためにランタイムを拡張します。

いくつかのセクションでは、Xamarin.iOS アプリケーションを作成するが、開発者を支援するユーザーの必要はありません、次の説明を理解する方法とという点はより複雑なアプリケーションを作成するときに役立ちます。



#### <a name="types"></a>種類

理範囲、c# の型は、c# universe への低レベルの Foundation 型の代わりに公開されます。  つまり、 [API NSString の代わりに、c# の「文字列」型を使用して](~/ios/internals/api-design/nsstring.md)NSArray を公開するのではなく c# の厳密に型の配列を使用しています。

一般に、設計では、Xamarin.iOS および Xamarin.Mac、基になる`NSArray`オブジェクトは公開されません。 ランタイムが自動的に変換する代わりに、`NSArray`いくつかの厳密に型指定された配列に s`NSObject`クラスです。 そのため、Xamarin.iOS は弱い型指定を返す、NSArray GetViews のようなメソッドを公開しません。

```csharp
NSArray GetViews ();
```

代わりに、このバインディングは、次のように、厳密に型指定の戻り値を公開します。

```csharp
UIView [] GetViews ();
```

公開されるメソッドの数が少ない`NSArray`を使用する場所のコーナー ケースの`NSArray`、直接の API バインドでの使用は推奨されませんが、します。

さらに、 **Classic API**を公開するのではなく`CGRect`、`CGPoint`と`CGSize`CoreGraphics API から置き換えましたを持つ、`System.Drawing`実装`RectangleF`、 `PointF`と`SizeF`開発者が OpenTK を使用する既存の OpenGL コードを保持するように、役立つ情報を入手します。 新しい 64 ビットを使用するときに**Unified API**、CoreGraphics API を使用する必要があります。

<a name="Inheritance" />

#### <a name="inheritance"></a>継承

Xamarin.iOS API の設計では、c# の型、派生クラスで"override"キーワードを使用するだけでなく、基底の実装に「基本」の c# のキーワードを使用してチェーンを拡張することと同じ方法でネイティブ Objective C 型を拡張するをすることができます。

この設計では、開発プロセスの一部として、OBJECTIVE-C セレクターと処理を避けるために開発者 OBJECTIVE-C システム全体は、Xamarin.iOS ライブラリ内にラップされた既にあるためです。


#### <a name="types-and-interface-builder"></a>型およびインターフェイスのビルダー

インターフェイスのビルダーによって作成された型のインスタンスである .NET クラスを作成するときに、1 つを受け取るコンス トラクターを提供する必要があります。`IntPtr`パラメーター。
これは、アンマネージ オブジェクトを使用して、マネージ オブジェクト インスタンスをバインドする必要があります。
コードは、次のように、1 つの行で構成されます。

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>デリゲート

Objective C および C# の場合は、各言語で word デリゲートの異なる意味を持ちます。

Objective C world と CocoaTouch に関するオンライン表示されているドキュメントでは、デリゲート、通常は、一連のメソッドに応答するクラスのインスタンスです。 これは、機能は、メソッドは常に必須にしないしていることの違いがある c# インターフェイスとよく似ています。

これらのデリゲートは、UIKit およびその他の CocoaTouch Api で重要な役割を果たします。 さまざまなタスクの実行に使用されます。

-  コード (c# または Gtk + でのイベントの配信に似ています) への通知を提供します。
-  データの視覚エフェクトのコントロールのモデルを実装します。
-  ドライブに対して、コントロールの動作。


プログラミングのパターンは、コントロールの動作を変更する派生クラスの作成を最小限に抑えるように設計されました。 このソリューションは長年にわたって他のツールキットの作業内容を基本と似ています。 Gtk の信号を、Qt スロット、Winforms イベント、WPF/Silverlight イベントなどです。 何百もの (各操作に対して 1 つ) インターフェイスを持つ必要がない多数のメソッドを実装する開発者を要求したりを回避するのには、Objective C は、省略可能なメソッド定義をサポートします。 これは、c# のインターフェイスを実装するすべてのメソッドを必要とするよりも異なります。

Objective C のクラスに表示されますこのプログラミング パターンを使用するクラスは、通常、プロパティを公開`delegate`、これは、インターフェイスの必須部分と 0 個以上、オプションの部分を実装するために必要です。

Xamarin.iOS でこれらのデリゲートにバインドする 3 つの相互に排他的なメカニズムが提供されています。

1.  [イベントを介して](#Via_Events)です。
2.  [厳密に型を使用して、`Delegate`プロパティ](#StrongDelegate)
3.  [弱い型定義を介して、`WeakDelegate`プロパティ](#WeakDelegate)

たとえば、 [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html)クラスです。 ディスパッチしてこれを[UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html)に割り当てられているインスタンス、[委任](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate)プロパティです。

<a name="Via_Events" />

##### <a name="via-events"></a>イベントを介して

多くの種類、Xamarin.iOS が自動的に作成、転送する適切なデリゲート、 `UIWebViewDelegate` c# イベントを呼び出します。 `UIWebView`の場合:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:)メソッドのマップ先、 [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/)イベント。
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:)メソッドのマップ先、 [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/)イベント。
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:)メソッドのマップ先、 [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/)イベント。

たとえば、この単純なプログラムは、web の読み込みを表示するときに開始および終了時刻を記録します。

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Via のプロパティ

イベントは、イベントに 2 つ以上のサブスクライバーがある可能性があるときに便利です。 また、イベントの場合のみに制限は、コードからの戻り値が存在しません。

場合は、値を返すコードを必要とする箇所を選択しました代わりにプロパティです。 これはオブジェクトで指定された、一度に 1 つのメソッドを設定できることを意味します。

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

`UITextField`の`ShouldReturn`プロパティここでは、引数としてブール値を返し、かどうか、テキスト フィールドが操作を行います戻り値のボタンが押されるとを決定するデリゲート。 このメソッドで戻り*true* 、呼び出し元に削除する必要も、キーボードの画面が (テキスト フィールドを呼び出すときに発生`ResignFirstResponder`)。

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>デリゲート プロパティ経由で厳密に型指定

イベントを使用していない場合は、使用できる独自[UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/)サブクラスに割り当てると、 [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/)プロパティです。 UIWebView.Delegate が割り当てられると、UIWebView イベント ディスパッチ メカニズムが機能しなくと対応するイベントの発生時に UIWebViewDelegate メソッドが呼び出されます。

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

上記を次のようにコードで使用します。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

上記は作成、UIWebViewer し Notifier、メッセージに応答するために作成してクラスのインスタンスにメッセージを送信するように指示します。

UIWebView ケースの例では、特定のコントロールの動作を制御するこのパターンを使用しても、 [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/)プロパティでは、`UIWebView`コントロールをインスタンスかどうか、`UIWebView`ロードは、ページまたはされません。

パターンは、必要に応じて、いくつかのコントロールにデータを提供するも使用されます。 たとえば、 [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/)コントロールは、強力なテーブルの描画コントロール – と外観と内容の両方が存在する背景のインスタンス、 [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>WeakDelegate プロパティ経由で弱い型定義

厳密に型指定のプロパティだけでなく弱い型指定されたデリゲートにより、開発者は必要な場合は、異なる方法で処理をバインドすることもできます。
厳密に型指定されたすべての場所で`Delegate`で Xamarin.iOS のバインディングを対応するプロパティが公開される`WeakDelegate`プロパティが公開されるもします。

使用する場合、 `WeakDelegate`、クラスを使用して、正しくを修飾することを担当する場合、[エクスポート](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)セレクターを指定する属性。 例えば:

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

2 回に注意してください、`WeakDelegate`プロパティが割り当てられて、`Delegate`プロパティは使用されません。 さらに、[エクスポート] する継承された基本クラスのメソッドを実装する場合は、する必要がありますが、パブリック メソッド。


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Objective C デリゲート パターンから C へのマッピング&#35;

ときに次のような Objective C のサンプルを参照してください。

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

これは、作成、"SomethingDelegate"クラスのインスタンスを構築し、foo 変数、デリゲート プロパティに値を代入する言語を指示します。 このメカニズムは、Xamarin.iOS によってサポートされ、c# の構文は。

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS で Objective C にマップされる厳密に型指定されたクラスにデリゲート クラスの指定があります。 を使用するには、サブクラス化およびする Xamarin.iOS の実装によって定義されたメソッドをオーバーライドします。 これらの動作方法の詳細については、セクション「モデル」以下を参照してください。


##### <a name="mapping-delegates-to-c35"></a>C へのデリゲートのマッピング&#35;

UIKit は一般に、2 つの形式で Objective C のデリゲートを使用します。

最初の形式では、コンポーネントのモデルへのインターフェイスを提供します。 などのリスト ビューのデータ ストレージ設備などを表示、要求時にデータを提供するメカニズムです。  このような場合は、常に、適切なクラスのインスタンスを作成し、変数を割り当てる必要があります。

次の例で提供する、`UIPickerView`文字列を使用するモデルの実装を持つ。

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

2 番目の形式は、イベントの通知を提供します。 ような場合、引き続き、上記で説明した形式で API を公開しておも用意されて c# イベント クイック操作を使用する方が簡単と匿名デリゲートとラムダ式 (C#) に統合する必要があります。

たとえば、サブスクライブする`UIAccelerometer`イベント。

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

意味をなしません、プログラマは 1 つまたはもう一方を選択する必要がありますが、2 つのオプションのとおりです。 厳密に型指定された応答側/デリゲートのインスタンスを作成して割り当てる場合は、c# イベントは機能できません。 C# でイベントを使用する場合、応答側/デリゲート クラスのメソッドは決して呼び出されません。

使用した前の例`UIWebView`次のように c# 3.0 形式のラムダを使用して記述できます。

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>イベントへの応答

Objective C コードでも複数のコントロールと複数のコントロールの情報のプロバイダーのイベント ハンドラーでホストされる同じクラスです。 これは、クラスは、メッセージに応答するため、およびクラスがメッセージに応答している限り、オブジェクトを相互にリンクすることはできます。

以前詳細として Xamarin.iOS 両方 c#-イベント ベースのプログラミング モデルをサポートしていると OBJECTIVE-C デリゲート パターンは、場所を作成する新しいクラスをデリゲートを実装し、目的のメソッドをオーバーライドします。

可能であればも OBJECTIVE-C のパターンをサポートするために複数のさまざまな操作の応答がすべてホストされているクラスの同じインスタンスでします。 これを行うには、Xamarin.iOS バインディングの低レベルの機能を使用する必要があります。

たとえば、両方に応答には、クラスの場合、 `UITextFieldDelegate.textFieldShouldClear`: メッセージと`UIWebViewDelegate.webViewDidStartLoad`: クラスの同じインスタンスでは、[エクスポート] の属性の宣言を使用する必要が。

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

メソッドの c# 名前が重要です。重要なは、[エクスポート] 属性に渡される文字列です。

このスタイルのプログラミングを使用する場合は、c# パラメーターが、ランタイム エンジンが送信される実際の型と一致することを確認します。

<a name="Models" />

#### <a name="models"></a>モデル

UIKit ストレージ設備、またはヘルパー クラスを使用して実装されているレスポンダーでは、これらは通常と呼ばれる Objective C コードで、デリゲートとプロトコルとして実装されます。

Objective C プロトコルは、インターフェイスに似ていますが、省略可能なメソッドをサポートしている – は、すべてのメソッド、プロトコルが動作するために実装する必要があります。

モデルを実装する 2 つの方法があります。 手動で実装するか、既存の厳密な型定義を使用します。


Xamarin.iOS によってバインドされていないクラスを実装しようとすると、手動の機構は必要があります。 行うには非常に簡単ですから。

-  ランタイムと登録のため、クラスのフラグを設定します。
-  各メソッドをオーバーライドする実際のセレクターの名前を持つ [エクスポート] 属性を適用します。
-  クラスのインスタンスを作成し、それを渡します。

たとえば、次を実装、省略可能なメソッドの 1 つだけ UIApplicationDelegate プロトコル定義に。

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective C セレクターの名前 ("applicationDidFinishLaunching:") がエクスポート属性で宣言されているクラスが登録されると、`[Register]`属性。

Xamarin.iOS は、厳密に型指定された宣言を使用して、手動のバインドを必要としないを提供します。 このプログラミング モデルをサポートするためには、Xamarin.iOS ランタイムは、クラス宣言で、[モデル] 属性をサポートします。 これは、その必要がありますネットワーク上でないクラスでは、すべてのメソッドをメソッドがない場合、ランタイムが明示的に実装したに通知します。

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

のみ一部のメソッドを実装するモデルを実装する場合は、行う必要があるすべてを興味のあるし、他のメソッドを無視するメソッドをオーバーライドします。 ランタイムはのみ上書きメソッド、OBJECTIVE-C 世界中に元のメソッドいないをフックするためです。

前の手動のサンプルには。

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

利点は、セレクター、引数、または C# の場合へのマッピングの種類を検索する Objective C ヘッダー ファイルを詳しく調べをする必要がないことになる intellisense から Visual Studio for Mac、厳密な型と共に


#### <a name="xib-outlets-and-c35"></a>XIB コンセントと C&#35;

> [!IMPORTANT]
> XIB ファイルを使用する場合は、IDE との統合をコンセントを説明します。 Xamarin デザイナーを使用して、iOS 用、これはすべて置き換えられますで名前を入力して**Identity > 名前**で次のように、IDE の Properties セクション。
>
> [![](images/designeroutlet.png "IOS デザイナー内で項目名を入力します。")](images/designeroutlet.png#lightbox)
>
>IOS デザイナーの詳細についてを参照してください、 [iOS デザイナーの概要](~/ios/user-interface/designer/introduction.md#how-it-works)ドキュメント。

これは、コンセントが c# と統合する方法の低レベルの説明は、され、Xamarin.iOS の上級ユーザー向けに提供されます。 Mac マッピング用の Visual Studio の使用が完了すると自動的にバック グラウンドでを使用して自動的に生成、航空券のコード。

インターフェイスのビルダーを使って、ユーザー インターフェイスを設計するときはアプリケーションの外観を設計するだけと、いくつかの既定の接続を確立します。 プログラムによって情報を取得、実行時にコントロールの動作を変更または実行時にコントロールを変更する場合は、必要がある、マネージ コードに一部のコントロールをバインドします。

これは、いくつかの手順で行います。

1.  追加、**コンセント宣言**を**ファイルの所有者**です。
1.  コントロールでの接続、**ファイルの所有者**です。
1.  UI および XIB/NIB ファイルに接続を保存します。
1.  NIB ファイルを実行時に読み込みます。
1.  コンセント変数にアクセスします。


手順 (1) から (3) をインターフェイスのビルダーを持つインターフェイスを構築するための Apple のドキュメントで説明します。

Xamarin.iOS を使用する場合は、アプリケーションが UIViewController から派生するクラスを作成する必要があります。 実装されている、次のようにします。

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

これにより、ユーザー インターフェイスが、NIB から読み込まれます。 今、コンセントにアクセスするにでは、それらにアクセスするようにランタイムに通知するために必要です。 これを行う、`UIViewController`サブクラスがプロパティを宣言し、[接続] 属性で注釈を付けることが必要です。 以下に例を示します。

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

プロパティの実装は、実際にフェッチされ、実際のネイティブ型の値を格納する 1 つです。

Mac と InterfaceBuilder の Visual Studio を使用する場合、これについて心配する必要はありません。 Mac 用の visual Studio では、コード プロジェクトの一部としてコンパイルされる部分クラスで宣言されたコンセントをすべて自動的に反映します。

#### <a name="selectors"></a>セレクター

Objective C のプログラミングの中核となる概念では、セレクターです。 セレクターを渡すことを必要としたり、セレクターに応答するコードが必要ですが Api を多くの場合、遭遇されます。

C# では新しいセレクターを作成することは非常に簡単 – だけの新しいインスタンスを作成する、`ObjCRuntime.Selector`クラスし、必要とする API 内のどの場所の結果を使用します。 例えば:

```csharp
var selector_add = new Selector ("add:plus:");
```

C# メソッドには、応答セレクターの呼び出しを継承する必要があります、`NSObject`セレクターの名前を使用してでは、型と c# のメソッドを装飾する必要があります、`[Export]`属性。 例えば:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

そのセレクターに注意してください名**必要があります**中間と後続のすべてのコロンを含む完全に一致する (":") がある場合。

#### <a name="nsobject-constructors"></a>NSObject コンス トラクター

ほとんどのクラスから派生する Xamarin.iOS`NSObject`オブジェクトの機能に固有のコンス トラクターを公開がすぐには明白ではないさまざまなコンス トラクターも公開されます。

コンス トラクターは、次のように使用されます。

```csharp
public Foo (IntPtr handle)
```

このコンス トラクターを使用して、ランタイムはアンマネージ クラスに、クラスをマップする必要がある場合、クラスのインスタンスを作成します。 これは、XIB/NIB ファイルを読み込むときに発生します。  この時点では、世界では、アンマネージ、Objective C のランタイムが、オブジェクトを作成したし、マネージ側を初期化するためにこのコンス トラクターが呼び出されます。

通常、ハンドル パラメーターを持つおよびの本文には、基底コンス トラクターを呼び出し、必要なすべての初期化を行うために必要なです。

```csharp
public Foo ()
```

クラスは、既定のコンス トラクターは、これとクラスを提供する Xamarin.iOS でこの間に、Foundation.NSObject クラスとすべてのクラスを初期化し、最後に、チェインこの Objective C に`init`クラスのメソッドです。

```csharp
public Foo (NSObjectFlag x)
```

このコンス トラクターは、インスタンスを初期化が、最後に OBJECTIVE-C"init"メソッドの呼び出しからコードを防ぐために使用されます。 既に登録済みの初期化時にこれも、一般的に使用する (使用すると`[Export]`コンス トラクターで) 場合がまだ完了して別の平均値で初期化またはします。

```csharp
public Foo (NSCoder coder)
```

NSCoding インスタンスからオブジェクトが初期化されている場所の場合、このコンス トラクターが提供されます。 詳細については、Apple を参照してください。[アーカイブしてシリアル化プログラミング ガイドです。](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>例外

Xamarin.iOS API の設計では、C# の場合は、例外として Objective C の例外は発生しません。 設計はガベージに送信されるありません OBJECTIVE-C 世界最初の場所を適用し、無効なデータがこれまでは前に、例外を生成する必要がありますが、バインディング自体によって生成されることは、OBJECTIVE-C 世界に渡されます。

#### <a name="notifications"></a>通知

IOS および OS X の両方では、開発者は、基になるプラットフォームによってブロードキャストされた通知にサブスクライブできます。 使用してこれは、`NSNotificationCenter.DefaultCenter.AddObserver`メソッドです。 `AddObserver`メソッドは、2 つのパラメーターを受け取ります以外の場合は 1 つは、通知をサブスクライブする以外の場合は、もう 1 つは、通知が発生したときに呼び出されるメソッド。

Xamarin.iOS および Xamarin.Mac でさまざまな通知のキーは、通知がトリガーされるクラスにホストされます。 たとえば、通知がによって生成、`UIMenuController`としてホストされます`static NSString`でプロパティ、`UIMenuController`クラス名「通知」で終了します。

### <a name="memory-management"></a>メモリ管理

Xamarin.iOS には、ガベージ コレクターを使用するには不要になったときにリソースを解放するがあれば問題があります。 ガベージ コレクターだけでなくすべてのオブジェクトから派生した`NSObject`実装、`System.IDisposable`インターフェイスです。

#### <a name="nsobject-and-idisposable"></a>NSObject および IDisposable

公開する、`IDisposable`インターフェイスは、大きいサイズのメモリ ブロックがカプセル化するオブジェクトを解放するときに開発者を支援の便利な方法 (たとえば、`UIImage`無害であるポインターでは同じようになりますが、2 つメガバイト イメージを指している可能性があります) およびその他の重要であり、有限のリソース (ビデオのデコード バッファー) などです。

NSObject、IDisposable インターフェイスを実装しても、 [.NET の Dispose パターン](http://msdn.microsoft.com/library/fs2xkftw.aspx)です。 これにより、開発者は、そのサブクラス NSObject Dispose 動作をオーバーライドし、必要に応じて、独自のリソースを解放します。 たとえば、多数のイメージを保持するようにこのビュー コント ローラーがあるとします。

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

マネージ オブジェクトが破棄されると、役に立たなくなった。 オブジェクトへの参照する必要がありますが、オブジェクトがの事実上、無効この時点でします。 一部の .NET Api など、破棄されたオブジェクトのすべてのメソッドにアクセスしようとする場合、ObjectDisposedException スローすることによってこれを確認します。

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

「イメージ」の変数にアクセスすることができますが、が実際に無効な参照および不要になったイメージを保持 OBJECTIVE-C オブジェクトへのポインター。

C# でオブジェクトを破棄できませんわけでは、オブジェクトが破棄とは限りません。 操作を行うすべてが C# の場合に、オブジェクト参照を解放します。 Cocoa 環境に可能性がありますの周囲の参照を独自に使用するが保持されていることができます。 やなどの場合は、イメージに UIImageView のイメージのプロパティを設定して、イメージを破棄し、基になる UIImageView 独自の参照を実行したが終了するまでにこのオブジェクトへの参照を保持を使用します。

#### <a name="when-to-call-dispose"></a>Dispose を呼び出すときに

オブジェクトの除去でモノラル必要がある場合は、Dispose を呼び出す必要があります。 可能なユース ケースは、Mono には、NSObject がメモリ、または情報プールなどの重要なリソースへの参照を保持している実際の知識があるない場合にです。 ような場合、すぐにガベージ コレクション サイクルを実行するモノラル待つことがなく、メモリへの参照を解放するために Dispose を呼び出す必要があります。

内部的には、Mono を作成すると[NSString を c# の文字列から参照](~/ios/internals/api-design/nsstring.md)、ガベージ コレクターを行うには、作業の量を削減するには、すぐにそれらが破棄されます。 約問題に対処する、速度は速く GC より少ないオブジェクトが実行されます。

#### <a name="when-to-keep-references-to-objects"></a>オブジェクトへの参照を保持する場合

自動メモリ管理には、1 つの副作用は、こと、GC は処分使用されていないオブジェクトへの参照がない限りです。 これは、場合もあります副作用があることにより意外などの場合は、上位ビュー コント ローラーを保持するローカル変数を作成することも、トップ レベル ウィンドウ、およびそれらが背後に表示されなくです。

場合は、オブジェクトに、静的参照またはインスタンス変数を維持しない、モノラル、それらの Dispose() メソッドを呼び出してさいわいおよびオブジェクトへの参照を解放します。 のみの未解決の参照などがありますので Objective C のランタイムはオブジェクトを破棄します。

## <a name="related-links"></a>関連リンク

- [フィールドのバインド](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
