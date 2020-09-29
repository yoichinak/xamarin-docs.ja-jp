---
title: Xamarin. Android の ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 45b6c080f6e50efc4648c5b2eab6db0634dee7f1
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457368"
---
# <a name="actionbar-for-xamarinandroid"></a>Xamarin. Android の ActionBar

を使用する場合 `TabActivity` 、タブアイコンを作成するコードは、Android 4.0 フレームワークに対して実行しても効果がありません。 2.3 より前のバージョンの Android と同様に機能しますが、 `TabActivity` クラス自体は4.0 で非推奨とされています。 次に説明する操作バーを使用する、タブ付きインターフェイスを作成する新しい方法が導入されました。

## <a name="action-bar-tabs"></a>操作バータブ

操作バーには、Android 4.0 でタブ付きインターフェイスを追加するためのサポートが含まれています。
次のスクリーンショットは、このようなインターフェイスの例を示しています。

[![エミュレーターで実行されているアプリのスクリーンショット。2つのタブが表示されます。](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

操作バーでタブを作成するには、まず、タブをサポートするようにプロパティを設定する必要があり `NavigationMode` ます。 Android 4 では、 `ActionBar` アクティビティクラスでプロパティを使用できます。このクラスを使用して、次のようにを設定でき `NavigationMode` ます。

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

この処理が完了したら、操作バーでメソッドを呼び出してタブを作成でき `NewTab` ます。 このタブインスタンスでは、 `SetText` メソッドとメソッドを呼び出して `SetIcon` タブのラベルのテキストとアイコンを設定できます。これらの呼び出しは、以下に示すコードの順序で行われます。

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

ただし、タブを追加する前に、イベントを処理する必要があり `TabSelected` ます。 このハンドラーでは、タブのコンテンツを作成できます。操作バータブは、アクティビティのユーザーインターフェイスの一部を表すクラスである *フラグメント*を使用するように設計されています。 この例では、フラグメントのビューに1つのが含まれています。これは、次の `TextView` ようにサブクラスで拡大し `Fragment` ます。

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

イベントで渡されるイベント引数の `TabSelected` 型は `TabEventArgs` であり、 `FragmentTransaction` 次に示すように、フラグメントを追加するために使用できるプロパティが含まれています。

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

最後に、 `AddTab` 次のコードに示すように、メソッドを呼び出すことによって、タブを操作バーに追加できます。

```csharp
this.ActionBar.AddTab (tab);
```

完全な例については、このドキュメントのサンプルコードの *HelloTabsICS* プロジェクトを参照してください。

## <a name="shareactionprovider"></a>/のアクションプロバイダー

`ShareActionProvider`クラスを使用すると、操作バーから共有アクションを実行できます。 共有の目的を処理し、以前に使用したアプリケーションの履歴を保存しておくことによって、操作バーから後で簡単にアクセスできるようにするアプリの一覧を使用して、アクションビューを作成します。 これにより、Android 全体で一貫性のあるユーザーエクスペリエンスを使用してアプリケーションがデータを共有できるようになります。

### <a name="image-sharing-example"></a>イメージ共有の例

たとえば、次に示すのは、イメージを共有するためのメニュー項目を持つ操作バーのスクリーンショットです (共有 [Actionprovider](/samples/xamarin/monodroid-samples/shareactionproviderdemo) サンプルから取得)。 ユーザーが操作バーのメニュー項目をタップすると、に関連付けられているインテントを処理するために、このアプリケーションが読み込まれ `ShareActionProvider` ます。 この例では、メッセージングアプリケーションが既に使用されているため、操作バーに表示されます。

[![操作バーのメッセージングアプリケーションアイコンのスクリーンショット](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)

ユーザーが操作バーの項目をクリックすると、次のように、共有イメージを含むメッセージングアプリが起動されます。

[![サル画像を表示しているメッセージングアプリのスクリーンショット](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)

### <a name="specifying-the-action-provider-class"></a>アクションプロバイダークラスの指定

を使用するには `ShareActionProvider` 、 `android:actionProviderClass` 次のように、操作バーのメニューの XML のメニュー項目に対して属性を設定します。

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

メニューを拡大するには、 `OnCreateOptionsMenu` Activity サブクラスでをオーバーライドします。 メニューへの参照を取得したら、 `ShareActionProvider` `ActionProvider` 次に示すように、メニュー項目のプロパティからを取得し、set/インテントメソッドを使用し `ShareActionProvider` て目的を設定できます。

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

は、上記のコードのメソッドに渡されたインテントを使用して、 `ShareActionProvider` `SetShareIntent` 適切なアクティビティを起動します。 この例では、次のコードを使用してイメージを送信するインテントを作成します。

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

- [Hello Tabs ICS (サンプル)](/samples/xamarin/monodroid-samples/hellotabsics)
- [/Sharepoint のデモ (サンプル)](/samples/xamarin/monodroid-samples/shareactionproviderdemo)