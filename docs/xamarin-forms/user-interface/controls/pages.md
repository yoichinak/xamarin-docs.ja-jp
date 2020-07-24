---
title: ':::no-loc(Xamarin.Forms)::: Pages'
description: ':::no-loc(Xamarin.Forms):::ページは、クロスプラットフォームモバイルアプリケーション画面を表します。 この記事では、に含まれるページの一覧を示し :::no-loc(Xamarin.Forms)::: ます。'
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: ca7e98e4e955ac45a3813a3c1cde97ab0939853a
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997268"
---
# <a name="no-locxamarinforms-pages"></a>:::no-loc(Xamarin.Forms)::: Pages

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_:::no-loc(Xamarin.Forms):::ページは、クロスプラットフォームモバイルアプリケーション画面を表します。_

以下で説明するすべてのページの種類は、クラスから派生し :::no-loc(Xamarin.Forms)::: [`Page`](xref::::no-loc(Xamarin.Forms):::.Page) ます。 これらのビジュアル要素は、画面のすべてまたは大部分を占めます。 オブジェクトは、 `Page` `ViewController` iOS の、およびユニバーサル Windows プラットフォーム内のを表し `Page` ます。 Android では、各ページはのような画面を占め `Activity` ますが、 :::no-loc(Xamarin.Forms)::: ページはオブジェクトでは*ありません* `Activity` 。

[![::: no-loc (Xamarin. Forms)::: ページの種類](pages-images/pages-sml.png)](pages-images/pages.png#lightbox "::: no-loc (Xamarin. Forms)::: ページの種類")

## <a name="pages"></a>ページ

:::no-loc(Xamarin.Forms):::では、次のページの種類がサポートされています。

| 種類 | 説明 | 外観 |
| --- | --- | --- |
| `ContentPage` | [`ContentPage`](xref::::no-loc(Xamarin.Forms):::.ContentPage)は、最も単純で一般的な種類のページです。 プロパティを [`Content`](xref::::no-loc(Xamarin.Forms):::.ContentPage.Content) 1 つのオブジェクトに設定します [`View`](views.md) 。これは、ほとんどの場合 [`Layout`](layouts.md) 、、、など [`StackLayout`](xref::::no-loc(Xamarin.Forms):::.StackLayout) [`Grid`](xref::::no-loc(Xamarin.Forms):::.Grid) [`ScrollView`](xref::::no-loc(Xamarin.Forms):::.ScrollView) です。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.ContentPage) | [![ContentPage の例](pages-images/ContentPage.png "ContentPage の例")](pages-images/ContentPage-Large.png#lightbox "ContentPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
| `MasterDetailPage` | は、 [`MasterDetailPage`](xref::::no-loc(Xamarin.Forms):::.MasterDetailPage) 2 つの情報ペインを管理します。 プロパティを、 [`Master`](xref::::no-loc(Xamarin.Forms):::.MasterDetailPage.Master) 通常はリストまたはメニューを表示するページに設定します。 [`Detail`](xref::::no-loc(Xamarin.Forms):::.MasterDetailPage.Detail)マスターページから選択されたアイテムを表示するページにプロパティを設定します。 プロパティは、 [`IsPresented`](xref::::no-loc(Xamarin.Forms):::.MasterDetailPage.IsPresented) マスターページまたは詳細ページを表示するかどうかを制御します。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.MasterDetailPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Masterのページの例](pages-images/MasterDetailPage.png "Masterのページの例")](pages-images/MasterDetailPage-Large.png#lightbox "Masterのページの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) |
| `NavigationPage` | は、 [`NavigationPage`](xref::::no-loc(Xamarin.Forms):::.NavigationPage) スタックベースのアーキテクチャを使用して、他のページ間の移動を管理します。 アプリケーションでページナビゲーションを使用する場合は、ホームページのインスタンスをオブジェクトのコンストラクターに渡す必要があり `NavigationPage` ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.NavigationPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)、および[3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![NavigationPage の例](pages-images/NavigationPage.png "NavigationPage の例")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs)  /  の C# コード[コードの背後](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs)にある[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) |
| `TabbedPage` | [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage)抽象クラスから派生 [`MultiPage`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1) し、タブを使用して子ページ間を移動できるようにします。 プロパティを [`Children`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1.Children) ページのコレクションに設定するか、プロパティを [`ItemsSource`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1.ItemsSource) データオブジェクトのコレクションに設定し、 [`ItemTemplate`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1.ItemTemplate) プロパティを、 [`DataTemplate`](xref::::no-loc(Xamarin.Forms):::.DataTemplate) 各オブジェクトを視覚的に表現する方法を説明するに設定します。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.TabbedPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![TabbedPage の例](pages-images/TabbedPage.png "TabbedPage の例")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
| `CarouselPage` | [`CarouselPage`](xref::::no-loc(Xamarin.Forms):::.CarouselPage)抽象クラスから派生 [`MultiPage`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1) し、指スワイプを通じて子ページ間を移動できるようにします。 プロパティを [`Children`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1.Children) オブジェクトのコレクションに設定するか、プロパティを [`ContentPage`](xref::::no-loc(Xamarin.Forms):::.ContentPage) [`ItemsSource`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1.ItemsSource) データオブジェクトのコレクションに設定し、プロパティを、 [`ItemTemplate`](xref::::no-loc(Xamarin.Forms):::.MultiPage`1.ItemTemplate) [`DataTemplate`](xref::::no-loc(Xamarin.Forms):::.DataTemplate) 各オブジェクトを視覚的に表現する方法を説明するに設定します。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.CarouselPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![CarouselPage の例](pages-images/CarouselPage.png "CarouselPage の例")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
| `TemplatedPage` | [`TemplatedPage`](xref::::no-loc(Xamarin.Forms):::.TemplatedPage)コントロールテンプレートを使用して全画面表示のコンテンツを表示します。これはの基本クラスです [`ContentPage`](xref::::no-loc(Xamarin.Forms):::.ContentPage) 。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.TemplatedPage)  / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedPage の例](pages-images/TemplatedPage.png "TemplatedPage の例")](pages-images/TemplatedPage.png "TemplatedPage の例") |
|     |     |     |

## <a name="related-links"></a>関連リンク

- [:::no-loc(Xamarin.Forms):::フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms)::: サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=:::no-loc(Xamarin.Forms):::)
- [:::no-loc(Xamarin.Forms):::API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
