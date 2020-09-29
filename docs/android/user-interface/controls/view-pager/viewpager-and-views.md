---
title: ViewPager とビュー
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、データページとしてビューを使用して、ViewPager および PagerTabStrip で swipeable UI を実装する方法について説明します (以降のガイドでは、ページにフラグメントを使用する方法について説明します)。
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: e4f4243c06d98eac6f3501c41b48508f260d4633
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456991"
---
# <a name="viewpager-with-views"></a>ViewPager とビュー

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、データページとしてビューを使用して、ViewPager および PagerTabStrip で swipeable UI を実装する方法について説明します (以降のガイドでは、ページにフラグメントを使用する方法について説明します)。_

## <a name="overview"></a>概要

このガイドでは、を使用して、 `ViewPager` 落葉樹および evergreen ツリーのイメージギャラリーを実装する方法について、ステップバイステップで説明します。 このアプリでは、ユーザーはツリーの画像を表示するために、"ツリーカタログ" を左および右にスワイプします。 カタログの各ページの上部に、ツリーの名前がに表示され、 `PagerTabStrip` ツリーのイメージがに表示され `ImageView` ます。 アダプターは、を `ViewPager` 基になるデータモデルへのインターフェイスとして使用されます。 このアプリは、から派生したアダプターを実装 `PagerAdapter` します。 

