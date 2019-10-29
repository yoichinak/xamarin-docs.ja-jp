---
title: Xamarin のコアアニメーション
description: この記事では、主要なアニメーションフレームワークについて説明します。これは、パフォーマンスの向上、UIKit での滑らかなアニメーション、および下位レベルのアニメーションコントロールに対して直接使用する方法を示しています。
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 60bab56440fc7227e14d31875a8b6108cd1a86f3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032482"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin のコアアニメーション

_この記事では、主要なアニメーションフレームワークについて説明します。これは、パフォーマンスの向上、UIKit での滑らかなアニメーション、および下位レベルのアニメーションコントロールに対して直接使用する方法を示しています。_

iOS には、アプリケーションでのビューのアニメーションサポートを提供するための[*コアアニメーション*](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html)が含まれています。
テーブルのスクロールやさまざまなビュー間のスワイプなど、iOS の非常にスムーズなアニメーションはすべて、内部的にコアアニメーションに依存しているため、実行されます。

コアアニメーションとコアグラフィックスフレームワークを連携させることで、美しいアニメーションの2D グラフィックスを作成できます。 実際のコアアニメーションでは、3D 空間の2D グラフィックスを変換して、驚くような映像エクスペリエンスを作成することもできます。 ただし、真の3D グラフィックスを作成するには、OpenGL ES のようなものを使用するか、またはゲームがモノゲームなどの API に切り替える必要があります。ただし、3D はこの記事の範囲を超えています。

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>コア アニメーション

iOS では、コアアニメーションフレームワークを使用して、ビュー間の切り替え、メニューのスライド表示、スクロール効果などのアニメーション効果を作成します。 アニメーションを操作するには、次の2つの方法があります。

