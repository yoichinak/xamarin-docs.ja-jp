---
title: "Razor テンプレートを使用して構築 HTML ビュー"
description: " 全画面表示の web ページを使用して HTML を表示するためには、web サイト プロジェクトから、HTML、Javascript と CSS が既にある場合は特に、クロスプラット フォームの方法で複雑な書式設定を表示するために単純で効果的な方法です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 5c69b8e71cac5d9f0385728ca75a5f311cb24fc0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="building-html-views-using-razor-templates"></a>Razor テンプレートを使用して構築 HTML ビュー

モバイル開発の世界で用語"ハイブリッド app"は、通常、ホストされる web ビューアー コントロールの内部 HTML ページとしてその画面の一部 (またはすべて) に表示するアプリケーションを指します。

使用する一部の開発環境がある、プラットフォームにも制限のそれらのアプリの複雑な処理や UI 効果を実現するしようとしているときにパフォーマンスの問題から低下することができ、HTML および Javascript で完全には、次のモバイル アプリをビルドアクセスする機能です。

Xamarin には、HTML の Razor テンプレート エンジンを利用するときに特にの両方の長所が提供しています。 Xamarin には、Javascript と CSS を使用してを持っていても、基になるプラットフォーム Api と c# を使用して高速処理に完全にアクセスするクロスプラット フォーム テンプレート化された HTML ビューを構築する柔軟性があります。

このドキュメントでは、Razor テンプレート エンジンには、Xamarin を使用してモバイル プラットフォーム間で使用できる HTML と Javascript + CSS ビューを構築する方法について説明します。

## <a name="using-web-views-programmatically"></a>プログラムによる Web ビューの使用

Razor について学習する前にこのセクションでは、アプリ内で生成される HTML コンテンツ具体的には直接 – HTML コンテンツを表示する web ビューを使用する方法について説明します。

Xamarin を簡単に作成し、c# を使用して HTML を表示するために iOS と Android の両方で、基になるプラットフォーム Api への完全なアクセスを提供します。 各プラットフォームの基本構文は、以下に示します。

### <a name="ios"></a>iOS

Xamarin.iOS で UIWebView コントロールの HTML を表示すると、はわずか数行のコードも受け取ります。

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

参照してください、 [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/)レシピ UIWebView コントロールの使用の詳細についてはします。

### <a name="android"></a>Android

Xamarin.Android を使用して、WebView コントロールでの HTML の表示は、少数の行のコードで行われます。

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

参照してください、 [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/)レシピ WebView コントロールの使用の詳細についてはします。

### <a name="specifying-the-base-directory"></a>基本のディレクトリを指定します。

両方のプラットフォームには、HTML ページの基本ディレクトリを指定するパラメーターがあります。 これは、画像や CSS ファイルなどのリソースへの相対参照を解決するのには使用されているデバイスのファイル システム上の場所です。 たとえば、タグなど

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

これらのファイルを参照してください: **style.css**、 **monkey.jpg**と**jscript.js**です。 基本ディレクトリの設定は、これらのファイルが配置されているページに読み込まれている可能性があるため、web ビューを指示します。

#### <a name="ios"></a>iOS

テンプレートの出力は次の c# コードで、iOS でレンダリングされます。

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

ベース ディレクトリとして指定`NSBundle.MainBundle.BundleUrl`にアプリケーションがインストールされているディレクトリを指します。 内のすべてのファイル、**リソース**フォルダーがなどのこの場所にコピーされて、 **style.css**次に示すファイル。

 ![iPhoneHybrid ソリューション](images/image1_240x163.png)

