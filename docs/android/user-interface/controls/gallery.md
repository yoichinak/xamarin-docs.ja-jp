---
title: Android ギャラリーコントロール
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 93eb7f98da6f3fe06f288eae5823f7173e58585f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029241"
---
# <a name="xamarinandroid-gallery-control"></a>Xamarin. Android ギャラリーコントロール

[`Gallery`](xref:Android.Widget.Gallery)は、水平スクロールリストに項目を表示し、現在の選択範囲をビューの中央に配置するために使用されるレイアウトウィジェットです。

> [!IMPORTANT]
> このウィジェットは、Android 4.1 (API レベル 16) で非推奨とされました。 

このチュートリアルでは、ギャラリー項目が選択されるたびに、写真のギャラリーを作成し、トーストメッセージを表示します。

コンテンツビューに `Main.axml` レイアウトを設定した後、`Gallery` は[`FindViewById`](xref:Android.App.Activity.FindViewById*)を使用してレイアウトからキャプチャされます。
[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
次に、プロパティを使用して、dallery に表示されるすべての項目のソースとして、カスタムアダプター (`ImageAdapter`) を設定します。 `ImageAdapter` は、次の手順で作成します。

ギャラリー内の項目がクリックされたときに操作を行うには、匿名デリゲートをサブスクライブして[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
イベント。 [`Toast`](xref:Android.Widget.Toast)が表示されます。
theselected item のインデックス位置 (0 から始まる) が表示されます (実際のシナリオでは、その位置を使用して、他のタスクのイメージ全体を取得できます)。

最初に、いくつかのメンバー変数があります。これには、描画リソースディレクトリ (**リソース/** 描画) に保存されたイメージを参照する id の配列が含まれます。

次はクラスコンストラクターです。ここでは、 [`Context`](xref:Android.Content.Context)
では、`ImageAdapter` インスタンスが定義され、ローカルフィールドに保存されます。
次に、 [`BaseAdapter`](xref:Android.Widget.BaseAdapter)から継承されたいくつかの必須メソッドを実装します。
コンストラクターと[`Count`](xref:Android.Widget.BaseAdapter.Count)
プロパティは自明です。 通常は、 [`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
は、アダプター内の指定した位置にある実際のオブジェクトを返す必要がありますが、この例では無視されます。 同様に、 [`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
は項目の行 id を返す必要がありますが、ここでは必要ありません。

メソッドは、イメージを[`ImageView`](xref:Android.Widget.ImageView)に適用する処理を実行します。
[`Gallery`](xref:Android.Widget.Gallery)に埋め込まれます。
このメソッドでは、メンバー [`Context`](xref:Android.Content.Context)
は、新しい[`ImageView`](xref:Android.Widget.ImageView)を作成するために使用されます。
[`ImageView`](xref:Android.Widget.ImageView)
は、作成されたリソースのローカル配列からイメージを適用して準備し、 [`Gallery.LayoutParams`](xref:Android.Widget.Gallery.LayoutParams)を設定します。
画像の高さと幅。 [`ImageView`](xref:Android.Widget.ImageView)に合わせてスケールを設定します。
最後に、コンストラクターで取得したスタイル設定可能な属性を使用するように背景を設定します。

その他のイメージスケーリングオプションについては、「 [`ImageView.ScaleType`](xref:Android.Widget.ImageView.ScaleType) 」を参照してください。

## <a name="walkthrough"></a>チュートリアル

"/" という*名前の新しい*プロジェクトを開始します。

[[新しいソリューション] ダイアログで新しい Android プロジェクトの![スクリーンショット](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

使用する写真を検索するか、[これらのサンプルイメージをダウンロード](https://developer.android.com/shareables/sample_images.zip)します。
イメージファイルをプロジェクトの**Resources/アブル**ディレクトリに追加します。 **プロパティ** ウィンドウで、それぞれの ビルド アクションを**Androidresource**に設定します。

**Resources/Layout/Main**を開き、次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

`MainActivity.cs` を開き、次のコードを[`OnCreate()`](xref:Android.App.Activity.OnCreate*)に挿入します。
b

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

サブクラス[`BaseAdapter`](xref:Android.Widget.BaseAdapter)を `ImageAdapter` という名前の新しいクラスを作成します。

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

アプリケーションを実行します。 次のスクリーンショットのようになります。

![サンプルイメージを表示している全画面のスクリーンショット](gallery-images/hellogallery3.png)

## <a name="references"></a>リファレンス

- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)
- [`Gallery`](xref:Android.Widget.Gallery)
- [`ImageView`](xref:Android.Widget.ImageView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
