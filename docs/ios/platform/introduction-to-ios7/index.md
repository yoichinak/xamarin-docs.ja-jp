---
title: IOS 7 の概要
description: この記事では、iOS ビュー コント ローラーの遷移、UIView アニメーション、UIKit Dynamics とテキストのキットの機能強化を含め、7 で導入された主要な新しい Api について説明します。 また、一部のユーザー インターフェイスと、新しい強化マルチタスク機能への変更について説明します。
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9ae82eba78f099f675d21bf53a250923630a0ff6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-7"></a>IOS 7 の概要

_この記事では、iOS ビュー コント ローラーの遷移、UIView アニメーション、UIKit Dynamics とテキストのキットの機能強化を含め、7 で導入された主要な新しい Api について説明します。また、一部のユーザー インターフェイスと、新しい強化マルチタスク機能への変更について説明します。_

iOS 7 は、iOS に大きな更新です。 アプリケーションではなくコンテンツのクロムでのフォーカスの設定を完全に新しいユーザー インターフェイスのデザインが導入されています。 ビジュアルの変更とは、iOS 7 は、多種多様な高度な相互作用とエクスペリエンスを作成する新しい Api を追加します。 このドキュメントの調査、新しいテクノロジでは、iOS 7 で導入され、さらに調査のための開始点として機能します。

## <a name="uiview-animation-enhancements"></a>UIView アニメーションの機能強化

iOS 7 では、アプリケーション、アニメーションのコア フレームワークに直接ドロップする必要があったことを許可し、UIKit でアニメーションのサポートが強化されます。 たとえば、`UIView`跳ねるアニメーションのキーフレーム アニメーションなどを実行できるようになりましたが以前、`CAKeyframeAnimation`に適用される、`CALayer`です。

### <a name="spring-animations"></a>跳ねるアニメーション

 `UIView` spring 効果でアニメーションのプロパティの変更をサポートしています。 これを追加するには、いずれかを呼び出す、`AnimateNotify`または`AnimateNotifyAsync`以下に示すよう spring 減衰比率と spring の初期ベロシティ値に渡して、メソッド。

-  `springWithDampingRatio` – 0 ~ 1 の場合、振幅が小さい方の値を増加値。
-  `initialSpringVelocity` – 1 秒あたりのアニメーションの総距離の割合として初期 spring 速度。


次のコードでは、イメージのビューの中心が変更されたときに、spring 効果が生成されます。

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

この spring 効果により、イメージ ビューを以下に示すように、新しい中央の場所に、アニメーションを完了したときをバウンドを表示します。

 ![](images/spring-animation.png "この spring 効果でバウンド センターの新しい場所に、アニメーションを完了したときに表示するイメージのビューは、します。")

### <a name="keyframe-animations"></a>キーフレーム アニメーション

`UIView`クラスが含まれています、`AnimateWithKeyframes`のキーフレーム アニメーションを作成する方法、`UIView`です。 このメソッドは互いに似ています`UIView`いる点を除き、アニメーション メソッドでは、追加の`NSAction`にキーフレームを含めるパラメーターとして渡されます。 内で、 `NSAction`、キーフレームが呼び出しによって追加された`UIView.AddKeyframeWithRelativeStartTime`です。

たとえば、次のコード スニペットでは、ビューを回転させるか、ビューの中心にもをアニメーション化するキーフレーム アニメーションを作成します。

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

最初の 2 つのパラメーター、`AddKeyframeWithRelativeStartTime`メソッド、開始時刻と期間を指定、キーフレームのそれぞれに、アニメーションの全体的な長さの割合として。 次の秒に 90 度回転させると、イメージ ビューをアニメーション化する新しいセンターに対する最初の 2 番目では、上での結果上の例の後にします。 アニメーションを示すため`UIViewKeyframeAnimationOptions.Autoreverse`も逆の順序でオプションとして、両方のキーフレームをアニメーション化します。 最後に、最終的な値は、完了ハンドラーに初期状態に設定されます。

次のスクリーン ショットでは、結合されたアニメーション キーフレームを示しています。

 ![](images/keyframes.png "このスクリーン ショットは、結合されたアニメーション キーフレームを通じてを示しています。")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit UIKit Api を使用する物理的な特性に基づくアニメーションの相互作用を作成するアプリケーションの新しいセットです。 UIKit Dynamics には、これを可能 2D 物理エンジンがカプセル化します。

API は、本質的に宣言します。 呼ばれる、オブジェクトを作成することで、物理操作の動作を宣言する*動作*- 重力、競合、springs などの高速物理概念です。呼ばれる別のオブジェクトに、問題をアタッチしてから、*動的アニメーター*ビューをカプセル化します。 動的アニメーターが宣言されている物理ビヘイビアーを適用する必要を受け取る*動的アイテム*-項目を実装する`IUIDynamicItem`など、`UIView`です。

これにはいくつか異なるプリミティブの動作を含む、複雑な相互作用をトリガーに使用できるがあります。

-  `UIAttachmentBehavior` –、まとめて移動するような動的な 2 つの項目を付加したり、接続点に動的な項目をアタッチします。
-  `UICollisionBehavior` – は、衝突に参加する動的項目。
-  `UIDynamicItemBehavior` – 一般的な柔軟性、密度摩擦などの動的アイテムに適用するプロパティのセットを指定します。
-  `UIGravityBehavior` -重力方向に高速化する項目を原因と、動的な項目を重力を適用します。
-  `UIPushBehavior` – Force を動的項目に適用されます。
-  `UISnapBehavior` – Spring 効果のある位置にスナップする動的な項目をできます。