`ViewPager`ベースのアプリは多くの場合、と共に実装され `Fragment` ますが、比較的複雑なのユースケースもあり `Fragment` ます。 たとえば、このチュートリアルで示されている基本的なイメージギャラリーアプリでは、を使用する必要はありません `Fragment` 。 コンテンツは静的なので、ユーザーは異なるイメージ間でスワイプを行うだけなので、標準の Android のビューとレイアウトを使用して実装を簡素化できます。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**Treepager**という名前の新しい android プロジェクトを作成します (新しい android プロジェクトの作成の詳細については[、「Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照してください)。 次に、NuGet パッケージマネージャーを起動します。 (NuGet パッケージのインストールの詳細については、「 [チュートリアル: プロジェクトに nuget を含める](/visualstudio/mac/nuget-walkthrough)」を参照してください)。 **Android サポートライブラリ v4**を検索してインストールします。 

[![NuGet パッケージマネージャーで選択されたサポート v4 NuGet のスクリーンショット](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

これにより、 **Android サポートライブラリ v4**によって reaquired された追加のパッケージもインストールされます。

## <a name="add-an-example-data-source"></a>サンプルデータソースを追加する

この例では、ツリーカタログデータソース (クラスによって表され `TreeCatalog` ます) が、を `ViewPager` 項目の内容と共に提供します。 
`TreeCatalog` アダプターが s を作成するために使用するツリーイメージとツリータイトルのコレクションが用意されてい `View` ます。 コンストラクターには `TreeCatalog` 引数は必要ありません。

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

のイメージのコレクション `TreeCatalog` は、各イメージがインデクサーによってアクセスできるように編成されています。 たとえば、次のコード行は、コレクション内の3番目のイメージのイメージリソース ID を取得します。 

```csharp
int imageId = treeCatalog[2].imageId;
```

の実装の詳細は `TreeCatalog` 理解には関係がないため `ViewPager` 、 `TreeCatalog` コードはここには記載されていません。 のソースコード `TreeCatalog` は、 [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)で入手できます。 このソースファイルをダウンロードして (またはコードをコピーして新しい **TreeCatalog.cs** ファイルに貼り付け)、プロジェクトに追加します。 また、 [イメージファイル](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) をダウンロードし、 **リソース/** 作成したフォルダーに解凍して、プロジェクトに含めます。 

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

この XML は、 `ViewPager` 画面全体を占有するを定義します。 はサポートライブラリにパッケージされているので、完全修飾名の "android... **ViewPager** " を使用する必要があることに注意してください。 `ViewPager` `ViewPager`[Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)からのみ使用できます。Android SDK では使用できません。 

## <a name="set-up-viewpager"></a>ViewPager の設定

**MainActivity.cs**を編集し、次のステートメントを追加し `using` ます。

```csharp
using Android.Support.V4.View;
```

`OnCreate` メソッドを次のコードに置き換えます。

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

2. レイアウトからへの参照を取得 `ViewPager` します。

3. 新しいを `TreeCatalog` データソースとしてインスタンス化します。

このコードをビルドして実行すると、次のスクリーンショットのような画面が表示されます。 

[![空の ViewPager を表示するアプリのスクリーンショット](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

この時点では、 `ViewPager` **TreeCatalog**内のコンテンツにアクセスするためのアダプターが不足しているため、は空です。 次のセクションでは、を TreeCatalog に接続するための**Pageradapter**が作成され `ViewPager` ます。 **TreeCatalog** 

## <a name="create-the-adapter"></a>アダプターを作成する

`ViewPager` は、データソースとデータソースの間にあるアダプターコントローラーオブジェクトを使用し `ViewPager` ます (「 [アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)」の図を参照してください)。 このデータにアクセスするために、では、 `ViewPager` から派生したカスタムアダプターを指定する必要があり `PagerAdapter` ます。 このアダプターは `ViewPager` 、各ページにデータソースのコンテンツを読み込みます。 このデータソースはアプリ固有であるため、カスタムアダプターは、データへのアクセス方法を理解するコードです。 ユーザーがのページをスワイプと、 `ViewPager` アダプターはデータソースから情報を抽出して、表示するのページに読み込み `ViewPager` ます。 

を実装する場合は、 `PagerAdapter` 次のものをオーバーライドする必要があります。

- **InstantiateItem** &ndash;`View`指定された位置のページ () を作成し、そのページ `ViewPager` のビューのコレクションに追加します。 

- **Destroyitem** &ndash; 指定された位置からページを削除します。

- **カウント** &ndash; 使用可能なビュー (ページ) の数を返す読み取り専用のプロパティ。 

- **Isviewfromobject 書類** &ndash; ページが特定のキーオブジェクトに関連付けられているかどうかを判断します。 (このオブジェクトは、メソッドによって作成され `InstantiateItem` ます)。この例では、キーオブジェクトは `TreeCatalog` データオブジェクトです。

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

このコードは、重要な実装をスタブに `PagerAdapter` します。 次のセクションでは、これらの各メソッドが作業コードに置き換えられています。 

### <a name="implement-the-constructor"></a>コンストラクターを実装する

アプリによってがインスタンス化されると `TreePagerAdapter` 、コンテキスト ( `MainActivity` ) とインスタンス化されたが提供さ `TreeCatalog` れます。 TreePagerAdapter.cs のクラスの先頭に、次のメンバー変数とコンストラクターを追加し `TreePagerAdapter` ます。 **TreePagerAdapter.cs** 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

このコンストラクターの目的は、が使用するコンテキストとインスタンスを格納する `TreeCatalog` ことです `TreePagerAdapter` 。 

### <a name="implement-count"></a>実装数

`Count`この実装は比較的単純です。ツリーカタログ内のツリーの数を返します。 `Count` を次のコードに置き換えます。

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees`のプロパティは、 `TreeCatalog` データセット内のツリー (ページ数) の数を返します。

### <a name="implement-instantiateitem"></a>InstantiateItem を実装する

メソッドは、 `InstantiateItem` 指定された位置のページを作成します。 また、新しく作成したビューをのビューコレクションに追加する必要があり `ViewPager` ます。 これを可能にするために、は `ViewPager` コンテナーのパラメーターとして自身を渡します。 

`InstantiateItem` メソッドを次のコードに置き換えます。

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

1. 新しいをインスタンス化して、 `ImageView` 指定した位置にツリーイメージを表示します。 アプリの `MainActivity` は、コンストラクターに渡されるコンテキストです `ImageView` 。

2. リソースを、 `ImageView` 指定し `TreeCatalog` た位置にあるイメージリソース ID に設定します。

3. 渡されたコンテナーを `View` 参照にキャストし `ViewPager` ます。
    このキャストを適切に実行するには、を使用する必要があることに注意してください `JavaCast<ViewPager>()` (これは、Android が実行時にチェックされる型変換を実行するために必要です)。

4. インスタンス化されたをに追加し、を `ImageView` `ViewPager` `ImageView` 呼び出し元に返します。

`ViewPager`でイメージが表示されると `position` 、これが表示され `ImageView` ます。 初期状態で `InstantiateItem` は、が2回呼び出され、最初の2ページにビューが設定されます。 ユーザーがスクロールすると、現在表示されている項目の前後のビューを維持するためにもう一度呼び出されます。 

### <a name="implement-destroyitem"></a>DestroyItem の実装

メソッドは、 `DestroyItem` 指定された位置からページを削除します。 特定の位置にあるビューが変更可能なアプリでは、は、 `ViewPager` その位置で古いビューを削除してから新しいビューに置き換える必要があります。 この例では、 `TreeCatalog` 各位置のビューが変更されていないため、によって削除されたビューは、 `DestroyItem` `InstantiateItem` その位置に対してが呼び出されると、単に再追加されます。 (効率を高めるために、同じ位置に再表示されるをリサイクルするためにプールを実装することもでき `View` ます)。 

`DestroyItem` メソッドを次のコードに置き換えます。 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

このコードは、次の処理を実行します。

1. 渡されたコンテナーを `View` 参照にキャストし `ViewPager` ます。

2. 渡された Java オブジェクト ( `view` ) を C# () にキャストし `View` `view as View` ます。

3. からビューを削除し `ViewPager` ます。 

### <a name="implement-isviewfromobject"></a>IsViewFromObject 実装する

ユーザーがコンテンツのページを左から右に移動すると、を呼び出して、 `ViewPager` `IsViewFromObject` 指定された位置にある子が `View` その同じ位置のアダプターのオブジェクトに関連付けられていることを確認します (そのため、アダプターのオブジェクトは *オブジェクトキー*と呼ばれます)。 比較的単純なアプリの場合、アソシエーションは id の1つであり、 &ndash; そのインスタンスのアダプターのオブジェクトキーは、以前に via に返されたビューです `ViewPager` `InstantiateItem` 。 ただし、その他のアプリの場合、オブジェクトキーは、その位置に表示される子ビューと関連付けられている他のアダプター固有のクラスインスタンスである可能性があり `ViewPager` ます。 渡されたビューとオブジェクトキーが関連付けられているかどうかを認識するのはアダプターだけです。 

`IsViewFromObject` を正しく機能させるには、を実装する必要があり `PagerAdapter` ます。 `IsViewFromObject` `false` 指定された位置に対してがを返した場合、 `ViewPager` はその位置にビューを表示しません。 アプリで `TreePager` は、によって返されるオブジェクトキー `InstantiateItem` *は* ツリーのページであるため、コードでは `View` id を確認するだけで済みます (つまり、オブジェクトキーとビューが1で、同じであること)。 `IsViewFromObject` を次のコードに置き換えます。 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager にアダプターを追加する

が実装されたので、これ `TreePagerAdapter` をに追加し `ViewPager` ます。 **MainActivity.cs**で、メソッドの末尾に次のコード行を追加し `OnCreate` ます。

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

このコードは、をインスタンス化し `TreePagerAdapter` 、 `MainActivity` コンテキスト () としてを渡し `this` ます。 インスタンス化 `TreeCatalog` されたは、コンストラクターの2番目の引数に渡されます。 `ViewPager`の `Adapter` プロパティは、インスタンス化されたオブジェクトに設定されます。これにより、がに接続され `TreePagerAdapter` `TreePagerAdapter` `ViewPager` ます。 

コア実装は、 &ndash; アプリのビルドと実行が完了しました。 次のスクリーンショットの左側に示すように、ツリーカタログの最初の画像が画面に表示されます。 左にスワイプしてさらにツリービューを表示し、右にスワイプしてツリーカタログに戻ります。 

[![ツリーイメージをスワイプしている TreePager アプリのスクリーンショット](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>ページャーインジケーターの追加

この最小実装では、 `ViewPager` ツリーカタログのイメージが表示されますが、ユーザーがカタログ内に存在する場所については示されません。 次の手順では、を追加し `PagerTabStrip` ます。 は、表示され `PagerTabStrip` ているページについてユーザーに通知し、前のページと次のページのヒントを表示することによってナビゲーションコンテキストを提供します。 `PagerTabStrip` は、の現在のページのインジケーターとして使用することを意図しています `ViewPager` 。ユーザーが各ページをスワイプすると、スクロールして更新します。 

**Resources/layout/Main**を開き、を `PagerTabStrip` レイアウトに追加します。

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

`ViewPager` と `PagerTabStrip` は、連携して動作するように設計されています。 レイアウト内でを宣言すると、によってが `PagerTabStrip` `ViewPager` `ViewPager` 自動的に検出され、 `PagerTabStrip` アダプターに接続されます。 アプリをビルドして実行すると、 `PagerTabStrip` 各画面の上部に空のが表示されます。 

[![クローズアップ空の PagerTabStrip のスクリーンショット](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)

### <a name="display-a-title"></a>タイトルを表示する

各ページタブにタイトルを追加するには、の `GetPageTitleFormatted` 派生クラスでメソッドを実装し `PagerAdapter` ます。 `ViewPager``GetPageTitleFormatted`(実装されている場合) を呼び出して、指定した位置にあるページを説明するタイトル文字列を取得します。 TreePagerAdapter.cs のクラスに次のメソッドを追加し `TreePagerAdapter` ます。 **TreePagerAdapter.cs** 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

このコードは、ツリーカタログ内の指定されたページ (位置) からツリーのキャプション文字列を取得し、それを Java に変換 `String` して、それをに返し `ViewPager` ます。 この新しいメソッドを使用してアプリを実行すると、各ページににツリーキャプションが表示され `PagerTabStrip` ます。 画面の上部に下線のないツリー名が表示されます。 

[![テキストが入力された PagerTabStrip のタブが表示されたページのスクリーンショット](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

順番にスワイプして、カタログ内の各キャプション付きツリーイメージを表示できます。 

### <a name="pagertitlestrip-variation"></a>Pagerタイトルストリップのバリエーション

`PagerTitleStrip` はとよく似 `PagerTabStrip` てい `PagerTabStrip` ますが、は、現在選択されているタブに下線を追加する点が異なります。 `PagerTabStrip` 上記のレイアウトでをに置き換えて `PagerTitleStrip` 、もう一度アプリを実行して、どのように表示されるかを確認でき `PagerTitleStrip` ます。 

[![テキストから下線が削除された Pagerタイトルストリップ](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

に変換すると、下線が削除されることに注意して `PagerTitleStrip` ください。 

## <a name="summary"></a>まとめ

このチュートリアルでは、 `ViewPager` を使用せずに基本的なベースのアプリを構築する方法の手順について説明しました `Fragment` 。 画像とキャプション文字列を含むデータソースの例、画像を `ViewPager` 表示するレイアウト、およびを `PagerAdapter` データソースに接続するサブクラスが示されて `ViewPager` います。 ユーザーがデータセット内を移動できるように、またはを追加して、 `PagerTabStrip` `PagerTitleStrip` 各ページの上部に画像キャプションを表示する方法を説明する手順が含まれていました。 

## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](/samples/xamarin/monodroid-samples/userinterface-treepager)