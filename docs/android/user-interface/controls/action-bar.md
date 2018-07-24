---
title: アクションバー
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 29726450e0899177f3223deeade1cb4766d933a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769095"
---
# <a name="actionbar"></a>アクションバー


## <a name="overview"></a>概要

使用する場合`TabActivity`、 タブのアイコンを作成するコードは、Android 4.0 framework に対して実行すると影響を与えません。 機能的に動作しますが、2.3 の場合前のバージョンの Android の場合と同様、 `TabActivity` 4.0 ではクラス自体は廃止されました。 次に説明する操作バーを使用するタブ付きインターフェイスを作成する新しい方法が導入されました。


## <a name="action-bar-tabs"></a>操作バー タブ

操作バーには、Android 4.0 でのタブ付きインターフェイスの追加のサポートが含まれています。
次のスクリーン ショットは、このようなインターフェイスの例を示します。

[![アプリがエミュレーターで実行されているのスクリーン ショット2 つのタブが表示されます。](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

操作バーで、タブを作成、おには、まず設定する必要がその`NavigationMode`プロパティ タブをサポートします。 Android 4 で、`ActionBar`プロパティが設定する際に使用できるアクティビティ クラスに使用できる、`NavigationMode`次のように。

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

これを完了すると、呼び出すことによって、タブを生み出すことができます、`NewTab`操作バー上のメソッドです。 このタブのインスタンスと呼ぶことに、`SetText`と`SetIcon` タブのラベルのテキストとアイコンを設定する方法以下に示すコード内の順序でこれらの呼び出しが行われました。

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

ただし、タブを追加することができます、前に処理する必要があります、`TabSelected`イベント。 このハンドラーで、タブのコンテンツを作成できます。操作バー タブが連携するように設計*フラグメント*、これらは、アクティビティのユーザー インターフェイスの一部を表すクラス。 この例では、フラグメントのビューが含まれています、1 つには`TextView`で展開している、`Fragment`サブクラスは次のように。

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

イベントの引数で渡されて、`TabSelected`型のイベントは、`TabEventArgs`が含まれている、`FragmentTransaction`プロパティを使用して、次に示すように、フラグメントを追加します。

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

最後に追加できるタブ操作バーを呼び出して、`AddTab`メソッドのこのコードに示すように。

```csharp
this.ActionBar.AddTab (tab);
```

完全な例については、 *HelloTabsICS*このドキュメントのサンプル コードでのプロジェクトです。


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider`クラスにより、共有の操作をアクション バーから実行します。 共有インテントを処理し、操作バーから後でそれらにアクセスしやすいように以前に使用したアプリケーションの履歴を保持するアプリの一覧でアクション ビューを作成することによって行われます。 これにより、アプリケーションを Android 全体で一貫性のあるユーザー エクスペリエンスを使用してデータを共有できます。


### <a name="image-sharing-example"></a>イメージの共有の例

たとえば、以下は、イメージを共有するメニュー項目とアクション バーのスクリーン ショット (から取得した、 [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)サンプル)。 ShareActionProvider 読み込みますに関連付けられているインテントを処理するアプリケーションでユーザーが操作バーのメニュー項目をタップしたときに、`ShareActionProvider`です。 この例では、メッセージング アプリケーションが使用されている以前、ためアクション バーに表示されます。

[![アクションのバーにアプリケーション アイコンをメッセージングのスクリーン ショット](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


操作バー内のアイテムに対してユーザーがクリックすると、共有のイメージが含まれるメッセージング アプリを起動すると、次のように。

[![サル イメージを表示するメッセージのアプリのスクリーン ショット](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>アクションのプロバイダー クラスを指定します。

使用する、 `ShareActionProvider`、設定、 `android:actionProviderClass` xml 操作バーのメニューのメニュー項目を次のように属性します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>メニューを膨張させる

オーバーライド、メニューを展開する`OnCreateOptionsMenu`Activity サブクラスにします。 メニューへの参照を作成したら、取得できる、`ShareActionProvider`から、`ActionProvider`プロパティのメニュー項目とし、SetShareIntent を設定するメソッドを使用して、`ShareActionProvider`の目的とした、次のように。

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


### <a name="creating-the-intent"></a>目的の作成

`ShareActionProvider`に渡される、インテントを使用する、`SetShareIntent`を適切な活動を起動する、上記のコードでメソッドです。 ここでは、次のコードを使用してイメージを送信するインテントを作成します。

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

上記のコード例ではイメージがアプリケーションを使用して、資産として含まれているされ、メッセージング アプリケーションなど、他のアプリケーションにアクセスできるように、アクティビティが作成されると、パブリックにアクセスできる場所にコピーします。 この記事に付属するサンプル コードには、その使用方法を示す、この例の完全なソースが含まれています。



## <a name="related-links"></a>関連リンク

- [こんにちはタブ ICS (サンプル)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [ShareActionProvider デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
