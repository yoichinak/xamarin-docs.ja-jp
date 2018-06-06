---
title: 動作の概要
description: 動作では、それらをサブクラス化しなくてもユーザー インターフェイス コントロール機能を追加できます。 代わりに、機能が動作クラスで実装され、コントロール自体の一部が場合と、コントロールにアタッチされています。 この記事では、動作を紹介します。
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: dc6d8396c2908d251290e4540dbb3cec3344542f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732789"
---
# <a name="introduction-to-behaviors"></a>動作の概要

_動作では、それらをサブクラス化しなくてもユーザー インターフェイス コントロール機能を追加できます。代わりに、機能が動作クラスで実装され、コントロール自体の一部が場合と、コントロールにアタッチされています。この記事では、動作を紹介します。_

動作を使用するを簡潔にコントロールに追加して複数のアプリケーション間で再利用するためにパッケージ化方法でコントロールの API と直接やり取りするため、分離コードとして記述する必要が通常のコードを実装することができます。 など、フル機能の範囲をコントロールを提供する、使用できます。

- 電子メール検証を追加する、 [ `Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)です。
- 評価をタップ ジェスチャ レコグナイザーを使用してコントロールを作成しています。
- アニメーションを制御します。
- コントロールへの効果を追加します。

動作には、高度なシナリオも有効にします。 コンテキストで*コマンド実行*動作は、コマンドにコントロールを接続するために有効な方法です。 さらに、コマンドと対話するように設計されていないコントロールにコマンドを関連付けに使用できます。 たとえばを発生させるイベントに応答コマンドの呼び出しに使用できます。

Xamarin.Forms では、動作の 2 つの異なるスタイルがサポートされています。

- **Xamarin.Forms 動作**– から派生したクラス、 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)または[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラス、場所`T`するコントロールの種類は、動作適用する必要があります。 Xamarin.Forms の動作に関する詳細については、次を参照してください。 [Xamarin.Forms 動作](~/xamarin-forms/app-fundamentals/behaviors/creating.md)と[再利用可能な動作](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)です。
- **動作をアタッチ**– `static` 1 つまたは複数の添付プロパティを持つクラス。 関連付けられた動作の詳細については、次を参照してください。[添付動作](~/xamarin-forms/app-fundamentals/behaviors/attached.md)です。

このガイドは、動作の構築に推奨できるアプローチであるために、Xamarin.Forms の動作について説明します。



## <a name="related-links"></a>関連リンク

- [動作](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [動作&lt;T&gt;](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
