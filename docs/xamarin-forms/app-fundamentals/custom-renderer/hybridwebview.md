---
title: HybridWebView を実装します。
description: Xamarin.Forms カスタム ユーザー インターフェイス コントロールは、レイアウトと、画面上のコントロールを配置に使用されるビュー クラスから派生する必要があります。 この記事では、JavaScript から呼び出せるように c# コードを許可するプラットフォーム固有の web コントロールを強化する方法を示します、HybridWebView カスタム コントロールのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: af0dbef84d8ceb178fe5c1ac6fc7194c178141dc
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="implementing-a-hybridwebview"></a>HybridWebView を実装します。

_Xamarin.Forms カスタム ユーザー インターフェイス コントロールは、レイアウトと、画面上のコントロールを配置に使用されるビュー クラスから派生する必要があります。この記事では、JavaScript から呼び出せるように c# コードを許可するプラットフォーム固有の web コントロールを強化する方法を示します、HybridWebView カスタム コントロールのカスタム レンダラーを作成する方法を示します。_

各 Xamarin.Forms ビューには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ios では、Xamarin.Forms アプリケーションによって表示される、`ViewRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`UIView`コントロール。 Android のプラットフォームでは、`ViewRenderer`クラスをインスタンス化、`View`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`ViewRenderer`クラスのインスタンスを作成、ネイティブな`FrameworkElement`コントロール。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)とそれを実装する、対応するネイティブ コントロール。

![](hybridwebview-images/view-classes.png "ビュー クラスとその実装のネイティブ クラス間のリレーションシップ")

カスタマイズを実装するプラットフォーム固有のカスタム レンダラーを作成することで、表示プロセスを使用できます、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_HybridWebView)、`HybridWebView`カスタム コントロールです。
1. [消費](#Consuming_the_HybridWebView)、 `HybridWebView`Xamarin.Forms からです。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)のカスタム レンダラー、`HybridWebView`プラットフォームごとにします。

各項目を実装する順番に説明するようになりましたが、`HybridWebView`レンダラー JavaScript から呼び出せるように c# コードを許可するプラットフォーム固有の web コントロールをさらに向上します。 `HybridWebView`インスタンスは、ユーザーの名前を入力するかを確認する HTML ページを表示する使用されます。 次に、ユーザーには、HTML のボタンがクリックすると、JavaScript 関数が呼び出す c#`Action`ユーザー名を含むポップアップを表示します。

JavaScript からの起動 C# でのプロセスに関する詳細については、次を参照してください。[を呼び出す c# JavaScript から](#Invoking_C_from_JavaScript)です。 HTML ページの詳細については、次を参照してください。 [Web ページを作成する](#Creating_the_Web_Page)です。

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>HybridWebView を作成します。

`HybridWebView`サブクラス化して、カスタム コントロールを作成することができます、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラスに、次のコード例に示すようにします。

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

`HybridWebView`カスタム コントロールは、標準的な .NET のライブラリ プロジェクトに作成され、コントロールの次の API を定義します。

- A`Uri`ロードする web ページのアドレスを指定するプロパティです。
- A`RegisterAction`を登録する方法、`Action`コントロールを使用します。 登録されているアクションを介して参照された HTML ファイルに格納されている JavaScript から呼び出される、`Uri`プロパティです。
- A`CleanUp`に登録済みの参照を削除するメソッド`Action`です。
- `InvokeAction`メソッドを呼び出す、登録されている`Action`です。 このメソッドが、各プラットフォームに固有のプロジェクトでのカスタム レンダラーから呼び出されます。

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>HybridWebView の使用

`HybridWebView`カスタム コントロールで参照できます XAML で標準的な .NET のライブラリ プロジェクトの場所の名前空間の宣言してカスタム コントロールの名前空間プレフィックスを使用します。 次のコード例に示す方法、`HybridWebView`カスタム コントロールを XAML ページで利用できることができます。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値がカスタム コントロールの詳細情報と一致する必要があります。 名前空間が宣言されると、カスタム コントロールを参照する、プレフィックスを使用します。

次のコード例に示す方法、`HybridWebView`カスタム コントロールは、c# のページで利用できることができます。

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

`HybridWebView`インスタンスは、各プラットフォームでネイティブ web コントロールを表示する使用されます。 `Uri`プロパティがプラットフォーム固有の各プロジェクトに格納されていると、どれをネイティブ web コントロールによって表示する HTML ファイルに設定します。 レンダリングされる HTML を呼び出して、c#、JavaScript 関数を使用、名前を入力するユーザーに確認`Action`HTML ボタン クリックに応答します。

`HybridWebViewPage`の次のコード例に示すように、JavaScript から呼び出されるアクションを登録します。

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

この操作を呼び出す、 [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/)によって表示される HTML ページに入力した名前を表示するモーダル ポップアップを表示するメソッド、`HybridWebView`インスタンス。

カスタム レンダラーは、c# コードを JavaScript から呼び出せるようにすることで、プラットフォーム固有の web コントロールを強化するために各アプリケーション プロジェクトを今すぐ追加できます。

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでは、カスタム レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ViewRenderer<T1,T2>`カスタム コントロールを表示するクラス。 最初の型引数は、この例では、レンダラーでは、カスタム コントロールをする必要があります`HybridWebView`です。 2 番目の型引数は、カスタム ビューを実装するネイティブ コントロールにする必要があります。
1. 上書き、`OnElementChanged`をカスタマイズするカスタム コントロールと書き込みのロジックを表示するメソッド。 このメソッドは、Xamarin.Forms、対応するカスタム コントロールの作成時に呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms のカスタム コントロールを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> Xamarin.Forms のほとんどの要素は各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラーが必要に各プラットフォームのプロジェクトでレンダリングするときに、[ビュー](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)要素。

