---
title: Xamarin.Forms のナビゲーション
description: このガイドでは、Xamarin.Forms アプリでナビゲーションを実行する方法について説明します。 Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d976048b15c1fc545e1fbdc6c911e3cb4542d4f2
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939096"
---
# <a name="no-locxamarinforms-navigation"></a>Xamarin.Forms のナビゲーション

_Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。_

![Xamarin.Formsページの種類](images/page-types.png)

または、Xamarin.Forms シェル アプリケーションでは、設定されたナビゲーション階層を適用しない URI ベースのナビゲーション エクスペリエンスが使われます。 詳細については、「[Xamarin.Forms シェルのナビゲーション](~/xamarin-forms/app-fundamentals/shell/navigation.md)」を参照してください。

## <a name="hierarchical-navigation"></a>[階層ナビゲーション](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。

## <a name="tabbedpage"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms の [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。

## <a name="carouselpage"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms の [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) は、ギャラリーのように、ユーザーが端から端までスワイプしてコンテンツの各ページをナビゲートできるページです。

## <a name="flyoutpage"></a>[FlyoutPage](flyoutpage.md)

Xamarin.Forms の [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) は、2 つの関連する情報ページ、つまり項目を表示するポップアップ ページと、ポップアップ ページ上の項目に関する詳細を表示する詳細ページを管理するページです。

## <a name="modal-pages"></a>[モーダル ページ](modal.md)

Xamarin.Forms ではモーダル ページもサポートされています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。
