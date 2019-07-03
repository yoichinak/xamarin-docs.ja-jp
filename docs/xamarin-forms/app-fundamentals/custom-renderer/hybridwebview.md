---
title: HybridWebView の実装
description: この記事では、HybridWebView カスタム コントロール用のカスタム レンダラーを作成する方法を示します。これにより、プラットフォームに固有の Web コントロールを強化して、JavaScript からの C# コードの呼び出しを実現する方法が示されます。
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
ms.openlocfilehash: d09188373d11b33f3b3d78b92faa46bf754797f6
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268990"
---
# <a name="implementing-a-hybridwebview"></a>HybridWebView の実装

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/HybridWebView/)

_Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、View クラスから派生させる必要があります。これは画面上にレイアウトとコントロールを配置するために使われます。この記事では、HybridWebView カスタム コントロール用のカスタム レンダラーを作成する方法を示します。これにより、プラットフォームに固有の Web コントロールを強化して、JavaScript からの C# コードの呼び出しを実現する方法が示されます。_

すべての Xamarin.Forms ビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 [`View`](xref:Xamarin.Forms.View) が Xamarin.Forms アプリケーションによってレンダリングされると、iOS で `ViewRenderer` クラスがインスタンス化され、それによってネイティブの `UIView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`ViewRenderer` クラスによって `View` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`ViewRenderer` クラスによってネイティブの `FrameworkElement` コントロールがインスタンス化されます。 Xamarin.Forms コントロールがマップするレンダラーとネイティブ コントロール クラスの詳細については、「[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」を参照してください。

次の図は、[`View`](xref:Xamarin.Forms.View) と、それを実装する、対応しているネイティブ コントロールの関係を示しています。

![](hybridwebview-images/view-classes.png "View クラスとそれを実装するネイティブ クラス間の関係")

レンダリング プロセスを使用して各プラットフォーム上の [`View`](xref:Xamarin.Forms.View) にカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装することができます。 その実行プロセスは次のとおりです。

1. `HybridWebView` カスタム コントロールを[作成](#Creating_the_HybridWebView)します。
1. Xamarin.Forms から `HybridWebView` を[使用](#Consuming_the_HybridWebView)します。
1. 各プラットフォーム上で `HybridWebView` のカスタム レンダラーを[作成](#creating-the-custom-renderer-on-each-platform)します。

プラットフォーム固有の Web コントロールを強化して、JavaScript から C# コードを呼び出すことができる `HybridWebView` レンダラーを実装するために、各項目について順番に説明します。 `HybridWebView` インスタンスは、ユーザーに名前の入力を求める HTML ページを表示するために使用されます。 それからユーザーが HTML ボタンをクリックすると、JavaScript 関数によって、ユーザー名を含むポップアップを表示する C# `Action` が呼び出されます。

JavaScript から C# を呼び出すプロセスの詳細については、「[JavaScript から C# を呼び出す](#Invoking_C_from_JavaScript)」を参照してください。 HTML ページの詳細については、「[Web ページの作成](#Creating_the_Web_Page)」を参照してください。

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>HybridWebView の作成

`HybridWebView` カスタム コントロールは、次のコード例のように、[`View`](xref:Xamarin.Forms.View) クラスをサブクラス化すると作成できます。

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

`HybridWebView` カスタム コントロールは、.NET Standard ライブラリ プロジェクトで作成され、コントロール用に次の API を定義します。

- 読み込まれる Web ページのアドレスを指定する `Uri` プロパティ。
- `Action` をコントロールに登録する `RegisterAction` メソッド。 登録済みのアクションは、`Uri` プロパティで参照される HTML ファイルに含まれる JavaScript から呼び出されます。
- 登録済みの `Action` への参照を削除する `CleanUp` メソッド。
- 登録済みの `Action` を呼び出す `InvokeAction` メソッド。 このメソッドは、各プラットフォーム固有のプロジェクトのカスタム レンダラーから呼び出されます。

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>HybridWebView の使用

`HybridWebView` カスタム コントロールは、その場所の名前空間を宣言し、カスタム コントロール上で名前空間プレフィックスを使用することで .NET Standard ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページがどのように `HybridWebView` カスタム コントロールを使用できるかを示しています。

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

`local` 名前空間プレフィックスには任意の名前を付けることができます。 ただし、`clr-namespace` と `assembly` の値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム コントロールが参照されます。

次のコード例は、C# ページがどのように `HybridWebView` カスタム コントロールを使用できるかを示しています。

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

`HybridWebView` インスタンスは、各プラットフォーム上でネイティブ Web コントロールを表示するために使用されます。 `Uri` プロパティは、各プラットフォーム固有のプロジェクトに格納されている HTML ファイルに設定され、ネイティブの Web コントロールによって表示されます。 レンダリングされた HTML は、HTML ボタンのクリックに応答して C# `Action` を呼び出す JavaScript 関数を使用して、ユーザーに名前を入力するように求めます。

`HybridWebViewPage` は、次のコード例に示すように、JavaScript から呼び出されるアクションを登録します。

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

このアクションから、モーダル ポップアップを表示する [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) メソッドが呼び出されます。このポップアップには、`HybridWebView` インスタンスによって表示される HTML ページに入力された名前が示されます。

カスタム レンダラーを各アプリケーション プロジェクトに追加して、C# コードを JavaScript から呼び出すことで、プラットフォーム固有の Web コントロールを強化することができます。

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム コントロールをレンダリングする `ViewRenderer<T1,T2>` クラスのサブクラスを作成します。 最初の型引数は、レンダラーが使用するカスタム コントロール (この場合は `HybridWebView`) にする必要があります。 2 つ目の型引数は、カスタム ビューを実装するネイティブ コントロールにする必要があります。
1. カスタム コントロールをレンダリングする `OnElementChanged` メソッドをオーバーライドして、ロジックを書き込んでカスタマイズします。 対応する Xamarin.Forms カスタム コントロールが作成されると、このメソッドが呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加して、Xamarin.Forms カスタム コントロールのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用します。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、[View](xref:Xamarin.Forms.View) 要素をレンダリングするときは、各プラットフォーム プロジェクトにカスタム レンダラーが必要です。

次の図に、サンプル アプリケーション内の各プロジェクトの役割とそれらの関係を示します。

![](hybridwebview-images/solution-structure.png "HybridWebView カスタム レンダラーのプロジェクトの役割")

`HybridWebView` カスタム コントロールはプラットフォーム固有のレンダラー クラスによってレンダリングされます。このクラスはすべて各プラットフォームの `ViewRenderer` クラスから派生しています。 この結果、次のスクリーンショットに示すように、プラットフォーム固有の Web コントロールを使用してそれぞれの `HybridWebView` カスタム コントロールがレンダリングされます。

![](hybridwebview-images/screenshots.png "各プラットフォーム上の HybridWebView")

`ViewRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ Web コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 このメソッドは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーが接続して*いた* Xamarin.Forms 要素と、レンダラーが現在接続して*いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `HybridWebView` インスタンスへの参照が含まれます。

