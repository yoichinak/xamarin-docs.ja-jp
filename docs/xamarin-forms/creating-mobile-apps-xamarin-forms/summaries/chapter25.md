---
title: 第 25 章の概要です。 ページの変数
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 25 章の概要。 ページの変数'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 148388b80137bd335bbb977ea230726da1f4a32d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995886"
---
# <a name="summary-of-chapter-25-page-varieties"></a>第 25 章の概要です。 ページの変数

ここまではから派生する 2 つのクラスを説明した`Page`:`ContentPage`と`NavigationPage`します。 この章では、他の 2 つ表示されます。

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 2 つのページ、マスターと詳細を管理します。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 複数の子ページのタブを通じてアクセスの管理します。

これらのページ型よりもより高度なナビゲーション オプションを提供する、`NavagationPage`で説明した[第 24 章です。ページ ナビゲーション](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md)します。

## <a name="master-and-detail"></a>マスター/詳細

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)型の 2 つのプロパティを定義`Page`: [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)と[ `Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)します。 通常、これらの各プロパティを設定、`ContentPage`します。 `MasterDetailPage`が表示され、これら 2 つのページに切り替えます。

これら 2 つのページ間を切り替える 2 つの基本的な方法はあります。

- *分割*マスターと詳細が並行して
- *ポップ オーバー*詳細ページがについて説明しますまたは、マスターが部分的にカバー ページ

いくつかのバリエーションがある、*ポップ オーバー*アプローチ (*スライド*、*重複*、および*スワップ*)、プラットフォームは、一般には依存します。 設定することができます、 [ `MasterDetailBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)プロパティの`MasterDetailPage`のメンバーに、 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterBehavior)列挙体。

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

ただし、このプロパティは、スマート フォン上の影響を与えません。 スマート フォンには、常にポップ オーバー動作があります。 タブレットとデスクトップの windows のみには、分割動作を持つことができます。

### <a name="exploring-the-behaviors"></a>動作の調査

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors)さまざまなデバイスで既定の動作で実験することができます。 プログラムには、2 つの異なるが含まれています。`ContentPage`マスター/詳細の派生物 (で、`Title`プロパティの両方に設定)、および別のクラスから派生した`MasterDetailPage`これらを結合します。 詳細ページがで囲まれた、`NavigationPage`のため、UWP プログラムなしで機能しません。

Windows 8.1 および Windows Phone 8.1 プラットフォームでは、ビットマップに設定することが必要です、`Icon`マスター ページのプロパティ。

### <a name="back-to-school"></a>新学期

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail)サンプルでは、若干異なるアプローチを採用から受講者を表示するプログラムを構築する、 [ **SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリ。

`Master`と`Detail`プロパティがビジュアル ツリー内で定義されている、 [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml)ファイルから派生した`MasterDetailPage`します。 この配置では、マスター/詳細ページ間で設定するデータ バインドを使用します。

XAML ファイルも設定される、 [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented)プロパティの`MasterDetailPage`に`True`します。 これにより、起動時に表示するマスター ページ既定では、詳細ページが表示されます。 [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs)ファイルのセット`IsPresented`に`false`から項目を選択すると、`ListView`でマスター ページ。 詳細ページが表示されます。

[![学校と詳細のスクリーン ショットをトリプル](images/ch25fg09-small.png "詳細ページ、MasterDetailPage から")](images/ch25fg09-large.png#lightbox "MasterDetailPage から詳細ページ")

### <a name="your-own-user-interface"></a>独自のユーザー インターフェイス

Xamarin.Forms には、マスター/詳細ビューを切り替えるためのユーザー インターフェイスが用意されていますを指定できます独自。 次の手順に従います。

- 設定、 [ `IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled)プロパティを`false`方向のスワイプ操作を無効にするには
- 上書き、 [ `ShouldShowToolbarButton` ](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton)メソッドと戻り値`false`Windows 8.1 および Windows Phone 8.1 のツール バー ボタンを非表示にします。

示されているなど、マスター/詳細ページ間で切り替える手段を提供する必要がありますし、 [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails)サンプル。

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps)サンプルでは別のアプローチを使用して、`TapGestureRecognizer`マスター/詳細ページにします。

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)タブを使用して切り替えるにはページのコレクションです。 派生した`MultiPage<Page>`し、独自のパブリック プロパティまたはメソッドは定義されません。 [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1)、ただし、プロパティを定義します。

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) 型のプロパティ `IList<T>`

これを入力する`Children`ページ オブジェクトのコレクション。

別の方法を定義することにより、`TabbedPage`子はほぼ同じように、`ListView`タブ付きページを自動的に生成する 2 つのプロパティを使用します。

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 型の `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 型の `DataTemplate`

ただし、この方法は使えません iOS でも、コレクションには、複数のいくつかの項目が含まれている場合。

`MultiPage<T>` ページが現在表示されているを追跡できる 2 つのプロパティを定義します。

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) 型の`T`ページを参照
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) 型の`Object`、内のオブジェクトを参照、`ItemsSource`コレクション

`MultiPage<T>` 2 つのイベントを定義します。

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) ときに、`ItemsSource`コレクションの変更
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) 表示されているページが変更されたとき

### <a name="discrete-tab-pages"></a>個別のタブ ページ

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors)サンプルは、次の 3 つの異なる方法で色を表示する 3 つのタブ付きページで構成されます。 各タブは、`ContentPage`派生クラスをクリックし、`TabbedPage`から派生[DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) 3 つのページを結合します。

表示されるページごとに、 `TabbedPage`、 `Title`  タブで、テキストを指定するプロパティが必要し、Apple ストアでは、同様に、アイコンを使用する必要があるため、`Icon`プロパティが iOS の設定。

[![個別のタブ付きの色の 3 倍になるスクリーン ショット](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes)サンプルでは、すべての学生を一覧表示するホーム ページ。 これに移動、学生がタップされたときに、`TabbedPage`派生クラス[ `StudentNotesDataPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml)が組み込まれている 3 つ`ContentPage`その学生の注意事項を入力することにより、うちの 1 つのビジュアル ツリー内のオブジェクトします。

### <a name="using-an-itemtemplate"></a>ItemTemplate を使用します。

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors)使用して、 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs)クラス、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。 [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml)ファイルのセット、`DataTemplate`プロパティの`TabbedPage`とビジュアル ツリーの先頭に`ContentPage`のプロパティへのバインドを格納している`NamedColor`(、へのバインドを含む`Title`プロパティ)。

ただし、これは、iOS で問題が発生します。 項目の一部のみを表示でき、およびそれらのアイコンを提供する優れた方法はありません。



## <a name="related-links"></a>関連リンク

- [第 25 章のフル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [第 25 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [マスター/詳細ページ](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [タブ付きページ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
