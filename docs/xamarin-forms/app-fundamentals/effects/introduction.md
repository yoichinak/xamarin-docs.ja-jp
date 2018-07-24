---
title: 効果の概要
description: カスタマイズするには、各プラットフォームのネイティブ コントロールを許可する効果は通常小さなスタイルの変更の使用とします。 この記事で効果を紹介するには、効果とカスタム レンダラーでは、境界の概要を説明します。 および PlatformEffect クラスについて説明します。
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997382"
---
# <a name="introduction-to-effects"></a>効果の概要

_カスタマイズするには、各プラットフォームのネイティブ コントロールを許可する効果は通常小さなスタイルの変更の使用とします。この記事で効果を紹介するには、効果とカスタム レンダラーでは、境界の概要を説明します。 および PlatformEffect クラスについて説明します。_

Xamarin.Forms[ページ、レイアウト、およびコントロール](~/xamarin-forms/user-interface/controls/index.md)クロス プラットフォーム モバイルのユーザー インターフェイスを記述する一般的な API を表示します。 各ページ、レイアウト、およびコントロールは、各プラットフォームで異なってレンダリング、 `Renderer` (Xamarin.Forms の表現に対応する)、ネイティブ コントロールを作成するクラスは、画面上で並べられし、共有で指定された動作を追加します。コードです。

開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 ただし、単純なコントロールのカスタマイズを実行するカスタム レンダラー クラスの実装は、ヘビーウェイトな応答では多くの場合です。 効果より簡単にカスタマイズするには、各プラットフォームのネイティブ コントロールをできるように、このプロセスを簡略化します。

効果をサブクラス化してプラットフォーム固有のプロジェクトで作成された、`PlatformEffect`コントロール、し、効果は、Xamarin.Forms .NET Standard ライブラリまたは共有ライブラリ プロジェクトに適切なコントロールに添付することによって使用されます。

## <a name="why-use-an-effect-over-a-custom-renderer"></a>カスタム レンダラー経由で効果を使用する理由

効果は、コントロールのカスタマイズを簡略化、再利用、およびパラメーター化を再利用をさらに向上することができます。

効果を達成できるものは、カスタム レンダラーを実現できます。 ただし、カスタム レンダラーは、柔軟性およびカスタマイズより効果を提供します。 次のガイドラインには、カスタム レンダラー経由で効果を選択するための状況が一覧表示します。

- 目的の結果を達成できますプラットフォームに固有のコントロールのプロパティを変更するときに、効果を使うことをお勧めします。
- プラットフォーム固有のコントロールのメソッドをオーバーライドする必要がある場合に、カスタム レンダラーが必要です。
- Xamarin.Forms のコントロールを実装するプラットフォーム固有のコントロールを置換する必要があるときに、カスタム レンダラーが必要です。

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect クラスをサブクラス化

次の表に、名前空間を`PlatformEffect`クラスは、各プラットフォームとそのプロパティの型。

|プラットフォーム|名前空間|コンテナー|コントロール|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|表示|
|ユニバーサル Windows プラットフォーム (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

各プラットフォーム固有`PlatformEffect`クラスは、次のプロパティを公開します。

- `Container` – レイアウトを実装するために使用されているプラットフォームに固有のコントロールを参照します。
- `Control` – Xamarin.Forms コントロールを実装するために使用されているプラットフォームに固有のコントロールを参照します。
- `Element` – レンダリングされている Xamarin.Forms コントロールを参照します。

効果には、コンテナー、コントロール、または任意の要素に接続されているために、関連付けられている要素に関する型情報はありません。 したがって、効果がサポートしていない要素にアタッチされて潔く機能を減らすか、例外をスローする必要があります。 ただし、 `Container`、 `Control`、および`Element`プロパティは、実装する型にキャストすることができます。 詳細については「」を参照の型の[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

各プラットフォーム固有`PlatformEffect`クラスは、次のメソッドは、効果の実装をオーバーライドする必要がありますを公開します。

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) – 効果が Xamarin.Forms コントロールに接続されているときに呼び出されます。 各プラットフォームに固有のエフェクト クラスでこのメソッドのオーバーライドされたバージョンでは、例外処理の場合は効果は、指定の Xamarin.Forms コントロールに適用されることはできませんと、コントロールのカスタマイズを実行する場所です。
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – 効果は、Xamarin.Forms コントロールからデタッチされるときに呼び出されます。 各プラットフォームに固有のエフェクト クラスでこのメソッドのオーバーライドされたバージョンでは、イベント ハンドラーを登録解除などの効果のクリーンアップを実行する場所です。

さらに、`PlatformEffect`公開、 [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs))メソッドをオーバーライドすることもできます。 要素のプロパティが変更されたときに、このメソッドが呼び出されます。 各プラットフォームに固有のエフェクト クラスでこのメソッドのオーバーライドされたバージョンでは、Xamarin.Forms コントロールにバインド可能なプロパティの変更に応答する場所です。 この上書きは、何度も呼び出すことが、変更されているプロパティのチェックを行った常にする必要があります。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