次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](hybridwebview-images/solution-structure.png "HybridWebView カスタム レンダラーのプロジェクトの責任")

`HybridWebView`カスタム コントロールがから派生するプラットフォーム固有のレンダラー クラスによって表示される、`ViewRenderer`各プラットフォームのクラスです。 これは、結果、各`HybridWebView`次のスクリーン ショットに示すように、プラットフォーム固有の web コントロールに表示されるカスタム コントロール。

![](hybridwebview-images/screenshots.png "各プラットフォームで HybridWebView")

`ViewRenderer`クラスが公開、 `OnElementChanged` Xamarin.Forms のカスタム コントロールが、対応するネイティブ web コントロールを表示するために作成されたときに呼び出されるメソッド。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティです。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラーでは、*が*に接続されていると Xamarin.Forms の要素をレンダラーでは、*は*に、それぞれをアタッチします。 サンプル アプリケーションで、`OldElement`プロパティ`null`と`NewElement`プロパティへの参照が格納されます、`HybridWebView`インスタンス。

オーバーライドのバージョン、`OnElementChanged`メソッドは、各プラットフォームに固有のレンダラー クラスでは、ネイティブ web コントロールのインスタンス化とカスタマイズを実行する場所です。 `SetNativeControl`ネイティブ web コントロールをインスタンス化するメソッドを使用し、このメソッドはコントロールの参照を代入しても、`Control`プロパティです。 さらに、を通じて表示される Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティです。

状況によっては、`OnElementChanged`メソッドは、複数回呼び出すことができます。 したがって、メモリ リークを防ぐためには、注意が必要、新しいネイティブのコントロールをインスタンス化するとき。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを設定し、イベント ハンドラーをサブスクライブします。 同様に、レンダラーが関連付けられている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーを登録解除します。 このアプローチを採用することは、メモリ リークを回避することが、パフォーマンスの高いカスタム レンダラーを作成するのに役立ちます。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、–、表示される Xamarin.Forms カスタム コントロールの型名とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各ネイティブ web コントロール、呼び出し元 c#、JavaScript からのプロセスおよび各プラットフォームに固有のカスタム レンダラー クラス内の実装によって読み込まれた web ページの構造について説明します。

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Web ページを作成します。

