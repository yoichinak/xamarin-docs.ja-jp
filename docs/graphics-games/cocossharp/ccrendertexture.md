---
title: パフォーマンスと CCRenderTexture に視覚効果
description: CCRenderTexture 描画呼び出しを減らすことによって、CocosSharp ゲームのパフォーマンスを向上させるために開発者を有効にして視覚効果を作成するために使用できます。 このガイドでは、このクラスを効果的に使用する方法の実践的な例を提供する CCRenderTexture サンプルに付属しています。
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 82d39542b24c6b1669798a0b4e1fb14a81f6b44e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922523"
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>パフォーマンスと CCRenderTexture に視覚効果

_CCRenderTexture 描画呼び出しを減らすことによって、CocosSharp ゲームのパフォーマンスを向上させるために開発者を有効にして視覚効果を作成するために使用できます。このガイドでは、このクラスを効果的に使用する方法の実践的な例を提供する CCRenderTexture サンプルに付属しています。_

`CCRenderTexture`クラスには、1 つのテクスチャに複数の CocosSharp オブジェクトを表示するための機能が用意されています。 作成すると、`CCRenderTexture`グラフィックスを効率的にレンダリングして、視覚効果を実装するインスタンスを使用できます。 `CCRenderTexture` により、複数のオブジェクトを 1 回に 1 つのテクスチャにレンダリングされます。 その後、そのテクスチャができる描画呼び出しの合計数を減らして、すべてのフレームを再利用します。

このガイドは、使用する方法を説明します、`CCRenderTexture`レンダリング カード回収可能なカード ゲーム (CCG) でのパフォーマンスを向上するオブジェクト。 についても示す方法`CCRenderTexture`エンティティ全体を透明にするために使用できます。 このガイドを参照して、 `CCRenderTexture` [サンプル プロジェクト](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)です。

![](ccrendertexture-images/image1.png "このガイドは、CCRenderTexture サンプル プロジェクトを参照します。")


## <a name="card--a-typical-entity"></a>カード – 一般的なエンティティ

使用する方法を探す前に`CCRenderTexture`オブジェクト、おありますまず理解するために社内で、`Card`を調査にこのプロジェクト全体で使用するエンティティ、`CCRenderTexture`クラスです。 `Card`クラスに記載されているエンティティのパターンに従う、標準的なエンティティ、[エンティティ ガイド](~/graphics-games/cocossharp/entities.md)です。 カード クラスにはすべてのビジュアル コンポーネント (のインスタンス`CCSprite`と`CCLabel`) フィールドとして一覧表示します。


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

