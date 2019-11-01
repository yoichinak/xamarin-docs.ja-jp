---
title: Xamarin. Android の ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: b2b7c2cc87b37ae0e7397988e37df6b9b1e3aa10
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029388"
---
# <a name="actionbar-for-xamarinandroid"></a>Xamarin. Android の ActionBar

`TabActivity`を使用する場合は、Android 4.0 フレームワークに対して実行しても、タブアイコンを作成するコードは効果がありません。 2\.3 より前のバージョンの Android では機能的に機能しますが、`TabActivity` クラス自体は4.0 で非推奨とされています。 次に説明する操作バーを使用する、タブ付きインターフェイスを作成する新しい方法が導入されました。

## <a name="action-bar-tabs"></a>操作バータブ

操作バーには、Android 4.0 でタブ付きインターフェイスを追加するためのサポートが含まれています。
次のスクリーンショットは、このようなインターフェイスの例を示しています。

[エミュレーターで実行されているアプリのスクリーンショットを![します。2つのタブが表示されます。](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

操作バーでタブを作成するには、まず、`NavigationMode` プロパティを [サポート] タブに設定する必要があります。 Android 4 では、`ActionBar` プロパティを Activity クラスで使用できます。このプロパティを使用して、次のように `NavigationMode` を設定できます。

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

この処理が完了したら、操作バーで `NewTab` メソッドを呼び出して、タブを作成できます。 このタブインスタンスを使用して、`SetText` および `SetIcon` メソッドを呼び出して、タブのラベルのテキストとアイコンを設定できます。これらの呼び出しは、次に示すコードの順序で行われます。

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

ただし、タブを追加する前に、`TabSelected` イベントを処理する必要があります。 このハンドラーでは、タブのコンテンツを作成できます。操作バータブは、アクティビティのユーザーインターフェイスの一部を表すクラスである*フラグメント*を使用するように設計されています。 この例では、フラグメントのビューには1つの `TextView`が含まれており、次のように `Fragment` サブクラスで拡大します。

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

`TabSelected` イベントで渡されるイベント引数の型は `TabEventArgs`であり、次に示すように、フラグメントを追加するために使用できる `FragmentTransaction` プロパティが含まれています。

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

最後に、次のコードに示すように、`AddTab` メソッドを呼び出すことによって、タブを操作バーに追加できます。

```csharp
this.ActionBar.AddTab (tab);
```

完全な例については、このドキュメントのサンプルコードの*HelloTabsICS*プロジェクトを参照してください。

## <a name="shareactionprovider"></a>/のアクションプロバイダー

`ShareActionProvider` クラスを使用すると、操作バーから共有アクションを実行できます。 共有の目的を処理し、以前に使用したアプリケーションの履歴を保存しておくことによって、操作バーから後で簡単にアクセスできるようにするアプリの一覧を使用して、アクションビューを作成します。 これにより、Android 全体で一貫性のあるユーザーエクスペリエンスを使用してアプリケーションがデータを共有できるようになります。

### <a name="image-sharing-example"></a>イメージ共有の例

たとえば、次に示すのは、イメージを共有するためのメニュー項目を持つ操作バーのスクリーンショットです (共有[Actionprovider](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo)サンプルから取得)。 ユーザーが操作バーのメニュー項目をタップすると、`ShareActionProvider`に関連付けられているインテントを処理するために、このアプリケーションが読み込まれます。 この例では、メッセージングアプリケーションが既に使用されているため、操作バーに表示されます。

[操作バー内のメッセージングアプリケーションアイコンのスクリーンショット![](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)

ユーザーが操作バーの項目をクリックすると、次のように、共有イメージを含むメッセージングアプリが起動されます。

[![、サル画像を表示しているメッセージングアプリのスクリーンショット](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)

### <a name="specifying-the-action-provider-class"></a>アクションプロバイダークラスの指定

`ShareActionProvider`を使用するには、操作バーのメニューの XML のメニュー項目の `android:actionProviderClass` 属性を次のように設定します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```

### <a name="inflating-the-menu"></a>メニューの拡大

メニューを拡大するには、Activity サブクラスの `OnCreateOptionsMenu` をオーバーライドします。 メニューへの参照を取得したら、次に示すように、メニュー項目の `ActionProvider` プロパティから `ShareActionProvider` を取得し、Set/インテントメソッドを使用して `ShareActionProvider`の意図を設定できます。

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

### <a name="creating-the-intent"></a>インテントの作成

`ShareActionProvider` は、上記のコードの `SetShareIntent` メソッドに渡されるインテントを使用して、適切なアクティビティを起動します。 この例では、次のコードを使用してイメージを送信するインテントを作成します。

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

上のコード例のイメージは、アプリケーションと共に資産として含まれており、アクティビティの作成時にパブリックにアクセスできる場所にコピーされます。そのため、メッセージングアプリなどの他のアプリケーションからアクセスできるようになります。 この記事に付属するサンプルコードには、この例の使用方法を示す完全なソースが含まれています。

## <a name="related-links"></a>関連リンク

- [Hello Tabs ICS (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/hellotabsics)
- [/Sharepoint のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo)
- [アイスクリームサンドイッチの導入](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
