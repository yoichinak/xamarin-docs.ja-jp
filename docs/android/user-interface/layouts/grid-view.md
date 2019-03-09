---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 63164d90419f3a49d9eb52a52d02e05fbee43dbf
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667621"
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) は、 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
2 次元のスクロール可能なグリッドの項目が表示されます。 使用して、レイアウトをグリッド項目を挿入する自動的に、 [ `ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)します。

このチュートリアルでは、サムネイル画像のグリッドを作成します。 項目を選択すると、トースト メッセージには、イメージの位置が表示されます。

という名前の新しいプロジェクトを開始**HelloGridView**します。

を使用するにはいくつかの写真を検索または[これらのサンプル イメージをダウンロード](https://developer.android.com/shareables/sample_images.zip)します。 イメージ ファイルをプロジェクトの追加**リソース/ディスプレイ**ディレクトリ。 **プロパティ**ごとにビルド アクション ウィンドウで、設定**AndroidResource**します。

開く、 **Resources/Layout/Main.axml**ファイルを開き、次を挿入します。

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

これは、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)画面全体を入力します。 属性ではなくわかりやすいです。 有効な属性の詳細については、次を参照してください。、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)参照。

開いている`HelloGridView.cs`の次のコードを挿入し、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
方法:

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

後に、 **Main.axml**コンテンツ ビューのレイアウトが設定されて、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)をレイアウトからキャプチャされた[ `FindViewById`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/)します。 、 [`Adapter`](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)
プロパティは、カスタム アダプターの設定を使用し (`ImageAdapter`)、グリッドに表示されるすべての項目のソースとして。 `ImageAdapter`が次の手順で作成されます。

匿名デリゲートの購読を行うには何か、グリッド内の項目がクリックされたときに、 [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)イベント。
表示、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)選択された項目のインデックス位置 (0 から始まる) を表示する (実際のシナリオで、位置される可能性があります他のタスクの完全な画像のサイズを取得する)。 .NET イベントではなく、リスナーの Java スタイル クラスを使用できることに注意してください。

という新しいクラスを作成`ImageAdapter`をサブクラスとして持つ[ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

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

継承されるいくつか必要なメソッドを最初に、この実装[ `BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)します。 コンス トラクターと[ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/)プロパティにはひとめでわかります。 通常であれば [`GetItem(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/)
アダプターで、指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 同様に、 [`GetItemId(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/)
項目の行の id を返す必要がありますが、ここで必要はありません。

必要な最初のメソッドは[ `GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)します。
このメソッドは、新規作成します [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
各イメージに追加の`ImageAdapter`します。 これが呼び出されると、 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
渡される、これは通常、再利用されるオブジェクト (少なくとも 1 回この呼び出された後)、オブジェクトが null かどうかをチェックがあるため、します。 場合、*は*null の場合、 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
インスタンス化され、イメージのプレゼンテーションの必要なプロパティで構成されています。

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) ビューの高さと幅を設定する&mdash;これにより、drawable のサイズに関係なく各イメージはサイズを変更し、必要に応じて、これらのディメンションに収まるようにトリミングされます。

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) (必要な) 場合に、イメージが中央にトリミングされることを宣言します。

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) 上下左右の余白を定義します。 (、イメージの縦横比が異なる場合は、し、少ないスペースにより、ImageView に指定されたディメンションが一致しない場合は、イメージの詳細トリミングするために注意してください)。

場合、 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)に渡される[ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)は*いない*null の場合、次に、ローカル [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
初期化されると、リサイクル[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)オブジェクト。

最後に、 [`GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)
メソッド、`position`からイメージを選択するメソッドに渡される整数が使用される、`thumbIds`のイメージ リソースとして設定されて、配列、 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)。

あとは、定義する、`thumbIds`ドローアブル リソースの配列。

アプリケーションを実行します。 グリッド レイアウトは、次のようになります。

[![15 のイメージを表示する GridView の例のスクリーン ショット](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

動作を試し、 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/)と [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
そのプロパティを調整することによって要素。 使用する代わりに、たとえば、 [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/)を使用してお試しください[ `SetAdjustViewBounds()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/)します。


## <a name="references"></a>関連項目

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