カードのインスタンスを使用して出力することができます、 `CCRenderTexture`、または各ビジュアル コンポーネントを個別に描画することによってです。 各コンポーネントは、独立したオブジェクトが、`CCNode`エンティティで使用されるシステムの親は、`Card`単一のオブジェクトとして – 以上で、ほとんどの部分に動作します。 たとえば場合、`Card`カードを 1 つのオブジェクトのように見えるに含まれているすべてのビジュアル オブジェクトに影響し、エンティティが再配置、サイズ変更、または回転します。 1 つのオブジェクトとして動作するカードを表示するには、変更できる、`GameLayer.AddedToScene`を設定するメソッド、`useRenderTextures`変数を`false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

`GameLayer`コードを移動しません visual の各要素とは別に、まだ内の各視覚的要素、`Card`エンティティが正しく配置されています。

![](ccrendertexture-images/image1.png "GameLayer コードを移動しません visual の各要素とは別に、まだカード エンティティ内の各 visual 要素が正しく配置されています。")

サンプルの各ビジュアル コンポーネントが自身を表示するときに発生する可能性がある 2 つの問題を公開するコードの記述します。

- 複数の描画呼び出しによりパフォーマンスが低下することができます。
- 後でについて学びますように、透明度などの特定の視覚効果を正確には、実装ことはできません。


### <a name="card-draw-calls"></a>描画呼び出しのカード

このコードは完全で見つかる可能性があります新機能の簡素化*回収可能なカード ゲーム*(CCG)「マジック::、収集しています」または"Hearthstone"などです。 ゲーム 3 枚を一度に表示、および単位を指定できます (青、緑とオレンジ色) の数が少ないのはのみです。 これに対し、完全ゲームが特定の時点で 20 を超えるカードを画面に表示される必要があります、プレーヤーは何百ものカードが、組の作成時に選択する必要があります。 ゲームがパフォーマンスの問題から低下しない場合でも同様の実装と完全ゲーム可能性があります。

CocosSharp では、実行されるフレームごとの描画呼び出しを公開することによりレンダリング パフォーマンスに関する洞察を提供します。 当社`GameLayer.AddedToScene`メソッドのセット、`GameView.Stats.Enabled`に`true`、その結果、画面の左下に表示されるパフォーマンス情報に。

![](ccrendertexture-images/image2.png "GameLayer.AddedToScene メソッドが true の場合、その結果、画面の左下に表示されるパフォーマンス情報を GameView.Stats.Enabled を設定します。")

(6 カテゴリにおける各カード結果テキストの描画呼び出し、1 つ以上のパフォーマンス情報のアカウントを表示する) nineteen の描画呼び出しが画面に 3 つのカードを用意することもあることに注意してください。 描画呼び出しは、ゲームのパフォーマンスに大きな影響を与えるので CocosSharp がそれらを軽減する方法の数を提供します。 1 つの方法については、「、 [CCSpriteSheet ガイド](~/graphics-games/cocossharp/ccspritesheet.md)です。 別の方法は、使用する、`CCRenderTexture`を 1 回の呼び出しまで各エンティティを減らすようにこのガイドで説明します。


### <a name="card-transparency"></a>カードの透過性

当社`Card`エンティティが含まれています、`Opacity`プロパティを次のコード スニペットで示すようにコントロールの透過性。


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

使用して、set アクセス操作子をサポートしている通知は、テクスチャまたは各コンポーネントを個別にレンダリングをレンダリングします。 その効果を表示するには、変更、`opacity`値を`127`(約半分の不透明度) で`GameLayer.AddedToScene`その結果、各コンポーネントを持つ、`Opacity`の値`127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

ゲームは、暗くを設定するため、背景色は黒になり、いくつか透過性のカードを今すぐレンダリングされます。

![](ccrendertexture-images/image3.png "ゲームでは、背景色は黒ためにが暗く原因と、いくつか透明度をカードをレンダリングするようになりました")

一見カードが正しく行わ透過的な場合とになります。 ただし、スクリーン ショットには、視覚的な問題の数が表示されます。

黒、バック グラウンドであるため、カードのすべての部分になるは、透過性のために暗いが予想されるがします。 透過的なほど、カードになり、色が濃いほど、します。 不透明度 0 の場合で、`Card`完全に透過的になります (完全に黒) です。 ただし、当社のカードの一部がありませんでしたが暗くなりますを不透明度が変更されたときに`127`です。 さらに悪い、カードの一部実際にようになりました明るいとき、透過性となりました。 黒、カードの部分を見てみましょう*する前に*が透過的な – 詳細テキストでは具体的には、モンスター グラフィック周囲に黒い枠です。 サイド バイ サイドをこれらに配置した場合、透過性の適用の影響を確認できます。

![](ccrendertexture-images/image4.png "透過性の適用の影響が見られますサイド バイ サイドで配置している場合")

透過的になるときに上記のとおり、カードのすべての部分が暗くなります必要がありますは、多くの点で大文字と小文字。

- ロボットのアウトラインが明るい (グレー表示には、黒から移動)
- 説明のテキストが明るい (グレー表示には、黒から移動)
- ロボットの緑の一部は、小さい飽和状態になりますが、しませんが暗くなります

