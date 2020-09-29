---
title: 基本的な RecyclerView の例
description: RecyclerView の使用方法を示すサンプルアプリ。
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/30/2018
ms.openlocfilehash: d4a775d1e96fd6650623c2a151ae74b8b68ce0ac
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457082"
---
# <a name="a-basic-recyclerview-example"></a>基本的な RecyclerView の例

一般的なアプリケーションでの動作を理解するために `RecyclerView` 、このトピックでは、を使用して写真の大きなコレクションを表示する簡単なコード例である [RecyclerViewer](/samples/xamarin/monodroid-samples/android50-recyclerviewer) サンプルアプリについて説明し `RecyclerView` ます。 

[![カードビューを使用して写真を表示する RecyclerView アプリの2つのスクリーンショット](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** は、 [CardView](~/android/user-interface/controls/card-view.md) を使用して、レイアウトに各写真項目を実装し `RecyclerView` ます。 パフォーマンス上の利点があるため `RecyclerView` 、このサンプルアプリでは、大きな写真のコレクションをすばやくスクロールできます。

### <a name="an-example-data-source"></a>データソースの例

このサンプルアプリでは、"フォトアルバム" データソース (クラスによって表される `PhotoAlbum` ) が `RecyclerView` 項目の内容と共に提供されます。
`PhotoAlbum` キャプション付きの写真のコレクションです。インスタンス化すると、準備が完了した32の写真のコレクションを取得できます。

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

の各フォトインスタンスは、 `PhotoAlbum` イメージのリソース ID、 `PhotoID` 、およびそのキャプション文字列を読み取ることができるプロパティを公開し `Caption` ます。 写真のコレクションは、各写真がインデクサーによってアクセスできるように整理されています。 たとえば、次のコード行は、コレクション内の10番目の写真のイメージリソース ID とキャプションにアクセスします。

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` また、には、コレクション内の `RandomSwap` 最初の写真を、コレクション内の他の場所でランダムに選択された写真とスワップするために呼び出すことができるメソッドも用意されています。

```csharp
mPhotoAlbum.RandomSwap ();
```

の実装の詳細は `PhotoAlbum` 理解には関係がないため `RecyclerView` 、 `PhotoAlbum` ソースコードはここには記載されていません。 のソースコード `PhotoAlbum` は、 [RecyclerViewer](/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリの[PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs)で入手できます。

### <a name="layout-and-initialization"></a>レイアウトと初期化

レイアウトファイル (Main) は、内の1つので構成され**ます。** `RecyclerView` `LinearLayout`

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

**android.support.v7.widget.RecyclerView** `RecyclerView` はサポートライブラリにパッケージされているため、完全修飾名 v7 を使用する必要があることに注意してください。 `OnCreate`のメソッドは、 `MainActivity` このレイアウトを初期化し、アダプターをインスタンス化し、基になるデータソースを準備します。

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

このコードは、次の処理を実行します。

1. データソースをインスタンス化 `PhotoAlbum` します。

2. フォトアルバムのデータソースをアダプターのコンストラクターに渡し `PhotoAlbumAdapter` ます (このガイドの後半で定義されています)。 
   データソースをパラメーターとしてアダプターのコンストラクターに渡すことがベストプラクティスと考えられることに注意してください。 

3. レイアウトからを取得し `RecyclerView` ます。

4. `RecyclerView` `RecyclerView` 上に示すように、メソッドを呼び出して、アダプターをインスタンスに接続し `SetAdapter` ます。

### <a name="layout-manager"></a>レイアウトマネージャー

の各項目 `RecyclerView` は、 `CardView` 写真の画像とフォトキャプションを含むで構成されます (詳細については、以下の「 [ビューホルダー](#view-holder) 」セクションで説明します)。 定義済みのは、 `LinearLayoutManager` 垂直方向のスクロールで各をレイアウトするために使用され `CardView` ます。

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

このコードは、メインアクティビティのメソッドに存在 `OnCreate` します。 レイアウトマネージャーのコンストラクターには *コンテキスト*が必要であるため、は `MainActivity` 上に示すようにを使用して渡され `this` ます。

定義済みのを使用する代わりに、 `LinearLayoutManager` 2 つの項目を並べて表示するカスタムレイアウトマネージャーをプラグインして `CardView` 、写真のコレクションを走査するためのページめくりアニメーション効果を実装することができます。 このガイドの後半では、別のレイアウトマネージャーでスワップすることによってレイアウトを変更する方法の例を示します。

<a name="view-holder"></a>

### <a name="view-holder"></a>表示ホルダー

ビューホルダークラスが呼び出され `PhotoViewHolder` ます。 各 `PhotoViewHolder` インスタンスは、関連付けられた行項目のおよびに対する参照を保持します `ImageView` `TextView` 。これは、 `CardView` ここで reservations としてレイアウトされます。

[![ImageView と TextView を含む CardView の図](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` から派生 `RecyclerView.ViewHolder` し、 `ImageView` 上のレイアウトに表示されるへの参照を格納するプロパティを格納し `TextView` ます。
`PhotoViewHolder` 2つのプロパティと1つのコンストラクターで構成されます。

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

このコード例では、を `PhotoViewHolder` ラップする親項目ビュー () への参照がコンストラクターに渡され `CardView` `PhotoViewHolder` ます。 常に親項目ビューを基本コンストラクターに転送することに注意してください。 `PhotoViewHolder`コンストラクターは親項目ビューでを呼び出して、それぞれ `FindViewById` の子ビュー参照を検索 `ImageView` し、それぞれの結果を `TextView` `Image` プロパティとプロパティに格納し `Caption` ます。 アダプターは、この子ビューを新しいデータで更新するときに、これらのプロパティからビュー参照を取得し `CardView` ます。

の詳細については `RecyclerView.ViewHolder` 、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)を参照してください。

### <a name="adapter"></a>アダプター

アダプターは、各行を `RecyclerView` 特定の写真のデータと共に読み込みます。 たとえば、行位置 *p*の特定の写真の場合、アダプターはデータソース内の位置 *p* にある関連データを検索し、コレクションの位置 *p* にある行項目にこのデータをコピーし `RecyclerView` ます。 アダプターはビューの所有者を使用して、その位置でとの参照を検索し `ImageView` `TextView` `FindViewById` ます。これにより、ユーザーが写真コレクションをスクロールしてビューを再利用するときに、これらのビューを繰り返し呼び出す必要がなくなります。

**RecyclerViewer**では、アダプタークラスはから派生 `RecyclerView.Adapter` して、次のものを作成し `PhotoAlbumAdapter` ます。

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

メンバーには、 `mPhotoAlbum` コンストラクターに渡されるデータソース (フォトアルバム) が格納されます。コンストラクターは、このメンバー変数にフォトアルバムをコピーします。 次の必須 `RecyclerView.Adapter` メソッドが実装されています。

- **`OnCreateViewHolder`**&ndash;項目レイアウトファイルとビューホルダーをインスタンス化します。

- **`OnBindViewHolder`**&ndash;指定したビューホルダーに参照が格納されているビューに、指定した位置にあるデータを読み込みます。

- **`ItemCount`**&ndash;データソース内の項目の数を返します。

レイアウトマネージャーは、内に項目を配置するときに、これらのメソッドを呼び出し `RecyclerView` ます。 これらのメソッドの実装については、次のセクションで説明します。

#### <a name="oncreateviewholder"></a>Oncreateの古い

`OnCreateViewHolder`が `RecyclerView` 項目を表す新しいビューホルダーを必要とする場合、レイアウトマネージャーはを呼び出します。 `OnCreateViewHolder` ビューのレイアウトファイルから項目ビューを増えし、新しいインスタンスにビューをラップし `PhotoViewHolder` ます。 この `PhotoViewHolder` コンストラクターは、前の「 [ビューホルダー](#view-holder)」で説明したように、レイアウト内の子ビューへの参照を検索して格納します。

各行項目は、 `CardView` `ImageView` (写真の場合) と (キャプションの場合) を含むで表され `TextView` ます。 このレイアウトは、 **PhotoCardView**ファイルにあります。

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

このレイアウトは、内の単一の行項目を表し `RecyclerView` ます。 `OnBindViewHolder`(以下で説明する) メソッドは、データソースから `ImageView` このレイアウトのおよびにデータをコピーし `TextView` ます。
`OnCreateViewHolder` このレイアウトを内の特定の写真の場所に対して増え `RecyclerView` し、新しいインスタンスをインスタンス化し `PhotoViewHolder` `ImageView` ます (関連付けられているレイアウトのおよび子ビューへの参照を検索してキャッシュし `TextView` `CardView` ます)。

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

結果のビューホルダーインスタンスは、 `vh` 呼び出し元 (レイアウトマネージャー) に返されます。

#### <a name="onbindviewholder"></a>OnBindViewHolder

レイアウトマネージャーは、表示されている画面領域に特定のビューを表示する準備ができたら `RecyclerView` 、アダプターのメソッドを呼び出して、 `OnBindViewHolder` 指定した行の位置にある項目にデータソースの内容を読み込みます。 `OnBindViewHolder` 指定した行の位置 (写真のイメージリソースと写真のキャプションの文字列) の写真情報を取得し、このデータを関連付けられたビューにコピーします。 ビューは、ビューホルダーオブジェクト (パラメーターを通じて渡されます) に格納されている参照によって検索され `holder` ます。

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

渡されたビューホルダーオブジェクトは、使用する前に、派生ビューの所有者の種類 (この場合は) にキャストする必要があり `PhotoViewHolder` ます。
アダプターは、ビューホルダーのプロパティによって参照されるビューにイメージリソースを読み込み、 `Image` ビューホルダーのプロパティによって参照されるビューにキャプションテキストをコピーし `Caption` ます。 これにより、関連付けられたビューがそのデータに *バインド* されます。

は、 `OnBindViewHolder` データの構造を直接処理するコードであることに注意してください。 この場合、は、 `OnBindViewHolder` `RecyclerView` データソース内の関連付けられているデータ項目に項目の位置をマップする方法を理解します。 この場合のマッピングは簡単です。この場合、位置はフォトアルバムの配列インデックスとして使用できます。ただし、より複雑なデータソースでは、このようなマッピングを確立するために余分なコードが必要になる場合があります。

#### <a name="itemcount"></a>ItemCount

メソッドは、 `ItemCount` データコレクション内の項目の数を返します。 フォトビューアーアプリの例では、項目数はフォトアルバムの写真の数です。

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

の詳細については `RecyclerView.Adapter` 、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)を参照してください。

### <a name="putting-it-all-together"></a>まとめ

例の `RecyclerView` 写真アプリの結果の実装は、 `MainActivity` データソース、レイアウトマネージャー、およびアダプターを作成するコードで構成されています。 `MainActivity` インスタンスを作成し `mRecyclerView` 、データソースとアダプターをインスタンス化して、レイアウトマネージャーとアダプターにプラグインします。

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

`PhotoViewHolder` ビュー参照を検索してキャッシュします。

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

[![写真カードを垂直方向にスクロールする、写真表示アプリの2つのスクリーンショット](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

影が描画されていない場合は (上のスクリーンショットを参照)、[ **AndroidManifest.xmlプロパティ ** ] を編集し、次の属性設定を要素に追加し `<application>` ます。

```xml
android:hardwareAccelerated="true"
```

この basic アプリでは、フォトアルバムの閲覧のみがサポートされています。 項目タッチイベントに応答せず、基になるデータの変更を処理しません。 この機能は [、RecyclerView の例を拡張することによって](~/android/user-interface/layouts/recycler-view/extending-the-example.md)追加されます。

### <a name="changing-the-layoutmanager"></a>LayoutManager の変更

柔軟性があるため `RecyclerView` 、別のレイアウトマネージャーを使用するようにアプリを変更するのは簡単です。 次の例では、垂直方向の線形レイアウトではなく水平方向にスクロールするグリッドレイアウトでフォトアルバムを表示するように変更されています。 これを行うために、次のようにを使用するようにレイアウトマネージャーのインスタンス化が変更され `GridLayoutManager` ます。

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

このコード変更は、垂直 `LinearLayoutManager` 方向を、 `GridLayoutManager` 水平方向にスクロールする2つの行で構成されるグリッドを表すに置き換えます。 アプリをもう一度コンパイルして実行すると、写真がグリッドに表示され、垂直ではなく水平にスクロールしていることがわかります。

[![グリッド内の写真を水平方向にスクロールするアプリのスクリーンショットの例](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

1行のコードのみを変更することで、異なる動作を持つ別のレイアウトを使用するように写真表示アプリを変更することができます。
レイアウトスタイルを変更するために、アダプターコードもレイアウト XML も変更する必要がないことに注意してください。 

次のトピックでは、 [RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)しています。この基本的なサンプルアプリは、項目クリックイベントを処理し、 `RecyclerView` 基になるデータソースが変更されたときに更新するように拡張されています。

## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)