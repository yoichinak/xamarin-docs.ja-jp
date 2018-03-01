---
title: "高速レンダラー"
description: "この記事では、高速レンダラーは、結果として得られるネイティブ コントロールの階層をフラット化によってが増加および Android 上の Xamarin.Forms コントロールのレンダリングのコストを削減を紹介します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 92a11ebe983840270d3679fd11f5faa0b8222cfe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="fast-renderers"></a>高速レンダラー

_この記事では、高速レンダラーは、結果として得られるネイティブ コントロールの階層をフラット化によってが増加および Android 上の Xamarin.Forms コントロールのレンダリングのコストを削減を紹介します。_

従来、Android で元のコントロールのレンダラーのほとんどは、2 つのビューから構成されます。

- ネイティブ制御など、`Button`または`TextView`です。
- コンテナー`ViewGroup`レイアウト作業、ジェスチャ処理、およびその他のタスクの一部を処理します。

ただし、この方法では、各論理的なコントロールは、結果をより多くのメモリと画面に表示するために処理を必要とするより複雑なビジュアル ツリー内の 2 つのビューが作成されるに、パフォーマンスに影響があります。

高速レンダラー コストの削減が増加レンダリング Xamarin.Forms コントロールの 1 つのビューにします。 そのため、2 つのビューを作成し、表示ツリーに追加するの代わりに 1 つだけが作成されます。 これにより、つまりさらに複雑な小さいツリーの表示、少数のオブジェクトを作成し、メモリ使用量 (につながることも少なくなりますガベージ コレクションの一時停止) 以下のパフォーマンスが向上します。

高速レンダラーは、Xamarin.Forms 2.4 で Android での次のコントロールで利用可能。

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

機能的には、これらの高速なレンダラーでは、元のレンダラーにまったく同じです。 ただし、現在の実験用され次のコード行を追加することでのみ使用できます、`MainActivity`呼び出す前にクラス`Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> **注**: 高速レンダラーにのみ適用アプリ compat Android バックエンドのため pre アプリ compat アクティビティでこの設定は無視されます。

レイアウトの複雑さに応じて、各アプリケーションのパフォーマンスの向上が異なります。 たとえば、x2 のパフォーマンスの向上は、考えられるスクロールされるときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)数千行ごとのセルが高速のレンダラーを使用するコントロールの行われる場所、データの行を含むその結果は目に見えるスムーズにスクロールします。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