これが発生する理由を視覚化するために、いただくために各ビジュアル コンポーネントが描画とは別に、それぞれ部分的に透過的なことに注意してください。 描画される最初のビジュアル コンポーネントは、カードのバック グラウンドです。 以降の透過的な要素がカード上に描画され、カードのバック グラウンドの影響を受けます。 このカードからいくつかのテキストを削除すると、ロボット グラフィックを下へ移動、カードの背景がロボットをどのように影響するかがわかります。 上部のボックスからオレンジ色の線は、ロボットを見なすことができ、カードの中央に青のストライプが重複しているロボットの領域を暗い描画することに注意してください。

![](ccrendertexture-images/image5.png "上部のボックスからオレンジ色の線は、ロボットを見なすことができ、カードの中央に青のストライプが重複しているロボットの領域を暗い描画することに注意してください。")

使用して、`CCRenderTexture`透明カード全体、カード内の個々 のコンポーネントのレンダリングの影響を与えずにこのガイドで紹介するようにします。


## <a name="using-ccrendertexture"></a>CCRenderTexture を使用します。

レンダリングを有効に各コンポーネントを個別にレンダリングの問題を識別してしたら、これで、`CCRenderTexture`と動作を比較します。

レンダリングを有効にする、 `CCRenderTexture`、変更、`userRenderTextures`変数を`true`で`GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>描画呼び出しのカード

ゲームを今すぐ実行お場合は、4 nineteen から短縮描画呼び出しを表示おされます (各カードは、いずれかに 6 個から reduced)。

![](ccrendertexture-images/image6.png "ゲームを今すぐ実行すると場合、描画呼び出しは少なくなります nineteen 4 から各カードから 1 つに 6 個の短縮")

前述のように、この種類の削減の画面に詳細 visual エンティティとゲームに大きく影響することができます。


### <a name="card-transparency"></a>カードの透過性

1 回、`useRenderTextures`に設定されている`true`、透過的なカードが異なる方法で表示されます。

![](ccrendertexture-images/image7.png "UseRenderTextures を設定する場合は true、透過的なカードには異なる方法で表示されます。")

(左) と (右) なしのレンダリング テクスチャを使用して、透過的なロボット カードを比較してみましょう。

![](ccrendertexture-images/image8.png "(右) なしのレンダリングのテクスチャ (左側) vs を使用して、透過的なロボット カードを比較します。")

最も明白な違いは、詳細テキスト (薄い灰色の代わりに黒) およびロボット スプライト (ライトではなく濃いおよび彩度) です。


## <a name="ccrendertexture-details"></a>CCRenderTexture 詳細

これを使用するメリットを見た`CCRenderTexture`での使用方法を見てみましょう、`Card`エンティティです。

`CCRenderTexture`がキャンバスのレンダリングのターゲットにすることです。 ゲームの画面と比較して 2 つの主な違いがあります。

1. `CCRenderTexture`間のフレームが引き続き発生します。 つまり、`CCRenderTexture`変更が発生したときにのみレンダリングする必要があります。 ここでは、`Card`エンティティ決して変化して、1 回だけ表示されるようにします。 存在する場合`Card`コンポーネントを変更し、カードを自動的に再描画する必要があります、`CCRenderTexture`です。 たとえば、HP 値 (正常性のポイント) が変更されたは、攻撃を受けたときに場合はし、カードが新しい HP 値を反映するようにそれ自体を表示するために必要なります。
1. `CCRenderTexture`ピクセル数が、画面に関連付けられていません。 A`CCRenderTexture`デバイスの解像度より大きくまたは小さくすることができます。 `Card`コードを作成、`CCRenderTexture`その背景スプライトのサイズを使用します。 カードにはへの参照が含まれています、`CCRenderTexture`と呼ばれる`renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture`インスタンスまま`null`まで、`UseRenderTexture`プロパティが true、呼び出しに割り当てられた`SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

`SwitchToRenderTexture`テクスチャを更新する必要があるたびに、メソッドを呼び出すことができます。 呼び出すことができます、カードは既に使用しているかどうか、`CCRenderTexture`またはへの切り替えは、`CCRenderTexture`が最初にします。

次のセクションでは、探索、`SwitchToRenderTexture`メソッドです。 


