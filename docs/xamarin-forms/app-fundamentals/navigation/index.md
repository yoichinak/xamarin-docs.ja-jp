---
title: Xamarin.Forms のナビゲーション
description: このガイドでは、Xamarin.Forms アプリでナビゲーションを実行する方法について説明します。 Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994729"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms のナビゲーション

_Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。_

![](images/page-types.png "Xamarin.Forms のページの種類")

## <a name="hierarchical-navigationhierarchicalmd"></a>[階層ナビゲーション](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Form の [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブのリストと大きい詳細領域で構成されていて、各タブによって詳細領域にコンテンツが読み込まれます。

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms の [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) は、ギャラリーのように、ユーザーが端から端までスワイプしてコンテンツの各ページをナビゲートできるページです。

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms の [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) は、関連する情報の 2 つのページ – 項目を表示するマスター ページと、マスター ページ上の項目に関する詳細を提示する詳細ページを管理するページです。

## <a name="modal-pagesmodalmd"></a>[モーダル ページ](modal.md)

Xamarin.Forms ではモーダル ページもサポートされています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。

## <a name="displaying-pop-upspop-upsmd"></a>[ポップアップを表示します。](pop-ups.md)

Xamarin.Forms には、ポップアップに似た 2 つのユーザー インターフェイス要素、アラートとアクション シートが用意されています。 これらのインターフェイス要素を使って、ユーザーに簡単な質問をしたり、タスクの手順をユーザーに示したりできます。
