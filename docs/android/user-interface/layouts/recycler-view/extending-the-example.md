---
title: RecyclerView の例を拡張する
description: RecyclerView サンプルアプリに項目クリックイベントハンドラーを追加します。
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: fd813427836b0250b84941eca54d6bbe6219518e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645330"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView の例を拡張する


[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)で説明されている基本的な&ndash;アプリでは、実際にはスクロールするだけではなく、写真項目の固定リストが表示されます。 実際のアプリケーションでは、ユーザーはディスプレイの項目をタップすることで、アプリと対話できることを期待しています。 また、基になるデータソースが変更される (またはアプリによって変更される) 可能性があります。また、表示の内容は、これらの変更との整合性が維持されている必要があります。 以下のセクションでは、アイテムクリックイベントを処理し、基になるデータ`RecyclerView`ソースが変更されたときに更新する方法について説明します。


### <a name="handling-item-click-events"></a>項目クリックイベントの処理

ユーザーが内の`RecyclerView`項目に触れると、項目クリックイベントが生成され、どの項目がタッチされたかをアプリに通知します。 このイベントは、代わりにに`RecyclerView`よって生成されるのではなく、項目ビュー (ビューの所有者にラップされています) が、クリックイベントとして接していること&ndash;を検出します。

項目クリックイベントを処理する方法を説明するために、次の手順では、ユーザーによってどの写真がタッチされたかをレポートするために、基本的な写真表示アプリがどのように変更されるかを説明します。 サンプルアプリで項目クリックイベントが発生すると、次のシーケンスが実行されます。

1.  写真の`CardView`は、項目クリックイベントを検出し、アダプターに通知します。

2.  アダプターは、(項目の位置情報を含む) イベントをアクティビティの項目クリックハンドラーに転送します。

3.  アクティビティの項目クリックハンドラーは、項目クリックイベントに応答します。

最初に、という名前`ItemClick`のイベントハンドラーメンバーが`PhotoAlbumAdapter`クラス定義に追加されます。

```csharp
public event EventHandler<int> ItemClick;
```

次に、項目クリックイベントハンドラーメソッドがに`MainActivity`追加されます。
このハンドラーは、どの写真項目がタッチされたかを示すトーストを簡単に表示します。

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

次に、 `OnItemClick`ハンドラーをに`PhotoAlbumAdapter`登録するためのコード行が必要です。 この作業は、 `PhotoAlbumAdapter`の作成直後に行うことをお勧めします。 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

この基本的な例では、ハンドラーの登録はメインアクティビティの`OnCreate`メソッドで行われますが、運用アプリ&ndash; `OnResume` `OnPause`では、アクティビティの[ライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)を参照してください。参照.

`PhotoAlbumAdapter`は、項目`OnItemClick`クリックイベントを受け取ったときにを呼び出します。 次の手順では、この`ItemClick`イベントを発生させるハンドラーをアダプターに作成します。 次のメソッド`OnClick`は、アダプターの`ItemCount`メソッドの直後に追加されます。

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

この`OnClick`メソッドは、項目ビューの項目クリックイベントのアダプターの*リスナー*です。 このリスナーを項目ビューに登録する前に (項目ビューのビューホルダーを使用)、 `PhotoViewHolder`このメソッドを追加の引数として受け取るようにコンストラクターを変更し、項目ビュー `Click`イベントに登録`OnClick`する必要があります。
変更`PhotoViewHolder`されたコンストラクターを次に示します。

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

パラメーターには、ユーザーがタッチ`CardView`したへの参照が含まれています。 `itemView` ビューホルダーの基本クラスは、(`CardView` `LayoutPosition`プロパティを使用して) それが表す項目 () のレイアウト位置を認識し、項目クリックイベントが発生した`OnClick`ときに、この位置がアダプターのメソッドに渡されることに注意してください。 アダプターのメソッドは、アダプターの`OnClick`メソッドをビューホルダーのコンストラクターに渡すように変更されます。 `OnCreateViewHolder`

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

これで、サンプルの写真表示アプリをビルドして実行すると、画面の写真をタップすると、タッチされた写真を報告するトーストが表示されます。

[![フォトカードをタップしたときに表示されるトーストの例](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

この例では、を使用して`RecyclerView`イベントハンドラーを実装する方法を1つだけ示します。 ここで使用できるもう1つの方法は、イベントをビューホルダーに配置し、アダプターがこれらのイベントをサブスクライブするように設定することです。 サンプルの写真アプリで写真編集機能が提供されている場合`ImageView`は、 `TextView`との間`CardView`で個別のイベントが必要になります`EditView` 。では、ユーザーが編集できるダイアログが起動されます。 `TextView`キャプションは、に`ImageView`触れて、ユーザーが写真をトリミングまたは回転できるようにするための写真補正ツールを起動します。 アプリのニーズに応じて、タッチイベントの処理と応答に最適な方法を設計する必要があります。

データセットが`RecyclerView`変更されたときにを更新する方法を示すために、サンプルの写真表示アプリを変更して、データソース内の写真をランダムに選択し、最初の写真と入れ替えることができます。 まず、**ランダムな選択**ボタンがサンプルのフォトアプリのメインの**axml**レイアウトに追加されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

次に、メインアクティビティの`OnCreate`メソッドの最後にコードを追加してレイアウト内の`Random Pick`ボタンを見つけ、ハンドラーをアタッチします。

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

このハンドラーは、**ランダム選択**ボタン`RandomSwap`がタップされたときに、フォトアルバムのメソッドを呼び出します。 この`RandomSwap`メソッドは、データソース内の最初の写真と写真をランダムに交換し、ランダムにスワップされた写真のインデックスを返します。 このコードを使用してサンプルアプリをコンパイルして実行する場合、 `RecyclerView`がデータソースへの変更を認識しないため、 **[ランダム選択]** ボタンをタップしても、表示が変更されることはありません。

データソース`RecyclerView`が変更された後も更新を維持するには、**ランダム選択**のクリックハンドラーを`NotifyItemChanged`変更して、変更されたコレクション内の各項目に対してアダプターのメソッドを呼び出す必要があります (この場合、2つの項目が変更されています。最初の写真とスワップされた写真)。 これに`RecyclerView`より、は、データソースの新しい状態と一貫性があるように表示を更新します。

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

これで、**ランダム選択**ボタンがタップされ`RecyclerView`たときに、コレクション内の下にある写真がコレクション内の最初の写真とスワップされたことを示す表示を更新します。

[![スワップ前の最初のスクリーンショット、スワップ後の2番目のスクリーンショット](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

もちろん、2 `NotifyDataSetChanged`つの`NotifyItemChanged`呼び出しを行うのではなく、を呼び出すこともできます`RecyclerView`が、コレクション内の項目が2つしか変更されていなくても、はコレクション全体を強制的に更新します。 の呼び出しは、を呼び出す`NotifyDataSetChanged`よりもはるかに効率的です。 `NotifyItemChanged`


## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
