---
title: WebView のカスタマイズ
description: Xamarin.Forms WebView は、アプリに Web コンテンツと HTML コンテンツを表示するビューです。 この記事では、JavaScript から C# コードを呼び出せるように WebView を拡張するカスタム レンダラーを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/31/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e0653e46d2c349e05df8716e5114de8f631cab1a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939543"
---
# <a name="customizing-a-webview"></a>WebView のカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-hybridwebview)

_Xamarin.Forms `WebView` は、アプリに Web コンテンツと HTML コンテンツを表示するビューです。この記事では、JavaScript から C# コードを呼び出せるように `WebView` を拡張するカスタム レンダラーを作成する方法について説明します。_

すべての Xamarin.Forms ビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 Xamarin.Forms アプリケーションによって [`WebView`](xref:Xamarin.Forms.WebView) がレンダリングされると、iOS では `WkWebViewRenderer` クラスがインスタンス化され、それによってネイティブの `WkWebView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`WebViewRenderer` クラスによってネイティブの `WebView` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`WebViewRenderer` クラスによってネイティブの `WebView` コントロールがインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスおよびネイティブ コントロール) を参照してください。

次の図は、[`View`](xref:Xamarin.Forms.View) と、それを実装する、対応しているネイティブ コントロールの関係を示しています。

![WebView クラスとそれを実装するネイティブ クラス間のリレーションシップ](hybridwebview-images/webview-classes.png)

レンダリング プロセスを使用して各プラットフォーム上の [`WebView`](xref:Xamarin.Forms.WebView) にカスタム レンダラーを作成することで、プラットフォームのカスタマイズを実装することができます。 その実行プロセスは次のとおりです。

1. `HybridWebView` カスタム コントロールを[作成](#create-the-hybridwebview)します。
1. Xamarin.Forms から `HybridWebView` を[使用](#consume-the-hybridwebview)します。
1. 各プラットフォーム上で `HybridWebView` のカスタム レンダラーを[作成](#create-the-custom-renderer-on-each-platform)します。

Xamarin.Forms [`WebView`](xref:Xamarin.Forms.WebView) を強化して、JavaScript から C# コードを呼び出すことができる `HybridWebView` レンダラーを実装するために、各項目について順番に説明します。 `HybridWebView` インスタンスは、ユーザーに名前の入力を求める HTML ページを表示するために使用されます。 それからユーザーが HTML ボタンをクリックすると、JavaScript 関数によって、ユーザー名を含むポップアップを表示する C# `Action` が呼び出されます。

JavaScript から C# を呼び出すプロセスの詳細については、「[JavaScript から C# を呼び出す](#invoke-c-from-javascript)」を参照してください。 HTML ページの詳細については、「[Web ページの作成](#create-the-web-page)」を参照してください。

> [!NOTE]
> [`WebView`](xref:Xamarin.Forms.WebView) は、C# から JavaScript 関数を呼び出して、呼び出し元の C# コードに結果を返すことができます。 詳細については、「[JavaScript の呼び出し](~/xamarin-forms/user-interface/webview.md#invoking-javascript)」を参照してください。

## <a name="create-the-hybridwebview"></a>HybridWebView の作成

`HybridWebView` カスタム コントロールは、[`WebView`](xref:Xamarin.Forms.WebView) クラスをサブクラス化することによって作成できます。

```csharp
public class HybridWebView : WebView
{
    Action<string> action;

    public static readonly BindableProperty UriProperty = BindableProperty.Create(
        propertyName: "Uri",
        returnType: typeof(string),
        declaringType: typeof(HybridWebView),
        defaultValue: default(string));

    public string Uri
    {
        get { return (string)GetValue(UriProperty); }
        set { SetValue(UriProperty, value); }
    }

    public void RegisterAction(Action<string> callback)
    {
        action = callback;
    }

    public void Cleanup()
    {
        action = null;
    }

