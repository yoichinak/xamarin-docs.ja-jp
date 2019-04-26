---
title: パフォーマンスと CCRenderTexture で視覚効果
description: CCRenderTexture は描画の呼び出しを減らすことで CocosSharp ゲームのパフォーマンスを向上させることができ、視覚効果を作成するために使用できます。 このガイドには、このクラスを効果的に使用する方法の実践的な例を提供する CCRenderTexture サンプルが付属しています。
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 95227689303a8367785202956a6aaef921c1c593
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085268"
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>パフォーマンスと CCRenderTexture で視覚効果

_CCRenderTexture は描画の呼び出しを減らすことで CocosSharp ゲームのパフォーマンスを向上させることができ、視覚効果を作成するために使用できます。このガイドには、このクラスを効果的に使用する方法の実践的な例を提供する CCRenderTexture サンプルが付属しています。_

`CCRenderTexture`クラスは、1 つのテクスチャに複数の CocosSharp オブジェクトをレンダリングするための機能を提供します。 作成すると、`CCRenderTexture`効率的にグラフィックスを描画し、視覚効果を実装するインスタンスを使用できます。 `CCRenderTexture` 複数のオブジェクトを 1 回に 1 つのテクスチャを表示できます。 次に、そのテクスチャができる描画呼び出しの合計数を減らすと、すべてのフレームを再利用します。

このガイドでは、使用する方法、`CCRenderTexture`収集可能なカード ゲーム (CCG) 内のカードのレンダリングのパフォーマンスを向上させるオブジェクト。 またどの`CCRenderTexture`エンティティ全体を透明にするために使用できます。 このガイドを参照して、 `CCRenderTexture` [サンプル プロジェクト](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)します。

![](ccrendertexture-images/image1.png "このガイドは、CCRenderTexture のサンプル プロジェクトを参照します。")


## <a name="card--a-typical-entity"></a>カード – 一般的なエンティティ

使用する方法を見ていく前に`CCRenderTexture`オブジェクト、自分たちで理解しましたを最初、`Card`調査にこのプロジェクト全体で使用するエンティティ、`CCRenderTexture`クラス。 `Card`クラスに記載されているエンティティのパターンに従う、標準的なエンティティ、[エンティティ ガイド](~/graphics-games/cocossharp/entities.md)します。 Card クラスがすべてのビジュアル コンポーネント (インスタンスの`CCSprite`と`CCLabel`) フィールドとして一覧表示します。


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

