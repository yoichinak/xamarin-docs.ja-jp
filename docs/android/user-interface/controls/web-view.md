---
title: Web ビュー
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8d7b0e1abc8eb11bf812a111764b9cccfb41e041
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241176"
---
# <a name="web-view"></a>Web ビュー

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) web ページを表示するための独自のウィンドウを作成します (または完全なブラウザーを開発しても) することができます。 このチュートリアルでは、単純なを作成します[ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)を表示して、web ページを移動することができます。

という名前の新しいプロジェクトを作成する**HelloWebView**します。

開いている**Resources/Layout/Main.axml**し、次を挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

このアプリケーションをインターネットにアクセスするため、適切な権限を Android マニフェスト ファイルを追加する必要があります。 動作するアプリケーションに必要なアクセス許可を指定するプロジェクトのプロパティを開きます。 有効にする、`INTERNET`次に示すようにアクセス許可。

![Android マニフェストでのインターネット アクセス許可の設定](web-view-images/01-set-internet-permissions.png)

今すぐ開きます**MainActivity.cs**を使用して、追加 Webkit のディレクティブ。

```csharp
using Android.Webkit;
```

上部にある、`MainActivity`クラスを宣言、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)オブジェクト。

```csharp
WebView web_view;
```

ときに、 **WebView**は URL をロードしようとすると、既定でに委託する既定のブラウザー要求。 **WebView** URL (既定のブラウザーではなく) を読み込み、サブクラスを作成する必要があります`Android.Webkit.WebViewClient`をオーバーライドし、`ShouldOverriderUrlLoading`メソッド。 このカスタムのインスタンス`WebViewClient`に提供される、`WebView`します。 これを行うには、追加、次の入れ子になった`HelloWebViewClient`クラス内で`MainActivity`:

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

ときに`ShouldOverrideUrlLoading`返します`false`、Android に通知を現在`WebView`インスタンスが要求を処理し、これ以上の操作は必要ありません。 

API レベル 24 以上をターゲットにする場合のオーバー ロードを使用して、`ShouldOverrideUrlLoading`を受け取る、`IWebResourceRequest`の 2 番目の引数の代わりに、 `string`:

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

次に、次のコードを使用して、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))メソッド。

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

メンバー初期化は、この[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)から 1 つで、 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)レイアウトの JavaScript を使用して、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) で[`JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (を参照してください、[呼び出す C\# JavaScript から](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)C の呼び出し方法についてはレシピ\#JavaScript からの関数)。 最初の web ページが最後に、読み込まれた[ `LoadUrl(String)`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl)します。

アプリケーションをビルドし、実行します。 次のスクリーン ショットで 1 つとして、単純な web ページ ビューアー アプリが表示されます。

[![Web ビューを表示するアプリの例](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

処理するために、**戻る**キーの押下をクリックし、次の追加ステートメントを使用します。

```csharp
using Android.Views;
```

内で次のメソッドを次に、追加、`HelloWebView`アクティビティ。

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

これは、 [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent))アクティビティの実行中にボタンが押されたときにコールバック メソッドが呼び出されます。 内部使用条件、 [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) 、キーが押されたかどうかを確認するのには、**戻る**ボタンかどうかと、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)が実際にできます。戻る (ある場合、履歴) を移動します。 どちらにも該当する場合、 [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/)メソッドが呼び出される 1 つ戻りますの手順を移動しますが、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)履歴。 返す`true`イベントが処理されたことを示します。 この条件が満たされていない場合は、イベントがシステムに送信されます。

アプリケーションをもう一度実行します。 リンクをたどるし、ページの履歴を後方に移動できるようになりましたに。

[![アクションで [戻る] ボタンのスクリーン ショットの例](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>関連リンク

- [JavaScript から呼び出す (C#)](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
