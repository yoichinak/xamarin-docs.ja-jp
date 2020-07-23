---
title: Xamarin のコントローラーの遷移を表示します。 iOS
description: このドキュメントでは、Xamarin iOS アプリケーションのビューコントローラー間のアニメーションの切り替え効果をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 902e59aa9f5c4aec1dc73f10410132b500932094
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928732"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Xamarin のコントローラーの遷移を表示します。 iOS

UIKit は、ビューコントローラーを表示するときに発生するアニメーションの切り替えをカスタマイズするためのサポートを追加します。 このサポートは、組み込みのコントローラー、およびから直接継承するカスタムコントローラーに含まれてい `UIViewController` ます。 また、は、 `UICollectionViewController` コントローラー遷移のカスタマイズを利用して、コレクションビューのレイアウトのアニメーション化された遷移を活用します。

## <a name="custom-transitions"></a>カスタム遷移

IOS 7 のビューコントローラー間のアニメーション化された切り替えは、完全にカスタマイズできます。 `UIViewController`には、 `TransitioningDelegate` 遷移が発生したときにシステムにカスタムアニメータークラスを提供するプロパティが含まれるようになりました。

カスタム遷移を使用するには、次のようにし `PresentViewController` ます。

1. 表示する `ModalPresentationStyle` コントローラーのをに設定し `UIModalPresentationStyle.Custom` ます。
2. を実装して、 `UIViewControllerTransitioningDelegate` のインスタンスであるアニメータークラスを作成 `UIViewControllerAnimatedTransitioning` します。
3. 表示する `TransitioningDelegate` コントローラーでも、プロパティをのインスタンスに設定し `UIViewControllerTransitioningDelegate` ます。
4. ビューコントローラーを表示します。

たとえば、次のコードは、という型のビューコントローラーを示してい `ControllerTwo` `UIViewController` ます。

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

アプリを実行してボタンをタップすると、次に示すように、2番目のコントローラーのビューの既定のアニメーションが下部からでアニメーション化されます。

 ![アプリを実行してボタンをタップすると、2つ目のコントローラービューの既定のアニメーションが下部からアニメーション化されます。](transitions-images/no-custom-transition.png)

ただし、およびを設定する `ModalPresentationStyle` と、 `TransitioningDelegate` 移行のためのカスタムアニメーションが生成されます。

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

`TransitioningDelegate`は、 `UIViewControllerAnimatedTransitioning` 次の `CustomAnimator` 例に示すように、サブクラスのインスタンスを作成します。

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

移行が行われると、システムによってのインスタンスが作成され `IUIViewControllerContextTransitioning` 、そのインスタンスがアニメーターのメソッドに渡されます。 `IUIViewControllerContextTransitioning`アニメーションが `ContainerView` 発生する場所、および遷移を開始するビューコントローラーと、遷移するビューコントローラーを格納します。

クラスは、 `UIViewControllerAnimatedTransitioning` 実際のアニメーションを処理します。 次の2つのメソッドを実装する必要があります。

1. `TransitionDuration`–アニメーションの継続時間を秒単位で返します。
1. `AnimateTransition`–実際のアニメーションを実行します。

たとえば、次のクラスはを実装して、 `UIViewControllerAnimatedTransitioning` コントローラーのビューのフレームをアニメーション化します。

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

これで、ボタンがタップされると、クラスに実装されているアニメーション `UIViewControllerAnimatedTransitioning` が使用されます。

 ![実行中の拡大効果の例](transitions-images/custom-transition.png)

## <a name="collection-view-transitions"></a>コレクションビューの切り替え

コレクションビューには、アニメーション化された遷移を作成するためのサポートが組み込まれています。

- **ナビゲーションコントローラー** : 2 つのインスタンスの間のアニメーション化された遷移は `UICollectionViewController` 、で管理するときに、必要に応じて自動的に処理できます `UINavigationController` 。
- **遷移レイアウト**–新しいクラスによって、 `UICollectionViewTransitionLayout` レイアウト間で対話形式の切り替えを行うことができます。

### <a name="navigation-controller-transitions"></a>ナビゲーションコントローラーの遷移