各プラットフォーム固有のレンダラー クラス内にある `OnElementChanged` メソッドのオーバーライドされたバージョンは、ネイティブ Web コントロールのインスタンス化とカスタマイズを実行する場所です。 `SetNativeControl` メソッドはネイティブ Web コントロールのインスタンス化に使用されます。このメソッドはまた、コントロール参照を `Control` プロパティに割り当てます。 さらに、レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

状況によっては、`OnElementChanged` メソッドが複数回呼び出されることがあります。 したがって、メモリ リークを防ぐため、新しいネイティブ コントロールをインスタンス化するときには慎重に行う必要があります。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    if (Control == null) {
      // Instantiate the native control and assign it to the Control property with
      // the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。  さらに、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを作成および構成し、イベント ハンドラーを登録する必要があります。 同様に、レンダラーが関連付けられている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーを登録解除します。 この手法を採用すると、メモリ リークが発生しない効率的なカスタム レンダラーを作成できます。

> [!IMPORTANT]
> `SetNativeControl` メソッドは、`e.NewElement` が `null` ではない場合にのみ、呼び出す必要があります。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms カスタム コントロールの種類名と、カスタム レンダラーの種類名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスは、属性がアセンブリ全体に適用されることを指定します。

以下のセクションでは、各ネイティブ Web コントロールによって読み込まれる Web ページの構造、JavaScript から C# を呼び出すプロセス、各プラットフォーム固有のカスタム レンダラー クラスでこれを実装する方法について説明します。

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Web ページの作成

次のコード例は、`HybridWebView` カスタム コントロールによって表示される Web ページを示しています。

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

この Web ページでは、ユーザーは `input` 要素に名前を入力することができます。また、クリックすると C# コードを呼び出す`button` 要素が用意されています。 これを実現するプロセスは次のとおりです。

- ユーザーが `button` 要素をクリックすると、`invokeCSCode` JavaScript 関数が呼び出され、`input` 要素の値が関数に渡されます。
- `invokeCSCode` 関数からは、C# `Action` に送信されているデータを表示する `log` 関数が呼び出されます。 次に、C# `Action` を呼び出す `invokeCSharpAction` メソッドが呼び出され、`input` 要素から受け取ったパラメーターが渡されます。

`invokeCSharpAction` JavaScript 関数は Web ページで定義されておらず、各カスタム レンダラーによって挿入されます。

iOS の場合、この HTML ファイルは、**BundleResource** のビルド アクションと一緒に、プラットフォーム プロジェクトの Content フォルダーに置かれます。 Android の場合、この HTML ファイルは、**AndroidAsset** のビルド アクションと一緒にプラットフォーム プロジェクトの Assets/Content フォルダーに置かれます。

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>JavaScript から C# を呼び出す

JavaScript から C# を呼び出すプロセスは、各プラットフォームで同じです。

- カスタム レンダラーによってネイティブ Web コントロールが作成され、`HybridWebView.Uri` プロパティに指定された HTML ファイルが読み込まれます。
- Web ページが読み込まれると、カスタム レンダラーによって `invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- ユーザーが名前を入力して HTML `button` 要素をクリックすると、`invokeCSCode` 関数が呼び出され、次に `invokeCSharpAction` 関数が呼び出されます。
- `invokeCSharpAction` 関数によってカスタム レンダラーのメソッドが呼び出され、次に `HybridWebView.InvokeAction` メソッドが呼び出されます。
- 登録済みの `Action` を呼び出す `HybridWebView.InvokeAction` メソッド。

以下のセクションでは、各プラットフォームでこのプロセスがどのように実装されるかについて説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示します。

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

            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                if (Control == null) {
                    userController = new WKUserContentController ();
                    var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                    userController.AddUserScript (script);
                    userController.AddScriptMessageHandler (this, "invokeAction");

                    var config = new WKWebViewConfiguration { UserContentController = userController };
                    var webView = new WKWebView (Frame, config);
                    SetNativeControl (webView);
                }
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

`HybridWebViewRenderer` クラスによって、`HybridWebView.Uri` プロパティに指定された Web ページがネイティブの [`WKWebView`](xref:WebKit.WKWebView) コントロールに読み込まれ、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。 ユーザーが名前を入力して HTML `button`要素をクリックすると、`invokeCSharpAction` JavaScript 関数が実行され、Web ページからメッセージが受信された後に `DidReceiveScriptMessage` メソッドが呼び出されます。 さらに、このメソッドによって `HybridWebView.InvokeAction` メソッドが呼び出され、ポップアップを表示する登録済みのアクションが呼び出されます。

この関数は、次のように実現されます。

- カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合:
  - `Control` プロパティが `null` の場合、次の操作が実行されます。
    - [`WKUserContentController`](xref:WebKit.WKUserContentController) インスタンスが作成され、メッセージの投稿とユーザー スクリプトの Web ページへの挿入が可能になります。
    - Web ページが読み込まれた後、`invokeCSharpAction` JavaScript 関数を Web ページに挿入する [`WKUserScript`](xref:WebKit.WKUserScript) インスタンスが作成されます。
    - [`WKUserContentController.AddUserScript`](xref:WebKit.WKUserContentController.AddUserScript(WebKit.WKUserScript)) メソッドによって、コンテンツ コントローラーに [`WKUserScript`](xref:WebKit.WKUserScript) インスタンスが追加されます。
    - [`WKUserContentController.AddScriptMessageHandler`](xref:WebKit.WKUserContentController.AddScriptMessageHandler(WebKit.IWKScriptMessageHandler,System.String)) メソッドによって、[`WKUserContentController`](xref:WebKit.WKUserContentController) インスタンスに `invokeAction` というスクリプト メッセージ ハンドラーが追加されます。その結果、`WKUserContentController` インスタンスを使用するすべての Web ビューのすべてのフレームで JavaScript 関数 `window.webkit.messageHandlers.invokeAction.postMessage(data)` が定義されます。
    - [`WKUserContentController`](xref:WebKit.WKUserContentController) インスタンスがコンテンツ コントローラーとして設定されている [`WKWebViewConfiguration`](xref:WebKit.WKWebViewConfiguration) インスタンスが作成されます。
    - [`WKWebView`](xref:WebKit.WKWebView) コントロールがインスタンス化され、`WKWebView` コントロールへの参照を `Control` プロパティに割り当てる `SetNativeControl` メソッドが呼び出されます。
  - [`WKWebView.LoadRequest`](xref:WebKit.WKWebView.LoadRequest(Foundation.NSUrlRequest)) メソッドによって、`HybridWebView.Uri` プロパティにより指定されている HTML ファイルが読み込まれます。 このコードにより、ファイルがプロジェクトの `Content` フォルダーに格納されることが指定されます。 Web ページが表示されると、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- レンダラーがアタッチされている要素が変更された場合:
  - リソースがリリースされます。

> [!NOTE]
> `WKWebView` クラスは、iOS 8 以降でのみサポートされています。

さらに、次の値を含むように、**Info.plist** を更新する必要があります。

```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavascriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                if (Control == null)
                {
                    var webView = new Android.Webkit.WebView(_context);
                    webView.Settings.JavaScriptEnabled = true;
                    webView.SetWebViewClient(new JavascriptWebViewClient($"javascript: {JavascriptFunction}"));
                    SetNativeControl(webView);
                }
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl($"file:///android_asset/Content/{Element.Uri}");
            }
        }
    }
}
```

`HybridWebViewRenderer` クラスによって、`HybridWebView.Uri` プロパティに指定された Web ページがネイティブの [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) コントロールに読み込まれます。また、Web ページの読み込みが完了した後に、`JavascriptWebViewClient` クラスの `OnPageFinished` オーバーライドを使用して、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。

```csharp
public class JavascriptWebViewClient : WebViewClient
{
    string _javascript;

