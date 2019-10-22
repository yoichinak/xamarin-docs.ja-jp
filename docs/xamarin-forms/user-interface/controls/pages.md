---
title: Xamarin. フォームページ
description: Xamarin のフォームは、クロスプラットフォームのモバイルアプリケーション画面を表します。 この記事では、Xamarin. Forms に含まれるページの一覧を示します。
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 278256f75d94fe47510ae4d15f12a3ff3a6a2b19
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "69976443"
---
# <a name="xamarinforms-pages"></a>Xamarin. フォームページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin のフォームは、クロスプラットフォームのモバイルアプリケーション画面を表します。_

以下で説明するすべてのページの種類は、Xamarin. Forms [`Page`](xref:Xamarin.Forms.Page)クラスから派生します。 これらのビジュアル要素は、画面のすべてまたは大部分を占めます。 @No__t_0 オブジェクトは、iOS の `ViewController` とユニバーサル Windows プラットフォーム内の `Page` を表します。 Android では、各ページが `Activity` のような画面に表示されますが、Xamarin のフォームページは `Activity` オブジェクトでは*ありません*。

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin は、次のページの種類をサポートしています。

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage)は、最も単純で一般的な種類のページです。 [@No__t_1](xref:Xamarin.Forms.ContentPage.Content)プロパティを1つの[`View`](views.md)オブジェクトに設定します。これは、ほとんどの場合、 [`StackLayout`](layouts.md#stackLayout)、 [`Grid`](layouts.md#grid)、 [`Content`1](layouts.md#scrollView)などの[`Layout`](layouts.md)です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPage) | [![ContentPage の例](pages-images/ContentPage.png "ContentPage の例")](pages-images/ContentPage-Large.png#lightbox "ContentPage の例")<br />このページ  / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| [@No__t_1](xref:Xamarin.Forms.MasterDetailPage)は、2つの情報ウィンドウを管理します。 [@No__t_1](xref:Xamarin.Forms.MasterDetailPage.Master)プロパティを、通常はリストまたはメニューを表示するページに設定します。 マスターページから選択されたアイテムを表示するページに[`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)プロパティを設定します。 [@No__t_1](xref:Xamarin.Forms.MasterDetailPage.IsPresented)プロパティは、マスターページまたは詳細ページを表示するかどうかを制御します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MasterDetailPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Masterのページの例](pages-images/MasterDetailPage.png "Masterのページの例")](pages-images/MasterDetailPage-Large.png#lightbox "Masterのページの例")<br />このページのコード  / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [@No__t_1](xref:Xamarin.Forms.NavigationPage)は、スタックベースのアーキテクチャを使用して、他のページ間の移動を管理します。 アプリケーションでページナビゲーションを使用する場合は、ホームページのインスタンスを `NavigationPage` オブジェクトのコンストラクターに渡す必要があります。<br /><br />[API ドキュメント](xref:Xamarin.Forms.NavigationPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)、および[3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![NavigationPage の例](pages-images/NavigationPage.png "NavigationPage の例")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage の例")<br />このページのコード  / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml)と[コード = 分離](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)は、抽象[`MultiPage`](xref:Xamarin.Forms.MultiPage`1)クラスから派生し、タブを使用して子ページ間を移動できます。 [@No__t_1](xref:Xamarin.Forms.MultiPage`1.Children)プロパティをページのコレクションに設定するか、 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)プロパティをデータオブジェクトのコレクションに設定し、 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティを、各オブジェクトを視覚的に表現する方法を説明する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TabbedPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![TabbedPage の例](pages-images/TabbedPage.png "TabbedPage の例")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage の例")<br />このページ  / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)は、抽象[`MultiPage`](xref:Xamarin.Forms.MultiPage`1)クラスから派生し、指スワイプによって子ページ間を移動できるようにします。 [@No__t_1](xref:Xamarin.Forms.MultiPage`1.Children)プロパティを[`ContentPage`](#contentPage)オブジェクトのコレクションに設定するか、または[`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)プロパティをデータオブジェクトのコレクションに設定し、 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティを各オブジェクトを視覚的に表現する方法を示す[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。示さ.<br /><br />[API ドキュメント](xref:Xamarin.Forms.CarouselPage) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![CarouselPage の例](pages-images/CarouselPage.png "CarouselPage の例")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage の例")<br />このページ  / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)は、コントロールテンプレートを使用して全画面コンテンツを表示します。これは[`ContentPage`](#contentPage)の基本クラスです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedPage) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage の例](pages-images/TemplatedPage.png "TemplatedPage の例")](pages-images/TemplatedPage.png "TemplatedPage の例") |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
