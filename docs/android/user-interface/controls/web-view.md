---
title: Web ビュー
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: c1fcb16bd40b818b27b57b877534e051a789a6c9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029017"
---
# <a name="xamarinandroid-web-view"></a>Xamarin Android Web ビュー

[`WebView`](xref:Android.Webkit.WebView)を使用すると、web ページを表示するための独自のウィンドウを作成できます (または、完全なブラウザーを開発することもできます)。 このチュートリアルでは、単純な[`Activity`](xref:Android.App.Activity)を作成します。
これにより、web ページを表示したり、移動したりできます。

"/" という**名前の新しい**プロジェクトを作成します。

**Resources/Layout/Main**を開き、次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

このアプリケーションはインターネットにアクセスするため、Android マニフェストファイルに適切なアクセス許可を追加する必要があります。 プロジェクトのプロパティを開いて、アプリケーションが動作するために必要なアクセス許可を指定します。 次に示すように、`INTERNET` のアクセス許可を有効にします。

![Android マニフェストでのインターネットアクセス許可の設定](web-view-images/01-set-internet-permissions.png)

ここで、 **MainActivity.cs**を開き、Webkit に using ディレクティブを追加します。

```csharp
using Android.Webkit;
```

`MainActivity` クラスの先頭に、 [`WebView`](xref:Android.Webkit.WebView)オブジェクトを宣言します。

```csharp
WebView web_view;
```

Web ビュー**で URL の読み込みが要求**されると、既定では既定のブラウザーに要求が委任されます。 **WebView**で (既定のブラウザーではなく) URL を読み込むには、`Android.Webkit.WebViewClient` をサブクラス化し、`ShouldOverriderUrlLoading` メソッドをオーバーライドする必要があります。 このカスタム `WebViewClient` のインスタンスは、`WebView`に提供されます。 これを行うには、`MainActivity`内に次の入れ子になった `HelloWebViewClient` クラスを追加します。

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

`ShouldOverrideUrlLoading` が `false`を返すと、現在の `WebView` インスタンスが要求を処理したことと、それ以上のアクションが不要であることが Android に通知されます。 

API レベル24以降を対象としている場合は、`string`ではなく、2番目の引数に対して `IWebResourceRequest` を受け取る `ShouldOverrideUrlLoading` のオーバーロードを使用します。

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

次に、 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)) メソッドに次のコードを使用します。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

これにより、 [`Activity`](xref:Android.App.Activity)レイアウトのメンバー [`WebView`](xref:Android.Webkit.WebView)が初期化され、 [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled)
`= true` を使用して[`WebView`](xref:Android.Webkit.WebView)の javascript が有効になります (javascript レシピ[からの呼び出し C\#](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)を参照してください)。JavaScript から C\# 関数を呼び出す方法について説明します)。 最後に、 [`LoadUrl(String)`](xref:Android.Webkit.WebView)と共に最初の web ページが読み込まれます。

アプリケーションをビルドし、実行します。 次のスクリーンショットに示すように、単純な web ページビューアーアプリが表示されます。

[WebView を表示するアプリの![例](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

**[戻る]** ボタンのキーを押すには、次の using ステートメントを追加します。

```csharp
using Android.Views;
```

次に、`HelloWebView` アクティビティ内に次のメソッドを追加します。

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

この[`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
コールバックメソッドは、アクティビティの実行中にボタンが押されるたびに呼び出されます。 内の条件では、 [`KeyEvent`](xref:Android.Views.KeyEvent)を使用して、押されたキーが**戻る**ボタンであるかどうか、および[`WebView`](xref:Android.Webkit.WebView)が実際に戻ることができるかどうかを確認します (履歴がある場合)。 両方が true の場合、 [`GoBack()`](xref:Android.Webkit.WebView.GoBack)メソッドが呼び出されます。これにより、 [`WebView`](xref:Android.Webkit.WebView)履歴の1つのステップが戻ります。 `true` を返すと、イベントが処理されたことを示します。 この条件が満たされていない場合は、イベントがシステムに送り返されます。

アプリケーションをもう一度実行します。 リンクに移動して、ページ履歴を表示できるようになります。

[![操作の [戻る] ボタンのスクリーンショットの例](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*され、
[*Creative Commons 2.5 属性のライセンス*](https://creativecommons.org/licenses/by/2.5/)に記載されている条項に従って使用される作業に基づいて変更されます。

## <a name="related-links"></a>関連リンク

- [JavaScript から C# を呼び出す](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Webkit](xref:Android.Webkit.WebView)
- [KeyEvent](xref:Android.Webkit.WebView)