    public JavascriptWebViewClient(string javascript)
    {
        _javascript = javascript;
    }

    public override void OnPageFinished(WebView view, string url)
    {
        base.OnPageFinished(view, url);
        view.EvaluateJavascript(_javascript, null);
    }
}
```

ユーザーが名前を入力して HTML `button` 要素をクリックすると、`invokeCSharpAction` JavaScript 関数が実行されます。 この関数は、次のように実現されます。

- カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合:
  - `Control` プロパティが `null` の場合、次の操作が実行されます。
    - ネイティブの [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) インスタンスが作成され、JavaScript がコントロールで有効になり、`JavascriptWebViewClient` インスタンスが `WebViewClient` の実装として設定されます。
    - ネイティブの [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) コントロールへの参照を `Control` プロパティに割り当てる `SetNativeControl` メソッドが呼び出されます。
  - [`WebView.AddJavascriptInterface`](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) メソッドによって、新しい `JSBridge` インスタンスが WebView の JavaScript コンテキストのメイン フレームに挿入され、`jsBridge` と名前が付けられます。 これにより、JavaScript から `JSBridge` クラスのメソッドにアクセスできるようになります。
  - [`WebView.LoadUrl`](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) メソッドによって、`HybridWebView.Uri` プロパティに指定された HTML ファイルが読み込まれます。 このコードにより、ファイルがプロジェクトの `Content` フォルダーに格納されることが指定されます。
  - `JavascriptWebViewClient` クラスでは、Web ページの読み込みが完了すると、`invokeCSharpAction` JavaScript 関数がページに挿入されます。
- レンダラーがアタッチされている要素が変更された場合:
  - リソースがリリースされます。

`invokeCSharpAction` JavaScript 関数が実行されると、次に `JSBridge.InvokeAction` メソッドが呼び出されます。これを次のコード例に示します。

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

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer))
    {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

このクラスは `Java.Lang.Object` から派生する必要があります。また、JavaScript に公開されるメソッドは `[JavascriptInterface]` および `[Export]` 属性で修飾する必要があります。 そのため、`invokeCSharpAction` JavaScript 関数が Web ページに挿入され、実行されると、`[JavascriptInterface]` および `[Export("invokeAction")]` 属性で修飾されているため、`JSBridge.InvokeAction` メソッドが呼び出されます。 さらに、`InvokeAction` メソッドによって `HybridWebView.InvokeAction` メソッドが呼び出され、ポップアップを表示する登録済みのアクションが呼び出されます。

> [!NOTE]
> `[Export]` 属性を使用するプロジェクトには、`Mono.Android.Export` への参照が含まれている必要があります。含まれていない場合は、コンパイラ エラーが発生します。

`JSBridge` クラスは `WeakReference` を `HybridWebViewRenderer` クラスに保持する点に注意してください。 これは、2 つのクラス間に循環参照を作成しないようにするためです。 詳細については、MSDN の「[弱い参照](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx)」を参照してください。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP 上でのカスタム レンダラーの作成

次のコード例は、UWP 用のカスタム レンダラーを示します。

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

            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                if (Control == null)
                {
                    SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
                }
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

`HybridWebViewRenderer` クラスによって、`HybridWebView.Uri` プロパティに指定された Web ページがネイティブの `WebView` コントロールに読み込まれます。また、Web ページの読み込みが完了した後に、`WebView.InvokeScriptAsync` メソッドを使用して `invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。 ユーザーが名前を入力して HTML `button`要素をクリックすると、`invokeCSharpAction` JavaScript 関数が実行され、Web ページから通知が受信された後に `OnWebViewScriptNotify` メソッドが呼び出されます。 さらに、このメソッドによって `HybridWebView.InvokeAction` メソッドが呼び出され、ポップアップを表示する登録済みのアクションが呼び出されます。

