---
title: Razor テンプレートを使用した HTML ビューの作成
description: " 全画面の web ページを使用して HTML をレンダリングすることは、複雑な書式をクロスプラットフォーム方式でレンダリングするための簡単で効果的な方法です。特に、HTML、JavaScript、および CSS が web サイトプロジェクトにある場合は特にそうです。"
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: ccad60f749732ae2d0bf8e9852859b13af3a629e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284921"
---
# <a name="building-html-views-using-razor-templates"></a>Razor テンプレートを使用した HTML ビューの作成

モバイル開発環境では、"ハイブリッドアプリ" という用語は、通常、ホストされている web ビューアーコントロール内のすべての画面を HTML ページとして表示するアプリケーションを指します。

モバイルアプリを完全に HTML および JavaScript でビルドできるようにする開発環境がいくつかありますが、これらのアプリは、複雑な処理や UI 効果を実現しようとしたときにパフォーマンスの問題が発生する可能性があり、プラットフォームでも制限されています。アクセスできる機能。

Xamarin は、特に Razor HTML テンプレートエンジンを利用する場合に、両方の長所を発揮します。 Xamarin では、JavaScript と CSS を使用するクロスプラットフォームのテンプレート化された HTML ビューを構築する柔軟性があります。また、を使用してC#、基になるプラットフォーム api と高速処理に完全にアクセスすることもできます。

このドキュメントでは、Razor テンプレートエンジンを使用して、Xamarin を使用してモバイルプラットフォーム間で使用できる HTML + JavaScript + CSS ビューを構築する方法について説明します。

## <a name="using-web-views-programmatically"></a>プログラムによる Web ビューの使用

Razor について学習する前に、このセクションでは、web ビューを使用して HTML コンテンツを直接表示する方法 (特に、アプリ内で生成される HTML コンテンツ) について説明します。

簡単に作成し、C# を使用して HTML を表示、Xamarin は iOS と Android の両方では、基になるプラットフォーム API に完全にアクセスを提供します。 各プラットフォームの基本構文を次に示します。

### <a name="ios"></a>iOS

Xamarin の UIWebView コントロールに HTML を表示すると、次の数行のコードが必要になります。

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

