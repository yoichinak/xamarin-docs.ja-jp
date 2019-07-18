---
title: Xamarin.iOS で TextKit
description: このドキュメントでは、Xamarin.iOS で TextKit を使用する方法について説明します。 TextKit は、強力なテキスト レイアウトとレンダリング機能を提供します。
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: f08e37d17cc32e45232d54cc4a51bb48d7ec94b1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61184516"
---
# <a name="textkit-in-xamarinios"></a>Xamarin.iOS で TextKit

TextKit とは、強力なテキスト レイアウトとレンダリングの機能を提供する新しい API です。 低レベルの中核となるテキスト フレームワーク上に構築されますが、使用する主要なテキストよりもはるかに簡単です。

で TextKit の機能を標準コントロールに使用できるようにいくつかの iOS テキスト コントロールは TextKit、を使用する再実装されているを含みます。

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>アーキテクチャ

TextKit には、レイアウトと表示、次のクラスを含むからテキストの記憶域を分離する階層型アーキテクチャが用意されています。

-  `NSTextContainer` – 座標系とテキストのレイアウトに使用されるジオメトリを提供します。
-  `NSLayoutManager` – グリフにテキストにすることでは、テキストをレイアウトします。 
-  `NSTextStorage` – テキストのデータを保持して、バッチ テキストのプロパティの更新を処理します。 バッチ更新プログラムは、レイアウトを再計算し、テキストを再描画など、変更の実際の処理のためのレイアウト マネージャーに渡されます。


これら 3 つのクラスは、テキストをレンダリングするビューに適用されます。 組み込みのテキストなど、ビューの処理`UITextView`、 `UITextField`、および`UILabel`設定すると、それらが既にあるが、作成し、いずれかに適用できます`UIView`インスタンスもします。

次の図は、このアーキテクチャを示しています。

 ![](textkit-images/textkitarch.png "この図は、TextKit アーキテクチャを示しています。")

## <a name="text-storage-and-attributes"></a>テキストの保存と属性

`NSTextStorage`クラスはビューによって表示されるテキストを保持します。 文字への変更などのテキストまたはその属性の表示のレイアウト マネージャーに変更を加えたも通信します。 `NSTextStorage` 継承`MSMutableAttributed`までのバッチに指定するテキスト属性の変更を許可する文字列`BeginEditing`と`EndEditing`呼び出し。

など、次のコード スニペットがフォア グラウンドへの変更を指定し、背景色は、それぞれ、および特定の範囲を対象とします。

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

後`EndEditing`が呼び出されると、変更は、さらに、必要なレイアウトとビューに表示されるテキストのレンダリングの計算を実行するレイアウト マネージャーに送信されます。

## <a name="layout-with-exclusion-path"></a>除外パスでのレイアウト

TextKit も、レイアウトをサポートでき、複雑なシナリオなど、複数列のテキストと指定したパスの周囲でフローさせる文字列と呼ばれます*除外パス*します。 除外パスは、指定したパスの周りをフローするテキストを原因と、テキストのレイアウトのジオメトリを変更するテキスト コンテナーに適用されます。

除外するパスを追加するには、設定が必要です、`ExclusionPaths`レイアウト マネージャーのプロパティ。 このプロパティを設定すると、テキストのレイアウトを無効にして除外パスの前後のテキストをフローするレイアウト マネージャー。

### <a name="exclusion-based-on-a-cgpath"></a>CGPath に基づいて除外

次を考慮`UITextView`サブクラスを実装します。

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

このコードは、コア グラフィックスを使用して、テキスト ビュー上に描画するためのサポートを追加します。 以降、 `UITextView` TextKit をテキスト レンダリングやレイアウトに使用するクラスが組み込まれた、TextKit、除外パスの設定などのすべての機能を使用できます。

> [!IMPORTANT]
> この例のサブクラス`UITextView`タッチ描画のサポートを追加します。 サブクラス化`UITextView`TextKit の機能を取得する必要はありません。



ユーザーが、描画、テキスト ビュー上に描画後`CGPath`に適用される、`UIBezierPath`インスタンスを設定して、`UIBezierPath.CGPath`プロパティ。

```csharp
bezierPath.CGPath = exclusionPath;
```

テキストのレイアウト パスの前後の更新は、次のコード行を更新します。

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

次のスクリーン ショットでは、描画パスの周りをフローするテキストのレイアウトがどのように変化するかを示します。

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "このスクリーン ショットは、描画パスの周りをフローするテキストのレイアウトがどのように変化するかを示します")

注意して、レイアウト マネージャーの`AllowsNonContiguousLayout`ここでプロパティが false に設定します。 これにより、常にテキストが変更の再計算するレイアウト。 これを true に設定すると、特にサイズの大きいドキュメントの場合、完全なレイアウトの更新を回避することでパフォーマンスが向上する可能性があります。 ただし、設定`AllowsNonContiguousLayout`に true を妨げる除外パス状況によっては、たとえば、レイアウトの更新から末尾の改行が設定されているパスの前に戻ることがなく実行時にテキストを入力した場合。


## <a name="related-links"></a>関連リンク

- [IOS 7 (サンプル) の概要](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
