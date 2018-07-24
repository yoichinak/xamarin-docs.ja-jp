---
title: RecyclerView の例を拡張
description: RecyclerView の例のアプリには、項目クリック イベント ハンドラーを追加します。
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 73c14e76a4a65c73c5fe0cc3d43329a9f4965c74
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038522"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView の例を拡張


説明されている基本的なアプリ[RecyclerView の例の基本的な A](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)実際にはほとんど意味がありません&ndash;単にスクロールし、見やすくために、写真アイテムの固定リストを表示します。 実際のアプリケーションでユーザーは表示内の項目をタップするアプリと対話できると想定します。 また、基になるデータ ソースを変更することができます (または、アプリによって変更される) と、表示内容をこれらの変更一貫性を維持する必要があります。 次のセクションでアイテムのクリック イベントを処理し、更新する方法が学習`RecyclerView`基になるデータ ソースの変更。


### <a name="handling-item-click-events"></a>項目のクリック イベントの処理

ユーザーが内の項目に触れたときに、`RecyclerView`項目のタッチなのか、アプリに通知する項目のクリック イベントが生成されます。 によってこのイベントが生成されない`RecyclerView`&ndash;項目ビュー (ビューの所有者にラップされています) が代わりに、タッチを検出し、イベントをクリックすると、これらのしあげを報告します。

項目のクリック イベントを処理する方法を示すためには、次の手順では、どの写真は、ユーザーがタッチされたがレポートに基本のフォト ビューアー アプリを変更する方法について説明します。 項目のクリック イベントが発生するサンプル アプリでは、次のシーケンスが行われます。

1.  写真の`CardView`項目クリック イベントを検出し、アダプターに通知します。

2.  アダプターは、アクティビティの項目のクリック ハンドラーに (項目の位置情報) を持つイベントを転送します。

3.  アクティビティの項目のクリック ハンドラーは、項目クリック イベントに応答します。

最初に、イベント ハンドラー メンバーと呼ばれる`ItemClick`に追加されます、`PhotoAlbumAdapter`クラスの定義。

```csharp
public event EventHandler<int> ItemClick;
```

項目のクリック イベント ハンドラー メソッドを次に、追加`MainActivity`します。
このハンドラーは、どの写真アイテムの変更を示すトーストを一時的に表示されます。

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

コード行が登録するために必要な次に、`OnItemClick`ハンドラーで`PhotoAlbumAdapter`します。 これを行う点としては、直後に`PhotoAlbumAdapter`が作成されます。 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

この基本的な例では、ハンドラーの登録が、メイン アクティビティの発生`OnCreate`メソッドが、運用アプリでハンドラーを登録可能性があります`OnResume`で登録を解除し、 `OnPause` &ndash;を参照してください[アクティビティのライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md)詳細についてはします。

`PhotoAlbumAdapter` 呼び出すようになりました`OnItemClick`項目のクリック イベントを受信したとき。 次の手順が発生させ、このアダプターのハンドラーを作成するには`ItemClick`イベント。 次のメソッドでは、 `OnClick`、すぐ後に、アダプターの追加は`ItemCount`メソッド。

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

これは、`OnClick`メソッドは、アダプターの*リスナー*アイテム ビューからイベントを項目 をクリックします。 このリスナーは、(項目ビューのビュー所有者) を使用して項目のビューに登録できる前に、`PhotoViewHolder`コンス トラクターは、追加の引数としてこのメソッドをそのまま使用し、登録するように変更する必要があります`OnClick`項目ビュー`Click`イベント。
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

`itemView`パラメーターにはへの参照が含まれています、`CardView`ですが、タッチ ユーザーしました。 ビューの所有者の基本クラスに項目のレイアウト位置が知っていることに注意してください (`CardView`) それが表す (を使用して、`LayoutPosition`プロパティ)、この位置は、アダプターに渡されると`OnClick`メソッドを項目のクリック イベントが発生しました。 アダプターの`OnCreateViewHolder`アダプターを渡すメソッドが変更された`OnClick`メソッド ビューの所有者のコンス トラクターに。

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

ようになりましたビルドして、サンプルのフォト ビューアー アプリを実行するときに、表示内の写真をタップしてにより、トーストを表示する写真の変更を報告します。

[![写真のカードのときに表示される例のトーストがタップされました。](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

この例でイベント ハンドラーを実装する方法の 1 つだけ`RecyclerView`です。 ここで使用できる方法は、ビューの所有者にイベントを配置し、これらのイベントを定期受信アダプターです。 サンプルのフォト アプリに、写真編集機能が提供されている場合は、別々 のイベントが必要とする、`ImageView`と`TextView`それぞれ`CardView`: に触れています、`TextView`起動、`EditView`ユーザーが編集できるダイアログキャプション、およびのしあげ、`ImageView`ユーザーがトリミングし、写真を回転できる写真補正ツールを起動します。 アプリのニーズに応じて処理と、タッチ イベントに応答に最適な方法を設計する必要があります。

示すためにどのように`RecyclerView`を更新できる場合、ランダムにデータ ソース内の写真を選択し、最初の写真とスワップするデータ セットの変更、サンプルのフォト ビューアー アプリを変更できます。 最初に、**ランダム選択**ボタンを追加する例のフォト アプリ**Main.axml**レイアウト。

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

メイン アクティビティの最後にコードを次に、追加`OnCreate`を検索するメソッド、`Random Pick`レイアウトでボタンをクリックし、ハンドラーをアタッチします。

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

このハンドラーの呼び出し、フォト アルバムの`RandomSwap`メソッドと、**ランダム選択** ボタンをタップします。 `RandomSwap`メソッドは、ランダムにデータ ソース内の最初の写真と写真を交換し、ランダムにスワップ写真のインデックスを返します。 コンパイルして、このコードでサンプル アプリを実行すると、タップ、**ランダム選択**ために、表示の変更でボタンが発生しない、`RecyclerView`はデータ ソースへの変更を認識しません。

保持する`RecyclerView`後に、データ ソースの変更、更新、**ランダム選択** をクリックを呼び出して、アダプターのハンドラーを変更する必要があります`NotifyItemChanged`変更されたコレクション内の各項目のメソッド (この場合、2 つの項目が変更: 最初の写真と写真をスワップした)。 これにより、`RecyclerView`データ ソースの新しい状態と一致するように、その表示を更新します。

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

今すぐ、**ランダム選択** ボタンをタップすると、`RecyclerView`ことさらに写真をコレクション内とスワップされ、コレクション内の最初の写真を表示する表示を更新します。

[![スワップ後に 2 つ目のスクリーン ショットをスワップする前に最初のスクリーン ショット](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

もちろん、`NotifyDataSetChanged`でしたが、2 つの呼び出しではなく、呼び出された`NotifyItemChanged`が実行のため強制実行は`RecyclerView`をコレクション内の 2 つの項目が変更された場合でも、コレクション全体を更新します。 呼び出す`NotifyItemChanged`呼び出すよりも大幅に効率が`NotifyDataSetChanged`します。


## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
