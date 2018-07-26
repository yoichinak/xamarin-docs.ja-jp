---
title: Hybridwebview の実装
description: この記事では、JavaScript から呼び出される c# コードを許可するプラットフォームに固有の web コントロールを強化する方法については、HybridWebView カスタム コントロールのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4715685fdf417ba2d08f9ae3d36d6e691fa701fa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242460"
---
# <a name="implementing-a-hybridwebview"></a>Hybridwebview の実装

_Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、レイアウトと画面上のコントロールを配置するために使用するビュー クラスから派生する必要があります。この記事では、JavaScript から呼び出される c# コードを許可するプラットフォームに固有の web コントロールを強化する方法については、HybridWebView カスタム コントロールのカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms のビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `View` ](xref:Xamarin.Forms.View)で iOS、Xamarin.Forms アプリケーションによって表示される、`ViewRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`UIView`コントロール。 Android のプラットフォームで、`ViewRenderer`クラスをインスタンス化、`View`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`ViewRenderer`クラスのインスタンスを作成、ネイティブ`FrameworkElement`コントロール。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `View` ](xref:Xamarin.Forms.View)およびそれを実装するネイティブ コントロールの対応します。

![](hybridwebview-images/view-classes.png "ビュー クラスとそのネイティブ クラスの実装間のリレーションシップ")

レンダリング プロセスのカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装するために使用できます、 [ `View` ](xref:Xamarin.Forms.View)各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_HybridWebView)、`HybridWebView`カスタム コントロール。
1. [消費](#Consuming_the_HybridWebView)、 `HybridWebView`Xamarin.Forms から。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)用のカスタム レンダラー、`HybridWebView`各プラットフォームで。

各項目が実装するためにさらに説明するようになりましたが、`HybridWebView`レンダラーに JavaScript から呼び出される c# コードを許可するプラットフォームに固有の web コントロールを拡張します。 `HybridWebView`インスタンスは、ユーザーの名前を入力するかを確認する HTML ページを表示する使用されます。 次に、ユーザーには、HTML ボタンがクリックすると、JavaScript 関数を呼び出すが、c#`Action`ユーザー名を含むポップアップ ウィンドウを表示します。

JavaScript から C# で呼び出し元のプロセスに関する詳細については、次を参照してください。[を呼び出す c# から JavaScript](#Invoking_C_from_JavaScript)します。 HTML ページの詳細については、次を参照してください。 [Web ページを作成する](#Creating_the_Web_Page)します。

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>HybridWebView の作成

`HybridWebView`をサブクラス化してカスタム コントロールを作成することができます、 [ `View` ](xref:Xamarin.Forms.View)クラスに、次のコード例に示すようにします。

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

`HybridWebView`カスタム コントロールは、.NET Standard ライブラリ プロジェクトが作成され、コントロールの次の API を定義します。

- A`Uri`読み込まれる web ページのアドレスを指定するプロパティ。
- A`RegisterAction`を登録するメソッド、`Action`コントロールを使用します。 登録されているアクションを使って参照する HTML ファイルに含まれている JavaScript から呼び出される、`Uri`プロパティ。
- A`CleanUp`登録への参照を削除するメソッド`Action`します。
- `InvokeAction`メソッドを呼び出す、登録済み`Action`します。 このメソッドは、各プラットフォーム固有プロジェクトでのカスタム レンダラーから呼び出すされます。

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>HybridWebView の使用

`HybridWebView`カスタム コントロールで参照できます XAML で .NET Standard ライブラリ プロジェクトの場所の名前空間の宣言して、カスタム コントロールに対する名前空間プレフィックスを使用します。 次のコード例に示す方法、 `HybridWebView` XAML ページで、カスタム コントロールを使用できます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム コントロールを参照します。

次のコード例に示す方法、 `HybridWebView` (C#) ページで、カスタム コントロールを使用できます。

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

`HybridWebView`インスタンスは、各プラットフォームでネイティブの web コントロールの表示に使用されます。 `Uri`プロパティが各プラットフォーム固有プロジェクトに格納されているとは、ネイティブの web コントロールによって表示される HTML ファイルに設定します。 レンダリングされる HTML、c# を呼び出す JavaScript 関数を使用した、名前を入力するユーザーに確認`Action`HTML ボタンのクリックに応答します。

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

この操作を呼び出す、 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))によって表示される HTML ページで入力した名前を表示するモーダル ポップアップを表示するメソッド、`HybridWebView`インスタンス。

