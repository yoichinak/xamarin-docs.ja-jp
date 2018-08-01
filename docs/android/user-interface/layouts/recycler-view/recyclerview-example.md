---
title: 基本的な RecyclerView の例
description: RecyclerView を使用する方法を示す例のアプリ。
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/30/2018
ms.openlocfilehash: d48796b3c62fc342bd86f2d58e74c5f1710174bb
ms.sourcegitcommit: 0a1c392829454468dbe92f81d975e124a22b7014
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "39360839"
---
# <a name="a-basic-recyclerview-example"></a>基本的な RecyclerView の例

理解しておく方法`RecyclerView`一般的なアプリケーションでは、このトピックについて説明、 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/)サンプル アプリを使用する単純なコード例`RecyclerView`写真の大規模なコレクションを表示します。 

[![CardViews を使用して写真を表示する RecyclerView アプリの 2 つのスクリーン ショット](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer**使用[CardView](~/android/user-interface/controls/card-view.md)内の各写真アイテムを実装するために、`RecyclerView`レイアウト。 ため`RecyclerView`のパフォーマンスの利点があります、このサンプル アプリは迅速に円滑かつ顕著な遅延なしに、多数の写真をスクロールできるようします。


### <a name="an-example-data-source"></a>例のデータ ソース

この例のアプリで「アルバム」のデータ ソース (によって表される、`PhotoAlbum`クラス) を提供`RecyclerView`項目の内容にします。
`PhotoAlbum` キャプションの; 写真のコレクションです。インスタンスを作成する場合は、32 の写真の既製のコレクションを取得します。

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

内の各写真インスタンス`PhotoAlbum`をそのイメージのリソース ID を読み取ることを許可するプロパティを公開します`PhotoID`、およびそのキャプション文字列`Caption`します。 写真のコレクションは、それぞれの写真は、インデクサーでアクセスできるように構成されています。 たとえば、次のコード行は、イメージ リソース ID と、コレクション内の 10 番目の写真のキャプションをアクセスします。

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` 用意されています、`RandomSwap`コレクションの他の場所でランダムに選択された写真を使用して、最初、コレクション内の写真のスワップを呼び出すことのできるメソッド。

```csharp
mPhotoAlbum.RandomSwap ();
```

の実装の詳細`PhotoAlbum`関係を理解することのない`RecyclerView`、`PhotoAlbum`ソース コードはここに表示されません。 ソース コードを`PhotoAlbum`いただけます[PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs)で、 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/)サンプル アプリです。


### <a name="layout-and-initialization"></a>レイアウトと初期化

レイアウト ファイル**Main.axml**、単一の`RecyclerView`内、 `LinearLayout`:

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

完全修飾名を使用する必要があります**android.support.v7.widget.RecyclerView**ため`RecyclerView`サポート ライブラリにパッケージ化されます。 `OnCreate`メソッドの`MainActivity`このレイアウトを初期化します、アダプターをインスタンス化、および基になるデータ ソースを準備します。

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

このコードは、次を行います。

1. インスタンスを作成、`PhotoAlbum`データ ソース。

2. フォト アルバムのデータ ソースをアダプターのコンス トラクターに渡します`PhotoAlbumAdapter`(これは、このガイドの後半で定義されます)。 
   データ ソースをパラメーターとして、アダプターのコンス トラクターに渡すことをお勧めと見なされることに注意してください。 

3. 取得、`RecyclerView`レイアウトから。

4. アダプターでは、`RecyclerView`インスタンスを呼び出すことによって、 `RecyclerView` `SetAdapter`メソッドの上に示すようにします。

### <a name="layout-manager"></a>レイアウト マネージャー

内の各項目、`RecyclerView`で構成された、`CardView`写真イメージと写真のキャプションを格納している (詳細については、[ビュー所有者](#view-holder)以下のセクション)。 定義済み`LinearLayoutManager`それぞれをレイアウトするために使用`CardView`を垂直方向スクロール上に配置します。

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

このコードは、メイン アクティビティの存在`OnCreate`メソッド。 レイアウト マネージャーにコンス トラクターが必要です、*コンテキスト*であり、`MainActivity`を使用して渡される`this`上記のようです。

使用する代わりに、predefind `LinearLayoutManager`、2 つを表示するカスタム レイアウト マネージャーをプラグインできる`CardView`項目をサイド バイ サイド、写真のコレクションをスキャンするページめくりのアニメーション効果を実装します。 このガイドで後で別のレイアウト マネージャーとスワップしてレイアウトを変更する方法の例が表示されます。

<a name="view-holder" />

### <a name="view-holder"></a>ビューの所有者

ビューの所有者クラスと呼びます`PhotoViewHolder`します。 各`PhotoViewHolder`インスタンスへの参照を保持する、`ImageView`と`TextView`レイアウトでは、関連する行項目の`CardView`ように次の図を作成します。

[![ImageView と TextView を含む CardView のダイアグラム](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` 派生した`RecyclerView.ViewHolder`への参照を格納するプロパティを格納し、`ImageView`と`TextView`上記のレイアウトで表示します。
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
このコード例では、`PhotoViewHolder`コンス トラクターには、親アイテムのビューへの参照が渡されます (、 `CardView`) を`PhotoViewHolder`をラップします。 常に転送する親 item ビューを基底コンス トラクターに注意してください。 `PhotoViewHolder`コンス トラクター呼び出し`FindViewById`それぞれの子ビューの参照を検索する親項目ビューで`ImageView`と`TextView`で結果を格納する、`Image`と`Caption`プロパティ、それぞれします。 アダプターがこれを更新する際、これらのプロパティから参照を表示するが後で取得します`CardView`の新しいデータで子ビュー。

詳細については`RecyclerView.ViewHolder`を参照してください、 [RecyclerView.ViewHolder クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)します。


### <a name="adapter"></a>アダプター

アダプターが読み込む各`RecyclerView`写真の特定のデータを含む行です。 行の位置にある指定された写真の*P*、アダプターが関連付けられている位置にあるデータを検索するなど、 *P*位置にある行にこのデータ項目のデータ ソースとコピー内で*P*で、`RecyclerView`コレクション。 アダプターの参照を検索、ビューの所有者を使用して、`ImageView`と`TextView`を繰り返し呼び出すことがあるないため、その位置にある`FindViewById`のように、ユーザーが写真のコレクションをスクロールし、ビューを再利用されたビューします。

**RecyclerViewer**からアダプター クラスを派生`RecyclerView.Adapter`を作成する`PhotoAlbumAdapter`:

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

`mPhotoAlbum`メンバーがコンス トラクターに渡されるデータ ソース (フォト アルバム) が含まれています。 コンス トラクターは、このメンバー変数にフォト アルバムをコピーします。 次の必要な`RecyclerView.Adapter`メソッドが実装されます。

-   **`OnCreateViewHolder`** &ndash; 項目のレイアウト ファイルとビューの所有者をインスタンス化します。

-   **`OnBindViewHolder`** &ndash; 指定されたビューの所有者の参照が格納されているビューには、指定した位置にあるデータを読み込みます。

-   **`ItemCount`** &ndash; データ ソース内の項目の数を返します。

レイアウト マネージャー内のアイテムの配置中にメソッドが呼び出される、`RecyclerView`します。 次のセクションでは、これらのメソッドの実装が検査されます。


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

レイアウト マネージャー呼び出し`OnCreateViewHolder`ときに、`RecyclerView`する項目を表す新しいビュー所有者必要があります。 `OnCreateViewHolder` ビューのレイアウト ファイルからアイテムのビューを拡張し、新しいビューをラップします`PhotoViewHolder`インスタンス。 `PhotoViewHolder`コンス トラクターを見つけてで既に説明したように、レイアウトで子ビューへの参照を格納[ビュー所有者](#view-holder)します。

各の行項目として表されます、`CardView`を格納している、 `ImageView` (の写真) と`TextView`(キャプション) 用です。 このレイアウトは、ファイルが存在する**PhotoCardView.axml**:

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

このレイアウト内の 1 つの行項目を表す、`RecyclerView`します。 `OnBindViewHolder`メソッド (下記参照) にデータ ソースからデータをコピーする、`ImageView`と`TextView`このレイアウトの。
`OnCreateViewHolder` 指定した写真の場所については、このレイアウトを拡張、`RecyclerView`し、新しいインスタンスを作成`PhotoViewHolder`インスタンス (を検索し、キャッシュへの参照、`ImageView`と`TextView`に関連付けられている子ビュー`CardView`レイアウト)。

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

結果ビューの所有者のインスタンスに`vh`、(レイアウト マネージャー) 呼び出し元に返されます。


#### <a name="onbindviewholder"></a>OnBindViewHolder

レイアウト マネージャーがの特定のビューを表示できる場合、`RecyclerView`のアダプターの呼び出し画面の表示領域、`OnBindViewHolder`メソッドがデータ ソースからコンテンツを持つ指定した行の位置にある項目を入力します。 `OnBindViewHolder` 指定した行の位置 (写真のイメージ リソースと写真のキャプションの文字列) の写真の情報を取得し、関連のビューにこのデータをコピーします。 ビューがビューの所有者のオブジェクトに格納されている参照を使用して配置されている (経由で渡されたが、`holder`パラメーター)。

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

渡されたビュー所有者のオブジェクトが派生ビューの所有者の型にキャストする必要があります最初に (この場合、 `PhotoViewHolder`) は、使用前にします。
アダプターは、ビューの所有者のによって参照されるビューに、イメージ リソースを読み込みます`Image`プロパティ、およびそれには、ビューの所有者のによって参照されるビューにキャプション テキストをコピー`Caption`プロパティ。 これは、*バインド*データに関連するビュー。

注意`OnBindViewHolder`データの構造を直接処理するコードに示します。 この場合、`OnBindViewHolder`にマップする方法を理解して、`RecyclerView`項目のデータ ソースに関連付けられているデータ項目に位置します。 マッピングは簡単ですこの場合、位置は、フォト アルバム; に配列インデックスとして使用できるためただしより複雑なデータ ソースは、このようなマッピングを確立するために余分なコードを必要があります。


#### <a name="itemcount"></a>ItemCount

`ItemCount`メソッドは、データ収集項目の数を返します。 例のフォト ビューアー アプリでは、項目数は、フォト アルバムの数です。

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

詳細については`RecyclerView.Adapter`を参照してください、 [RecyclerView.Adapter クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)します。


### <a name="putting-it-all-together"></a>まとめ

結果の`RecyclerView`フォト アプリの例の実装から成る`MainActivity`データ ソース、レイアウト マネージャーと、アダプターを作成するコードです。 `MainActivity` 作成、`mRecyclerView`インスタンスは、データ ソースと、アダプターをインスタンス化し、レイアウト マネージャーとアダプターでは。

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

このコードはコンパイルして実行して、次のスクリーン ショットに示すようにアプリを表示する基本的な写真が作成されます。

[![写真のカードに垂直方向にスクロールできるアプリを表示する写真の 2 つのスクリーン ショット](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

(上記のスクリーン ショットに表示) と影の描画されていない、編集**Properties/AndroidManifest.xml**に次の属性設定を追加して、`<application>`要素。

```xml
android:hardwareAccelerated="true"
```

この基本的なアプリでは、フォト アルバムの閲覧のみサポートされます。 項目タッチ イベントに応答しません。 また基になるデータの変更が処理されること。 この機能が追加された[RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)します。




### <a name="changing-the-layoutmanager"></a>LayoutManager を変更します。

ため`RecyclerView`の柔軟性は簡単に別のレイアウト マネージャーを使用するアプリを変更できます。 次の例では、垂直方向の線形レイアウトではなく、水平方向にスクロールするグリッド レイアウトによるフォト アルバムを表示する変更されます。 使用するには、レイアウト マネージャーのインスタンス化が変更された、`GridLayoutManager`次のようにします。

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

このコードの変更が、垂直方向に置き換えられます`LinearLayoutManager`で、`GridLayoutManager`の水平方向にスクロールして 2 つの行から成るグリッドを表示します。 をコンパイルして、アプリをもう一度実行するときに、写真がグリッドに表示されることと、スクロールが垂直方向ではなく水平方向に表示されます。

[![グリッド内の写真の水平方向にスクロールしたアプリのスクリーン ショットの例](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

コードの 1 行のみを変更するは異なる動作を別のレイアウトを使用するフォト ビューアー アプリを変更することができます。
レイアウト スタイルを変更するために変更する必要があるアダプター コードもレイアウト XML でもないことに注意してください。 

次のトピックで[RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)、この基本的なサンプル アプリの拡張項目のクリック イベントを処理し、更新`RecyclerView`基になるデータ ソースの変更。



## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
