---
title: ViewPager とビュー
description: ViewPager は、レイアウト マネージャー ジェスチャー ナビゲーションを実装することができます。 ジェスチャー ナビゲーションでは、左および右のデータ ページの手順をユーザーがスワイプができます。 このガイドは、ViewPager と PagerTabStrip、データ ページとしてビューを使用すると、スワイプ可能な UI を実装する方法を説明します。 (後続のガイドでは、ページのフラグメントを使用する方法を説明します)。
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a8b7fa53d3384821d028e4a88ba22071a17e5bd9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113925"
---
# <a name="viewpager-with-views"></a>ViewPager とビュー

_ViewPager は、レイアウト マネージャー ジェスチャー ナビゲーションを実装することができます。ジェスチャー ナビゲーションでは、左および右のデータ ページの手順をユーザーがスワイプができます。このガイドは、ViewPager と PagerTabStrip、データ ページとしてビューを使用すると、スワイプ可能な UI を実装する方法を説明します。 (後続のガイドでは、ページのフラグメントを使用する方法を説明します)。_

 
## <a name="overview"></a>概要

このガイドは、ステップ バイ ステップ デモに使用する方法を説明するチュートリアル`ViewPager`各種と付きまとう木の画像ギャラリーの実装にします。 ユーザーが「カタログのツリー」を左右にスワイプこのアプリで画像のツリーを表示します。 カタログの各ページの上部にあるツリーの名前が表示されている、`PagerTabStrip`、ツリーのイメージが表示されます、`ImageView`します。 アダプターが使用されるインターフェイスを`ViewPager`を基になるデータ モデルにします。 このアプリから派生したアダプターを実装する`PagerAdapter`します。 

`ViewPager`-ベースのアプリは多くの場合に実装`Fragment`s、な比較的単純なユース ケースがある場所の複雑さが増して`Fragment`s は必要ありません。 このチュートリアルで示す基本的なイメージのギャラリー アプリでの使用が必要ありません、`Fragment`秒。 コンテンツは静的でありのみカードのユーザーに異なるイメージ間実装保持できる単純な標準の Android ビューとレイアウトを使用しています。 



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

という新しい Android プロジェクトを作成**TreePager** (を参照してください[こんにちは, Android](~/android/get-started/hello-android/hello-android-quickstart.md)詳細については、新しい Android プロジェクトの作成) します。 次に、NuGet パッケージ マネージャーを起動します。 (NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: NuGet でプロジェクトを含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough))。 検索およびインストール**Android サポート ライブラリ v4**: 

