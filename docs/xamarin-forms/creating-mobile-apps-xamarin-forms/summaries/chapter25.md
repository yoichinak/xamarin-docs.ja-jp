---
title: 第 25 章の概要です。 ページの種類
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 951ae41763d8338d5adf73fb46ebc6defa64f8f8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>第 25 章の概要です。 ページの種類

これまでに 2 つのクラスから派生するを見てきました`Page`:`ContentPage`と`NavigationPage`です。 この章では、他の 2 つのユーザーが表示されます。

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 2 つのページ、マスター詳細を管理します。
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 複数の子ページのタブを通じてアクセスを管理します。

これらのページの種類より高度なのナビゲーション オプションを提供、`NavagationPage`で説明した[章 24 です。ページ ナビゲーション](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md)です。

## <a name="master-and-detail"></a>マスター/詳細

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)型の 2 つのプロパティを定義`Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)と[ `Detail`](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)です。 通常、これらの各プロパティを設定、`ContentPage`です。 `MasterDetailPage`が表示され、これら 2 つのページに切り替えます。

これにはこれら 2 つのページ間を切り替える 2 つの基本的な方法があります。

- *分割*マスターと詳細がサイド バイ サイド
- *重なって*または詳細ページがカバー、マスターが部分的にカバー ページ

いくつかのバリエーションがある、*重なって*アプローチ (*スライド*、*重複*、および*スワップ*)、プラットフォームは、一般には依存します。 設定することができます、 [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/)のプロパティ`MasterDetailPage`のメンバーに、 [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/)列挙します。

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

ただし、このプロパティは、携帯電話に影響を与えません。 携帯電話には、常に重なって動作があります。 タブレットやデスクトップ ウィンドウのみには、分割動作を持つことができます。

### <a name="exploring-the-behaviors"></a>動作を調べる

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors)サンプルでは、さまざまなデバイス上の既定の動作をテストすることができます。 プログラムには、2 つの異なるが含まれています。`ContentPage`マスター/詳細の派生 (で、`Title`プロパティの両方に設定)、および別のクラスから派生した`MasterDetailPage`を結合します。 詳細ページがで囲まれた、 `NavigationPage` UWP プログラムがないと機能はありません。

Windows 8.1 および Windows Phone 8.1 のプラットフォームでは、ビットマップに設定することが必要、`Icon`マスター ページのプロパティです。

### <a name="back-to-school"></a>学校に戻る

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail)サンプルのアプローチを若干異なるから受講者を表示するプログラムの構築、 [ **SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリです。

`Master`と`Detail`のビジュアル ツリーを持つプロパティが定義されている、 [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml)から派生するファイル`MasterDetailPage`です。 このような配置では、マスター/詳細のページ間で設定するデータ バインドを許可します。

XAML ファイルも設定される、 [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/)プロパティ`MasterDetailPage`に`True`です。 これが原因でスタートアップ; に表示されるマスター ページ既定では、[詳細] ページが表示されます。 [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs)ファイルのセットを`IsPresented`に`false`から項目を選択すると、`ListView`がマスター ページ。 詳細ページが表示されます。

[![学校と詳細のスクリーン ショットをトリプル](images/ch25fg09-small.png "詳細ページ、MasterDetailPage から")](images/ch25fg09-large.png#lightbox "MasterDetailPage から詳細ページ")

### <a name="your-own-user-interface"></a>独自のユーザー インターフェイス

Xamarin.Forms では、マスター/詳細ビューを切り替えるためのユーザー インターフェイスを提供しますが、指定できます独自です。 次の手順に従います。

- 設定、 [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/)プロパティを`false`スワイプを無効にするには
- 上書き、 [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/)メソッドと戻り値`false`Windows 8.1 および Windows Phone 8.1 のツール バー ボタンを非表示にします。

必要がありますで示されるなど、マスター/詳細のページ間で切り替える手段を提供し、 [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails)サンプルです。

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps)サンプルでは別のアプローチを使用して、`TapGestureRecognizer`マスター/詳細ページにします。

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)タブを使用する間で切り替えることができるページのコレクションです。 派生して`MultiPage<Page>`し、独自のパブリック プロパティまたはメソッドが定義されません。 [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/)、ただし、で、プロパティを定義します。

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) 型のプロパティ `IList<T>`

これを入力する`Children`ページのオブジェクトのコレクション。

別の方法では、定義することができます、`TabbedPage`子とほぼ同様に、`ListView`タブ付きページを自動的に生成する 2 つのプロパティを使用します。

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) 型の `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) 型の `DataTemplate`

ただし、この方法では使えませんも iOS コレクションには、複数の項目が含まれている場合。

`MultiPage<T>` 追跡が現在どのページを表示するのに便利な 2 つの他のプロパティを定義します。

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) 型の`T`ページを参照します。
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) 型の`Object`内のオブジェクトを参照する、`ItemsSource`コレクション

`MultiPage<T>` 2 つのイベントも定義されています。

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) ときに、`ItemsSource`コレクションの変更
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) 表示されたページが変更されたとき

### <a name="discrete-tab-pages"></a>個別のタブ ページ

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors)サンプルは、次の 3 つの異なる方法で色を表示する次の 3 つのタブ付きページで構成されます。 各タブは、`ContentPage`から派生した、し、`TabbedPage`派生[DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) 3 つのページを結合します。

表示されるページごとに、 `TabbedPage`、 `Title`  タブで、テキストを指定するプロパティは必要であり、Apple ストアは、アイコンが同様に、使用することが必要ですので、`Icon`プロパティが iOS の設定。

[![個別のタブ付きの色のトリプル スクリーン ショット](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes)サンプルでは、すべての受講者を一覧するホーム ページです。 受講者をタップすると、これが移動する、`TabbedPage`から派生した、 [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml)、3 を組み込んだ`ContentPage`そのビジュアル ツリー内のうちの 1 つにより、その受講者のいくつかの注意事項を入力するオブジェクトします。

### <a name="using-an-itemtemplate"></a>ItemTemplate を使用します。

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors)サンプルは、 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。 [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml)ファイルのセットを`DataTemplate`プロパティ`TabbedPage`のビジュアル ツリーを最初に`ContentPage`のプロパティへのバインディングを格納している`NamedColor`(、へのバインドを含む`Title`プロパティ)。

ただし、これは iOS の問題です。 項目の一部のみが表示されることができ、アイコンを付与することはありません。



## <a name="related-links"></a>関連リンク

- [第 25 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [第 25 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [マスター/詳細 ページ](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [タブ ページ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
