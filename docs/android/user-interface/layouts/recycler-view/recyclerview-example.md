---
title: 基本的な RecyclerView の例
description: RecyclerView の使用方法を示すサンプルアプリ。
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/30/2018
ms.openlocfilehash: 7266d13136dbfc858bc3c882cacb802d83c92f52
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028838"
---
# <a name="a-basic-recyclerview-example"></a>基本的な RecyclerView の例

一般的なアプリケーションで `RecyclerView` がどのように動作するかを理解するために、このトピックでは、`RecyclerView` を使用して写真の大きなコレクションを表示する簡単なコード例である[RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリについて説明します。 

[カードビューを使用して写真を表示する RecyclerView アプリの2つのスクリーンショットを![します](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer**は、 [CardView](~/android/user-interface/controls/card-view.md)を使用して、`RecyclerView` レイアウトに各写真項目を実装します。 `RecyclerView`のパフォーマンス上の利点があるため、このサンプルアプリでは、大きな写真のコレクションをすばやくすばやくスクロールできます。

### <a name="an-example-data-source"></a>データソースの例

このサンプルアプリでは、"フォトアルバム" データソース (`PhotoAlbum` クラスによって表されます) によって、項目の内容が `RecyclerView` 提供されます。
`PhotoAlbum` は、キャプション付きの写真のコレクションです。インスタンス化すると、準備が完了した32の写真のコレクションを取得できます。

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

`PhotoAlbum` の各フォトインスタンスは、イメージのリソース ID、`PhotoID`、キャプション文字列、`Caption`を読み取ることができるプロパティを公開します。 写真のコレクションは、各写真がインデクサーによってアクセスできるように整理されています。 たとえば、次のコード行は、コレクション内の10番目の写真のイメージリソース ID とキャプションにアクセスします。

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` には、コレクション内の最初の写真を、コレクション内の他の場所でランダムに選択された写真とスワップするために呼び出すことができる `RandomSwap` メソッドも用意されています。

```csharp
mPhotoAlbum.RandomSwap ();
```

`PhotoAlbum` の実装の詳細は `RecyclerView`を理解するためのものではないため、`PhotoAlbum` ソースコードはここには記載されていません。 `PhotoAlbum` するソースコードは、 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリの[PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs)で入手できます。

### <a name="layout-and-initialization"></a>レイアウトと初期化

レイアウトファイル (Main) は、`LinearLayout`内の1つの `RecyclerView` で構成され**ます**。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

`RecyclerView` はサポートライブラリにパッケージされているため、完全修飾名**RecyclerView**を使用する必要があることに注意してください。 `MainActivity` の `OnCreate` メソッドによって、このレイアウトが初期化され、アダプターがインスタンス化され、基になるデータソースが準備されます。

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

このコードでは、次のことを行います。

1. `PhotoAlbum` データソースをインスタンス化します。

2. フォトアルバムのデータソースをアダプターのコンストラクターに渡します (このガイドの後半で定義されている `PhotoAlbumAdapter`)。 
   データソースをパラメーターとしてアダプターのコンストラクターに渡すことがベストプラクティスと考えられることに注意してください。 

3. レイアウトから `RecyclerView` を取得します。

4. 前述のように `RecyclerView` `SetAdapter` メソッドを呼び出して、アダプターを `RecyclerView` インスタンスに接続します。

### <a name="layout-manager"></a>レイアウトマネージャー

`RecyclerView` 内の各項目は、写真の画像とフォトキャプションを含む `CardView` で構成されます (詳細については、以下の「[ビューホルダー](#view-holder) 」セクションで説明します)。 定義済みの `LinearLayoutManager` は、垂直方向のスクロールで各 `CardView` をレイアウトするために使用されます。

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

このコードは、メインアクティビティの `OnCreate` メソッドに存在します。 レイアウトマネージャーのコンストラクターには*コンテキスト*が必要なので、上記のように `this` を使用して `MainActivity` が渡されます。

定義済みの `LinearLayoutManager`を使用する代わりに、2つの `CardView` 項目を並べて表示するカスタムレイアウトマネージャーをプラグインして、写真のコレクションを走査するためのページめくりアニメーション効果を実装することができます。 このガイドの後半では、別のレイアウトマネージャーでスワップすることによってレイアウトを変更する方法の例を示します。

<a name="view-holder" />

### <a name="view-holder"></a>表示ホルダー

ビューホルダークラスは `PhotoViewHolder`と呼ばれます。 各 `PhotoViewHolder` インスタンスは、関連付けられた行アイテムの `ImageView` と `TextView` への参照を保持します。これは、次のように reservations として `CardView` にレイアウトされます。

[ImageView と TextView を含む CardView の![ダイアグラム](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` は `RecyclerView.ViewHolder` から派生し、上のレイアウトに示されている `ImageView` および `TextView` への参照を格納するプロパティを含みます。
`PhotoViewHolder` は、次の2つのプロパティと1つのコンストラクターで構成されます。

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

このコード例では、`PhotoViewHolder` コンストラクターに、`PhotoViewHolder` ラップする親項目ビュー (`CardView`) への参照が渡されます。 常に親項目ビューを基本コンストラクターに転送することに注意してください。 `PhotoViewHolder` コンストラクターは、親項目ビューで `FindViewById` を呼び出して、それぞれの子ビュー参照、`ImageView` および `TextView`を検索し、結果をそれぞれ `Image` および `Caption` プロパティに格納します。 アダプターは、この `CardView`の子ビューを新しいデータで更新するときに、これらのプロパティからビュー参照を取得します。

`RecyclerView.ViewHolder`の詳細については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)を参照してください。

### <a name="adapter"></a>アダプター

アダプターは、各 `RecyclerView` 行を特定の写真のデータと共に読み込みます。 たとえば、行位置*p*の特定の写真の場合、アダプターはデータソース内の位置*p*にある関連データを検索し、このデータを `RecyclerView` コレクションの位置*p*の行項目にコピーします。 アダプターはビューホルダーを使用して、その位置で `ImageView` および `TextView` の参照を検索します。これにより、ユーザーが写真コレクションをスクロールしてビューを再利用するときに、ビューに対して `FindViewById` を繰り返し呼び出す必要がなくなります。

**RecyclerViewer**では、アダプタークラスは `RecyclerView.Adapter` から派生して `PhotoAlbumAdapter`を作成します。

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

`mPhotoAlbum` メンバーには、コンストラクターに渡されるデータソース (フォトアルバム) が含まれています。コンストラクターは、このメンバー変数にフォトアルバムをコピーします。 次の必須 `RecyclerView.Adapter` メソッドが実装されています。

- **`OnCreateViewHolder`** &ndash; 項目レイアウトファイルとビューホルダーをインスタンス化します。

- **`OnBindViewHolder`** は、指定された位置にあるデータを、指定されたビューホルダーに格納されている参照を持つビューに読み込み &ndash; ます。

- **`ItemCount`** &ndash; は、データソース内の項目の数を返します。

レイアウトマネージャーは、`RecyclerView`内に項目を配置するときに、これらのメソッドを呼び出します。 これらのメソッドの実装については、次のセクションで説明します。

#### <a name="oncreateviewholder"></a>Oncreateの古い

レイアウトマネージャーは、`RecyclerView` が項目を表す新しいビューホルダーを必要とする場合に `OnCreateViewHolder` を呼び出します。 `OnCreateViewHolder`、ビューのレイアウトファイルから項目ビューを増えし、新しい `PhotoViewHolder` インスタンスにビューをラップします。 `PhotoViewHolder` のコンストラクターは、前の「[ビューホルダー](#view-holder)」で説明したように、レイアウト内の子ビューを検索し、その参照を格納します。

各行項目は、`ImageView` (写真の場合) と `TextView` (キャプションの場合) を含む `CardView` によって表されます。 このレイアウトは、 **PhotoCardView**ファイルにあります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

このレイアウトは、`RecyclerView`内の1つの行項目を表します。 `OnBindViewHolder` メソッド (以下を参照) は、データソースからこのレイアウトの `ImageView` と `TextView` にデータをコピーします。
このレイアウトを、`RecyclerView` 内の特定の写真の場所に対して増えし、新しい `PhotoViewHolder` インスタンスをインスタンス化します (関連付けられている `TextView` レイアウト内の `ImageView` および `CardView` の子ビューへの参照を検索してキャッシュします)。 `OnCreateViewHolder`:

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

生成されたビューホルダーインスタンス `vh`が、呼び出し元 (レイアウトマネージャー) に返されます。

#### <a name="onbindviewholder"></a>OnBindViewHolder

レイアウトマネージャーは、`RecyclerView`の表示される画面領域に特定のビューを表示する準備ができたら、アダプターの `OnBindViewHolder` メソッドを呼び出して、指定した行の位置にある項目にデータソースの内容を読み込みます。 `OnBindViewHolder` は、指定された行の位置 (写真の画像リソースと写真のキャプションの文字列) の写真情報を取得し、このデータを関連付けられたビューにコピーします。 ビューは、ビューホルダーオブジェクトに格納されている参照 (`holder` パラメーターを通じて渡される) を介して配置されます。

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

渡されたビューホルダーオブジェクトは、使用する前に、派生ビューの所有者の種類 (この場合は `PhotoViewHolder`) にキャストする必要があります。
アダプターは、ビューホルダーの `Image` プロパティによって参照されるビューにイメージリソースを読み込み、ビューホルダーの `Caption` プロパティによって参照されるビューにキャプションテキストをコピーします。 これにより、関連付けられたビューがそのデータに*バインド*されます。

`OnBindViewHolder` は、データの構造を直接処理するコードであることに注意してください。 この場合、`OnBindViewHolder` は、`RecyclerView` 項目の位置を、データソース内の関連付けられているデータ項目にマップする方法を認識します。 この場合のマッピングは簡単です。この場合、位置はフォトアルバムの配列インデックスとして使用できます。ただし、より複雑なデータソースでは、このようなマッピングを確立するために余分なコードが必要になる場合があります。

#### <a name="itemcount"></a>ItemCount

`ItemCount` メソッドは、データコレクション内の項目の数を返します。 フォトビューアーアプリの例では、項目数はフォトアルバムの写真の数です。

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

`RecyclerView.Adapter`の詳細については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)を参照してください。

### <a name="putting-it-all-together"></a>まとめ

この例の写真アプリの `RecyclerView` 実装は、データソース、レイアウトマネージャー、およびアダプターを作成する `MainActivity` のコードで構成されています。 `MainActivity` は、`mRecyclerView` インスタンスを作成し、データソースとアダプターをインスタンス化して、レイアウトマネージャーとアダプターに接続します。

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` はビュー参照を検索してキャッシュします。

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` は、次の3つの必須メソッドオーバーライドを実装します。

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

このコードをコンパイルして実行すると、次のスクリーンショットに示すように、基本的な写真表示アプリが作成されます。

[写真表示アプリの2つのスクリーンショットを![垂直方向にスクロールするフォトカードを使用する](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

影が描画されていない場合は (上のスクリーンショットを参照)、 **Properties/AndroidManifest**を編集し、次の属性設定を `<application>` 要素に追加します。

```xml
android:hardwareAccelerated="true"
```

この basic アプリでは、フォトアルバムの閲覧のみがサポートされています。 項目タッチイベントに応答せず、基になるデータの変更を処理しません。 この機能は[、RecyclerView の例を拡張することによって](~/android/user-interface/layouts/recycler-view/extending-the-example.md)追加されます。

### <a name="changing-the-layoutmanager"></a>LayoutManager の変更

`RecyclerView`の柔軟性があるため、別のレイアウトマネージャーを使用するようにアプリを変更するのは簡単です。 次の例では、垂直方向の線形レイアウトではなく水平方向にスクロールするグリッドレイアウトでフォトアルバムを表示するように変更されています。 これを行うには、次のように `GridLayoutManager` を使用するようにレイアウトマネージャーのインスタンス化を変更します。

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

このコード変更により、垂直 `LinearLayoutManager` が、水平方向にスクロールする2つの行で構成されるグリッドを表す `GridLayoutManager` に置き換えられます。 アプリをもう一度コンパイルして実行すると、写真がグリッドに表示され、垂直ではなく水平にスクロールしていることがわかります。

[グリッド内の写真を水平方向にスクロールするアプリのスクリーンショットの![例](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

1行のコードのみを変更することで、異なる動作を持つ別のレイアウトを使用するように写真表示アプリを変更することができます。
レイアウトスタイルを変更するために、アダプターコードもレイアウト XML も変更する必要がないことに注意してください。 

次のトピック「 [RecyclerView の拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)」の例では、この基本的なサンプルアプリを拡張して、項目クリックイベントを処理し、基になるデータソースが変更されたときに `RecyclerView` を更新します。

## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
