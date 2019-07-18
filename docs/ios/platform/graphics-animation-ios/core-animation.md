---
title: Xamarin.iOS でコア アニメーション
description: この記事では、下位レベルのアニメーション コントロールで直接使用する方法についても、UIKit で滑らかなアニメーションの高パフォーマンスが使用する方法を示す、Core アニメーション フレームワークについて説明します。
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a40d0911b7dabc900a4c6e50c692e4f091f22be9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61206354"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin.iOS でコア アニメーション

_この記事では、下位レベルのアニメーション コントロールで直接使用する方法についても、UIKit で滑らかなアニメーションの高パフォーマンスが使用する方法を示す、Core アニメーション フレームワークについて説明します。_

iOS を含む[*コア アニメーション*](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html)アプリケーションでのビューのアニメーション サポートを提供します。
コア アニメーションに内部的に依存するためともすべてのテーブルのスクロールとさまざまなビューでスワイプなどの iOS で滑らかなアニメーションの実行します。

コア アニメーションとグラフィックスのコア フレームワークが美しく、作成する連携 2D グラフィックスをアニメーション化します。 実際にコア アニメーションも変換できます 3D 空間を 2D グラフィックス驚くような映画のようなエクスペリエンスを作成します。 ただし、真の 3D グラフィックスを作成することになる OpenGL ES のように、またはゲームを有効にする、MonoGame などの API を使用する 3D は、この記事の範囲外ですが。

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>コア アニメーション

iOS では、コア アニメーション フレームワークを使用して、ビュー間で移行、メニューのスライディング効果をいくつかの名前をスクロールなどのアニメーション効果を作成します。 アニメーションを使用する 2 つの方法はあります。

