---
title: ViewPager とビュー
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、データページとしてビューを使用して、ViewPager および PagerTabStrip で swipeable UI を実装する方法について説明します (以降のガイドでは、ページにフラグメントを使用する方法について説明します)。
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 15ff11c5100f697e1945793da0baca68add082be
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646557"
---
# <a name="viewpager-with-views"></a>ViewPager とビュー

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、データページとしてビューを使用して、ViewPager および PagerTabStrip で swipeable UI を実装する方法について説明します (以降のガイドでは、ページにフラグメントを使用する方法について説明します)。_

 
## <a name="overview"></a>概要

このガイドでは、を使用`ViewPager`して、落葉樹および evergreen ツリーのイメージギャラリーを実装する方法について、ステップバイステップで説明します。 このアプリでは、ユーザーはツリーの画像を表示するために、"ツリーカタログ" を左および右にスワイプします。 カタログの各ページの上部に、ツリーの名前が`PagerTabStrip`に表示され、ツリーのイメージがに表示さ`ImageView`れます。 アダプターは、を基になる`ViewPager`データモデルへのインターフェイスとして使用されます。 このアプリは、から`PagerAdapter`派生したアダプターを実装します。 

ベースのアプリは多くの場合、 `Fragment`と共に実装されますが、比較的複雑なの`Fragment`ユースケースもあります。 `ViewPager` たとえば、このチュートリアルで示されている基本的なイメージギャラリーアプリでは、を`Fragment`使用する必要はありません。 コンテンツは静的なので、ユーザーは異なるイメージ間でスワイプを行うだけなので、標準の Android のビューとレイアウトを使用して実装を簡素化できます。 



## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**Treepager**という名前の新しい android プロジェクトを作成します (新しい android プロジェクトの作成の詳細については[、「Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照してください)。 次に、NuGet パッケージマネージャーを起動します。 (NuGet パッケージのインストールの詳細について[は、「チュートリアル:プロジェクト](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)に NuGet を含めます)。 **Android サポートライブラリ v4**を検索してインストールします。 