ナビゲーションコントローラー内で使用する場合、には、 `UICollectionViewController` コントローラー間のアニメーション化された遷移のサポートが含まれます。 このサポートは組み込まれており、を実装するにはいくつかの簡単な手順が必要です。

1. `UseLayoutToLayoutNavigationTransitions`でをに設定 `false` `UICollectionViewController` します。
1. のインスタンス `UICollectionViewController` をナビゲーションコントローラーのスタックのルートに追加します。
1. 2番目のを作成 `UICollectionViewController` し、その `UseLayoutToLayoutNavigtionTransitions` プロパティをに設定し `true` ます。
1. 2番目のを `UICollectionViewController` ナビゲーションコントローラーのスタックにプッシュします。

次のコードでは、 `UICollectionViewController` という名前のサブクラスを `ImagesCollectionViewController` ナビゲーションコントローラーのスタックのルートに追加し `UseLayoutToLayoutNavigationTransitions` ます。プロパティはに設定されてい `false` ます。

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

項目が選択されると、の2番目のインスタンス `ImagesController` が作成されます。これは、この時点で別のレイアウトクラスを使用しています。 このコントローラーで `UseLayoutToLayoutNavigtionTransitions` は、は次のようにに設定され `true` ます。

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

プロパティは、 `UseLayoutToLayoutNavigationTransitions` コントローラーをナビゲーションスタックに追加する前に設定する必要があります。 このプロパティを設定すると、次に示すように、通常の水平方向のスライド切り替えが、2つのコントローラーのレイアウト間のアニメーション化された切り替えに置き換えられます。

![2つのコントローラーのレイアウト間のアニメーション化された切り替え](transitions-images/nav2.png)

### <a name="transition-layout"></a>移行レイアウト

ナビゲーションコントローラー内のレイアウト移行サポートに加えて、という新しいレイアウト `UICollectionViewTransitionLayout` が使用できるようになりました。 このレイアウトクラスでは、をコードから設定できるようにすることで、レイアウトの遷移プロセス中に対話型コントロールを使用でき `TransitionProgress` ます。 `UICollectionViewTransitionLayout`は、とは異なり、に代わるものではありません。これは、 `SetCollectionViewLayout` アニメーションレイアウトの遷移が発生する原因となった iOS 6 のメソッドです。 このメソッドは、アニメーション化された遷移の進行状況を制御するための組み込みサポートを提供しませんでした。

 `UICollectionViewTransitionLayout`たとえば、ユーザーの操作に応じてレイアウト間の切り替えを制御するようにジェスチャ認識エンジンを構成できます。これには、元のレイアウトと、に移行する目的のレイアウトを管理します。

を使用して、ジェスチャレコグナイザーに対話型の遷移を実装する手順は次のとおりです `UICollectionViewTransitionLayout` 。

1. ジェスチャ認識エンジンを作成します。
1. `StartInteractiveTransition`のメソッドを呼び出し `UICollectionView` 、ターゲットレイアウトと完了ハンドラーを渡します。
1. `TransitionProgress` `UICollectionViewTransitionLayout` メソッドから返されたインスタンスのプロパティを設定 `StartInteractiveTransition` します。
1. レイアウトを無効にします。
1. `FinishInteractiveTransition`のメソッドを呼び出して `UICollectionView` 遷移を完了するか、 `CancelInteractiveTransition` メソッドを呼び出してキャンセルします。  `FinishInteractiveTransition`アニメーションが対象のレイアウトへの遷移を完了させ `CancelInteractiveTransition` ます。一方、アニメーションは元のレイアウトに戻ります。
1. メソッドの完了ハンドラーで、遷移の完了を処理し `StartInteractiveTransition` ます。
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

ユーザーがコレクションビューを pinches すると、は `TransitionProgress` ピンチのスケールに対して相対的に設定されます。 この実装では、遷移が50% 完了する前にユーザーがピンチを終了すると、移行はキャンセルされます。 それ以外の場合は、移行が完了します。

## <a name="related-links"></a>関連リンク

- [IOS 7 の概要 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
