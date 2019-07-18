---
title: Xamarin.Forms のデータ テンプレート
description: DataTemplate は、サポートされているコントロール上のデータの外観を指定するために使われ、通常は表示されるデータにバインドします。
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: ab48d6d3a463a287af8de7d3926287b799ae43a6
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926258"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms のデータ テンプレート

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/Templates/DataTemplates/)

_DataTemplate は、サポートされているコントロール上のデータの外観を指定するために使われ、通常は表示されるデータにバインドします。_

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms のデータ テンプレートを使うと、サポートされているコントロール上のデータの表現方法を定義することができます。 この記事では、データ テンプレートの概要について説明し、これが必要である理由について調べます。

## <a name="creating-a-datatemplatecreatingmd"></a>[DataTemplate の作成](creating.md)

データ テンプレートは、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 内でインラインで作成したり、またはカスタム型や適切な Xamarin.Forms のセルの種類から作成したりできます。 データ テンプレートを他の場所で再利用する必要がない場合は、インライン テンプレートを使用する必要があります。 または、データ テンプレートをカスタム型として定義することで、あるいは制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで、それを再利用できます。

## <a name="creating-a-datatemplateselectorselectormd"></a>[DataTemplateSelector の作成](selector.md)

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) を使って、データ バインドされたプロパティの値に基づいて実行時に [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を選択できます。 これにより、複数の `DataTemplate` インスタンスを同じ種類のオブジェクトに適用し、特定のオブジェクトの外観をカスタマイズできます。 この記事では、`DataTemplateSelector` を作成して使用する方法を示します。


## <a name="related-links"></a>関連リンク

- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Templates/DataTemplates/)
