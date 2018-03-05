---
title: "ビューで ViewPager"
description: "ViewPager は、レイアウト マネージャーで、ジェスチャー ナビゲーションを実装することができます。 ジェスチャー ナビゲーションにより、左と右のデータ ページの手順をスワイプするユーザー。 このガイドは、ViewPager とデータ ページとビューを使用して、PagerTabStrip swipeable UI を実装する方法を説明します。 (後続のガイドには、ページのフラグメントを使用する方法がについて説明します)。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d81f897fb7af39334cec4ea9f806533f09754079
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="viewpager-with-views"></a>ビューで ViewPager

_ViewPager は、レイアウト マネージャーで、ジェスチャー ナビゲーションを実装することができます。ジェスチャー ナビゲーションにより、左と右のデータ ページの手順をスワイプするユーザー。このガイドは、ViewPager とデータ ページとビューを使用して、PagerTabStrip swipeable UI を実装する方法を説明します。 (後続のガイドには、ページのフラグメントを使用する方法がについて説明します)。_

<a name="overview" />
 
## <a name="overview"></a>概要

このガイドは、ステップ バイ ステップのデモについては、使用する方法を提供するチュートリアル`ViewPager`各種と常緑木のイメージ ギャラリーを実装します。 左右「ツリー カタログ」このアプリでユーザーを読み取りますツリー画像を表示します。 カタログの各ページの上部には、ツリーの名前が表示されている、`PagerTabStrip`、ツリーのイメージが表示されます、`ImageView`です。 アダプターが使用されるインターフェイスを`ViewPager`を基になるデータ モデル。 このアプリから派生したアダプターを実装して`PagerAdapter`です。 

`ViewPager`-ベースのアプリは、一般に実装`Fragment`s、比較的単純なユース ケースがある場所の余分な複雑さ`Fragment`s は必要ありません。 たとえば、このチュートリアルで基本イメージ ギャラリーのアプリが不要の使用`Fragment`s。 コンテンツが静的では、さまざまなイメージの間で切り替えるユーザーのみカード、実装では、維持しておくことシンプルな標準の Android ビューとレイアウトを使用してため。 


<a name="start" />

## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

いう新しい Android プロジェクトを作成する**TreePager** (を参照してください[こんにちは, Android](~/android/get-started/hello-android/hello-android-quickstart.md)詳細については、Android プロジェクトの新規作成) します。 次に、NuGet パッケージ マネージャーを起動します。 (NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough))。 検索およびインストール**Android のサポート ライブラリ v4**: 

