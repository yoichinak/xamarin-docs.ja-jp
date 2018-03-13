---
title: GridView
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2fb3133833dbaa0b174c4611d204f6c8ceb42a2b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) 2 次元のスクロール可能なグリッドでアイテムを表示します。 グリッド項目は自動的に使用して、レイアウトに挿入、 [ `ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)です。

このチュートリアルでは、サムネイル画像のグリッドを作成します。 項目を選択すると、通知メッセージに画像の位置が表示されます。

という名前の新しいプロジェクトを開始**HelloGridView**です。

を使用するにはいくつかの写真を検索または[これらのサンプル画像をダウンロード](http://developer.android.com/shareables/sample_images.zip)です。 イメージ ファイルをプロジェクトの追加**リソース/ディスプレイ**ディレクトリ。 **プロパティ**ウィンドウで、列ごとに、ビルド アクションを設定する**AndroidResource**です。

開く、 **Resources/Layout/Main.axml**ファイルし、次の挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

これは、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)画面全体を入力します。 属性は、個人ではなく表記です。 有効な属性の詳細については、次を参照してください。、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)参照します。

開いている`HelloGridView.cs`の次のコードを挿入し、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)メソッド。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

後に、 **Main.axml**レイアウトが、コンテンツ ビューの設定、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)をレイアウトからキャプチャされた[ `FindViewById`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/)です。 [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)プロパティは、カスタム アダプターの設定を使用して (`ImageAdapter`)、グリッドに表示されるすべての項目のソースとして。 `ImageAdapter`が次の手順で作成します。

グリッド内のアイテムがクリックされたときに何かには、匿名デリゲートがサブスクライブしている、 [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)イベント。
表示、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)選択された項目のインデックス位置 (0 から始まる) を表示する (実際のシナリオでの位置を使用できますの他のいくつかのタスクの完全画像のサイズを取得する)。 .NET イベントではなく Java スタイルのリスナー クラスを使用できることに注意してください。

いう新しいクラスを作成する`ImageAdapter`をサブクラスとして持つ[ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

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
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

継承されるいくつか必要なメソッドを実装して最初に、 [ `BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)です。 コンス トラクターは、および[ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/)プロパティには、ひとめでわかります。 通常、 [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/)アダプターで、指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 同様に、 [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/)項目の行 id を返す必要がありますが、ここには必要ありません。

必要な最初のメソッドは[ `GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)です。
このメソッドは、新しい作成[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)に追加する各イメージの`ImageAdapter`です。 呼び出されると、 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)渡される、オブジェクトでは通常、リサイクル (には、少なくともこれが呼び出された後 1 回) でオブジェクトが null かどうかをチェックがあるため、します。 場合、*は*null の場合、 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)がインスタンス化され、イメージのプレゼンテーションの必要なプロパティで構成されています。

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) ビューの高さと幅を設定&mdash;これにより、ドロウアブルのサイズに関係なく各イメージはサイズを変更し、必要に応じて、これらのディメンションに収まるようにトリミングします。

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) (必要な場合) に、イメージが中央にトリミングすることを宣言します。

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) 上下左右の余白を定義します。 (、画像の縦横比が異なる場合は、小さい余白が発生、ImageView に指定されたディメンションが一致しない場合は、イメージの詳細トリミングを注意してください)。

場合、 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)に渡される[ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)は*いない*null の場合、次に、ローカル[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)で初期化されますリサイクル[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)オブジェクト。

最後に、 [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) 、メソッド、`position`からイメージを選択するメソッドに渡される整数が使用される、`thumbIds`のイメージ リソースとして設定されている配列、 [ `ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

定義するのには、あと、`thumbIds`ドロウアブル リソースの配列。

アプリケーションを実行します。 グリッド レイアウトは、次のようになります。

[![15 のイメージを表示する GridView の例のスクリーン ショット](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

動作を試し、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)と[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)それらのプロパティを調整することによって要素。 使用する代わりに、たとえば、 [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/)を使用してみて[ `SetAdjustViewBounds()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/)です。


## <a name="references"></a>参照

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).
