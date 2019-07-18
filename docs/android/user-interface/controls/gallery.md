---
title: Gallery
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: f9b73428531deeacc7bdea271cdc0c2872038e99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218561"
---
# <a name="gallery"></a>Gallery

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) レイアウト ウィジェットが、水平方向にスクロール リストに項目を表示するために使用され、ビューの中心に現在の選択範囲を配置します。

> [!IMPORTANT]
> このウィジェットは、Android 4.1 (API レベル 16) で非推奨とされました。 

このチュートリアルではフォト ギャラリーを作成し、ギャラリー項目を選択するたびにトースト メッセージを表示します。

後に、`Main.axml`コンテンツ ビューのレイアウトが設定されて、`Gallery`をレイアウトからキャプチャされた[ `FindViewById`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/)します。
、 [`Adapter`](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)
プロパティは、カスタム アダプターの設定を使用し ( `ImageAdapter`)、dallery に表示されるすべての項目のソースとして。 `ImageAdapter`が次の手順で作成されます。

匿名デリゲートの購読を行うには何か、ギャラリー内の項目がクリックされたときに、 [`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)
イベント。 表示します。 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
選択した項目のインデックス位置 (0 から始まる) を表示する (実際のシナリオで、位置される可能性があります他のタスクの完全な画像のサイズを取得する)。

まず、ドローアブル リソース ディレクトリに保存されたイメージを参照する Id の配列を含む、いくつかのメンバー変数がある (**リソース/drawable**)。

次に、クラスのコンス トラクターは、場所、 [`Context`](https://developer.xamarin.com/api/type/Android.Content.Context/)
`ImageAdapter`インスタンスが定義されているし、ローカル フィールドに保存します。
継承されるいくつか必要なメソッドを次に、この実装[ `BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)します。
コンス トラクター、 [`Count`](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/)
プロパティは、説明されています。 通常であれば [`GetItem(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/)
アダプターで、指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 同様に、 [`GetItemId(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/)
項目の行の id を返す必要がありますが、ここで必要はありません。

メソッドにイメージを適用する作業では、 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
埋め込まれている、 [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
このメソッドは、メンバー [`Context`](https://developer.xamarin.com/api/type/Android.Content.Context/)
新たに作成するために使用[ `ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)します。
、 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
ローカル ドローアブル リソース、設定の配列からイメージを適用することで準備は、 [`Gallery.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/)
高さと幅に合わせてスケールを設定、イメージの場合、 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
ディメンション、およびコンス トラクターで取得した高めます属性を使用するバック グラウンドを最後に設定します。

参照してください[ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/)の他のイメージのスケーリングのオプション。

## <a name="walkthrough"></a>チュートリアル

という名前の新しいプロジェクトを開始*HelloGallery*します。

[![新しいソリューション ダイアログ ボックスで新しい Android プロジェクトのスクリーン ショット](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

を使用するにはいくつかの写真を検索または[これらのサンプル イメージをダウンロード](https://developer.android.com/shareables/sample_images.zip)します。
イメージ ファイルをプロジェクトの追加**リソース/ディスプレイ**ディレクトリ。 **プロパティ**ごとにビルド アクション ウィンドウで、設定**AndroidResource**します。

開いている**Resources/Layout/Main.axml**し、次を挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

開いている`MainActivity.cs`の次のコードを挿入し、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
方法:

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

という新しいクラスを作成`ImageAdapter`をサブクラスとして持つ[ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

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

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).


