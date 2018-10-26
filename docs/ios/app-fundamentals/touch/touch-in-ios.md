---
title: タッチ イベントと Xamarin.iOS でジェスチャ
description: このドキュメントでは、タッチ イベント、マルチタッチ、ジェスチャ、複数のジェスチャ、および Xamarin.iOS アプリケーションでカスタム ジェスチャを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f7160c48e1b1ac85f4aa0173c0eb9f42b8fefca2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114770"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>タッチ イベントと Xamarin.iOS でジェスチャ

デバイスとのすべての物理的な対話の中心となる、タッチ イベントを理解し、iOS アプリケーションで Api のタッチに重要です。 すべてのタッチ操作が含まれる、`UITouch`オブジェクト。 この記事を使用する方法をしますが、`UITouch`クラスとタッチをサポートするためには、その Api。 後で、ジェスチャがサポートする方法について知識を発展します。

## <a name="enabling-touch"></a>タッチを有効にします。

コントロールで`UIKit`– これらサブクラス化された < から – UIKit に組み込まれているジェスチャを持っているユーザーの操作のため依存しているし、したがってタッチを有効にする必要はありません。 既に有効です。

ただし、多くのビューの`UIKit`は既定で有効になっているタッチではありません。 コントロールにタッチを有効にする 2 つの方法はあります。 最初の方法は、次のスクリーン ショットに示すように、iOS デザイナーのプロパティ パッドでユーザーの相互作用有効になっているチェック ボックスをオンには。

 [![](touch-in-ios-images/image1.png "IOS Designer の [プロパティ] パッドでユーザーの対話を有効にチェック ボックスをオンします。")](touch-in-ios-images/image1.png#lightbox)

コント ローラーを使用して設定します、`UserInteractionEnabled`プロパティを true に、`UIView`クラス。 コードで、UI が作成された場合に必要です。

次のコード行では、例を示します。

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>タッチ イベント

ユーザーが画面をタッチ、指を移動または指を削除します。 ときに発生するタッチの 3 つのフェーズがあります。 これらのメソッドが定義されている`UIResponder`UIView の基本クラスであります。 iOS はで、関連付けられたメソッドをオーバーライド、 `UIView` 、`UIViewController`タッチを処理するために。

-  `TouchesBegan` – これは、画面が最初に操作されたときに呼び出されます。
-  `TouchesMoved` – これは、タッチの変更の場所として、ユーザーが画面に指をスライドするときに呼び出されます。
-  `TouchesEnded` または`TouchesCancelled`–`TouchesEnded`ユーザーの本の指が画面から無効になるときに呼び出されます。  `TouchesCancelled` ユーザーは、キーを押してをキャンセルするためのボタンから自分の指をスライドしている場合、iOS など、タッチ – を取り消した場合に呼び出されます。


タッチ イベント旅行を再帰的に UIViews、タッチ イベントをビュー オブジェクトの境界内でチェックするのスタックを経由。 これは多くの場合に呼び出されます_ヒット テスト_します。 最上位で呼び出す最初`UIView`または`UIViewController`しで呼び出されると、`UIView`と`UIViewControllers`ビュー階層内でその下。

A`UITouch`オブジェクトは、ユーザーが画面をタッチするたびに作成されます。 `UITouch`オブジェクトには、タッチが発生したとき、発生した場所へスワイプしなど、タッチがあった場合など、タッチに関するデータが含まれています。タッチ イベントのしあげプロパティ – 渡された、 `NSSet` 1 つまたは複数のしあげを格納しています。 このプロパティを使用して、タッチへの参照を取得してアプリケーションの対応を判断します。

タッチ イベントの 1 つをオーバーライドするクラスが最初に基本の実装を呼び出す必要があり、取得、`UITouch`イベントに関連付けられているオブジェクト。 最初のタッチへの参照を取得する呼び出し、`AnyObject`プロパティとしてキャスト、`UITouch`次の例に示すように。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS は、連続するクイックが画面に触れるしはすべて、1 つで 1 回のタップとして収集に自動的に認識`UITouch`オブジェクト。 ダブルタップ簡単にチェックを確認します。 これにより、`TapCount`プロパティは、次のコードに示すようにします。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>マルチタッチ

コントロールに既定では、マルチタッチが有効になっていません。 次のスクリーン ショットに示すように、iOS Designer でのマルチ タッチを有効にできます。

 [![](touch-in-ios-images/image2.png "IOS Designer で有効になっているマルチタッチ")](touch-in-ios-images/image2.png#lightbox)

マルチタッチを設定してプログラムで設定することも、`MultipleTouchEnabled`プロパティを次のコード行で示すようにします。

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

何本の指タッチ画面を調べるには、`Count`プロパティを`UITouch`プロパティ。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>タッチの場所を決定します。

メソッド`UITouch.LocationInView`指定されたビュー内でのタッチの座標を保持する CGPoint オブジェクトを返します。 さらに、メソッドを呼び出してその場所が、コントロール内でかどうかをテストできる`Frame.Contains`します。 次のコード スニペットでは、この例を示します。

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

IOS でのタッチ イベントについて理解したらは、みましょうジェスチャ レコグナイザーについて説明します。

## <a name="gesture-recognizers"></a>ジェスチャ レコグナイザー

ジェスチャ レコグナイザーは大幅に簡略化し、アプリケーションでタッチをサポートするプログラミング作業を軽減します。 iOS ジェスチャ レコグナイザーは、1 つのタッチ イベントに、一連のタッチ イベントを集計します。

Xamarin.iOS は、クラスを提供します。 `UIGestureRecognizer` 、次の組み込みのジェスチャ認識の基本クラスとして。

-  *UITapGestureRecognizer* – これは、1 つまたは複数回のタップします。
-  *UIPinchGestureRecognizer* – Pinching と分解本の指を展開します。
-  *UIPanGestureRecognizer* – パンまたはドラッグします。
-  *UISwipeGestureRecognizer* – 任意の方向にスワイプします。
-  *UIRotationGestureRecognizer* – 時計回りまたは反時計回りに描くように 2 本の指を回転します。
-  *UILongPressGestureRecognizer* – プレス アンド ホールド、時間の長い press または時間の長いクリックとも呼ばします。


ジェスチャ レコグナイザーを使用する基本的なパターンは次のとおりです。

1.  **ジェスチャ認識エンジンをインスタンス化**– 最初に、インスタンス化、`UIGestureRecognizer`サブクラスです。 ビューでインスタンス化されるオブジェクトを関連付けるされガベージが収集されますのビューが破棄されたときにします。 クラス レベルの変数としては、このビューを作成する必要はありません。
1.  **ジェスチャ設定を構成**– 次の手順ではジェスチャ レコグナイザーを構成します。 上の Xamarin のドキュメントを参照してください`UIGestureRecognizer`とそのサブクラスの動作を制御する設定できるプロパティの一覧については、`UIGestureRecognizer`インスタンス。
1.  **ターゲット構成**– Xamarin.iOS により、OBJECTIVE-C 遺産ジェスチャ レコグナイザーがジェスチャと一致する場合にイベントを発生させるしません。  `UIGestureRecognizer` – メソッドを持つ`AddTarget`– ジェスチャ レコグナイザーは、一致するときに実行するには、匿名デリゲートまたは、OBJECTIVE-C セレクターのコードをそのまま使用することができます。
1.  **ジェスチャ レコグナイザーを有効にする**– タッチ イベントとジェスチャだけ認識されますタッチ操作が有効になっている場合と同様にだけです。
1.  **ジェスチャ レコグナイザーをビューに追加**– 最後の手順が呼び出すことによって、ビューに、ジェスチャを追加するには`View.AddGestureRecognizer`、ジェスチャ認識エンジン オブジェクトを渡すとします。

参照してください、[ジェスチャ認識エンジン サンプル](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples)それらをコードに実装する方法の詳細について。

ジェスチャのターゲットが呼び出されたときに、発生したジェスチャへの参照が渡されます。 これにより、発生したジェスチャに関する情報を取得するジェスチャのターゲットです。 使用可能な情報の範囲は、サポートされているジェスチャ レコグナイザーの種類によって異なります。 それぞれの使用可能なデータについては、Xamarin のドキュメントを参照してください`UIGestureRecognizer`サブクラスです。

ジェスチャ レコグナイザーはビューに追加されると、ビュー (とその下のすべてのビュー) がない受信する、タッチ イベントに注意してください。 同時に、ジェスチャ、タッチ イベントを許可する、`CancelsTouchesInView`プロパティは、次のコードに示すように、false に設定する必要があります。

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

各`UIGestureRecognizer`ジェスチャ認識エンジンの状態に関する重要な情報を提供する状態プロパティがあります。 このプロパティの値が変更されるたびに、iOS は、更新プログラムを付けますサブスクライブ メソッドを呼び出します。 カスタム ジェスチャ認識エンジンは、決して State プロパティを更新する場合、サブスクライバーが呼び出されず、役に立たないジェスチャ レコグナイザーをレンダリングします。

2 つの型の 1 つのジェスチャを要約できます。

1.  *不連続*– これらのジェスチャだけ火災最初の時間認識されます。
1.  *継続的な*– これらのジェスチャを続行させるとして認識されます。


ジェスチャ レコグナイザーは、次の状態のいずれかに存在します。

-  *考えられる*– これはすべてのジェスチャ レコグナイザーの初期状態です。 これは、State プロパティの既定値です。
-  *開始*– 継続的なジェスチャが最初に認識された場合、状態が開始に設定されています。 これにより、ジェスチャ認識の開始時と変更されたときの区別をサブスクライブします。
-  *Changed* : 継続的なジェスチャが開始されましたが、終了していない、状態が設定されます Changed にタッチが移動または変更するたびにジェスチャの必要なパラメーターも内にある限り、します。
-  *取り消された*– 認識エンジンは、変更を開始から問題が発生しました、できなくするとしてこのような方法で変更するタッチ ジェスチャのパターンに合わせてし場合この状態が設定されます。
-  *認識*– ジェスチャ レコグナイザー仕上げのセットに一致し、ジェスチャが完了したこと、サブスクライバーが通知されますと、状態が設定されます。
-  *終了*– これは、認識の状態の別名。
-  *失敗*– ジェスチャ レコグナイザーできますが一致しなくなったタッチをリッスンしているため、状態が失敗に変更するとします。


Xamarin.iOS でこれらの値を表す、`UIGestureRecognizerState`列挙体。

## <a name="working-with-multiple-gestures"></a>複数のジェスチャの使用

既定では、iOS に同時に実行する既定のジェスチャはできません。 代わりに、各ジェスチャ レコグナイザーは非確定的な順序でタッチ イベントを受信します。 次のコード スニペットでは、ジェスチャ レコグナイザーが同時に実行する方法について説明します。

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

IOS でジェスチャを無効にすることもできます。 方法の決定とジェスチャを認識する必要があるかどうかに、アプリケーションと、現在のタッチ イベントの状態を調査するジェスチャ レコグナイザーを許可する 2 つのデリゲート プロパティがあります。 2 つのイベントは次のとおりです。

1.  *ShouldReceiveTouch* – ジェスチャ レコグナイザー、タッチ イベントが渡され、タッチを調べ、どのタッチ ジェスチャ認識エンジンによって処理されるかを決定する機会を提供する前にすぐにこのデリゲートが呼び出されます。
1.  *ShouldBegin* – 認識エンジンが状態可能なからその他の状態に変更を試みるときに呼び出されます。 False を返すことで、失敗に変更するジェスチャ認識エンジンの状態を強制します。


厳密に型指定されたこれらのメソッドをオーバーライドできます`UIGestureRecognizerDelegate`、脆弱なデリゲート、または次のコード スニペットに示すように、イベント ハンドラー構文を使用してバインドします。

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

最後に、もう 1 つのジェスチャ認識エンジンが失敗した場合にのみ成功するために、ジェスチャ レコグナイザーをキューになります。 たとえば、1 回のタップ ジェスチャ認識エンジンはする必要がありますダブルタップ ジェスチャ認識エンジンが失敗した場合にのみ成功します。 次のコード スニペットでは、この例を示します。

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>カスタム ジェスチャを作成します。

IOS ではいくつかの既定値、ジェスチャ レコグナイザーは、場合によってはカスタム ジェスチャ レコグナイザーを作成する必要があります。 カスタムのジェスチャ認識エンジンを作成するには、次の手順が含まれます。

1.  サブクラス`UIGestureRecognizer`します。
1.  適切なタッチ イベントのメソッドをオーバーライドします。
1.  基底クラスの状態プロパティを使用して状態を認識向かいます (バブル)。


この実用的な例では説明、 [iOS でのタッチを使用して](ios-touch-walkthrough.md)チュートリアル。
