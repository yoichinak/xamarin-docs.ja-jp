---
title: Xamarin.iOS の色
description: このドキュメントでは、色と Xamarin.iOS または Xamarin.Mac アプリでの使用方法について説明します。
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 523aa1a97e1c536b5afbdb66c52f605382fa338d
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268810"
---
# <a name="wide-color-in-xamarinios"></a>Xamarin.iOS の色

_この記事では、色と Xamarin.iOS または Xamarin.Mac アプリでの使用方法について説明します。_

iOS 10 デバイスと macOS Sierra 拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属、および AVFoundation などのフレームワークを含めて、システム全体で全体の色域スペースのサポートが強化されます。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

## <a name="asset-catalogs"></a>資産カタログ

### <a name="supporting-wide-color-with-asset-catalogs"></a>資産カタログの色をサポートしています。

Apple は iOS 10 と macOS Sierra で色をサポートするために資産カタログを含めるし、アプリのバンドルで静的なイメージのコンテンツを分類するために使用を拡大しました。

使用して資産カタログがアプリに次の利点を提供します。

- 静的イメージ資産の最適な展開オプションを提供します。
- 色の自動修正をサポートしています。
- 自動のピクセル形式の最適化をサポートしています。
- アプリのスライスとは関連する get のコンテンツのみが、エンドユーザーのデバイスに配信されるようにするアプリが簡略化をサポートしています。

Apple が行われて、次の拡張機能資産カタログに色のサポート。

- 16 ビット (色のチャネル) ごとのソース コンテンツをサポートしています。
- 表示の色域によってカタログのコンテンツをサポートしています。 コンテンツは、sRGB または Display P3 色域のいずれかのタグ付けすることができます。

開発者には、それぞれのアプリでワイド カラー コンテンツをサポートするための 3 つのオプションがあります。

1. **何もしない**-としてこの要件以外でもコンテンツを残しておく必要がある色コンテンツは、アプリが (場所、ユーザーのエクスペリエンスを向上するは) 鮮明な色を表示する必要がある状況でのみ使用する必要があります、ためには。 すべてのハードウェアの状況で正しくレンダリングされる続けます。
2. **Display P3 に既存のコンテンツをアップグレード**-P3 形式で、アップグレードされた新しいファイルを使用して、資産カタログで既存のイメージのコンテンツを交換し、資産エディターでタグを開発者が必要です。 ビルド時に、これらの資産から sRGB 派生イメージが生成されます。
3. **資産のコンテンツの最適化を提供**-8 ビット、sRGB と (正しく資産エディターでタグ付け) 資産カタログ内の各イメージの 16 ビット Display P3 ビジョンの両方でこのような状況では、開発者は、します。

### <a name="asset-catalog-deployment"></a>資産カタログの展開

次のことを開発者は、ワイド カラー イメージのコンテンツを含む資産カタログを使用してアプリ ストアにアプリを送信するとき。

- エンドユーザーにアプリが展開されると、アプリのスライスことによりユーザーのデバイスに適切なコンテンツのバリアントのみが配信されるようにします。
- 色をサポートしていないデバイスではありませんの色のコンテンツを含むペイロードのコストは、デバイスを出荷ことはありません。
- `NSImage` macos Sierra (以降) がハードウェアの表示に最適なコンテンツの表現を自動的に選択します。
- 表示されているコンテンツは、デバイスのハードウェア特性の変更を表示する場合、または自動的に更新されます。

### <a name="asset-catalog-storage"></a>資産カタログ ストレージ

資産カタログ ストレージには、次のプロパティと、アプリに影響があります。

- ビルド時に、Apple は、高品質なイメージの変換を使用して、イメージ コンテンツの記憶域を最適化しようとします。
- ワイド カラー コンテンツのカラー チャネルあたり 16 ビットが使用されます。
- コンテンツの依存するイメージの圧縮を使用して、成果物のコンテンツ サイズを削減します。 コンテンツのサイズをさらに最適化するために、新しい「非可逆」の圧縮が追加されました。

## <a name="rendering-off-screen-images-in-app"></a>アプリの画面イメージのレンダリング

作成されているアプリの種類に基づいて、インターネットから収集したか (アプリをたとえば描画ベクトル) など、アプリ内から直接イメージ コンテンツを作成、イメージ コンテンツを追加するユーザーを許可する可能性がありますに。

どちらのこのような場合、アプリをレンダリングできます必要な画像で iOS と macOS の両方に追加された拡張機能を使用して色をワイド画面。