### <a name="ccrendertexture-size"></a>CCRenderTexture サイズ

CCRenderTexture コンス トラクターには、2 つのディメンションのセットが必要です。 最初のコントロールのサイズが、`CCRenderTexture`が描画し、2 つ目は、ピクセル幅とその内容の高さを指定します。 `Card`エンティティのインスタンスを作成、`CCRenderTexture`背景を使用する[ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/)です。 このゲームに、 `DesignResolution` 512 によって 384 のように`ViewController.LoadGame`iOS でと`MainActivity.LoadGame`Android で。


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture`でコンス トラクターが呼び出されます、 `background.ContentSize` 、最初のパラメーターとしてことを示す、`CCRenderTexture`だけ、背景と同じ大きさにする必要があります`CCSprite`です。 カードのバック グラウンド以降`CCSprite`高さ 200 ピクセル、カードが占めるおおよそ画面の縦の高さの半分です。

渡される 2 番目のパラメーター、`CCRenderTexture`コンス トラクターのピクセルの解像度を指定する、`CCRenderTexture`です。 説明したように、 [CocosSharp 解決ガイド](~/graphics-games/cocossharp/resolutions.md)、ゲームの単位で表示可能領域の高さと幅は多くの場合、画面のピクセルの解像度と同じです。 同様に、ビジュアルが高解像度のデバイスに鮮明な表示されるように、CCRenderTexture はそのサイズより大きい解像度を使用できます。

ピクセルの解像度は、見た目の汚いからテキストを防ぐために CCRenderTexture の 2 倍のサイズを示します。


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

比較するに一致する pixelResolution 値を変更できます、 `background.ContentSize` (せずにされている 2 倍になります) と、結果を比較します。 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "比較するには、背景に合わせて pixelResolution 値を変更できます。2 倍にすることがなく contentSize し、結果の比較")


### <a name="rendering-to-a-ccrendertexture"></a>CCRenderTexture に表示します。

通常、CocosSharp でビジュアルのオブジェクトは明示的にレンダリングされません。 ビジュアル オブジェクトを追加する代わりに、`CCLayer`の一部では、`CCScene`です。 CocosSharp を自動的に表示、`CCScene`とすべてのフレームが呼び出されるレンダリング コードなしでそのビジュアルの階層です。 

これに対し、`CCRenderTexture`に明示的に描画する必要があります。 この表示は、3 つの手順に分けることができます。

1. `CCRenderTexture.BeginWithClear` 呼び出されると、後続のすべてのレンダリングを呼び出し元は対象とすることを示す`CCRenderTexture`です。
1. オブジェクトから継承する`CCNode`(と同様に、`Card`エンティティ) にレンダリングされる、`CCRenderTexture`を呼び出して`Visit`です。
1. `CCRenderTexture.End` 呼び出されるに表示することを示す、`CCRenderTexture`が完了しました。

オブジェクトの任意の数を表示することができます、`CCRenderTexture`間でその`Begin`と`End`呼び出しです。 レンダリングの前にすべての必要な表示されているオブジェクトが子として追加されます。


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

`renderTexture`することはできません、カードの一部、レンダリング時に削除されるようにします。


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

これで、`Card`インスタンスはそれ自体を表示できます、`CCRenderTexture`インスタンス。


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

レンダリングが完了したら、個々 のコンポーネントが削除され、`CCRenderTexture`再追加されました。


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

## <a name="summary"></a>まとめ

このガイドの説明、`CCRenderTexture`クラスを使用して、`Card`収集可能なカード ゲームで使用できるエンティティです。 使用する方法を示しましたが、`CCRenderTexture`フレーム率を向上して、エンティティ全体の透明度を適切に実装するクラス。

このガイドの使用、`CCRenderTexture`エンティティ内に含まれる、このできるクラスは、複数のエンティティを表示するために使用されるまたは全体`CCLayer`画面全体に関わるとパフォーマンスの向上のインスタンス。

## <a name="related-links"></a>関連リンク

- [CCRenderTexture API リファレンス](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [完全なプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
