---
title: Xamarin でのコアグラフィックスとコアアニメーションの使用
description: この記事では、コアグラフィックスとコアアニメーションを使用するアプリケーションを作成する手順について説明します。 この例では、ユーザータッチに応答して画面上に描画する方法について説明します。また、画像をアニメーション化してパスに沿って移動する方法も示します。
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: b35e88cfdc0bce321068951f1617885c90331c83
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032448"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Xamarin でのコアグラフィックスとコアアニメーションの使用

このチュートリアルでは、タッチ入力に応じてコアグラフィックスを使用してパスを描画します。 次に、パスに沿ってアニメーション化するイメージを含む `CALayer` を追加します。

次のスクリーンショットは、完成したアプリケーションを示しています。

![](graphics-animation-walkthrough-images/00-final-app.png "The completed application")

このガイドに付属する*GraphicsDemo*サンプルのダウンロードを開始する前に、こちらを参照してください。 [ここで](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation)ダウンロードでき、 **GraphicsWalkthrough**ディレクトリ内に配置されています。 **GraphicsDemo_starter**という名前のプロジェクトを開き、ダブルクリックして、`DemoView` クラスを開きます。

## <a name="drawing-a-path"></a>パスの描画

1. `DemoView`、`CGPath` 変数をクラスに追加し、コンストラクターでインスタンス化します。 また、パスの構築元のタッチポイントをキャプチャするために使用する2つの `CGPoint` 変数 `initialPoint` と `latestPoint`も宣言します。

    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;

        public DemoView ()
        {
            BackgroundColor = UIColor.White;

            path = new CGPath ();
        }
    }
    ```

2. 次の using ディレクティブを追加します。

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. 次に、`TouchesBegan` と `TouchesMoved,` をオーバーライドし、次の実装を追加して、最初のタッチポイントと後続の各タッチポイントをそれぞれキャプチャします。

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){

        base.TouchesBegan (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt){

        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` は、次の実行ループパスで `Draw` を呼び出すために、移動するたびに呼び出されます。

4. `Draw` メソッドのパスに行を追加し、で描画するには赤い破線を使用します。 次に示すコードを使用して[`Draw`を実装](~/ios/platform/graphics-animation-ios/core-graphics.md)します。

    ```csharp
    public override void Draw (CGRect rect){

        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){

                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();

                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }

                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });

                //add geometry to graphics context and draw it
                g.AddPath (path);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

ここでアプリケーションを実行すると、次のスクリーンショットに示すように、画面に触れることができます。

![](graphics-animation-walkthrough-images/01-path.png "Drawing on the screen")

## <a name="animating-along-a-path"></a>パスに沿ってアニメーション化する

ユーザーがパスを描画できるようにコードを実装したので、次に、描画されたパスに沿ってレイヤーをアニメーション化するコードを追加します。

1. まず、 [`CALayer`](~/ios/platform/graphics-animation-ios/core-animation.md)変数をクラスに追加し、コンストラクターで作成します。

    ```csharp
    public class DemoView : UIView
        {
            …

            CALayer layer;

            public DemoView (){
                …

                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. 次に、ユーザーが画面から指を離したときに、ビューのレイヤーのサブレイヤーとしてレイヤーを追加します。 次に、パスを使用してキーフレームアニメーションを作成し、レイヤーの `Position`をアニメーション化します。

    これを実現するには、`TouchesEnded` をオーバーライドし、次のコードを追加する必要があります。

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. ここでアプリケーションを実行し、描画した後、イメージを含むレイヤーが追加され、描画されたパスに沿って移動します。

![](graphics-animation-walkthrough-images/00-final-app.png "A layer with an image is added and travels along the drawn path")

## <a name="summary"></a>まとめ

この記事では、グラフィックスとアニメーションの概念を連携させる例について説明します。 まず、コアグラフィックスを使用して、ユーザータッチに応じて `UIView` にパスを描画する方法を説明しました。 次に、コアアニメーションを使用して、そのパスに沿ってイメージを移動する方法を説明しました。

## <a name="related-links"></a>関連リンク

- [コア アニメーション](~/ios/platform/graphics-animation-ios/core-animation.md)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [コアアニメーションのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
