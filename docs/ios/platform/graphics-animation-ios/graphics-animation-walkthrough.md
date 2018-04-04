---
title: チュートリアル - CoreGraphics および CoreAnimation を使用して
description: この記事ステップ バイ ステップ コア グラフィックスおよびアニメーションのコアを使用するアプリケーションを作成する方法を示します。 ユーザーのタッチへの応答で画面に描画する方法と、パスに沿って移動する画像をアニメーション化する方法を示しています。
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f857accfcdec4cb60e781936d1d0836dbf8d6ffb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-and-animating-along-a-path"></a>描画し、パスに沿ってアニメーション化します。

このチュートリアルで行う応答でタッチ入力にコア グラフィックスを使用してパスの描画します。 次に、追加、`CALayer`おをパスに沿ってアニメーション化した画像を含むです。

次のスクリーン ショットは、完成したアプリケーションを示しています。

![](graphics-animation-walkthrough-images/00-final-app.png "完成したアプリケーション")

ダウンロードを始める前に、 *GraphicsDemo*このガイドに付属するサンプルです。 ダウンロードすることができます[ここ](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)内にあると、 **GraphicsWalkthrough**ディレクトリという名前のプロジェクトを起動して**GraphicsDemo_starter**をダブルクリックしてと開く、`DemoView`クラスです。

## <a name="drawing-a-path"></a>パスの描画


1. `DemoView`を追加、`CGPath`変数をクラスにコンス トラクターでインスタンス化します。 2 つの宣言も`CGPoint`変数、`initialPoint`と`latestPoint`、キャプチャ元となるパスを作成、タッチ ポイントを使用しています。
    
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

2. 次の追加ディレクティブを使用します。

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. 次に、オーバーライド`TouchesBegan`と`TouchesMoved,`それぞれ初期のタッチ ポイントと各後続のタッチ ポイントをキャプチャする次の実装を追加。

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

    `SetNeedsDisplay` タッチの順に移動するたびに呼び出される`Draw`次のループ パスで呼び出されます。

4. 行のパスに追加されます、`Draw`メソッドとを描くには、赤い破線の行を使用します。 [実装`Draw`](~/ios/platform/graphics-animation-ios/core-graphics.md)以下に示すコードで。

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

今すぐアプリケーションを実行する場合、次のスクリーン ショットに示すように、画面上に描画する接するおことができます。

![](graphics-animation-walkthrough-images/01-path.png "画面上に描画")

## <a name="animating-along-a-path"></a>パスに沿ってアニメーション化します。

パスを描画するようにするコードを実装しましたが、これでは、描画パスに沿ったレイヤーをアニメーション化するコードを追加してみましょう。

1. 最初に、追加、 [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md)変数をクラスにコンス トラクターで作成および。

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

2. 次に、ユーザーが画面から指を離したときに、ビューのレイヤーの副層として、レイヤーを追加します。 次を作成します、パスを使用してキーフレーム アニメーションをアニメーション化レイヤーの`Position`します。

    オーバーライドする必要があります。 これを実現する、`TouchesEnded`し、次のコードを追加します。

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

3. 今すぐと描画後は、アプリケーションを実行、イメージ レイヤーが追加され移動描画パスに沿った。

![](graphics-animation-walkthrough-images/00-final-app.png "イメージ レイヤーが追加され、移動、描画パス")

## <a name="summary"></a>まとめ

この記事でステップ スルー グラフィックスおよびアニメーションの概念を一緒に扱わする例です。 最初に、コア グラフィックスを使用して、パスの描画する方法を紹介しました、`UIView`ユーザー タッチへの応答。 コア アニメーションを使用して、そのパスに沿って移動イメージを作成する方法を紹介します。


## <a name="related-links"></a>関連リンク

- [コア アニメーション](~/ios/platform/graphics-animation-ios/core-animation.md)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [アニメーションのレシピをコアします。](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
