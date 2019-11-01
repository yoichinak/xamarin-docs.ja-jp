---
title: RecyclerView の例を拡張する
description: RecyclerView サンプルアプリに項目クリックイベントハンドラーを追加します。
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: d50971c1ef0064463e576a895729ad84577e1788
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028878"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView の例を拡張する

[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)で説明されている基本的なアプリは、実際には、スクロールして、参照を容易にするために写真項目の固定リストを表示する &ndash; あまりありません。 実際のアプリケーションでは、ユーザーはディスプレイの項目をタップすることで、アプリと対話できることを期待しています。 また、基になるデータソースが変更される (またはアプリによって変更される) 可能性があります。また、表示の内容は、これらの変更との整合性が維持されている必要があります。 以下のセクションでは、アイテムクリックイベントを処理する方法と、基になるデータソースが変更されたときに `RecyclerView` を更新する方法について説明します。

### <a name="handling-item-click-events"></a>項目クリックイベントの処理

ユーザーが `RecyclerView`の項目に触れると、項目クリックイベントが生成され、どの項目がタッチされたかをアプリに通知します。 このイベントは `RecyclerView` &ndash; によって生成されません。代わりに、項目ビュー (ビューの所有者にラップされています) が、クリックイベントとして接しているレポートを検出します。

項目クリックイベントを処理する方法を説明するために、次の手順では、ユーザーによってどの写真がタッチされたかをレポートするために、基本的な写真表示アプリがどのように変更されるかを説明します。 サンプルアプリで項目クリックイベントが発生すると、次のシーケンスが実行されます。

1. 写真の `CardView` は、項目クリックイベントを検出し、アダプターに通知します。

2. アダプターは、(項目の位置情報を含む) イベントをアクティビティの項目クリックハンドラーに転送します。

3. アクティビティの項目クリックハンドラーは、項目クリックイベントに応答します。

まず、`ItemClick` という名前のイベントハンドラーメンバーを `PhotoAlbumAdapter` クラス定義に追加します。

```csharp
public event EventHandler<int> ItemClick;
```

次に、項目クリックイベントハンドラーメソッドが `MainActivity`に追加されます。
このハンドラーは、どの写真項目がタッチされたかを示すトーストを簡単に表示します。

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

次に、`PhotoAlbumAdapter`に `OnItemClick` ハンドラーを登録するためのコード行が必要です。 `PhotoAlbumAdapter` が作成された直後にこれを行うことをお勧めします。 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

この基本的な例では、ハンドラーの登録はメインアクティビティの `OnCreate` メソッドで実行されますが、運用アプリでは `OnResume` にハンドラーを登録し、&ndash; `OnPause` で登録を解除することがあります。詳細については、[アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)を参照してください。

これで、`PhotoAlbumAdapter` は、項目クリックイベントを受信したときに `OnItemClick` を呼び出します。 次の手順では、この `ItemClick` イベントを発生させるハンドラーをアダプターに作成します。 次のメソッド `OnClick`は、アダプターの `ItemCount` メソッドの直後に追加されます。

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

この `OnClick` メソッドは、項目ビューの項目クリックイベントのアダプターの*リスナー*です。 このリスナーを項目ビューに登録する前に (項目ビューのビューホルダーを使用)、このメソッドを追加の引数として受け取るように `PhotoViewHolder` コンストラクターを変更し、項目ビュー `Click` イベントに `OnClick` を登録する必要があります。
次に、変更された `PhotoViewHolder` コンストラクターを示します。

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` パラメーターには、ユーザーがタッチした `CardView` への参照が含まれています。 ビューホルダーの基本クラスは、(`LayoutPosition` のプロパティを使用して) それが表すアイテム (`CardView`) のレイアウト位置を認識し、この位置は、項目クリックイベントが発生したときにアダプターの `OnClick` メソッドに渡されることに注意してください。 アダプターの `OnCreateViewHolder` メソッドは、アダプターの `OnClick` メソッドをビューホルダーのコンストラクターに渡すように変更されます。

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

これで、サンプルの写真表示アプリをビルドして実行すると、画面の写真をタップすると、タッチされた写真を報告するトーストが表示されます。

[フォトカードをタップしたときに表示されるトーストの![例](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

この例では、`RecyclerView`でイベントハンドラーを実装する方法の1つを示します。 ここで使用できるもう1つの方法は、イベントをビューホルダーに配置し、アダプターがこれらのイベントをサブスクライブするように設定することです。 サンプルの写真アプリで写真編集機能が提供されている場合は、`ImageView` と各 `CardView`内の `TextView` で個別のイベントが必要になります。 `TextView` に触れると、ユーザーがキャプションを編集できるようにするための `EditView` ダイアログが起動されます。そして、`ImageView` に触れて、ユーザーが写真をトリミングまたは回転できるようにするための写真補正ツールを起動します。 アプリのニーズに応じて、タッチイベントの処理と応答に最適な方法を設計する必要があります。

データセットが変更されたときに `RecyclerView` を更新する方法を示すために、サンプルの写真表示アプリを変更して、データソース内の写真をランダムに選択し、最初の写真と入れ替えることができます。 まず、**ランダムな選択**ボタンがサンプルのフォトアプリのメインの**axml**レイアウトに追加されます。

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

次に、メインアクティビティの `OnCreate` メソッドの最後にコードを追加して、レイアウト内の `Random Pick` ボタンを見つけ、ハンドラーをアタッチします。

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

このハンドラーは、**ランダム選択**ボタンがタップされたときに、フォトアルバムの `RandomSwap` メソッドを呼び出します。 `RandomSwap` メソッドは、データソース内の最初の写真と写真をランダムに交換し、ランダムに交換された写真のインデックスを返します。 このコードを使用してサンプルアプリをコンパイルして実行する場合、 **[ランダム選択]** ボタンをタップしても、データソースへの変更が認識されない `RecyclerView` ため、表示が変更されることはありません。

データソースが変更された後も `RecyclerView` 更新しないようにするには、**ランダム選択**クリックハンドラーを変更して、変更されたコレクション内の各項目に対してアダプターの `NotifyItemChanged` メソッドを呼び出す必要があります (この場合、2つの項目が変更されています。最初の写真と写真を交換した場合)。 これにより、`RecyclerView` によって、データソースの新しい状態と一致するように表示が更新されます。

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

これで、**ランダム選択**ボタンがタップされたときに、`RecyclerView` によってディスプレイが更新され、コレクション内のさらに下の写真がコレクションの最初の写真と交換されたことが示されます。

[スワップ前の最初のスクリーンショット![スワップ後の2番目のスクリーンショット](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

もちろん、`NotifyItemChanged`への2つの呼び出しを行うのではなく、`NotifyDataSetChanged` を呼び出すこともできましたが、コレクション内の項目が2つしか変更されていなくても、コレクション全体の更新が強制的に `RecyclerView` されます。 `NotifyItemChanged` の呼び出しは、`NotifyDataSetChanged`を呼び出すよりもはるかに効率的です。

## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
