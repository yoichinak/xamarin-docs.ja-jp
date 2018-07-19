---
title: Xamarin.Forms データ テンプレート
description: DataTemplate は、サポートされているコントロールのデータの外観を指定するために使用し、通常表示されるデータにバインドします。
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994972"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms データ テンプレート

_DataTemplate は、サポートされているコントロールのデータの外観を指定するために使用し、通常表示されるデータにバインドします。_

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms データ テンプレートは、サポートされているコントロールのデータのプレゼンテーションを定義する機能を提供します。 この記事では、必要とされる理由を調べて、データ テンプレートの概要を提供します。

## <a name="creating-a-datatemplatecreatingmd"></a>[DataTemplate を作成します。](creating.md)

データ テンプレートでインラインで作成できますが、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、またはカスタムの型または適切な Xamarin.Forms セルの種類。 データ テンプレートを他の場所を再利用する必要がない場合、インライン テンプレートを使用する必要があります。 または、データ テンプレートは、カスタムの型、または制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで再利用できます。

## <a name="creating-a-datatemplateselectorselectormd"></a>[DataTemplateSelector を作成します。](selector.md)

A [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)選択に使用できる、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)データ バインド プロパティの値に基づいて実行時にします。 これにより、複数`DataTemplate`インスタンス オブジェクト、特定のオブジェクトの外観をカスタマイズするのと同じ型に適用します。 この記事で作成および使用する方法を示します、`DataTemplateSelector`します。


## <a name="related-links"></a>関連リンク

- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
