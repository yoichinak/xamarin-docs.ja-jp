---
title: ViewPager とビュー
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、データページとしてビューを使用して、ViewPager および PagerTabStrip で swipeable UI を実装する方法について説明します (以降のガイドでは、ページにフラグメントを使用する方法について説明します)。
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 7413fbe3f08988cfdb7c7b4e5237539aca250772
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940844"
---
# <a name="viewpager-with-views"></a>ViewPager とビュー

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、データページとしてビューを使用して、ViewPager および PagerTabStrip で swipeable UI を実装する方法について説明します (以降のガイドでは、ページにフラグメントを使用する方法について説明します)。_

## <a name="overview"></a>の概要

このガイドは、`ViewPager` を使用して、落葉樹および evergreen ツリーのイメージギャラリーを実装する方法を示すチュートリアルです。 このアプリでは、ユーザーはツリーの画像を表示するために、"ツリーカタログ" を左および右にスワイプします。 カタログの各ページの上部には、ツリーの名前が`PagerTabStrip`に表示され、ツリーのイメージが `ImageView`に表示されます。 アダプターは、基になるデータモデルに `ViewPager` をインターフェイスするために使用されます。 このアプリは、`PagerAdapter`から派生したアダプターを実装します。 

`ViewPager`ベースのアプリは、多くの場合 `Fragment`と共に実装されますが、`Fragment`の複雑さが増すことが比較的単純な場合もあります。 たとえば、このチュートリアルで示されている基本的なイメージギャラリーアプリでは `Fragment`s を使用する必要はありません。 コンテンツは静的なので、ユーザーは異なるイメージ間でスワイプを行うだけなので、標準の Android のビューとレイアウトを使用して実装を簡素化できます。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**Treepager**という名前の新しい android プロジェクトを作成します (新しい android プロジェクトの作成の詳細については[、「Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照してください)。 次に、NuGet パッケージマネージャーを起動します。 (NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください)。 **Android サポートライブラリ v4**を検索してインストールします。 

[![NuGet パッケージマネージャーで選択されたサポート v4 NuGet のスクリーンショット](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

これにより、 **Android サポートライブラリ v4**によって reaquired された追加のパッケージもインストールされます。

## <a name="add-an-example-data-source"></a>サンプルデータソースを追加する

この例では、ツリーカタログデータソース (`TreeCatalog` クラスによって表されます) が、`ViewPager` に項目の内容を提供します。 
`TreeCatalog` には、アダプターが `View`を作成するために使用するツリーイメージとツリータイトルのコレクションがあらかじめ用意されています。 `TreeCatalog` コンストラクターには引数は必要ありません。

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

`TreeCatalog` 内のイメージのコレクションは、各イメージがインデクサーによってアクセスできるように編成されます。 たとえば、次のコード行は、コレクション内の3番目のイメージのイメージリソース ID を取得します。 

```csharp
int imageId = treeCatalog[2].imageId;
```

`TreeCatalog` の実装の詳細は `ViewPager`を理解するためのものではないため、`TreeCatalog` コードはここには記載されていません。 `TreeCatalog` するソースコードは、 [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)で入手できます。 このソースファイルをダウンロードして (またはコードをコピーして新しい**TreeCatalog.cs**ファイルに貼り付け)、プロジェクトに追加します。 また、[イメージファイル](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true)をダウンロードし、**リソース/** 作成したフォルダーに解凍して、プロジェクトに含めます。 

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
```

この XML は、画面全体を占有する `ViewPager` を定義します。 `ViewPager` はサポートライブラリにパッケージされているので、完全修飾名の android... **ViewPager**を使用する必要があることに注意してください。 `ViewPager` は、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)からのみ使用できます。Android SDK では使用できません。 

## <a name="set-up-viewpager"></a>ViewPager の設定

**MainActivity.cs**を編集し、次の `using` ステートメントを追加します。

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

このコードは、次の処理を実行します。

1. **メインの axml**レイアウトリソースからビューを設定します。

2. レイアウトから `ViewPager` への参照を取得します。

3. 新しい `TreeCatalog` をデータソースとしてインスタンス化します。

このコードをビルドして実行すると、次のスクリーンショットのような画面が表示されます。 

[![空の ViewPager を表示するアプリのスクリーンショットを します](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

この時点では、 **TreeCatalog**のコンテンツにアクセスするためのアダプターが不足しているため、`ViewPager` は空です。 次のセクションでは、`ViewPager` を**TreeCatalog**に接続するための**pageradapter**を作成します。 

## <a name="create-the-adapter"></a>アダプターを作成する

`ViewPager` は、`ViewPager` とデータソースの間にあるアダプターコントローラーオブジェクトを使用します (「[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)」の図を参照してください)。 このデータにアクセスするには、`ViewPager` で `PagerAdapter`から派生したカスタムアダプターを指定する必要があります。 このアダプターは、各 `ViewPager` ページにデータソースのコンテンツを設定します。 このデータソースはアプリ固有であるため、カスタムアダプターは、データへのアクセス方法を理解するコードです。 `ViewPager`のページを介してユーザーがスワイプと、アダプターはデータソースから情報を抽出し、`ViewPager` を表示するためにページに読み込みます。 

`PagerAdapter`を実装する場合は、次のものをオーバーライドする必要があります。

- **InstantiateItem** &ndash; は、特定の位置に対してページ (`View`) を作成し、`ViewPager`のビューのコレクションに追加します。 

- **Destroyitem** &ndash; 指定した位置からページを削除します。

- 使用可能なビュー (ページ) の数を返す読み取り専用プロパティ &ndash;**カウント**します。 

- **Isviewfromobject** &ndash; は、ページが特定のキーオブジェクトに関連付けられているかどうかを判断します。 (このオブジェクトは `InstantiateItem` メソッドによって作成されます)。この例では、キーオブジェクトは `TreeCatalog` データオブジェクトです。

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

このコードは、重要な `PagerAdapter` 実装をスタブします。 次のセクションでは、これらの各メソッドが作業コードに置き換えられています。 

### <a name="implement-the-constructor"></a>コンストラクターを実装する

アプリによって `TreePagerAdapter`がインスタンス化されると、コンテキスト (`MainActivity`) とインスタンス化された `TreeCatalog`が提供されます。 **TreePagerAdapter.cs**の `TreePagerAdapter` クラスの先頭に、次のメンバー変数とコンストラクターを追加します。 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

このコンストラクターの目的は、`TreePagerAdapter` が使用するコンテキストと `TreeCatalog` インスタンスを格納することです。 

### <a name="implement-count"></a>実装数

`Count` の実装は比較的単純なので、ツリーカタログ内のツリー数が返されます。 `Count` を次のコードに置き換えます。

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`TreeCatalog` の `NumTrees` プロパティは、データセット内のツリー (ページ数) の数を返します。

### <a name="implement-instantiateitem"></a>InstantiateItem を実装する

`InstantiateItem` メソッドは、指定された位置のページを作成します。 また、新しく作成したビューを `ViewPager`のビューコレクションに追加する必要があります。 これを可能にするために、`ViewPager` はコンテナーパラメーターとして渡されます。 

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

このコードは、次の処理を実行します。

1. 指定した位置にツリーイメージを表示する新しい `ImageView` をインスタンス化します。 アプリの `MainActivity` は、`ImageView` コンストラクターに渡されるコンテキストです。

2. 指定した位置にある `TreeCatalog` イメージリソース ID に `ImageView` リソースを設定します。

3. 渡されたコンテナー `View` を `ViewPager` 参照にキャストします。
    このキャストを適切に実行するには `JavaCast<ViewPager>()` を使用する必要があることに注意してください (これは、Android が実行時にチェックされる型変換を実行するために必要です)。

4. インスタンス化された `ImageView` を `ViewPager` に追加し、`ImageView` を呼び出し元に返します。

`ViewPager` が `position`に画像を表示すると、この `ImageView`が表示されます。 最初の2つのページにビューを設定するには、最初に `InstantiateItem` を2回呼び出します。 ユーザーがスクロールすると、現在表示されている項目の前後のビューを維持するためにもう一度呼び出されます。 

### <a name="implement-destroyitem"></a>DestroyItem の実装

`DestroyItem` メソッドは、指定された位置からページを削除します。 特定の位置にあるビューを変更できるアプリでは、`ViewPager` は、その位置で古いビューを削除してから新しいビューに置き換える必要があります。 `TreeCatalog` の例では、各位置のビューが変更されないため、`DestroyItem` によって削除されたビューは、その位置に対して `InstantiateItem` が呼び出されると、単に再追加されます。 (効率を上げるために、同じ位置に再表示される `View`をリサイクルするプールを実装することもできます)。 

`DestroyItem` メソッドを次のコードで置き換えます。 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

このコードは、次の処理を実行します。

1. 渡されたコンテナー `View` を `ViewPager` 参照にキャストします。

2. 渡された Java オブジェクト (`view`) をC# `View` (`view as View`) にキャストします。

3. `ViewPager`からビューを削除します。 

### <a name="implement-isviewfromobject"></a>Implement IsViewFromObject

ユーザーがコンテンツのページを左右に移動すると、`ViewPager` は `IsViewFromObject` を呼び出して、指定された位置にある子 `View` が同じ位置にあるアダプターのオブジェクトに関連付けられていることを確認します (したがって、アダプターのオブジェクトは*オブジェクトキー*と呼ばれます)。 比較的単純なアプリの場合、アソシエーションは id の1つであり、そのインスタンスのアダプターのオブジェクトキーが `InstantiateItem`経由で `ViewPager` に返されたビューである &ndash; ます。 ただし、他のアプリの場合、オブジェクトキーは、その位置で表示 `ViewPager` 子ビューと関連付けられている、他のアダプター固有のクラスインスタンスである場合があります。 渡されたビューとオブジェクトキーが関連付けられているかどうかを認識するのはアダプターだけです。 

`PagerAdapter` を正常に機能させるには、`IsViewFromObject` を実装する必要があります。 `IsViewFromObject` が特定の位置に対して `false` を返す場合、`ViewPager` はその位置にビューを表示しません。 `TreePager` アプリでは、`InstantiateItem` によって返されるオブジェクトキー*は*ツリーのページ `View` であるため、コードは id を確認するだけで済みます (つまり、オブジェクトキーとビューが1で、同じであること)。 `IsViewFromObject` を次のコードに置き換えます。 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager にアダプターを追加する

`TreePagerAdapter` が実装されたので、次に `ViewPager`に追加します。 **MainActivity.cs**で、`OnCreate` メソッドの末尾に次のコード行を追加します。

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

このコードにより、`TreePagerAdapter`がインスタンス化され、`MainActivity` がコンテキスト (`this`) として渡されます。 インスタンス化された `TreeCatalog` は、コンストラクターの2番目の引数に渡されます。 `ViewPager`の `Adapter` プロパティは、インスタンス化された `TreePagerAdapter` オブジェクトに設定されます。これにより、`TreePagerAdapter` が `ViewPager`にプラグされます。 

これで、アプリケーションをビルドして実行 &ndash; コア実装が完成しました。 次のスクリーンショットの左側に示すように、ツリーカタログの最初の画像が画面に表示されます。 左にスワイプしてさらにツリービューを表示し、右にスワイプしてツリーカタログに戻ります。 

[![TreePager アプリのスクリーンショットを ツリーイメージをスワイプする](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>ページャーインジケーターの追加

この最小限の `ViewPager` 実装では、ツリーカタログのイメージが表示されますが、ユーザーがカタログ内にある場所については示されません。 次の手順では、`PagerTabStrip`を追加します。 `PagerTabStrip` は、表示されているページについてユーザーに通知し、前のページと次のページのヒントを表示することによってナビゲーションコンテキストを提供します。 `PagerTabStrip` は、`ViewPager`の現在のページのインジケーターとして使用することを目的としています。ユーザーが各ページをスワイプと、スクロールして更新します。 

**Resources/layout/Main**を開き、レイアウトに `PagerTabStrip` を追加します。

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

`ViewPager` と `PagerTabStrip` は、連携して動作するように設計されています。 `ViewPager` レイアウト内で `PagerTabStrip` を宣言すると、`ViewPager` によって自動的に `PagerTabStrip` が検索され、アダプターに接続されます。 アプリをビルドして実行すると、各画面の上部に空の `PagerTabStrip` が表示されます。 

[![空の PagerTabStrip の クローズアップスクリーンショット](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)

### <a name="display-a-title"></a>タイトルを表示する

各ページタブにタイトルを追加するには、`PagerAdapter`派生クラスに `GetPageTitleFormatted` メソッドを実装します。 `ViewPager` は `GetPageTitleFormatted` (実装されている場合) を呼び出して、指定した位置にあるページを説明するタイトル文字列を取得します。 **TreePagerAdapter.cs**の `TreePagerAdapter` クラスに次のメソッドを追加します。 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

このコードは、ツリーカタログ内の指定されたページ (位置) からツリーのキャプション文字列を取得し、それを Java `String`に変換して、`ViewPager`に返します。 この新しいメソッドを使用してアプリを実行すると、各ページには `PagerTabStrip`のツリーキャプションが表示されます。 画面の上部に下線のないツリー名が表示されます。 

[![テキストが入力された PagerTabStrip タブを使用してページのスクリーンショットを](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

順番にスワイプして、カタログ内の各キャプション付きツリーイメージを表示できます。 

### <a name="pagertitlestrip-variation"></a>Pagerタイトルストリップのバリエーション

`PagerTitleStrip` は `PagerTabStrip` と非常によく似ていますが、`PagerTabStrip` は現在選択されているタブに下線を追加する点が異なります。上記のレイアウトで `PagerTabStrip` を `PagerTitleStrip` に置き換え、アプリをもう一度実行して `PagerTitleStrip`でどのように表示されるかを確認できます。 

[![テキストから下線が削除された Pagerタイトルストリップ](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

`PagerTitleStrip`に変換すると、下線が削除されることに注意してください。 

## <a name="summary"></a>要約

このチュートリアルでは、`Fragment`s を使用せずに基本的な `ViewPager`ベースのアプリを構築する方法の手順について説明しました。 画像とキャプション文字列を含むデータソースの例、画像を表示するための `ViewPager` レイアウト、および `ViewPager` をデータソースに接続する `PagerAdapter` サブクラスが示されています。 ユーザーがデータセット内を移動できるようにするために、各ページの上部に画像キャプションを表示するための `PagerTabStrip` または `PagerTitleStrip` を追加する方法を説明する手順が含まれていました。 

## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
