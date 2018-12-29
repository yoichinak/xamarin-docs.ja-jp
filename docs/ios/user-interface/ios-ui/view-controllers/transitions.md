---
title: Xamarin.iOS でビュー コント ローラーの切り替え
description: このドキュメントでは、Xamarin.iOS アプリケーションでのビュー コント ローラー間のアニメーション効果をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 6711d3af06619aa54a2d735cb83862ed64abacec
ms.sourcegitcommit: 06f88979db160fb8dd1c9ee0d5000d8749107489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806900"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Xamarin.iOS でビュー コント ローラーの切り替え

UIKit では、ビュー コント ローラーを表示するときに発生するアニメーション遷移をカスタマイズするためのサポートを追加します。 このサポートはから直接継承するカスタムのコント ローラーと同様に、組み込みのコント ローラーに含まれている`UIViewController`します。 さらに、`UICollectionViewController`コレクション ビューのレイアウトの切り替えのアニメーションを利用するコント ローラーの遷移のカスタマイズを活用します。

## <a name="custom-transitions"></a>カスタムの遷移

IOS 7 でのビュー コント ローラーの間のアニメーション化された移行は完全にカスタマイズ可能です。 `UIViewController` 含まれています、`TransitioningDelegate`遷移が発生した場合、システムにカスタム animator クラスを提供するプロパティ。

カスタムの遷移を使用して`PresentViewController`:

1.  設定、`ModalPresentationStyle`に`UIModalPresentationStyle.Custom`でコント ローラーに表示されます。
2.  実装`UIViewControllerTransitioningDelegate`、animator クラスを作成するのインスタンスである`UIViewControllerAnimatedTransitioning`します。
3.  設定、`TransitioningDelegate`プロパティのインスタンスを`UIViewControllerTransitioningDelegate`、表示するコント ローラーにもします。
4.  ビュー コント ローラーを紹介します。


たとえば、次のコードは型のビュー コント ローラーを表示します。 `ControllerTwo` -`UIViewController`サブクラスです。

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

アプリを実行し、ボタンをタップして次に示すように、下からアニメーション化する 2 番目のコント ローラーのビューの既定のアニメーションが発生します。

 ![](transitions-images/no-custom-transition.png "アプリを実行し、ボタンをタップすると、下からのアニメーション化する 2 番目のコント ローラー ビューの既定のアニメーション")

ただし、設定、`ModalPresentationStyle`と`TransitioningDelegate`遷移のアニメーションになります。

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

`TransitioningDelegate`のインスタンスを作成する責任を負いますが、`UIViewControllerAnimatedTransitioning`サブクラス - と呼ばれる`CustomAnimator`次の例で。

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

システムがのインスタンスを作成して、移行が行われるときに`IUIViewControllerContextTransitioning`animator のメソッドに渡されることができます。 `IUIViewControllerContextTransitioning` 含まれています、`ContainerView`だけでなく、移行を開始するビュー コント ローラーおよびビュー コント ローラーに移行するため、アニメーションが発生します。

`UIViewControllerAnimatedTransitioning`クラスは、実際のアニメーションを処理します。 2 つのメソッドを実装する必要があります。

1.  `TransitionDuration` –、アニメーションの時間を秒単位で返します。
1.  `AnimateTransition` – 実際のアニメーションを実行します。


たとえば、次のクラス実装`UIViewControllerAnimatedTransitioning`コント ローラーのビューのフレームをアニメーション化します。

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

ここで、ボタンをタップすると、アニメーションはで実装された、`UIViewControllerAnimatedTransitioning`クラスを使用します。

 ![](transitions-images/custom-transition.png "実行されている有効なズームの例")

## <a name="collection-view-transitions"></a>コレクション ビューの切り替え

コレクション ビューでは、切り替えのアニメーションを作成するための組み込みのサポートがあります。

-  **ナビゲーション コント ローラー** – 2 つの間の遷移のアニメーション`UICollectionViewController`インスタンスできます必要に応じて自動的に処理すると、`UINavigationController`それらを管理します。
-  **レイアウトの切り替え**– 新しい`UICollectionViewTransitionLayout`クラスでは対話型のレイアウトの間で移行できます。


### <a name="navigation-controller-transitions"></a>ナビゲーション コント ローラーの切り替え

