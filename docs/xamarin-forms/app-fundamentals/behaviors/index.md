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
ms.openlocfilehash: 83952982bd163725fb931c860cac3e267726315c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135812"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms のビヘイビアー

_ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。ビヘイビアーはコードとして記述され、XAML のコントロールまたはコードに追加されます。_

## <a name="introduction-to-behaviors"></a>[ビヘイビアーの概要](introduction.md)

ビヘイビアーを使うと、通常はコードビハインドとして記述する必要があるコードを実装できます。それはこれが、コントロールに簡潔にアタッチされるような方法で、コントロールの API と直接やり取りするためです。 この記事では、ビヘイビアーの概要を説明します。

## <a name="attached-behaviors"></a>[アタッチされたビヘイビアー](attached.md)

アタッチされたビヘイビアーは、1 つ以上のプロパティがアタッチされた `static` クラスです。 この記事では、アタッチされたビヘイビアーを作成して使用する方法を示します。

## <a name="xamarinforms-behaviorscreatingmd"></a>[Xamarin.Forms のビヘイビアー](creating.md)

Xamarin.Forms のビヘイビアーは、[`Behavior`](xref:Xamarin.Forms.Behavior) または [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生させて作成されます。 この記事では、Xamarin.Forms のビヘイビアーを作成して使用する方法を示します。

## <a name="reusable-behaviors"></a>[再利用可能なビヘイビアー](reusable/index.md)

ビヘイビアーは複数のアプリケーション全体で再利用可能です。 これらの記事では、便利なビヘイビアーを作成して一般的に使用される機能を実行する方法について説明します。
