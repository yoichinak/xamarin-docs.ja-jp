---
title: ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 95ebcc8ef436c90e807045bd009b35ff1c3e9c1f
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740798"
---
# <a name="actionbar"></a>ActionBar


## <a name="overview"></a>概要

使用する場合`TabActivity`タブのアイコンを作成するコードは、Android 4.0 framework に対して実行すると影響を与えません。 以前のバージョンの Android 2.3 以降、同様に動作します機能的、`TabActivity`クラス自体が 4.0 の非推奨とされました。 次に説明する操作バーを使用して、タブ付きインターフェイスを作成する新しい方法が導入されています。


## <a name="action-bar-tabs"></a>操作バーのタブ

アクション バーには、Android 4.0 でのタブ付きインターフェイスを追加するためのサポートが含まれています。
次のスクリーン ショットでは、このようなインターフェイスの例を示します。

[![は、エミュレーターで実行されているアプリのスクリーン ショット2 つのタブが表示されます。](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

操作バーのタブを作成することには、まず設定する必要がその`NavigationMode`プロパティ タブをサポートします。 Android 4 で、`ActionBar`プロパティが設定に使用できるアクティビティ クラスに使用可能な`NavigationMode`次のように。

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

これを完了すると、呼び出すことによって、タブを生み出すことができます、`NewTab`アクション バーでのメソッド。 呼び出し、このタブのインスタンスで、`SetText`と`SetIcon` タブのラベルのテキストとアイコンを設定するメソッドを以下に示すコード内の順序でこれらの呼び出しが行われました。

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

ただし、タブを追加することができます、前に処理する必要があります、`TabSelected`イベント。 このハンドラーで、タブのコンテンツを作成できます。操作バーのタブが使用するように設計*フラグメント*、これらは、アクティビティのユーザー インターフェイスの一部を表すクラス。 フラグメントのビューにはこの例では、1 つが含まれています`TextView`に拡大しましたが、`Fragment`このようなサブクラスです。

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);

        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

イベント引数が渡されて、`TabSelected`型のイベントは、`TabEventArgs`が含まれています、`FragmentTransaction`次に示すように、フラグメントを追加するために使用できるプロパティ。

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

呼び出して、操作バーに、タブを追加最後に、`AddTab`メソッドのこのコードで示すようにします。

```csharp
this.ActionBar.AddTab (tab);
```

完全な例では、次を参照してください。、 *HelloTabsICS*このドキュメントのサンプル コードでのプロジェクト。


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider`クラスが共有の動作が、操作バーから実行できるようにします。 共有インテントを処理し、操作バーから後でそれらを簡単にアクセスに使用されていたアプリケーションの履歴を保持するアプリの一覧でアクション ビューの作成を行います。 これにより、アプリケーションを Android 全体で一貫性のあるユーザー エクスペリエンスを使用してデータを共有できます。


### <a name="image-sharing-example"></a>イメージの共有の例

たとえば、以下は、イメージを共有するとメニュー項目の操作バーのスクリーン ショット (から取得した、 [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)サンプル)。 ShareActionProvider に関連付けられているインテントを処理するためにアプリケーションを読み込み、ユーザーが、操作バーのメニュー項目をタップしたとき、`ShareActionProvider`します。 この例でのメッセージング アプリケーションが過去に使用された、ため、操作バーに表示されます。

[![メッセージング操作バーのアプリケーション アイコンのスクリーン ショット](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


ユーザーは、操作バーの項目をクリックすると、共有のイメージが含まれるメッセージング アプリを起動すると、次に示します。

[![Monkey のイメージを表示するメッセージング アプリのスクリーン ショット](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>アクション プロバイダー クラスを指定します。

使用する、 `ShareActionProvider`、設定、 `android:actionProviderClass` xml 操作バーのメニューにメニュー項目を次のように属性します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>メニューを膨張させること

無効に、メニューを展開する`OnCreateOptionsMenu`でアクティビティ サブクラスです。 メニューへの参照を取得したら、取得できる、`ShareActionProvider`から、`ActionProvider`し設定する SetShareIntent メソッドを使用して、メニュー項目のプロパティ、`ShareActionProvider`の目的とした、次に示すよう。

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       

    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>インテントを作成します。

`ShareActionProvider`に渡される、インテントを使用、`SetShareIntent`適切なアクティビティを起動する、上記のコード内のメソッド。 ここでは、次のコードを使用してイメージを送信するインテントを作成します。

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

上記のコード例のイメージがアプリケーション資産として含まれており、メッセージング アプリなどの他のアプリケーションにアクセスできるように、アクティビティが作成されると、パブリックにアクセスできる場所にコピーします。 この記事に付属するサンプル コードには、その使用方法を示す、この例の完全なソースが含まれています。



## <a name="related-links"></a>関連リンク

- [こんにちはタブ ICS (サンプル)](https://developer.xamarin.com/samples/monodroid/HelloTabsICS/)
- [ShareActionProvider デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