    public void InvokeAction(string data)
    {
        if (action == null || data == null)
        {
            return;
        }
        action.Invoke(data);
    }
}
```

`HybridWebView` カスタム コントロールは、.NET Standard ライブラリ プロジェクトで作成され、コントロール用に次の API を定義します。

- 読み込まれる Web ページのアドレスを指定する `Uri` プロパティ。
- `Action` をコントロールに登録する `RegisterAction` メソッド。 登録済みのアクションは、`Uri` プロパティで参照される HTML ファイルに含まれる JavaScript から呼び出されます。
- 登録済みの `Action` への参照を削除する `CleanUp` メソッド。
- 登録済みの `Action` を呼び出す `InvokeAction` メソッド。 このメソッドは、各プラットフォームのプロジェクトのカスタム レンダラーから呼び出されます。

## <a name="consume-the-hybridwebview"></a>HybridWebView の使用

`HybridWebView` カスタム コントロールは、その場所の名前空間を宣言し、カスタム コントロール上で名前空間プレフィックスを使用することで .NET Standard ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページがどのように `HybridWebView` カスタム コントロールを使用できるかを示しています。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,40,0,0">
    <local:HybridWebView x:Name="hybridWebView"
                         Uri="index.html" />
</ContentPage>

```

`local` 名前空間プレフィックスには任意の名前を付けることができます。 ただし、`clr-namespace` と `assembly` の値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム コントロールが参照されます。

次のコード例は、C# ページがどのように `HybridWebView` カスタム コントロールを使用できるかを示しています。

```csharp
public HybridWebViewPageCS()
{
    var hybridWebView = new HybridWebView
    {
        Uri = "index.html"
    };
    // ...
    Padding = new Thickness(0, 40, 0, 0);
    Content = hybridWebView;
}
```

`HybridWebView` インスタンスは、各プラットフォーム上でネイティブ Web コントロールを表示するために使用されます。 `Uri` プロパティは、各プラットフォームのプロジェクトに格納されている HTML ファイルに設定され、ネイティブの Web コントロールによって表示されます。 レンダリングされた HTML は、HTML ボタンのクリックに応答して C# `Action` を呼び出す JavaScript 関数を使用して、ユーザーに名前を入力するように求めます。

`HybridWebViewPage` は、次のコード例に示すように、JavaScript から呼び出されるアクションを登録します。

```csharp
public partial class HybridWebViewPage : ContentPage
{
    public HybridWebViewPage()
    {
        // ...
        hybridWebView.RegisterAction(data => DisplayAlert("Alert", "Hello " + data, "OK"));
    }
}
```

このアクションから、モーダル ポップアップを表示する [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) メソッドが呼び出されます。このポップアップには、`HybridWebView` インスタンスによって表示される HTML ページに入力された名前が示されます。

カスタム レンダラーを各アプリケーション プロジェクトに追加して、C# コードを JavaScript から呼び出すことで、プラットフォームの Web コントロールを強化することができます。

## <a name="create-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. iOS で `WkWebViewRenderer` クラスのサブクラス、Android と UWP で `WebViewRenderer` クラスを作成して、カスタム コントロールをレンダリングします。
1. [`WebView`](xref:Xamarin.Forms.WebView) をレンダリングする `OnElementChanged` メソッドをオーバーライドし、ロジックを書き込んでカスタマイズします。 このメソッドは、`HybridWebView` オブジェクトが作成されるときに呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスまたは *AssemblyInfo.cs* に追加して、Xamarin.Forms カスタム コントロールのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラス用の既定のレンダラーが使用されます。 ただし、[View](xref:Xamarin.Forms.View) 要素をレンダリングするときは、各プラットフォーム プロジェクトにカスタム レンダラーが必要です。

次の図に、サンプル アプリケーション内の各プロジェクトの役割と、それらの関係を示します。

![HybridWebView カスタム レンダラーのプロジェクトの役割](hybridwebview-images/solution-structure.png)

`HybridWebView` カスタム コントロールはプラットフォームのレンダラー クラスによってレンダリングされます。このクラスは iOS では `WkWebViewRenderer` クラスから、Android と UWP では `WebViewRenderer` クラスから派生します。 この結果、次のスクリーンショットに示すように、ネイティブの Web コントロールを使用してそれぞれの `HybridWebView` カスタム コントロールがレンダリングされます。

![各プラットフォーム上の HybridWebView](hybridwebview-images/screenshots.png)

`WkWebViewRenderer` クラスと `WebViewRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ Web コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 このメソッドでは、`OldElement` および `NewElement` プロパティを含む `VisualElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーがアタッチされて*いた* Xamarin.Forms 要素と、レンダラーが現在アタッチされて*いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `HybridWebView` インスタンスへの参照が含まれます。

各プラットフォームのレンダラー クラス内の、オーバーライドされたバージョンの `OnElementChanged` メソッドは、ネイティブの Web コントロールのカスタマイズを行う場所です。 レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms カスタム コントロールの型名と、カスタム レンダラーの型名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

