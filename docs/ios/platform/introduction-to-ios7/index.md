---
title: iOS 7 の概要
description: この記事では、iOS 7 で導入された主要な新しい Api について説明します。これには、ビューコントローラーの遷移、UIView アニメーションの機能強化、Uiview Dynamics、テキストキットが含まれます。 また、ユーザーインターフェイスに加えられた変更の一部と、新しい enchanced のマルチタスキング機能についても説明します。
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: d3a3c28e30e38562035b4d0c7c05366865157dd5
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70752061"
---
# <a name="introduction-to-ios-7"></a>iOS 7 の概要

_この記事では、iOS 7 で導入された主要な新しい Api について説明します。これには、ビューコントローラーの遷移、UIView アニメーションの機能強化、Uiview Dynamics、テキストキットが含まれます。また、ユーザーインターフェイスに加えられた変更の一部と、新しい enchanced のマルチタスキング機能についても説明します。_

ios 7 は、iOS の主要な更新プログラムです。 アプリケーション chrome ではなくコンテンツにフォーカスを移す、まったく新しいユーザーインターフェイスの設計が導入されています。 ビジュアルの変更と同時に、iOS 7 では、豊富な相互作用とエクスペリエンスを作成するための多数の新しい Api が追加されています。 このドキュメントでは、iOS 7 で導入された新しいテクノロジについて説明し、詳細な調査の出発点として機能します。

## <a name="uiview-animation-enhancements"></a>UIView アニメーションの機能強化

iOS 7 では、UIKit でのアニメーションのサポートが強化されており、以前にコアアニメーションフレームワークへの直接の削除が必要な処理をアプリケーションで実行できます。 たとえば、`UIView` は、以前は `CALayer` に適用された `CAKeyframeAnimation`、スプリングアニメーションとキーフレームアニメーションを実行できるようになりました。

### <a name="spring-animations"></a>Spring アニメーション

 `UIView` は、スプリング効果を使用してプロパティの変更をアニメーション化できるようになりました。 これを追加するには、次に示すように、`AnimateNotify` または `AnimateNotifyAsync` のいずれかのメソッドを呼び出して、spring の減衰率と最初の spring ベロシティの値を渡します。

- `springWithDampingRatio` –0と1の間の値。この値を超えると、振幅が小さくなります。
- `initialSpringVelocity` –1秒あたりのアニメーション距離の合計に対する割合で示す、最初の spring velocity。

次のコードでは、イメージビューの中心が変化したときに spring effect が生成されます。

```csharp
void AnimateWithSpring ()
{
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;

    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {

        imageView.Center = new CGPoint (imageView.Center.X, 400);

    }, null);
}
```

このばね効果は、次に示すように、イメージビューが新しい中心位置にアニメーションを完了すると、移動するように見えます。

 ![](images/spring-animation.png "This spring effect causes the image view to appear to bounce as it completes its animation to a new center location")

### <a name="keyframe-animations"></a>キーフレームアニメーション

@No__t_0 クラスに、`UIView` でキーフレームアニメーションを作成するための `AnimateWithKeyframes` メソッドが含まれるようになりました。 このメソッドは、他の `UIView` アニメーションメソッドと似ていますが、キーフレームを含む追加の `NSAction` がパラメーターとして渡される点が異なります。 @No__t_0 内では、`UIView.AddKeyframeWithRelativeStartTime` を呼び出すことによってキーフレームが追加されます。

