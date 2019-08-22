---
title: 基本的な RecyclerView の例
description: RecyclerView の使用方法を示すサンプルアプリ。
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/30/2018
ms.openlocfilehash: ca80dc9a064e81d9b81b1cd53237df818d409576
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887541"
---
# <a name="a-basic-recyclerview-example"></a>基本的な RecyclerView の例

一般的なアプリケーション`RecyclerView`での動作を理解するために、このトピックでは、を使用`RecyclerView`して写真の大きなコレクションを表示する簡単なコード例である[RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリについて説明します。 

[![カードビューを使用して写真を表示する RecyclerView アプリの2つのスクリーンショット](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer**は、 [CardView](~/android/user-interface/controls/card-view.md)を使用して、 `RecyclerView`レイアウトに各写真項目を実装します。 `RecyclerView`パフォーマンス上の利点があるため、このサンプルアプリでは、大きな写真のコレクションをすばやくスクロールできます。


### <a name="an-example-data-source"></a>データソースの例

このサンプルアプリでは、"フォトアルバム" データソース ( `PhotoAlbum`クラスによって表される) が項目の内容と共に提供`RecyclerView`されます。
`PhotoAlbum`キャプション付きの写真のコレクションです。インスタンス化すると、準備が完了した32の写真のコレクションを取得できます。

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

の`PhotoAlbum`各フォトインスタンスは、イメージのリソース ID、 `PhotoID`、およびそのキャプション文字列を`Caption`読み取ることができるプロパティを公開します。 写真のコレクションは、各写真がインデクサーによってアクセスできるように整理されています。 たとえば、次のコード行は、コレクション内の10番目の写真のイメージリソース ID とキャプションにアクセスします。

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum`また、に`RandomSwap`は、コレクション内の最初の写真を、コレクション内の他の場所でランダムに選択された写真とスワップするために呼び出すことができるメソッドも用意されています。

```csharp
mPhotoAlbum.RandomSwap ();
```

の実装の`PhotoAlbum`詳細は理解`RecyclerView`には関係がないため`PhotoAlbum` 、ソースコードはここには記載されていません。 のソースコード`PhotoAlbum`は、 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリの[PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs)で入手できます。


### <a name="layout-and-initialization"></a>レイアウトと初期化

レイアウトファイル (Main) は、内`LinearLayout`の1つ`RecyclerView`ので構成され**ます。**

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

はサポートライブラリにパッケージされているため `RecyclerView` 、完全修飾名 v7 を使用する必要があることに注意してください。 `OnCreate` の`MainActivity`メソッドは、このレイアウトを初期化し、アダプターをインスタンス化し、基になるデータソースを準備します。

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

1. データソース`PhotoAlbum`をインスタンス化します。

2. フォトアルバムのデータソースをアダプター `PhotoAlbumAdapter`のコンストラクターに渡します (このガイドの後半で定義されています)。 
   データソースをパラメーターとしてアダプターのコンストラクターに渡すことがベストプラクティスと考えられることに注意してください。 

3. `RecyclerView`レイアウトからを取得します。

4. 上に示すように`RecyclerView` 、メソッドを`RecyclerView` `SetAdapter`呼び出して、アダプターをインスタンスに接続します。

### <a name="layout-manager"></a>レイアウトマネージャー

の`RecyclerView`各項目は、写真`CardView`の画像とフォトキャプションを含むで構成されます (詳細については、以下の「[ビューホルダー](#view-holder) 」セクションで説明します)。 定義済み`LinearLayoutManager`のは、垂直方向の`CardView`スクロールで各をレイアウトするために使用されます。

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

このコードは、メインアクティビティの`OnCreate`メソッドに存在します。 レイアウトマネージャーのコンストラクターには*コンテキスト*が必要であるため`MainActivity` 、は上`this`に示すようにを使用して渡されます。

定義済み`LinearLayoutManager`のを使用する代わりに、2つ`CardView`の項目を並べて表示するカスタムレイアウトマネージャーをプラグインして、写真のコレクションを走査するためのページめくりアニメーション効果を実装することができます。 このガイドの後半では、別のレイアウトマネージャーでスワップすることによってレイアウトを変更する方法の例を示します。

<a name="view-holder" />

### <a name="view-holder"></a>表示ホルダー

ビューホルダークラスが呼び出さ`PhotoViewHolder`れます。 各`PhotoViewHolder`インスタンスは、関連付け`ImageView`られた行項目のおよび`TextView`に対する参照を保持し`CardView`ます。これは、ここで reservations としてレイアウトされます。

[![ImageView と TextView を含む CardView の図](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder`から`RecyclerView.ViewHolder`派生し、上のレイアウトに`TextView`表示さ`ImageView`れるへの参照を格納するプロパティを格納します。
`PhotoViewHolder`2つのプロパティと1つのコンストラクターで構成されます。

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

このコード例`PhotoViewHolder`では、をラップする`PhotoViewHolder`親項目ビュー `CardView`() への参照がコンストラクターに渡されます。 常に親項目ビューを基本コンストラクターに転送することに注意してください。 コンストラクター `PhotoViewHolder`は親`FindViewById`項目ビューでを呼び出して、それぞれの子ビュー参照を`ImageView`検索し`TextView`、それぞれの結果をプロパティ`Image`と`Caption`プロパティに格納します。 アダプターは、この`CardView`子ビューを新しいデータで更新するときに、これらのプロパティからビュー参照を取得します。

の詳細`RecyclerView.ViewHolder`については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)を参照してください。


### <a name="adapter"></a>アダプター

アダプターは、各行`RecyclerView`を特定の写真のデータと共に読み込みます。 たとえば、行位置*p*の特定の写真の場合、アダプターはデータソース内の位置*p*にある関連データを検索し、 `RecyclerView`コレクションの位置*p*にある行項目にこのデータをコピーします。 アダプターはビューの所有者を使用して、その`ImageView`位置`TextView`でとの参照を検索します。これ`FindViewById`により、ユーザーが写真コレクションをスクロールしてビューを再利用するときに、これらのビューを繰り返し呼び出す必要がなくなります。

**RecyclerViewer**では、アダプタークラスはから`RecyclerView.Adapter`派生して、次のものを作成`PhotoAlbumAdapter`します。

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

メンバー `mPhotoAlbum`には、コンストラクターに渡されるデータソース (フォトアルバム) が格納されます。コンストラクターは、このメンバー変数にフォトアルバムをコピーします。 次の必須`RecyclerView.Adapter`メソッドが実装されています。

- **`OnCreateViewHolder`** &ndash;項目レイアウトファイルとビューホルダーをインスタンス化します。

- **`OnBindViewHolder`** &ndash;指定したビューホルダーに参照が格納されているビューに、指定した位置にあるデータを読み込みます。

- **`ItemCount`** &ndash;データソース内の項目の数を返します。

レイアウトマネージャーは、内に項目を配置するときに、 `RecyclerView`これらのメソッドを呼び出します。 これらのメソッドの実装については、次のセクションで説明します。


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

`OnCreateViewHolder` が`RecyclerView`項目を表す新しいビューホルダーを必要とする場合、レイアウトマネージャーはを呼び出します。 `OnCreateViewHolder`ビューのレイアウトファイルから項目ビューを増えし、新しい`PhotoViewHolder`インスタンスにビューをラップします。 この`PhotoViewHolder`コンストラクターは、前の「[ビューホルダー](#view-holder)」で説明したように、レイアウト内の子ビューへの参照を検索して格納します。

各行項目は、 `CardView` `ImageView` (写真の場合) と`TextView` (キャプションの場合) を含むで表されます。 このレイアウトは、 **PhotoCardView**ファイルにあります。

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

このレイアウトは、 `RecyclerView`内の単一の行項目を表します。 ( `OnBindViewHolder`以下で説明する) メソッドは、データソースからこのレイアウト`ImageView`の`TextView`およびにデータをコピーします。
`OnCreateViewHolder`このレイアウトを内`RecyclerView`の特定の写真の場所に対して増えし、新しい`PhotoViewHolder`インスタンスをインスタンス化します`ImageView` ( `TextView`関連付けられている`CardView`レイアウトのおよび子ビューへの参照を検索してキャッシュします)。

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

結果のビューホルダーインスタンス`vh`は、呼び出し元 (レイアウトマネージャー) に返されます。


#### <a name="onbindviewholder"></a>OnBindViewHolder

レイアウトマネージャーは、表示されている画面領域に`RecyclerView`特定のビューを表示する準備ができたら、アダプターの`OnBindViewHolder`メソッドを呼び出して、指定した行の位置にある項目にデータソースの内容を読み込みます。 `OnBindViewHolder`指定した行の位置 (写真のイメージリソースと写真のキャプションの文字列) の写真情報を取得し、このデータを関連付けられたビューにコピーします。 ビューは、ビューホルダーオブジェクト (パラメーターを`holder`通じて渡されます) に格納されている参照によって検索されます。

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

渡されたビューホルダーオブジェクトは、使用する前に、派生ビューの所有者の種類 (この`PhotoViewHolder`場合は) にキャストする必要があります。
アダプターは、ビューホルダーの`Image`プロパティによって参照されるビューにイメージリソースを読み込み、ビューホルダーの`Caption`プロパティによって参照されるビューにキャプションテキストをコピーします。 これにより、関連付けられたビューがそのデータに*バインド*されます。

は、 `OnBindViewHolder`データの構造を直接処理するコードであることに注意してください。 この場合、は`OnBindViewHolder` 、データソース内の`RecyclerView`関連付けられているデータ項目に項目の位置をマップする方法を理解します。 この場合のマッピングは簡単です。この場合、位置はフォトアルバムの配列インデックスとして使用できます。ただし、より複雑なデータソースでは、このようなマッピングを確立するために余分なコードが必要になる場合があります。


#### <a name="itemcount"></a>ItemCount

メソッド`ItemCount`は、データコレクション内の項目の数を返します。 フォトビューアーアプリの例では、項目数はフォトアルバムの写真の数です。

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

の詳細`RecyclerView.Adapter`については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)を参照してください。


### <a name="putting-it-all-together"></a>まとめ

例の`RecyclerView`写真アプリの結果の実装は、 `MainActivity`データソース、レイアウトマネージャー、およびアダプターを作成するコードで構成されています。 `MainActivity``mRecyclerView`インスタンスを作成し、データソースとアダプターをインスタンス化して、レイアウトマネージャーとアダプターにプラグインします。

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

`PhotoViewHolder`ビュー参照を検索してキャッシュします。

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

`PhotoAlbumAdapter`は、次の3つの必須メソッドオーバーライドを実装します。

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

(上のスクリーンショットに示されているように) 影が描画されていない場合は、 **Properties/androidmanifest**を編集`<application>`し、要素に次の属性設定を追加します。

```xml
android:hardwareAccelerated="true"
```

この basic アプリでは、フォトアルバムの閲覧のみがサポートされています。 項目タッチイベントに応答せず、基になるデータの変更を処理しません。 この機能は[、RecyclerView の例を拡張することによって](~/android/user-interface/layouts/recycler-view/extending-the-example.md)追加されます。




### <a name="changing-the-layoutmanager"></a>LayoutManager の変更

`RecyclerView`柔軟性があるため、別のレイアウトマネージャーを使用するようにアプリを変更するのは簡単です。 次の例では、垂直方向の線形レイアウトではなく水平方向にスクロールするグリッドレイアウトでフォトアルバムを表示するように変更されています。 これを行うために、次の`GridLayoutManager`ようにを使用するようにレイアウトマネージャーのインスタンス化が変更されます。

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

このコード変更は`LinearLayoutManager` `GridLayoutManager` 、垂直方向を、水平方向にスクロールする2つの行で構成されるグリッドを表すに置き換えます。 アプリをもう一度コンパイルして実行すると、写真がグリッドに表示され、垂直ではなく水平にスクロールしていることがわかります。

[![グリッド内の写真を水平方向にスクロールするアプリのスクリーンショットの例](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

1行のコードのみを変更することで、異なる動作を持つ別のレイアウトを使用するように写真表示アプリを変更することができます。
レイアウトスタイルを変更するために、アダプターコードもレイアウト XML も変更する必要がないことに注意してください。 

次のトピックでは、 [RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)しています。この基本的なサンプルアプリは、項目`RecyclerView`クリックイベントを処理し、基になるデータソースが変更されたときに更新するように拡張されています。



## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