以下のセクションでは、各ネイティブ Web コントロールによって読み込まれる Web ページの構造、JavaScript から C# を呼び出すプロセス、各プラットフォームのカスタム レンダラー クラスでこれを実装する方法について説明します。

### <a name="create-the-web-page"></a>Web ページの作成

次のコード例は、`HybridWebView` カスタム コントロールによって表示される Web ページを示しています。

```html
<html>
<body>
    <script src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
    <h1>HybridWebView Test</h1>
    <br />
    Enter name: <input type="text" id="name">
    <br />
    <br />
    <button type="button" onclick="javascript: invokeCSCode($('#name').val());">Invoke C# Code</button>
    <br />
    <p id="result">Result:</p>
    <script type="text/javascript">function log(str) {
            $('#result').text($('#result').text() + " " + str);
        }

        function invokeCSCode(data) {
            try {
                log("Sending Data:" + data);
                invokeCSharpAction(data);
            }
            catch (err) {
                log(err);
            }
        }</script>
</body>
</html>
```

この Web ページでは、ユーザーは `input` 要素に名前を入力することができます。また、クリックすると C# コードを呼び出す`button` 要素が用意されています。 これを実現するプロセスは次のとおりです。

- ユーザーが `button` 要素をクリックすると、`invokeCSCode` JavaScript 関数が呼び出され、`input` 要素の値が関数に渡されます。
- `invokeCSCode` 関数からは、C# `Action` に送信されているデータを表示する `log` 関数が呼び出されます。 次に、C# `Action` を呼び出す `invokeCSharpAction` メソッドが呼び出され、`input` 要素から受け取ったパラメーターが渡されます。

`invokeCSharpAction` JavaScript 関数は Web ページで定義されておらず、各カスタム レンダラーによって挿入されます。

iOS の場合、この HTML ファイルは、**BundleResource** のビルド アクションと一緒に、プラットフォーム プロジェクトの Content フォルダーに置かれます。 Android の場合、この HTML ファイルは、**AndroidAsset** のビルド アクションと一緒にプラットフォーム プロジェクトの Assets/Content フォルダーに置かれます。

### <a name="invoke-c-from-javascript"></a>JavaScript から C# を呼び出す

JavaScript から C# を呼び出すプロセスは、各プラットフォームで同じです。

