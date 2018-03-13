---
title: Gallery
ms.topic: article
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: da6815a073d93379c8564f3ff91023deb20b0d55
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="gallery"></a>Gallery

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) レイアウト ウィジェットが水平方向にスクロール リストに項目を表示するために使用し、ビューの中央に現在の選択範囲を配置します。

> [!IMPORTANT]
> このウィジェットは、Android 4.1 (API レベル 16) で推奨されなくなりました。 

このチュートリアルをフォト ギャラリーを作成し、ギャラリー項目を選択するたびにし、トースト メッセージを表示します。

後に、`Main.axml`レイアウトが、コンテンツ ビューの設定、`Gallery`をレイアウトからキャプチャされた[ `FindViewById`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/)です。
[ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)プロパティは、カスタム アダプターの設定を使用して ( `ImageAdapter`)、dallery に表示されるすべての項目のソースとして。 `ImageAdapter`が次の手順で作成します。

ギャラリー内のアイテムがクリックされたときに何かには、匿名デリゲートがサブスクライブしている、 [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)イベント。 表示、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)選択した項目の (0 から始まる) インデックス位置を表示する (実際のシナリオでの位置を使用できますの他のいくつかのタスクの完全画像のサイズを取得する)。

まず、ドロウアブル リソース ディレクトリに保存されているイメージを参照する Id の配列を含む、いくつかのメンバー変数がある (**リソース/描画**)。

次に、クラスのコンス トラクターは、ここで、 [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/)の`ImageAdapter`インスタンスが定義されているし、ローカル フィールドに保存します。
継承されるいくつか必要なメソッドを次に、これを実装[ `BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)です。
コンス トラクターは、および[ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/)プロパティには、ひとめでわかります。 通常、 [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/)アダプターで、指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 同様に、 [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/)項目の行 id を返す必要がありますが、ここには必要ありません。

メソッドにイメージを適用する作業では、 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)に埋め込まれている、 [ `Gallery` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)メンバーは、このメソッドで[ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/)は新規作成するために使用[ `ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)です。
[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)ドロウアブル リソース、設定のローカルの配列からイメージを適用することによって作成された、 [ `Gallery.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/) に合わせてスケールを設定、画像の高さと幅[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)ディメンション、およびコンス トラクターで取得した styleable 属性を使用して、バック グラウンドを最後に設定します。

参照してください[ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/)の他のイメージのスケーリングの各オプションです。

## <a name="walkthrough"></a>チュートリアル

という名前の新しいプロジェクトを開始*HelloGallery*です。

![新しいソリューション ダイアログ ボックスで新しい Android プロジェクトのスクリーン ショット](gallery-images/hellogallery1.png)

を使用するにはいくつかの写真を検索または[これらのサンプル画像をダウンロード](http://developer.android.com/shareables/sample_images.zip)です。
イメージ ファイルをプロジェクトの追加**リソース/ディスプレイ**ディレクトリ。 **プロパティ**ウィンドウで、列ごとに、ビルド アクションを設定する**AndroidResource**です。

開いている**Resources/Layout/Main.axml**し、次の挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

開いている`MainActivity.cs`の次のコードを挿入し、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)メソッド。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

いう新しいクラスを作成する`ImageAdapter`をサブクラスとして持つ[ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

    public override Java.Lang.Object GetItem (int position)
    {
          return null;
    }

    public override long GetItemId (int position)
    {
          return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

アプリケーションを実行します。 次のスクリーン ショットのようになります。

![サンプル イメージを表示する HelloGallery のスクリーン ショット](gallery-images/hellogallery3.png)



## <a name="references"></a>参照

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).