- [UIKit を介して](#Using_UIKit_Animation)、ビュー ベースのアニメーションとコント ローラー間のアニメーション効果が含まれます。
- [コア アニメーションを使用して](#Using_Core_Animation)、どのレイヤーを直接を細かく制御することができます。

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>UIKit アニメーションを使用します。

UIKit では、アプリケーションにアニメーションを追加しやすくいくつかの機能を提供します。 内部的には、コア アニメーションが、その抽象、ビューとコント ローラーだけを使用するため。

このセクションでは、含む UIKit アニメーション機能について説明します。

-  コント ローラー間の遷移
-  ビュー間の遷移
-  ビュー プロパティのアニメーション


### <a name="view-controller-transitions"></a>ビュー コントローラーの切り替え

 `UIViewController` を介してビュー コント ローラー間で移行の組み込みサポートを提供します、`PresentViewController`メソッド。 使用する場合`PresentViewController`、2 番目のコント ローラーへの移行は必要に応じてアニメーション化されることができます。

たとえば、最初のコント ローラー内のボタンをタッチを呼び出す 2 つのコント ローラーを持つアプリケーション`PresentViewController`を 2 番目のコント ローラーを表示します。 どのような遷移のアニメーションを使用して、2 番目のコント ローラーの表示を制御する設定だけその[ `ModalTransitionStyle` ](xref:UIKit.UIModalTransitionStyle)次に示すようにプロパティ。

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

この場合、`PartialCurl`などの他のいくつかのユーザーは、アニメーションを使います。

-  `CoverVertical` – スライドを画面の下部にあります。
-  `CrossDissolve` – フェードアウトします元のビュー (&)、新しいビューをフェードイン
-  `FlipHorizontal` 水平方向の右から左に反転します。 解雇では、移行は、左から右を反転します。


移行をアニメーション化する渡す`true`2 番目の引数として`PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

次のスクリーン ショット移行がどのように、`PartialCurl`ケース。

 ![](core-animation-images/06-view-transitions.png "このスクリーン ショットは、PartialCurl 移行を示しています。")

### <a name="view-transitions"></a>ビューの切り替え

UIKit には、コント ローラー間の遷移だけでなく、別の 1 つのビューを切り替えるビューを切り替えをアニメーション化もサポートしています。

たとえば、コント ローラーでがたとえば`UIImageView`、画像をタップが秒を表示する必要があります`UIImageView`します。 イメージをアニメーション化する 2 つ目のイメージ ビューへの移行のビューのスーパーが呼び出す`UIView.Transition`、渡す、`toView`と`fromView`次に示します。

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` また、`duration`アニメーション実行時間の長さを制御するパラメーターだけでなく[ `options` ](xref:UIKit.UIViewAnimationOptions)およびイージング関数を使用する、アニメーションなどを指定します。 さらに、アニメーションの完了時に呼び出される完了ハンドラーを指定することができます。

次に示すスクリーン ショット イメージ遷移のアニメーション化されたときにビュー`TransitionFlipFromTop`使用されます。

 ![](core-animation-images/07-animated-transition.png "このスクリーン ショットは、TransitionFlipFromTop を使用する場合は、イメージ ビューの間でアニメーション化された移行を示しています。")

### <a name="view-property-animations"></a>ビュー プロパティのアニメーション

UIKit でさまざまなプロパティをアニメーション化をサポートしている、`UIView`などを無料でクラスします。

-  フレーム
-  境界
-  中央揃え
-  [アルファ]
-  変換
-  色


これらのアニメーションがプロパティの変更を指定することで暗黙的に行われること、 `NSAction` 、静的に渡されたデリゲート`UIView.Animate`メソッド。 次のコードの中心点をアニメーション化など、 `UIImageView`:

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

これは、結果、次に示すように、イメージをアニメーション化までの間、画面の上部に。

 ![](core-animation-images/08-animate-center.png "イメージをアニメーション化を行き来画面の上部に、出力として")

同様、`Transition`メソッド、`Animate`イージング関数を設定する期間を使用します。 この例の使用も、`UIViewAnimationOptions.Autoreverse`オプションは、最初に、値からアニメーション化するアニメーションします。 ただし、コードも設定、`Center`完了ハンドラーでその初期値に戻ります。 アニメーションは、時間の経過と共にプロパティ値を補間は、プロパティの実際のモデル値は常に設定されている最後の値。 この例では、値は、スーパー ビューの右側の近くのポイントが。 設定がない場合、`Center`最初のポイントにあるために、アニメーションが完了する、`Autoreverse`設定されているイメージに戻ります、右側にあるアニメーションが完了すると、次に示すよう。

 ![](core-animation-images/09-animation-complete.png "最初の点を中心を設定しなくても、イメージに戻ります、右側にあるアニメーションの完了後")

## <a name="using-core-animation"></a>コア アニメーションを使用します。

 `UIView` アニメーションは、多くの機能を許可して、容易な実装のための可能な場合に使用する必要があります。 前述のように、UIView アニメーションは、コア アニメーション フレームワークを使用します。 ただし、いくつかの点で実行できない`UIView`アニメーションでは、ビュー、アニメーション化することはできません、追加のプロパティをアニメーション化など、非線形パスに沿って補間します。 このような場合は、さらに細かく制御する必要があります、コア アニメーションを直接にも使用できます。

### <a name="layers"></a>レイヤー

アニメーションの動作を使用してコア アニメーションを使用する場合*レイヤー*、これは`CALayer`します。 レイヤーは層階層では、かなりビュー階層があるようにビューに概念的に似ています。 実際には、レイヤーは、ビュー、ユーザーの操作のサポートを追加するビューをバックアップします。 ビューの経由で任意のビューの層にアクセスすることができます`Layer`プロパティ。 コンテキストが実際で使用される`Draw`メソッドの`UIView`レイヤーから実際に作成されます。 内部的には、バックアップ、レイヤーを`UIView`そのデリゲートを呼び出すものであるビュー自体に設定を持つ`Draw`します。 描画するときに、 `UIView`、その層に実際に描画します。

レイヤーのアニメーションは、暗黙的または明示的にできます。 暗黙的なアニメーションでは、宣言型です。 レイヤーのプロパティで変更する必要があります単に宣言し、アニメーションの動作だけです。 その一方で、明示的なアニメーションはレイヤーに追加されるアニメーション クラスを使用して作成されます。 明示的なアニメーションでは、アニメーションを作成する方法をさらに制御を許可します。 次のセクションでは、さらに詳しく暗黙的および明示的なアニメーションについて掘り下げ。

### <a name="implicit-animations"></a>暗黙的なアニメーション

レイヤーのプロパティをアニメーション化する方法の 1 つでは、暗黙のアニメーション使用します。 `UIView` アニメーションは、暗黙的なアニメーションを作成します。 ただし、レイヤーもに対して直接暗黙的なアニメーションを作成できます。

たとえば、次のコードはレイヤーを設定します。`Contents`イメージからの罫線の幅と色を設定し、ビューのレイヤーの副層としてレイヤーを追加します。

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

レイヤーの暗黙のアニメーションを追加するには、単純にプロパティの変更をラップする`CATransaction`します。 これにより、されていないビューのアニメーションでは、アニメーションなどのプロパティをアニメーション化、`BorderWidth`と`BorderColor`次に示すよう。

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

このコードでは、レイヤーのもアニメーション化`Position`superlayer の座標の左上から測定のレイヤーのアンカー ポイントの場所であります。 レイヤーのアンカー ポイントは、レイヤーの座標システム内で正規化されたポイントです。

次の図は、位置とアンカー ポイントを示しています。

 ![](core-animation-images/10-postion-anchorpt.png "この図は、位置とアンカー ポイントを示しています。")

例が実行される、 `Position`、`BorderWidth`と`BorderColor`次のスクリーン ショットに示すようにアニメーション化します。

 ![](core-animation-images/11-implicit-animation.png "位置、BorderWidth および BorderColor が示すようにアニメーション化例が実行されます。")

### <a name="explicit-animations"></a>明示的なアニメーション

コア アニメーションにはだけでなく、暗黙のアニメーションにはさまざまなクラスを継承するクラスが含まれています`CAAnimation`レイヤーに明示的に追加し、アニメーションをカプセル化することができます。 これにより、アニメーションの開始値を変更、アニメーションをグループ化および非線形パスを許可するキーフレームを指定するなど、アニメーションの詳細に制御できます。

次のコードを使用して、明示的なアニメーションの例を示します、`CAKeyframeAnimation`前 (暗黙のアニメーション セクション) で示したレイヤー。

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

このコードの変更、`Position`キーフレーム アニメーションを定義し、使用されるパスを作成してレイヤーの。 注意してレイヤーの`Position`の最終的な値に設定されている、`Position`アニメーションから。 これを行わない、レイヤーは突然に戻りますその`Position`アニメーション前に、アニメーションは、プレゼンテーションの値と実際のモデル値ではなくにのみ変更されるためです。 アニメーションをモデル値を最終的な値に設定は、レイヤーは、アニメーションの終了位置で維持します。

次のスクリーン ショットは、指定されたパスは、イメージのアニメーションを含むレイヤーを表示します。

 ![](core-animation-images/12-explicit-animation.png "このスクリーン ショットには、指定されたパスは、イメージのアニメーションを含むレイヤーが表示されます。")
 
## <a name="summary"></a>まとめ

この記事でを介して提供されるアニメーション機能しました、*コア アニメーション*フレームワーク。 コア アニメーション、UIKit でのアニメーションを強化する方法と下位レベルのアニメーション コントロールの直接の使用方法の両方を調べる。

## <a name="related-links"></a>関連リンク

- [コア アニメーションのサンプル](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [グラフィックスとアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
