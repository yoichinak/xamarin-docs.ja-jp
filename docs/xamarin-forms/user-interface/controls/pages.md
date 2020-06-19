---
title: Xamarin.Forms Pages
description: Xamarin.Formsページは、クロスプラットフォームモバイルアプリケーション画面を表します。 この記事では、に含まれるページの一覧を示し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b72564ebbb057c8154140c9845961d4c42e93db3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573314"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms Pages

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin のフォームは、クロスプラットフォームのモバイルアプリケーション画面を表します。_

以下で説明するすべてのページの種類は、クラスから派生し Xamarin.Forms [`Page`](xref:Xamarin.Forms.Page) ます。 これらのビジュアル要素は、画面のすべてまたは大部分を占めます。 オブジェクトは、 `Page` `ViewController` iOS の、およびユニバーサル Windows プラットフォーム内のを表し `Page` ます。 Android では、各ページはのような画面を占め `Activity` ますが、 Xamarin.Forms ページはオブジェクトでは*ありません* `Activity` 。

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin.Formsでは、次のページの種類がサポートされています。

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage)は、最も単純で一般的な種類のページです。 プロパティを [`Content`](xref:Xamarin.Forms.ContentPage.Content) 1 つのオブジェクトに設定します [`View`](views.md) 。これは、ほとんどの場合 [`Layout`](layouts.md) 、、、など [`StackLayout`](layouts.md#stacklayout) [`Grid`](layouts.md#grid) [`ScrollView`](layouts.md#scrollview) です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPage) | [![ContentPage の例](pages-images/ContentPage.png "ContentPage の例")](pages-images/ContentPage-Large.png#lightbox "ContentPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| は、 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 2 つの情報ペインを管理します。 プロパティを、 [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 通常はリストまたはメニューを表示するページに設定します。 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)マスターページから選択されたアイテムを表示するページにプロパティを設定します。 プロパティは、 [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) マスターページまたは詳細ページを表示するかどうかを制御します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MasterDetailPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Masterのページの例](pages-images/MasterDetailPage.png "Masterのページの例")](pages-images/MasterDetailPage-Large.png#lightbox "Masterのページの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| は、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) スタックベースのアーキテクチャを使用して、他のページ間の移動を管理します。 アプリケーションでページナビゲーションを使用する場合は、ホームページのインスタンスをオブジェクトのコンストラクターに渡す必要があり `NavigationPage` ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.NavigationPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)、および[3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![NavigationPage の例](pages-images/NavigationPage.png "NavigationPage の例")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs)  /  の C# コード[コードの背後](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs)にある[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)抽象クラスから派生 [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) し、タブを使用して子ページ間を移動できるようにします。 プロパティを [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) ページのコレクションに設定するか、プロパティを [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) データオブジェクトのコレクションに設定し、 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティを、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 各オブジェクトを視覚的に表現する方法を説明するに設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TabbedPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![TabbedPage の例](pages-images/TabbedPage.png "TabbedPage の例")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)抽象クラスから派生 [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) し、指スワイプを通じて子ページ間を移動できるようにします。 プロパティを [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) オブジェクトのコレクションに設定するか、プロパティを [`ContentPage`](#contentpage) [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) データオブジェクトのコレクションに設定し、プロパティを、 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 各オブジェクトを視覚的に表現する方法を説明するに設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.CarouselPage)  / [ガイド](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![CarouselPage の例](pages-images/CarouselPage.png "CarouselPage の例")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)コントロールテンプレートを使用して全画面表示のコンテンツを表示します。これはの基本クラスです [`ContentPage`](#contentpage) 。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedPage)  / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedPage の例](pages-images/TemplatedPage.png "TemplatedPage の例")](pages-images/TemplatedPage.png "TemplatedPage の例") |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Formsフォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