[![サポートのスクリーン ショット v4 の Nuget、NuGet パッケージ マネージャーを選択](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

追加のパッケージ reaquired もインストールされる**Android サポート ライブラリ v4**します。



## <a name="add-an-example-data-source"></a>例のデータ ソースを追加します。

この例では、ツリーのカタログのデータ ソースで (によって表される、`TreeCatalog`クラス) を提供、`ViewPager`項目の内容にします。 
`TreeCatalog` ツリーのイメージと、アダプターが作成するために使用するツリーのタイトルの既製のコレクションを含む`View`秒。 `TreeCatalog`コンス トラクター引数は必要ありません。

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

コレクション内のイメージの`TreeCatalog`は各イメージは、インデクサーでアクセスできるように構成されています。 たとえば、次のコード行は、コレクション内の 3 つ目のイメージのイメージ リソース ID を取得します。 

```csharp
int imageId = treeCatalog[2].imageId;
```

の実装の詳細`TreeCatalog`関係を理解することのない`ViewPager`、`TreeCatalog`コードがここに表示されません。 ソース コードを`TreeCatalog`いただけます[TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)します。 このソース ファイルをダウンロード (またはコピーして、新しいコードを貼り付けます**TreeCatalog.cs**ファイル)、プロジェクトに追加します。 また、ダウンロードして解凍、[イメージ ファイル](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true)に、**リソース/drawable**フォルダーと、プロジェクトに追加します。 



## <a name="create-a-viewpager-layout"></a>ViewPager レイアウトを作成します。

開いている**Resources/layout/Main.axml**内容を次の XML に置き換えます。

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

このコードは、次を行います。

1.  設定から、ビュー、 **Main.axml**レイアウトのリソース。

2.  参照を取得、`ViewPager`レイアウトから。

3.  新しいインスタンスを作成`TreeCatalog`データ ソースとして。

ビルドして、このコードを実行するときに、次のスクリーン ショットのような画面が表示されます。 

[![空の ViewPager を表示するアプリのスクリーン ショット](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

この時点で、`ViewPager`内のコンテンツにアクセスするため、アダプターが不足している、ためには何**TreeCatalog**します。 次のセクションで、 **PagerAdapter**接続するため、`ViewPager`を**TreeCatalog**します。 


## <a name="create-the-adapter"></a>アダプターを作成します。

`ViewPager` 間に位置するアダプター コント ローラー オブジェクトを使用して、`ViewPager`とデータ ソース (の図を参照してください。[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter))。 このデータにアクセスするために`ViewPager`から派生したカスタム アダプターを指定する必要があります`PagerAdapter`します。 このアダプターの設定の各`ViewPager`データ ソースからコンテンツを含むページ。 このデータ ソースは、アプリに固有であるため、カスタム アダプターは、データにアクセスする方法を理解しているコードを示します。 ページのユーザーのカードとして、 `ViewPager`、アダプターは、データ ソースから情報を抽出しのページに読み込みます、`ViewPager`を表示します。 

実装する場合、`PagerAdapter`次をオーバーライドする必要があります。

-   **InstantiateItem** &ndash;ページを作成します (`View`) の指定した位置に追加します、`ViewPager`のビューのコレクション。 

-   **DestroyItem** &ndash;ページの指定された位置から削除します。

-   **カウント**&ndash;利用可能なビュー (ページ) の数を返す、読み取り専用プロパティ。 

-   **IsViewFromObject** &ndash;ページが特定のキー オブジェクトに関連付けられているかどうかを判断します。 (このオブジェクトがによって作成された、`InstantiateItem`メソッドです)。この例では、キー オブジェクトは、`TreeCatalog`データ オブジェクト。

という新しいファイルを追加**TreePagerAdapter.cs**し、その内容を次のコードに置き換えます。 

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

このコードをスタブして、必要な`PagerAdapter`実装します。 次のセクションでこれらの各メソッドは作業中のコードに置き換えられます。 



### <a name="implement-the-constructor"></a>コンス トラクターを実装します。

アプリがインスタンス化、 `TreePagerAdapter`、コンテキストが指定されます (、 `MainActivity`) およびインスタンス化された`TreeCatalog`します。 先頭に次のメンバー変数とコンス トラクターを追加、`TreePagerAdapter`クラス**TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

このコンス トラクターの目的は、コンテキストを格納して`TreeCatalog`インスタンスが、`TreePagerAdapter`が使用されます。 



### <a name="implement-count"></a>実装の数

`Count`実装が比較的単純な: ツリー カタログのツリーの数を返します。 `Count` を次のコードに置き換えます。

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees`プロパティの`TreeCatalog`データ セット内でツリー (ページの数) の数を返します。



### <a name="implement-instantiateitem"></a>InstantiateItem を実装します。

`InstantiateItem`メソッドは、指定した位置のページを作成します。 新しく作成されたビューを追加する必要がありますも、`ViewPager`のコレクションを表示します。 これを実現する、`ViewPager`自体をコンテナー パラメーターとして渡します。 

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

このコードは、次を行います。

1.  新しいインスタンスを作成`ImageView`指定位置にあるツリーのイメージを表示します。 アプリの`MainActivity`に渡されるコンテキスト、`ImageView`コンス トラクター。

2.  セット、`ImageView`リソースを`TreeCatalog`イメージの指定位置にあるリソースの ID。

3.  渡されたコンテナーではキャスト`View`を`ViewPager`参照。
    使用する必要がありますので注意`JavaCast<ViewPager>()`(これが必要な Android ランタイム チェックの型変換を実行できるように) このキャストを正しく実行します。

4.  追加のインスタンス化された`ImageView`を`ViewPager`を返します、`ImageView`呼び出し元にします。

ときに、`ViewPager`で画像を表示します。 `position`、これが表示されます`ImageView`します。 最初に、`InstantiateItem`はビューで最初の 2 つのページを設定するには、2 回呼び出されます。 ユーザーがスクロールするともう一度だけ背後にあると、現在表示されている項目の前のビューを維持するために呼び出されます。 



### <a name="implement-destroyitem"></a>DestroyItem を実装します。

`DestroyItem`メソッドは、指定した位置からページを削除します。 所定の位置にビューの変更できるアプリで`ViewPager`何らかの方法で新しいビューを交換する前にその位置にある古いビューを削除する必要があります。 `TreeCatalog`例では、各位置にあるビューが変更されないのでビューを削除して`DestroyItem`として扱われます再追加したときに`InstantiateItem`その位置に呼び出されます。 (効率を高めるため、プールをリサイクルする 1 つ実装`View`を同じ位置に再表示されます)。 

`DestroyItem` メソッドを次のコードで置き換えます。 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

このコードは、次を行います。

1.  渡されたコンテナーではキャスト`View`に、`ViewPager`参照。

2.  渡された Java オブジェクトにキャスト (`view`) に、 C# `View` (`view as View`)。

3.  ビューを削除、`ViewPager`します。 



### <a name="implement-isviewfromobject"></a>Implement IsViewFromObject

ユーザー スライド左と、コンテンツのページの右`ViewPager`呼び出し`IsViewFromObject`ことを確認する子`View`指定された位置には、その同じ位置のアダプターのオブジェクトに関連付けられた (そのため、アダプターのオブジェクトが呼び出されると、*オブジェクト キー*)。 アソシエーションでは、比較的単純なアプリは id のいずれか&ndash;そのインスタンスで、アダプターのオブジェクト キーが以前に返された、ビュー、`ViewPager`を介して`InstantiateItem`します。 ただし、他のアプリ オブジェクト キーがありますが関連付けられているその他の何らかのアダプター固有のクラス インスタンス (ただしとは異なります)、子を表示する`ViewPager`その位置に表示します。 専用のアダプターでは、渡されたビューとオブジェクトのキーが関連付けられているかどうかを認識します。 

`IsViewFromObject` 実装する必要があります`PagerAdapter`適切に機能します。 場合`IsViewFromObject`返します`false`、指定された位置の`ViewPager`その位置にあるビューは表示されません。 `TreePager`アプリ、オブジェクトによって返されるキー `InstantiateItem` *は*ページ`View`コードは、id を確認するだけがあるできるように、ツリーの (つまり、オブジェクト キーと、ビューは 1 つ)。 `IsViewFromObject` を次のコードに置き換えます。 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>アダプター、ViewPager を追加します。

これで、`TreePagerAdapter`が実装されると、いよいよに追加する、`ViewPager`します。 **MainActivity.cs**の末尾に次のコード行を追加、`OnCreate`メソッド。

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

このコードをインスタンス化、`TreePagerAdapter`で渡し、`MainActivity`コンテキストとして (`this`)。 インスタンス化された`TreeCatalog`コンス トラクターの 2 番目の引数に渡されます。 `ViewPager`の`Adapter`プロパティを設定する、インスタンス化された`TreePagerAdapter`オブジェクト。 このプラグインするとき、`TreePagerAdapter`に、 `ViewPager`。 

実装の中核はこれで完了&ndash;をビルドして、アプリを実行します。 左側の [次へ] のスクリーン ショットに示すように、画面に表示ツリー カタログの最初のイメージが表示されます。 スワイプして、複数のツリー ビューでは、左スワイプ右ツリー カタログ後方へ移動する: 

[![ツリーのイメージをスワイプ スクリーン ショットの TreePager アプリ](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>ポケットベル インジケーターを追加します。

この最小`ViewPager`の実装には、カタログ ツリーのイメージが表示されますが、カタログ内のユーザーのいるかを示す値は提供されません。 次の手順が追加するには、`PagerTabStrip`します。 `PagerTabStrip`ページが表示され、前または次のページのヒントを表示することで、ナビゲーションのコンテキストを提供しますがユーザーに通知されます。 `PagerTabStrip` 現在のページを示すインジケーターとして使用するためのものでは、 `ViewPager`; までスクロールし、各ページをユーザーのカードとして更新プログラムのことです。 

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

`ViewPager` `PagerTabStrip`は連携して動作するように設計します。 宣言すると、`PagerTabStrip`内で、`ViewPager`レイアウト、`ViewPager`が自動的に検出、`PagerTabStrip`し、アダプターに接続します。 空がわかりますビルド アプリを実行すると、`PagerTabStrip`各画面の上部に表示されます。 

[![空の PagerTabStrip のクローズ アップ スクリーン ショット](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>タイトルを表示します。

各ページのタブにタイトルを追加するには、実装、`GetPageTitleFormatted`メソッドで、 `PagerAdapter`-クラスを派生します。 `ViewPager` 呼び出し`GetPageTitleFormatted`(実装される) 場合、指定した位置にあるページを表すタイトル文字列を取得します。 次のメソッドを追加、`TreePagerAdapter`クラス**TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

このコードはツリーのカタログの指定されたページ (位置) からツリー キャプションの文字列を取得、Java に変換する`String`、し、それを返します、`ViewPager`します。 この新しいメソッドを使用してアプリを実行するときに各ページ キャプションを表示、ツリーで、`PagerTabStrip`します。 下線なし、画面の上部にあるツリー名が表示されます。 

[![テキスト入力 PagerTabStrip タブ ページのスクリーン ショット](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

カタログのキャプションのツリーの各イメージを表示するには、前後へスワイプすることができます。 



### <a name="pagertitlestrip-variation"></a>PagerTitleStrip バリエーション

`PagerTitleStrip` よく似ています`PagerTabStrip`する点を除いて`PagerTabStrip`現在選択されているタブの下線を追加します。置き換えることができます`PagerTabStrip`で`PagerTitleStrip`で上記のレイアウトと、アプリの外観を確認するには、もう一度実行`PagerTitleStrip`: 

[![テキストから削除する下線の付いた PagerTitleStrip](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

変換する場合、下線が削除されたことに注意してください。`PagerTitleStrip`します。 


 
## <a name="summary"></a>まとめ

このチュートリアルには、基本的なを構築する方法の手順の例が提供されている`ViewPager`-ベースのアプリを使用せず`Fragment`秒。 イメージとキャプションの文字列を含む例のデータ ソースを表示、 `ViewPager` 、画像を表示するレイアウトと`PagerAdapter`接続サブクラス、`ViewPager`データ ソースにします。 手順が含まれていたため、データ セット間を移動するユーザーに、追加する方法を説明する、`PagerTabStrip`または`PagerTitleStrip`各ページの上部にあるイメージのキャプションを表示します。 


## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
