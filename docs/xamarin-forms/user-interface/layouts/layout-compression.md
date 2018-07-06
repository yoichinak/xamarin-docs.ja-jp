---
title: レイアウト圧縮
description: レイアウト圧縮は、ページのレンダリング パフォーマンスを向上させるための試みとして、ビジュアル ツリーから指定したレイアウトを削除します。この記事では、レイアウト圧縮を有効にする方法とそれがもたらす利点について説明します。
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
---
# <a name="layout-compression"></a>レイアウト圧縮

_レイアウト圧縮は、ページのレンダリング パフォーマンスを向上させるための試みとして、ビジュアル ツリーから指定したレイアウトを削除します。この記事では、レイアウト圧縮を有効にする方法とそれがもたらす利点について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms では、再帰的なメソッド呼び出しの 2 つの系列を使用してレイアウトを実行します。

- Layout は、ページのビジュアル ツリーの上部から始まり、ページ上のすべての視覚要素を網羅するためにビジュアル ツリーのすべての分岐を進みます。他の要素の親である要素は、サイズ計算と自身からの相対的な子の配置の責任があります。
- Invalidation は、ページ上の要素の変更によって新しいレイアウトサイクルを引き起こさせるための処理です。要素は、サイズと位置が正しくなくなった時に、無効と見なされます。子を持つビジュアルツリー内の全ての要素は、子のひとつがサイズを変更するときは常に警告されます。そのため、ビジュアルツリー内の要素のひとつのサイズ変更は、ツリーに遡って波及する変更になる可能性があります。

Xamarin.Forms でレイアウトを実行する方法の詳細については、[カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md) を参照してください。

レイアウトの処理の結果は、ネイティブ コントロールの階層です。ただし、この階層はプラットフォームレンダラーのために、追加のコンテナレンダラーとラッパーを含み、さらにそのビューの入れ子の階層を含めます。入れ子のレベルが深くなるにつれて、Xamarin.Forms がページを表示するために実行する作業量が増加します。複雑なレイアウトのために、複数のレベルの入れ子を使って、ビュー階層を深く広くすることができます。

たとえば、Facebook にログインするためのサンプル アプリケーションに次のようなボタンがあるとします。

![](layout-compression-images/facebook-button.png "Facebook のボタン")

このボタンは、次の XAML のビュー階層を使ったカスタム コントロールとして指定されます。

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

結果の入れ子のビュー階層は、[Xamarin インスペクター](~/tools/inspector/index.md) で
調査することができます。Android では、入れ子のビュー階層には、17 個のビューが含まれています。

![](layout-compression-images/no-compression.png "Facebook のボタンのビュー階層")

レイアウト圧縮は、iOS および Android プラットフォームの Xamarin.Forms アプリケーションで使用でき、ビジュアル ツリーから指定したレイアウトを削除することでビュー階層を平坦化することを目的とし、ページレンダリングのパフォーマンスを向上させることができます。この機能が提供するパフォーマンスの効果は、ページの複雑さ、使用しているオペレーティング システムのバージョン、およびアプリケーションを実行しているデバイスによって異なります。しかし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。

> [!NOTE]
> この記事では、Android でレイアウト圧縮を適用した結果に焦点を当てていますが、iOS も同様に適用できます。

## <a name="layout-compression"></a>レイアウト圧縮

XAML では、レイアウト圧縮はレイアウトクラス上で `CompressedLayout.IsHeadless` 添付プロパティに `true` を設定することで有効にできます。

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

または、C# で第一引数としてレイアウトのインスタンスを `CompressedLayout.SetIsHeadless` メソッドに設定することで有効にできます。

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> レイアウト圧縮は、ビジュアル ツリーからレイアウトを削除するため、視覚的な外観のあるレイアウトや、タッチ入力があるレイアウトには適していません。このため、[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) プロパティ ([ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)、 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/)、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)、 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)と[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)など) が設定されているレイアウト、またはジェスチャを受け付けるレイアウトはレイアウト圧縮の対象ではありません。ただし、視覚的な外観のプロパティを設定したレイアウトやジェスチャを受け付けるレイアウトで、レイアウト圧縮を有効にした場合でも、ビルド時または実行時にエラーにはなりません。その代わり、レイアウト圧縮が適用され、視覚的外観のプロパティやジェスチャー認識は失敗します。

Facebook ボタンは、次の 3 つのレイアウトのクラスにレイアウト圧縮を有効にすることができます。

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

Android では、この結果として 14 個のビューの入れ子のビュー階層になります。

![](layout-compression-images/layout-compression.png "レイアウト圧縮を使った Facebook ボタンのビュー階層表示")

17 個のビューがある元の入れ子のビュー階層と比較すると、ビューの数を 17% 削減したことが分かります。このような削減は意味のないように思えるかもしれませんが、ページ全体にわたるビューの削減は、より重要になる可能性があります。

### <a name="fast-renderers"></a>高速レンダラー

高速レンダラーは、Android 上で、結果として得られるネイティブ ビュー階層をフラット化することによって、Xamarin.Forms コントロールのレンダリングとインフレーションコストを削減します。これは、より少数のオブジェクトを生成することでパフォーマンスをさらに向上させ、より複雑でないビジュアルツリーとより少ないメモリー使用をもたらします。高速レンダラーについての詳細は、[高速レンダラー](~/xamarin-forms/internals/fast-renderers.md) を参照してください。

サンプル アプリケーションの [Facebook] ボタンに、レイアウト圧縮と高速のレンダラーを組み合わせることにより、8 個の入れ子のビュー階層が生成されます。

![](layout-compression-images/layout-compression-with-fast-renderers.png "レイアウトの圧縮と高速なレンダラーを使った Facebook ボタン のビュー階層表示")

元の 17 個の入れ子のビュー階層と比較して、52% 削減したことが分かります。

サンプル アプリケーションには、実際のアプリケーションから抽出されたページが含まれています。 レイアウト圧縮と高速レンダラーが無ければ、そのページは、Android で 130 個のビューの入れ子ビュー階層が生成されます。高速レンダラーと適切なレイアウトクラスでのレイアウト圧縮を有効にすると、入れ子のビュー階層が 70 個のビューに減少し、46% の削減になります。

## <a name="summary"></a>まとめ

レイアウト圧縮は、ページのレンダリング パフォーマンスを向上させるために、ビジュアル ツリーから指定したレイアウトを削除します。 この機能が提供するパフォーマンスの効果は、ページの複雑さ、使用しているオペレーティング システムのバージョン、およびアプリケーションを実行しているデバイスによって異なります。しかし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。


## <a name="related-links"></a>関連リンク

- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
