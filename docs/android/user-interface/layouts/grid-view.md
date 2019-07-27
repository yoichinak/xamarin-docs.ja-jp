---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: a1bcb83d6057cb7d4a43c510d7b5805b574812e6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510063"
---
# <a name="xamarinandroid-gridview"></a>Xamarin Android GridView

[`GridView`](xref:Android.Widget.GridView)はです。[`ViewGroup`](xref:Android.Views.ViewGroup)
2次元のスクロール可能なグリッドに項目が表示されます。 グリッド項目は、 [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)を使用してレイアウトに自動的に挿入されます。

このチュートリアルでは、イメージサムネイルのグリッドを作成します。 項目を選択すると、トーストメッセージに画像の位置が表示されます。

**HelloGridView**という名前の新しいプロジェクトを開始します。

使用する写真を検索するか、[これらのサンプルイメージをダウンロード](https://developer.android.com/shareables/sample_images.zip)します。 イメージファイルをプロジェクトの**Resources/アブル**ディレクトリに追加します。 **プロパティ** ウィンドウで、それぞれの ビルド アクションを**Androidresource**に設定します。

**Resources/Layout/Main. axml**ファイルを開き、次のように挿入します。

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

これ[`GridView`](xref:Android.Widget.GridView)により、画面全体が表示されます。 属性は、わかりやすいものにします。 有効な属性の詳細については[`GridView`](xref:Android.Widget.GridView) 、「」を参照してください。

を`HelloGridView.cs`開き、次のコードを挿入します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

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

コンテンツビュー [`GridView`](xref:Android.Widget.GridView)に対して**メインの axml**レイアウトを設定した後、を使用[`FindViewById`](xref:Android.App.Activity.FindViewById*)してレイアウトからがキャプチャされます。 、[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
次に、プロパティを使用して、グリッド`ImageAdapter`に表示されるすべての項目のソースとしてカスタムアダプター () を設定します。 は`ImageAdapter` 、次の手順で作成します。

グリッド内の項目がクリックされたときに何かを実行するために、 [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)匿名デリゲートがイベントにサブスクライブされます。
これには[`Toast`](xref:Android.Widget.Toast) 、選択した項目のインデックス位置 (0 から始まる) を表示するが表示されます (実際のシナリオでは、その位置を使用して、他のタスクのイメージ全体のサイズを取得できます)。 .NET イベントの代わりに Java スタイルのリスナークラスを使用できることに注意してください。

サブクラス`ImageAdapter` [`BaseAdapter`](xref:Android.Widget.BaseAdapter)と呼ばれる新しいクラスを作成します。

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

最初に、から継承されたいくつ[`BaseAdapter`](xref:Android.Widget.BaseAdapter)かの必須メソッドを実装します。 コンストラクターと[`Count`](xref:Android.Widget.BaseAdapter.Count)プロパティは、自明です。 通常であれば[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
は、アダプター内の指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 に[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
は項目の行 id を返す必要がありますが、ここでは必要ありません。

最初に必要なメソッド[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)はです。
このメソッドは、新しいを作成します。[`View`](xref:Android.Views.View)
に追加されたイメージ`ImageAdapter`ごとに。 このが呼び出されると、[`View`](xref:Android.Views.View)
が渡されます。これは通常、リサイクルされたオブジェクト (少なくとも一度呼び出された後) であるため、オブジェクトが null かどうかを確認します。 *Null の*場合、[`ImageView`](xref:Android.Widget.ImageView)
は、イメージプレゼンテーションに必要なプロパティを使用してインスタンス化され、構成されます。

- [`LayoutParams`](xref:Android.Views.View.LayoutParameters)ビュー&mdash;の高さと幅を設定します。これにより、描画のサイズに関係なく、各イメージのサイズが変更され、必要に応じてトリミングされます。

- [`SetScaleType()`](xref:Android.Widget.ImageView.SetScaleType*)必要に応じて、イメージを中心方向にトリミングする必要があることを宣言します。

- [`SetPadding(int, int, int, int)`](xref:Android.Views.View.SetPadding*)すべての辺の埋め込みを定義します。 (画像の縦横比が異なる場合、画像が ImageView に与えられている寸法と一致しない場合、余白が小さくなると画像がトリミングされることに注意してください)。

[`View`](xref:Android.Views.View)に[渡さ`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)れたが null で*ない*場合は、ローカルの[`ImageView`](xref:Android.Widget.ImageView)
は、リサイクル[`View`](xref:Android.Views.View)されたオブジェクトで初期化されます。

の最後にあります。[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)
メソッドは、メソッドに渡された`thumbIds` [`ImageView`](xref:Android.Widget.ImageView)整数を使用して、配列からイメージを選択します。この配列は、のイメージリソースとして設定`position`されます。

残されているのは、 `thumbIds`描画できるリソースの配列を定義することだけです。

アプリケーションを実行します。 グリッドレイアウトは次のようになります。

[![15個の画像を表示している GridView のスクリーンショットの例](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

[`GridView`](xref:Android.Widget.GridView)との動作を試してみてください。[`ImageView`](xref:Android.Widget.ImageView)
要素のプロパティを調整します。 たとえば、を使用する代わり[`LayoutParams`](xref:Android.Views.View.LayoutParameters)にを[`SetAdjustViewBounds()`](xref:Android.Widget.ImageView.SetAdjustViewBounds*)使用します。

## <a name="references"></a>リファレンス

- [`GridView`](xref:Android.Widget.GridView)
- [`ImageView`](xref:Android.Widget.ImageView)
- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*
され、[*Creative Commons 2.5 属性*](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。
