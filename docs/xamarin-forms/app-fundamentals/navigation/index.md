---
title: Xamarin.Forms のナビゲーション
description: このガイドでは、Xamarin.Forms アプリでナビゲーションを実行する方法について説明します。 Xamarin.Forms は、さまざまな使用されているページの種類に応じて、別のページ ナビゲーション エクスペリエンスを提供します。
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994729"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms のナビゲーション

_Xamarin.Forms は、さまざまな使用されているページの種類に応じて、別のページ ナビゲーション エクスペリエンスを提供します。_

![](images/page-types.png "Xamarin.Forms のページの種類")

## <a name="hierarchical-navigationhierarchicalmd"></a>[階層ナビゲーション](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)一連のタブと、詳細領域が大きく、詳細領域にコンテンツを読み込む各タブで構成されます。

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)ギャラリーなどのコンテンツのページ間を移動するユーザーが左右にスワイプできるページです。

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)は、項目を表示するマスター ページとマスター ページの項目に関する詳細を表示する詳細ページ 2 つのページの関連情報を管理するページです。

## <a name="modal-pagesmodalmd"></a>[モーダル ページ](modal.md)

Xamarin.Forms には、モーダル ページのサポートも提供します。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。

## <a name="displaying-pop-upspop-upsmd"></a>[ポップアップを表示します。](pop-ups.md)

Xamarin.Forms は、2 つのポップアップ サインアップのようなユーザー インターフェイス要素を提供します。 アラートとアクション シート。 簡単な質問をユーザーに確認して、タスクを使用してユーザーをガイドは、これらのインターフェイス要素を使用できます。
