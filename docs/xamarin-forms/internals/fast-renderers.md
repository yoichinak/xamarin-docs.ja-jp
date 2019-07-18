---
title: Xamarin.Forms の高速レンダラー
description: この記事では、高速レンダラーは、結果として得られるネイティブ コントロール階層をフラット化して、増加し、android、Xamarin.Forms コントロールのレンダリング コストを削減について説明します。
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 861d9e3f898dcd61015d9aca27ae66c3fe72d1a9
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970721"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms の高速レンダラー

これまでは、Android で元のコントロールのレンダラーのほとんどは、2 つのビューから構成されます。

- ネイティブ コントロールなど、`Button`または`TextView`します。
- コンテナー`ViewGroup`レイアウト作業、ジェスチャの処理、およびその他のタスクの一部を処理します。

ただし、この方法には大容量メモリ、および詳細画面にレンダリングするために処理を必要とするより複雑なビジュアル ツリー内の結果、論理各コントロールの 2 つのビューを作成することで、パフォーマンスに影響します。

高速レンダラーは、1 つのビューに増加を抑制し、Xamarin.Forms コントロールのレンダリング コストを削減します。 そのため、2 つのビューを作成し、ビュー、ツリーに追加するではなく 1 つだけが作成されます。 これにより、あまり複雑なツリーの表示をさらには、少数のオブジェクトを作成し、メモリ使用量 (その結果が少ないガベージ コレクションの一時停止、また) 以下のパフォーマンスが向上します。

高速レンダラーは、Android で次のコントロールを Xamarin.Forms で利用できます。

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

機能的には、これらの高速レンダラーは、従来のレンダラーと変わりません。 Xamarin.Forms 4.0 以降では、対象とするすべてのアプリケーションから`FormsAppCompatActivity`既定ではこれらの高速レンダラーを使用します。 など、すべての新しいコントロールのレンダラー [ `ImageButton` ](xref:Xamarin.Forms.ImageButton)と[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)、高速レンダラー アプローチを使用します。

高速レンダラーを使用する場合、パフォーマンスの向上は、レイアウトの複雑さに応じて、アプリケーションごとに異なります。 たとえば、x2 のパフォーマンスの向上する場合は、スクロール、 [ `ListView` ](xref:Xamarin.Forms.ListView)何千もの行の行ごとのセルが高速レンダラーを使用するコントロールの行われる場所、データを格納しているその結果は非表示スムーズにスクロールします。

> [!NOTE]
> カスタム レンダラーは、従来のレンダラーを使用したのと同じアプローチを使用して高速レンダラーを作成できます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。

## <a name="backwards-compatibility"></a>下位互換性

高速レンダラーは、次の方法で上書きできます。

1. 次のコードの行を追加することで従来のレンダラーを有効にすると、`MainActivity`クラスを呼び出す前に`Forms.Init`:

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. 従来のレンダラーを対象とするカスタム レンダラーを使用します。 任意の既存のカスタム レンダラーは引き続き従来のレンダラーで機能します。
1. 別の指定`View.Visual`など`Material`、さまざまなレンダラーを使用します。 マテリアルのビジュアルの詳細については、次を参照してください。 [Xamarin.Forms マテリアル Visual](~/xamarin-forms/user-interface/visual/material-visual.md)します。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