ナビゲーション コント ローラー内で使用すると、`UICollectionViewController`コント ローラー間で切り替えのアニメーションのサポートが含まれています。 このサポートが組み込まれており、のみを実装するいくつかの簡単な手順が必要です。

1.  設定`UseLayoutToLayoutNavigationTransitions`に`false`上、`UICollectionViewController`します。
1.  インスタンスを追加、`UICollectionViewController`ナビゲーション コント ローラーのスタックのルートにします。
1.  1 秒あたりの作成`UICollectionViewController`設定とその`UseLayoutToLayoutNavigtionTransitions`プロパティを`true`します。
1.  2 つ目のプッシュ`UICollectionViewController`ナビゲーション コント ローラーのスタックにします。


次のコードを追加、`UICollectionViewController`という名前のサブクラス`ImagesCollectionViewController`、ナビゲーション コント ローラーのスタックのルートにで、`UseLayoutToLayoutNavigationTransitions`プロパティに設定`false`:

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

項目が選択されている場合、2 番目のインスタンス、`ImagesController`別のレイアウトのクラスを使用してこの時点でのみ、作成されます。 このコント ローラーの`UseLayoutToLayoutNavigtionTransitions`に設定されている`true`以下に示すように。

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

`UseLayoutToLayoutNavigationTransitions`コント ローラーをナビゲーション スタックに追加する前にプロパティを設定する必要があります。 このプロパティ セットを含む通常の水平方向のスライディング遷移は次のように 2 つのコント ローラーのレイアウトの切り替えがアニメーション化されたに置き換えられます。

![](transitions-images/nav2.png "2 つのコント ローラーのレイアウトの間のアニメーションの移行")

### <a name="transition-layout"></a>レイアウトの切り替え

ナビゲーション コント ローラー内でレイアウトの切り替えサポート、だけでなく、新しいレイアウトと呼ばれる`UICollectionViewTransitionLayout`られるようになりました。 このレイアウト クラスでは、レイアウトの移行プロセス中に対話型のコントロールを使用できるようにして、`TransitionProgress`コードから設定します。 `UICollectionViewTransitionLayout` 異なる - との置換ではありません -、`SetCollectionViewLayout`メソッド iOS 6、アニメーション化されたレイアウトの遷移を発生の原因となったからです。 メソッドは、アニメーション化された移行の進行状況を制御するための組み込みサポートを提供しませんでした。

 `UICollectionViewTransitionLayout` たとえばに移行する目的のレイアウトと元のレイアウトを管理することで、ユーザーの操作への応答でレイアウト間の遷移を制御するように構成するジェスチャ レコグナイザーを使用できます。

使用してジェスチャ レコグナイザー内で対話型の遷移を実装する手順`UICollectionViewTransitionLayout`次に示します。

1.  ジェスチャ レコグナイザーを作成します。
1.  呼び出す、`StartInteractiveTransition`のメソッド、`UICollectionView`ターゲットのレイアウトと完了ハンドラーに渡します。
1.  設定、`TransitionProgress`のプロパティ、`UICollectionViewTransitionLayout`から返されるインスタンス、`StartInteractiveTransition`メソッド。
1.  レイアウトを無効にします。
1.  呼び出す、`FinishInteractiveTransition`のメソッド、`UICollectionView`切り替えを完了するか、`CancelInteractiveTransition`メソッドを取り消してください。  `FinishInteractiveTransition` 一方、ターゲットのレイアウトへの切り替えを完了するアニメーションを原因`CancelInteractiveTransition`アニメーションが元のレイアウトに返すことになります。
1.  処理の完了ハンドラーが完成したら、移行、`StartInteractiveTransition`メソッド。
1.  コレクション ビューには、ジェスチャ レコグナイザーを追加します。


次のコードでは、ピンチ ジェスチャ認識エンジン内で対話型のレイアウトの切り替えを実装します。

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

ユーザーは、コレクション ビューを pinches よう、`TransitionProgress`ピンチのスケールから相対的に設定されます。 この実装での移行が完了すると、50% する前に、ユーザーは、ピンチが終了した場合、遷移が取り消されました。 それ以外の場合、移行が完了しました。




## <a name="related-links"></a>関連リンク

- [IOS 7 (サンプル) の概要](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
