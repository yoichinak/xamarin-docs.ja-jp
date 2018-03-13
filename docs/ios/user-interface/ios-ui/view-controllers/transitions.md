---
title: "コント ローラーのトランジションの表示"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 88849d3007c007a5ac8820ca84083aa01459c4ce
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="view-controller-transitions"></a>コント ローラーのトランジションの表示

UIKit は、コント ローラーの表示を表示するときに発生するアニメーションの移行をカスタマイズするためのサポートを追加します。 このサポートは、どのから直接継承するカスタム コント ローラーと同様に、組み込みのコント ローラーに含まれる`UIViewController`です。 さらに、`UICollectionViewController`コレクション ビューのレイアウトでのアニメーション効果を活用するコント ローラーの遷移のカスタマイズを活用します。

## <a name="custom-transitions"></a>カスタム遷移

アニメーション化された iOS 7 でコント ローラーの表示切り替えは、完全にカスタマイズできます。 `UIViewController` 含まれています、`TransitioningDelegate`遷移が発生したときにシステムにカスタム アニメーター クラスを提供するプロパティです。

カスタム遷移を使用する`PresentViewController`:

1.  設定、`ModalPresentationStyle`に`UIModalPresentationStyle.Custom`を提示するコント ローラーにします。
2.  実装`UIViewControllerTransitioningDelegate`アニメーター クラスを作成するのインスタンスは`UIViewControllerAnimatedTransitioning`します。
3.  設定、`TransitioningDelegate`プロパティのインスタンスを`UIViewControllerTransitioningDelegate`、またを提示するコント ローラーにします。
4.  ビューのコント ローラーを表示します。


たとえば、次のコードがタイプのビュー コント ローラーを提示`ControllerTwo`- a`UIViewController`サブクラス。

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

アプリケーションを実行して、ボタンをタップすると次のように、下からにアニメーション化する 2 番目のコント ローラーのビューの既定のアニメーションが発生します。

 ![](transitions-images/no-custom-transition.png "アプリケーションを実行して、ボタンをタップすると、下からにアニメーション化する 2 番目のコント ローラー ビューの既定のアニメーション")

ただし、設定、`ModalPresentationStyle`と`TransitioningDelegate`遷移のアニメーションの結果します。

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate`のインスタンスを作成するには、`UIViewControllerAnimatedTransitioning`サブクラス - と呼ばれる`CustomAnimator`次の例。

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning PresentingController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

システムがのインスタンスを作成、遷移の実行時`IUIViewControllerContextTransitioning`アニメーターのメソッドに渡すことができます。 `IUIViewControllerContextTransitioning` 含まれています、`ContainerView`だけでなく、移行を開始するビューのコント ローラーおよびに移行されているビュー コント ローラー、アニメーションが発生します。

`UIViewControllerAnimatedTransitioning`クラスは、実際のアニメーションを処理します。 2 つのメソッドを実装する必要があります。

1.  `TransitionDuration` –、アニメーションの時間を秒単位で返します。
1.  `AnimateTransition` – 実際のアニメーションを実行します。


たとえば、次のクラスを実装`UIViewControllerAnimatedTransitioning`コント ローラーのビューのフレームをアニメーション化します。

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

ここで、ボタンをタップすると、ときに、アニメーションとして実装された、`UIViewControllerAnimatedTransitioning`クラスを使用します。

 ![](transitions-images/custom-transition.png "有効で実行されているズームの例")

## <a name="collection-view-transitions"></a>コレクション ビューの切り替え

コレクション ビューには、アニメーション効果を作成するための組み込みサポートがあります。

-  **コント ローラーのナビゲーション**– 2 つの間の遷移のアニメーション`UICollectionViewController`インスタンスことができます必要に応じて自動的に処理すると、`UINavigationController`を管理します。
-  **レイアウトを切り替え**– 新しい`UICollectionViewTransitionLayout`クラスにより、対話型のレイアウトを遷移します。


### <a name="navigation-controller-transitions"></a>ナビゲーション コント ローラーの遷移

ナビゲーションのコント ローラー内で使用されるときに、`UICollectionViewController`コント ローラー間のアニメーション効果のサポートが含まれています。 このサポートが組み込まれており、のみを実装するいくつかの簡単な手順が必要です。

1.  設定`UseLayoutToLayoutNavigationTransitions`に`false`上、`UICollectionViewController`です。
1.  インスタンスを追加、`UICollectionViewController`ナビゲーション コント ローラーのスタックのルートにします。
1.  1 秒あたりの作成`UICollectionViewController`設定とその`UseLayoutToLayoutNavigtionTransitions`プロパティを`true`です。
1.  2 番目のプッシュ`UICollectionViewController`ナビゲーション コント ローラーのスタックにします。


次のコードを追加、`UICollectionViewController`という名前のサブクラス`ImagesCollectionViewController`ナビゲーション コント ローラーのスタックのルートで、`UseLayoutToLayoutNavigationTransitions`プロパティに設定`false`:

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
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

項目を選択すると、2 番目のインスタンス、`ImagesController`作成されると、別のレイアウトのクラスを使用してこの時点のみです。 このコント ローラーの`UseLayoutToLayoutNavigtionTransitions`に設定されている`true`、次のようにします。

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
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions`ナビゲーション スタックに、コント ローラーを追加する前にプロパティを設定する必要があります。 このプロパティ セットを持つ通常の水平方向のスライディング遷移が以下に示すよう、アニメーションの移行の 2 つのコント ローラーのレイアウトに置き換えられます。

