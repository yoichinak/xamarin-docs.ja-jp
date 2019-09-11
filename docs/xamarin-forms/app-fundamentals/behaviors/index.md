---
title: Xamarin.Forms のビヘイビアー
description: ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。 ビヘイビアーはコードとして記述され、XAML のコントロールまたはコードに追加されます。
ms.prod: xamarin
ms.assetid: 42E32AD7-8E3B-48B3-B402-E75B758DA913
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: df0a767976247166205ae8a3d70fd59c521646f6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997160"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms のビヘイビアー

_ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。ビヘイビアーはコードとして記述され、XAML のコントロールまたはコードに追加されます。_

## <a name="introduction-to-behaviorsintroductionmd"></a>[ビヘイビアーの概要](introduction.md)

ビヘイビアーを使うと、通常はコードビハインドとして記述する必要があるコードを実装できます。それはこれが、コントロールに簡潔にアタッチされるような方法で、コントロールの API と直接やり取りするためです。 この記事では、ビヘイビアーの概要を説明します。

## <a name="attached-behaviorsattachedmd"></a>[アタッチされたビヘイビアー](attached.md)

アタッチされたビヘイビアーは、1 つ以上のアタッチされたプロパティを含む `static` クラスです。 この記事では、アタッチされたビヘイビアーを作成して使用する方法を示します。

## <a name="xamarinforms-behaviorscreatingmd"></a>[Xamarin.Forms のビヘイビアー](creating.md)

Xamarin.Forms のビヘイビアーは、[`Behavior`](xref:Xamarin.Forms.Behavior) または [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生させて作成します。 この記事では、Xamarin.Forms のビヘイビアーを作成して使用する方法を示します。

## <a name="reusable-behaviorsreusableindexmd"></a>[再利用可能なビヘイビアー](reusable/index.md)

ビヘイビアーは複数のアプリケーション全体で再利用可能です。 これらの記事では、便利なビヘイビアーを作成して一般的に使用される機能を実行する方法について説明します。
