---
title: Xamarin のタッチイベントとジェスチャ (iOS)
description: このドキュメントでは、Xamarin iOS アプリケーションでタッチイベント、マルチタッチ、ジェスチャ、複数のジェスチャ、およびカスタムジェスチャを操作する方法について説明します。
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 885e5862e517303b4a2556b0b1bd3fa8759bbca6
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940895"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>Xamarin のタッチイベントとジェスチャ (iOS)

IOS アプリケーションのタッチイベントとタッチ Api は、デバイスとのすべての物理的なやり取りの中心となるため、理解しておくことが重要です。 すべてのタッチ操作には、`UITouch` オブジェクトが含まれます。 この記事では、`UITouch` クラスとその Api を使用してタッチをサポートする方法について説明します。 ジェスチャをサポートする方法については、後で詳しく説明します。

## <a name="enabling-touch"></a>タッチの有効化

`UIKit` のコントロール (UIControl からサブクラス) は、UIKit に組み込まれているジェスチャを持つユーザーの操作に依存しているため、タッチを有効にする必要はありません。 既に有効になっています。

ただし、`UIKit` のビューの多くは、既定ではタッチが有効になっていません。 コントロールでタッチを有効にするには、2つの方法があります。 最初の方法は、次のスクリーンショットに示すように、iOS デザイナーのプロパティパッドで [ユーザーの操作を有効にする] チェックボックスをオンにすることです。

 [![](touch-in-ios-images/image1.png "Check the User Interaction Enabled checkbox in the Property Pad of the iOS Designer")](touch-in-ios-images/image1.png#lightbox)

また、コントローラーを使用して、`UIView` クラスの `UserInteractionEnabled` プロパティを true に設定することもできます。 これは、コードで UI を作成する場合に必要です。

次のコード行は一例です。

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>タッチ イベント

タッチの3つのフェーズがあります。これは、ユーザーが画面に触れるか、指を動かすか、または指を削除すると発生します。 これらのメソッドは `UIResponder`で定義されています。これは、UIView の基本クラスです。 iOS は、`UIView` に関連付けられているメソッドと、タッチを処理するための `UIViewController` を上書きします。

- `TouchesBegan` –画面に最初に触れたときに呼び出されます。
- `TouchesMoved` –ユーザーが画面の周りに指をスライドさせながら、タッチの位置が変化したときに呼び出されます。
- `TouchesEnded` または `TouchesCancelled` –ユーザーの指が画面から離れたときに `TouchesEnded` が呼び出されます。  `TouchesCancelled` は、iOS がタッチをキャンセルした場合に呼び出されます。たとえば、ユーザーがボタンから指を押して、押しをキャンセルした場合などです。

Touch イベントは、UIViews のスタックを介して再帰的に移動し、タッチイベントがビューオブジェクトの境界内にあるかどうかを確認します。 これは、しばしば_ヒットテスト_と呼ばれます。 これらは、最初に最上位の `UIView` または `UIViewController` で呼び出され、次にビュー階層の下の `UIView` と `UIViewControllers` に呼び出されます。

ユーザーが画面に触れるたびに、`UITouch` オブジェクトが作成されます。 `UITouch` オブジェクトには、タッチが発生したとき、どこでタッチしたかなど、タッチに関するデータが含まれます。Touch イベントには、タッチプロパティが渡されます。1つ以上のタッチを含む `NSSet` です。 このプロパティを使用して、タッチへの参照を取得し、アプリケーションの応答を確認できます。

Touch イベントの1つをオーバーライドするクラスは、最初に基本実装を呼び出し、次にイベントに関連付けられている `UITouch` オブジェクトを取得する必要があります。 最初のタッチへの参照を取得するには、`AnyObject` プロパティを呼び出し、次の例に示すように `UITouch` としてキャストします。

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

iOS では、画面上の連続するクイックタッチが自動的に認識され、1つの `UITouch` オブジェクトで1回のタップとして収集されます。 これにより、次のコードに示すように、`TapCount` プロパティを確認するのと同じように、ダブルタップを確認しやすくなります。

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

コントロールでは、マルチタッチは既定では有効になっていません。 次のスクリーンショットに示すように、iOS デザイナーでマルチタッチを有効にすることができます。

 [![](touch-in-ios-images/image2.png "Multi-touch enabled in the iOS Designer")](touch-in-ios-images/image2.png#lightbox)

次のコード行に示すように、`MultipleTouchEnabled` プロパティを設定して、プログラムでマルチタッチを設定することもできます。

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

画面にタッチした指の数を確認するには、`UITouch` プロパティの `Count` プロパティを使用します。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>タッチ位置の決定

メソッド `UITouch.LocationInView` は、特定のビュー内のタッチの座標を保持する CGPoint オブジェクトを返します。 さらに、`Frame.Contains`メソッドを呼び出すことによって、その場所がコントロール内にあるかどうかをテストできます。 次のコードスニペットは、この例を示しています。

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

IOS のタッチイベントについて理解できたので、ジェスチャレコグナイザーについて説明します。

## <a name="gesture-recognizers"></a>ジェスチャレコグナイザー

ジェスチャレコグナイザーを使用すると、アプリケーションでのタッチをサポートするためのプログラミング作業を大幅に簡略化し、減らすことができます。 iOS のジェスチャレコグナイザーは、一連のタッチイベントを1つのタッチイベントに集約します。

Xamarin iOS は、次の組み込みのジェスチャレコグナイザーの基底クラスとしてクラス `UIGestureRecognizer` を提供します。

- *UITapGestureRecognizer* –1つ以上のタップ用です。
- *UIPinchGestureRecognizer* –ピンチと、指を離します。
- *UIPanGestureRecognizer* –パンまたはドラッグします。
- *UISwipeGestureRecognizer* –任意の方向にスワイプします。
- *UIRotationGestureRecognizer* –2本の指を時計回りまたは反時計回りに回転させます。
- *UILongPressGestureRecognizer* –プレスアンドホールド。長押しまたは長いクリックと呼ばれることもあります。

ジェスチャ認識エンジンを使用する基本的なパターンは次のとおりです。

1. **ジェスチャ認識エンジンをインスタンス化**します。最初に、`UIGestureRecognizer` サブクラスをインスタンス化します。 インスタンス化されたオブジェクトはビューによって関連付けられ、ビューが破棄されるときにガベージコレクションされます。 このビューをクラスレベル変数として作成する必要はありません。
1. **ジェスチャ設定を構成**します。次の手順はジェスチャ認識エンジンを構成することです。 `UIGestureRecognizer` インスタンスの動作を制御するために設定できるプロパティの一覧については、`UIGestureRecognizer` とそのサブクラスに関する Xamarin のドキュメントを参照してください。
1. **ターゲットを構成する**–その目的は C の場合、Xamarin はジェスチャの認識エンジンがジェスチャに一致したときにイベントを発生させません。  `UIGestureRecognizer` には、`AddTarget` というメソッドがあります。これは、ジェスチャレコグナイザーが一致するときに実行するコードと共に、匿名デリゲートまたは目的の C セレクターを受け入れることができます。
1. **ジェスチャ認識エンジンを有効にする**: タッチイベントと同様に、タッチ操作が有効になっている場合にのみジェスチャが認識されます。
1. **ジェスチャ認識エンジンをビューに追加**する–最後の手順は、`View.AddGestureRecognizer` を呼び出し、それにジェスチャ認識エンジンオブジェクトを渡すことによって、そのジェスチャをビューに追加することです。

コードで実装する方法の詳細については、「[ジェスチャレコグナイザーのサンプル](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples)」を参照してください。

ジェスチャのターゲットが呼び出されると、発生したジェスチャへの参照が渡されます。 これにより、ジェスチャターゲットは、発生したジェスチャに関する情報を取得できます。 使用できる情報の範囲は、使用されたジェスチャ認識エンジンの種類によって異なります。 各 `UIGestureRecognizer` サブクラスで利用可能なデータの詳細については、Xamarin のドキュメントを参照してください。

ジェスチャ認識エンジンがビューに追加されると、ビュー (およびその下のすべてのビュー) がタッチイベントを受信しなくなることに注意してください。 ジェスチャでタッチイベントを同時に実行できるようにするには、次のコードに示すように、`CancelsTouchesInView` プロパティを false に設定する必要があります。

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

各 `UIGestureRecognizer` には、ジェスチャ認識エンジンの状態に関する重要な情報を提供する State プロパティがあります。 このプロパティの値が変更されるたびに、iOS はサブスクライブメソッドを呼び出して更新を提供します。 カスタムジェスチャ認識エンジンが State プロパティを更新しない場合、サブスクライバーは呼び出されず、ジェスチャ認識エンジンが不要になります。

ジェスチャは、次の2種類のいずれかとしてまとめることができます。

1. *不連続*-これらのジェスチャは、初めて認識されたときにのみ起動します。
1. *Continuous* –これらのジェスチャは、認識されている限り、引き続き起動します。

ジェスチャレコグナイザーは、次のいずれかの状態にあります。

- *可能*: すべてのジェスチャレコグナイザーの初期状態です。 これは、State プロパティの既定値です。
- *開始*–連続ジェスチャが最初に認識されると、状態は "開始" に設定されます。 これにより、定期受信の開始時と変更時の区別をサブスクライブできます。
- *変更*–連続するジェスチャが開始された後、完了していない場合、ジェスチャの予期されるパラメーター内にある限り、タッチが移動または変更されるたびに状態が変更されるように設定されます。
- [*キャンセル*] –この状態は、認識エンジンの変更が開始された場合に設定され、その後、ジェスチャのパターンに適合しなくなるように変更されます。
- *認識*済み–ジェスチャ認識エンジンが一連のタッチに一致し、ジェスチャが終了したことをサブスクライバーに通知するときに、状態が設定されます。
- *終了*: 認識された状態のエイリアスです。
- *Failed* –ジェスチャ認識エンジンがリッスンしているタッチと一致しなくなると、状態が "失敗" に変わります。

Xamarin は、これらの値を `UIGestureRecognizerState` 列挙で表します。

## <a name="working-with-multiple-gestures"></a>複数のジェスチャの使用

既定では、iOS では、既定のジェスチャを同時に実行することはできません。 代わりに、各ジェスチャ認識エンジンは、非決定的な順序でタッチイベントを受け取ります。 次のコードスニペットは、ジェスチャ認識エンジンを同時に実行する方法を示しています。

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

IOS でジェスチャを無効にすることもできます。 ジェスチャ認識エンジンがアプリケーションの状態と現在のタッチイベントを調べて、ジェスチャを認識するかどうかを判断するには、2つのデリゲートプロパティを使用します。 次の2つのイベントがあります。

1. *ShouldReceiveTouch* –このデリゲートは、ジェスチャレコグナイザーがタッチイベントに渡される直前に呼び出され、タッチを調べて、ジェスチャ認識エンジンによって処理されるタッチを決定する機会を提供します。
1. [*開始*] –これは、レコグナイザーが状態を可能から他の状態に変更しようとしたときに呼び出されます。 False を返すと、ジェスチャ認識エンジンの状態が "失敗" に変更されます。

これらのメソッドは、次のコードスニペットに示すように、厳密に型指定された `UIGestureRecognizerDelegate`、弱いデリゲート、またはイベントハンドラー構文を介してバインドすることでオーバーライドできます。

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

最後に、ジェスチャ認識エンジンをキューに入れて、別のジェスチャ認識エンジンでエラーが発生した場合にのみ成功するようにすることができます。 たとえば、シングルタップジェスチャレコグナイザーは、ダブルタップジェスチャレコグナイザーが失敗した場合にのみ成功します。 次のコードスニペットは、この例を示しています。

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>カスタムジェスチャの作成

IOS にはいくつかの既定のジェスチャレコグナイザーが用意されていますが、特定の場合にカスタムのジェスチャレコグナイザーを作成することが必要になる場合があります。 カスタムジェスチャ認識エンジンを作成するには、次の手順を実行します。

1. サブクラス `UIGestureRecognizer`。
1. 適切な touch イベントメソッドをオーバーライドします。
1. 基本クラスの State プロパティを使用して、認識状態をバブルアップします。

実際の例については、 [「iOS でのタッチの使用」](ios-touch-walkthrough.md)チュートリアルで説明します。
