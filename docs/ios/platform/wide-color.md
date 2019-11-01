---
title: Xamarin の幅の広い色
description: このドキュメントでは、ワイド色と、Xamarin iOS または Xamarin. Mac アプリでの使用方法について説明します。
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: e7240a271de1f0199c2c9fc045f5c95745eb98c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031243"
---
# <a name="wide-color-in-xamarinios"></a>Xamarin の幅の広い色

_この記事では、広範な色と、Xamarin iOS または Xamarin. Mac アプリでの使用方法について説明します。_

iOS 10 と macOS Sierra は、コアグラフィックス、コアイメージ、メタル、AVFoundation などのフレームワークを含む、システム全体にわたる拡張範囲のピクセル形式と広い範囲の色空間のサポートを強化します。 グラフィックススタック全体でこの動作を提供することにより、さまざまな色で表示されるデバイスのサポートがさらに緩和さます。

## <a name="asset-catalogs"></a>資産カタログ

### <a name="supporting-wide-color-with-asset-catalogs"></a>アセットカタログを使用したワイド色のサポート

IOS 10 と macOS Sierra では、Apple は、アプリのバンドルに静的なイメージコンテンツを含めて分類するために使用される資産カタログを拡張して、ワイド色をサポートしています。

アセットカタログを使用すると、アプリに次のような利点があります。

- 静的イメージ資産に最適なデプロイオプションを提供します。
- 自動カラー補正がサポートされています。
- 自動ピクセル形式の最適化をサポートしています。
- アプリスライスとアプリ Thinning をサポートしています。これにより、関連するコンテンツのみがエンドユーザーのデバイスに配信されます。

Apple は、次のように、資産カタログに対して幅広く色のサポートを強化しました。

- 16ビット (カラーチャネルあたり) のソースコンテンツをサポートしています。
- コンテンツのカタログ化は、色域外でサポートされます。 コンテンツには、sRGB または表示 P3 色のいずれかにタグを付けることができます。

開発者には、アプリで幅広い色のコンテンツをサポートするための3つのオプションがあります。

1. **何もしない**-アプリで鮮やかな色を表示する必要がある場合 (ユーザーのエクスペリエンスを強化する場合) にのみ、この要件の外部のコンテンツをそのまま使用する必要があります。 すべてのハードウェア状況で、引き続き正しくレンダリングされます。
2. **既存のコンテンツをアップグレードして p3 を表示する**-これにより、開発者は、アセットカタログ内の既存のイメージコンテンツを p3 形式の新しいアップグレードされたファイルに置き換え、アセットエディターでタグ付けする必要があります。 ビルド時に、これらのアセットから sRGB 派生イメージが生成されます。
3. 最適化された**アセットコンテンツを提供**する-この状況では、開発者は、資産カタログ内の各イメージに対して8ビットの sRGB と16ビットの表示 P3 を提供します (アセットエディターで適切にタグ付けされています)。

### <a name="asset-catalog-deployment"></a>アセットカタログの展開

次のような状況が発生するのは、開発者が、画像のコンテンツが含まれているアセットカタログを使用してアプリストアにアプリを送信する場合です。

- アプリをエンドユーザーに展開すると、アプリスライスによって、適切なコンテンツバリアントのみがユーザーのデバイスに配信されます。
- ワイドカラーをサポートしていないデバイスでは、デバイスに出荷されることはないため、ワイドカラーコンテンツを含むペイロードコストは発生しません。
- macOS Sierra (およびそれ以降) の `NSImage` により、ハードウェアの表示に最適なコンテンツ表現が自動的に選択されます。
- 表示されたコンテンツは、デバイスのハードウェア表示特性が変更されたとき、またはそのときに自動的に更新されます。

### <a name="asset-catalog-storage"></a>アセットカタログストレージ

アセットカタログストレージには、アプリに対して次のプロパティと影響があります。

- Apple はビルド時に、高品質のイメージ変換を使用してイメージコンテンツのストレージを最適化しようとします。
- 16ビットは、カラーチャネルごとに、ワイドカラーコンテンツに使用されます。
- コンテンツに依存する画像の圧縮は、成果物コンテンツのサイズを小さくするために使用されます。 コンテンツサイズをさらに最適化するために、新しい "損失" 圧縮が追加されました。

## <a name="rendering-off-screen-images-in-app"></a>アプリでの画面以外のイメージのレンダリング

作成されるアプリの種類に基づいて、ユーザーがインターネットから収集したイメージコンテンツを含めたり、アプリの内部に直接イメージコンテンツを作成したりすることができます (ベクターの描画アプリなど)。

どちらの場合も、アプリでは、iOS と macOS の両方に追加された拡張機能を使用して、必要な画像を画面外に表示することができます。