カードのインスタンスを使用して出力することができます、 `CCRenderTexture`、または各ビジュアル コンポーネントを個別に描画しています。 各コンポーネントは、独立したオブジェクトが、`CCNode`により、親エンティティで使用されるシステム、 `Card` – 以上で、ほとんどの部分を単一オブジェクトとして動作します。 たとえば場合、`Card`カードを 1 つのオブジェクトのように見えるに含まれているすべてのビジュアル オブジェクトが影響を受けるエンティティの位置を変更、サイズ変更、または回転します。 1 つのオブジェクトとして動作するカードを表示するを変更する、`GameLayer.AddedToScene`を設定するメソッド、`useRenderTextures`変数を`false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

`GameLayer`コードを移動しません各ビジュアル要素とは別に、まだ内の各ビジュアル要素、`Card`エンティティが正しく配置されています。

![](ccrendertexture-images/image1.png "GameLayer コードを移動しません各ビジュアル要素とは別に、まだカード エンティティ内の各ビジュアル要素が正しく配置されています。")

各ビジュアル コンポーネントが自身をレンダリングするときに発生する可能性がある 2 つの問題を公開するサンプルはコーディングしています。

- 複数の描画呼び出しによりパフォーマンスが低下することができます。
- 後で調査するときに透明性などの特定の視覚効果を正確には、実装することはできません。


### <a name="card-draw-calls"></a>描画呼び出しのカード

コードの内容は、完全で見つかる可能性がありますの簡素化*収集可能なカード ゲーム*(CCG) など、"マジック。収集"または"Hearthstone"。 のみ、ゲームは 3 つのカードを一度に表示あり (青、緑、およびオレンジ色) 可能なユニットの数が少ないになります。 これに対し、完全なゲームが特定の時点で 20 を超えるカードを画面に表示される必要があり、プレーヤーは数百のカードのデッキを作成するときに選択する必要があります。 ゲームがパフォーマンスの問題から低下しない場合でも同様の実装と完全なゲーム可能性があります。

CocosSharp では、描画呼び出しを公開することにより、レンダリングのパフォーマンスに関する洞察フレームごとの実行を提供します。 `GameLayer.AddedToScene`メソッドのセット、`GameView.Stats.Enabled`に`true`、パフォーマンスについては、画面の左下に表示されます。

![](ccrendertexture-images/image2.png "GameLayer.AddedToScene メソッドが true の場合、その結果、画面の左下に表示されるパフォーマンス情報を GameView.Stats.Enabled を設定します。")

(6 では、各カード結果テキストの描画呼び出し、もう 1 つのパフォーマンス情報のアカウントを表示する)」に綴っての描画呼び出しが画面に 3 つのカードをいてもあることに注意してください。 CocosSharp それらを軽減する方法のいくつか提供されますので、描画呼び出しは、ゲームのパフォーマンスに大きな影響を与えます。 1 つの方法が記載されて、 [CCSpriteSheet ガイド](~/graphics-games/cocossharp/ccspritesheet.md)します。 別の手法は、使用する、`CCRenderTexture`を 1 回の呼び出しには、各エンティティを減らすようにこのガイドで説明します。


### <a name="card-transparency"></a>カードの透過性

この`Card`エンティティが含まれています、`Opacity`プロパティを次のコード スニペットに示すようにコントロールの透過性。


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

使用して、set アクセス操作子をサポートしている通知は、テクスチャや各コンポーネントを個別にレンダリングをレンダリングします。 その効果を表示する変更、`opacity`値を`127`(おおよそ半分の不透明度) で`GameLayer.AddedToScene`その結果、それぞれのコンポーネントが、 `Opacity` @property `127`:


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

ゲームでは、いくつかの透明度、発生しているため、背景が黒、暗くカードはレンダリングようになりました。

![](ccrendertexture-images/image3.png "ゲームはいくつかの透明度、発生しているため、背景が黒、暗くカードが表示されます。")

一見、カードが正しく行われた透明かのようになります。 ただし、スクリーン ショットでは、さまざまな視覚的な問題が表示されます。

背景が黒にするため、カードのすべての部分になるは、透過性のために暗いを予想します。 つまりより透過的なカードは暗くなります。 不透明度 0 の場合で、`Card`を完全に透明にする (完全に黒)。 ただし、このカードの一部でしたを不透明度が変更されたときに暗くなります`127`します。 さらに悪いこと、カードの一部実際になりました明るいときより透過的するようになりました。 黒、カードの部分を見てみましょう*する前に*された透過的な – 詳細なテキストでは具体的には、モンスター グラフィックの周囲に黒い枠。 場合はサイド バイ サイドをこれらに行っています透明度を適用することの影響がわかります。

![](ccrendertexture-images/image4.png "透明度を適用することの影響が見られます、並行して配置する場合")

透過的になるときに、上記のとおり、カードのすべての部分が暗くなる必要がありますが、多くの点でない場合。

- ロボットのアウトラインが薄くなります (黒から灰色が)
- 説明のテキストが薄くなります (黒から灰色が)
- ロボットの緑色部分は、小さい飽和状態になりますが、暗くしません。

これが発生する理由を視覚化するためは、各ビジュアル コンポーネントは、描画とは別に、それぞれ部分的に透明なことに留意する必要があります。 描画される最初のビジュアル コンポーネントは、カードの背景です。 後続の透明な要素は、カードの上に描画され、カードの背景の影響を受けます。 ロボット グラフィックを下へ移動、カードからいくつかのテキストを削除すると、カードの背景が、ロボットをどのように影響するかを確認できます。 ロボットでは、上部のボックスからオレンジ色の線を表示できるし、カードの中央に青いストライプと重なるロボットの領域を濃い描画することに注意してください。

![](ccrendertexture-images/image5.png "ロボットでは、上部のボックスからオレンジ色の線を表示できるし、カードの中央に青いストライプと重なるロボットの領域を濃い描画することに注意してください。")

使用して、`CCRenderTexture`を透明にカード全体、カード内の個々 のコンポーネントのレンダリングに影響を与えることがなくこのガイドの後半で紹介するようできます。


## <a name="using-ccrendertexture"></a>CCRenderTexture を使用します。

レンダリングを有効にできたので、各コンポーネントを個別にレンダリングの問題を特定できました、`CCRenderTexture`と動作を比較します。

レンダリングを有効にする、 `CCRenderTexture`、変更、`userRenderTextures`変数を`true`で`GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>描画呼び出しのカード