- ビューベースのアニメーションだけでなく、コントローラー間のアニメーション化された遷移を含む[UIKit を使用](#Using_UIKit_Animation)します。
- [コアアニメーションを通じ](#Using_Core_Animation)て、どのレイヤーを直接使用してより細かい制御を可能にします。

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>UIKit アニメーションの使用

UIKit には、アプリケーションにアニメーションを簡単に追加できるようにする機能がいくつか用意されています。 内部的なアニメーションを内部的に使用しますが、ビューとコントローラーだけを操作できるように抽象化します。

このセクションでは、次のような UIKit アニメーション機能について説明します。

- コントローラー間の切り替え
- ビュー間の切り替え
- プロパティアニメーションの表示

### <a name="view-controller-transitions"></a>ビュー コントローラーの切り替え

 `UIViewController` には、`PresentViewController` 方法を使用してビューコントローラー間を移行するための組み込みサポートが用意されています。 `PresentViewController`を使用する場合は、必要に応じて2つ目のコントローラーへの移行をアニメーション化できます。

たとえば、2つのコントローラーを持つアプリケーションがあるとします。1つ目のコントローラーのボタンに触れると `PresentViewController` が呼び出され、2つ目のコントローラーが表示されます。 2番目のコントローラーを表示するために使用する遷移アニメーションを制御するには、次のように[`ModalTransitionStyle`](xref:UIKit.UIModalTransitionStyle)プロパティを設定します。

```csharp
SecondViewController vc2 = new SecondViewController {
  ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

この場合、`PartialCurl` のアニメーションが使用されますが、他にも次のようなものがあります。

- `CoverVertical` –画面の下部からスライドアップします。
- `CrossDissolve`-古いビューはフェードアウトし & 新しいビューがフェードインします。
- `FlipHorizontal`-右から左への水平方向の反転です。 無視では、遷移は左から右へとフリップします。

遷移をアニメーション化するには、`PresentViewController`の2番目の引数として `true` を渡します。

```csharp
PresentViewController (vc2, true, null);
```

次のスクリーンショットは、`PartialCurl` の場合の遷移を示しています。

 ![](core-animation-images/06-view-transitions.png "This screenshot shows the PartialCurl transition")

### <a name="view-transitions"></a>切り替え効果の表示

UIKit では、コントローラー間の遷移に加えて、ビュー間の切り替え効果をアニメーション化して、別のビューを切り替えることもできます。

たとえば、`UIImageView`を持つコントローラーがあるとします。この場合、イメージでタップすると2つ目の `UIImageView`が表示されます。 イメージビューのスーパービューをアニメーション化して2番目のイメージビューに切り替えるには、`UIView.Transition`を呼び出したときと同じように単純にし、次に示すように `toView` と `fromView` を渡します。

```csharp
UIView.Transition (
  fromView: view1,
  toView: view2,
  duration: 2,
  options: UIViewAnimationOptions.TransitionFlipFromTop |
    UIViewAnimationOptions.CurveEaseInOut,
  completion: () => { Console.WriteLine ("transition complete"); });
```

また `UIView.Transition` は、アニメーションの実行時間を制御する `duration` パラメーターと、使用するアニメーションやイージング関数などを指定するための[`options`](xref:UIKit.UIViewAnimationOptions)を受け取ります。 また、アニメーションが完了したときに呼び出される完了ハンドラーを指定することもできます。

次のスクリーンショットは、`TransitionFlipFromTop` が使用されている場合のイメージビュー間のアニメーション化された切り替えを示しています。

 ![](core-animation-images/07-animated-transition.png "This screenshot shows the animated transition between the image views when TransitionFlipFromTop is used")

### <a name="view-property-animations"></a>プロパティアニメーションの表示

UIKit では、`UIView` クラスのさまざまなプロパティを自由にアニメーション化できます。これには次のものが含まれます。

- フレーム
- 線
- 中央揃え
- [アルファ]
- Transform
- Color

これらのアニメーションは、静的な `UIView.Animate` メソッドに渡される `NSAction` デリゲートでプロパティの変更を指定することによって暗黙的に行われます。 たとえば、次のコードは、`UIImageView`の中心点をアニメーション化します。

```csharp
pt = imgView.Center;

UIView.Animate (
  duration: 2, 
  delay: 0, 
  options: UIViewAnimationOptions.CurveEaseInOut | 
    UIViewAnimationOptions.Autoreverse,
  animation: () => {
    imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
      - imgView.Frame.Width / 2, pt.Y);},
  completion: () => {
    imgView.Center = pt; }
);
```

次に示すように、この結果、イメージが画面の上部で前後にアニメーション化されます。

 ![](core-animation-images/08-animate-center.png "An image animating back and forth across the top of the screen as the output")

`Transition` メソッドと同様に、`Animate` では、イージング関数と共に期間を設定できます。 この例では、`UIViewAnimationOptions.Autoreverse` オプションも使用しています。これにより、アニメーションは、値から最初の値にアニメーション化されます。 ただし、このコードでは、`Center` を完了ハンドラーの初期値に設定することもできます。 アニメーションは、時間の経過と共にプロパティ値を補間しますが、プロパティの実際のモデル値は常に設定されている最終的な値になります。 この例では、値はスーパービューの右側にあるポイントです。 最初のポイントに `Center` を設定しないと、次に示すように、アニメーションが完了した後に、アニメーションが `Autoreverse` 完了した後で、イメージが右側にスナップバックされます。

 ![](core-animation-images/09-animation-complete.png "Without setting the Center to the initial point, the image would snap back to the right side after the animation completes")

## <a name="using-core-animation"></a>コアアニメーションの使用

 `UIView` のアニメーションでは多数の機能が許可されるため、実装が簡単になるため、可能であれば使用する必要があります。 既に説明したように、UIView アニメーションでは、コアアニメーションフレームワークが使用されます。 ただし、`UIView` のアニメーションでは実行できないものもあります。たとえば、ビューでアニメーション化できない追加のプロパティをアニメーション化する場合や、非線形パスに沿って補間する場合などです。 さらに細かい制御が必要な場合は、コアアニメーションを直接使用することもできます。

### <a name="layers"></a>レイヤー

コアアニメーションを使用する場合、アニメーションは `CALayer`型の*レイヤー*を介して行われます。 レイヤーは、ビュー階層のようにレイヤー階層があるという点で、概念的にはビューに似ています。 実際には、レイヤーがビューに戻り、ビューでユーザー操作のサポートが追加されます。 ビューの `Layer` プロパティを使用して、任意のビューのレイヤーにアクセスできます。 実際、`UIView` の `Draw` メソッドで使用されるコンテキストは、実際にはレイヤーから作成されます。 内部的には、`UIView` をバッキングするレイヤーには、そのデリゲートがビュー自体に設定されます。これは `Draw`呼び出しです。 したがって、`UIView`に描画すると、実際にはレイヤーに描画されます。

レイヤーアニメーションは、暗黙的または明示的にすることができます。 暗黙のアニメーションは宣言型です。 変更するレイヤープロパティを宣言するだけで、アニメーションが動作します。 一方、明示的なアニメーションは、レイヤーに追加されるアニメーションクラスを使用して作成されます。 明示的なアニメーションを使用すると、アニメーションの作成方法をさらに制御できます。 以下のセクションでは、暗黙的および明示的なアニメーションについて詳しく説明します。

### <a name="implicit-animations"></a>暗黙のアニメーション

レイヤーのプロパティをアニメーション化する方法の1つは、暗黙的なアニメーションを使用することです。 `UIView` アニメーションは、暗黙的なアニメーションを作成します。 ただし、暗黙的なアニメーションをレイヤーに対して直接作成することもできます。

たとえば、次のコードでは、イメージからレイヤーの `Contents` を設定し、境界線の幅と色を設定し、ビューのレイヤーのサブレイヤーとしてレイヤーを追加します。

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  layer = new CALayer ();
  layer.Bounds = new CGRect (0, 0, 50, 50);
  layer.Position = new CGPoint (50, 50);
  layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
  layer.ContentsGravity = CALayer.GravityResize;
  layer.BorderWidth = 1.5f;
  layer.BorderColor = UIColor.Green.CGColor;

  View.Layer.AddSublayer (layer);
}
```

レイヤーに対して暗黙的なアニメーションを追加するには、`CATransaction`でプロパティの変更をラップするだけです。 これにより、次に示すように、`BorderWidth` や `BorderColor` などのビューアニメーションで system.windows.media.animation.animatable> されないプロパティをアニメーション化できます。

```csharp
public override void ViewDidAppear (bool animated)
{
  base.ViewDidAppear (animated);

  CATransaction.Begin ();
  CATransaction.AnimationDuration = 10;
  layer.Position = new CGPoint (50, 400);
  layer.BorderWidth = 5.0f;
  layer.BorderColor = UIColor.Red.CGColor;
  CATransaction.Commit ();
}
```

このコードは、スーパーレイヤーの座標の左上から測定されたレイヤーのアンカーポイントの位置であるレイヤーの `Position`もアニメーション化します。 レイヤーのアンカーポイントは、レイヤーの座標系内の正規化されたポイントです。

次の図は、位置とアンカーポイントを示しています。

 ![](core-animation-images/10-postion-anchorpt.png "This figure shows the position and anchor point")

この例を実行すると、次のスクリーンショットに示すように、`Position`、`BorderWidth`、および `BorderColor` アニメーション化されます。

 ![](core-animation-images/11-implicit-animation.png "When the example is run, the Position, BorderWidth and BorderColor animate as shown")

### <a name="explicit-animations"></a>明示的なアニメーション

暗黙的なアニメーションに加えて、コアアニメーションには、レイヤーに明示的に追加されたアニメーションをカプセル化できるようにする `CAAnimation` から継承するさまざまなクラスが含まれています。 これにより、アニメーションの開始値の変更、アニメーションのグループ化、非線形パスを許可するためのキーフレームの指定など、アニメーションをきめ細かく制御できます。

次のコードは、前に示したレイヤー ([暗黙的なアニメーション] セクション) の `CAKeyframeAnimation` を使用した明示的なアニメーションの例を示しています。

```csharp
public override void ViewDidAppear (bool animated)
{
  base.ViewDidAppear (animated);
  
  // get the initial value to start the animation from
  CGPoint fromPt = layer.Position;
  
  /* set the position to coincide with the final animation value
  to prevent it from snapping back to the starting position
  after the animation completes*/
  layer.Position = new CGPoint (200, 300);
  
  // create a path for the animation to follow
  CGPath path = new CGPath ();
  path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
  
  // create a keyframe animation for the position using the path
  CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
  animPosition.Path = path;
  animPosition.Duration = 2;
  
  // add the animation to the layer.
  /* the "position" key is used to overwrite the implicit animation created
  when the layer positino is set above*/
  layer.AddAnimation (animPosition, "position");
}
```

このコードでは、キーフレームアニメーションの定義に使用されるパスを作成することによって、レイヤーの `Position` を変更します。 レイヤーの `Position` が、アニメーションの `Position` の最終的な値に設定されていることに注意してください。 この操作を行わないと、アニメーションは実際のモデル値ではなくプレゼンテーション値のみを変更するため、レイヤーはアニメーションの前に `Position` に戻ります。 モデル値をアニメーションの最終的な値に設定すると、アニメーションの最後にレイヤーが配置されたままになります。

次のスクリーンショットは、指定されたパスをアニメーション化するイメージを含むレイヤーを示しています。

 ![](core-animation-images/12-explicit-animation.png "This screenshot shows the layer containing the image animating through the specified path")

## <a name="summary"></a>まとめ

この記事では、*主要なアニメーション*フレームワークによって提供されるアニメーション機能について説明しました。 ここでは、UIKit でのアニメーションの動作と、下位レベルのアニメーションコントロールに対して直接使用する方法の両方を示す、コアアニメーションを検証しています。

## <a name="related-links"></a>関連リンク

- [コアアニメーションのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [グラフィックスとアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
