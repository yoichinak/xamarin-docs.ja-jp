---
title: Xamarin.Forms のページ
description: Xamarin.Forms のページでは、クロス プラットフォーム モバイル アプリケーション画面を表します。 この記事では、Xamarin.Forms に含まれているページが一覧表示します。
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 101b7aa014064e07de69e8c6b12f0ad1000ae0dd
ms.sourcegitcommit: 211fed94fb96127a3e158ae1ff5d7eb831a203d8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75955750"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms のページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin.Forms のページでは、クロス プラットフォーム モバイル アプリケーション画面を表します。_

以下に説明するすべてのページの種類は、Xamarin.Forms から派生[ `Page` ](xref:Xamarin.Forms.Page)クラス。 これらのビジュアル要素は、すべてまたはほとんどの画面が入ります。 A`Page`オブジェクトが表す、 `ViewController` ios と`Page`ユニバーサル Windows プラットフォームにします。 Android では、各ページは、画面のように、 `Activity`、Xamarin.Forms のページが、*いない*`Activity`オブジェクト。

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin.Forms には、次のページの種類がサポートされています。

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) ページの最も簡単で最も一般的な型です。 設定、 [ `Content` ](xref:Xamarin.Forms.ContentPage.Content)プロパティを 1 つ[ `View` ](views.md)オブジェクトで、ほとんどの場合、 [ `Layout` ](layouts.md)など[ `StackLayout`](layouts.md#stackLayout)、 [ `Grid` ](layouts.md#grid)、または[ `ScrollView`](layouts.md#scrollView)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPage) | [![ContentPage の例](pages-images/ContentPage.png "ContentPage の例")](pages-images/ContentPage-Large.png#lightbox "ContentPage の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)情報の 2 つのウィンドウを管理します。 設定、 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)プロパティを一般に、リストやメニューを表示するページ。 設定、 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)マスター ページから選択した項目を表示するページにプロパティ。 [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented)プロパティは、マスターまたは詳細ページが表示されるかどうかを制御します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MasterDetailPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Masterのページの例](pages-images/MasterDetailPage.png "Masterのページの例")](pages-images/MasterDetailPage-Large.png#lightbox "Masterのページの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)スタック ベースのアーキテクチャを使用して他のページ間のナビゲーションを管理します。 コンス トラクターにページ ナビゲーションをアプリケーションで使用する場合、ホーム ページのインスタンスを渡す必要があります、`NavigationPage`オブジェクト。<br /><br />[API ドキュメント](xref:Xamarin.Forms.NavigationPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)、および[3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![NavigationPage の例](pages-images/NavigationPage.png "NavigationPage の例")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml)で[コードの背後にある =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 抽象から派生した[ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1)クラスし、子間のナビゲーション タブを使用して、ページをできるようにします。 設定、 [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children)プロパティ ページ、またはセットのコレクションを[ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)データ オブジェクトのコレクションにプロパティと[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)各オブジェクトが表示される視覚的にする方法を説明します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TabbedPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)と[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![TabbedPage の例](pages-images/TabbedPage.png "TabbedPage の例")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 抽象から派生した[ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1)クラスし、子間のナビゲーションの本の指でスワイプ ページングを許可します。 設定、 [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children)プロパティのコレクションを[ `ContentPage` ](#contentPage)オブジェクト、またはセット、 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)データ オブジェクトのコレクションにプロパティおよび[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)各オブジェクトが表示される視覚的にする方法を説明します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.CarouselPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)と[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![CarouselPage の例](pages-images/CarouselPage.png "CarouselPage の例")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) コントロール テンプレートで全画面表示のコンテンツを表示し、基本クラスです[ `ContentPage`](#contentPage)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedPage) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedPage の例](pages-images/TemplatedPage.png "TemplatedPage の例")](pages-images/TemplatedPage.png "TemplatedPage の例") |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms FormsGallery サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