数が少なくなって nineteen を 4 つに、描画呼び出しが表示されますゲームを今すぐ実行した場合 (各カードは、いずれかに 6 から減少)。

![](ccrendertexture-images/image6.png "Nineteen 4 に短縮されました描画呼び出しが、それぞれのカードを使用する場合は、ゲームを今すぐ実行すると、いずれかに 6 つの数が少なくなって")

前述のように、この種類の削減は画面にビジュアル エンティティの詳細を使用したゲームに大きな影響があります。


### <a name="card-transparency"></a>カードの透過性

1 回、`useRenderTextures`に設定されている`true`、透過的なカードを異なる方法でレンダリングされます。

![](ccrendertexture-images/image7.png "UseRenderTextures を設定する場合は true、透過的なカードには異なる方法でレンダリングされます。")

(左) と (右) なしのレンダリング テクスチャを使用して、透過的なロボットのカードを比較してみましょう。

![](ccrendertexture-images/image8.png "(右) なしのレンダリングのテクスチャ (左側) vs を使用した透過的なロボットのカードを比較します。")

最も明白な違いは詳細テキスト (明るい灰色ではなく黒) と、ロボット スプライト (暗色 light ではなく、再度が下げられた)。


## <a name="ccrendertexture-details"></a>CCRenderTexture の詳細

使用する利点を見たできた`CCRenderTexture`での使用方法を見てみましょう、`Card`エンティティ。

`CCRenderTexture`がキャンバスのレンダリングの対象にすることです。 ゲーム画面と比較した場合の 2 つの主な違いがあります。

1. `CCRenderTexture`中間フレームが引き続き発生します。 つまり、`CCRenderTexture`変更が発生したときにのみレンダリングする必要があります。 ここでは、`Card`エンティティ不変、1 回だけ表示されるようにします。 存在する場合`Card`コンポーネントが変更された後、カード自体に再描画する必要があります、`CCRenderTexture`します。 たとえば、HP 値 (正常性のポイント) が変更されたは、攻撃を受けるときに場合、しカードは新しい HP 値を反映するように自身をレンダリングする必要は。
1. `CCRenderTexture`ピクセル寸法は、画面に関連付けられていません。 A`CCRenderTexture`デバイスの解像度より大きくまたは小さくすることができます。 `Card`コードを作成、`CCRenderTexture`その背景スプライトのサイズを使用します。 カードにはへの参照が含まれています、`CCRenderTexture`と呼ばれる`renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture`インスタンスまま`null`まで、`UseRenderTexture`プロパティが true にすると、どの呼び出しに割り当てられている`SwitchToRenderTexture`:


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

`SwitchToRenderTexture`テクスチャを更新する必要があるたびに、メソッドを呼び出すことができます。 カードが既に使用しているかどうかに呼び出すことがその`CCRenderTexture`またはへの切り替えは、`CCRenderTexture`最初にします。

次のセクションでは、探索、`SwitchToRenderTexture`メソッド。 