[![サポートのスクリーン ショット v4 Nuget の NuGet Package Manager での選択](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png)

によって、その他のパッケージ reaquired もインストールされる**Android のサポート ライブラリ v4**です。


<a name="datasource" />

## <a name="add-an-example-data-source"></a>例のデータ ソースを追加します。

この例では、ツリーのカタログのデータ ソースで (によって表される、`TreeCatalog`クラス) を提供、`ViewPager`項目の内容にします。 
`TreeCatalog` ツリーのイメージと、アダプターが作成するために使用するツリー タイトルの既製のコレクションを含む`View`s。 `TreeCatalog`コンス トラクターに引数が必要ないです。

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

コレクション内のイメージの`TreeCatalog`は各イメージは、インデクサーによってアクセスできるように構成されています。 たとえば、次のコード行は、コレクション内の 3 つ目のイメージのイメージ リソース ID を取得します。 

```csharp
int imageId = treeCatalog[2].imageId;
```

の実装の詳細`TreeCatalog`理解するには関係ない`ViewPager`、`TreeCatalog`コードが記載されていません。 ソース コードを`TreeCatalog`は、「 [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)です。 このソース ファイルをダウンロードする (またはコピーして、新しいコードを貼り付けます**TreeCatalog.cs**ファイル) をプロジェクトに追加します。 また、ダウンロードし、展開、[イメージ ファイル](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true)に、**リソース/描画**フォルダー、プロジェクトに含めるとします。 


<a name="layout" />

## <a name="create-a-viewpager-layout"></a>ViewPager レイアウトを作成します。

開いている**Resources/layout/Main.axml**しその内容を次の XML に置き換えます。

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


<a name="setup" />

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

このコードは次のとおり

1.  ビューを設定、 **Main.axml**レイアウト リソース。

2.  参照を取得、`ViewPager`レイアウトからです。

3.  新しいインスタンスを作成`TreeCatalog`データ ソースとして。

ビルドし、このコードを実行すると、次のスクリーン ショットのような画面が表示されます。 

[![空の ViewPager を表示するアプリのスクリーン ショット](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png)

この時点で、`ViewPager`それが不足しているアダプターのコンテンツにアクセスするためには何**TreeCatalog**です。 次のセクションで、 **PagerAdapter**接続するため、`ViewPager`を**TreeCatalog**です。 


<a name="adapter" />

## <a name="create-the-adapter"></a>アダプターを作成します。

`ViewPager` 間に位置するアダプター コント ローラー オブジェクトを使用して、`ViewPager`とデータ ソース (の図を参照してください[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter))。 このデータにアクセスするために`ViewPager`から派生したカスタム アダプターを指定する必要があります`PagerAdapter`です。 このアダプターでは追加の各`ViewPager`データ ソースからコンテンツを含むページです。 このデータ ソースは、アプリに固有であるため、カスタム アダプターは、データにアクセスする方法を理解しているコードを示します。 ページのユーザーのカードとして、 `ViewPager`、アダプターは、データ ソースから情報を抽出しのページに読み込まれます、`ViewPager`を表示します。 

実装する場合、`PagerAdapter`次をオーバーライドする必要があります。

-   **InstantiateItem** &ndash;ページを作成 (`View`) の指定した位置に追加し、`ViewPager`のビューのコレクション。 

-   **DestroyItem** &ndash;されたページの指定された位置から削除します。

-   **カウント**&ndash;読み取り専用のプロパティを使用可能なビュー (ページ) の数を返します。 

-   **IsViewFromObject** &ndash;ページが特定のキー オブジェクトに関連付けられているかどうかを判断します。 (このオブジェクトによって作成されて、`InstantiateItem`メソッドです)。この例では、キー オブジェクトが、`TreeCatalog`データ オブジェクト。

という名前の新しいファイルを追加**TreePagerAdapter.cs**しその内容を次のコードに置き換えます。 

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

このコードをスタブして、必要な`PagerAdapter`実装します。 次のセクションでこれらの各メソッドは作業中のコードで置き換えられます。 


<a name="ctor" />

### <a name="implement-the-constructor"></a>コンス トラクターを実装します。

アプリがインスタンス化、 `TreePagerAdapter`、コンテキストを提供 (、 `MainActivity`) と、インスタンス化された`TreeCatalog`です。 先頭に次のメンバー変数とコンス トラクターを追加、`TreePagerAdapter`クラス**TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

このコンス トラクターの目的は、コンテキストを格納する、および`TreeCatalog`インスタンスが、`TreePagerAdapter`が使用されます。 


<a name="count" />

### <a name="implement-count"></a>実装のカウント

`Count`実装が比較的単純な: ツリー カタログのツリーの数を返します。 `Count` を次のコードに置き換えます。

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees`プロパティ`TreeCatalog`データ セット内でツリー (ページの数) の数を返します。


<a name="instantiateitem" />

### <a name="implement-instantiateitem"></a>InstantiateItem を実装します。

`InstantiateItem`メソッドは、指定された位置のページを作成します。 新しく作成されたビューを追加する必要がありますも、`ViewPager`のコレクションを表示します。 可能であれば、そのために、`ViewPager`自体をコンテナー パラメーターとして渡します。 

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

このコードは次のとおり

1.  新しいインスタンスを作成`ImageView`指定位置にあるツリーのイメージを表示します。 アプリの`MainActivity`に渡されるコンテキスト、`ImageView`コンス トラクターです。

2.  セット、`ImageView`リソース、`TreeCatalog`画像の指定位置にあるリソースの ID。

3.  渡されたコンテナーではキャスト`View`を`ViewPager`参照します。
    使用する必要がある注意`JavaCast<ViewPager>()`(これは、必要な Android は、ランタイム チェックの型の変換を実行できるように) このキャストを正しく実行します。

4.  インスタンス化された追加`ImageView`を`ViewPager`を返します、`ImageView`呼び出し元にします。

ときに、`ViewPager`でイメージが表示されます`position`、これが表示されます`ImageView`です。 最初に、`InstantiateItem`ビューで最初の 2 つのページを設定するには、2 回呼び出されます。 ユーザーがスクロール、もう一度だけの背後にあると、現在表示されている項目の前のビューを維持するために呼び出されます。 


<a name="destroyitem" />

### <a name="implement-destroyitem"></a>DestroyItem を実装します。

`DestroyItem`メソッドは、指定した位置からページを削除します。 所定の位置にビューの変更できるアプリで`ViewPager`何らかの方法、新しいビューを交換する前にその位置にある古いビューを削除する必要があります。 `TreeCatalog`例では、各位置にあるビューが変更されないのでビューを削除して`DestroyItem`は単に再度追加する場合に`InstantiateItem`その位置に呼び出されます。 (リサイクルするプールを実装いずれかの効率を高めるため`View`を同じ位置に再表示されます)。 

`DestroyItem` メソッドを次のコードで置き換えます。 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

このコードは次のとおり

1.  渡されたコンテナーではキャスト`View`に、`ViewPager`参照します。

2.  渡された Java オブジェクトにキャスト (`view`) に、c# `View` (`view as View`) です。

3.  ビューが削除、`ViewPager`です。 


<a name="isviewfromobject" />

### <a name="implement-isviewfromobject"></a>Implement IsViewFromObject

ユーザー スライド左と右、コンテンツのページとして`ViewPager`呼び出し`IsViewFromObject`ことを確認する子`View`指定された位置には、その同じ位置に、アダプターのオブジェクトに関連付けられて (そのため、アダプターのオブジェクトが呼び出されると、*オブジェクト キー*)。 比較的単純なアプリは、関連付けが 1 つの id の&ndash;そのインスタンスで、アダプターのオブジェクトのキーに返された以前のビューである、`ViewPager`を介して`InstantiateItem`です。 ただし、他のアプリのオブジェクト キーがありますの何らかの他のアダプターに固有のクラスのインスタンスに関連付けられている (ただし、同じではありません)、子を表示する`ViewPager`その位置に表示します。 専用のアダプターでは、渡されたビューとオブジェクトのキーが関連付けられているかどうかを認識します。 

`IsViewFromObject` 実装する必要があります`PagerAdapter`を正常に機能します。 場合`IsViewFromObject`返します`false`指定された位置の`ViewPager`その位置にあるビューは表示されません。 `TreePager`アプリ、オブジェクトから返されたキー `InstantiateItem` *は*ページ`View`ツリーで、コードだけに身元を確認するための (つまり、オブジェクト キーと、ビューは 1 つの同じ)。 `IsViewFromObject` を次のコードに置き換えます。 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

<a name="addadapter" />

## <a name="add-the-adapter-to-the-viewpager"></a>アダプター、ViewPager を追加します。

これで、`TreePagerAdapter`に追加するとき、実装は、`ViewPager`です。 **MainActivity.cs**の末尾に次のコード行を追加、`OnCreate`メソッド。

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

このコードをインスタンス化、`TreePagerAdapter`を渡して、`MainActivity`コンテキストとして (`this`)。 インスタンス化された`TreeCatalog`は、コンス トラクターの 2 番目の引数に渡されます。 `ViewPager`の`Adapter`プロパティが設定されて、インスタンス化する`TreePagerAdapter`オブジェクトです。 このプラグインするとき、`TreePagerAdapter`に、`ViewPager`です。 

実装の中核はこれで完了&ndash;をビルドして、アプリを実行します。 左側の [次へ] のスクリーン ショットに示すように画面に表示ツリー カタログの最初のイメージが表示されます。 方向にスワイプして、複数のツリー ビューでは、左にツリー カタログ後方へ移動し、右の方向にスワイプ: 

[![ツリーのイメージをスワイプ スクリーン ショットの TreePager アプリ](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png)


<a name="pagetabstrip" />

## <a name="add-a-pager-indicator"></a>ポケットベル インジケーターを追加します。

この最小`ViewPager`の実装には、ツリーのカタログのイメージが表示されますが、ユーザーが、カタログ内にいるかを示す値が用意されていません。 次の手順が追加するには、`PagerTabStrip`です。 `PagerTabStrip`ページが表示され、前または次のページのヒントを表示することによってナビゲーション コンテキストを提供するユーザーに通知します。 `PagerTabStrip` 現在のページの指標として使用するものでは、 `ViewPager`; までスクロールし、各ページをユーザーのカードとして更新プログラムのことです。 

開いている**Resources/layout/Main.axml**を追加し、`PagerTabStrip`レイアウトに。

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

`ViewPager` および`PagerTabStrip`連携して動作するよう設計されています。 宣言する場合、`PagerTabStrip`内、`ViewPager`レイアウト、`ViewPager`自動的に検出されます、`PagerTabStrip`し、アダプターに接続します。 ビルド アプリを実行すると、空が表示されます`PagerTabStrip`各画面の上部に表示されます。 

[![空の PagerTabStrip のクローズ アップ スクリーン ショット](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png)


<a name="title" />

### <a name="display-a-title"></a>タイトルを表示します。

各ページのタブにタイトルを追加するには、実装、`GetPageTitleFormatted`メソッドで、 `PagerAdapter`-クラスを派生します。 `ViewPager` 呼び出し`GetPageTitleFormatted`(実装される場合) を指定した位置にあるページを表すタイトル文字列を取得します。 次のメソッドを追加、`TreePagerAdapter`クラス**TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

このコードは、ツリーのカタログの指定されたページ (位置) からツリー キャプション文字列を取得、Java に変換します`String`、し、それを返します、`ViewPager`です。 この新しいメソッドを使用してアプリを実行するときに各ページ キャプションを表示、ツリーで、`PagerTabStrip`です。 下線のない画面の上部にあるツリー名が表示されます。 

[![テキスト入力 PagerTabStrip タブ付きページのスクリーン ショット](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png)

カタログでキャプション ツリーの各イメージを表示するには、前後スワイプすることができます。 


<a name="pagertitlestrip" />

### <a name="pagertitlestrip-variation"></a>PagerTitleStrip バリエーション

`PagerTitleStrip` よく似ています`PagerTabStrip`する点を除いて`PagerTabStrip`現在選択されているタブの下線を追加します。置き換えることができます`PagerTabStrip`で`PagerTitleStrip`、上記のレイアウトと、アプリの外観を確認するには、もう一度の実行で`PagerTitleStrip`: 

[![テキストから削除された下線の付いた PagerTitleStrip](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png)

変換する場合、下線が削除されたことに注意してください`PagerTitleStrip`です。 


<a name="summary" />
 
## <a name="summary"></a>まとめ

このチュートリアルには、基本的な構築方法の手順の例が提供される`ViewPager`-を使用せずにアプリを基づいて`Fragment`s。 イメージとキャプションの文字列を含む例のデータ ソースを表示、 `ViewPager` 、画像を表示するレイアウトと`PagerAdapter`接続サブクラス、`ViewPager`データ ソースにします。 手順が含まれていたため、データ セット内を移動、ユーザーに、追加する方法を説明する、`PagerTabStrip`または`PagerTitleStrip`各ページの上部にあるイメージのキャプションを表示します。 


## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