多くのプリミティブはありますが、物理ベースの相互作用を UIKit Dynamics を使用してビューに追加するための一般的なプロセス間で一貫性が動作します。

1.  動的アニメーターを作成します。
1.  問題を作成します。
1.  動的アニメーターにビヘイビアーを追加します。


### <a name="dynamics-example"></a>Dynamics 例

重力や競合の境界を追加する例を見てみましょう、`UIView`です。

#### <a name="uigravitybehavior"></a>UIGravityBehavior

重力をイメージのビューに追加する、上記で説明した 3 つの手順に従います。

操作を行います。、`ViewDidLoad`この例のメソッドです。 最初に、追加、`UIImageView`インスタンスの次のようにします。

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

これにより、画面の上端を中心とイメージ ビューが作成されます。 インスタンスを作成するのには、イメージ「秋」重力で、 `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

`UIDynamicAnimator`の参照のインスタンスを受け取り`UIView`または`UICollectionViewLayout`、接続の問題あたりアニメーション化されるアイテムが含まれています。

次に、作成、`UIGravityBehavior`インスタンス。 実装する 1 つまたは複数のオブジェクトを渡すことができます、`IUIDynamicItem`同様に、 `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

動作の配列に渡されます`IUIDynamicItem`、ここでは、1 つを含む`UIImageView`アニメーションおインスタンス。

最後に、動的アニメーターに動作を追加します。

```csharp
dynAnimator.AddBehavior (gravity);
```

これは、結果重力、以下を参照として使用下方向にアニメーション化イメージになります。

![](images/gravity2.png "イメージの開始場所") 
![](images/gravity3.png "終了のイメージの場所")

ないので、画面の境界を制限する、イメージのビューだけで外れる下部です。 イメージが、画面の端と競合するように、ビューを制限に追加できる、`UICollisionBehavior`です。 これは、次のセクションで説明します。

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

まず、作成、`UICollisionBehavior`とまったく同じよう、動的アニメーターへの追加、`UIGravityBehavior`です。

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

`UICollisionBehavior`というプロパティを持つ`TranslatesReferenceBoundsIntoBoundry`します。 これを設定する`true`衝突境界として使用するビューの境界、参照が発生します。

ここで、イメージは、重力と下方向にアニメーション化、ときに跳ね返ります若干画面の下部からある rest 定着する前にします。

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

さらに落とすのイメージ ビューの他の動作での動作を制御おことができます。 たとえば、追加でした、`UIDynamicItemBehavior`と競合して画面の下部にあるときに詳細跳ね返りますイメージのビューの原因と、柔軟性を向上させる。

追加する、`UIDynamicItemBehavior`他の動作と同じ手順に従います。 最初の動作を作成します。

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

次に、動的アニメーターに動作を追加します。

 `dynAnimator.AddBehavior (dynBehavior);`

この動作で、境界と競合しているときに複数イメージ ビューを bounces です。

## <a name="general-user-interface-changes"></a>一般的なユーザー インターフェイスの変更

だけでなく、新しい UIKit Api UIKit Dynamics、コント ローラーの遷移、および強化された UIView アニメーション上記など、iOS 7 では、さまざまな UI を視覚的な変更やさまざまなビューとコントロールに関連する API の変更が導入されています。 詳細については、次を参照してください。、 [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)です。

## <a name="text-kit"></a>テキスト キット

テキスト キットは、強力なテキストのレイアウトとレンダリングの機能を提供する新しい API です。 低レベルの中核となるテキストのフレームワーク上に構築されますが、中核となるテキストよりも使用する方が簡単です。

詳細についてを参照してください、 [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>マルチタスキング

iOS 7 では、ときに、およびバック グラウンド処理を実行する方法を変更します。 タスクの完了 iOS 7 で不要になったによりアプリケーション起動場合、バック グラウンドでタスクが実行されていると、アプリケーションが連続していない方法で処理するバック グラウンドのウェイク アップされます。 iOS 7 には、バック グラウンドで新しいコンテンツを含むアプリケーションを更新するための 3 つの新しい Api も追加します。

-  バック グラウンドでフェッチ – コンテンツを一定の間隔でバック グラウンドで更新するアプリケーションが可能になります。
-  リモートの通知 - により、アプリケーションは、プッシュ通知を受信するときにコンテンツを更新します。 通知には、いずれかを指定できるサイレント ロック画面で、バナーを表示することもできます。
-  バック グラウンド転送サービス – 固定時間制限なしの大きなファイルなどのデータのアップロードやダウンロードが可能になります。


マルチタスクの新機能に関する詳細については、Xamarin の iOS セクションを参照してください。 [Backgrounding ガイド](~/ios/app-fundamentals/backgrounding/index.md)です。

## <a name="summary"></a>まとめ

この記事では、iOS にいくつかの主要な新しい点について説明します。 最初に、コント ローラーの表示にカスタム遷移を追加する方法を示します。 次に、両方のナビゲーションのコント ローラー内で対話的に間コレクション ビューとコレクション ビュー内の遷移が使用する方法を示します。 次に、UIView アニメーションでは、アプリケーションがコア アニメーションに対して直接プログラムを必要とした点の UIKit を使用する方法を表示するいくつかの拡張機能を紹介します。 最後に、テキスト キット フレームワークで使用できるようになりましたリッチ テキストのサポートと共に UIKit に物理エンジンを表示、新しい UIKit Dynamics API が導入されました。

## <a name="related-links"></a>関連リンク

- [IOS 7 (サンプル) の概要](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 ユーザー インターフェイスの概要](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)
