---
title: Xamarin の TextKit
description: このドキュメントでは、Xamarin の TextKit を使用する方法について説明します。 TextKit は、強力なテキストレイアウトとレンダリング機能を提供します。
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 6c1dee464de1f7ba708b1f7d60affc1616e71ee9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031417"
---
# <a name="textkit-in-xamarinios"></a>Xamarin の TextKit

TextKit は、強力なテキストレイアウトとレンダリング機能を提供する新しい API です。 これは、低レベルのコアテキストフレームワークの上に構築されていますが、コアテキストよりもはるかに使いやすくなっています。

TextKit の機能を標準コントロールで使用できるようにするために、次のような TextKit を使用するようにいくつかの iOS テキストコントロールが再実装されています。

- UITextView
- UITextField
- UILabel

## <a name="architecture"></a>アーキテクチャ

TextKit には、次のクラスを含む、レイアウトと表示からテキストストレージを分離するレイヤーアーキテクチャが用意されています。

- `NSTextContainer` –テキストのレイアウトに使用される座標系と geometry を提供します。
- `NSLayoutManager` –テキストをグリフに変えることによってテキストをレイアウトします。
- `NSTextStorage` –テキストデータを保持すると共に、バッチテキストプロパティの更新を処理します。 バッチ更新は、レイアウトの再計算やテキストの再描画など、実際に変更を処理するためにレイアウトマネージャーに渡されます。

これら3つのクラスは、テキストを表示するビューに適用されます。 `UITextView`、`UITextField`、`UILabel` などの組み込みのテキスト処理ビューには、既に設定されていますが、`UIView` インスタンスにも作成して適用することができます。

次の図は、このアーキテクチャを示しています。

 ![](textkit-images/textkitarch.png "This figure illustrates the TextKit architecture")

## <a name="text-storage-and-attributes"></a>テキストの格納と属性

`NSTextStorage` クラスは、ビューによって表示されるテキストを保持します。 また、文字やその属性に対する変更などのテキストへの変更が、表示のためにレイアウトマネージャーに伝達されます。 `NSTextStorage` は `MSMutableAttributed` 文字列から継承されるので、`BeginEditing` と `EndEditing` の呼び出しの間に、テキスト属性を変更してバッチで指定することができます。

たとえば、次のコードスニペットでは、前景色と背景色の変更をそれぞれ特定の範囲で指定しています。

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

`EndEditing` が呼び出されると、変更がレイアウトマネージャーに送信されます。レイアウトマネージャーは、ビューに表示されるテキストに必要なレイアウトとレンダリングの計算を実行します。

## <a name="layout-with-exclusion-path"></a>除外パスを使用したレイアウト

TextKit はレイアウトもサポートしており、複数列テキストなどの複雑なシナリオに対応し、*除外パス*と呼ばれる指定されたパスにテキストを流し込むことができます。 除外パスはテキストコンテナーに適用されます。これによりテキストレイアウトのジオメトリが変更され、テキストが指定したパスの周りに流れます。

除外パスを追加するには、レイアウトマネージャーで `ExclusionPaths` プロパティを設定する必要があります。 このプロパティを設定すると、レイアウトマネージャーによってテキストレイアウトが無効になり、除外パスの周囲にテキストが流し込まれます。

### <a name="exclusion-based-on-a-cgpath"></a>CGPath に基づく除外

次の `UITextView` サブクラス実装について考えてみます。

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

このコードでは、コアグラフィックスを使用してテキストビューに描画するためのサポートが追加されています。 `UITextView` クラスは、テキストのレンダリングとレイアウトに TextKit を使用するように構築されているため、除外パスの設定など、TextKit のすべての機能を使用できます。

> [!IMPORTANT]
> この例では、`UITextView` サブクラスを使用してタッチ描画サポートを追加します。 `UITextView` サブクラス化は、TextKit の機能を取得するためには必要ありません。

ユーザーがテキストビューに描画した後、描画された `CGPath` は、`UIBezierPath.CGPath` プロパティを設定することによって `UIBezierPath` インスタンスに適用されます。

```csharp
bezierPath.CGPath = exclusionPath;
```

次のコード行を更新すると、パスの周りにテキストレイアウトが更新されます。

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

次のスクリーンショットは、描画されたパスの周囲にテキストのレイアウトがどのように変化するかを示しています。

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")-->
![](textkit-images/exclusionpath2.png "This screenshot illustrates how the text layout changes to flow around the drawn path")

この場合、レイアウトマネージャーの `AllowsNonContiguousLayout` プロパティが false に設定されていることに注意してください。 これにより、テキストが変更されるすべてのケースに対して、レイアウトが再計算されます。 この値を true に設定すると、大規模なドキュメントの場合は特に、レイアウト全体の更新を回避することでパフォーマンスが向上する可能性があります。 ただし、`AllowsNonContiguousLayout` を true に設定すると、場合によっては、除外パスによってレイアウトが更新されないようにすることができます。たとえば、パスを設定する前に末尾のキャリッジリターンを行わずに実行時にテキストを入力した場合などです。

## <a name="related-links"></a>関連リンク

- [IOS 7 の概要 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
