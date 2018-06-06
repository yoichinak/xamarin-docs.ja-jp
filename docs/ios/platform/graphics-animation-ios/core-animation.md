---
title: Xamarin.iOS でのアニメーションのコア
description: この記事では、表示することにより高いパフォーマンス、方法 UIKit で滑らかなアニメーションをアニメーションの下位のコントロールを直接使用する、コア アニメーション フレームワークについて説明します。
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5cc6019ed148b870e38659eb30ac7f2738481a50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786821"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin.iOS でのアニメーションのコア

_この記事では、表示することにより高いパフォーマンス、方法 UIKit で滑らかなアニメーションをアニメーションの下位のコントロールを直接使用する、コア アニメーション フレームワークについて説明します。_

iOS を含む[*コア アニメーション*](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html)アプリケーションでのビューのアニメーション サポートを提供します。
すべてのテーブルのスクロールとさまざまなビューの間での読み取りなど、iOS で滑らかなアニメーションを実行できるだけでなくコア アニメーションに内部的に基づいているため、操作を行います。

コア アニメーションとコア グラフィックス フレームワークは連携して、すばらしいを作成する 2 次元グラフィックスをアニメーション化します。 実際にはコア アニメーションでも変換できます 3D 空間で 2 次元グラフィックスすばらしい、映画のようなエクスペリエンスを作成します。 ただし、3 D グラフィックスの場合は true を作成するにする必要があります OpenGL ES と同様に、またはゲームを有効にする MonoGame などの API を使用する 3D は、この記事の範囲外ですが。

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>コア アニメーション

iOS では、アニメーションのコア フレームワークを使用して、ビューの間で移行中、メニューをスライドさせて、いくつかの例の効果をスクロールなどのアニメーション効果を作成します。 これにはアニメーションを使用する 2 つの方法があります。

