---
title: Xamarin.Forms のページ
description: Xamarin.Forms のページでは、クロス プラットフォーム モバイル アプリケーション画面を表します。 この記事では、Xamarin.Forms に含まれているページが一覧表示します。
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: fea1db6c65e533692601439f4712d15371740644
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995899"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms のページ

_Xamarin.Forms のページでは、クロス プラットフォーム モバイル アプリケーション画面を表します。_

以下に説明するすべてのページの種類は、Xamarin.Forms から派生[ `Page` ](xref:Xamarin.Forms.Page)クラス。 これらのビジュアル要素は、すべてまたはほとんどの画面が入ります。 A`Page`オブジェクトが表す、 `ViewController` ios と`Page`ユニバーサル Windows プラットフォームにします。 Android では、各ページは、画面のように、 `Activity`、Xamarin.Forms のページが、*いない*`Activity`オブジェクト。

[ ![](pages-images/pages-sml.png "Xamarin.Forms のページの種類")](pages-images/pages.png#lightbox "Xamarin.Forms ページの種類")

## <a name="pages"></a>ページ数

Xamarin.Forms には、次のページの種類がサポートされています。

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) ページの最も簡単で最も一般的な型です。 設定、 [ `Content` ](xref:Xamarin.Forms.ContentPage.Content)プロパティを 1 つ[ `View` ](views.md)オブジェクトで、ほとんどの場合、 [ `Layout` ](layouts.md)など[ `StackLayout`](layouts.md#stackLayout)、 [ `Grid` ](layouts.md#grid)、または[ `ScrollView`](layouts.md#scrollView)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPage) | [![コンテンツ ページ例](pages-images/ContentPage.png "ContentPage 例")](pages-images/ContentPage-Large.png#lightbox "ContentPage の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)情報の 2 つのウィンドウを管理します。 設定、 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)プロパティを一般に、リストやメニューを表示するページ。 設定、 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)マスター ページから選択した項目を表示するページにプロパティ。 [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented)プロパティは、マスターまたは詳細ページが表示されるかどうかを制御します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MasterDetailPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![MasterDetailPage 例](pages-images/MasterDetailPage.png "MasterDetailPage 例")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)スタック ベースのアーキテクチャを使用して他のページ間のナビゲーションを管理します。 コンス トラクターにページ ナビゲーションをアプリケーションで使用する場合、ホーム ページのインスタンスを渡す必要があります、`NavigationPage`オブジェクト。<br /><br />[API ドキュメント](xref:Xamarin.Forms.NavigationPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)、 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)、および[3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![NavigationPage 例](pages-images/NavigationPage.png "NavigationPage 例")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml)で[コードの背後にある =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 抽象から派生した[ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1)クラスし、子間のナビゲーション タブを使用して、ページをできるようにします。 設定、 [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children)プロパティ ページ、またはセットのコレクションを[ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)データ オブジェクトのコレクションにプロパティと[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)各オブジェクトが表示される視覚的にする方法を説明します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TabbedPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)と[2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![TabbedPage 例](pages-images/TabbedPage.png "TabbedPage 例")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 抽象から派生した[ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1)クラスし、子間のナビゲーションの本の指でスワイプ ページングを許可します。 設定、 [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children)プロパティのコレクションを[ `ContentPage` ](#contentPage)オブジェクト、またはセット、 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)データ オブジェクトのコレクションにプロパティおよび[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)各オブジェクトが表示される視覚的にする方法を説明します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.CarouselPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)と[2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![CarouselPage 例](pages-images/CarouselPage.png "CarouselPage 例")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) コントロール テンプレートで全画面表示のコンテンツを表示し、基本クラスです[ `ContentPage`](#contentPage)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedPage) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage 例](pages-images/TemplatedPage.png "TemplatedPage 例")](pages-images/TemplatedPage.png "TemplatedPage 例") |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
