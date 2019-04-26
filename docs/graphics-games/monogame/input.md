---
title: MonoGame GamePad リファレンス
description: このドキュメントでは、ゲームパッド、MonoGame の入力デバイスにアクセスするためのクロスプラット フォーム対応クラスについて説明します。 ゲームパッドから入力を読み取る方法について説明し、コード例を示します。
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 235166b78dfbd4998086a2925a54137f1922f5d1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61385703"
---
# <a name="monogame-gamepad-reference"></a>MonoGame GamePad リファレンス

_GamePad は、MonoGame の入力デバイスにアクセスするための標準的なクロス プラットフォームのクラスです。_

`GamePad` MonoGame の複数のプラットフォームでの入力デバイスから入力を読み取るために使用できます。 このガイドでは、ゲームパッド クラスを操作する方法を示します。 レイアウトと、ボタンの数で各入力デバイスが異なる場合は、以降このガイドには、さまざまなデバイスのマッピングを示すダイアグラムが含まれています。

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Xbox360GamePad の代わりに、ゲーム パッド

提供される元の XNA API、 `Xbox360GamePad` PC、Xbox 360 のゲーム コント ローラーから入力を読み取るのためのクラス。 これを MonoGame は置き換えられて、`GamePad`クラスのほとんどの MonoGame プラットフォーム (iOS、Xbox One) などで Xbox 360 コント ローラーを使用できないためです。 名前の変更の使用量に関係なく、`GamePad`クラスと似ています、`Xbox360GamePad`クラス。

## <a name="reading-input-from-gamepad"></a>GamePad から入力を読み取る

`GamePad`クラスは、任意の MonoGame プラットフォームでの入力を読み取る、標準化された方法を提供します。 2 つの方法の情報を提供します。

- `GetState` – コント ローラーのボタン、アナログ スティックおよびパッドの現在の状態を返します。
- `GetCapabilities` – ボタンまたは振動をサポートしているコント ローラーに特定できるかどうかなど、ハードウェアの機能に関する情報を返します。

### <a name="example-moving-a-character"></a>例:文字の移動

次のコードは、左スティックを使用して設定して、文字を移動する方法を示しています。 その`XVelocity`と`YVelocity`プロパティ。 このコードで`characterInstance`を持つオブジェクトのインスタンスである`XVelocity`と`YVelocity`プロパティ。

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>例:プッシュの検出

`GamePadState` 特定のボタンが押されたかどうかなど、コント ローラーの現在の状態に関する情報を提供します。 ジャンプ、文字などの特定のアクションでは、チェック ボタンがプッシュされた場合の要求 (最後のフレームをでしたがこのフレーム ダウン)、またはリリース (最後のフレームのダウンがこのフレーム ダウンできませんでした)。

この種のロジックでは、前のフレームを格納するローカル変数を実行する`GamePadState`と現在のフレームの`GamePadState`作成する必要があります。 次の例は、保存し、前のフレームを使用する方法を示しています。`GamePadState`ジャンプを実装します。

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

### <a name="example-checking-for-buttons"></a>例:ボタンのチェック

`GetCapabilities` かどうか、コント ローラーには、特定のボタンやアナログ スティックなどの特定のハードウェアを確認するために使用します。 次のコードでは、両方のボタンの存在を必要とするゲーム内のコント ローラー B と Y のボタンを確認する方法を示します。

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

iOS アプリでは、ワイヤレスのゲーム コント ローラーの入力をサポートします。

> [!IMPORTANT]
> MonoGame 3.5 用の NuGet パッケージでは、ワイヤレスのゲーム コント ローラーのサポートは含まれません。 IOS でゲームパッド クラスを使用するには、MonoGame 3.5 ソースからを構築または MonoGame 3.6 NuGet バイナリを使用する必要があります。

### <a name="ios-game-controller"></a>iOS ゲーム コント ローラー

`GamePad`クラスは、ワイヤレス コント ローラーから読み取られたプロパティを返します。 内のプロパティ、`GamePad`に十分対応の標準的な iOS コント ローラー ハードウェアを提供、次の図に示すようにします。

![](input-images/image1.png "プロパティ、ゲームパッドを提供に十分対応、標準的な ios コント ローラーのハードウェア、この図に示すように")

## <a name="apple-tv"></a>Apple TV

Apple TV のゲームでは、入力の Siri のリモートまたはワイヤレスのゲーム コント ローラーを使用できます。

### <a name="siri-remote"></a>Siri のリモート

*Siri のリモート*ネイティブの入力デバイスを Apple TV にです。 Siri のリモートからの値はイベントを介して読み取ることができます (ように、 [Siri のリモートと Bluetooth コント ローラーのガイド](~/ios/tvos/platform/remote-bluetooth.md))、`GamePad`クラスは、Siri のリモートから値を返すことができます。

注意`GamePad`のみ再生 ボタンから入力を読み取りし、画面をタッチすることができます。

![](input-images/image2.png "ゲームパッドの再生 ボタンから入力を読み取り、画面をタッチすることができますのみに注目してください。")

Surface の移動を読んで、タッチから、`DPad`プロパティの値が報告を使用して移動、`ButtonState`クラス。 つまり、値として使用可能なだけは`ButtonState.Pressed`または`ButtonState.Released`数値やジェスチャではなく、します。

### <a name="apple-tv-game-controller"></a>Apple TV のゲーム コント ローラー

Apple TV のゲーム コント ローラーの動作は iOS アプリのゲーム コント ローラーに同じです。 詳細については、次を参照してください。、 [iOS ゲーム コント ローラー](#ios-game-controller)セクション。 

## <a name="xbox-one"></a>Xbox One

Xbox One のコンソールには、Xbox One のゲーム コント ローラーからの入力がサポートしています。

### <a name="xbox-one-game-controller"></a>Xbox One のゲーム コント ローラー

Xbox One のゲーム コント ローラーは、Xbox One の最も一般的な入力デバイスです。 `GamePad`クラスは、ゲーム コント ローラー ハードウェアからの入力値を提供します。

![](input-images/image3.png "GamePad クラスは、ゲーム コント ローラー ハードウェアから入力値を提供します。")

## <a name="summary"></a>まとめ

このガイドの MonoGame の概要を提供する`GamePad`クラス、入力を読み取るロジック、および一般的な図を実装する方法`GamePad`実装します。

## <a name="related-links"></a>関連リンク

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
