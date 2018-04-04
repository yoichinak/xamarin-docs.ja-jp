---
title: RecyclerView 例を拡張します。
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 83147261a2d5458272f7e2bc105154da4308f4b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView 例を拡張します。


基本的なアプリの記載[A の基本的な RecyclerView 例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)実際にほとんど意味がありません&ndash;単にスクロールし、固定見やすくために写真の項目の一覧を表示します。 実際のアプリケーションで、ユーザーは表示内の項目をタップして、アプリを操作できると想定します。 また、基になるデータ ソースを変更することができます (または、アプリによって変更する)、表示内容をこれらの変更一貫性を維持する必要があります。 次のセクションで項目をクリックしてイベントを処理し、更新する方法を学習`RecyclerView`基になるデータ ソースの変更。


### <a name="handling-item-click-events"></a>アイテムのクリック イベントの処理

ユーザーが内の項目に触れたときに、`RecyclerView`項目の変更に関するアプリに通知する項目をクリックしてイベントを生成します。 このイベントは生成されません`RecyclerView` &ndash; (ビューの所有者でラップされて) います item ビューが代わりに、仕上げを検出し、イベントをクリックすると、これらの調整を報告します。

アイテムのクリック イベントを処理する方法を示すためには、次の手順は、どの写真は、ユーザーによって操作されたされていたレポートに基本的な写真表示アプリを変更する方法を説明します。 サンプル アプリでアイテムのクリック イベントの発生時に、次のシーケンスが実行されます。

1.  写真の`CardView`項目 click イベントを検出し、アダプターに通知します。

2.  アダプターは、アクティビティのアイテムのクリック ハンドラーに (項目の位置情報) を持つイベントを転送します。

3.  アクティビティのアイテムのクリック ハンドラー イベントに応答する項目をクリックします。

最初に、イベント ハンドラーのメンバーと呼ばれる`ItemClick`に追加、`PhotoAlbumAdapter`クラス定義。

```csharp
public event EventHandler<int> ItemClick;
```

項目をクリックしてイベント ハンドラー メソッドを次に、追加`MainActivity`です。
このハンドラーは、どの写真アイテムが影響を受けるかを示すトーストを一時的に表示されます。

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

コードの行が登録するために必要な次に、`OnItemClick`ハンドラー`PhotoAlbumAdapter`です。 これを行う点としては、直後に`PhotoAlbumAdapter`作成 (メイン アクティビティの`OnCreate`メソッド)。

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` 呼び出すようになりました`OnItemClick`項目をクリックしてイベントを受信するとします。 次の手順がさせ、このアダプターのハンドラーを作成するには`ItemClick`イベント。 次のメソッドでは、 `OnClick`、アダプターの直後に追加された`ItemCount`メソッド。

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

これは、`OnClick`メソッドは、アダプターの*リスナー*アイテムのビューからアイテムのクリック イベント用です。 このリスナーは、(アイテムのビューのビューの所有者) を使用して、アイテムのビューに登録できる前に、`PhotoViewHolder`コンス トラクターは、追加の引数としてこのメソッドをそのまま使用し、登録するように変更する必要があります`OnClick`item ビューと`Click`イベント。
ここでは、変更`PhotoViewHolder`コンス トラクター。

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView`パラメーターにはへの参照が含まれています、`CardView`ユーザーによって影響を受けることができます。 ビューの所有者の基本クラスに項目のレイアウト位置が知っていることに注意してください (`CardView`) それが表す (を使用して、`LayoutPosition`プロパティ)、この位置は、アダプターに渡されます`OnClick`メソッドは、アイテムのクリック イベントが発生するとします。 アダプターの`OnCreateViewHolder`メソッドは、アダプターを渡すように修正が`OnClick`メソッド ビュー所有者のコンス トラクターを。

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

ようになりましたビルド写真表示のサンプル アプリを実行すると、ディスプレイの写真をタップすると、どの写真が影響を受けるにレポートを表示するトースト。

[![フォト カードときに表示される例トーストをタップします。](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

この例でイベント ハンドラーを実装するための 1 つのアプローチ`RecyclerView`です。 ここで使用可能な方法は、ビューの所有者にイベントを設定して、これらのイベントに定期受信アダプターです。 別々 のイベントを必要とする場合は、写真のサンプル アプリでは、写真編集機能が提供される、`ImageView`と`TextView`各`CardView`: 接して、`TextView`を起動、`EditView`ユーザーが編集できるダイアログキャプション、およびで、`ImageView`ユーザーがトリミングし、写真を回転できるフォト補正ツールを起動します。 ニーズに応じて、アプリ、タッチ イベントに応答の処理と最適な方法を設計する必要があります。

示すためにどのように`RecyclerView`更新できる場合、データ セットの変更、写真表示のサンプル アプリは、ランダムにデータ ソースの写真を選択し、最初の写真とスワップするように変更できます。 最初に、**ランダムな選択**ボタンを追加する例のフォト アプリ**Main.axml**レイアウト。

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

メインのアクティビティの最後にコードを次に、追加`OnCreate`を探す方法、`Random Pick`レイアウト ボタンをクリックして、ハンドラーをアタッチします。

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

このハンドラーを呼び出すフォト アルバムの`RandomSwap`メソッドと、**ランダムな選択** ボタンをタップします。 `RandomSwap`メソッドにランダムにデータ ソース内の最初の写真と写真を交換し、ランダムにスワップ写真のインデックスを返します。 コンパイル、次のコード サンプル アプリを実行するをタップすると、**ランダムな選択**ボタンは行われません表示の変更であるため、`RecyclerView`は、データ ソースに対する変更を認識しません。

保持する`RecyclerView`、データ ソースが、変更後に更新、**ランダムな選択** をクリックを呼び出して、アダプターのハンドラーを変更する必要があります`NotifyItemChanged`変更されたコレクション内の各項目のメソッド (この場合、2 つの項目があります。変更された: 最初の写真、スワップの写真)。 これにより、`RecyclerView`データ ソースの新しい状態と一致するように、その表示を更新します。

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

これで、**ランダムな選択** ボタンをタップすると、`RecyclerView`こと、写真をさらにダウン コレクション内ではスワップ済みコレクション内の最初の写真を表示する表示を更新します。

[![実行前に、2 番目のスクリーン ショットをスワップした後の最初のスクリーン ショット](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

もちろん、`NotifyDataSetChanged`でしたが、2 つを呼び出すことではなく呼び出された`NotifyItemChanged`が実行を強制ため`RecyclerView`コレクション内の 2 つの項目が変更された場合でも、コレクション全体を更新します。 呼び出す`NotifyItemChanged`は呼び出し元よりも大幅に効率的`NotifyDataSetChanged`です。


## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView パーツおよび機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView の基本的な例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
