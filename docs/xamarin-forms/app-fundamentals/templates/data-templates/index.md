---
title: Xamarin.Forms のデータ テンプレート
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e0961fad18ccd961a5b84b2a5535bca70781dd8d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136124"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms のデータ テンプレート

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_DataTemplate は、サポートされているコントロール上のデータの外観を指定するために使われ、通常は表示されるデータにバインドします。_

## <a name="introduction"></a>[はじめに](introduction.md)

Xamarin.Forms のデータ テンプレートを使うと、サポートされているコントロール上のデータの表現方法を定義することができます。 この記事では、データ テンプレートの概要について説明し、これが必要である理由について調べます。

## <a name="creating-a-datatemplate"></a>[DataTemplate の作成](creating.md)

データ テンプレートは、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 内でインラインで作成したり、またはカスタム型や適切な Xamarin.Forms のセルの種類から作成したりできます。 データ テンプレートを他の場所で再利用する必要がない場合は、インライン テンプレートを使用する必要があります。 または、データ テンプレートをカスタム型として定義することで、あるいは制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで、それを再利用できます。

## <a name="creating-a-datatemplateselector"></a>[DataTemplateSelector の作成](selector.md)

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) を使って、データ バインドされたプロパティの値に基づいて実行時に [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を選択できます。 これにより、複数の `DataTemplate` インスタンスを同じ種類のオブジェクトに適用し、特定のオブジェクトの外観をカスタマイズできます。 この記事では、`DataTemplateSelector` を作成して使用する方法を示します。

## <a name="related-links"></a>関連リンク

- [データ テンプレート (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
