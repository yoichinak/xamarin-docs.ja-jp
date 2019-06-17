---
title: Xamarin.Forms のナビゲーション
description: このガイドでは、Xamarin.Forms アプリでナビゲーションを実行する方法について説明します。 Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 682e3bd0ac4cdd651203496dd28586db2cef3165
ms.sourcegitcommit: c2bba24233624c2ec0e9ee9827310ca022212a2c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66835265"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms のナビゲーション

_Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。_

![](images/page-types.png "Xamarin.Forms のページの種類")

または、Xamarin.Forms シェル アプリケーションでは、設定されたナビゲーション階層を適用しない URI ベースのナビゲーション エクスペリエンスが使われます。 詳細については、「[Xamarin.Forms シェルのナビゲーション](~/xamarin-forms/app-fundamentals/shell/navigation.md)」をご覧ください。

## <a name="hierarchical-navigationhierarchicalmd"></a>[階層ナビゲーション](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Form の [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms の [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) は、ギャラリーのように、ユーザーが端から端までスワイプしてコンテンツの各ページをナビゲートできるページです。

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms の [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) は、関連する情報の 2 つのページ – 項目を表示するマスター ページと、マスター ページ上の項目に関する詳細を提示する詳細ページを管理するページです。

## <a name="modal-pagesmodalmd"></a>[モーダル ページ](modal.md)

Xamarin.Forms ではモーダル ページもサポートされています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。
