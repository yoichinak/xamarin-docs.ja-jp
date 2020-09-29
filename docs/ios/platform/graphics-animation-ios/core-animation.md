---
title: Xamarin のコアアニメーション
description: この記事では、主要なアニメーションフレームワークについて説明します。これは、パフォーマンスの向上、UIKit での滑らかなアニメーション、および下位レベルのアニメーションコントロールに対して直接使用する方法を示しています。
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d25d48421ad9b05925c1fa373ddba600ad3bac2e
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431229"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin のコアアニメーション

_この記事では、主要なアニメーションフレームワークについて説明します。これは、パフォーマンスの向上、UIKit での滑らかなアニメーション、および下位レベルのアニメーションコントロールに対して直接使用する方法を示しています。_

iOS には、アプリケーションでのビューのアニメーションサポートを提供するための [*コアアニメーション*](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) が含まれています。
テーブルのスクロールやさまざまなビュー間のスワイプなど、iOS の非常にスムーズなアニメーションはすべて、内部的にコアアニメーションに依存しているため、実行されます。

コアアニメーションとコアグラフィックスフレームワークを連携させることで、美しいアニメーションの2D グラフィックスを作成できます。 実際のコアアニメーションでは、3D 空間の2D グラフィックスを変換して、驚くような映像エクスペリエンスを作成することもできます。 ただし、真の3D グラフィックスを作成するには、OpenGL ES のようなものを使用するか、またはゲームがモノゲームなどの API に切り替える必要があります。ただし、3D はこの記事の範囲を超えています。

<a name="Using_Core_Animation"></a>

## <a name="core-animation"></a>コア アニメーション

iOS では、コアアニメーションフレームワークを使用して、ビュー間の切り替え、メニューのスライド表示、スクロール効果などのアニメーション効果を作成します。 アニメーションを操作するには、次の2つの方法があります。

