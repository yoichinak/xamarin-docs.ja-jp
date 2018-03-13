---
title: "IOS でタッチします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 838a11f078d735759eda1d45a082ccbad51e2779
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="touch-in-ios"></a>IOS でタッチします。

中心となるデバイスとのすべての物理操作はそのタッチ イベントを理解し、iOS アプリケーションで Api をタッチする重要なは。 タッチのすべての操作を伴う、`UITouch`オブジェクト。 この記事を使用する方法を学習して、`UITouch`クラスとその Api touch をサポートするためにします。 後で、ジェスチャをサポートする方法についての知識をおが展開されます。

## <a name="enabling-touch"></a>タッチを有効にします。

コントロールの`UIKit`– これらサブクラス化された < から – は UIKit に組み込まれているジェスチャを持っているユーザーとの対話には依存、したがってタッチを有効にする必要はありません。 これは既に有効です。

ただし、多くのビューの`UIKit`タッチが既定で有効になっている必要はありません。 コントロールでのタッチを有効にする 2 つの方法ができます。 最初の方法は、次のスクリーン ショットに示すように、iOS デザイナーのプロパティ パッドでユーザーの対話を有効にチェック ボックスをオンには。

 [![](touch-in-ios-images/image1.png "チェック ボックスをユーザーの対話を有効に iOS デザイナーのプロパティ パッド")](touch-in-ios-images/image1.png#lightbox)

コント ローラー設定にも使用できます、`UserInteractionEnabled`プロパティは true を`UIView`クラスです。 コードで UI を作成する場合に必要です。

次のコード行は、例を示します。

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>タッチ イベント

ユーザーが画面に触れる、指を移動または指を削除するときに発生するタッチの 3 つのフェーズがあります。 これらのメソッドが定義されている`UIResponder`UIView の基底クラスであります。 iOS に関連付けられているメソッドによってオーバーライドされます、`UIView`と`UIViewController`タッチを処理します。

-  `TouchesBegan` – これは、画面が最初に操作されたときに呼び出されます。
-  `TouchesMoved` – これは、タッチの変更の場所として、ユーザーが画面の周りには、その指をスライドさせてときに呼び出されます。
-  `TouchesEnded` または`TouchesCancelled`–`TouchesEnded`画面の外、ユーザーの指が解除されたときに呼び出されます。  `TouchesCancelled` ユーザーが、キーを押してをキャンセルするためのボタンから自分の指をスライドする場合、たとえば、iOS にタッチ – が取り消された場合に呼び出されます。


タッチ イベント旅行を再帰的にタッチ イベントをビュー オブジェクトの境界内でチェックする、UIViews のスタックを経由します。 これは多くの場合と呼ばれる_ヒット テスト_です。 最上位で最初に呼び出されます`UIView`または`UIViewController`しで呼び出されると、`UIView`と`UIViewControllers`階層の表示でそれらの下。

A`UITouch`オブジェクトは、ユーザーが画面に触れるたびに作成されます。 `UITouch`オブジェクトには、タッチが発生したとき、発生した場所の読み取りなど、タッチがあった場合など、タッチに関するデータが含まれています。タッチ イベント渡さ仕上げプロパティ –、`NSSet`を含む 1 つまたは複数の調整します。 このプロパティを使用して、タッチへの参照を取得するアプリケーションの応答を決定したりできます。

タッチ イベントの 1 つをオーバーライドするクラス必要があります最初に基本実装を呼び出すし、取得、`UITouch`イベントに関連付けられているオブジェクト。 最初のタッチへの参照を取得する呼び出し、`AnyObject`プロパティとしてキャストし、`UITouch`次の例で示すように。

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

連続するクイックが画面に触れるし、すべてを 1 つの 1 回のタップとしてまとめることが自動的に認識 iOS`UITouch`オブジェクト。 これにより、ダブルタップ簡単にチェックのチェック、`TapCount`プロパティ、次のコードに示すようにします。

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

マルチタッチには、コントロールでは既定では無効です。 マルチタッチは、次のスクリーン ショットに示すように、iOS、デザイナーで有効にすることができます。

 [![](touch-in-ios-images/image2.png "IOS デザイナーで有効になっているマルチタッチ")](touch-in-ios-images/image2.png#lightbox)

設定して、マルチタッチをコードから設定することも、`MultipleTouchEnabled`プロパティの次のコード行に示すようにします。

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

数の本の指タッチ画面を確認するには`Count`プロパティを`UITouch`プロパティ。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>タッチの場所を決定します。

メソッド`UITouch.LocationInView`特定のビュー内でタッチの座標を保持する CGPoint オブジェクトを返します。 さらに、メソッドを呼び出して、その場所が、コントロール内かどうかをテストできます`Frame.Contains`です。 次のコード スニペットは、この例を示しています。

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

タッチ イベントの理解、iOS であるは、みましょうジェスチャ レコグナイザーについて説明します。

## <a name="gesture-recognizers"></a>ジェスチャ レコグナイザー

ジェスチャ レコグナイザーは大幅に簡略化し、アプリケーションで touch をサポートするプログラミングの手間をできるだけ省きます。 iOS ジェスチャ レコグナイザーを 1 つのタッチ イベントに、一連のタッチ イベントを集計します。

クラスを提供する Xamarin.iOS`UIGestureRecognizer`次の組み込みのジェスチャ レコグナイザーの基本クラスとして。

-  *UITapGesturesRecognizer* – これは、1 つまたは複数のタップします。
-  *UIPinchGestureRecognizer* – Pinching と離れて指を展開します。
-  *UIPanGestureRecognizer* – パンまたはドラッグします。
-  *UISwipeGestureRecognizer* – 任意の方向にスワイプします。
-  *UIRotationGestureRecognizer* – 時計回りまたは反時計回りを描くように 2 本の指を回転します。
-  *UILongPressGestureRecognizer* – プレス アンド ホールド、時間の長いキーを押してまたは時間の長いクリックとも呼ばします。


ジェスチャ レコグナイザーを使用する基本的なパターンは次のとおりです。

1.  **ジェスチャ レコグナイザーをインスタンス化**: 最初に、インスタンス化、`UIGestureRecognizer`サブクラスです。 インスタンス化されるオブジェクトはビューによって関連付けられているされガベージが収集されますのビューが破棄されるときにします。 クラス レベルの変数としては、このビューを作成する必要はありません。
1.  **すべてのジェスチャの設定を構成する**– ジェスチャ レコグナイザーを構成するのには、次の手順です。 Xamarin のドキュメントを参照してください`UIGestureRecognizer`とそのサブクラスの動作を制御する設定できるプロパティの一覧については、`UIGestureRecognizer`インスタンス。
1.  **ターゲットを構成する**– Xamarin.iOS、OBJECTIVE-C ヘリテージのためのジェスチャ レコグナイザー ジェスチャに一致する場合のイベントが発生しません。  `UIGestureRecognizer` – メソッドを持つ`AddTarget`– ジェスチャ レコグナイザーは、一致するときに実行するには、匿名デリゲートまたはコードで、OBJECTIVE-C セレクターを受け入れることができます。
1.  **ジェスチャ レコグナイザーを有効にする**– タッチ イベントとジェスチャのみ認識されますタッチの相互作用が有効になっている場合と同様にします。
1.  **ジェスチャ レコグナイザーをビューに追加**– 最後の手順が呼び出すことによって、ビューにジェスチャを追加するには`View.AddGestureRecognizer`、ジェスチャ認識エンジン オブジェクトを渡すこととします。

参照してください、[ジェスチャ レコグナイザー サンプル](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples)コードでそれらを実装する方法の詳細。

ジェスチャのターゲットが呼び出されたときに発生したジェスチャへの参照を渡されます。 これにより、ジェスチャ ターゲットが発生したジェスチャに関する情報を取得します。 使用可能な情報の範囲は、使用されたジェスチャ認識エンジンの種類によって異なります。 データについては、使用可能な各 Xamarin のドキュメントを参照してください`UIGestureRecognizer`サブクラスです。

ジェスチャ レコグナイザーはビューに追加されると、ビュー (とその下のすべてのビュー) が受信しないこと、タッチ イベントの保存に重要です。 同時に、ジェスチャ、タッチ イベントを許可する、`CancelsTouchesInView`プロパティは、次のコードに示すように false に設定する必要があります。

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

各`UIGestureRecognizer`は、ジェスチャ認識エンジンの状態に関する重要な情報を提供する状態プロパティがあります。 このプロパティの値が変更されるたびに、iOS は、更新プログラムを提供サブスクライブを行うメソッドを呼び出します。 場合は、カスタム ジェスチャ レコグナイザーは、State プロパティを更新しない、サブスクライバーは決して呼び出さ、レンダリング ジェスチャ レコグナイザーが役に立ちません。

ジェスチャは、次の 2 種類の 1 つとして要約できます。

1.  *不連続*– これらジェスチャ火災、最初にのみ認識されます。
1.  *継続的な*– これらのジェスチャの続行を認識する限りを起動します。


ジェスチャ レコグナイザーは、次の状態のいずれかで存在する場合します。

-  *考えられる*– これは、すべてのジェスチャ レコグナイザーの初期状態です。 これは、State プロパティの既定値です。
-  *開始*– 継続的なジェスチャが最初に認識されたとき、状態は開始に設定します。 これにより、ジェスチャ認識を開始して、変更されたときに区別するために定期受信します。
-  *変更された*– 継続的なジェスチャが開始されましたが、終了していない後の状態に設定されます Changed タッチの移動や、変更するたびにまだジェスチャの予想されるパラメーター内に収まる限り、します。
-  *取り消された*– レコグナイザー通っていた開始から変更された、および不要になったとするように変更されたタッチ ジェスチャのパターンに合わせて、場合、この状態は設定されます。
-  *認識*– ジェスチャ レコグナイザー仕上げのセットと一致して、ジェスチャが完了したこと、サブスクライバーに通知されるときの状態に設定されます。
-  *終了*– これは、認識の状態の別名です。
-  *失敗*– ジェスチャ レコグナイザーが一致しなくなります、状態が失敗に変更の待機中の調整とします。


Xamarin.iOS でこれらの値を表す、`UIGestureRecognizerState`列挙します。

## <a name="working-with-multiple-gestures"></a>複数のジェスチャの使用

既定では、iOS に同時に実行する既定のジェスチャはできません。 代わりに、各ジェスチャ レコグナイザーは非確定的な順序でタッチ イベントを受信します。 次のコード スニペットでは、ジェスチャ認識エンジンが同時に実行する方法について説明します。

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

IOS でジェスチャを無効にすることもできます。 方法の決定およびジェスチャを認識する必要があるかどうかに、アプリケーションと現在のタッチ イベントの状態を調べるジェスチャ レコグナイザーを許可する 2 つのデリゲート プロパティがあります。 2 つのイベントは次のとおりです。

1.  *ShouldReceiveTouch* – ジェスチャ認識エンジンが、タッチ イベントが渡され、タッチを確認し、ジェスチャ認識エンジンによって処理されるどの仕上げを決定する機会を提供する前にすぐにこのデリゲートが呼び出されます。
1.  *ShouldBegin* – 認識エンジンが、他の状態可能なから状態を変更しようとしています。 ときに呼び出されます。 False を返すことと、失敗に変更する、ジェスチャ認識エンジンの状態が実行されます。


厳密に型指定でこれらのメソッドをオーバーライドする`UIGestureRecognizerDelegate`、脆弱なデリゲート、または次のコード スニペットに示すように、イベント ハンドラー構文を使用してバインドします。

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

最後に、可能であれば別のジェスチャ レコグナイザーが失敗した場合にのみ成功ようにジェスチャ レコグナイザーをキューに登録します。 たとえば、1 回のタップ ジェスチャ レコグナイザーは必要がありますダブルタップ ジェスチャ レコグナイザーが失敗した場合にのみ成功します。 次のコード スニペットは、この例を示します。

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>カスタム ジェスチャを作成します。

IOS は、ジェスチャ レコグナイザーをいくつかの既定値に提供しますが、場合によってはカスタム ジェスチャ レコグナイザーを作成する必要があります。 カスタムのジェスチャ レコグナイザーを作成するには、次の手順が含まれます。

1.  サブクラス`UIGestureRecognizer`です。
1.  適切なタッチ イベント メソッドをオーバーライドします。
1.  基底クラスの状態プロパティ経由で認識ステータス上方向に通知します。


これの実用的な例については、説明、 [iOS でタッチを使用して](ios-touch-walkthrough.md)チュートリアルです。
