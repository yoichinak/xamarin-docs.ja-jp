---
title: Xamarin.iOS でのコア グラフィックスとアニメーションのコアの使用
description: この記事でステップ バイ ステップ コア グラフィックスとアニメーションのコアを使用するアプリケーションを作成する方法を示します。 ユーザーのタッチへの応答では、画面に描画する方法と、パスに沿って移動するイメージをアニメーション化する方法を示しています。
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 2690b60abe963cf7b02ca282b32091098a224ccf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104246"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Xamarin.iOS でのコア グラフィックスとアニメーションのコアの使用

このチュートリアルでは、応答でタッチ入力にコア グラフィックスを使用して、パスを描画するためにいきます。 追加し、`CALayer`パスに沿ってアニメーション化することは、イメージを格納しています。

次のスクリーン ショットは、完成したアプリケーションを示しています。

![](graphics-animation-walkthrough-images/00-final-app.png "完成したアプリケーション")

ダウンロードを始める前に、 *GraphicsDemo*このガイドに付属するサンプルです。 ダウンロードすることができます[ここ](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)の内側にあると、 **GraphicsWalkthrough**ディレクトリという名前のプロジェクトを起動する**GraphicsDemo_starter**をダブルクリックして、開く、`DemoView`クラス。

## <a name="drawing-a-path"></a>パスの描画


1. `DemoView`追加、`CGPath`変数をクラスに、コンス トラクターでインスタンス化します。 2 つの宣言も`CGPoint`変数、`initialPoint`と`latestPoint`パスを構築します、タッチ ポイントをキャプチャすることが使用します。
    
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

    `SetNeedsDisplay` ためにタッチが移動するたびに呼び出される`Draw`実行ループの次のパスで呼び出されます。

4. 内のパスに追加します線、`Draw`メソッドとを描くには、赤色の破線のラインを使用します。 [実装`Draw`](~/ios/platform/graphics-animation-ios/core-graphics.md)以下に示すコードで。

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

今すぐアプリケーションを実行した場合は、次のスクリーン ショットに示すように、画面に描画するためにタッチことができます。

![](graphics-animation-walkthrough-images/01-path.png "画面上に描画")

## <a name="animating-along-a-path"></a>パスに沿ってアニメーション化

これで、パスを描画するためにユーザーを許可するコードを実装しましたが、描画パスに沿ってレイヤーをアニメーション化するコードを追加してみましょう。

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

2. 次に、ユーザーが画面から指を離したときに、ビューのレイヤーの副層としてレイヤーを追加します。 次に、作成、パスを使用してキーフレーム アニメーション レイヤーのアニメーション化`Position`します。

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

3. これで、および描画後にアプリケーションを実行イメージ レイヤーが追加され移動描画パス。

![](graphics-animation-walkthrough-images/00-final-app.png "イメージ レイヤーが追加され、移動、描画パス")

## <a name="summary"></a>まとめ

この記事では、グラフィックスとアニメーションの概念相互に関連する例をステップ実行します。 最初に、コア グラフィックスを使用して、パスを描画する方法を紹介しました、`UIView`ユーザー タッチに応答します。 コア アニメーションを使用して、そのパスに沿って移動イメージを作成する方法を紹介しましたその後。


## <a name="related-links"></a>関連リンク

- [コア アニメーション](~/ios/platform/graphics-animation-ios/core-animation.md)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [コア アニメーションのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
