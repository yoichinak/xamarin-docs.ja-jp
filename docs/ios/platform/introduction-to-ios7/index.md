---
title: IOS 7 の概要
description: この記事では、iOS ビュー コント ローラーの遷移、UIView アニメーション、UIKit Dynamics とテキストのキットの機能強化を含め、7 で導入された主要な新しい Api について説明します。 また、一部のユーザー インターフェイス、および新しい強化マルチタスク機能への変更について説明します。
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: db2ce779962947e2121ff03280544a080e193e2e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118046"
---
# <a name="introduction-to-ios-7"></a>IOS 7 の概要

_この記事では、iOS ビュー コント ローラーの遷移、UIView アニメーション、UIKit Dynamics とテキストのキットの機能強化を含め、7 で導入された主要な新しい Api について説明します。また、一部のユーザー インターフェイス、および新しい強化マルチタスク機能への変更について説明します。_

iOS 7 では、iOS へのメジャー アップデートです。 アプリケーションではなくコンテンツのクロムでのフォーカスの設定を完全に新しいユーザー インターフェイスの設計が導入されています。 ビジュアルの変更と並行は、iOS 7 は、多くの高度な相互作用とエクスペリエンスを作成する新しい Api を追加します。 このドキュメントの調査、新しいテクノロジでは、iOS 7 で導入され、さらに探索の開始点として機能します。

## <a name="uiview-animation-enhancements"></a>UIView アニメーションの機能強化

iOS 7 では、UIKit、Core アニメーション フレームワークに直接ドロップするが必要だった作業を行うアプリケーションを可能でアニメーションのサポートが強化されます。 たとえば、`UIView`跳ねるアニメーションだけでなく、キーフレーム アニメーションを実行できるようになりましたが以前、`CAKeyframeAnimation`に適用される、 `CALayer`。

### <a name="spring-animations"></a>Spring のアニメーション

 `UIView` spring 効果が適用されたプロパティの変更をアニメーション化をサポートします。 を追加するには、いずれかを呼び出す、`AnimateNotify`または`AnimateNotifyAsync`メソッドを以下に示すよう、spring の減衰率と、spring の初期速度の値で渡します。

-  `springWithDampingRatio` – 0 ~ 1 に、振動がより小さい値を増加値。
-  `initialSpringVelocity` 1 秒あたりの合計のアニメーションの距離の割合として spring の初期速度。


次のコードでは、イメージのビューの中心の変更されたときに、spring 効果が生成されます。

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

この spring 効果を新しい中央の場所に、アニメーションが完了するをバウンド以下に示すように表示するイメージの表示が発生します。

 ![](images/spring-animation.png "この spring 効果により、跳ねる center の新しい場所に、アニメーションが完了すると表示されるイメージ ビュー")

### <a name="keyframe-animations"></a>キーフレーム アニメーション

`UIView`クラスが含まれています、`AnimateWithKeyframes`でキーフレーム アニメーションを作成するためのメソッド、`UIView`します。 このメソッドは、その他のような`UIView`いる点を除き、アニメーションのメソッドを追加`NSAction`キーフレームは、パラメーターとして渡されます。 内で、 `NSAction`、キーフレームは、呼び出しによって追加された`UIView.AddKeyframeWithRelativeStartTime`します。

たとえば、次のコード スニペットは、ビューを回転させるか、ビューの中心もをアニメーション化するキーフレーム アニメーションを作成します。

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

最初の 2 つのパラメーターを`AddKeyframeWithRelativeStartTime`メソッド、開始時刻と期間を指定したキーフレームのそれぞれに、全体的なアニメーションの長さの割合として。 結果は、イメージ ビューをアニメーション化にその新しいセンター経由での最初の 2 番目では、上記の例で、次の 2 つ目に 90 度の回転後にします。 アニメーションが指定するため`UIViewKeyframeAnimationOptions.Autoreverse`も逆の順序でオプションとして、両方のキーフレームをアニメーション化します。 最後に、最終的な値は、完了ハンドラーの初期状態に設定されます。

次のスクリーン ショットは、結合されたキーフレーム アニメーションを示しています。

 ![](images/keyframes.png "このスクリーン ショットは、結合されたキーフレーム アニメーションを示しています。")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit UIKit での Api を使用する物理法則に基づいてアニメーションの対話を作成するアプリケーションの新しいセットです。 UIKit Dynamics は、これを可能にする、2 D 物理エンジンをカプセル化します。

API は、本質的に宣言します。 呼ばれる、オブジェクトを作成して、物理運動の相互作用の動作を宣言する*動作*- 重力や衝突、springs などの高速な物理学の概念をします。呼ばれる別のオブジェクトに、動作がありますをアタッチし、*動的 animator*ビューをカプセル化します。 動的アニメーターに宣言された物理学の動作を適用する必要は、*動的アイテム*-アイテムを実装する`IUIDynamicItem`など、`UIView`します。

動作があるいくつか異なるプリミティブなど、複雑な相互作用をトリガーに使用できます。

-  `UIAttachmentBehavior` –、一緒に移動するよう動的の 2 つの項目を付加したり、添付ファイルのポイントに動的な項目をアタッチします。
-  `UICollisionBehavior` – 衝突に参加する動的な項目を使用できます。
-  `UIDynamicItemBehavior` – 一般的な一連の弾力性、密度摩擦などの動的なアイテムに適用するプロパティを指定します。
-  `UIGravityBehavior` -重力を重力の方向に加速する項目を原因と、動的な項目に適用されます。
-  `UIPushBehavior` – Force を項目を動的に適用されます。
-  `UISnapBehavior` – Spring 効果が適用された位置にスナップする動的な項目をできます。