- カスタム レンダラーによってネイティブ Web コントロールが作成され、`HybridWebView.Uri` プロパティに指定された HTML ファイルが読み込まれます。
- Web ページが読み込まれると、カスタム レンダラーによって `invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- ユーザーが名前を入力して HTML `button` 要素をクリックすると、`invokeCSCode` 関数が呼び出され、次に `invokeCSharpAction` 関数が呼び出されます。
- `invokeCSharpAction` 関数によってカスタム レンダラーのメソッドが呼び出され、次に `HybridWebView.InvokeAction` メソッドが呼び出されます。
- 登録済みの `Action` を呼び出す `HybridWebView.InvokeAction` メソッド。

以下のセクションでは、各プラットフォームでこのプロセスがどのように実装されるかについて説明します。

### <a name="create-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : WkWebViewRenderer, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        public HybridWebViewRenderer() : this(new WKWebViewConfiguration())
        {
        }

        public HybridWebViewRenderer(WKWebViewConfiguration config) : base(config)
        {
            userController = config.UserContentController;
            var script = new WKUserScript(new NSString(JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
            userController.AddUserScript(script);
            userController.AddScriptMessageHandler(this, "invokeAction");
        }

        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                userController.RemoveAllUserScripts();
                userController.RemoveScriptMessageHandler("invokeAction");
                HybridWebView hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }

            if (e.NewElement != null)
            {
                string filename = Path.Combine(NSBundle.MainBundle.BundlePath, $"Content/{((HybridWebView)Element).Uri}");
                LoadRequest(new NSUrlRequest(new NSUrl(filename, false)));
            }
        }

        public void DidReceiveScriptMessage(WKUserContentController userContentController, WKScriptMessage message)
        {
            ((HybridWebView)Element).InvokeAction(message.Body.ToString());
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

`HybridWebViewRenderer` クラスによって、`HybridWebView.Uri` プロパティに指定された Web ページがネイティブの [`WKWebView`](xref:WebKit.WKWebView) コントロールに読み込まれ、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。 ユーザーが名前を入力して HTML `button`要素をクリックすると、`invokeCSharpAction` JavaScript 関数が実行され、Web ページからメッセージが受信された後に `DidReceiveScriptMessage` メソッドが呼び出されます。 さらに、このメソッドによって `HybridWebView.InvokeAction` メソッドが呼び出され、ポップアップを表示する登録済みのアクションが呼び出されます。

この関数は、次のように実現されます。

- レンダラー コンストラクターによって `WkWebViewConfiguration` オブジェクトが作成され、その [`WKUserContentController`](xref:WebKit.WKUserContentController) オブジェクトが取得されます。 `WkUserContentController` オブジェクトにより、メッセージの投稿とユーザー スクリプトの Web ページへの挿入が可能になります。
- レンダラー コンストラクターにより [`WKUserScript`](xref:WebKit.WKUserScript) オブジェクトが作成されます。これにより、Web ページが読み込まれた後に、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- レンダラー コンストラクターによって、[`WKUserContentController.AddUserScript`](xref:WebKit.WKUserContentController.AddUserScript(WebKit.WKUserScript)) メソッドが呼び出され、[`WKUserScript`](xref:WebKit.WKUserScript) オブジェクトがコンテンツ コントローラーに追加されます。
- レンダラー コンストラクターによって [`WKUserContentController.AddScriptMessageHandler`](xref:WebKit.WKUserContentController.AddScriptMessageHandler(WebKit.IWKScriptMessageHandler,System.String)) メソッドが呼び出され、[`WKUserContentController`](xref:WebKit.WKUserContentController) オブジェクトに `invokeAction` というスクリプト メッセージ ハンドラーが追加されます。その結果、`WKUserContentController` オブジェクトを使用するすべての `WebView` インスタンス内のすべてのフレームで JavaScript 関数 `window.webkit.messageHandlers.invokeAction.postMessage(data)` が定義されます。
- カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合:
  - [`WKWebView.LoadRequest`](xref:WebKit.WKWebView.LoadRequest(Foundation.NSUrlRequest)) メソッドによって、`HybridWebView.Uri` プロパティにより指定されている HTML ファイルが読み込まれます。 このコードにより、ファイルがプロジェクトの `Content` フォルダーに格納されることが指定されます。 Web ページが表示されると、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- レンダラーがアタッチされている要素が変更されると、リソースが解放されます。
- レンダラーが破棄されると、Xamarin.Forms 要素がクリーンアップされます。

> [!NOTE]
> `WKWebView` クラスは、iOS 8 以降でのみサポートされています。

さらに、次の値を含むように、**Info.plist** を更新する必要があります。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

### <a name="create-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : WebViewRenderer
    {
        const string JavascriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<WebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                ((HybridWebView)Element).Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.SetWebViewClient(new JavascriptWebViewClient(this, $"javascript: {JavascriptFunction}"));
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl($"file:///android_asset/Content/{((HybridWebView)Element).Uri}");
            }
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

`HybridWebViewRenderer` クラスによって、`HybridWebView.Uri` プロパティに指定された Web ページがネイティブの [`WebView`](xref:Android.Webkit.WebView) コントロールに読み込まれます。また、Web ページの読み込みが完了した後に、`JavascriptWebViewClient` クラスの `OnPageFinished` オーバーライドを使用して、`invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。

```csharp
public class JavascriptWebViewClient : FormsWebViewClient
{
    string _javascript;

    public JavascriptWebViewClient(HybridWebViewRenderer renderer, string javascript) : base(renderer)
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
  - `SetWebViewClient` メソッドにより `WebViewClient` の実装として新しい `JavascriptWebViewClient` オブジェクトが設定されます。
  - [`WebView.AddJavascriptInterface`](xref:Android.Webkit.WebView.AddJavascriptInterface*) メソッドによって、新しい `JSBridge` インスタンスが WebView の JavaScript コンテキストのメイン フレームに挿入され、`jsBridge` と名前が付けられます。 これにより、JavaScript から `JSBridge` クラスのメソッドにアクセスできるようになります。
  - [`WebView.LoadUrl`](xref:Android.Webkit.WebView.LoadUrl*) メソッドによって、`HybridWebView.Uri` プロパティに指定された HTML ファイルが読み込まれます。 このコードにより、ファイルがプロジェクトの `Content` フォルダーに格納されることが指定されます。
  - `JavascriptWebViewClient` クラスでは、Web ページの読み込みが完了すると、`invokeCSharpAction` JavaScript 関数がページに挿入されます。
- レンダラーがアタッチされている要素が変更されると、リソースが解放されます。
- レンダラーが破棄されると、Xamarin.Forms 要素がクリーンアップされます。

`invokeCSharpAction` JavaScript 関数が実行されると、次に `JSBridge.InvokeAction` メソッドが呼び出されます。これを次のコード例に示します。

```csharp
public class JSBridge : Java.Lang.Object
{
    readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