UIWebView コントロールの使用方法の詳細については、「 [IOS uiwebview](http://docs.xamarin.com/recipes/ios/content_controls/web_view/)レシピ」を参照してください。

### <a name="android"></a>Android

Xamarin を使用して WebView コントロールに HTML を表示するには、数行のコードを実行します。

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable JavaScript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

WebView コントロールの使用方法の詳細については、「 [Android webview](http://docs.xamarin.com/recipes/android/controls/webview/)レシピ」を参照してください。

### <a name="specifying-the-base-directory"></a>基本ディレクトリの指定

両方のプラットフォームには、HTML ページの基本ディレクトリを指定するパラメーターがあります。 これは、イメージや CSS ファイルなどのリソースへの相対参照を解決するために使用される、デバイスのファイルシステム上の場所です。 たとえば、次のようなタグがあるとします。

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

次のファイルを参照してください。 **.css**、**サル .jpg** 、および**jscript .js**。 基本ディレクトリの設定により、これらのファイルをページに読み込むことができるように、これらのファイルが配置されている web ビューが示されます。

#### <a name="ios"></a>iOS

テンプレートの出力は次の C# コードでの iOS でレンダリングされます。

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

ベースディレクトリは、アプリケーションが`NSBundle.MainBundle.BundleUrl`インストールされているディレクトリを参照するとして指定されます。 **Resources**フォルダー内のすべてのファイルは、次に示す**スタイルの .css**ファイルなど、この場所にコピーされます。

 ![I、ハイブリッドソリューション](images/image1_240x163.png)

すべての静的コンテンツファイルのビルドアクションは、 **BundleResource**である必要があります。

 ![iOS プロジェクトのビルドアクション:BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android では、html 文字列が web ビューに表示されるときに、パラメーターとして基本ディレクトリを渡す必要もあります。

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

特別な文字列**file:///android_asset/** は、アプリ内の android Assets フォルダーを参照します。ここには、**スタイルの .css**ファイルが含まれています。

 ![AndroidHybrid ソリューション](images/image3_240x167.png)

すべての静的コンテンツファイルのビルドアクションは、 **Androidasset**である必要があります。

 ![Android プロジェクトのビルドアクション:AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>HTML C#および JavaScript からの呼び出し

Html ページが web ビューに読み込まれると、ページがサーバーから読み込まれた場合と同じように、リンクとフォームが処理されます。 つまり、ユーザーがリンクをクリックするかフォームを送信すると、web ビューは、指定されたターゲットへの移動を試みます。

リンクが外部サーバー (google.com など) へのリンクである場合、web ビューは外部 web サイトの読み込みを試みます (インターネット接続があることを前提とします)。

```html
<a href="http://google.com/">Google</a>
```

リンクが相対パスの場合、web ビューはそのコンテンツをベースディレクトリから読み込もうとします。 デバイス上のアプリにコンテンツが格納されているため、これが機能するためにネットワーク接続は必要ありません。

```html
<a href="somepage.html">Local content</a>
```

フォームアクションは同じルールに従います。

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

クライアントで web サーバーをホストすることはありません。ただし、現在の応答性の高いデザインパターンで採用されているのと同じサーバー通信手法を使用して、HTTP GET でサービスを呼び出したり、JavaScript (または web ビューで既にホストされている JavaScript を呼び出すことによって) 応答を非同期的に処理したりすることができます。 これにより、簡単に処理し、表示、HTML ページ上の結果をバックアップ用の C# コードに戻りましょう、HTML からデータを渡すことができます。

IOS と Android はどちらも、アプリケーションコードがこれらのナビゲーションイベントをインターセプトして、アプリコードが応答できるようにするためのメカニズムを提供します (必要な場合)。 この機能は、ネイティブコードが web ビューと対話できるようにするため、ハイブリッドアプリを構築する上で重要です。

#### <a name="ios"></a>iOS

IOS の web ビューの "指定しない" イベントを上書きして、アプリケーションコードがナビゲーション要求 (リンククリックなど) を処理できるようにすることができます。 メソッドパラメーターは、すべての情報を提供します。

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

次に、イベントハンドラーを割り当てます。

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android では、WebViewClient を単にサブクラス化してから、ナビゲーション要求に応答するコードを実装します。

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

次に、web ビューでクライアントを設定します。

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>呼び出し (C から JavaScript を)\#

新しい HTML ページを読み込むように web ビューに指示するだけでC#なく、コードは現在表示されているページ内で JavaScript を実行することもできます。 Javascript コードブロック全体は、文字列をC#使用して作成し、実行できます。また、ページで既に使用可能`script`な javascript のメソッド呼び出しをタグ経由で作成することもできます。

#### <a name="android"></a>Android

実行する JavaScript コードを作成し、"javascript:" でプレフィックスを付け、その文字列を読み込むように web ビューに指示します。

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS web ビューでは、JavaScript を呼び出すためのメソッドが提供されるようになります。

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Summary

このセクションでは、Android と iOS の両方で、次のようなハイブリッドアプリケーションを構築するための web ビューコントロールの機能について紹介しました。

- コードで生成された文字列から HTML を読み込む機能
- ローカルファイル (CSS、JavaScript、イメージ、またはその他の HTML ファイル) を参照する機能
- コード内のC#ナビゲーション要求をインターセプトする機能
- コードからC# JavaScript を呼び出すことができる。


次のセクションでは、ハイブリッドアプリで使用する HTML を簡単に作成できる Razor について説明します。

## <a name="what-is-razor"></a>Razor とは

Razor は、ASP.NET MVC で導入されたテンプレートエンジンであり、もともとサーバー上で実行し、web ブラウザーに提供する HTML を生成します。

Razor テンプレートエンジンは、レイアウトを表現しC# 、CSS スタイルシートと JavaScript を簡単に組み込むことができるように、標準の HTML 構文をで拡張します。 このテンプレートは、モデルクラスを参照できます。これは任意のカスタム型で、そのプロパティにはテンプレートから直接アクセスできます。 その主な利点の 1 つは、簡単に HTML と C# の構文を混在させる機能です。

Razor テンプレートはサーバー側での使用に限定されておらず、Xamarin アプリに含めることもできます。 Razor テンプレートを使用すると、プログラムによって web ビューを操作できるようになり、Xamarin を使用して高度なクロスプラットフォームハイブリッドアプリケーションを構築できます。

### <a name="razor-template-basics"></a>Razor テンプレートの基礎

Razor テンプレートファイルの拡張子は、 **cshtml**です。 これらは、**新しいファイル** ダイアログの テキストテンプレート セクションから Xamarin プロジェクトに追加できます。

 ![新しいファイル-Razor テンプレート](images/image5_400x201.png)

単純な Razor テンプレート ( **RazorView**) を次に示します。

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

通常の HTML ファイルとは次の点に注意してください。

- `@`シンボルは、Razor テンプレートで特別な意味を持つ – こと、次の式は、C# に評価されることを示します。
- `@model`ディレクティブは常に Razor テンプレートファイルの最初の行として表示されます。
- ディレクティブ`@model`の後に型を指定する必要があります。 この例では、単純な文字列がテンプレートに渡されていますが、これは任意のカスタムクラスである可能性があります。
- テンプレート`@Model`全体でが参照されている場合、テンプレートの生成時にテンプレートに渡されたオブジェクトへの参照が提供されます (この例では文字列になります)。
- IDE によって、テンプレートの部分クラスが自動的に生成されます (ファイル拡張子は**cshtml** )。 このコードを表示することはできますが、編集することはできません。
 ![RazorView 部分クラスは、cshtml テンプレートファイル名と一致するように RazorView という名前が付けられています。](images/image6_125x34.png) この名前は、コードでC#テンプレートを参照するために使用されます。
- `@using`ステートメントを Razor テンプレートの先頭に追加して、追加の名前空間を含めることもできます。


最終的な HTML 出力は、次の C# コードを生成できます。 モデルは、表示されるテンプレートの出力に組み込まれる文字列 "Hello World" として指定されていることに注意してください。

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

IOS シミュレーターの web ビューに表示される出力と Android Emulator を次に示します。

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>その他の Razor 構文

このセクションでは、使用を開始する際に役立つ基本的な Razor 構文を紹介します。 このセクションの例では、次のクラスにデータを設定し、Razor を使用して表示します。

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

すべての例では、次のデータ初期化コードを使用します。

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>モデルのプロパティの表示

モデルがプロパティを持つクラスである場合は、次のテンプレートの例に示すように、Razor テンプレートで簡単に参照できます。

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

これは、次のコードを使用して文字列にレンダリングできます。

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

最終的な出力は、iOS シミュレーターの web ビューと Android Emulator に表示されます。

 ![、Pert](images/image8_516x160.png)

#### <a name="c-statements"></a>C#命令

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

このテンプレートの例に示す`@if`ように、コード分岐はで表すことができます。

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

のような`foreach`ループ構造を追加することもできます。 このプレフィックスは、ループ変数 ( `@food`この場合は) で使用して、HTML 形式で表示できます。 `@`

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

上記のテンプレートの出力は、iOS シミュレーターおよび Android Emulator で実行されています。

 ![サル (pert)](images/image9_520x277.png)

このセクションでは、Razor テンプレートを使用して単純な読み取り専用ビューを表示する方法の基本について説明しました。 次のセクションでは、Razor を使用してより完全なアプリを構築する方法について説明します。これC#は、HTML ビューとの JavaScript 間でユーザー入力を受け入れ、相互運用することができます。

## <a name="using-razor-templates-with-xamarin"></a>Xamarin での Razor テンプレートの使用

このセクションでは、Visual Studio for Mac のソリューションテンプレートを使用して独自のハイブリッドアプリケーションを構築する方法について説明します。 **[新しい > ソリューションの >]** ウィンドウでは、次の3つのテンプレートを使用できます。

- **Android > アプリ > Android WebView アプリケーション**
- **iOS > アプリ > WebView アプリケーション**
- **ASP.NET MVC プロジェクト**



**新しいソリューション**ウィンドウは、IPhone と Android プロジェクトの場合は次のようになります。右側のソリューションの説明では、Razor テンプレートエンジンのサポートが強調表示されています。

 ![IPhone と Android ソリューションの作成](images/image13_1139x959.png)

既存の Xamarin プロジェクトには簡単に**cshtml** Razor テンプレートを追加できることに注意してください。これらのソリューション*テンプレートを使用*する必要はありません。 iOS プロジェクトでは、Razor を使用するためのストーリーボードは必要ありません。プログラムによって任意のビューに UIWebView コントロールを追加するだけで、コードC#内で Razor テンプレート全体をレンダリングできます。

IPhone と Android プロジェクトの既定のテンプレートソリューションコンテンツを次に示します。

 ![iPhone と Android のテンプレート](images/image10_428x310.png)

テンプレートを使用すると、すぐに使用できるアプリケーションインフラストラクチャを利用して、データモデルオブジェクトを含む Razor テンプレートを読み込み、ユーザー入力を処理して、JavaScript を介してユーザーに返信できます。

ソリューションの重要な部分は次のとおりです。

- **スタイルの .css**ファイルなどの静的コンテンツ。
- **RazorView**のような Razor テンプレートファイル。
- **ExampleModel.cs**などの Razor テンプレートで参照されるモデルクラス。
- Web ビューを作成してテンプレートをレンダリングするプラットフォーム固有のクラス。たとえば、 `MainActivity` Android `iPhoneHybridViewController`の場合は、iOS の場合はです。


次のセクションでは、プロジェクトの動作について説明します。

### <a name="static-content"></a>静的コンテンツ

静的コンテンツには、web ビューに表示されている HTML ファイルからリンクしたり、参照したりできる CSS スタイルシート、画像、JavaScript ファイル、その他のコンテンツが含まれます。

テンプレートプロジェクトには、ハイブリッドアプリに静的コンテンツを含める方法を示すための最小限のスタイルシートが含まれています。 次のように、テンプレートで CSS スタイルシートが参照されます。

```html
<link rel="stylesheet" href="style.css" />
```

JQuery などのフレームワークを含む、必要な任意のスタイルシートと JavaScript ファイルを追加できます。

### <a name="razor-cshtml-templates"></a>Razor cshtml テンプレート

このテンプレートには、HTML/JavaScript とC#の間でデータをやり取りするための事前に記述されたコードを含む Razor ファイルが含まれています。 これによりビルド処理や保存の C# コードを高度なハイブリッドをしないだけ、モデルからの読み取り専用のデータの表示がも HTML 内のユーザー入力を受け付けるとアプリを渡します。

#### <a name="rendering-the-template"></a>テンプレートを表示する

テンプレートで`GenerateString`を呼び出すと、web ビューで表示できる HTML がレンダリングされます。 テンプレートでモデルを使用する場合は、レンダリングの前に指定する必要があります。 この図は、レンダリングの動作を示しています。静的なリソースは実行時に web ビューによって解決され、指定されたファイルを検索するために指定されたベースディレクトリを使用します。

 ![Razor フローチャート](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>テンプレートから C# コードを呼び出す

Web ビューの URL を設定し、web ビューを再度読み込まなくても、ネイティブの要求を処理するためには、C# での要求をインターセプトし、C# にコールバック レンダリングされた web ビューからの通信が行われます。

例については、「RazorView のボタンの処理方法」を参照してください。 このボタンには、次の HTML があります。

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

JavaScript `InvokeCSharpWithFormValues`関数は、HTML フォームからすべての値を読み取り、web ビュー `location.href`のを設定します。

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

これにより、web ビューがカスタムスキームの URL に移動しようとします ( `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

ネイティブ web ビューがこのナビゲーション要求を処理するときは、それをインターセプトすることができます。 IOS では、これは UIWebView の HandleShouldStartLoad イベントを処理することによって行われます。 Android では、フォームで使用される WebViewClient を単にサブクラス化し、ShouldOverrideUrlLoading をオーバーライドします。

これら2つのナビゲーションインターセプターの内部構造は、基本的に同じです。

まず、web ビューが読み込もうとしている URL を確認し、カスタムスキーム (`hybrid:`) で開始されていない場合は、ナビゲーションを通常どおりに実行できるようにします。

カスタム URL スキームの場合、スキームと "?" の間の URL 内のすべてのものが含まれます。 処理するメソッド名を指定します (この例では "UpdateLabel")。 クエリ文字列内のすべてのものは、メソッド呼び出しのパラメーターとして扱われます。

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` このサンプルでは (前の「C# という」文字列に)、テキスト ボックスにパラメーターに最小限の文字列操作し、web ビューにコールバックします。

URL の処理後、メソッドはナビゲーションを中止して、web ビューがカスタム URL への移動を完了しないようにします。

#### <a name="manipulating-the-template-from-c"></a>C からテンプレートを操作する\#

からC#表示される HTML web ビューへの通信は、web ビューで JavaScript を呼び出すことによって行われます。 IOS では、これは uiwebview `EvaluateJavascript`でを呼び出すことによって行われます。

```csharp
webView.EvaluateJavascript (js);
```

Android では、 `"javascript:"` url スキームを使用して javascript を url として読み込むことにより、web ビューで javascript を呼び出すことができます。

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>アプリを真のハイブリッドにする

これらのテンプレートは、各プラットフォームでネイティブコントロールを使用しません。画面全体に1つの web ビューが設定されます。

HTML は、プロトタイプに適しています。また、リッチテキストや応答性の高いレイアウトなど、web が最適な種類の情報を表示できます。 ただし、すべてのタスクが HTML および JavaScript に適しているわけではありません。たとえば、長いデータリストをスクロールすると、ネイティブ UI コントロール (iOS の UITableView、Android の ListView など) を使用してパフォーマンスが向上します。

テンプレート内の web ビューは、プラットフォーム固有のコントロールを使用して簡単に拡張できます。 iOS デザイナーの**mainstoryboard.storyboard ファイル**を編集するか、Android の**Resources/layout/Main. axml**を編集するだけです。

### <a name="razortodo-sample"></a>RazorTodo サンプル

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)リポジトリには、完全な html 駆動型アプリと、html をネイティブコントロールと組み合わせるアプリの違いを示すための2つのソリューションが含まれています。

- **RazorTodo** -Razor テンプレートを使用する完全な HTML 駆動型アプリ。
- **RazorNativeTodo** -IOS および Android 用のネイティブのリストビューコントロールを使用しますが、編集画面は HTML および Razor で表示されます。


これらの Xamarin アプリは、iOS と Android の両方で動作し、ポータブルクラスライブラリ (Pcl) を利用してデータベースやモデルクラスなどの共通コードを共有します。 Razor**テンプレート**は、複数のプラットフォーム間で簡単に共有できるように、PCL に含めることもできます。

両方のサンプル アプリでは、Twitter で共有および Xamarin を使用したハイブリッド アプリケーションでもあるアクセス基になるすべての機能にテンプレート駆動の HTML Razor ビューからのデモ、ネイティブ プラットフォームからの音声合成 API を組み込みます。

**RazorTodo**アプリでは、リストビューと編集ビューに HTML Razor テンプレートを使用します。 つまり、アプリケーションは、ほとんどの場合、共有のポータブルクラスライブラリ (データベースおよび**cshtml** Razor テンプレートを含む) でビルドできます。 以下のスクリーンショットは、iOS アプリと Android アプリを示しています。

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo**アプリでは、編集ビューに HTML Razor テンプレートが使用されますが、各プラットフォームにはネイティブスクロールリストが実装されています。 これには、次のようなさまざまな利点があります。

- パフォーマンス-ネイティブスクロールコントロールでは仮想化を使用して、データのリストが非常に長い場合でも、高速でスムーズにスクロールできるようにします。
- ネイティブエクスペリエンス-プラットフォーム固有の UI 要素は、iOS および Android での高速スクロールインデックスのサポートなど、簡単に有効にすることができます。


Xamarin を使用してハイブリッドアプリを構築する主な利点は、(最初のサンプルのように) HTML 駆動型のユーザーインターフェイスを使用して開始し、必要に応じてプラットフォーム固有の機能を追加できることです (2 番目のサンプルで示すように)。 IOS と Android の両方のネイティブリスト画面と HTML Razor 編集画面を次に示します。

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>まとめ

この記事では、ハイブリッドアプリケーションの構築を容易にする、iOS および Android で使用できる web ビューコントロールの機能について説明しました。

次に、を使用して、Xamarin アプリで HTML を簡単に生成するために使用できる Razor テンプレートエンジンと構文について説明しました。**cshtml**Razor テンプレートファイル。 また、Xamarin を使用してハイブリッドアプリケーションの構築をすぐに開始できるようにするための Visual Studio for Mac ソリューションテンプレートについても説明しました。

最後に、ネイティブ ユーザー インターフェイスと API と web ビューを結合する方法を示す RazorTodo サンプルが導入されました。

### <a name="related-links"></a>関連リンク

- [RazorTodo サンプル](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3-Razor ビューエンジン (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor 構文を使用した ASP.NET Web プログラミングの概要 (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
