---
title: Razor テンプレートを使用して構築 HTML ビュー
description: " 全画面表示の web ページを使用して HTML を表示するためには、web サイト プロジェクトから、HTML、JavaScript と CSS が既にある場合に特にクロスプラット フォーム対応の方法で複雑な書式設定を表示するためにシンプルかつ効果的な方法です。"
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: 539f59b9835cab6281327bcd1a37482ef82b62cc
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650174"
---
# <a name="building-html-views-using-razor-templates"></a>Razor テンプレートを使用して構築 HTML ビュー

モバイル開発の世界で用語「ハイブリッド アプリ」は、通常、ホストされた web ビューアー コントロールの内部 HTML ページとしてその画面の一部 (またはすべて) を表示するアプリケーションを指します。

使用する一部の開発環境がある、プラットフォームにも制限が、それらのアプリの複雑な処理や UI 効果を実現しようとしていますが、パフォーマンスの問題から低下し、は HTML と JavaScript で完全には、次のモバイル アプリをビルド機能にアクセスできます。

Xamarin は、HTML を Razor テンプレート エンジンを利用する場合に特にの両方の長所を提供します。 Xamarin を使用した JavaScript と CSS を使用してもが、基になるプラットフォーム Api に完全にアクセスし使用した処理を高速、クロスプラット フォームで template 宣言された HTML ビューを構築する柔軟性があるC#します。

このドキュメントでは、Razor テンプレート エンジンを使用して、Xamarin を使用してモバイル プラットフォーム間で使用できる HTML と JavaScript と CSS のビューを構築する方法について説明します。

## <a name="using-web-views-programmatically"></a>プログラムによる Web ビューの使用

Razor について学習する前に、アプリ内で生成される HTML コンテンツ具体的には直接 – HTML コンテンツを表示する web ビューを使用する方法について説明します。

簡単に作成し、C# を使用して HTML を表示、Xamarin は iOS と Android の両方では、基になるプラットフォーム API に完全にアクセスを提供します。 各プラットフォームの基本構文は、以下に示します。

### <a name="ios"></a>iOS

数行のコードを受け取るも Xamarin.iOS で UIWebView コントロールの HTML を表示します。

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

参照してください、 [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) UIWebView コントロールの使用の詳細についてはレシピです。

### <a name="android"></a>Android

Xamarin.Android を使用して、WebView コントロールの HTML を表示すると、わずか数行のコードで実現されます。

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable JavaScript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

参照してください、 [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) WebView コントロールの使用に関する詳細のレシピです。

### <a name="specifying-the-base-directory"></a>基本ディレクトリを指定します。

両方のプラットフォームでは、HTML ページの基本ディレクトリを指定するパラメーターです。 これは、イメージ、CSS ファイルなどのリソースへの相対参照を解決するのには使用されているデバイスのファイル システム上の場所です。 たとえば、タグのような

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

これらのファイルを参照してください: **style.css**、 **monkey.jpg**と**jscript.js**します。 これらのファイルが配置されるページに読み込むことができるように、web ビューのベース ディレクトリの設定に指示します。

#### <a name="ios"></a>iOS

テンプレートの出力は次の C# コードでの iOS でレンダリングされます。

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

ベース ディレクトリとして指定`NSBundle.MainBundle.BundleUrl`にアプリケーションがインストールされているディレクトリを参照します。 内のすべてのファイル、**リソース**などフォルダーをこの場所にコピーされる、 **style.css**ファイルを次に示します。

 ![iPhoneHybrid ソリューション](images/image1_240x163.png)