![](transitions-images/nav2.png "レイアウトを 2 つのコント ローラーのアニメーションの移行")

### <a name="transition-layout"></a>遷移のレイアウト

ナビゲーションのコント ローラー内でレイアウトの移行のサポート、だけでなく、新しいレイアウトと呼ばれる`UICollectionViewTransitionLayout`が利用できるようになりました。 このレイアウト クラスでは、レイアウトの移行プロセス中に対話型コントロールを使用できるようにして、`TransitionProgress`をコードから設定します。 `UICollectionViewTransitionLayout` 異なる - との置換ではありません -、`SetCollectionViewLayout`アニメーション化されたレイアウト遷移を発生の原因となった iOS 6 からメソッドです。 そのメソッドでアニメーションの移行の進行状況を制御するための組み込みサポートを提供しませんでした。

 `UICollectionViewTransitionLayout` たとえば、ジェスチャ レコグナイザーの元のレイアウトだけでなく、目的のレイアウトへの移行を管理することにより、ユーザーとの対話に応答でレイアウト間の遷移の制御を構成することを許可します。

使用してジェスチャ レコグナイザー内の対話型の遷移を実装する手順`UICollectionViewTransitionLayout`次に示します。

1.  ジェスチャ レコグナイザーを作成します。
1.  呼び出す、`StartInteractiveTransition`のメソッド、`UICollectionView`ターゲットのレイアウトと、完了ハンドラーを渡します。
1.  設定、`TransitionProgress`のプロパティ、`UICollectionViewTransitionLayout`から返されるインスタンス、`StartInteractiveTransition`メソッドです。
1.  レイアウトが無効にします。
1.  呼び出す、`FinishInteractiveTransition`のメソッド、`UICollectionView`切り替えを完了する、または`CancelInteractiveTransition`メソッドをキャンセルします。  `FinishInteractiveTransition` 一方、アニメーション、ターゲットのレイアウトへの切り替えを完了するを原因`CancelInteractiveTransition`アニメーションの元のレイアウトに返す結果します。
1.  処理の完了ハンドラーで遷移補完、`StartInteractiveTransition`メソッドです。
1.  コレクション ビューには、ジェスチャ レコグナイザーを追加します。


次のコードでは、ピンチ ジェスチャ レコグナイザー内の対話型のレイアウト遷移を実装します。

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

ユーザー pinches コレクション ビューと、`TransitionProgress`ピンチ操作のスケールから相対的に設定します。 この実装では、遷移が 50% が完了すると、前に、ユーザーが、ピンチ操作を終了した場合、遷移はキャンセルされます。 それ以外の場合、移行が完了しました。




## <a name="related-links"></a>関連リンク

- [IOS 7 (サンプル) の概要](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
