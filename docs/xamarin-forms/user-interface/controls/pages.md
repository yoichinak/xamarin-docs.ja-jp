---
title: Xamarin.Forms ページ
description: Xamarin.Forms ページは、クロス プラットフォーム モバイル アプリケーションの画面を表します。
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: bc1345bfaaf02464e2cf1ea0b3aceb28eb91e1d4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms ページ

_Xamarin.Forms ページは、クロス プラットフォーム モバイル アプリケーションの画面を表します。_

以下に説明したすべてのページの種類は、Xamarin.Forms から派生[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)クラスです。 これらのビジュアル要素は、すべてまたはほとんどの画面を占有します。 A`Page`オブジェクトが表す、 `ViewController` ios、および`Page`ユニバーサル Windows プラットフォームにします。 Android での各ページは、画面のように、`Activity`が Xamarin.Forms ページ*いない*`Activity`オブジェクト。

[ ![](pages-images/pages-sml.png "Xamarin.Forms ページの種類")](pages-images/pages.png#lightbox "Xamarin.Forms ページの種類")

## <a name="pages"></a>ページ数

Xamarin.Forms は、次のページの種類をサポートします。

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ページの最も一般的な単純型です。 設定、 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/)を 1 つのプロパティ[ `View` ](views.md)オブジェクトで、ほとんどの場合、 [ `Layout` ](layouts.md)など[ `StackLayout`](layouts.md#stackLayout)、 [ `Grid` ](layouts.md#grid)、または[ `ScrollView`](layouts.md#scrollView)です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![コンテンツ ページ例](pages-images/ContentPage.png "コンテンツ ページ例")](pages-images/ContentPage-Large.png#lightbox "コンテンツ ページの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)情報の 2 つのペインを管理します。 設定、 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)一般に、リストやメニューを表示するページにプロパティです。 設定、 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)マスター ページから、選択した項目を表示するページにプロパティです。 [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/)プロパティは、マスターまたは詳細ページが表示されているかどうかを制御します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![MasterDetailPage 例](pages-images/MasterDetailPage.png "MasterDetailPage 例")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)スタック ベースのアーキテクチャを使用して他のページ間のナビゲーションを管理します。 コンス トラクターをアプリケーションのページ ナビゲーションを使用する場合、ホーム ページのインスタンスを渡す必要があります、`NavigationPage`オブジェクト。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)、 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)、および[3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![NavigationPage 例](pages-images/NavigationPage.png "NavigationPage 例")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml)で[コードの背後にある =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 抽象から派生した[ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/)クラスし、子間のナビゲーション タブを使用して、ページをできるようにします。 設定、 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/)プロパティ ページ、またはセットのコレクションを[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/)データ オブジェクトのコレクションにプロパティと[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/)プロパティを[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)視覚的に表現されている各オブジェクトは、方法を説明します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)と[2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![TabbedPage 例](pages-images/TabbedPage.png "TabbedPage 例")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 抽象から派生した[ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/)クラスし、子間のナビゲーションに指を読み取ることによって、ページを使用します。 設定、 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/)プロパティのコレクションを[ `ContentPage` ](#contentPage)オブジェクト、またはセット、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/)データ オブジェクトのコレクションにプロパティと、[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/)プロパティを[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)視覚的に表現されている各オブジェクトは、方法を説明します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [ガイド](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)と[2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![CarouselPage 例](pages-images/CarouselPage.png "CarouselPage 例")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) コントロール テンプレート付きの全画面表示のコンテンツを表示しの基本クラスは、 [ `ContentPage`](#contentPage)です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage 例](pages-images/TemplatedPage.png "TemplatedPage 例")](pages-images/TemplatedPage.png "TemplatedPage 例") |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/root/Xamarin.Forms/)