すべての静的なコンテンツ ファイルのビルド アクションをする必要があります**BundleResource**:

 ![iOS プロジェクトのビルド アクション:BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android では、html 文字列は、web ビューで表示されるときに、パラメーターとして渡されるベース ディレクトリも必要です。

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

特殊な文字列**file:///android_asset/** アプリで、表示は、ここを格納している Android Assets フォルダーを指す、 **style.css**ファイル。

 ![AndroidHybrid ソリューション](images/image3_240x167.png)

すべての静的なコンテンツ ファイルのビルド アクションをする必要があります**AndroidAsset**します。

 ![Android プロジェクトのビルド アクション:AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>呼び出すC#HTML および JavaScript から

Web ビューに html ページが読み込まれると、ページがサーバーから読み込まれた場合と、リンクとフォームに扱います。 つまり、ユーザーがリンクをクリックしてまたはフォームを送信する場合、web ビューに指定したターゲットに移動しようとします。

リンクが (google.com) などの外部のサーバーにある場合、web ビューは (インターネット接続があると仮定)、外部の web サイトの読み込みにしようとします。

```html
<a href="http://google.com/">Google</a>
```

リンクが相対の場合、web ビューはベースのディレクトリからそのコンテンツの読み込みにしようとします。 明らかにネットワーク接続は必要ありませんするのには、このデバイス上のアプリにコンテンツが保存されています。

```html
<a href="somepage.html">Local content</a>
```

フォームのアクションでは、同じ規則に従います。

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

クライアントの web サーバーをホストするねただし、HTTP GET 経由でサービスを呼び出す今日のレスポンシブ デザイン パターンで採用されている同じサーバー通信技術を使用して JavaScript を生成することによって、応答を非同期に処理 (または呼び出し側の JavaScript は web ビューで既にホストされている)。 これにより、簡単に処理し、表示、HTML ページ上の結果をバックアップ用の C# コードに戻りましょう、HTML からデータを渡すことができます。

IOS と Android の両方は、アプリケーション コード (必要な) 場合、アプリのコードは応答できるように、これらのナビゲーション イベントをインターセプトするためのメカニズムを提供します。 この機能は、ネイティブ コードが web ビューを操作できるので、ハイブリッド アプリを構築するために重要です。

#### <a name="ios"></a>iOS

(リンクをクリックして) など、ナビゲーション要求を処理するためにアプリケーション コードを許可する iOS web ビューに関する ShouldStartLoad イベントをオーバーライドできます。 メソッドのパラメーターは、すべての情報を提供します。

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

イベント ハンドラーを割り当てます。

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android で単にサブクラス WebViewClient し、ナビゲーション要求に応答するコードを実装します。

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

web ビューで、クライアントを設定します。

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>JavaScript からの呼び出しC#

読み込む新しい HTML ページでは、web ビューを示すだけでなくC#コードが現在表示されているページ内で JavaScript をでも実行できます。 使用して全体の JavaScript コードのブロックを作成できますC#文字列し、実行すると、JavaScript を使用してページで既に使用可能なメソッドの呼び出しを作成するまたは`script`タグ。

#### <a name="android"></a>Android

JavaScript コードが実行され、それをプレフィックスの作成"javascript:"web ビューは、その文字列を読み込むように指示するとします。

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS web ビューでは、JavaScript を呼び出す具体的には、メソッドを提供します。

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>まとめ

このセクションには、Android と xamarin では、ハイブリッド アプリケーションを構築する iOS の両方の web ビュー コントロールの機能が導入されてなど。

-  コードでは、生成された文字列から HTML をロードする機能
-  ローカル ファイル (CSS、JavaScript、イメージまたはその他の HTML ファイル) を参照する機能
-  C# のコードでナビゲーション要求をインターセプトする機能
-  JavaScript から呼び出す機能C#コード。


次のセクションでは、Razor で、簡単にハイブリッド アプリで使用する HTML を作成することについて説明します。

## <a name="what-is-razor"></a>Razor とは何ですか。

Razor は、サーバー上で実行し、web ブラウザーに提供する HTML を生成する最初の ASP.NET MVC で導入されたテンプレート エンジンです。

Razor テンプレート エンジンで標準の HTML の構文を拡張するC#、レイアウトを表現して CSS スタイル シートおよび JavaScript を簡単に組み込むようにします。 テンプレートには、任意のカスタム型を指定することができ、プロパティを持つは、テンプレートから直接アクセスできるモデル クラスを参照できます。 その主な利点の 1 つは、簡単に HTML と C# の構文を混在させる機能です。

Razor テンプレートがサーバー側の使用に限定されない、Xamarin アプリでそれらに含めることもできます。 プログラムの web ビューで処理する機能だけでなく Razor テンプレートを使用すると、Xamarin でビルドする高度なクロスプラット フォーム対応のハイブリッド アプリケーションができます。

### <a name="razor-template-basics"></a>Razor テンプレートの基礎

Razor テンプレート ファイルが、 **.cshtml**ファイル拡張子。 テキスト テンプレートのセクションから Xamarin プロジェクトに追加することができます、**新しいファイル**ダイアログ。

 ![新しいファイル - Razor テンプレート](images/image5_400x201.png)

単純な Razor テンプレート ( **RazorView.cshtml**) を次に示します。

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

通常の HTML ファイルから次の相違点に注意してください。

-  `@`シンボルは、Razor テンプレートで特別な意味を持つ – こと、次の式は、C# に評価されることを示します。
- `@model` ディレクティブは、常に、Razor テンプレート ファイルの最初の行として表示されます。
-  `@model`型ディレクティブの後にする必要があります。 この例をテンプレートに単純な文字列が渡されるが、任意のカスタム クラスを使用できます。
-  ときに`@Model`参照は、テンプレート全体で (この例では、これは文字列になります) で生成されたテンプレートに渡されたオブジェクトへの参照を提供します。
-  IDE のテンプレートの部分クラスが自動的に生成 (を使用するファイル、 **.cshtml**拡張機能)。 このコードを表示できますが、編集しないでください。
 ![RazorView.cshtml](images/image6_125x34.png) RazorView .cshtml テンプレートのファイル名と一致するという名前の部分クラスです。 この名前の c# コードでテンプレートを参照するために使用することをお勧めします。
- `@using` ステートメントは、Razor テンプレートに含める追加の名前空間の上部に含めることもできます。


最終的な HTML 出力は、次の C# コードを生成できます。 文字列"Hello World"が表示されるテンプレートの出力に組み込まれる予定であること、モデルを指定するに注意してください。

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

IOS シミュレーターと Android エミュレーターでの web ビューに表示される出力を次に示します。

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Razor 構文の詳細

開始するためのいくつかの基本的な Razor 構文を紹介するこのセクションでは、これを使用します。 このセクションの例では、次のクラスにデータを設定、表示 Razor を使用します。

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

すべての例は、次のデータの初期化コードを使用します。

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>モデルのプロパティを表示します。

モデルがプロパティを持つクラスである場合は、このテンプレートの例で示すように Razor テンプレートで簡単に参照がされます。

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

これは、次のコードを使用して文字列に表示することができます。

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

最終的な出力が iOS シミュレーターと Android エミュレーターでの web ビューのとおりです。

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>C# のステートメント

複雑な C# モデル プロパティの更新プログラムとこの例では、年齢の計算など、テンプレートに含めることができます。

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

使用してコードを囲むことで (経過時間の書式設定) などの複雑な単一行 C# 式を記述する`@()`します。

囲んで、複数の C# ステートメントを記述できます`@{}`します。

#### <a name="if-else-statements"></a>If-else ステートメント

コード分岐で表現できる`@if`このテンプレートの例で示すようにします。

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>ループ

ループのような構造`foreach`も追加できます。 `@`ループ変数のプレフィックスを使用できます (`@food`ここで) HTML でレンダリングします。

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

上記のテンプレートの出力を表示するには、iOS シミュレーターと Android エミュレーターで実行されています。

 ![Rupert X Monkey](images/image9_520x277.png)

このセクションでは、Razor テンプレートを使用した単純な読み取り専用ビューをレンダリングの基礎について説明しました。 次のセクションがユーザー入力を受け付け、HTML ビューでの JavaScript の間で相互運用する、Razor を使用してより完全なアプリを構築する方法について説明し、C#します。

## <a name="using-razor-templates-with-xamarin"></a>Xamarin を使用した Razor テンプレートの使用

このセクションは、Visual studio for mac。 ソリューション テンプレートを使用して、独自のハイブリッド アプリケーションをビルドに使用する方法を説明します 次の 3 つのテンプレートがあるから使用可能な**ファイル > 新規 > ソリューション.** ウィンドウ。

- **Android > アプリ > Android WebView アプリケーション**
- **iOS > アプリ > の web ビュー アプリケーション**
- **ASP.NET MVC プロジェクト**



**新しいソリューション**ウィンドウ次のような iPhone と Android プロジェクト - 右側のソリューションの説明には、Razor テンプレート エンジンのサポートが強調表示されます。

 ![IPhone と Android のソリューションを作成します。](images/image13_1139x959.png)

簡単に追加することに注意してください、 **.cshtml** Razor テンプレート*任意*の既存の Xamarin プロジェクト、その必要はありませんこれらのソリューション テンプレートを使用します。 iOS プロジェクトにはいずれも; Razor を使用するストーリー ボードは必要ありません。単にプログラムで UIWebView コントロールを任意のビューに追加し、c# コードで全体 Razor テンプレートをレンダリングすることができます。

IPhone と Android プロジェクト用の既定テンプレート ソリューションの内容を次に示します。

 ![iPhone や Android テンプレート](images/image10_428x310.png)

テンプレートでは、データ モデル オブジェクトに Razor テンプレートを読み込み、ユーザー入力を処理、および JavaScript を使用して、ユーザーへの通信をすぐにアプリケーション インフラストラクチャを提供します。

ソリューションの重要な部分は次のとおりです。

-  などの静的コンテンツ、 **style.css**ファイル。
-  Razor テンプレート ファイルの .cshtml **RazorView.cshtml**します。
-  モデル クラスなど、Razor テンプレートで参照されている**ExampleModel.cs**します。
-  Web ビューを作成しなど、テンプレートをレンダリング プラットフォーム固有のクラス、 `MainActivity` android、および`iPhoneHybridViewController`iOS でします。


次のセクションでは、プロジェクトの作業について説明します。

### <a name="static-content"></a>静的コンテンツ

静的コンテンツには、CSS スタイル シートやイメージ、JavaScript ファイルからリンクまたは web ビューに表示されている HTML ファイルで参照できるその他のコンテンツが含まれています。

プロジェクト テンプレートには、ハイブリッド アプリでの静的コンテンツを含める方法を示すために最小限のスタイル シートが含まれます。 このようなテンプレートでは、CSS スタイル シートを参照します。

```html
<link rel="stylesheet" href="style.css" />
```

任意のスタイル シートや JQuery などのフレームワークなど、必要な JavaScript ファイルを追加することができます。

### <a name="razor-cshtml-templates"></a>Razor cshtml テンプレート

テンプレートには、Razor **.cshtml**が HTML または JavaScript の間でデータを伝達するためのコード、事前に記述ファイルとC#します。 これによりビルド処理や保存の C# コードを高度なハイブリッドをしないだけ、モデルからの読み取り専用のデータの表示がも HTML 内のユーザー入力を受け付けるとアプリを渡します。

#### <a name="rendering-the-template"></a>テンプレートの表示

呼び出す、`GenerateString`テンプレート レンダリングすぐに web ビューで表示できる HTML。 テンプレートは、モデルを使用する場合は、レンダリングの前に指定する必要があります。 この図は、レンダリングのしくみ-しない静的なリソースが指定された基本ディレクトリを使用して、指定されたファイルを検索する、実行時に web ビューによって解決されるを示しています。

 ![Razor のフローチャート](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>テンプレートから C# コードを呼び出す

Web ビューの URL を設定し、web ビューを再度読み込まなくても、ネイティブの要求を処理するためには、C# での要求をインターセプトし、C# にコールバック レンダリングされた web ビューからの通信が行われます。

RazorView のボタンの処理方法の例を確認できます。 ボタンには、次の HTML があります。

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` JavaScript 関数が HTML フォーム セットからすべての値を読み込み、 `location.href` web 表示。

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

これは、URL (例: カスタム スキーマを使用する web ビュー内を移動しよう `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

ネイティブの web ビューでは、このナビゲーション要求を処理するときインターセプトする機会があります。 Ios でこれは UIWebView の HandleShouldStartLoad イベントを処理することによって行われます。 Android では、私たちサブクラス化だけ、WebViewClient、フォームで使用し、ShouldOverrideUrlLoading をオーバーライドします。

これら 2 つのナビゲーション インターセプターの内部は、基本的に同じです。

最初に、web ビューを読み込むには、試行する URL を確認し、カスタムのスキームで開始しない場合 (`hybrid:`)、通常どおり、移動を発生を許可します。

カスタムの URL スキーム、スキーム間 URL 内のすべてのおよび"?" (この例では、"UpdateLabel") で処理するメソッドの名前です。 クエリ文字列内のすべてのメソッド呼び出しへのパラメーターとして扱われます。

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` このサンプルでは (前の「C# という」文字列に)、テキスト ボックスにパラメーターに最小限の文字列操作し、web ビューにコールバックします。

URL を処理が完了したら、メソッドは、web ビューがカスタムの URL に移動する [完了] を試行しませんようにナビゲーションを中止します。

#### <a name="manipulating-the-template-from-c"></a>C# からテンプレートを操作します。

レンダリングされた HTML web ビューへの通信C#は web ビューでの JavaScript の呼び出しによって行われます。 Ios では、呼び出すことによってこれは、 `EvaluateJavascript` UIWebView で。

```csharp
webView.EvaluateJavascript (js);
```

Android では、JavaScript で呼び出せる web ビューとして URL を使用して、JavaScript を読み込むことによって、 `"javascript:"` URL スキーム。

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>アプリを作成する真のハイブリッド

これらのテンプレートがようにしない各プラットフォームのネイティブ コントロールの使用 – 1 つの web ビューの画面全体が入力されます。

種類の項目を表示する web が最も得意とリッチ テキストおよびレスポンシブのレイアウトなど、HTML をプロトタイプ作成、すばらしいことができます。 Android でより適切 (iOS の UITableView) または ListView などのネイティブ UI コントロールを使用して実行しますなどされないすべてのタスクは、HTML および JavaScript – データの長い一覧をスクロールに適しています。

テンプレートの web ビューは、プラットフォーム固有のコントロール – 編集だけで簡単に拡張でき、 **MainStoryboard.storyboard** iOS デザイナーで、または**Resources/layout/Main.axml** Android で。

### <a name="razortodo-sample"></a>RazorTodo サンプル

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)リポジトリには、完全に HTML 駆動型アプリとネイティブ コントロールと HTML を組み合わせたアプリ間の差を表示する 2 つの個別のソリューションが含まれています。

-  **RazorTodo** -Razor テンプレートを使用して完全に HTML 駆動型アプリ。
-  **RazorNativeTodo** - iOS および Android 用のネイティブなリスト ビュー コントロールの使用が、HTML および Razor での編集 画面が表示されます。


これらの Xamarin アプリは、iOS と Android では、ポータブル クラス ライブラリ (Pcl) データベースとモデルのクラスなどの一般的なコードを共有するを使用しての両方で実行します。 Razor **.cshtml**プラットフォーム間で簡単に共有しているように、テンプレートを PCL に含めるもできます。

両方のサンプル アプリでは、Twitter で共有および Xamarin を使用したハイブリッド アプリケーションでもあるアクセス基になるすべての機能にテンプレート駆動の HTML Razor ビューからのデモ、ネイティブ プラットフォームからの音声合成 API を組み込みます。

**RazorTodo**アプリは、リストと編集ビューを HTML Razor テンプレートを使用します。 つまり、共有ポータブル クラス ライブラリでほぼ完全にアプリを構築できます (データベースを含むと **.cshtml** Razor テンプレート)。 次のスクリーン ショットでは、iOS および Android アプリを表示します。

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo**アプリは、編集ビューに、HTML Razor テンプレートを使用しますが、各プラットフォームでネイティブのスクロール リストを実装します。 これは、数などの利点を提供します。

-  パフォーマンス - ネイティブ スクロール コントロールは、高速で滑らかなのも非常に長いデータの一覧をスクロールすることを確認するのに仮想化を使用します。
-  ネイティブのエクスペリエンスでは、プラットフォーム固有の UI 要素を簡単に、iOS と Android で高速スクロール インデックスのサポートなどを有効にします。


Xamarin を使用したハイブリッド アプリのビルドの主な利点は、(最初のサンプル) のような完全に HTML 主導のユーザー インターフェイスを起動して (2 つ目のサンプルでは) として、必要に応じてプラットフォーム固有の機能を追加しことができます。 ネイティブ リスト画面および HTML Razor は、両方の iOS の画面を編集し、Android を以下に示します。

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>まとめ

この記事では iOS と Android のビルドを容易にするに使用できる web ビュー コントロールの機能について説明しましたハイブリッド アプリケーション。

また、Razor テンプレート エンジンと構文を使用して Xamarin アプリで簡単に HTML を生成するために使用できる、について説明します。**cshtml** Razor テンプレート ファイル。 Visual Studio は、Mac すばやくソリューション テンプレートは Xamarin を使用したハイブリッド アプリケーションの構築を開始するのもについて説明します。

最後に、ネイティブ ユーザー インターフェイスと API と web ビューを結合する方法を示す RazorTodo サンプルが導入されました。

### <a name="related-links"></a>関連リンク

- [RazorTodo サンプル](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 - Razor ビュー エンジン (マイクロソフト)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor 構文 (Microsoft) を使用して ASP.NET Web プログラミングの概要](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