    public JSBridge(HybridWebViewRenderer hybridRenderer)
    {
        hybridWebViewRenderer = new WeakReference<HybridWebViewRenderer>(hybridRenderer);
    }

    [JavascriptInterface]
    [Export("invokeAction")]
    public void InvokeAction(string data)
    {
        HybridWebViewRenderer hybridRenderer;

        if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget(out hybridRenderer))
        {
            ((HybridWebView)hybridRenderer.Element).InvokeAction(data);
        }
    }
}
```

このクラスは `Java.Lang.Object` から派生する必要があります。また、JavaScript に公開されるメソッドは `[JavascriptInterface]` および `[Export]` 属性で修飾する必要があります。 そのため、`invokeCSharpAction` JavaScript 関数が Web ページに挿入され、実行されると、`[JavascriptInterface]` および `[Export("invokeAction")]` 属性で修飾されているため、`JSBridge.InvokeAction` メソッドが呼び出されます。 さらに、`InvokeAction` メソッドによって `HybridWebView.InvokeAction` メソッドが呼び出され、ポップアップを表示する登録済みのアクションが呼び出されます。

> [!IMPORTANT]
> `[Export]` 属性を使用する Android プロジェクトには、`Mono.Android.Export` への参照が含まれている必要があります。含まれていない場合は、コンパイラ エラーが発生します。

`JSBridge` クラスは `WeakReference` を `HybridWebViewRenderer` クラスに保持する点に注意してください。 これは、2 つのクラス間に循環参照を作成しないようにするためです。 詳細については、「[弱い参照](/en-us/dotnet/standard/garbage-collection/weak-references)」を参照してください。

### <a name="create-the-custom-renderer-on-uwp"></a>UWP 上でのカスタム レンダラーの作成

次のコード例で、UWP 用のカスタム レンダラーを示します。

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : WebViewRenderer
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.WebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri($"ms-appx-web:///Content//{((HybridWebView)Element).Uri}");
            }
        }

        async void OnWebViewNavigationCompleted(Windows.UI.Xaml.Controls.WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            ((HybridWebView)Element).InvokeAction(e.Value);
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

`HybridWebViewRenderer` クラスによって、`HybridWebView.Uri` プロパティに指定された Web ページがネイティブの `WebView` コントロールに読み込まれます。また、Web ページの読み込みが完了した後に、`WebView.InvokeScriptAsync` メソッドを使用して `invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。 ユーザーが名前を入力して HTML `button`要素をクリックすると、`invokeCSharpAction` JavaScript 関数が実行され、Web ページから通知が受信された後に `OnWebViewScriptNotify` メソッドが呼び出されます。 さらに、このメソッドによって `HybridWebView.InvokeAction` メソッドが呼び出され、ポップアップを表示する登録済みのアクションが呼び出されます。

この関数は、次のように実現されます。

- カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合:
  - `NavigationCompleted` および `ScriptNotify` イベントのイベント ハンドラーが登録されています。 `NavigationCompleted` イベントは、ネイティブの `WebView` コントロールが現在のコンテンツの読み込みを完了したとき、またはナビゲーションが失敗したときに発生します。 `ScriptNotify`イベントは、ネイティブの `WebView` コントロールのコンテンツが JavaScript を使用して文字列をアプリケーションに渡したときに発生します。 Web ページは、`string` パラメーターを渡しながら `window.external.notify` を呼び出して `ScriptNotify` イベントを発生させます。
  - `WebView.Source` プロパティは、`HybridWebView.Uri` プロパティに指定された HTML ファイルの URI に設定されます。 このコードでは、ファイルがプロジェクトの `Content` フォルダーに格納されることを想定しています。 Web ページが表示されると、`NavigationCompleted` イベントが発生し、`OnWebViewNavigationCompleted` メソッドが呼び出されます。 ナビゲーションが正常に完了した場合、`WebView.InvokeScriptAsync` メソッドを使用して `invokeCSharpAction` JavaScript 関数が Web ページに挿入されます。
- レンダラーがアタッチされている要素が変更されると、イベントのサブスクライブが解除されます。
- レンダラーが破棄されると、Xamarin.Forms 要素がクリーンアップされます。

## <a name="related-links"></a>関連リンク

- [HybridWebView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-hybridwebview)