たとえば、次のコードスニペットは、ビューの中心をアニメーション化し、ビューを回転するためのキーフレームアニメーションを作成します。

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => {
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => {
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

@No__t_0 メソッドの最初の2つのパラメーターでは、キーフレームの開始時刻と期間をそれぞれ、アニメーションの全体の長さに対する割合として指定します。 上の例では、最初の1秒間に新しい中心をアニメーション化した後、次の1秒間に90度回転したイメージビューが生成されます。 アニメーションでは `UIViewKeyframeAnimationOptions.Autoreverse` がオプションとして指定されているため、両方のキーフレームが逆にアニメーション化されます。 最後に、最終的な値は、完了ハンドラーの初期状態に設定されます。

次のスクリーンショットは、キーフレームを使用したアニメーションの結合を示しています。

 ![](images/keyframes.png "This screenshots illustrates the combined animation through the keyframes")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics は、アプリケーションが物理に基づいてアニメーション化された対話を作成できるようにする、UIKit の新しい Api のセットです。 UIKit Dynamics は、これを可能にするために2D 物理エンジンをカプセル化します。

API は本質的に宣言されています。 オブジェクトと呼ばれる*動作*を作成して、物理的な相互作用がどのように動作するかを宣言します。これは、重力、コリジョン、スプリングなどの物理概念を表します。次に、ビューをカプセル化する*動的アニメーター*と呼ばれる別のオブジェクトに動作をアタッチします。 動的なアニメーターは、宣言された物理動作を*動的な項目*(`UIView` などの `IUIDynamicItem` を実装する項目) に適用することを考慮します。

複雑な相互作用をトリガーするには、次のようなさまざまなプリミティブ動作があります。

- `UIAttachmentBehavior` –2つの動的項目が一緒に移動するように、または動的な項目を添付ファイルポイントにアタッチします。
- `UICollisionBehavior` –動的な項目が競合に参加できるようにします。
- `UIDynamicItemBehavior` –弾力性、密度、摩擦などの動的な項目に適用するプロパティの一般的なセットを指定します。
- `UIGravityBehavior`-動的な項目に重力を適用し、gravitational 方向に項目を加速させます。
- `UIPushBehavior` –動的な項目に強制的に適用されます。
- `UISnapBehavior` –動的な項目を spring 効果を持つ位置にスナップできるようにします。

多くのプリミティブがありますが、UIKit Dynamics を使用して、ビューに物理的に基づく相互作用を追加する一般的なプロセスは、動作間で一貫しています。

1. 動的なアニメーターを作成します。
1. 動作を作成します。
1. 動的なアニメーターにビヘイビアーを追加します。

### <a name="dynamics-example"></a>Dynamics の例

@No__t_0 に重心とコリジョン境界を追加する例を見てみましょう。

#### <a name="uigravitybehavior"></a>UIGravityBehavior

画像ビューに重力を追加すると、上記の3つの手順に従います。

この例では、`ViewDidLoad` メソッドで作業します。 まず、次のように `UIImageView` インスタンスを追加します。

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

これにより、画面の上端に中央揃えでイメージビューが作成されます。 画像が重力で "フォール" されるようにするには、`UIDynamicAnimator` のインスタンスを作成します。

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

@No__t_0 は、参照 `UIView` または `UICollectionViewLayout` のインスタンスを受け取ります。このインスタンスには、添付された動作ごとにアニメーション化される項目が含まれています。

次に、`UIGravityBehavior` インスタンスを作成します。 @No__t_1 のように、`IUIDynamicItem` を実装する1つ以上のオブジェクトを渡すことができます。

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

動作には `IUIDynamicItem` の配列が渡されます。この例では、アニメーション化する単一の `UIImageView` インスタンスが含まれています。

最後に、動的なアニメーターに動作を追加します。

```csharp
dynAnimator.AddBehavior (gravity);
```

次に示すように、この結果、画像は重力で下にアニメーション化されます。

![](images/gravity2.png "The starting image location")
![](images/gravity3.png "The ending image location")

画面の境界が制限されていないため、イメージビューは単に一番下にありません。 画像が画面の端と競合するようにビューを制限するには、`UICollisionBehavior` を追加します。 これについては、次のセクションで説明します。

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

まず、`UICollisionBehavior` を作成し、それを動的なアニメーターに追加します。 `UIGravityBehavior` の場合と同様です。

@No__t_0 を含めるようにコードを変更します。

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

@No__t_0 には `TranslatesReferenceBoundsIntoBoundry` というプロパティがあります。 これを `true` に設定すると、参照ビューの境界が衝突境界として使用されます。

これで、画像が重力で下にアニメーション化されたときに、残りの部分になる前に画面の下部から少し離れています。

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

その他の動作によって、イメージの表示ビューの動作をさらに制御できます。 たとえば、`UIDynamicItemBehavior` を追加して弾力性を高めることができます。これにより、画面の下部との競合が発生したときに、イメージビューのバウンスが増加します。

@No__t_0 の追加は、他の動作と同じ手順に従います。 最初に動作を作成します。

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

次に、動的なアニメーターに動作を追加します。

 `dynAnimator.AddBehavior (dynBehavior);`

この動作が適用されると、イメージビューが境界と競合すると、さらに多くの処理が行われます。

## <a name="general-user-interface-changes"></a>一般的なユーザーインターフェイスの変更

ここで説明したように、UIKit Dynamics、コントローラーの遷移、強化された Uikit アニメーションなどの新しい UIKit Api に加えて、iOS 7 では、UI に対するさまざまなビジュアル変更と、さまざまなビューとコントロールの関連 API 変更が導入されています。 詳細については、「 [iOS 7 のユーザーインターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)」を参照してください。

## <a name="text-kit"></a>テキストキット

テキストキットは、強力なテキストレイアウトとレンダリング機能を提供する新しい API です。 これは、低レベルのコアテキストフレームワークの上に構築されていますが、コアテキストよりもはるかに使いやすくなっています。

詳細については、 [Textkit](~/ios/platform/textkit.md)を参照してください。

## <a name="multitasking"></a>マルチタスキング

iOS 7 は、バックグラウンド処理を実行するタイミングと方法を変更します。 タスクがバックグラウンドで実行されている場合、iOS 7 のタスクの完了によってアプリケーションが起動されなくなり、バックグラウンド処理のためにアプリケーションが連続しない方法でウェイクアップされます。 iOS 7 では、バックグラウンドで新しいコンテンツを使用してアプリケーションを更新するために、次の3つの新しい Api も追加されています。

- [バックグラウンドフェッチ] –アプリケーションで、一定の間隔でコンテンツをバックグラウンドで更新できるようにします。
- リモート通知-アプリケーションがプッシュ通知を受信したときにコンテンツを更新できるようにします。 通知はサイレントにするか、ロック画面にバナーを表示できます。
- バックグラウンド転送サービス–サイズの大きなファイルなど、一定の時間制限なしでデータをアップロードおよびダウンロードできます。

新しいマルチタスキング機能の詳細については、Xamarin[バックグラウンド処理 guide](~/ios/app-fundamentals/backgrounding/index.md)の iOS のセクションを参照してください。

## <a name="summary"></a>まとめ

この記事では、iOS のいくつかの主要な新機能について説明します。 まず、ビューコントローラーにカスタム遷移を追加する方法を示します。 次に、コレクションビューで、ナビゲーションコントローラー内から、またはコレクションビュー間で対話形式で遷移を使用する方法を示します。 次に、UIView アニメーションに対していくつかの機能強化が施されています。これは、以前にコアアニメーションに対して直接プログラミングを行う必要がある場合に、アプリケーションが Uiview を使用する方法を示します。 最後に、新しい UIKit Dynamics API が追加されました。この API は、物理エンジンを UIKit に提供します。これは、テキストキットフレームワークで使用できるようになったリッチテキストのサポートと共に導入されました。

## <a name="related-links"></a>関連リンク

- [IOS 7 の概要 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
