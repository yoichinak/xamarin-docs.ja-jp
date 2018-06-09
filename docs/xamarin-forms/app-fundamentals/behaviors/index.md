---
title: Xamarin.Forms の動作
description: 動作では、それらをサブクラス化しなくてもユーザー インターフェイス コントロール機能を追加することができます。 動作はコードで記述され、XAML またはコード内のコントロールに追加します。
ms.prod: xamarin
ms.assetid: 42E32AD7-8E3B-48B3-B402-E75B758DA913
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: edb8929a4e5ffcff74714f65154cd78795bb9568
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239811"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms の動作

_動作では、それらをサブクラス化しなくてもユーザー インターフェイス コントロール機能を追加することができます。動作はコードで記述され、XAML またはコード内のコントロールに追加します。_

## <a name="introduction-to-behaviorsintroductionmd"></a>[動作の概要](introduction.md)

動作を使用すると、通常しなければならないコード ビハインドとして書き込むことを簡潔にアタッチするコントロールのような方法でコントロールの API と直接やり取りするためのコードを実装することができます。 この記事では、動作を紹介します。

## <a name="attached-behaviorsattachedmd"></a>[関連付けられた動作](attached.md)

接続されている動作は`static`1 つまたは複数の添付プロパティを持つクラス。 この記事では、作成および接続されている動作を使用する方法を示します。

## <a name="xamarinforms-behaviorscreatingmd"></a>[Xamarin.Forms の動作](creating.md)

派生することによって Xamarin.Forms 動作を作成する、 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)または[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラスです。 この記事では、作成および Xamarin.Forms ビヘイビアーを使用する方法を示します。

## <a name="reusable-behaviorsreusableindexmd"></a>[再利用可能な動作](reusable/index.md)

動作は、複数のアプリケーション間で再利用可能です。 これらの記事では、一般的に使用される機能を実行するのに使用できる動作を作成する方法について説明します。