カスタム レンダラーは、c# コードを JavaScript から呼び出せるようにすることで、プラットフォーム固有の web コントロールを強化するためには、各アプリケーション プロジェクトを今すぐ追加できます。

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ViewRenderer<T1,T2>`カスタム コントロールをレンダリングするクラス。 最初の型引数は、この例では、レンダラーでは、カスタム コントロールをする必要があります`HybridWebView`します。 2 番目の型引数が、カスタム ビューを実装するネイティブ コントロールがあります。
1. 上書き、`OnElementChanged`それをカスタマイズするカスタム コントロールと書き込みロジックをレンダリングするメソッド。 対応する、Xamarin.Forms カスタム コントロールが作成されたときに、このメソッドが呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラスは、Xamarin.Forms カスタム コントロールを表示するために使用するように指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラー必要は各プラットフォーム プロジェクトに表示するときに、[ビュー](xref:Xamarin.Forms.View)要素。

次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](hybridwebview-images/solution-structure.png "HybridWebView のカスタム レンダラーのプロジェクトの責任")

`HybridWebView`でプラットフォーム固有のレンダラー クラスから派生するカスタム コントロールが表示される、`ViewRenderer`各プラットフォームのクラス。 これは、結果、各`HybridWebView`の次のスクリーン ショットに示すようにプラットフォーム固有の web コントロールと表示されるカスタム コントロール。

![](hybridwebview-images/screenshots.png "各プラットフォームで HybridWebView")

`ViewRenderer`クラスでは、`OnElementChanged`メソッドで、Xamarin.Forms カスタム コントロールが、対応するネイティブの web コントロールを表示するために作成されたときに呼び出されます。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティ。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラー*が*に接続されていると Xamarin.Forms 要素をレンダラー*は*に、それぞれに接続されています。 サンプル アプリケーションでは、`OldElement`プロパティになります`null`と`NewElement`プロパティへの参照には、`HybridWebView`インスタンス。

オーバーライドされたバージョン、`OnElementChanged`各プラットフォームに固有のレンダラー クラスのメソッドは、ネイティブの web コントロールのインスタンス化とカスタマイズを実行する場所。 `SetNativeControl` 、ネイティブの web コントロールをインスタンス化するメソッドを使用する必要があり、このメソッドは、コントロールの参照を割り当てることも、`Control`プロパティ。 さらでレンダリングされている Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティ。

状況によっては、`OnElementChanged`メソッドは、複数回呼び出すことができます。 そのため、メモリ リークを防ぐためには、注意が必要、新しいネイティブ コントロールをインスタンス化するとき。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

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

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを設定し、イベント ハンドラーをサブスクライブします。 同様に、レンダラーが関連付けられている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーを登録解除します。 このアプローチを採用すると、メモリ リークが発生しないパフォーマンスの高い、カスタム レンダラーを作成するのに役立ちます。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms カスタム コントロールの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各ネイティブ web コントロール、javascript、C# で呼び出し元のプロセス、および各プラットフォームに固有のカスタム レンダラー クラスの実装によって読み込まれた web ページの構造について説明します。

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

Web ページでは、ユーザーに名前を入力する、`input`要素を示し、`button`がクリックされたときに c# コードを呼び出す要素。 これを実現するためのプロセスは次のとおりです。

- ユーザーがクリックしたときに、`button`要素、`invokeCSCode`の値を持つ JavaScript 関数を呼び出す、`input`関数に渡された要素。
- `invokeCSCode`関数呼び出し、 `log` c# に送信するデータを表示する関数`Action`します。 呼び出して、`invokeCSharpAction`メソッドを呼び出すと、C#`Action`から受信したパラメーターを渡して、`input`要素。

`invokeCSharpAction`の JavaScript 関数は、web ページで定義されていないと、各カスタム レンダラーによってそこに挿入されます。

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>C# JavaScript からの呼び出し

JavaScript から C# で呼び出し元のプロセスは、各プラットフォームで同じです。

- カスタム レンダラーがネイティブの web コントロールを作成しで指定された HTML ファイルを読み込み、`HybridWebView.Uri`プロパティ。
- カスタム レンダラーを挿入します。 web ページが読み込まれると、 `invokeCSharpAction` web ページに JavaScript 関数。
- ユーザーがその名前を入力し、HTML のクリックして`button`要素、`invokeCSCode`関数が呼び出される、順番に呼び出します`invokeCSharpAction`関数。
- `invokeCSharpAction`関数が呼び出すのカスタムのレンダラーでメソッドを呼び出す、`HybridWebView.InvokeAction`メソッド。
- `HybridWebView.InvokeAction`メソッドは、登録済み`Action`します。

次のセクションでは、各プラットフォームでこのプロセスを実装する方法について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

次のコード例では、iOS プラットフォーム用のカスタム レンダラーを示します。

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

`HybridWebViewRenderer`クラスで指定された web ページの読み込み、`HybridWebView.Uri`プロパティに、ネイティブ[ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/)コントロール、および`invokeCSharpAction`JavaScript 関数は、web ページに挿入されます。 ユーザーの名前を入力し、HTML をクリックすると`button`要素、 `invokeCSharpAction` JavaScript 関数が実行されると、 `DidReceiveScriptMessage` web ページから、メッセージを受信した後に呼び出されるメソッド。 さらに、このメソッドを呼び出す、`HybridWebView.InvokeAction`メソッドは、ポップアップ ウィンドウを表示する登録されているアクションを呼び出します。

この機能は、次のように実現されます。

- されるとき、`Control`プロパティは`null`、次の操作が実行されます。
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/)インスタンスを作成すると、メッセージを投稿して、web ページにユーザーのスクリプトを挿入することができます。
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/)インスタンスを作成すると、挿入、 `invokeCSharpAction` web ページが読み込まれた後に、web ページに JavaScript 関数。
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/)メソッドを追加、 [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/)コンテンツのコント ローラー インスタンス。
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/)メソッドは、という名前のスクリプト メッセージ ハンドラーを追加します`invokeAction`を、 [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/)により、JavaScript 関数のインスタンス`window.webkit.messageHandlers.invokeAction.postMessage(data)`すべてで定義するには。使用するすべての web ビュー内のフレーム、`WKUserContentController`インスタンス。
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/)インスタンスが作成されると、 [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/)コンテンツのコント ローラーとして設定されているインスタンス。
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/)コントロールがインスタンス化と`SetNativeControl`メソッドが呼び出されへの参照を割り当てる、`WKWebView`への制御、`Control`プロパティ。
- カスタム レンダラーが新しい Xamarin.Forms 要素に接続されている提供されています。
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/)メソッドで指定された HTML ファイルの読み込み、`HybridWebView.Uri`プロパティ。 コードでは、ファイルに保存することを指定します、`Content`プロジェクトのフォルダー。 Web ページが表示されたら、 `invokeCSharpAction` JavaScript 関数は、web ページに挿入されます。
- 要素は、レンダラーが変更にアタッチされている場合。
  - リソースが解放されます。

> [!NOTE]
> `WKWebView`クラスは iOS 8 以降でのみサポートされます。

### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

次のコード例では、Android プラットフォーム用のカスタム レンダラーを示します。

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

`HybridWebViewRenderer`クラスで指定された web ページの読み込み、`HybridWebView.Uri`プロパティに、ネイティブ[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)コントロール、および`invokeCSharpAction`JavaScript 関数は、web ページが読み込まれた後、web ページに挿入されます`InjectJS`メソッド。 ユーザーの名前を入力し、HTML のクリックして後`button`要素、 `invokeCSharpAction` JavaScript 関数を実行します。 この機能は、次のように実現されます。

- されるとき、`Control`プロパティは`null`、次の操作が実行されます。
  - ネイティブ[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)インスタンスが作成され、コントロールに JavaScript を有効にします。
  - `SetNativeControl`メソッドを呼び出して、ネイティブへの参照を代入する[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)への制御、`Control`プロパティ。
- カスタム レンダラーが新しい Xamarin.Forms 要素に接続されている提供されています。
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/)メソッドを挿入します。 新しい`JSBridge`WebView の JavaScript のコンテキスト、名前のメイン フレームにインスタンス`jsBridge`します。 これにより、メソッドで、 `JSBridge` JavaScript からアクセスするクラス。
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/)メソッドで指定された HTML ファイルの読み込み、`HybridWebView.Uri`プロパティ。 コードでは、ファイルに保存することを指定します、`Content`プロジェクトのフォルダー。
  - `InjectJS`を挿入するメソッドが呼び出される、 `invokeCSharpAction` web ページに JavaScript 関数。
- 要素は、レンダラーが変更にアタッチされている場合。
  - リソースが解放されます。

ときに、 `invokeCSharpAction` JavaScript 関数が実行され、さらに呼び出す、`JSBridge.InvokeAction`メソッドは、次のコード例に示されています。

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

クラスを派生する必要があります`Java.Lang.Object`で修飾する必要がありますが、JavaScript に公開されるメソッドと、`[JavascriptInterface]`と`[Export]`属性。 したがって、ときに、`invokeCSharpAction`は呼び出すことが、JavaScript 関数が web ページには挿入され、実行、`JSBridge.InvokeAction`メソッドで修飾されているため、`[JavascriptInterface]`と`[Export("invokeAction")]`属性。 さらに、`InvokeAction`メソッドを呼び出す、`HybridWebView.InvokeAction`なるには、ポップアップ ウィンドウを表示する登録されているアクションが呼び出されたメソッド。

> [!NOTE]
> 使用するプロジェクト、`[Export]`属性への参照を含める必要があります`Mono.Android.Export`、またはコンパイラ エラーが発生します。

なお、`JSBridge`クラスを保持、`WeakReference`を`HybridWebViewRenderer`クラス。 これは、2 つのクラス間の循環参照を作成しないようにします。 詳細については、次を参照してください。[弱い参照](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx)msdn です。

> [!IMPORTANT]
> Android Oreo Android マニフェストを設定することを確認、 **Target Android version**に**自動**します。 それ以外の場合、このコードを実行すると、エラー メッセージ「invokeCSharpAction が定義されていません」で、します。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP のカスタム レンダラーを作成します。

次のコード例では、UWP 用のカスタム レンダラーを示します。

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

`HybridWebViewRenderer`クラスで指定された web ページの読み込み、`HybridWebView.Uri`プロパティをネイティブに`WebView`コントロール、および`invokeCSharpAction`JavaScript 関数は、web ページが読み込まれた後、web ページに挿入されますと、`WebView.InvokeScriptAsync`メソッド。 ユーザーの名前を入力し、HTML をクリックすると`button`要素、 `invokeCSharpAction` JavaScript 関数が実行されると、 `OnWebViewScriptNotify` web ページから通知を受信した後に呼び出されるメソッド。 さらに、このメソッドを呼び出す、`HybridWebView.InvokeAction`メソッドは、ポップアップ ウィンドウを表示する登録されているアクションを呼び出します。

この機能は、次のように実現されます。

- されるとき、`Control`プロパティは`null`、次の操作が実行されます。
  - `SetNativeControl`メソッドを呼び出して、新しいネイティブのインスタンスを作成する`WebView`制御し、そのにへの参照を割り当てる、`Control`プロパティ。
- カスタム レンダラーが新しい Xamarin.Forms 要素に接続されている提供されています。
  - イベント ハンドラー、`NavigationCompleted`と`ScriptNotify`イベントが登録されます。 `NavigationCompleted`イベントを発生させるときに、ネイティブ`WebView`コントロールでは、現在のコンテンツの読み込みが完了したか、ナビゲーションが失敗したかどうか。 `ScriptNotify`イベントを発生させるときに、ネイティブの内容は、`WebView`コントロールでは、JavaScript を使用して、アプリケーションに文字列を渡します。 Web ページが起動、`ScriptNotify`を呼び出してイベント`window.external.notify`渡す、`string`パラメーター。
  - `WebView.Source`プロパティで指定された HTML ファイルの URI に設定されて、`HybridWebView.Uri`プロパティ。 コード ファイルに保存することを想定しています、`Content`プロジェクトのフォルダー。 Web ページが表示されたら、`NavigationCompleted`イベントが発生し、`OnWebViewNavigationCompleted`メソッドが呼び出されます。 `invokeCSharpAction`し、JavaScript 関数は、web ページに挿入される、`WebView.InvokeScriptAsync`メソッドを提供、ナビゲーションが正常に完了しました。
- 要素は、レンダラーが変更にアタッチされている場合。
  - イベントはのサブスクライブが解除されます。

## <a name="summary"></a>まとめ

この記事では、用のカスタム レンダラーを作成する方法を示しましたが、 `HybridWebView` JavaScript から呼び出される c# コードを許可するプラットフォームに固有の web コントロールを強化する方法については、カスタム コントロール。


## <a name="related-links"></a>関連リンク

- [CustomRendererHybridWebView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [JavaScript から呼び出す (C#)](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