[![NuGet パッケージマネージャーで選択されたサポート v4 Nuget のスクリーンショット](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

これにより、 **Android サポートライブラリ v4**によって reaquired された追加のパッケージもインストールされます。



## <a name="add-an-example-data-source"></a>サンプルデータソースを追加する

この例では、ツリーカタログデータソース ( `TreeCatalog`クラスによって表されます) が、 `ViewPager`を項目の内容と共に提供します。 
`TreeCatalog`アダプターが s を作成`View`するために使用するツリーイメージとツリータイトルのコレクションが用意されています。 コンストラクター `TreeCatalog`には引数は必要ありません。

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

の`TreeCatalog`イメージのコレクションは、各イメージがインデクサーによってアクセスできるように編成されています。 たとえば、次のコード行は、コレクション内の3番目のイメージのイメージリソース ID を取得します。 

```csharp
int imageId = treeCatalog[2].imageId;
```

の実装の`TreeCatalog`詳細は理解`ViewPager`には関係がないため`TreeCatalog` 、コードはここには記載されていません。 のソースコード`TreeCatalog`は、 [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)で入手できます。 このソースファイルをダウンロードして (またはコードをコピーして新しい**TreeCatalog.cs**ファイルに貼り付け)、プロジェクトに追加します。 また、[イメージファイル](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true)をダウンロードし、**リソース/** 作成したフォルダーに解凍して、プロジェクトに含めます。 



## <a name="create-a-viewpager-layout"></a>ViewPager レイアウトを作成する

**Resources/layout/Main**を開き、その内容を次の xml に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 


## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

`OnCreate` メソッドを次のコードで置き換えます。

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

このコードでは、次のことを行います。

1.  **メインの axml**レイアウトリソースからビューを設定します。

2.  レイアウトからへの`ViewPager`参照を取得します。

3.  新しい`TreeCatalog`をデータソースとしてインスタンス化します。

このコードをビルドして実行すると、次のスクリーンショットのような画面が表示されます。 

[![空の ViewPager を表示するアプリのスクリーンショット](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

この時点では、 `ViewPager` **TreeCatalog**内のコンテンツにアクセスするためのアダプターが不足しているため、は空です。 次のセクションでは、 `ViewPager`を**TreeCatalog**に接続するための**pageradapter**が作成されます。 


## <a name="create-the-adapter"></a>アダプターを作成する

`ViewPager`は、データソース`ViewPager`とデータソースの間にあるアダプターコントローラーオブジェクトを使用します (「[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)」の図を参照してください)。 このデータにアクセスするために`ViewPager` 、では、から`PagerAdapter`派生したカスタムアダプターを指定する必要があります。 このアダプターは、 `ViewPager`各ページにデータソースのコンテンツを読み込みます。 このデータソースはアプリ固有であるため、カスタムアダプターは、データへのアクセス方法を理解するコードです。 ユーザーがの`ViewPager`ページをスワイプと、アダプターはデータソースから情報を抽出して、 `ViewPager`表示するのページに読み込みます。 

を実装`PagerAdapter`する場合は、次のものをオーバーライドする必要があります。

-   **InstantiateItem**指定された`View`位置のページ () を作成`ViewPager`し、そのページのビューのコレクションに追加します。 &ndash; 

-   **Destroyitem**&ndash;指定された位置からページを削除します。

-   **カウント**&ndash;使用可能なビュー (ページ) の数を返す読み取り専用のプロパティ。 

-   **Isviewfromobject 書類**&ndash;ページが特定のキーオブジェクトに関連付けられているかどうかを判断します。 (このオブジェクトは、 `InstantiateItem`メソッドによって作成されます)。この例では、キーオブジェクトは`TreeCatalog`データオブジェクトです。

**TreePagerAdapter.cs**という名前の新しいファイルを追加し、その内容を次のコードに置き換えます。 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

このコードは、重要`PagerAdapter`な実装をスタブにします。 次のセクションでは、これらの各メソッドが作業コードに置き換えられています。 



### <a name="implement-the-constructor"></a>コンストラクターを実装する

アプリによってが`TreePagerAdapter`インスタンス化されると、コンテキスト`MainActivity`() とインスタンス`TreeCatalog`化されたが提供されます。 `TreePagerAdapter` **TreePagerAdapter.cs**のクラスの先頭に、次のメンバー変数とコンストラクターを追加します。 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

このコンストラクターの目的は、 `TreeCatalog` `TreePagerAdapter`が使用するコンテキストとインスタンスを格納することです。 



### <a name="implement-count"></a>実装数

この`Count`実装は比較的単純です。ツリーカタログ内のツリーの数を返します。 `Count` を次のコードに置き換えます。

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees` の`TreeCatalog`プロパティは、データセット内のツリー (ページ数) の数を返します。



### <a name="implement-instantiateitem"></a>InstantiateItem を実装する

メソッド`InstantiateItem`は、指定された位置のページを作成します。 また、新しく作成したビューを`ViewPager`のビューコレクションに追加する必要があります。 これを可能にするため`ViewPager`に、はコンテナーのパラメーターとして自身を渡します。 

`InstantiateItem` メソッドを次のコードで置き換えます。

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

このコードでは、次のことを行います。

1.  新しい`ImageView`をインスタンス化して、指定した位置にツリーイメージを表示します。 アプリの`MainActivity`は、 `ImageView`コンストラクターに渡されるコンテキストです。

2.  リソースを、指定し`TreeCatalog`た位置にあるイメージリソース ID に設定します。 `ImageView`

3.  渡されたコンテナー `View`を`ViewPager`参照にキャストします。
    このキャストを適切に`JavaCast<ViewPager>()`実行するには、を使用する必要があることに注意してください (これは、Android が実行時にチェックされる型変換を実行するために必要です)。

4.  インスタンス化`ImageView` `ImageView`されたをに追加し、を呼び出し元に返します。`ViewPager`

`ImageView`で`ViewPager` イメージが表示されると、これが表示されます。`position` 初期状態`InstantiateItem`では、が2回呼び出され、最初の2ページにビューが設定されます。 ユーザーがスクロールすると、現在表示されている項目の前後のビューを維持するためにもう一度呼び出されます。 



### <a name="implement-destroyitem"></a>DestroyItem の実装

メソッド`DestroyItem`は、指定された位置からページを削除します。 特定の位置にあるビューが変更可能なアプリで`ViewPager`は、は、その位置で古いビューを削除してから新しいビューに置き換える必要があります。 この例では、各位置のビューが変更されていないため、 `DestroyItem`によって削除された`InstantiateItem`ビューは、その位置に対してが呼び出されると、単に再追加されます。 `TreeCatalog` (効率を高めるために、同じ位置に再表示`View`されるをリサイクルするためにプールを実装することもできます)。 

`DestroyItem` メソッドを次のコードで置き換えます。 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

このコードでは、次のことを行います。

1.  渡されたコンテナー `View`を`ViewPager`参照にキャストします。

2.  渡された Java オブジェクト (`view`) をC# `View` (`view as View`) にキャストします。

3.  からビューを削除し`ViewPager`ます。 



### <a name="implement-isviewfromobject"></a>Implement IsViewFromObject

ユーザーがコンテンツのページを左および右に移動する`ViewPager`と`IsViewFromObject` 、を呼び出して、 `View`指定された位置にある子がその同じ位置のアダプターのオブジェクトに関連付けられていることを確認します (そのため、アダプターのオブジェクトは*オブジェクトキー*)。 比較的単純なアプリの場合、アソシエーションは id &ndash;の1つであり、そのインスタンスのアダプターのオブジェクトキーは、 `ViewPager`以前に via `InstantiateItem`に返されたビューです。 ただし、その他のアプリの場合、オブジェクトキーは、その位置に表示される`ViewPager`子ビューと関連付けられている他のアダプター固有のクラスインスタンスである可能性があります。 渡されたビューとオブジェクトキーが関連付けられているかどうかを認識するのはアダプターだけです。 

`IsViewFromObject`を正しく機能さ`PagerAdapter`せるには、を実装する必要があります。 指定`IsViewFromObject`さ`false`れた位置に対し`ViewPager`てがを返した場合、はその位置にビューを表示しません。 アプリで*は*、によって返されるオブジェクトキーは`View`ツリーのページであるため、コードでは id を確認するだけで済みます (つまり、オブジェクトキーとビューが1で、同じであること`InstantiateItem` )。 `TreePager` `IsViewFromObject` を次のコードに置き換えます。 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager にアダプターを追加する

が実装されたので、これ`ViewPager`をに追加します。 `TreePagerAdapter` **MainActivity.cs**で、 `OnCreate`メソッドの末尾に次のコード行を追加します。

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

このコードは、 `TreePagerAdapter`をインスタンス化し`MainActivity` 、コンテキスト (`this`) としてを渡します。 インスタンス化`TreeCatalog`されたは、コンストラクターの2番目の引数に渡されます。 `TreePagerAdapter` `TreePagerAdapter`のプロパティは、インスタンス化されたオブジェクトに設定されます`ViewPager`。これにより、がに接続されます。 `Adapter` `ViewPager` 

コア実装は、アプリの&ndash;ビルドと実行が完了しました。 次のスクリーンショットの左側に示すように、ツリーカタログの最初の画像が画面に表示されます。 左にスワイプしてさらにツリービューを表示し、右にスワイプしてツリーカタログに戻ります。 

[![ツリーイメージをスワイプしている TreePager アプリのスクリーンショット](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>ページャーインジケーターの追加

この最小`ViewPager`実装では、ツリーカタログのイメージが表示されますが、ユーザーがカタログ内に存在する場所については示されません。 次の手順では、を`PagerTabStrip`追加します。 は`PagerTabStrip` 、表示されているページについてユーザーに通知し、前のページと次のページのヒントを表示することによってナビゲーションコンテキストを提供します。 `PagerTabStrip`は、 `ViewPager`の現在のページのインジケーターとして使用することを意図しています。ユーザーが各ページをスワイプすると、スクロールして更新します。 

**Resources/layout/Main**を開き、を`PagerTabStrip`レイアウトに追加します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager`と`PagerTabStrip`は、連携して動作するように設計されています。 レイアウト`ViewPager` 内で`PagerTabStrip`を宣言すると、によってが自動的`PagerTabStrip`に検出され、アダプターに接続されます。`ViewPager` アプリをビルドして実行すると、各画面の上部`PagerTabStrip`に空のが表示されます。 

[![クローズアップ空の PagerTabStrip のスクリーンショット](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>タイトルを表示する

各ページタブにタイトルを追加するには、 `GetPageTitleFormatted`の派生クラス`PagerAdapter`でメソッドを実装します。 `ViewPager`( `GetPageTitleFormatted`実装されている場合) を呼び出して、指定した位置にあるページを説明するタイトル文字列を取得します。 `TreePagerAdapter` **TreePagerAdapter.cs**のクラスに次のメソッドを追加します。 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

このコードは、ツリーカタログ内の指定されたページ (位置) からツリーのキャプション文字列を取得し`String`、それを Java に変換`ViewPager`して、それをに返します。 この新しいメソッドを使用してアプリを実行すると、各ページにに`PagerTabStrip`ツリーキャプションが表示されます。 画面の上部に下線のないツリー名が表示されます。 

[![テキストが入力された PagerTabStrip のタブが表示されたページのスクリーンショット](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

順番にスワイプして、カタログ内の各キャプション付きツリーイメージを表示できます。 



### <a name="pagertitlestrip-variation"></a>Pagerタイトルストリップのバリエーション

`PagerTitleStrip`はとよく似`PagerTabStrip`てい`PagerTabStrip`ますが、は、現在選択されているタブに下線を追加する点が異なります。上記のレイアウト`PagerTabStrip`で`PagerTitleStrip`をに置き換えて、もう一度アプリを実行して、どのよう`PagerTitleStrip`に表示されるかを確認できます。 

[![テキストから下線が削除された Pagerタイトルストリップ](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

に`PagerTitleStrip`変換すると、下線が削除されることに注意してください。 


 
## <a name="summary"></a>まとめ

このチュートリアルでは、を使用`ViewPager` `Fragment`せずに基本的なベースのアプリを構築する方法の手順について説明しました。 画像とキャプション文字列を含むデータソースの例、 `ViewPager`画像を表示するレイアウト、 `PagerAdapter`およびをデータソース`ViewPager`に接続するサブクラスが示されています。 ユーザーがデータセット内を移動できるように、または`PagerTabStrip` `PagerTitleStrip`を追加して、各ページの上部に画像キャプションを表示する方法を説明する手順が含まれていました。 


## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
