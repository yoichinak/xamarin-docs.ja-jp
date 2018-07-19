---
title: 動作の概要
description: ビヘイビアーを使用して、それらをサブクラス化しなくても、ユーザー インターフェイス コントロールに機能を追加できます。 代わりに、機能が動作クラスで実装し、コントロール自体の一部であったかのように、コントロールに適用します。 この記事では、動作の概要を提供します。
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995815"
---
# <a name="introduction-to-behaviors"></a>動作の概要

_ビヘイビアーを使用して、それらをサブクラス化しなくても、ユーザー インターフェイス コントロールに機能を追加できます。代わりに、機能が動作クラスで実装し、コントロール自体の一部であったかのように、コントロールに適用します。この記事では、動作の概要を提供します。_

動作を簡潔にコントロールに適用できパッケージには、複数のアプリケーション全体で再利用方法でコントロールの API を直接操作するため、分離コードとして記述する必要が通常のコードを実装できます。 これらはなどさまざまなコントロール、機能を使用できます。

- 電子メール検証を追加、 [ `Entry`](xref:Xamarin.Forms.Entry)します。
- タップ ジェスチャ認識エンジンを使用して評価コントロールを作成します。
- アニメーションを制御します。
- コントロールへの効果を追加します。

動作より高度なシナリオも実現します。 コンテキストで*コマンド実行*動作は、コマンドにコントロールを接続するために役立つアプローチです。 さらに、コマンドに関連付けるコマンドとの対話にデザインされていないコントロールを使用できます。 たとえば、イベントの発生に対してコマンドを呼び出す、使用できます。

Xamarin.Forms には、動作の 2 つの異なるスタイルがサポートされています。

- **Xamarin.Forms の動作**– から派生するクラス、 [ `Behavior` ](xref:Xamarin.Forms.Behavior)または[ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス、`T`するコントロールの種類は、動作適用する必要があります。 Xamarin.Forms の動作の詳細については、次を参照してください。 [Xamarin.Forms の動作](~/xamarin-forms/app-fundamentals/behaviors/creating.md)と[再利用可能なビヘイビアー](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)します。
- **ビヘイビアーがアタッチされている**–`static`クラスと 1 つまたは複数の添付プロパティ。 関連付けられた動作の詳細については、次を参照してください。[アタッチされている動作](~/xamarin-forms/app-fundamentals/behaviors/attached.md)します。

このガイドは、動作の構築に推奨されるアプローチであるために、Xamarin.Forms の動作について説明します。



## <a name="related-links"></a>関連リンク

- [動作](xref:Xamarin.Forms.Behavior)
- [動作&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
