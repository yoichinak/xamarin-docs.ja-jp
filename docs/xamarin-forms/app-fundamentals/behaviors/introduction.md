---
title: ビヘイビアーの概要
description: ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。 代わりに、その機能はビヘイビアー クラスで実装され、それがコントロール自体の一部であるかのようにコントロールにアタッチされます。 この記事では、ビヘイビアーの概要を説明します。
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995815"
---
# <a name="introduction-to-behaviors"></a>ビヘイビアーの概要

_ビヘイビアーを使うと、ユーザー インターフェイス コントロールをサブクラス化することなくそれらに機能を追加できます。代わりに、その機能はビヘイビアー クラスで実装され、それがコントロール自体の一部であるかのようにコントロールにアタッチされます。この記事では、ビヘイビアーの概要を説明します。_

ビヘイビアーを使うと、通常はコードビハインドとして記述する必要があるコードを実装できます。これは、コントロールに簡潔にアタッチされ、複数のアプリケーション全体で再利用できるようにパッケージ化される方法で、コントロールの API と直接対話しているためです。 これらを使用して、次のようなさまざまな機能をコントロールに提供できます。

- [`Entry`](xref:Xamarin.Forms.Entry) にメール検証機能を追加する。
- タップ ジェスチャ認識エンジンを使用して評価コントロールを作成する。
- アニメーションを制御する。
- コントロールに効果を追加する。

ビヘイビアーによって、さらに高度なシナリオも実現できます。 *コマンド実行*のコンテキストでは、ビヘイビアーは、コントロールをコマンドに接続する場合に便利なアプローチです。 さらに、コマンドと対話するために設計されていないコントロールにコマンドを関連付けるために使用することができます。 たとえば、イベントの発生に応答してコマンドを呼び出すために使用できます。

Xamarin.Forms は、2 つの異なるスタイルのビヘイビアーをサポートしています。

- **Xamarin.Forms ビヘイビアー**: [`Behavior`](xref:Xamarin.Forms.Behavior) または [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生するクラス。`T` は、このビヘイビアーが適用されるコントロールの種類です。 Xamarin.Forms ビヘイビアーの詳細については、「[Xamarin.Forms Behaviors](~/xamarin-forms/app-fundamentals/behaviors/creating.md)」(Xamarin.Forms ビヘイビアー) と「[再利用可能なビヘイビアー](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)」を参照してください。
- **アタッチされたビヘイビアー**: 1 つ以上のプロパティがアタッチされている `static` クラス。 アタッチされたビヘイビアーの詳細については、「[Attached Behaviors](~/xamarin-forms/app-fundamentals/behaviors/attached.md)」(アタッチされたビヘイビアー) を参照してください。

このガイドでは、ビヘイビアーの構築に推奨されるアプローチなので Xamarin.Forms ビヘイビアーに重点を置いています。



## <a name="related-links"></a>関連リンク

- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
