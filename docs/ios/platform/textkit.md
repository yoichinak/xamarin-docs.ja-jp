---
title: Xamarin の TextKit
description: このドキュメントでは、Xamarin の TextKit を使用する方法について説明します。 TextKit は、強力なテキストレイアウトとレンダリング機能を提供します。
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: c0ee125090a2bb817f09a18c34033a1f8a788569
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227284"
---
# <a name="textkit-in-xamarinios"></a>Xamarin の TextKit

TextKit は、強力なテキストレイアウトとレンダリング機能を提供する新しい API です。 これは、低レベルのコアテキストフレームワークの上に構築されていますが、コアテキストよりもはるかに使いやすくなっています。

TextKit の機能を標準コントロールで使用できるようにするために、次のような TextKit を使用するようにいくつかの iOS テキストコントロールが再実装されています。

- UITextView
- UITextField
- UILabel

## <a name="architecture"></a>アーキテクチャ

TextKit には、次のクラスを含む、レイアウトと表示からテキストストレージを分離するレイヤーアーキテクチャが用意されています。

- `NSTextContainer`–テキストのレイアウトに使用される座標系と geometry を提供します。
- `NSLayoutManager`–テキストをグリフに変えることによってテキストをレイアウトします。
- `NSTextStorage`–テキストデータを保持し、バッチテキストプロパティの更新を処理します。 バッチ更新は、レイアウトの再計算やテキストの再描画など、実際に変更を処理するためにレイアウトマネージャーに渡されます。


これら3つのクラスは、テキストを表示するビューに適用されます。 `UITextView` `UIView` 、`UILabel` 、などの組み込みのテキスト処理ビューでは、既に設定されていますが、インスタンスを作成して任意のインスタンスに適用することもできます。 `UITextField`

次の図は、このアーキテクチャを示しています。

 ![](textkit-images/textkitarch.png "次の図は、TextKit のアーキテクチャを示しています。")

## <a name="text-storage-and-attributes"></a>テキストの格納と属性

クラス`NSTextStorage`は、ビューによって表示されるテキストを保持します。 また、文字やその属性に対する変更などのテキストへの変更が、表示のためにレイアウトマネージャーに伝達されます。 `NSTextStorage`文字列から`MSMutableAttributed`継承されます。これにより、との呼び出しの`BeginEditing`間`EndEditing`で、テキスト属性を変更してバッチで指定することができます。

たとえば、次のコードスニペットでは、前景色と背景色の変更をそれぞれ特定の範囲で指定しています。

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

`EndEditing`が呼び出されると、変更がレイアウトマネージャーに送信され、ビューに表示されるテキストに必要なレイアウトとレンダリングの計算が実行されます。

## <a name="layout-with-exclusion-path"></a>除外パスを使用したレイアウト

TextKit はレイアウトもサポートしており、複数列テキストなどの複雑なシナリオに対応し、*除外パス*と呼ばれる指定されたパスにテキストを流し込むことができます。 除外パスはテキストコンテナーに適用されます。これによりテキストレイアウトのジオメトリが変更され、テキストが指定したパスの周りに流れます。

除外パスを追加するには`ExclusionPaths` 、レイアウトマネージャーでプロパティを設定する必要があります。 このプロパティを設定すると、レイアウトマネージャーによってテキストレイアウトが無効になり、除外パスの周囲にテキストが流し込まれます。

### <a name="exclusion-based-on-a-cgpath"></a>CGPath に基づく除外

次`UITextView`のサブクラス実装について考えてみます。

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

このコードでは、コアグラフィックスを使用してテキストビューに描画するためのサポートが追加されています。 `UITextView`クラスはテキストのレンダリングとレイアウトに textkit を使用するように構築されているため、除外パスの設定など、textkit のすべての機能を使用できます。

> [!IMPORTANT]
> この例で`UITextView`は、サブクラスを使用してタッチ描画サポートを追加しています。 サブ`UITextView`クラス化は、textkit の機能を取得するためには必要ありません。



ユーザーがテキストビューに描画した後、次`CGPath`のように`UIBezierPath.CGPath`プロパティ`UIBezierPath`を設定することによって、描画がインスタンスに適用されます。

```csharp
bezierPath.CGPath = exclusionPath;
```

次のコード行を更新すると、パスの周りにテキストレイアウトが更新されます。

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

次のスクリーンショットは、描画されたパスの周囲にテキストのレイアウトがどのように変化するかを示しています。

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")-->
![](textkit-images/exclusionpath2.png "このスクリーンショットは、描画されたパスの周囲にテキストレイアウトがどのように変化するかを示しています。")

この場合、レイアウトマネージャーの`AllowsNonContiguousLayout`プロパティが false に設定されていることに注意してください。 これにより、テキストが変更されるすべてのケースに対して、レイアウトが再計算されます。 この値を true に設定すると、大規模なドキュメントの場合は特に、レイアウト全体の更新を回避することでパフォーマンスが向上する可能性があります。 ただし、を`AllowsNonContiguousLayout` true に設定すると、場合によっては、除外パスによってレイアウトが更新されないようにすることができます。たとえば、パスを設定する前に後続のキャリッジリターンを使用せずに実行時にテキストを入力する場合などです。


## <a name="related-links"></a>関連リンク

- [IOS 7 の概要 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