- ビューベースのアニメーションだけでなく、コントローラー間のアニメーション化された遷移を含む[UIKit を使用](#Using_UIKit_Animation)します。
- [コアアニメーションを通じ](#Using_Core_Animation)て、どのレイヤーを直接使用してより細かい制御を可能にします。

<a name="Using_UIKit_Animation"></a>

## <a name="using-uikit-animation"></a>UIKit アニメーションの使用

UIKit には、アプリケーションにアニメーションを簡単に追加できるようにする機能がいくつか用意されています。 内部的なアニメーションを内部的に使用しますが、ビューとコントローラーだけを操作できるように抽象化します。

このセクションでは、次のような UIKit アニメーション機能について説明します。

- コントローラー間の切り替え
- ビュー間の切り替え
- プロパティアニメーションの表示

### <a name="view-controller-transitions"></a>ビュー コントローラーの切り替え

 `UIViewController` には、メソッドを使用したビューコントローラー間の移行のサポートが組み込まれて `PresentViewController` います。 を使用する場合 `PresentViewController` 、2つ目のコントローラーへの切り替えをアニメーション化することもできます。

たとえば、2つのコントローラーを持つアプリケーションについて考えてみます。この場合、最初のコントローラーのボタンに触れると `PresentViewController` 2 つ目のコントローラーが表示されます。 2番目のコントローラーを表示するために使用する遷移アニメーションを制御するには、 [`ModalTransitionStyle`](xref:UIKit.UIModalTransitionStyle) 次に示すようにプロパティを設定するだけです。

```csharp
SecondViewController vc2 = new SecondViewController {
  ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

この場合、 `PartialCurl` アニメーションが使用されますが、他にも次のようなものがあります。

- `CoverVertical` –画面の下部からスライドアップします。
- `CrossDissolve` –新しいビューがフェードイン & 前のビューがフェードアウトします。
- `FlipHorizontal` -右から左への水平方向の反転。 無視では、遷移は左から右へとフリップします。

遷移をアニメーション化するには、 `true` 次のように2番目の引数としてを渡し `PresentViewController` ます。

```csharp
PresentViewController (vc2, true, null);
```

次のスクリーンショットは、このような場合に移行がどのように見えるかを示してい `PartialCurl` ます。

 ![このスクリーンショットは、PartialCurl の遷移を示しています。](core-animation-images/06-view-transitions.png)

### <a name="view-transitions"></a>切り替え効果の表示

UIKit では、コントローラー間の遷移に加えて、ビュー間の切り替え効果をアニメーション化して、別のビューを切り替えることもできます。

たとえば、というコントローラーがあるとし `UIImageView` ます。この場合、イメージでタップすると2番目のが表示さ `UIImageView` れます。 イメージビューのスーパービューをアニメーション化して2番目のイメージビューに切り替えるには、を呼び出すだけで、次のように `UIView.Transition` とを渡し `toView` `fromView` ます。

```csharp
UIView.Transition (
  fromView: view1,
  toView: view2,
  duration: 2,
  options: UIViewAnimationOptions.TransitionFlipFromTop |
    UIViewAnimationOptions.CurveEaseInOut,
  completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` また、は、 `duration` アニメーションの実行時間を制御したり、 [`options`](xref:UIKit.UIViewAnimationOptions) 使用するアニメーションやイージング関数などを指定したりするためのパラメーターを受け取ります。 また、アニメーションが完了したときに呼び出される完了ハンドラーを指定することもできます。

次のスクリーンショットは、を使用した場合のイメージビュー間のアニメーション化された切り替えを示して `TransitionFlipFromTop` います。

 ![このスクリーンショットは、TransitionFlipFromTop が使用されたときのイメージビュー間のアニメーション化された切り替えを示しています。](core-animation-images/07-animated-transition.png)

### <a name="view-property-animations"></a>プロパティアニメーションの表示

UIKit では、クラスのさまざまなプロパティを自由にアニメーション化できます。これには `UIView` 次のものが含まれます。

- フレーム
- Bounds
- Center
- [アルファ]
- 変換
- Color

これらのアニメーションは `NSAction` 、静的メソッドに渡されるデリゲートでプロパティの変更を指定することによって暗黙的に行わ `UIView.Animate` れます。 たとえば、次のコードでは、の中心点をアニメーション化してい `UIImageView` ます。

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

 ![画面の上部から出力としてアニメーション化する画像](core-animation-images/08-animate-center.png)

メソッドと同様に `Transition` 、では、 `Animate` イージング関数と共に期間を設定できます。 この例では、オプションも使用して `UIViewAnimationOptions.Autoreverse` います。これにより、アニメーションは、値から最初の値にアニメーション化されます。 ただし、このコードでは、 `Center` 完了ハンドラーの初期値にも設定します。 アニメーションは、時間の経過と共にプロパティ値を補間しますが、プロパティの実際のモデル値は常に設定されている最終的な値になります。 この例では、値はスーパービューの右側にあるポイントです。 を初期の `Center` ポイントに設定しない `Autoreverse` と、次に示すように、アニメーションが完了した後、イメージは右側にスナップバックされます。

 ![中央を最初のポイントに設定しないと、アニメーションが完了した後、イメージは右側にスナップバックされます。](core-animation-images/09-animation-complete.png)

## <a name="using-core-animation"></a>コアアニメーションの使用

 `UIView` アニメーションは多数の機能を使用でき、実装が簡単なため、可能であれば使用する必要があります。 既に説明したように、UIView アニメーションでは、コアアニメーションフレームワークが使用されます。 ただし、アニメーションでは実行できないものもあり `UIView` ます。たとえば、ビューでアニメーション化できない追加のプロパティをアニメーション化したり、非線形パスに沿って補間したりすることはできません。 さらに細かい制御が必要な場合は、コアアニメーションを直接使用することもできます。

### <a name="layers"></a>レイヤー

コアアニメーションを使用する場合、アニメーションは *レイヤー*(型) を介して行われ `CALayer` ます。 レイヤーは、ビュー階層のようにレイヤー階層があるという点で、概念的にはビューに似ています。 実際には、レイヤーがビューに戻り、ビューでユーザー操作のサポートが追加されます。 ビューのプロパティを使用して、任意のビューのレイヤーにアクセスでき `Layer` ます。 実際、のメソッドで使用されるコンテキスト `Draw` `UIView` は、実際にはレイヤーから作成されます。 内部的には、をバッキングするレイヤーには、 `UIView` そのデリゲートがビュー自体に設定されます。これはを呼び出し `Draw` ます。 そのため、に描画すると `UIView` 、実際にはレイヤーに描画されます。

レイヤーアニメーションは、暗黙的または明示的にすることができます。 暗黙のアニメーションは宣言型です。 変更するレイヤープロパティを宣言するだけで、アニメーションが動作します。 一方、明示的なアニメーションは、レイヤーに追加されるアニメーションクラスを使用して作成されます。 明示的なアニメーションを使用すると、アニメーションの作成方法をさらに制御できます。 以下のセクションでは、暗黙的および明示的なアニメーションについて詳しく説明します。

### <a name="implicit-animations"></a>暗黙のアニメーション

レイヤーのプロパティをアニメーション化する方法の1つは、暗黙的なアニメーションを使用することです。 `UIView` アニメーションでは、暗黙的なアニメーションを作成します。 ただし、暗黙的なアニメーションをレイヤーに対して直接作成することもできます。

たとえば、次のコードでは、イメージからレイヤーのを設定し、 `Contents` 境界線の幅と色を設定し、ビューのレイヤーのサブレイヤーとしてレイヤーを追加しています。

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

レイヤーの暗黙的なアニメーションを追加するには、でプロパティの変更をラップするだけ `CATransaction` です。 これにより、次に示すように、ビューアニメーションで system.windows.media.animation.animatable> されないプロパティをアニメーション化でき `BorderWidth` `BorderColor` ます。

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

また、このコード `Position` では、スーパーレイヤーの座標の左上から測定されたレイヤーのアンカーポイントの位置であるレイヤーのをアニメーション化します。 レイヤーのアンカーポイントは、レイヤーの座標系内の正規化されたポイントです。

次の図は、位置とアンカーポイントを示しています。

 ![この図は、位置とアンカーポイントを示しています。](core-animation-images/10-postion-anchorpt.png)

この例を実行すると、 `Position` `BorderWidth` 次の `BorderColor` スクリーンショットに示すように、、およびアニメーション化されます。

 ![この例を実行すると、Position、BorderWidth、BorderColor は、次のようにアニメーション化されます。](core-animation-images/11-implicit-animation.png)

### <a name="explicit-animations"></a>明示的なアニメーション

暗黙的なアニメーションに加えて、コアアニメーションには、から継承されるさまざまなクラスが含まれてい `CAAnimation` ます。これを使用すると、レイヤーに明示的に追加されたアニメーションをカプセル化できます。 これにより、アニメーションの開始値の変更、アニメーションのグループ化、非線形パスを許可するためのキーフレームの指定など、アニメーションをきめ細かく制御できます。

次のコードは、前に示したレイヤーに対してを使用する明示的なアニメーションの例を示してい `CAKeyframeAnimation` ます (「暗黙的なアニメーション」セクション)。

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

このコードでは、 `Position` キーフレームアニメーションの定義に使用されるパスを作成することによって、レイヤーのを変更します。 レイヤーの `Position` がアニメーションからの最終的な値に設定されていることに注意 `Position` してください。 これを行わないと、アニメーションは実際の `Position` モデル値ではなくプレゼンテーション値のみを変更するため、レイヤーはアニメーションの前にに戻ります。 モデル値をアニメーションの最終的な値に設定すると、アニメーションの最後にレイヤーが配置されたままになります。

次のスクリーンショットは、指定されたパスをアニメーション化するイメージを含むレイヤーを示しています。

 ![このスクリーンショットは、指定されたパスをアニメーション化している画像を含むレイヤーを示しています。](core-animation-images/12-explicit-animation.png)

## <a name="summary"></a>まとめ

この記事では、 *主要なアニメーション* フレームワークによって提供されるアニメーション機能について説明しました。 ここでは、UIKit でのアニメーションの動作と、下位レベルのアニメーションコントロールに対して直接使用する方法の両方を示す、コアアニメーションを検証しています。

## <a name="related-links"></a>関連リンク

- [コアアニメーションのサンプル](/samples/xamarin/ios-samples/graphicsandanimation)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [グラフィックスとアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)