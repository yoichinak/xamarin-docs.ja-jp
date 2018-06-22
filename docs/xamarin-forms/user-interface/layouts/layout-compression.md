---
title: レイアウト圧縮
description: レイアウトの圧縮は、ページのレンダリング パフォーマンスを向上させるためにでは、ビジュアル ツリーから指定したレイアウトを削除します。 この記事では、レイアウトの圧縮とがもたらす利点を有効にする方法について説明します。
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: 9c698d539ab671ee2a033ae5943a46e0cc870f76
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30791127"
---
# <a name="layout-compression"></a>レイアウト圧縮

_レイアウトの圧縮は、ページのレンダリング パフォーマンスを向上させるためにでは、ビジュアル ツリーから指定したレイアウトを削除します。この記事では、レイアウトの圧縮とがもたらす利点を有効にする方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms では、再帰的なメソッド呼び出しの 2 つの系列を使用してレイアウトを実行します。

- レイアウトがビジュアル ツリーをページの上部にあるが開始され、ページ上のすべての visual 要素が含まれるようにビジュアル ツリーのすべての分岐進みます。 その他の要素の親である要素は、相対自体の子を配置してサイズ変更を行います。
- 無効化は、ページ上の要素の変更が新しいレイアウト サイクルをトリガーするプロセスです。 要素は不要になったがインストールされている正しいサイズや位置が、無効と見なされます。 子を持つビジュアル ツリー内のすべての要素がその子の 1 つのサイズが変更されるたびに警告を表示します。 そのため、ビジュアル ツリー内の要素のサイズの変更には、ツリーに波及変更可能性があります。

Xamarin.Forms でレイアウトを実行する方法の詳細については、次を参照してください。[カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)です。

レイアウトの処理の結果は、ネイティブ コントロールの階層です。 ただし、この階層が含まれるコンテナーを追加レンダラーとラッパー プラットフォーム レンダラー、さらに入れ子階層の表示を拡大します。 入れ子のレベルが深くなるにつれて、Xamarin.Forms のページを表示するために実行にある作業量は大きくします。 複雑なレイアウトは、階層の表示の詳細と、広範囲にわたって、複数レベルの入れ子にできます。

たとえば、Facebook にログインするためのサンプル アプリケーションから次のボタンがあるとします。

![](layout-compression-images/facebook-button.png "Facebook のボタン")

このボタンは、次の XAML ビュー階層にカスタム コントロールとして指定されます。

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

結果の入れ子になったビュー階層に調査できる[Xamarin インスペクター](~/tools/inspector/index.md)です。 Android では、入れ子になったビュー階層には、17 ビューが含まれています。

![](layout-compression-images/no-compression.png "Facebook のボタンの階層の表示")

レイアウトの圧縮は、iOS および Android プラットフォームで Xamarin.Forms アプリケーションで使用できますは、ページのレンダリング パフォーマンスを向上させることができます、ビジュアル ツリーから指定したレイアウトを削除することで入れ子ビューを平坦化する目的としています。 提供されるパフォーマンスの利点は、ページ、使用されているオペレーティング システムのバージョンおよびアプリケーションが実行されているデバイスの複雑さによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。

> [!NOTE]
> この記事では、Android でレイアウトの圧縮を適用した結果に焦点を当てていますは、iOS にも適用されます。

## <a name="layout-compression"></a>レイアウト圧縮

XAML では、レイアウト圧縮できますを有効に設定して、`CompressedLayout.IsHeadless`添付プロパティ`true`レイアウト クラスで。

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

または、有効にする (C#) 1 番目の引数として、レイアウトのインスタンスを指定することによって、`CompressedLayout.SetIsHeadless`メソッド。

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> レイアウト圧縮は、ビジュアル ツリーからレイアウトを削除したので、レイアウト、外観があるか、タッチ入力を取得するのに適したはありません。 このため、レイアウトを設定[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)プロパティ (など[ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)、 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/)、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)、 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)と[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) またはジェスチャをそのまま使用する、レイアウトの対象ではありません圧縮します。 ただし、視覚的な外観のプロパティを設定するか、ジェスチャを受け入れるレイアウトでレイアウトの圧縮を有効にするは、ビルドまたは実行時エラーは発生しません。 代わりに、レイアウトの圧縮が適用され、視覚的な外観のプロパティ、および、ジェスチャ認識は失敗します。

Facebook ボタンには、次の 3 つのレイアウトのクラスにレイアウトの圧縮を有効にすることができます。

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

Android では、14 ビューの入れ子になったビュー階層でこの結果します。

![](layout-compression-images/layout-compression.png "Facebook ボタン レイアウトの圧縮で階層の表示")

17 ビューの元の階層の入れ子になった表示と比較して、17% のビューの数を削減することを表します。 このような削減を意味のないに見えますが、ページ全体に対してビュー リダクションがさらに重要なあります。

### <a name="fast-renderers"></a>高速レンダラー

高速レンダラー コストの削減が増加レンダリング Xamarin.Forms コントロール Android での結果として得られるネイティブ ビュー階層をフラット化しています。 さらに、これは、さらに結果が複雑のビジュアル ツリー内、およびメモリ使用量を少なくする少数のオブジェクトを作成することでパフォーマンスを向上します。 高速のレンダラーについての詳細については、次を参照してください。[高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)です。

サンプル アプリケーションで [Facebook] ボタンのレイアウトの圧縮と高速のレンダラーを組み合わせることにより、8 ビューの入れ子になったビュー階層が生成されます。

![](layout-compression-images/layout-compression-with-fast-renderers.png "Facebook ボタン レイアウトの圧縮と高速なレンダラーで階層の表示")

17 ビューの元の階層の入れ子になった表示と比較して、52% を小さくすることを表します。

サンプル アプリケーションには、実際のアプリケーションから抽出されたページが含まれています。 レイアウトの圧縮と高速なレンダラーでは、なし、ページには、Android での 130 のビューの入れ子になったビュー階層が生成されます。 高速レンダラーと適切なレイアウトのクラスのレイアウトの圧縮を有効にすると、入れ子になったビュー階層が 46% の削減、70 のビューが減少します。

## <a name="summary"></a>まとめ

レイアウトの圧縮は、ページのレンダリング パフォーマンスを向上させるためにでは、ビジュアル ツリーから指定したレイアウトを削除します。 この操作によって得られるパフォーマンスのメリットは、ページの複雑さ、使用しているオペレーティング システムのバージョン、このアプリケーションが実行されているデバイスによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。


## <a name="related-links"></a>関連リンク

- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