### <a name="drawing-wide-color-in-ios"></a>IOS でのワイド色の描画

IOS 10 でワイドカラーイメージを正しく描画する方法について説明する前に、次の一般的な iOS の描画コードを見てみましょう。

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...

    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
    }
```

標準コードに問題があり、それを使用して幅の広い色の画像を描画する_前_に対処する必要があります。 IOS イメージの描画を開始するために使用する `UIGraphics.BeginImageContext (size)` 方法には、次の制限があります。

- カラーチャネルあたり8ビットを超えるイメージコンテキストを作成することはできません。
- 拡張範囲の sRGB 色空間の色を表すことはできません。
- 非 sRGB 色空間でコンテキストを作成するためのインターフェイスを提供する機能はありません。これは、下位レベルの C ルーチンがバックグラウンドで呼び出されるためです。

IOS 10 でこれらの制限を処理し、ワイドカラーイメージを描画するには、代わりに次のコードを使用します。

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

新しい `UIGraphicsImageRenderer` クラスは、拡張された範囲の sRGB 色空間を処理できる新しいイメージコンテキストを作成し、次の機能を備えています。

- 既定では、完全な色で管理されています。
- 既定では、拡張範囲の sRGB 色空間がサポートされています。
- これは、アプリが実行されている iOS デバイスの機能に基づいて、sRGB または拡張範囲の sRGB 色空間でレンダリングするかどうかを決定します。
- また、イメージコンテキスト (`CGContext`) の有効期間が完全かつ自動的に管理されるため、開発者は begin コンテキストコマンドと end コンテキストコマンドの呼び出しについて心配する必要がありません。
- `UIGraphics.GetCurrentContext()` メソッドと互換性があります。

`UIGraphicsImageRenderer` クラスの `CreateImage` メソッドを呼び出して、全体の色のイメージを作成し、に描画するイメージコンテキストを含む完了ハンドラーを渡します。 すべての描画は、次のように、この完了ハンドラー内で実行されます。

- `UIColor.FromDisplayP3` メソッドは、新しい完全に飽和状態になっている赤の色空間の色空間を作成し、四角形の前半を描画するために使用されます。 
- 四角形の2番目の半分は、比較のために通常の sRGB の完全に飽和状態になっている赤の色で描画されます。

### <a name="drawing-wide-color-in-macos"></a>MacOS での幅広色の描画

`NSImage` クラスが macOS Sierra で拡張され、ワイドカラーイメージの描画をサポートするようになりました。 (例:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);

    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();

    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();

    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>アプリで画面上のイメージをレンダリングする

画面上にワイド色の画像を表示するには、上記で説明した macOS と iOS 用の画面の広いカラーイメージを描画するのと同様の処理が行われます。

### <a name="rendering-on-screen-in-ios"></a>IOS での画面上でのレンダリング

アプリが iOS の画面上でイメージをワイドカラーでレンダリングする必要がある場合は、問題の `UIView` の `Draw` 方法を通常どおりオーバーライドします。 (例:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
        ...
        }
    }
}
```

IOS 10 は上に示した `UIGraphicsImageRenderer` クラスで動作するため、`Draw` メソッドが呼び出されたときに、アプリが実行されている iOS デバイスの機能に基づいて、sRGB または拡張範囲の sRGB 色空間でレンダリングするかどうかをインテリジェントに決定します。 さらに、iOS 9.3 以降、`UIImageView` も色で管理されています。

アプリで `UIView` または `UIViewController`でのレンダリングの実行方法を認識する必要がある場合は、`UITraitCollection` クラスの新しい `DisplayGamut` プロパティを確認できます。 この値は、次の `UIDisplayGamut` 列挙型になります。

- P3
- Srgb
- 指定されていません。

イメージの描画に使用する色空間を制御する必要がある場合は、`CALayer` の新しい `ContentsFormat` プロパティを使用して、目的の色空間を指定できます。 この値は、次の `CAContentsFormat` 列挙型にすることができます。

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS での画面上でのレンダリング

アプリが macOS の画面上でイメージをワイドカラーでレンダリングする必要がある場合は、問題の `NSView` の `DrawRect` 方法を通常どおりオーバーライドします。 (例:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

ここでも、`DrawRect` メソッドが呼び出されたときに、アプリが実行されている Mac ハードウェアの機能に基づいて、sRGB または拡張範囲の sRGB 色空間でレンダリングするかどうかを、インテリジェントに決定します。

イメージの描画に使用する色空間を制御する必要がある場合は、`NSWindow` クラスの新しい `DepthLimit` プロパティを使用して、目的の色空間を指定できます。 この値は、次の `NSWindowDepth` 列挙型にすることができます。

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- Twenty4 Bitrgb
