---
title: モノゲームのゲームパッドリファレンス
description: このドキュメントでは、モノゲームの入力デバイスにアクセスするためのクロスプラットフォームクラスであるゲームパッドについて説明します。 ここでは、ゲームパッドから入力を読み取り、コード例を示します。
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 11b1cfda435e97b09f9d1eded22f11f912d1783d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934031"
---
# <a name="monogame-gamepad-reference"></a>モノゲームのゲームパッドリファレンス

_ゲームパッドは、モノゲームの入力デバイスにアクセスするための標準的なクロスプラットフォームクラスです。_

`GamePad`を使用すると、複数のモノゲームプラットフォームの入力デバイスから入力を読み取ることができます。 このガイドでは、ゲームパッドクラスを使用する方法について説明します。 各入力デバイスはレイアウトと表示されるボタンの数が異なるため、このガイドにはさまざまなデバイスマッピングを示す図が含まれています。

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Xbox360GamePad の代わりとしてのゲームパッド

元の XNA API は、 `Xbox360GamePad` Xbox 360 または PC のゲームコントローラーから入力を読み取るためのクラスを提供していました。 モノゲームはこれをクラスに置き換えました。これは、 `GamePad` ほとんどのモノゲームプラットフォーム (iOS や Xbox One など) では xbox 360 コントローラーを使用できないためです。 名前の変更にかかわらず、クラスの使用方法 `GamePad` はクラスに似てい `Xbox360GamePad` ます。

## <a name="reading-input-from-gamepad"></a>ゲームパッドからの入力の読み取り

クラスは、 `GamePad` 任意のモノゲームプラットフォームで入力を読み取るための標準化された方法を提供します。 次の2つの方法で情報を提供します。

- `GetState`–コントローラーのボタン、アナログスティック、および d パッドの現在の状態を返します。
- `GetCapabilities`–コントローラーに特定のボタンがあるかどうか、振動をサポートするかどうかなど、ハードウェアの機能に関する情報を返します。

### <a name="example-moving-a-character"></a>例: 文字の移動

次のコードは、とのプロパティを設定して、左のサムスティックを使用して文字を移動する方法を示して `XVelocity` `YVelocity` います。 このコード `characterInstance` は、がプロパティとプロパティを持つオブジェクトのインスタンスであることを前提としてい `XVelocity` `YVelocity` ます。

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>例: プッシュの検出

`GamePadState`特定のボタンが押されているかどうかなど、コントローラーの現在の状態に関する情報を提供します。 文字のジャンプなどの特定のアクションでは、ボタンが押されたかどうかを確認する必要があります (つまり、最後のフレームではなく、このフレームの下にある) か、または解放されています (このフレームの下ではなく、最後のフレームを除く)。

この種類のロジックを実行するには、前のフレームと現在のフレームのを格納するローカル変数を `GamePadState` `GamePadState` 作成する必要があります。 次の例は、前のフレームのを格納して使用し、ジャンプを実装する方法を示してい `GamePadState` ます。

```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed =
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```

### <a name="example-checking-for-buttons"></a>例: ボタンのチェック

`GetCapabilities`特定のボタンやアナログスティックなど、コントローラーに特定のハードウェアがあるかどうかを確認するために使用できます。 次のコードは、両方のボタンが存在する必要があるゲームで、コントローラー上の B ボタンと Y ボタンを確認する方法を示しています。

```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```

## <a name="ios"></a>iOS

iOS アプリは、ワイヤレスゲームコントローラーの入力をサポートしています。

> [!IMPORTANT]
> モノゲーム3.5 の NuGet パッケージには、ワイヤレスゲームコントローラーのサポートは含まれていません。 IOS でゲームパッドクラスを使用するには、ソースからのモノのゲーム3.5 をビルドするか、またはモノ Game 3.6 NuGet バイナリを使用する必要があります。

### <a name="ios-game-controller"></a>iOS ゲームコントローラー

クラスは、 `GamePad` ワイヤレスコントローラーから読み取られたプロパティを返します。 のプロパティは、 `GamePad` 次の図に示すように、標準の iOS コントローラーハードウェアに適しています。

![ゲームパッドのプロパティは、次の図に示すように、標準の iOS コントローラーハードウェアに適しています。](input-images/image1.png)

## <a name="apple-tv"></a>Apple TV

Apple TV ゲームでは、Siri リモコンまたはワイヤレスゲームコントローラーを入力に使用できます。

### <a name="siri-remote"></a>Siri リモート

*Siri リモート*は Apple TV のネイティブ入力デバイスです。 Siri リモートからの値は、( [Siri リモコンおよび Bluetooth コントローラーガイド](~/ios/tvos/platform/remote-bluetooth.md)に示されているように) イベントを使用して読み取ることができますが、 `GamePad` クラスは siri リモートから値を返すことができます。

は `GamePad` 、再生ボタンとタッチサーフェスからの入力のみを読み取ることができることに注意してください。

![ゲームパッドは、再生ボタンとタッチ画面からの入力のみを読み取ることができることに注意してください。](input-images/image2.png)

タッチサーフェスの移動はプロパティによって読み取られるため `DPad` 、移動値はクラスを使用して報告され `ButtonState` ます。 つまり、数値は、 `ButtonState.Pressed` 数値やジェスチャではなく、またはとしてのみ使用でき `ButtonState.Released` ます。

### <a name="apple-tv-game-controller"></a>Apple TV ゲームコントローラー

Apple TV のゲームコントローラーは、iOS アプリのゲームコントローラーと同じように動作します。 詳細については、「 [IOS Game Controller](#ios-game-controller) 」セクションを参照してください。 

## <a name="xbox-one"></a>Xbox One

Xbox One コンソールは、Xbox One ゲームコントローラーからの入力の読み取りをサポートしています。

### <a name="xbox-one-game-controller"></a>Xbox One ゲームコントローラー

Xbox One ゲームコントローラーは、Xbox One の最も一般的な入力デバイスです。 クラスは、 `GamePad` ゲームコントローラーハードウェアからの入力値を提供します。

![ゲームパッドクラスは、ゲームコントローラーハードウェアからの入力値を提供します。](input-images/image3.png)

## <a name="summary"></a>まとめ

このガイドでは、モノゲームのクラスの概要 `GamePad` 、入力読み取りロジックの実装方法、および一般的な実装の図について説明しました `GamePad` 。

## <a name="related-links"></a>関連リンク

- [モノゲームのゲームパッド](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
