---
title: Xamarin.Forms のビヘイビアー
description: ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。 ビヘイビアーはコードとして記述され、XAML のコントロールまたはコードに追加されます。
ms.prod: xamarin
ms.assetid: 42E32AD7-8E3B-48B3-B402-E75B758DA913
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d917d7d6421cfae7fc877c81023a835573fa99b1
ms.sourcegitcommit: a003b036f6fb83818e2ecc9c72a641e3aeb373bd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2020
ms.locfileid: "88964624"
---
# <a name="no-locxamarinforms-behaviors"></a>Xamarin.Forms のビヘイビアー

_ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。ビヘイビアーはコードとして記述され、XAML のコントロールまたはコードに追加されます。_

## <a name="introduction-to-behaviors"></a>[ビヘイビアーの概要](introduction.md)

ビヘイビアーを使うと、通常はコードビハインドとして記述する必要があるコードを実装できます。それはこれが、コントロールに簡潔にアタッチされるような方法で、コントロールの API と直接やり取りするためです。 この記事では、ビヘイビアーの概要を説明します。

## <a name="attached-behaviors"></a>[アタッチされたビヘイビアー](attached.md)

アタッチされたビヘイビアーは、1 つ以上のプロパティがアタッチされた `static` クラスです。 この記事では、アタッチされたビヘイビアーを作成して使用する方法を示します。

## <a name="no-locxamarinforms-behaviors"></a>[Xamarin.Forms のビヘイビアー](creating.md)

Xamarin.Forms のビヘイビアーは、[`Behavior`](xref:Xamarin.Forms.Behavior) または [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生させて作成されます。 この記事では、Xamarin.Forms のビヘイビアーを作成して使用する方法を示します。

## <a name="reusable-effectbehavior"></a>[再利用可能な EffectBehavior](effect-behavior.md)

ビヘイビアーは、コントロールにエフェクトを追加するために役立つ方法です。エフェクトを処理する定型コードを分離コード ファイルから削除します。 この記事では、Xamarin.Forms のビヘイビアーを作成および使用して、コントロールにエフェクトを追加する方法を示します。