- [UIKit を介して](#Using_UIKit_Animation)アニメーションのビューをベースとコント ローラー間のアニメーション効果が含まれます。
- [コア アニメーションを介して](#Using_Core_Animation)、細かい制御を許可する、直接レイヤー。

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>UIKit アニメーションを使用します。

UIKit をアプリケーションにアニメーションを追加するが簡単にするいくつかの機能を提供します。 コア アニメーションを内部的には、使用していますを抽象にビューとコント ローラーだけを使用するようにします。

このセクションでは、含む UIKit アニメーション機能について説明します。

-  コント ローラー間の遷移
-  ビュー間の遷移
-  ビュー プロパティのアニメーション


### <a name="view-controller-transitions"></a>コント ローラーのトランジションの表示

 `UIViewController` を通じてビュー コント ローラー間で移行中の組み込みサポートを提供、`PresentViewController`メソッドです。 使用する場合`PresentViewController`、2 番目のコント ローラーへの切り替えをアニメーション化できる必要に応じて。

たとえば、最初のコント ローラー内のボタンを変更することを呼び出す 2 つのコント ローラー アプリケーション`PresentViewController`を 2 番目のコント ローラーを表示します。 どのような遷移アニメーションを使用して、2 番目のコント ローラーの表示を制御する設定だけでその[ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/)次に示すようにプロパティ。

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

この場合、`PartialCurl`などの他のいくつかのユーザーは、アニメーションが使用します。

-  `CoverVertical` – スライドを画面の下部から
-  `CrossDissolve` – フェードアウトします元のビュー (&)、新しいビューがフェードイン
-  `FlipHorizontal` 水平方向の右から左への上下を反転します。 棄却では、遷移は、左から右へを反転します。


切り替えをアニメーション化する渡す`true`2 番目の引数として`PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

次のスクリーン ショットを示しています、遷移`PartialCurl`場合。

 ![](core-animation-images/06-view-transitions.png "このスクリーン ショットは、PartialCurl 移行を示しています。")

### <a name="view-transitions"></a>ビューの切り替え

UIKit には、コント ローラー間の遷移だけでなく、別の 1 つのビューをスワップするビューの間で切り替え効果をアニメーション化もサポートしています。

たとえばとしていたコント ローラーで`UIImageView`画像をタップするが、秒を表示する場所、`UIImageView`です。 画像をアニメーション化する 2 つ目のイメージ ビューへの移行のビューのスーパー ビューは呼び出し元などの単純な`UIView.Transition`、渡す、`toView`と`fromView`次のように。

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` 受け取る、`duration`をどのくらいの時間、アニメーションの実行を制御するパラメーターとして[ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/)を使用し、イージング関数をアニメーションなどを指定します。 さらに、アニメーションが完了したときに呼び出される完了ハンドラーを指定できます。

次に示すスクリーン ショットは、イメージの間のアニメーション化された移行ビュー`TransitionFlipFromTop`を使用します。

 ![](core-animation-images/07-animated-transition.png "このスクリーン ショットは、アニメーション化された移行 TransitionFlipFromTop を使用する場合は、イメージ ビュー間を示しています。")

### <a name="view-property-animations"></a>ビュー プロパティのアニメーション

UIKit でさまざまなプロパティをアニメーション化をサポートしている、`UIView`を含むクラスを無料で。

-  フレーム
-  境界
-  中央揃え
-  [アルファ]
-  変換
-  色


これらのアニメーションがプロパティの変更を指定することによって暗黙的に発生する可能性、 `NSAction` 、静的に渡されたデリゲート`UIView.Animate`メソッドです。 たとえば、次のコードがの中心点をアニメーション化、 `UIImageView`:

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

これは、結果、次に示すように、画像をアニメーション化する前後、画面の一番上に。

 ![](core-animation-images/08-animate-center.png "画像をアニメーション化する前後、画面の一番上に、出力として")

同様、`Transition`メソッド、`Animate`イージング関数と共に、設定する期間を使用します。 この例を使用しても、`UIViewAnimationOptions.Autoreverse`オプションは、最初に値をアニメーション化するアニメーションします。 ただし、コードも設定、`Center`完了ハンドラーに初期値に戻す。 アニメーションは、時間の経過と共に、プロパティの値を補間は、プロパティの実際のモデル値は常に最終的な値が設定されています。 この例では、値は、スーパー ビューの右側付近のポイントです。 設定を指定せず、`Center`最初のポイントにあるために、アニメーションの終了位置、`Autoreverse`設定されているイメージに戻ります、右側にあるアニメーションが完了したら、次のようにします。

 ![](core-animation-images/09-animation-complete.png "最初のポイントを中心を設定しない場合、イメージに戻ります、右側にある、アニメーションの終了後に")

## <a name="using-core-animation"></a>コア アニメーションを使用します。

 `UIView` アニメーションは、多数の機能を使用して、実装の容易性のための可能な場合に使用する必要があります。 前述のように、UIView アニメーションはコア アニメーション フレームワークを使用します。 ただし、いくつかの点で実行できない`UIView`アニメーション、ビュー、アニメーション化することはできません、追加のプロパティをアニメーション化するなど、非線形のパスに沿った補間します。 このような場合は、さらに細かく制御する必要があります、コア アニメーションを直接にも使用できます。

### <a name="layers"></a>レイヤー

によって行われアニメーション、コア アニメーションを使用するときに*レイヤー*、型のある`CALayer`です。 レイヤーは、レイヤーの階層が、はるかには、階層の表示をビューに概念的に似ています。 実際には、レイヤーは、ビュー、ユーザーの操作のサポートを追加するビューをバックアップします。 ビューの経由で任意のビューの層にアクセスすることができます`Layer`プロパティです。 使用されるコンテキストの実際には、`Draw`メソッドの`UIView`レイヤーから実際に作成します。 内部的には、バックアップ、レイヤー、`UIView`そのデリゲートを呼び出すものであるビュー自体に設定を持つ`Draw`します。 描画するときに、 `UIView`、実際には、レイヤーを描画します。

レイヤーのアニメーションは、暗黙的または明示的に指定できます。 暗黙的なアニメーションは、宣言型。 レイヤーのプロパティで変更する必要がありますを単に宣言して、アニメーションがだけ動作します。 その一方で、明示的なアニメーションはレイヤーに追加されるアニメーション クラスを使用して作成されます。 明示的なアニメーションでは、追加のアニメーションの作成方法を制御できるようにします。 次のセクションでは、さらに深いレベルで暗黙的および明示的なアニメーションを掘り下げてです。

### <a name="implicit-animations"></a>暗黙的なアニメーション

レイヤーのプロパティをアニメーション化する 1 つの方法は、暗黙的なアニメーションを使用してです。 `UIView` アニメーションは、暗黙的なアニメーションを作成します。 ただし、レイヤーもに対して直接暗黙的なアニメーションを作成できます。

次のコードのレイヤーの設定など、`Contents`イメージからの罫線の幅と色を設定し、ビューのレイヤーの副層として、レイヤーを追加します。

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

レイヤーの暗黙的なアニメーションを追加するには、単純にプロパティの変更をラップする`CATransaction`です。 これによりと思われるビューのアニメーションでアニメーションなどのプロパティをアニメーション化する、`BorderWidth`と`BorderColor`次のようにします。

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

このコードでは、レイヤーのもアニメーション化`Position`superlayer の座標の左上から測定レイヤーのアンカー ポイントの場所であります。 レイヤーのアンカー点は、レイヤーの座標システム内で正規化された点です。

次の図は、位置とアンカー ポイントを示しています。

 ![](core-animation-images/10-postion-anchorpt.png "この図では、位置とアンカー ポイントを示しています。")

この例を実行すると、ときに、 `Position`、`BorderWidth`と`BorderColor`次のスクリーン ショットに示すようにアニメーション化します。

 ![](core-animation-images/11-implicit-animation.png "この例を実行すると、位置、BorderWidth および BorderColor をアニメーション化するように")

### <a name="explicit-animations"></a>明示的なアニメーション

だけでなく、暗黙のアニメーションは、コア アニメーションは、さまざまなから継承するクラスを含まれています。`CAAnimation`レイヤーに明示的に追加し、アニメーションをカプセル化するためです。 これらには、アニメーションのアニメーションの開始値を変更する、アニメーションをグループ化、非線形のパスを許可するキーフレームを指定するなど、細かい制御ができるようにします。

次のコードを使用して、明示的なアニメーションの例を示します、`CAKeyframeAnimation`前 (暗黙的なアニメーション セクション) で示したレイヤー。

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

このコードを変更、`Position`キーフレーム アニメーションを定義し、使用されるパスを作成することで、レイヤーのです。 注意してレイヤーの`Position`の最終的な値に設定されている、`Position`アニメーションからです。 これを行わない場合、レイヤーとその場に戻り、`Position`アニメーションする前に、アニメーションは、プレゼンテーションの値と実際のモデル値ではなくにのみ変更されるためです。 最終的な値にアニメーションからのモデル値を設定、レイヤーは、アニメーションの最後の位置に留まります。

次のスクリーン ショットは、指定されたパスをアニメーション化イメージを含むレイヤーを表示します。

 ![](core-animation-images/12-explicit-animation.png "このスクリーン ショットは、指定されたパスをアニメーション化イメージを含むレイヤーを示しています。")
 
## <a name="summary"></a>まとめ

この記事の内容について説明しましたを介して提供されるアニメーション機能、*コア アニメーション*フレームワークです。 ここには、コア アニメーション、表示方法に電源がオン UIKit でのアニメーションと下位レベルのアニメーション コントロールの直接の使用方法の両方が調べられます。

## <a name="related-links"></a>関連リンク

- [コア アニメーションのサンプル](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [コア グラフィックス](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [グラフィックスおよびアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
