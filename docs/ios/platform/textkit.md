---
title: TextKit
description: "テキストのキット API は、強力なテキスト Xamarin.iOS のレイアウトとレンダリングの機能を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 6b065c4b695edc3c88c5aed8c407c53fa6bbc578
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="text-kit"></a>テキスト キット

テキスト キットは、強力なテキストのレイアウトとレンダリングの機能を提供する新しい API です。 低レベルの中核となるテキストのフレームワーク上に構築されますが、中核となるテキストよりも使用する方が簡単です。

テキスト キットの機能を標準コントロールで使用できるようにするには、いくつかの iOS テキスト コントロールはテキスト Kit などを使用する再実装されています。

-  UITextView
-  UITextField
-  UILabel


## <a name="architecture"></a>アーキテクチャ

テキスト Kit は、レイアウトと、次のクラスを含め、表示とテキストの保存を分離する、レイヤー アーキテクチャを提供します。

-  `NSTextContainer` – 座標系とレイアウトのテキストに使用されるジオメトリを提供します。
-  `NSLayoutManager` – グリフにテキストにすることによってテキストをレイアウトします。 
-  `NSTextStorage` – テキスト データを保持するだけでなくバッチ テキストのプロパティの更新を処理します。 バッチ更新は、実際の処理のレイアウトを再計算やテキストを再描画など、変更のレイアウト マネージャーに渡されます。


これら 3 つのクラスは、テキストをレンダリングするビューに適用されます。 ように、ビューを処理する組み込みのテキスト`UITextView`、 `UITextField`、および`UILabel`設定すると、それらを使用する必要があるが、作成してそれらを任意に適用`UIView`インスタンスもします。

次の図は、このアーキテクチャを示しています。

 ![](textkit-images/textkitarch.png "この図は、テキスト キット アーキテクチャを示しています。")

## <a name="text-storage-and-attributes"></a>テキストの保存と属性

`NSTextStorage`クラスは、ビューによって表示されるテキストを保持します。 文字の変更などのテキストまたはその属性は - レイアウト マネージャーの表示には、すべての変更も通信します。 `NSTextStorage` 継承`MSMutableAttributed`文字列、バッチの間で指定するテキスト属性の変更を許可する`BeginEditing`と`EndEditing`呼び出しです。

たとえば、次のコード スニペットが前面に変更を指定し、背景色は、それぞれとし、特定の範囲を対象しします。

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

後に`EndEditing`が呼び出されると、変更は、さらに、必要なレイアウトと、ビューに表示されるテキストのレンダリングの計算を実行するレイアウト マネージャーに送信されます。

## <a name="layout-with-exclusion-path"></a>除外パスを含むレイアウト

テキスト キットも、レイアウトをサポートでき、複雑なシナリオと呼ばれる複数の列のテキストと指定したパスの周りでフローさせるテキストなど*除外パス*です。 除外パスは、指定したパスの周囲にテキストを原因と、テキスト レイアウトのジオメトリを変更するテキスト コンテナーに適用されます。

除外するパスを追加すると設定が必要です、`ExclusionPaths`レイアウト マネージャーのプロパティです。 このプロパティの設定が原因でテキストのレイアウトを無効にし、除外パスの前後のテキストをフローするレイアウト マネージャー。

### <a name="exclusion-based-on-a-cgpath"></a>CGPath に基づいて除外

次の検討`UITextView`サブクラス実装。

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

このコードは、主要なグラフィックスを使用してテキスト ビューで描画するためのサポートを追加します。 以降、`UITextView`テキストのレンダリングやレイアウトにテキスト キットを使用するクラスが組み込まれた、除外のパスを設定するなどのテキストのキットのすべての機能を使用できます。

> [!IMPORTANT]
>   注: この例サブクラス`UITextView`タッチ描画のサポートを追加します。 サブクラス化`UITextView`テキスト キットの機能を取得する必要はありません。



ユーザーが、描画、テキスト ビューで描画後`CGPath`に適用される、`UIBezierPath`インスタンスを設定して、`UIBezierPath.CGPath`プロパティ。

```csharp
bezierPath.CGPath = exclusionPath;
```

テキスト レイアウトが、パスの前後の更新は、次のコード行を更新します。

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

次のスクリーン ショットは、描画のパスの前後をフローするテキスト レイアウトがどのように変化するかを示します。

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "このスクリーン ショットは、描画のパスの前後をフローするテキストのレイアウトがどのように変化するかを示します")

注意して、レイアウト マネージャーの`AllowsNonContiguousLayout`ここではプロパティが false に設定します。 これにより、テキストが変更されるすべてのケースの再計算するレイアウトされます。 これを true に設定と、サイズの大きいドキュメントの場合は特に、完全レイアウトの更新を回避するでパフォーマンスを得ることがあります。 ただし、設定`AllowsNonContiguousLayout`に true を妨げる除外パス状況によっては、たとえば、レイアウトが更新されない場合末尾の改行が設定されているパスの前に返すことがなく実行時にテキストを入力します。


## <a name="related-links"></a>関連リンク

- [IOS 7 (サンプル) の概要](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
