---
title: データ テンプレート
description: DataTemplate はサポートされているコントロールにデータの外観を指定するために使用し、通常表示されるデータにバインドします。
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 14de42acd1bde00df146a9fe5d772366735ed295
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="data-templates"></a>データ テンプレート

_DataTemplate はサポートされているコントロールにデータの外観を指定するために使用し、通常表示されるデータにバインドします。_

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms データ テンプレートでは、サポートされているコントロールのデータの表示を定義する機能を提供します。 この記事では、必要とされる理由を調べて、データ テンプレートに紹介します。

## <a name="creating-a-datatemplatecreatingmd"></a>[DataTemplate を作成します。](creating.md)

データ テンプレートで作成できます、インライン、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、またはカスタムの型または適切な Xamarin.Forms セルの種類。 データ テンプレートを他の場所を再利用する必要がない場合は、インライン テンプレートを使用してください。 代わりに、データ テンプレートは、カスタムの型、または、コントロール レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで再利用できます。

## <a name="creating-a-datatemplateselectorselectormd"></a>[DataTemplateSelector を作成します。](selector.md)

A [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)選択に使用できる、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)データ バインドされたプロパティの値に基づいて実行時にします。 これにより、複数`DataTemplate`インスタンスを特定のオブジェクトの外観をカスタマイズする、オブジェクトの同じ型に適用できます。 この記事の内容を作成および使用する方法を示しています、`DataTemplateSelector`です。


## <a name="related-links"></a>関連リンク

- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