この関数は、次のように実現されます。

- カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合:
  - `Control` プロパティが `null` の場合、次の操作が実行されます。
    - `SetNativeControl` メソッドは、新しいネイティブの `WebView` コントロールをインスタンス化し、それに対する参照を `Control` プロパティに割り当てるために呼び出されます。
  - `NavigationCompleted` および `ScriptNotify` イベントのイベント ハンドラーが登録されています。 `NavigationCompleted` イベントは、ネイティブの `WebView` コントロールが現在のコンテンツの読み込みを完了したとき、またはナビゲーションが失敗したときに発生します。 `ScriptNotify`イベントは、ネイティブの `WebView` コントロールのコンテンツが JavaScript を使用して文字列をアプリケーションに渡したときに発生します。 Web ページは、`string` パラメーターを渡しながら `window.external.notify` を呼び出して `ScriptNotify` イベントを発生させます。
  - `WebView.Source` プロパティは、`HybridWebView.Uri` プロパティに指定された HTML ファイルの URI に設定されます。 このコードでは、ファイルがプロジェクトの `Content` フォルダーに格納されることを想定しています。 Web ページが表示されると、`NavigationCompleted` イベントが発生し、`OnWebViewNavigationCompleted` メソッドが呼び出されます。 ナビゲーションが正常に完了した場合、`WebView.InvokeScriptAsync` メソッドを使用して `invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- レンダラーがアタッチされている要素が変更された場合:
  - イベントの登録が解除されます。

## <a name="summary"></a>まとめ

この記事では、`HybridWebView` カスタム コントロール用のカスタム レンダラーを作成する方法を説明し、プラットフォームに固有の Web コントロールを強化して、JavaScript からの C# コードの呼び出しを実現する方法を示しました。


## <a name="related-links"></a>関連リンク

- [CustomRendererHybridWebView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/HybridWebView/)
- [JavaScript から C# を呼び出す](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
