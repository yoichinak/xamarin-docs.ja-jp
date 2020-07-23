---
title: レイアウト圧縮
description: レイアウトの圧縮では、ページレンダリングのパフォーマンスを向上させるために、指定したレイアウトがビジュアルツリーから削除されます。 この記事では、レイアウト圧縮を有効にする方法と、それによってもたらされる利点について説明します。
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5fa9c7592ecd2cb314ce12d7e303677447a5e104
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931171"
---
# <a name="layout-compression"></a>レイアウト圧縮

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)

_レイアウトの圧縮では、ページレンダリングのパフォーマンスを向上させるために、指定したレイアウトがビジュアルツリーから削除されます。この記事では、レイアウト圧縮を有効にする方法と、それによってもたらされる利点について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms2つの一連の再帰メソッド呼び出しを使用してレイアウトを実行します。

- レイアウトは、ビジュアルツリーの一番上からページを使用して開始され、ビジュアルツリーのすべての分岐を通じて、ページ上のすべてのビジュアル要素をカバーします。 他の要素の親である要素は、それ自体を基準にして子のサイズ設定と配置を行います。
- 無効化とは、ページ上の要素の変更によって新しいレイアウトサイクルがトリガーされるプロセスです。 適切なサイズまたは位置がなくなった場合、要素は無効と見なされます。 子を持つビジュアルツリー内のすべての要素は、その子の1つがサイズを変更するたびに警告されます。 そのため、ビジュアルツリー内の要素のサイズを変更すると、ツリーに波及する変更が発生する可能性があります。

がレイアウトを実行する方法の詳細につい Xamarin.Forms ては、「[カスタムレイアウトを作成](~/xamarin-forms/user-interface/layouts/custom.md)する」を参照してください。

レイアウトプロセスの結果は、ネイティブコントロールの階層構造になります。 ただし、この階層には、プラットフォームレンダラー用の追加のコンテナーレンダラーとラッパーが含まれており、さらにビュー階層の入れ子に拡大ます。 入れ子のレベルが深いほど、 Xamarin.Forms ページを表示するために実行する必要がある作業量が多くなります。 複雑なレイアウトでは、複数レベルの入れ子を持つ深さと広い範囲のビュー階層を使用できます。

たとえば、Facebook にログインするためのサンプルアプリケーションの次のボタンを考えてみます。

![Facebook ボタン](layout-compression-images/facebook-button.png)

このボタンは、次の XAML ビュー階層でカスタムコントロールとして指定されます。

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

結果として生成される入れ子になったビュー階層は、 [Xamarin Inspector](~/tools/inspector/index.md)で調べることができます。 Android では、入れ子になったビュー階層に17個のビューが含まれています。

![Facebook ボタンの階層を表示する](layout-compression-images/no-compression.png)

レイアウト圧縮は、 Xamarin.Forms iOS プラットフォームと Android プラットフォームのアプリケーションで使用でき、指定されたレイアウトをビジュアルツリーから削除することによってビューの入れ子をフラット化することを目的としています。これにより、ページレンダリングのパフォーマンスが向上します。 提供されるパフォーマンスの利点は、ページの複雑さ、使用されているオペレーティングシステムのバージョン、およびアプリケーションが実行されているデバイスによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。

> [!NOTE]
> この記事では、Android でレイアウト圧縮を適用した結果に焦点を当てていますが、iOS にも同様に適用できます。

## <a name="layout-compression"></a>レイアウト圧縮

XAML では、 `CompressedLayout.IsHeadless` レイアウトクラスで添付プロパティをに設定することにより、レイアウト圧縮を有効にすることができ `true` ます。

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

または、メソッドの最初の引数として layout インスタンスを指定することで、C# で有効にすることもでき `CompressedLayout.SetIsHeadless` ます。

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> レイアウトの圧縮では、ビジュアルツリーからレイアウトが削除されるため、視覚的な外観を持つレイアウトやタッチ入力を取得するレイアウトには適していません。 したがって、プロパティを設定するレイアウト [`VisualElement`](xref:Xamarin.Forms.VisualElement) ( [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) ジェスチャを受け入れる、、、、など) [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) は、 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) レイアウトの圧縮の対象になりません。 ただし、視覚的な外観プロパティを設定したり、ジェスチャを受け入れたりするレイアウトでレイアウト圧縮を有効にしても、ビルドエラーやランタイムエラーは発生しません。 代わりに、レイアウトの圧縮が適用され、視覚的な外観のプロパティとジェスチャ認識が自動的に失敗します。

Facebook ボタンの場合、次の3つのレイアウトクラスでレイアウト圧縮を有効にすることができます。

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

Android では、次のように、入れ子になったビュー階層で14個のビューが生成されます。

![レイアウト圧縮を使用した Facebook ボタンの階層の表示](layout-compression-images/layout-compression.png)

17ビューの入れ子になった元のビュー階層と比較して、これは17% のビュー数の減少を表します。 この削減はあまり意味がないかもしれませんが、ページ全体に対するビューの縮小がより重要になる場合があります。

### <a name="fast-renderers"></a>高速レンダラー

[高速レンダラー] Xamarin.Forms 結果のネイティブビュー階層をフラット化することで、Android 上のコントロールのインフレとレンダリングのコストを削減します。 これにより、作成されるオブジェクトが少なくなるため、パフォーマンスがさらに向上します。これにより、より複雑なビジュアルツリーが生成され、メモリ使用量が少なくなります。 高速レンダラーの詳細については、「[高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)」を参照してください。

サンプルアプリケーションの [Facebook] ボタンの場合、レイアウト圧縮と高速レンダラーを組み合わせることで、入れ子になったビュー階層で8つのビューが生成されます。

![レイアウトの圧縮と高速レンダラーを使用した Facebook ボタンの階層の表示](layout-compression-images/layout-compression-with-fast-renderers.png)

17ビューの入れ子になった元のビュー階層と比較して、これは52% の減少を表します。

サンプルアプリケーションには、実際のアプリケーションから抽出されたページが含まれています。 レイアウトの圧縮と高速レンダラーを使用しない場合、ページは Android で130ビューの入れ子になったビュー階層を生成します。 適切なレイアウトクラスで高速レンダラーとレイアウト圧縮を有効にすると、入れ子になったビュー階層が70ビューに減少し、46% が減少します。

## <a name="summary"></a>要約

レイアウトの圧縮では、ページレンダリングのパフォーマンスを向上させるために、指定したレイアウトがビジュアルツリーから削除されます。 この操作によって得られるパフォーマンスのメリットは、ページの複雑さ、使用しているオペレーティング システムのバージョン、このアプリケーションが実行されているデバイスによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。

## <a name="related-links"></a>関連リンク

- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)