多くのプリミティブは UIKit Dynamics を使用してビューを物理ベースの相互作用を追加するための一般的なプロセスは一貫性のある動作の間で。

1.  動的 animator を作成します。
1.  問題を作成します。
1.  動的アニメーターには、動作を追加します。


### <a name="dynamics-example"></a>Dynamics の例

重力や衝突の境界を追加する例を見て、`UIView`します。

#### <a name="uigravitybehavior"></a>UIGravityBehavior

重力を追加するイメージのビューに、上記で説明した 3 つの手順に従います。

操作を行います。、`ViewDidLoad`この例のメソッド。 最初に、追加、`UIImageView`インスタンスの次のようにします。

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

これには、画面の上端を中心とイメージのビューが作成されます。 重力をイメージ"fall"するためのインスタンスを作成、 `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

`UIDynamicAnimator`の参照のインスタンスを受け取り`UIView`または`UICollectionViewLayout`、アタッチされた問題ごと、アニメーション化する項目が含まれています。

次に、作成、`UIGravityBehavior`インスタンス。 実装する 1 つまたは複数のオブジェクトを渡すことができます、`IUIDynamicItem`と同様に、 `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

動作の配列を渡します`IUIDynamicItem`、ここでは、1 つを含む`UIImageView`インスタンスをアニメーション化します。

最後に、動的アニメーターに動作を追加します。

```csharp
dynAnimator.AddBehavior (gravity);
```

これは、結果、重力は、以下を参照として使用下方向をアニメーション化するイメージ。

![](images/gravity2.png "イメージの開始場所") 
![](images/gravity3.png "終了のイメージの場所")

何も画面の境界の制約があるためは、イメージ ビューは、一番下から単純に分類されます。 追加、イメージは、画面の端と競合するために、ビューを制約することができます、`UICollisionBehavior`します。 この次のセクションで説明します。

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

まず、作成、`UICollisionBehavior`に対して実行したのと同じように、動的のアニメーターに追加して、`UIGravityBehavior`します。

含めるコードの変更、 `UICollisionBehavior`:

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

`UICollisionBehavior`という名前のプロパティを持つ`TranslatesReferenceBoundsIntoBoundry`します。 これを設定`true`衝突境界として使用するビューの範囲、参照が発生します。

ここで、イメージは、重力を下にアニメーション化、ときに跳ね返ります若干画面の下部に存在する前にします。

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

さらに、落下のイメージ ビューの追加の動作の動作を制御できます。 たとえば、追加でした、`UIDynamicItemBehavior`原因で、画面の下部にぶつかったときの詳細は跳ねるイメージ ビュー、弾力性を向上させる。

追加、`UIDynamicItemBehavior`他の動作と同じ手順に従います。 まず、動作を作成します。

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

次に、動的アニメーターに動作を追加します。

 `dynAnimator.AddBehavior (dynBehavior);`

この動作で、境界ぶつかったときの詳細はイメージの表示を bounces します。

## <a name="general-user-interface-changes"></a>一般的なユーザー インターフェイスの変更

Api だけでなく、新しい UIKit UIKit Dynamics、コント ローラーの切り替え、上記で説明した UIView アニメーションの強化など、iOS 7 では、さまざまな UI を視覚的な変更とさまざまなビューとコントロールの関連する API の変更が導入されています。 詳細については、、 [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)を参照してください。

## <a name="text-kit"></a>テキストのキット

テキストのキットは、強力なテキスト レイアウトとレンダリングの機能を提供する新しい API です。 低レベルの主要なテキストのフレームワーク上に構築されますが、使用する主要なテキストよりもはるかに簡単です。

詳細についてを参照してください、 [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>マルチタスキング

iOS 7 では、ときに、およびバック グラウンド作業を実行する方法を変更します。 タスクの完了 iOS 7 で不要になった保持アプリケーション起動状態、バック グラウンドでタスクが実行されていると、アプリケーションが連続しない複数の方法で処理するバック グラウンドのウェイク アップします。 iOS 7 には、新しいコンテンツをバック グラウンドでアプリケーションを更新するための 3 つの新しい Api も追加します。

-  バック グラウンドでフェッチ – コンテンツを一定の間隔でバック グラウンドで更新するアプリケーションを許可します。
-  リモート通知 - アプリケーションがプッシュ通知を受信するときにコンテンツを更新します。 通知には、いずれかを指定できるサイレント ロック画面にバナーを表示することもできます。
-  バック グラウンド転送サービス – 固定の時間制限なしの大きなファイルなどのデータのアップロードとダウンロードを許可します。


マルチタスクの新機能の詳細については、xamarin iOS のセクションを参照してください。[バック グラウンド処理のガイド](~/ios/app-fundamentals/backgrounding/index.md)します。

## <a name="summary"></a>まとめ

この記事では、iOS にいくつかの主要な新しい点について説明します。 最初に、ビュー コント ローラーにカスタム遷移を追加する方法を示します。 次に、コレクション ビューは、両方から、ナビゲーション コント ローラー内と対話形式でコレクション ビューの間で遷移を使用する方法を示します。 次に、アプリケーションが以前コア アニメーションに対して直接プログラミングを必要とするものについて UIKit を使用する方法を示す、UIView アニメーションに加えられたいくつかの拡張機能を紹介します。 最後に、リッチ テキストのサポートをテキスト キット フレームワークで利用できると共に、物理運動エンジンを UIKit には、新しい UIKit Dynamics API が導入されました。

## <a name="related-links"></a>関連リンク

- [IOS 7 (サンプル) の概要](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
