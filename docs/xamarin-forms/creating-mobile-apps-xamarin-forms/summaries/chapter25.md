---
title: '第 25 章の概要: さまざまなページ'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 25 章の概要: さまざまなページ'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: b86f2d7216a6344b14fc4d8c538ea68871eda5ae
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "70760544"
---
# <a name="summary-of-chapter-25-page-varieties"></a>第 25 章の概要: さまざまなページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

ここまで、`Page` から派生した 2 つのクラス `ContentPage` と `NavigationPage` を見てきました。 この章では、他の 2 つを紹介します。

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) では、マスターと詳細の 2 つのページが管理されます
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) では、タブを介してアクセスされる複数の子ページが管理されます

これらのページの種類では、`NavagationPage` (「[第 24 章: ページのナビゲーション](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md)」を参照) より高度なナビゲーション オプションが提供されます。

## <a name="master-and-detail"></a>マスターと詳細

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) では、`Page` 型の 2 つのプロパティ [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) と [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) が定義されています。 通常、これらの各プロパティは `ContentPage` に設定します。 `MasterDetailPage` では、これら 2 つのページが表示され、切り替えられます。

これら 2 つのページを切り替えるには、次の 2 つの基本的な方法があります。

- "*分割*" では、マスターと詳細が併置されます
- "*ポップオーバー*" では、詳細ページによってマスター ページが完全または部分的に覆われます

"*ポップオーバー*" アプローチにはいくつかのバリエーション ("*スライド*"、"*オーバーラップ*"、"*スワップ*") がありますが、これらは一般にプラットフォームに依存します。 `MasterDetailPage` の [`MasterDetailBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) プロパティを、[`MasterBehavior`](xref:Xamarin.Forms.MasterBehavior) 列挙型のメンバーに設定できます。

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

ただし、このプロパティは電話では何の影響もありません。 電話では常にポップオーバー動作になります。 分割動作を使用できるのは、タブレットとデスクトップ ウィンドウだけです。

### <a name="exploring-the-behaviors"></a>動作の調査

[**MasterDetailBehaviors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) サンプルを使用すると、さまざまなデバイスでの既定の動作を調べることができます。 このプログラムには、マスター用と詳細用に 2 つの独立した `ContentPage` の派生が含まれており (どちらでも `Title` プロパティが設定されています)、それらを組み合わせる `MasterDetailPage` の別の派生クラスが含まれます。 詳細ページは `NavigationPage` の中に入れられています。これは、そうなっていないと UWP プログラムが動かないためです。

Windows 8.1 および Windows Phone 8.1 プラットフォームでは、マスター ページの `Icon` プロパティにビットマップが設定されている必要があります。

### <a name="back-to-school"></a>学校に戻る

[**SchoolAndDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) サンプルでは、[**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) ライブラリとは若干異なる方法を使用して、学生を表示するプログラムが作成されています。

`Master` プロパティと `Detail` プロパティは、`MasterDetailPage` から派生した [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) ファイルのビジュアル ツリーで定義されています。 このように配置することで、マスター ページと詳細ページの間にデータ バインディングを設定できます。

その XAML ファイルでは、`MasterDetailPage` の [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) プロパティも `True` に設定されています。 これにより、起動時にマスター ページが表示されます。既定では、詳細ページが表示されます。 [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) ファイルでは、マスター ページの `ListView` から項目が選択されると、`IsPresented` が `false` に設定されます。 次に、詳細ページが表示されます。

[![School And Detail のトリプル スクリーンショット](images/ch25fg09-small.png "MasterDetailPage からの詳細ページ")](images/ch25fg09-large.png#lightbox "MasterDetailPage からの詳細ページ")

### <a name="your-own-user-interface"></a>独自のユーザー インターフェイス

Xamarin.Forms では、マスター ビューと詳細ビューを切り替えるためのユーザー インターフェイスが提供されていますが、独自のビューを提供することもできます。 次の手順に従います。

- [`IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) プロパティを `false` に設定して、スワイプを無効にします
- [`ShouldShowToolbarButton`](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) メソッドをオーバーライドし、`false` を返して Windows 8.1 と Windows Phone 8.1 でツール バー ボタンを非表示にします。

次に、マスター ページと詳細ページを切り替える手段を提供する必要があります。[**ColorsDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) サンプルで示されているようなものです。

[**MasterDetailTaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) サンプルでは、マスター ページと詳細ページで `TapGestureRecognizer` を使用する別の方法が示されています。

## <a name="tabbedpage"></a>TabbedPage

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブを使用して切り替えることができるページのコレクションです。 `MultiPage<Page>` から派生し、独自のパブリック プロパティまたはパブリック メソッドは定義されていません。 ただし、[`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1) ではプロパティが定義されています。

- `IList<T>` 型の [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) プロパティ

この `Children` コレクションに、ページ オブジェクトを設定します。

別の方法として、タブ付きページが自動的に生成される次の 2 つのプロパティを使用して、`ListView` のように `TabbedPage` の子を定義することもできます。

- `IEnumerable` 型の [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)
- `DataTemplate` 型の [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)

ただし、この方法は、iOS では、コレクションに含まれる項目が 2、3 個より多くなると、うまく機能しません。

`MultiPage<T>` ではさらに 2 つのプロパティが定義されており、それを使用すると現在表示されているページを追跡できます。

- `T` 型の [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) は、ページを参照します
- `Object` 型の [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) は、`ItemsSource` コレクション内のオブジェクトを参照します

`MultiPage<T>` では、次の 2 つのイベントも定義されています。

- `ItemsSource` コレクションが変更されたときの [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged)
- 表示されているページが変更されたときの [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged)

### <a name="discrete-tab-pages"></a>不連続なタブ ページ

[**DiscreteTabbedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) サンプルは、3 つの異なる方法で色が表示される 3 つのタブ付きページで構成されています。 各タブは `ContentPage` の派生であり、`TabbedPage` の派生 [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) によって 3 つのページが結合されます。

`TabbedPage` に表示される各ページでは、`Title` プロパティでタブ内のテキストを指定する必要があります。また、Apple ストアではアイコンを使用する必要もあるため、iOS の場合は `Icon` プロパティが設定されます。

[![Discrete Tabbed Colors のトリプル スクリーンショット](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[**StudentNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) サンプルには、すべての学生が一覧表示されるホーム ページがあります。 学生がタップされると、`TabbedPage` の派生である [`StudentNotesDataPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml) に移動します。そこでは、ビジュアル ツリーに 3 つの `ContentPage` オブジェクトが組み込まれており、そのうちの 1 つでは、その学生に対するメモを入力できます。

### <a name="using-an-itemtemplate"></a>ItemTemplate の使用

[**MultiTabbedColor**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) サンプルでは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) クラスが使用されています。 [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) ファイルでは、`TabbedPage` の `DataTemplate` プロパティが、`NamedColor` のプロパティへのバインドが含まれる (`Title` プロパティへのバインディングを含む) `ContentPage` で始まるビジュアル ツリーに設定されます。

ただし、これは iOS では問題になります。 いくつかの項目しか表示できず。アイコンを表示する適切な方法はありません。

## <a name="related-links"></a>関連リンク

- [第 25 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [第 25 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [マスター - 詳細ページ](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [タブ付きページ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
