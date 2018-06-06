---
title: MonoGame ゲーム パッドなどの参照
description: このドキュメントでは、ゲーム パッド、MonoGame で入力デバイスにアクセスするクロスプラット フォーム クラスについて説明します。 ゲーム パッドから入力を読み取る方法について説明し、コード例を紹介します。
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: f8a833dbab2b8a69f328cd26cb69db84f3083222
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783325"
---
# <a name="monogame-gamepad-reference"></a>MonoGame ゲーム パッドなどの参照

_ゲーム パッドは、MonoGame で入力デバイスにアクセスするための標準、クロスプラット フォームのクラスです。_

`GamePad` 入力プラットフォームのデバイスを複数 MonoGame から入力を読み取るために使用できます。 このガイドでは、ゲーム パッド クラスを使用する方法を示します。 各入力デバイスは、レイアウトと、ボタンの数で異なる場合は、ので、このガイドには、さまざまなデバイスのマッピングを示すダイアグラムにはが含まれています。

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Xbox360GamePad の代わりに、ゲーム パッド

提供元 XNA API、 `Xbox360GamePad` PC、Xbox 360 にゲーム コント ローラーからの入力を読み取るのためのクラスです。 これを MonoGame が置き換えられます、`GamePad`クラスのため、ほとんどの MonoGame プラットフォーム (iOS または Xbox One) などで Xbox 360 コント ローラーは使用できません。 使用法、名前の変更に関係なく、`GamePad`クラスがに似ていますが、`Xbox360GamePad`クラスです。

## <a name="reading-input-from-gamepad"></a>ゲーム パッドなどからの入力を読み取る

`GamePad`クラスは、任意の MonoGame プラットフォームでの入力を読み取るの標準化された方法を提供します。 2 つの方法で情報を提供します。

- `GetState` – コント ローラーのボタン、アナログ スティック、および方向パッドの現在の状態を返します。
- `GetCapabilities` – ボタンまたは振動をサポートしているコント ローラーに特定できるかどうかなど、ハードウェアの機能に関する情報を返します。

### <a name="example-moving-a-character"></a>例: の文字を移動します。

次のコードは、左スティックを使用して設定して、文字を移動する方法を示しています。 その`XVelocity`と`YVelocity`プロパティです。 このコードでは`characterInstance`を持つオブジェクトのインスタンスは、`XVelocity`と`YVelocity`プロパティ。

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>例: プッシュの検出

`GamePadState` 特定のボタンが押されたかどうかなど、コント ローラーの現在の状態に関する情報を提供します。 ジャンプ、文字など、特定のアクションが、ボタンがプッシュされた場合のチェックを要求 (最後のフレームをでしたがこのフレーム ダウン)、または離された (最後のフレームがこのフレーム ダウンされませんでした)。 

このようなロジック、前のフレームを格納するローカル変数の実行に`GamePadState`と現在のフレームの`GamePadState`作成する必要があります。 次の例は、保存し、前のフレームを使用する方法を示しています。`GamePadState`ジャンプを実装します。

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

### <a name="example-checking-for-buttons"></a>例: がボタンのチェック

`GetCapabilities` コント ローラーに、特定のボタンやアナログ スティックなどの特定のハードウェアを確認して使用できます。 次のコードは、コント ローラーで、両方のボタンの存在を必要とするゲーム B と Y のボタンをチェックする方法を示しています。

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

iOS アプリでは、ワイヤレス ゲーム コント ローラーの入力をサポートします。

> [!IMPORTANT]
> MonoGame 3.5 用の NuGet パッケージでは、ワイヤレス ゲーム コント ローラーのサポートは含まれません。 IOS で、ゲーム パッド クラスを使用するには、ソースから MonoGame 3.5 を構築または MonoGame 3.6 NuGet バイナリを使用して必要があります。 

### <a name="ios-game-controller"></a>iOS ゲーム コント ローラー

`GamePad`クラスは、ワイヤレス コント ローラーから読み取られたプロパティを返します。 内のプロパティ、`GamePad`提供カバレッジ標準的な iOS のコント ローラーのハードウェア、次の図に示すようにします。

![](input-images/image1.png "プロパティ、ゲーム パッドなどを提供カバレッジ標準的な iOS のコント ローラーのハードウェア、この図に示すように")

## <a name="apple-tv"></a>Apple TV

Apple TV のゲームでは、入力の Siri リモートまたはワイヤレスのゲーム コント ローラーを使用できます。

### <a name="siri-remote"></a>Siri リモート

*Siri リモート*Apple TV のネイティブの入力デバイスがします。 イベントによって、Siri リモートからの値を読み取ることができますが (のように、 [Siri リモート コンピューターと Bluetooth コント ローラーのガイド](~/ios/tvos/platform/remote-bluetooth.md)) では、`GamePad`クラスは、Siri リモートからの値を返すことができます。

注意して`GamePad`再生 ボタンからの入力を読み取るし、画面をタッチすることができますのみ。 

![](input-images/image2.png "ゲーム パッドの [再生] ボタンからの入力を読み取るし、表面に触れてできますのみに注意してください。")

以降、タッチ画面の動きが目を通して、`DPad`プロパティ、値が使用してレポートの移動、`ButtonState`クラスです。 つまり、値として使用できるだけは`ButtonState.Pressed`または`ButtonState.Released`数値やジェスチャではなく、します。

### <a name="apple-tv-game-controller"></a>Apple TV のゲーム コント ローラー

Apple TV のゲーム コント ローラーの iOS アプリのゲーム コント ローラーに動作は同じです。 詳細については、次を参照してください。、 [iOS ゲーム コント ローラー セクション](#iOS_Game_Controller)です。 

## <a name="xbox-one"></a>Xbox One

Xbox One コンソールには、Xbox One ゲーム コント ローラーからの入力を読み取るがサポートしています。

### <a name="xbox-one-game-controller"></a>Xbox 1 つのゲーム コント ローラー

Xbox One ゲーム コント ローラーは、Xbox One の最も一般的な入力デバイスです。 `GamePad`クラスは、ゲーム コント ローラーのハードウェアからの入力値を提供します。

![](input-images/image3.png "ゲーム パッド クラスには、ゲーム コント ローラーのハードウェアからの入力値が用意されています")

## <a name="summary"></a>まとめ

このガイドの MonoGame の概要を説明する`GamePad`クラス、入力を読み取るロジック、および一般的な図を実装する方法`GamePad`実装します。

## <a name="related-links"></a>関連リンク

- [MonoGame ゲーム パッド](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