### <a name="ccrendertexture-size"></a>CCRenderTexture サイズ

CCRenderTexture コンス トラクターでは、ディメンションの 2 つのセットが必要です。 最初のコントロールのサイズが、`CCRenderTexture`が描画し、2 つ目は、ピクセル幅とその内容の高さを指定します。 `Card`エンティティをインスタンス化、`CCRenderTexture`バック グラウンドを使用して[ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/)します。 ゲームが、 `DesignResolution` 512 で 384 のように`ViewController.LoadGame`iOS 上および`MainActivity.LoadGame`Android で。


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture`でコンス トラクターが呼び出されます、`background.ContentSize`として最初のパラメーターであることを示す、`CCRenderTexture`バック グラウンドだけのサイズである必要があります`CCSprite`します。 カードの背景から`CCSprite`高さ 200 ピクセル、カードの使用がほぼは画面の縦の高さの半分は。

渡される 2 番目のパラメーター、`CCRenderTexture`コンス トラクターのピクセルの解像度を指定します、`CCRenderTexture`します。 説明したように、 [CocosSharp 解決ガイド](~/graphics-games/cocossharp/resolutions.md)、ゲームの単位で表示可能領域の高さと幅は多くの場合、画面のピクセルの解像度と同じです。 同様に、ビジュアルが高解像度のデバイスで鮮明な表示されるように、CCRenderTexture はサイズより大きい解像度を使用可能性があります。

ピクセルの解像度は、見た目の汚いからテキストを防ぐために CCRenderTexture の 2 倍のサイズを示します。


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

比較するには、一致するように pixelResolution 値を変更できます、 `background.ContentSize` (せずにされている 2 倍になります) と、結果を比較します。 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "比較するには、背景に合わせて pixelResolution 値を変更できます。ContentSize 2 倍にすることがなく、結果の比較")


### <a name="rendering-to-a-ccrendertexture"></a>CCRenderTexture での表示

通常、CocosSharp でのビジュアル オブジェクトは、明示的にはレンダリングされません。 ビジュアル オブジェクトを追加する代わりに、`CCLayer`の一部では、`CCScene`します。 CocosSharp を自動的に表示、`CCScene`とすべてのフレームが呼び出されるレンダリング コードなしでそのビジュアルの階層。 

これに対し、`CCRenderTexture`を明示的に描画する必要があります。 この表示は、3 つの手順に分けることができます。

1. `CCRenderTexture.BeginWithClear` 呼び出されると、呼び出し元が後続のすべてのレンダリングを対象とすることを示す`CCRenderTexture`します。
1. オブジェクトから継承する`CCNode`(など、`Card`エンティティ) に表示されます、`CCRenderTexture`呼び出して`Visit`。
1. `CCRenderTexture.End` 呼び出されると、そのレンダリングすることを示す、`CCRenderTexture`が完了します。

任意の数のオブジェクトをレンダリングする、`CCRenderTexture`間その`Begin`と`End`呼び出し。 レンダリングの前に必要なすべての表示可能なオブジェクトは子として追加されます。


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

`renderTexture`することはできません、カードの一部を表示するときに削除されるようにします。


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

これで、`Card`インスタンスがレンダリングに、`CCRenderTexture`インスタンス。


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

レンダリングが完了したら、個々 のコンポーネントが削除、および`CCRenderTexture`再追加されます。


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

このガイドの説明、`CCRenderTexture`クラスを使用して、`Card`収集可能なカード ゲームで使用できるエンティティです。 使用する方法を示しましたが、`CCRenderTexture`フレーム レートを改善し、エンティティ全体の透明度を適切に実装するクラス。

このガイドの使用、`CCRenderTexture`エンティティ内に含まれている、このクラスに、複数のエンティティを表示するために使用されるかできます全体`CCLayer`画面全体に関わるおよびパフォーマンスの向上のインスタンス。

## <a name="related-links"></a>関連リンク

- [CCRenderTexture API リファレンス](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [完全なプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
