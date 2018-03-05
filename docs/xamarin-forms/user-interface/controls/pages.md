---
title: "Xamarin.Forms ページ"
description: "Xamarin.Forms ページは、クロス プラットフォーム モバイル アプリの画面を表します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 35822dbbb7d5694e7f1f0a3f35f10df404206af9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms ページ

_Xamarin.Forms ページは、クロス プラットフォーム モバイル アプリの画面を表します。_

<style>.tableimg { max-width: none !important;}</style>

## <a name="pages"></a>ページ数

[ `Page` ](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page)クラスはビジュアル要素を画面のほとんどまたはすべてを占有し、1 つの子が含まれています。 A `Xamarin.Forms.Page` ios ビュー コント ローラーまたは Windows Phone でのページを表します。 Android の各ページは、画面のアクティビティと同様が Xamarin.Forms ページ*いない*アクティビティ。

 [ ![](pages-images/pages-sml.png "Xamarin.Forms ページの種類")](pages-images/pages.png "Xamarin.Forms ページの種類")

<br clear="all" />

Xamarin.Forms をサポートします。

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
  <thead>
    <th>
      <strong>型</strong>
    </th>
    <th>
      <strong>説明</strong>
    </th>
    <th style="min-width:400px">
      <strong>スクリーン ショット</strong>
    </th>
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">コンテンツ ページ</a>、1 つが表示されます<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">ビュー</a>のコンテナーなど、多くの場合、 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>または<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>です。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="コンテンツ ページの例" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">ページ</a>情報の 2 つのペインを管理します。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="MasterDetailPage 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">ページ</a>ナビゲーションおよびその他のページのスタックのユーザー エクスペリエンスを管理します。  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="NavigationPage 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">ページ</a>タブを使用して、ページが子間のナビゲーションことができます。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="TabbedPage 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">ページ</a>コントロール テンプレートとの基本クラスの全画面表示のコンテンツを表示する<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">コンテンツ ページ</a>です。
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="TemplatedPage 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">ページ</a>ギャラリーと同様に、サブページ間スワイプ ジェスチャを許可します。
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="CarouselPage 例" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms ギャラリー (サンプル)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API ドキュメント](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
