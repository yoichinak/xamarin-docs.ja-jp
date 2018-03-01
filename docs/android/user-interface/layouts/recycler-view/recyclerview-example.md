---
title: "RecyclerView の基本的な例"
ms.topic: article
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: a8de515563d9b9e38f049fd92c94b95e75239eb2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="a-basic-recyclerview-example"></a>RecyclerView の基本的な例


理解しておく方法`RecyclerView`一般的なアプリケーションでは、このトピックについて説明、 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/)サンプル アプリケーション、シンプルなコード例を使用する`RecyclerView`多数の写真を表示します。 

[ ![CardViews を使用して写真を表示する RecyclerView アプリの 2 つのスクリーン ショット](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png)

**RecyclerViewer**使用[CardView](~/android/user-interface/controls/card-view.md)内の各写真項目を実装する、`RecyclerView`レイアウトです。 ため`RecyclerView`のパフォーマンスの利点があります、このサンプル アプリは迅速に円滑かつ大きな遅延なしに、多数の写真をスクロールできるようです。

<a name="datasource" />

### <a name="an-example-data-source"></a>例のデータ ソース

この例のアプリ「アルバム」データ ソースで (によって表される、`PhotoAlbum`クラス) を提供`RecyclerView`項目の内容にします。
`PhotoAlbum` キャプションと写真のコレクションです。インスタンスを作成する場合は、32 の写真の既製のコレクションを取得します。

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

写真の各インスタンスに`PhotoAlbum`その画像リソース ID を取得するプロパティを公開`PhotoID`、およびそのキャプション文字列`Caption`です。 写真のコレクションは、それぞれの写真は、インデクサーによってアクセスできるように構成されています。 たとえば、次のコード行は、画像リソース ID とコレクション内の 10 番目の写真のキャプションをアクセスします。

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` 用意されています、`RandomSwap`メソッドを呼び出すことができるコレクションの他の場所でランダムに選択された写真のコレクションで最初の写真をスワップします。

```csharp
mPhotoAlbum.RandomSwap ();
```

の実装の詳細`PhotoAlbum`理解するには関係ない`RecyclerView`、`PhotoAlbum`ソース コードがここに示されていません。 ソース コードを`PhotoAlbum`は、「 [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs)で、 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/)サンプル アプリケーションです。

<a name="preliminaries" />

### <a name="layout-and-initialization"></a>レイアウトと初期化

レイアウト ファイル**Main.axml**、単一の`RecyclerView`内で、 `LinearLayout`:

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

完全修飾名を使用する必要があります**android.support.v7.widget.RecyclerView**ため`RecyclerView`サポート ライブラリ内にパッケージ化します。 `OnCreate`メソッドの`MainActivity`このレイアウトを初期化、アダプターをインスタンス化して基になるデータ ソースに準備します。

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

このコードは次のとおり

1. インスタンスを作成、`PhotoAlbum`データ ソース。

2. フォト アルバムのデータ ソースをアダプターのコンス トラクターに渡します`PhotoAlbumAdapter`(これは、このガイドの後半で定義されます)。 
   データ ソースをパラメーターとして、アダプターのコンス トラクターに渡すことをお勧めと見なされることに注意してください。 

3. 取得、`RecyclerView`レイアウトからです。

4. アダプターに接続されるため、`RecyclerView`を呼び出してインスタンス、 `RecyclerView` `SetAdapter`メソッド上記のようにします。

### <a name="layout-manager"></a>レイアウト マネージャー

内の各項目、`RecyclerView`で構成された、`CardView`フォト イメージと写真のキャプションを格納している (詳細については、「、[ビュー所有者](#view-holder)以下のセクション)。 定義済み`LinearLayoutManager`それぞれをレイアウトするために使用`CardView`垂直方向のスクロール方法で配置します。

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

このコードは、メインのアクティビティに存在`OnCreate`メソッドです。 レイアウト マネージャーにコンス トラクターが必要です、*コンテキスト*ので、`MainActivity`を使用して渡される`this`上で説明します。

使用する代わりに、predefind `LinearLayoutManager`、2 つを表示するカスタム レイアウト マネージャーに接続できます`CardView`項目をサイド バイ サイド、写真のコレクションを走査するページめくりアニメーション効果を実装します。 このガイドでは、異なるレイアウト マネージャーでをスワップしてレイアウトを変更する方法の例が表示されます。

<a name="view-holder" />

### <a name="view-holder"></a>ビューの所有者

ビューの所有者のクラスが呼び出されます`PhotoViewHolder`です。 各`PhotoViewHolder`インスタンスへの参照を保持する、`ImageView`と`TextView`でレイアウトに関連する行の項目の`CardView`ように次の図を作成します。

[ ![ImageView と TextView を含む CardView のダイアグラム](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png)

`PhotoViewHolder` 派生した`RecyclerView.ViewHolder`への参照を保存するプロパティが含まれています、`ImageView`と`TextView`上記のレイアウトで表示します。
`PhotoViewHolder` 2 つのプロパティと 1 つのコンス トラクターで構成されます。

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
このコード例では、`PhotoViewHolder`コンス トラクターは、親アイテムのビューへの参照に渡されます (、 `CardView`) を`PhotoViewHolder`をラップします。 常に転送すること、親項目ビュー、基底コンス トラクターを注意してください。 `PhotoViewHolder`コンス トラクター呼び出し`FindViewById`それぞれその子ビューの参照を検索する親アイテムのビューで`ImageView`と`TextView`で結果を格納する、`Image`と`Caption`プロパティ、それぞれします。 アダプターがこれを更新するとき、これらのプロパティから参照の表示が後でを取得`CardView`の新しいデータで子ビュー。

詳細については`RecyclerView.ViewHolder`を参照してください、 [RecyclerView.ViewHolder クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)です。


### <a name="adapter"></a>アダプター

アダプターが読み込む各`RecyclerView`写真の特定のデータを含む行です。 行の位置にある指定された写真の*P*、アダプターが関連付けられている位置にあるデータを検索するなど、 *P*位置にある行にこのデータ項目のデータ ソースおよびコピー内で*P*で、`RecyclerView`コレクション。 アダプターの参照を検索するビューの所有者を使用して、`ImageView`と`TextView`を繰り返し呼び出すしないように、その位置にある`FindViewById`ビュー、ユーザーは、写真のコレクションをスクロールし、ビューを再利用のです。

**RecyclerViewer**、アダプター クラスから派生して`RecyclerView.Adapter`を作成する`PhotoAlbumAdapter`:

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

`mPhotoAlbum`メンバーには、コンス トラクターに渡されるデータ ソース (フォト アルバム) が含まれています。 コンス トラクターは、このメンバー変数にフォト アルバムをコピーします。 次の必要な`RecyclerView.Adapter`メソッドを実装します。

-   **`OnCreateViewHolder`** &ndash; 項目のレイアウト ファイルとビューの所有者をインスタンス化します。

-   **`OnBindViewHolder`** &ndash; 指定した位置にあるデータを指定されたビューの所有者での参照が格納されているビューに読み込みます。

-   **`ItemCount`** &ndash; データ ソース内の項目数を返します。

レイアウト マネージャー内のアイテムを配置には、メソッドが呼び出される、`RecyclerView`です。 次のセクションでは、これらのメソッドの実装が検査されます。

<a name="oncreateviewholder" />

#### <a name="oncreateviewholder"></a>OnCreateViewHolder

レイアウト マネージャー呼び出し`OnCreateViewHolder`ときに、`RecyclerView`する項目を表す新しいビューの所有者が必要です。 `OnCreateViewHolder` ビューのレイアウト ファイルからアイテムのビューを拡張し、新しいビューをラップ`PhotoViewHolder`インスタンス。 `PhotoViewHolder`コンス トラクターを検索し、子ビューへの参照をレイアウトで既に述べたよう[ビュー所有者](#view-holder)です。

行の各項目で表される、`CardView`を格納している、 `ImageView` (の写真) と`TextView`(キャプション) 用です。 このレイアウトが、ファイル内に存在**PhotoCardView.axml**:

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

このレイアウト内の 1 つの行項目を表す、`RecyclerView`です。 `OnBindViewHolder`メソッド (下記参照) にデータ ソースからデータをコピーする、`ImageView`と`TextView`このレイアウトのです。
`OnCreateViewHolder` 指定した写真の場所については、このレイアウトを拡張、`RecyclerView`し、新しいインスタンスを作成`PhotoViewHolder`インスタンス (を検索し、キャッシュへの参照、`ImageView`と`TextView`、関連する子ビュー`CardView`レイアウト)。

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

結果のビューの所有者インスタンス`vh`、(レイアウト マネージャーの) 呼び出し元に返されます。

<a name="onbindviewholder" />

#### <a name="onbindviewholder"></a>OnBindViewHolder

レイアウト マネージャーをいつの特定のビューを表示する、`RecyclerView`のアダプターの呼び出し画面の表示領域、`OnBindViewHolder`メソッドで、データ ソースからコンテンツを持つ指定した行の位置にある項目を設定します。 `OnBindViewHolder` 指定した行の位置 (写真の画像リソースと写真のキャプションの文字列) の写真の情報を取得し、このデータを関連するビューにコピーします。 ビューは、ビューの所有者のオブジェクトに格納された参照を使用して検索 (経由で渡される、`holder`パラメーター)。

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

派生ビューの所有者の型に渡されたビュー所有者オブジェクトをキャスト最初必要があります (この場合、 `PhotoViewHolder`) を使用前にします。
アダプターは、ビューの所有者によって参照されるビューにイメージ リソースを読み込みます`Image`プロパティ、およびそれには、ビューの所有者によって参照されるビューにキャプション テキストをコピー`Caption`プロパティです。 これは、*バインド*そのデータと関連するビュー。

注意して`OnBindViewHolder`データの構造を直接処理するコードに示します。 この場合、`OnBindViewHolder`にマップする方法を理解して、`RecyclerView`項目のデータ ソースに関連付けられたデータ項目に位置します。 マッピングは簡単なここでは、位置は、フォト アルバム; に配列インデックスとして使用できます。ただしより複雑なデータ ソースでは、このようなマッピングを確立するために余分なコードが必要です。

<a name="itemcount" />

#### <a name="itemcount"></a>ItemCount

`ItemCount`メソッドは、データ収集項目の数を返します。 例の写真のビューアー アプリケーションでは、項目の数は、フォト アルバムに写真の数は。

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

詳細については`RecyclerView.Adapter`を参照してください、 [RecyclerView.Adapter クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)です。

<a name="together" />

### <a name="putting-it-all-together"></a>まとめ

結果として得られる`RecyclerView`例フォト アプリの実装から成る`MainActivity`データ ソース、レイアウト マネージャーと、アダプターを作成するコードです。 `MainActivity` 作成、`mRecyclerView`インスタンスは、データ ソースと、アダプターをインスタンス化し、レイアウト マネージャーとアダプターではプラグインします。

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

`PhotoViewHolder` 検索し、ビューの参照をキャッシュします。

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

`PhotoAlbumAdapter` 次の 3 つの必要なメソッドのオーバーライドを実装します。

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

このコードをコンパイルして実行時に次のスクリーン ショットに示すようにアプリを表示する基本的な写真を作成します。

[ ![写真のカードを垂直方向にスクロールできるアプリを表示する写真の 2 つのスクリーン ショット](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png)

この基本的なアプリは、フォト アルバムの参照のみサポートされます。 項目タッチ イベントに応答しません。 また、基になるデータの変更が処理されること。 この機能が追加される[RecyclerView 例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)です。

<a name="layoutmanagerchange" />

### <a name="changing-the-layoutmanager"></a>LayoutManager を変更します。

ため`RecyclerView`の柔軟性が簡単に別のレイアウト マネージャーを使用するアプリを変更します。 次の例では、フォト アルバムを垂直方向の線形のレイアウトではなく、水平方向にスクロールするグリッド レイアウトでの表示に変更されます。 これを行うには、レイアウト マネージャーのインスタンス化が使用するよう変更、`GridLayoutManager`次のようにします。

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

このコードの変更には、垂直方向が置き換えられます`LinearLayoutManager`で、`GridLayoutManager`水平方向にスクロールする 2 つの行から成るグリッドが表示されます。 コンパイルして、アプリをもう一度実行するときに、写真がグリッドに表示されていることとスクロールが垂直方向のではなく、水平方向に表示されます。

[ ![グリッド内の水平方向にスクロールの写真でアプリのサンプルのスクリーン ショット](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png)

コードの 1 行のみを変更すると、動作の違いとは異なるレイアウトを使用する写真表示アプリを変更することはできます。
アダプター コードもレイアウト XML がレイアウト スタイルを変更するように変更する必要があることに注意してください。 

次のトピックで[RecyclerView 例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)、アイテムのクリック イベントを処理し、更新にこの基本的なサンプル アプリを拡張`RecyclerView`基になるデータ ソースの変更。



## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView パーツおよび機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView 例を拡張します。](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
