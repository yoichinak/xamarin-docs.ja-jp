---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 6409a4cb65186cade454253e1afdd38a66f3357f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028977"
---
# <a name="xamarinandroid-gridview"></a>Xamarin Android GridView

[`GridView`](xref:Android.Widget.GridView)は[`ViewGroup`](xref:Android.Views.ViewGroup)
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

この[`GridView`](xref:Android.Widget.GridView)は画面全体に表示されます。 属性は、わかりやすいものにします。 有効な属性の詳細については、 [`GridView`](xref:Android.Widget.GridView)リファレンスを参照してください。

`HelloGridView.cs` を開き、次のコードを[`OnCreate()`](xref:Android.App.Activity.OnCreate*)に挿入します。
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

コンテンツビューに対して**メインの axml**レイアウトが設定されると、 [`FindViewById`](xref:Android.App.Activity.FindViewById*)を使用してレイアウトから[`GridView`](xref:Android.Widget.GridView)がキャプチャされます。 [`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
次に、プロパティを使用して、グリッドに表示されるすべての項目のソースとして、カスタムアダプター (`ImageAdapter`) を設定します。 `ImageAdapter` は、次の手順で作成します。

グリッド内の項目がクリックされたときに操作を実行するには、匿名デリゲートが[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)イベントにサブスクライブされます。
選択した項目のインデックス位置 (0 から始まる) を表示する[`Toast`](xref:Android.Widget.Toast)を示します (実際のシナリオでは、その位置を使用して、他のタスクのイメージ全体を取得できます)。 .NET イベントの代わりに Java スタイルのリスナークラスを使用できることに注意してください。

サブクラス[`BaseAdapter`](xref:Android.Widget.BaseAdapter)を `ImageAdapter` という名前の新しいクラスを作成します。

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

まず、 [`BaseAdapter`](xref:Android.Widget.BaseAdapter)から継承されたいくつかの必須メソッドを実装します。 コンストラクターと[`Count`](xref:Android.Widget.BaseAdapter.Count)プロパティは、わかりやすいものです。 通常は、 [`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
は、アダプター内の指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 同様に、 [`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
は項目の行 id を返す必要がありますが、ここでは必要ありません。

最初に必要なメソッドは[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)です。
このメソッドは、新しい[`View`](xref:Android.Views.View)を作成します。
`ImageAdapter`に追加された各イメージ。 このが呼び出されると、 [`View`](xref:Android.Views.View)
が渡されます。これは通常、リサイクルされたオブジェクト (少なくとも一度呼び出された後) であるため、オブジェクトが null かどうかを確認します。 *Null の場合は、* [`ImageView`](xref:Android.Widget.ImageView)
は、イメージプレゼンテーションに必要なプロパティを使用してインスタンス化され、構成されます。

- &mdash;ビューの高さと幅を[`LayoutParams`](xref:Android.Views.View.LayoutParameters)設定します。これにより、描画のサイズに関係なく、各イメージのサイズが変更され、必要に応じて、これらのサイズに合わせてトリミングされます。

- [`SetScaleType()`](xref:Android.Widget.ImageView.SetScaleType*)は、必要に応じて、画像を中央にトリミングする必要があることを宣言します。

- [`SetPadding(int, int, int, int)`](xref:Android.Views.View.SetPadding*)は、すべての辺の埋め込みを定義します。 (画像の縦横比が異なる場合、画像が ImageView に与えられている寸法と一致しない場合、余白が小さくなると画像がトリミングされることに注意してください)。

[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)に渡された[`View`](xref:Android.Views.View)が null で*ない*場合は、ローカル[`ImageView`](xref:Android.Widget.ImageView)
は、リサイクルされた[`View`](xref:Android.Views.View)オブジェクトで初期化されます。

[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)の最後に
メソッドでは、メソッドに渡される `position` 整数を使用して、`thumbIds` 配列からイメージを選択します。この配列は[`ImageView`](xref:Android.Widget.ImageView)のイメージリソースとして設定されます。

残されているのは、描画リソースの `thumbIds` 配列を定義することだけです。

アプリケーションを実行します。 グリッドレイアウトは次のようになります。

[![例15個のイメージを表示している GridView のスクリーンショット](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

[`GridView`](xref:Android.Widget.GridView)の動作と[`ImageView`](xref:Android.Widget.ImageView)を試してみてください
要素のプロパティを調整します。 たとえば、 [`LayoutParams`](xref:Android.Views.View.LayoutParameters)を使用する代わりに[`SetAdjustViewBounds()`](xref:Android.Widget.ImageView.SetAdjustViewBounds*)を使用します。

## <a name="references"></a>関連項目

- [`GridView`](xref:Android.Widget.GridView)
- [`ImageView`](xref:Android.Widget.ImageView)
- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
