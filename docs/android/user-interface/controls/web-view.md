---
title: Web ビュー
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2baf7dae71ce7607c629b570ad25f477dec66c17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="web-view"></a>Web ビュー

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 使用すると、web ページを表示するための独自のウィンドウを作成 (または完全なブラウザーの開発も) できます。 このチュートリアルで作成、単純な[ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)を表示して、web ページを移動することができます。

という名前の新しいプロジェクトを作成する**HelloWebView**です。

開いている**Resources/Layout/Main.axml**し、次の挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

このアプリケーションをインターネットにアクセスするため、Android に適切なアクセス許可のマニフェスト ファイルを追加する必要があります。 動作するアプリケーションに必要なアクセス許可を指定するプロジェクトのプロパティを開きます。 有効にする、`INTERNET`次に示すようにアクセス許可。

![Android マニフェストでインターネット アクセス許可の設定](web-view-images/01-set-internet-permissions.png)

これで開く**MainActivity.cs**しを使用して、追加ディレクティブ Webkit を。

```csharp
using Android.Webkit;
```

上部にある、`MainActivity`クラスを宣言、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)オブジェクト。

```csharp
WebView web_view;
```

ときに、 **WebView**は URL をロードしようとすると、これは既定で委任既定のブラウザーに要求します。 **WebView** (既定のブラウザーではなく)、URL を読み込み、サブクラスを作成する必要があります`Android.Webkit.WebViewClient`をオーバーライドし、`ShouldOverriderUrlLoading`メソッドです。 このカスタムのインスタンス`WebViewClient`に提供される、`WebView`です。 これを行うには、追加、次の入れ子になった`HelloWebViewClient`内部クラス`MainActivity`:

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

API レベル 24 またはそれ以降を対象とする場合は、オーバー ロードを使用して`ShouldOverrideUrlLoading`を受け取る、`IWebResourceRequest`の 2 番目の引数の代わりに、 `string`:

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

これにより、メンバーが初期化[ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)から 1 つで、 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)レイアウトの JavaScript を有効にし、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) と[`JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (を参照してください、[呼び出す C\# JavaScript から](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)C の呼び出しをする方法についてのレシピ\#JavaScript からの関数)。 最後に、最初の web ページに読み込まれる[ `LoadUrl(String)`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl)です。

アプリケーションをビルドし、実行します。 次のスクリーン ショットに見られるものとして、単純な web ページ ビューアー アプリが表示されます。

[![アプリの WebView の表示の例](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

処理するために、**戻る**キーを押すのボタンを次の追加ステートメントを使用します。

```csharp
using Android.Views;
```

内の次のメソッドを次に、追加、`HelloWebView`アクティビティ。

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

これは、 [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent))アクティビティの実行中にボタンが押されたときにコールバック メソッドが呼び出されます。 内部使用条件、 [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/)キーが押されたかどうかを確認するのには、**戻る**ボタンかどうかおよび、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)が実際に可能戻る (ある場合、履歴) に移動します。 True の場合、両方がある場合、 [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/)メソッドは、バックアップ 1 つの手順を移動する、 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)履歴。 返す`true`イベントが処理されたことを示します。 この条件が満たされない場合は、イベントがシステムに送信されます。

アプリケーションをもう一度実行します。 リンクに従い、ページの履歴を後方に移動することができます。

[![アクションで [戻る] ボタンの例のスクリーン ショット](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>関連リンク

- [JavaScript から呼び出す (C#)](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