すべての静的なコンテンツ ファイルのビルド アクションがする必要があります**BundleResource**:

 ![iOS プロジェクトのビルド アクション: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android では、html 文字列が、web ビューに表示されるときに、パラメーターとして渡されるベース ディレクトリも必要です。

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

特殊な文字列**file:///android_asset/**アプリで、表示は、ここを格納している Android Assets フォルダーを指す、 **style.css**ファイル。

 ![AndroidHybrid ソリューション](images/image3_240x167.png)

すべての静的なコンテンツ ファイルのビルド アクションがする必要があります**AndroidAsset**です。

 ![Android プロジェクトのビルド アクション: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>HTML および Javascript からの c# の呼び出し

Web ビューに html ページが読み込まれると、サーバーからページが読み込まれた場合と同様のリンクとフォームに扱います。 つまり、ユーザーがリンクをクリックするか、フォームを送信する場合、web ビューを試行する、指定したターゲットに移動します。

場合は (google.com) などの外部のサーバーへのリンクですし、web ビューを試みます (インターネット接続があると仮定した場合)、外部の web サイトを読み込みます。

```html
<a href="http://google.com/">Google</a>
```

リンクの相対的な場合、web ビューしようと、ベース ディレクトリからそのコンテンツを読み込みます。 明らかにネットワーク接続は必要ありませんこれを行う、デバイス上でのアプリに内容が保存されています。

```html
<a href="somepage.html">Local content</a>
```

フォーム アクションでは、同じルールに従います。

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

クライアント上で web サーバーをホストしません。ただし、HTTP GET を経由でサービスを呼び出す今日レスポンシブ デザイン パターンで採用されているサーバーと同じ通信方法を使用して Javascript を生成することによって、応答を非同期的に処理 (または呼び出し側の Javascript は、web ビューで既にホストされている)。 これにより、簡単に処理し、表示する HTML ページ上のバックアップを結果の c# コードに HTML からデータを渡すことができます。

IOS および Android の両方は、アプリケーション コードをアプリのコードが (必要な場合) に応答できるように、これらのナビゲーション イベントをインターセプトするためのメカニズムを提供します。 この機能は、ネイティブ コードの web ビューとの対話をことができるため、ハイブリッド アプリを構築するために重要です。

#### <a name="ios"></a>iOS

IOS の web ビューを ShouldStartLoad イベントをオーバーライドすると、(リンクをクリックします) などのナビゲーション要求を処理するアプリケーション コードを許可できます。 メソッドのパラメーターが、すべての情報を指定します。

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
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

web ビューで、クライアントを設定します。

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>C# から呼び出す Javascript

新しい HTML ページを読み込む web ビューを示すだけでなく、c# コードは、現在表示されているページ内で Javascript も実行できます。 全体の Javascript コード ブロックを c# の文字列を使用して作成し、実行、または javascript を使用してページで既に使用可能なメソッド呼び出しを作成できます`script`タグ。

#### <a name="android"></a>Android

実行して、前にプレフィックスを Javascript コードを作成する"javascript:"し、web ビューをその文字列を読み込むように指示します。

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS の web ビューには、明示的に呼び出すメソッドを Javascript が提供します。

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>まとめ

このセクションでは、Android と Xamarin を使用してハイブリッド アプリケーションを構築することのできる iOS の両方の web ビュー コントロールの機能は加えられてなど。

-  コードでは、生成された文字列から HTML をロードする機能
-  ローカル ファイル (CSS、Javascript、イメージまたはその他の HTML ファイル) を参照する機能
-  C# のコードでナビゲーション要求をインターセプトする機能
-  Javascript コードからを呼び出す c# 権限です。


次のセクションでは、ハイブリッド アプリで使用する HTML を作成しやすくなる Razor について説明します。

## <a name="what-is-razor"></a>Razor とは何ですか。

Razor は、サーバー上で実行し、web ブラウザーに提供するための HTML を生成する最初の ASP.NET MVC で導入されたテンプレート エンジンです。

Razor テンプレート エンジンは、レイアウトを表現して CSS スタイル シートおよび Javascript を簡単に組み込むように c# を使用して標準の HTML 構文を拡張します。 テンプレートには、任意のカスタム型があり、テンプレートから直接アクセスできるプロパティを持つモデルのクラスを参照できます。 その主な利点の 1 つは、HTML および c# の構文を簡単に混在させる機能です。

Razor テンプレートはサーバー側で使用するだけではありません、Xamarin アプリでそれらに含めることもできます。 Web ビューをプログラムで使用する機能と共に Razor テンプレートを使用すると、Xamarin を構築する高度なクロスプラット フォームのハイブリッド アプリケーションができます。

### <a name="razor-template-basics"></a>Razor テンプレートの基礎

Razor テンプレート ファイルが、 **.cshtml**ファイル拡張子。 テキスト テンプレート セクションで、Xamarin のプロジェクトに追加することができます、**新しいファイル** ダイアログ。

 ![新しいファイルで Razor テンプレート](images/image5_400x201.png)

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

-  `@`シンボルは、Razor テンプレートで特別な意味を持つ: 次の式を c# に評価されることを示します。
- `@model` ディレクティブは、Razor テンプレート ファイルの最初の行として常に表示されます。
-  `@model`型ディレクティブの後にする必要があります。 テンプレートにこの例では、単純な文字列が渡されるが、任意のカスタム クラスが考えられます。
-  ときに`@Model`が参照されているテンプレートを全体にわたって (この例では、これは、文字列になります) での生成時に、テンプレートに渡されるオブジェクトへの参照を提供します。
-  IDE が自動的にテンプレートの部分クラスを生成 (ファイルが、 **.cshtml**拡張子) です。 このコードを表示できますが、編集しないでください。
 ![RazorView.cshtml](images/image6_125x34.png)部分クラスは .cshtml テンプレートのファイル名に合わせて RazorView をという名前です。 この名前の c# コードでテンプレートを参照するために使用することをお勧めします。
- `@using` ステートメントは、Razor テンプレートに含める追加の名前空間の上部に含めることもできます。


HTML 出力の最終的なは、次の c# コードを生成できます。 文字列"Hello World"が表示されるテンプレートの出力に組み込まれますモデルを指定することに注意してください。

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

IOS シミュレーターと Android エミュレーターでの web ビューに表示される出力を次に示します。

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Razor 構文の詳細

このセクションでは作業を開始するためのいくつかの基本的な Razor 構文を紹介することでは、これを使用します。 このセクションの例では、データでは、次のクラスを作成し、Razor を使用して表示します。

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

すべての例は、次のデータの初期化コードを使用します

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>モデルのプロパティを表示します。

モデルがプロパティを持つクラスである場合は、簡単にで参照される、Razor テンプレートこのテンプレートの例に示すように。

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

最終的な出力が、iOS シミュレーターと Android エミュレーター上の web ビューのとおりです。

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>(C#) ステートメント

複雑な C# の場合、モデル プロパティの更新プログラムとこの例では年齢の計算など、テンプレートに含めることができます。

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

使用してコードを囲むことで (年齢を書式設定) などの複雑な単一行 c# 式を記述する`@()`です。

囲んで、複数の (C#) ステートメントを記述できます`@{}`です。

#### <a name="if-else-statements"></a>If-else ステートメント

コード分岐を表すことができます`@if`このテンプレートの例に示すようにします。

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

ループ構造と同様に`foreach`も追加できます。 `@`ループ変数のプレフィックスを使用できます (`@food`ここでは) を HTML でレンダリングします。

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

IOS シミュレーターおよび Android エミュレーターで実行されているこのテンプレートの出力が表示されます。

 ![Rupert X サル](images/image9_520x277.png)

このセクションでは、Razor テンプレートを使用した単純な読み取り専用ビューをレンダリングの基本について説明しました。 次のセクションでは、ユーザー入力をそのまま使用したり、HTML ビューと c# での Javascript の間で相互運用できる Razor を使用するより完全なアプリをビルドする方法について説明します。

## <a name="using-razor-templates-with-xamarin"></a>Xamarin で Razor テンプレートの使用

このセクションの内容がソリューション テンプレートを Visual studio for mac を使用して、独自のハイブリッド アプリケーションをビルドに使用する方法について説明します 次の 3 つのテンプレートがから利用可能な**ファイル > 新規 > ソリューションしています.**ウィンドウ。

- **Android > アプリ > Android WebView アプリケーション**
- **iOS > アプリ > WebView アプリケーション**
- **ASP.NET MVC Project**



**新しいソリューション**ウィンドウが iPhone、Android プロジェクトの次のよう - 右側のソリューションの説明には、Razor テンプレート エンジンのサポートが強調表示されます。

 ![IPhone、Android のソリューションを作成します。](images/image13_1139x959.png)

簡単に追加することに注意してください、 **.cshtml** Razor テンプレート*任意*既存の Xamarin プロジェクト必要はありませんこれらのソリューション テンプレートを使用します。 iOS のプロジェクトには、いずれも; Razor を使用するストーリー ボードは不要します。任意のビューに UIWebView コントロールをプログラムで単に追加し、c# コードで全体 Razor テンプレートを表示することができます。

IPhone、Android プロジェクトの既定テンプレート ソリューションの内容を次に示します。

 ![iPhone、Android のテンプレート](images/image10_428x310.png)

テンプレートでは、データ モデル オブジェクトに Razor テンプレートを読み込み、ユーザー入力を処理および Javascript を使用して、ユーザーへの通信の準備完了の go アプリケーション インフラストラクチャを提供します。

ソリューションの重要な部分は次のとおりです。

-  などの静的なコンテンツ、 **style.css**ファイル。
-  Razor .cshtml テンプレート ファイルと同様に**RazorView.cshtml**です。
-  モデルなど、Razor テンプレートで参照されているクラス**ExampleModel.cs**です。
-  Web ビューを作成し、ように、テンプレートをレンダリングするプラットフォームに固有のクラス、 `MainActivity` Android で、 `iPhoneHybridViewController` iOS でします。


次のセクションでは、プロジェクトの作業について説明します。

### <a name="static-content"></a>静的コンテンツ

静的なコンテンツには、CSS スタイル シート、画像、Javascript ファイルまたはからリンクまたは web ビューに表示されている HTML ファイルで参照できるその他のコンテンツが含まれています。

プロジェクト テンプレートには、ハイブリッド アプリの静的なコンテンツを追加する方法を説明する最小限のスタイル シートが含まれます。 CSS スタイル シートが次のように、テンプレート内で参照します。

```html
<link rel="stylesheet" href="style.css" />
```

どのようなスタイル シートや JQuery などのフレームワークなど、必要な Javascript ファイルを追加することができます。

### <a name="razor-cshtml-templates"></a>Razor cshtml テンプレート

テンプレートには、Razor **.cshtml** HTML または Javascript と c# の間でデータを伝達するためのコードが事前に記述されたファイルです。 これによりできますしないだけモデルからの読み取り専用のデータを表示、HTML でのユーザー入力を受け付けるやも渡すことを高度なハイブリッド アプリのバックアップ処理または記憶域の c# コードをビルドします。

#### <a name="rendering-the-template"></a>テンプレートの表示

呼び出す、`GenerateString`テンプレートに HTML の表示の web ビューに表示する準備ができています。 テンプレートは、モデルを使用している場合は、レンダリングの前に指定する必要があります。 この図は、レンダリングの動作方法: いない静的なリソースは、指定したベース ディレクトリを使用して、指定されたファイルを検索する、実行時に、web ビューによって解決されるを示しています。

 ![Razor のフローチャート](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>テンプレートから c# コードの呼び出し

コールバックする C# の場合、レンダリングされた web ビューからの通信は、web ビューの URL を設定し、web ビューを再読み込みすることがなく、ネイティブの要求を処理する C# の場合、要求を受信して行われます。

RazorView のボタンの処理方法の例を確認できます。 次の HTML にこのボタンには。

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Javascript 関数では、すべての値を読み取り、HTML フォームとセットから、 `location.href` web ビュー。

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

これは、カスタム スキーム (を使用した URL を web ビュー内を移動しよう `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

ネイティブ web ビューでは、このナビゲーション要求を処理するとき、インターセプトする機会があります。 Ios では、これは UIWebView の HandleShouldStartLoad イベントを処理することによって行われます。 Android でおサブクラス化だけで、WebViewClient、フォームで使用し、ShouldOverrideUrlLoading をオーバーライドします。

これら 2 つのナビゲーション インターセプターの内部は、基本的に同じです。

最初に、読み込むには、web ビューが開こうとしている URL を確認し、カスタム スキームで始まらない場合 (`hybrid:`)、発生する可能性への移動を通常どおりに許可します。

スキームの間の URL 内のすべてのカスタムの URL スキームと"?" (この場合は"UpdateLabel") で処理するメソッドの名前です。 クエリ文字列内のすべてのメソッド呼び出しへのパラメーターとして扱われます。

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` このサンプルでは、最小限の文字列操作 (「c# という」文字列を付加すること、) ボックスにパラメーターとし、web ビューに返信します。

URL を処理するには、後に、メソッドは、web ビューがカスタム URL への移動を完了しないようにナビゲーションを中止します。

#### <a name="manipulating-the-template-from-c"></a>C# からテンプレートを操作します。

C# からレンダリングされた HTML web ビューへの通信は、web ビューに Javascript を呼び出すことによって行われます。 Ios、呼び出すことによってこれは、 `EvaluateJavascript` UIWebView で。

```csharp
webView.EvaluateJavascript (js);
```

Android で Javascript によって呼び出される web ビューとして URL を使用して、Javascript を読み込み、 `"javascript:"` URL スキーム。

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>アプリを行う完全にハイブリッド

これらのテンプレートを行わない各プラットフォームでネイティブ コントロールの使用: 全画面は、1 つの web ビューで塗りつぶされます。

物の種類を表示する web は、リッチ テキストと応答性のレイアウトなどに最適な HTML のプロトタイプを作成、優れたことができます。 向上 (iOS で UITableView) または ListView などのネイティブ UI コントロールを使用して Android でを実行など、されないすべてのタスクは、HTML および Javascript – データの長い一覧をスクロールに適しています。

テンプレートの web ビューは、プラットフォーム固有のコントロール – 編集するだけで簡単に拡張できます、 **MainStoryboard.storyboard** iOS デザイナーで、または**Resources/layout/Main.axml** Android でします。

### <a name="razortodo-sample"></a>RazorTodo サンプル

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)リポジトリには、完全に HTML ドリブンのアプリと HTML をネイティブ コントロールと結合するアプリの違いを表示する 2 つの独立したソリューションが含まれています。

-  **RazorTodo** -Razor テンプレートを使用してアプリケーションを完全に HTML 駆動します。
-  **RazorNativeTodo** - iOS および Android のネイティブ リスト ビュー コントロールを使用しますが、HTML および Razor での編集 画面が表示されます。


これらの Xamarin アプリは、iOS と Android、ポータブル クラス ライブラリ (Pcl) データベースとモデルのクラスなどの一般的なコードを共有するを利用しての両方で実行します。 Razor **.cshtml**プラットフォーム間で簡単に共有しているために、テンプレートを PCL に含めるもできます。

両方のサンプル アプリは、Twitter の共有および Xamarin を使用したハイブリッド アプリケーションもあるアクセス基になるすべての機能に HTML Razor テンプレートに基づくビューからのデモンストレーション、ネイティブ プラットフォームからの音声合成 Api を組み込みます。

**RazorTodo**アプリは、リストや編集のビューの HTML Razor テンプレートを使用します。 つまり、共有ポータブル クラス ライブラリでほぼ完全にアプリを構築できます (データベースを含むと**.cshtml** Razor テンプレート)。 次のスクリーン ショットは、iOS と Android アプリを表示します。

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo**アプリは、編集ビューを HTML Razor テンプレートを使用しますが、各プラットフォームでネイティブのスクロール リストを実装します。 これには数などの利点があります。

-  [パフォーマンス] - ネイティブ スクロール コントロール virtualization を使用して非常に長いデータの一覧を使用しても高速でスムーズ スクロールを確認してください。
-  ネイティブ エクスペリエンス - プラットフォーム固有の UI 要素を簡単に高速スクロール インデックスでサポートされる iOS や Android など、有効にします。


Xamarin のハイブリッド アプリを構築する主な利点は、(最初の例では) のように完全に HTML によるユーザー インターフェイスから始めておよび (2 つ目のサンプルでは) として、必要に応じてプラットフォーム固有の機能を追加できます。 ネイティブなリスト画面および HTML Razor が両方の iOS の画面を編集し、Android を以下に示します。

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>まとめ

この記事の内容が iOS および Android を構築を容易にする上の web ビュー コントロールの機能を説明したハイブリッド アプリケーションです。

また、Razor テンプレート エンジンと構文を使用した Xamarin アプリで HTML を簡単に生成するために使用する、について説明します。**cshtml** Razor テンプレート ファイル。 すばやくソリューション テンプレート Xamarin を使用したハイブリッド アプリケーションの構築を開始 Mac 用の Visual Studio も説明します。

最後のネイティブ ユーザー インターフェイスおよび Api の web ビューを結合する方法を示す RazorTodo サンプルが導入されました。

### <a name="related-links"></a>関連リンク

- [RazorTodo サンプル](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 の Razor ビュー エンジン (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor 構文 (Microsoft) を使用して ASP.NET Web プログラミングの概要](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)