次のコード例で表示される web ページを示しています、`HybridWebView`カスタム コントロール。

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
        log(err);
    }
}
</script>
</body>
</html>
```

Web ページにより、ユーザーは自分の名前を入力する、`input`要素を提供し、`button`クリックされたときに c# コードで起動される要素。 これを実現するためのプロセスは次のとおりです。

- ユーザーがクリックしたときに、`button`要素、`invokeCSCode`値は、JavaScript 関数が呼び出されます、`input`関数に渡される要素です。
- `invokeCSCode`関数呼び出し、 `log` c# に送信するデータを表示する関数`Action`です。 呼び出して、`invokeCSharpAction`メソッドを呼び出す c#`Action`から受信したパラメーターを渡して、`input`要素。

`invokeCSharpAction` JavaScript 関数が、web ページで定義されていないと、各カスタム レンダラーによってそこに挿入されます。

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>C# では、JavaScript から呼び出す

呼び出す c# JavaScript からのプロセスは、各プラットフォームで同じです。

- カスタムのレンダラーでは、ネイティブ web コントロールを作成し、によって指定された HTML ファイルを読み込み、`HybridWebView.Uri`プロパティです。
- Web ページが読み込まれると、カスタム レンダラーを挿入、 `invokeCSharpAction` web ページに JavaScript 関数。
- ときにユーザーは、名前を入力し、HTML をクリックする`button`要素、`invokeCSCode`関数が呼び出され、これを呼び出す、`invokeCSharpAction`関数。
- `invokeCSharpAction`関数でカスタムのレンダラーでは、呼び出すメソッドを呼び出すと、`HybridWebView.InvokeAction`メソッドです。
- `HybridWebView.InvokeAction`メソッドを呼び出して、登録されている`Action`です。

次のセクションでは、各プラットフォームにこのプロセスを実装する方法について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

`HybridWebViewRenderer`クラスで指定された web ページを読み込みます、`HybridWebView.Uri`プロパティに、ネイティブな[ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/)コントロール、および`invokeCSharpAction`JavaScript 関数が、web ページに挿入されます。 ユーザーの名前を入力し、HTML のクリックして後`button`要素、 `invokeCSharpAction` JavaScript 関数が実行されると、 `DidReceiveScriptMessage` web ページからメッセージを受信した後に呼び出されるメソッド。 さらに、このメソッドは、`HybridWebView.InvokeAction`メソッドで、ポップアップを表示する登録されているアクションを呼び出します。

この機能は、次のように実現は。

- `Control`プロパティは`null`、次の操作が実行されます。
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/)インスタンスを作成すると、メッセージを送信したり、web ページにユーザーのスクリプトを挿入できるようにします。
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/)を挿入するインスタンスが作成される、 `invokeCSharpAction` web ページ、web ページが読み込まれた後に JavaScript 関数。
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/)メソッドを追加、 [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/)コンテンツのコント ローラー インスタンス。
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/)メソッドという名前のスクリプト メッセージ ハンドラーを追加する`invokeAction`を[ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) JavaScript 関数を原因となるインスタンス`window.webkit.messageHandlers.invokeAction.postMessage(data)`すべてで定義するにはすべての web ビューを使用するフレーム、`WKUserContentController`インスタンス。
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/)インスタンスが作成されると、 [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/)コンテンツのコント ローラーとして設定されているインスタンス。
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/)コントロールがインスタンス化され、`SetNativeControl`への参照を割り当てるためにはメソッドが呼び出される、`WKWebView`コントロールを`Control`プロパティです。
- 新しい Xamarin.Forms 要素には、カスタム レンダラーを添付する用意されています。
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/)メソッドで指定された HTML ファイルのロード、`HybridWebView.Uri`プロパティです。 コードでファイルが格納されていることを指定します、`Content`プロジェクトのフォルダーです。 Web ページが表示されたら、 `invokeCSharpAction` web ページに挿入される JavaScript 関数。
- 要素は、レンダラーが変更にアタッチされている場合。
  - リソースが解放されます。

> [!NOTE]
> `WKWebView`クラスは iOS 8 以降でのみサポートされます。

### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

`HybridWebViewRenderer`クラスで指定された web ページの読み込み、`HybridWebView.Uri`プロパティ ネイティブに[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)コントロール、および`invokeCSharpAction`web ページが読み込まれた後、web ページに JavaScript 関数が組み込まれて`InjectJS`メソッドです。 ユーザーの名前を入力し、HTML のクリックして後`button`要素、 `invokeCSharpAction` JavaScript 関数を実行します。 この機能は、次のように実現は。

- `Control`プロパティは`null`、次の操作が実行されます。
  - ネイティブ[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)インスタンスが作成され、コントロールで JavaScript を有効にします。
  - `SetNativeControl`メソッドは、ネイティブへの参照を割り当てるために呼び出される[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)コントロールを`Control`プロパティです。
- 新しい Xamarin.Forms 要素には、カスタム レンダラーを添付する用意されています。
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/)メソッドは、新しい挿入`JSBridge`WebView の JavaScript のコンテキスト、名前のメイン フレームにインスタンス`jsBridge`です。 これにより、メソッドで、 `JSBridge` JavaScript からアクセスするクラス。
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/)メソッドで指定された HTML ファイルのロード、`HybridWebView.Uri`プロパティです。 コードでファイルが格納されていることを指定します、`Content`プロジェクトのフォルダーです。
  - `InjectJS`を挿入するメソッドが呼び出され、 `invokeCSharpAction` web ページに JavaScript 関数。
- 要素は、レンダラーが変更にアタッチされている場合。
  - リソースが解放されます。

ときに、 `invokeCSharpAction` JavaScript 関数が実行され、それを呼び出す、`JSBridge.InvokeAction`メソッドは、次のコード例に示されています。

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

クラスから派生しなければなりません`Java.Lang.Object`、使用する JavaScript に公開されるメソッドを装飾する必要がありますと、`[JavascriptInterface]`と`[Export]`属性。 したがって、 `invokeCSharpAction` JavaScript 関数が web ページに挿入されが実行されるが呼び出されます、`JSBridge.InvokeAction`メソッドで装飾されているため、`[JavascriptInterface]`と`[Export("invokeAction")]`属性。 さらに、`InvokeAction`メソッドを呼び出して、`HybridWebView.InvokeAction`なるには、ポップアップを表示する登録されているアクションが呼び出されたメソッド。

> [!NOTE]
> 使用するプロジェクト、`[Export]`属性への参照を含める必要があります`Mono.Android.Export`、またはコンパイラ エラーが発生します。

なお、`JSBridge`クラスを保持、`WeakReference`を`HybridWebViewRenderer`クラスです。 これは、2 つのクラス間の循環参照を作成しないようにします。 詳細については、次を参照してください。[弱い参照](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx)msdn です。

> [!IMPORTANT]
> Android Oreo Android マニフェストが設定されることを確認、**ターゲット Android バージョン**に**自動**です。 それ以外の場合、このコードを実行して、、エラー メッセージ「invokeCSharpAction が定義されていません」します。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP にカスタム レンダラーを作成します。

次のコード例は、UWP のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

`HybridWebViewRenderer`クラスで指定された web ページを読み込みます、`HybridWebView.Uri`プロパティ ネイティブに`WebView`コントロール、および`invokeCSharpAction`web ページが読み込まれた後に、web ページに JavaScript 関数が挿入されると、`WebView.InvokeScriptAsync`メソッドです。 ユーザーの名前を入力し、HTML のクリックして後`button`要素、 `invokeCSharpAction` JavaScript 関数が実行されると、 `OnWebViewScriptNotify` web ページから通知を受信した後に呼び出されるメソッド。 さらに、このメソッドは、`HybridWebView.InvokeAction`メソッドで、ポップアップを表示する登録されているアクションを呼び出します。

この機能は、次のように実現は。

- `Control`プロパティは`null`、次の操作が実行されます。
  - `SetNativeControl`新しいネイティブのインスタンスを作成するメソッドが呼び出された`WebView`を制御し、するために参照を代入、`Control`プロパティです。
- 新しい Xamarin.Forms 要素には、カスタム レンダラーを添付する用意されています。
  - イベント ハンドラー、`NavigationCompleted`と`ScriptNotify`イベントが登録されます。 `NavigationCompleted`イベントが発生したときに、ネイティブ`WebView`コントロールでは、現在のコンテンツの読み込みが完了したか、ナビゲーションが失敗したかどうか。 `ScriptNotify`イベントが発生したときに、ネイティブの内容は、`WebView`コントロールでは、JavaScript を使用して、アプリケーションに文字列を渡します。 Web ページが起動、`ScriptNotify`を呼び出してイベント`window.external.notify`渡すときに、`string`パラメーター。
  - `WebView.Source`プロパティで指定された HTML ファイルの URI に設定されて、`HybridWebView.Uri`プロパティです。 コードで、ファイルが格納されていることを想定しています、`Content`プロジェクトのフォルダーです。 Web ページが表示されたら、`NavigationCompleted`イベントが発生し、`OnWebViewNavigationCompleted`メソッドが呼び出されます。 `invokeCSharpAction` JavaScript 関数は、web ページに挿入し、`WebView.InvokeScriptAsync`メソッドが正常に完了したナビゲーションを提供します。
- 要素は、レンダラーが変更にアタッチされている場合。
  - イベントからの購読です。

## <a name="summary"></a>まとめ

この記事は、用のカスタム レンダラーを作成する方法を示しましたが、 `HybridWebView` JavaScript から呼び出せるように c# コードを許可するプラットフォーム固有の web コントロールを強化する方法については、カスタム コントロールです。


## <a name="related-links"></a>関連リンク

- [CustomRendererHybridWebView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [JavaScript から呼び出す (C#)](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