### <a name="drawing-wide-color-in-ios"></a>IOS での色の描画

IOS 10 でワイドの色の画像を正しく描画する方法を説明する前に共通の iOS 描画コードは次を参照してください。

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

対処する必要がある標準のコードに問題がある_する前に_ワイド カラー イメージを描画するために使用できます。 `UIGraphics.BeginImageContext (size)` IOS イメージの描画を開始するために使用するメソッドは、次の制限。

- カラー チャネルあたり 8 ビット以上のイメージのコンテキストを作成できません。
- これは、Extended Range sRGB 色空間内の色を表すことはできません。
- バック グラウンドで呼び出される低レベル C ルーチンのため、非 sRGB 色空間でコンテキストを作成するインターフェイスを提供する機能はありません。

これらの制限を処理し、iOS 10 でワイドの色の画像を描画するには、代わりに次のコードを使用します。

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

新しい`UIGraphicsImageRenderer`クラスは、Extended Range sRGB 色空間の処理に対応している新しいイメージ コンテキストを作成し、次の機能があります。

- 既定で管理されている色では完全にします。
- 既定では、Extended Range sRGB 色空間をサポートします。
- また、sRGB、Extended Range sRGB 色空間ので、アプリが実行されている iOS デバイスの機能に基づくでレンダリングするかどうかは適切に決定されます。
- 完全かつ自動的に管理イメージ コンテキスト (`CGContext`) 呼び出しについて心配する必要はありません、開発者のための有効期間が開始およびコンテキスト コマンドを終了します。
- 互換性があります、`UIGraphics.GetCurrentContext()`メソッド。

`CreateImage`のメソッド、`UIGraphicsImageRenderer`クラスがワイド カラー イメージを作成すると呼ばれ、に描画するイメージのコンテキストに完了ハンドラーに渡されます。 次のようにこの完了ハンドラー内で実行すべての描画は。

- `UIColor.FromDisplayP3`メソッドが作る Display P3 色空間に新しい色の彩度赤を作成し、四角形の前半の描画に使用されます。 
- 2 番目の完全に通常の sRGB で描画する四角形の半分には、比較の赤のカラーが飽和状態。

### <a name="drawing-wide-color-in-macos"></a>MacOS での色の描画

`NSImage` Macos Sierra ワイド カラー イメージの描画をサポートするクラスが拡張されています。 例:

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

## <a name="rendering-on-screen-images-in-app"></a>アプリでイメージを画面に表示

ワイド カラー イメージを画面に表示される表示するために、プロセスは、macOS、および iOS 上で示したワイド画面のカラー イメージの描画のような動作します。

### <a name="rendering-on-screen-in-ios"></a>IOS の画面に表示

アプリで iOS の画面に表示される色のイメージをレンダリングする場合、オーバーライド、`Draw`のメソッド、`UIView`通常どおり、問題の。 例えば:

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

IOS 10 と同様、 `UIGraphicsImageRenderer` sRGB、Extended Range sRGB 色空間のときに、アプリが実行されている iOS デバイスの機能に基づくでレンダリングするかどうかに適切に決定されます、上記のようにクラス、`Draw`メソッドが呼び出されます。 さらに、`UIImageView`されて管理される iOS の 9.3 以降の色。

アプリがでレンダリングが行われる方法を把握する必要があるかどうか、`UIView`または`UIViewController`、新しいチェック`DisplayGamut`のプロパティ、`UITraitCollection`クラス。 この値になります、`UIDisplayGamut`次の列挙型。

- P3
- Srgb
- 指定されていません。

アプリは、イメージを描画するために使用される色空間コントロールする場合、使用できる新しい`ContentsFormat`のプロパティ、`CALayer`目的の色空間を指定します。 この値は、`CAContentsFormat`次の列挙型。

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS の画面に表示

アプリは、macOS の画面に表示される色でイメージをレンダリングする必要がある、オーバーライド、`DrawRect`のメソッド、`NSView`通常どおり問題。 例:

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

ここでも、それが適切に決定 sRGB、Extended Range sRGB 色空間の場合で、アプリが実行されている Mac のハードウェアの機能に基づくでレンダリングするかどうか、`DrawRect`メソッドが呼び出されます。

アプリは、イメージを描画するために使用される色空間コントロールする場合、使用できる新しい`DepthLimit`のプロパティ、`NSWindow`クラスが目的の色空間を指定します。 この値は、`NSWindowDepth`次の列挙型。

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb
