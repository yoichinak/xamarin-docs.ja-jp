---
title: Xamarin のコントローラーの遷移を表示します。 iOS
description: このドキュメントでは、Xamarin iOS アプリケーションのビューコントローラー間のアニメーションの切り替え効果をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: f0886ac9d47e7ab08a6f74365bcc25163b803e11
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002405"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Xamarin のコントローラーの遷移を表示します。 iOS

UIKit は、ビューコントローラーを表示するときに発生するアニメーションの切り替えをカスタマイズするためのサポートを追加します。 このサポートは、組み込みのコントローラー、および `UIViewController`から直接継承するカスタムコントローラーに含まれています。 さらに、`UICollectionViewController` は、コントローラー遷移のカスタマイズを利用して、コレクションビューのレイアウトのアニメーション化された遷移を活用します。

## <a name="custom-transitions"></a>カスタム遷移

IOS 7 のビューコントローラー間のアニメーション化された切り替えは、完全にカスタマイズできます。 `UIViewController` には、遷移が発生したときにカスタムのアニメータークラスをシステムに提供する `TransitioningDelegate` プロパティが含まれるようになりました。

`PresentViewController`でカスタム遷移を使用するには:

1. 表示するコントローラーで `ModalPresentationStyle` を `UIModalPresentationStyle.Custom` に設定します。
2. `UIViewControllerTransitioningDelegate` を実装して、`UIViewControllerAnimatedTransitioning` のインスタンスであるアニメータークラスを作成します。
3. 表示するコントローラーでも、`TransitioningDelegate` プロパティを `UIViewControllerTransitioningDelegate` のインスタンスに設定します。
4. ビューコントローラーを表示します。

たとえば、次のコードは、`UIViewController` サブクラス `ControllerTwo` 型のビューコントローラーを示しています。

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

アプリを実行してボタンをタップすると、次に示すように、2番目のコントローラーのビューの既定のアニメーションが下部からでアニメーション化されます。

 ![](transitions-images/no-custom-transition.png "Running the app and tapping the button causes the default animation of the second controllers view to animate in from the bottom")

ただし、`ModalPresentationStyle` と `TransitioningDelegate` を設定すると、移行のためのカスタムアニメーションが生成されます。

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` は、次の例に示すように、`UIViewControllerAnimatedTransitioning` サブクラスと呼ばれる `CustomAnimator` のインスタンスを作成します。

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning GetAnimationControllerForPresentedController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

移行が行われると、システムによって `IUIViewControllerContextTransitioning`のインスタンスが作成され、そのインスタンスがアニメーターのメソッドに渡されます。 `IUIViewControllerContextTransitioning` には、アニメーションが発生した `ContainerView`、および遷移を開始するビューコントローラーと、切り替え先のビューコントローラーが含まれています。

`UIViewControllerAnimatedTransitioning` クラスは、実際のアニメーションを処理します。 次の2つのメソッドを実装する必要があります。

1. `TransitionDuration` –アニメーションの期間を秒単位で返します。
1. `AnimateTransition` –実際のアニメーションを実行します。

たとえば、次のクラスは、コントローラーのビューのフレームをアニメーション化するために `UIViewControllerAnimatedTransitioning` を実装しています。

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
    {
        var inView = transitionContext.ContainerView;
        var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
        var toView = toVC.View;

        inView.AddSubview (toView);

        var frame = toView.Frame;
        toView.Frame = CGRect.Empty;

        UIView.Animate (TransitionDuration (transitionContext), () => {
            toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
        }, () => {
            transitionContext.CompleteTransition (true);
        });
    }
}
```

これで、ボタンがタップされると、`UIViewControllerAnimatedTransitioning` クラスに実装されているアニメーションが使用されます。

 ![](transitions-images/custom-transition.png "An example of the zoom in effect running")

## <a name="collection-view-transitions"></a>コレクションビューの切り替え

コレクションビューには、アニメーション化された遷移を作成するためのサポートが組み込まれています。

- **ナビゲーションコントローラー** – `UINavigationController` によって管理されている場合は、必要に応じて、2つの `UICollectionViewController` インスタンス間のアニメーション化された遷移を自動的に処理できます。
- **遷移レイアウト**–新しい `UICollectionViewTransitionLayout` クラスを使用すると、レイアウト間で対話的な切り替えを行うことができます。

### <a name="navigation-controller-transitions"></a>ナビゲーションコントローラーの遷移

ナビゲーションコントローラー内で使用する場合、`UICollectionViewController` には、コントローラー間のアニメーション化された遷移のサポートが含まれます。 このサポートは組み込まれており、を実装するにはいくつかの簡単な手順が必要です。

1. `UICollectionViewController` で `false` するように `UseLayoutToLayoutNavigationTransitions` を設定します。
1. ナビゲーションコントローラーのスタックのルートに `UICollectionViewController` のインスタンスを追加します。
1. 2つ目の `UICollectionViewController` を作成し、その `UseLayoutToLayoutNavigtionTransitions` プロパティを `true` に設定します。
1. 2番目の `UICollectionViewController` をナビゲーションコントローラーのスタックにプッシュします。

次のコードでは、`ImagesCollectionViewController` という名前の `UICollectionViewController` サブクラスをナビゲーションコントローラーのスタックのルートに追加します。 `UseLayoutToLayoutNavigationTransitions` プロパティは `false`に設定されています。

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false
        };

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();

    return true;
}
```

項目を選択すると、`ImagesController` の2番目のインスタンスが作成されます。この時点では、別のレイアウトクラスを使用します。 このコントローラーでは、次に示すように、`UseLayoutToLayoutNavigtionTransitions` は `true`に設定されます。

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
    circleLayout = new CircleLayout (Monkeys.Instance.Count){
        ItemSize = new CGSize (100, 100)
    };

    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true
        };

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions` プロパティは、コントローラーをナビゲーションスタックに追加する前に設定する必要があります。 このプロパティを設定すると、次に示すように、通常の水平方向のスライド切り替えが、2つのコントローラーのレイアウト間のアニメーション化された切り替えに置き換えられます。

![](transitions-images/nav2.png "An animated transition between the layouts of the two controllers")

### <a name="transition-layout"></a>移行レイアウト

ナビゲーションコントローラー内のレイアウト移行サポートに加えて、`UICollectionViewTransitionLayout` という新しいレイアウトが使用できるようになりました。 このレイアウトクラスでは、`TransitionProgress` をコードから設定できるようにすることで、レイアウトの遷移プロセス中に対話型コントロールを使用できます。 `UICollectionViewTransitionLayout` は、に代わるものではなく、iOS 6 の `SetCollectionViewLayout` メソッドであり、アニメーション化されたレイアウトの切り替えが発生する原因となっています。 このメソッドは、アニメーション化された遷移の進行状況を制御するための組み込みサポートを提供しませんでした。

 `UICollectionViewTransitionLayout` を使用すると、たとえば、ユーザーの操作に応じてレイアウト間の切り替えを制御するようにジェスチャ認識エンジンを構成できます。これには、元のレイアウトと、に移行する目的のレイアウトを管理します。

`UICollectionViewTransitionLayout` を使用してジェスチャレコグナイザーに対話型の遷移を実装する手順は次のとおりです。

1. ジェスチャ認識エンジンを作成します。
1. `UICollectionView` の `StartInteractiveTransition` メソッドを呼び出して、ターゲットレイアウトと完了ハンドラーを渡します。
1. `StartInteractiveTransition` メソッドから返された `UICollectionViewTransitionLayout` インスタンスの `TransitionProgress` プロパティを設定します。
1. レイアウトを無効にします。
1. `UICollectionView` の `FinishInteractiveTransition` メソッドを呼び出して遷移を完了するか、`CancelInteractiveTransition` メソッドを呼び出してキャンセルします。  `FinishInteractiveTransition` を実行すると、アニメーションはターゲットのレイアウトへの遷移を完了しますが、`CancelInteractiveTransition` 結果は元のレイアウトに戻ります。
1. `StartInteractiveTransition` メソッドの完了ハンドラーで、遷移の完了を処理します。
1. ジェスチャ認識エンジンをコレクションビューに追加します。

次のコードは、ピンチのジェスチャレコグナイザー内で対話型レイアウトの切り替えを実装しています。

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

ユーザーがコレクションビューを pinches すると、`TransitionProgress` はピンチのスケールに対して相対的に設定されます。 この実装では、遷移が50% 完了する前にユーザーがピンチを終了すると、移行はキャンセルされます。 それ以外の場合は、移行が完了します。

## <a name="related-links"></a>関連リンク

- [IOS 7 の概要 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
