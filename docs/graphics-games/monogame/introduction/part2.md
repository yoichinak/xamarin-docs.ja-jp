---
title: パート 2-Walkinggame の実装
description: このチュートリアルでは、タッチ入力をゲーム ロジック、および空の MonoGame プロジェクトを使用した移動アニメーションのスプライトのデモを作成するコンテンツを追加する方法を示します。
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: c761f9f11e8053dcd0960129a28251ed6acf473c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120281"
---
# <a name="part-2--implementing-the-walkinggame"></a>パート 2-Walkinggame の実装

_このチュートリアルでは、タッチ入力をゲーム ロジック、および空の MonoGame プロジェクトを使用した移動アニメーションのスプライトのデモを作成するコンテンツを追加する方法を示します。_

このチュートリアルの前のパートでは、空の MonoGame プロジェクトを作成する方法を示しました。 ゲームの簡単なデモをこれら前のパートで構築されます。 この記事は、次のセクションで構成されています。

- 当社のゲームのコンテンツを解凍します。
- MonoGame クラスの概要
- 最初、スプライトをレンダリング
- CharacterEntity を作成します。
- ゲームに CharacterEntity を追加します。
- アニメーション クラスを作成します。
- CharacterEntity への最初のアニメーションの追加
- 文字への移動の追加
- 一致する移動とアニメーション


## <a name="unzipping-our-game-content"></a>当社のゲームのコンテンツを解凍します。

コードの記述を始める前に、ゲームを解凍します*コンテンツ*します。 ゲーム開発者は多くの場合、用語を使用して*コンテンツ*には、通常 visual アーティスト、ゲームのデザイナー、またはオーディオ デザイナーによって作成される非コード ファイルを参照してください。 一般的な種類コンテンツにはでは、ビジュアルを表示、サウンドを再生または人工知能 (AI) の動作を制御するために使用するファイルが含まれます。 ゲームの開発からチームのパースペクティブのコンテンツは通常、プログラマーが作成されます。

ここで使用されるコンテンツが見つかります[github](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)します。 これらのファイルにこのチュートリアルの後半でアクセスできる場所にダウンロードする必要があります。

## <a name="monogame-class-overview"></a>MonoGame クラスの概要

まず基本的なレンダリングで使用される MonoGame クラスをについて説明します。

- `SpriteBatch` – 2D グラフィックス、画面を描画するために使用します。 *スプライト*2D の visual 要素が画面にイメージを表示するために使用します。 `SpriteBatch`オブジェクト間で、一度に 1 つのスプライトを描画できるその`Begin`と`End`メソッド、または複数のスプライトは一緒にグループ化、または*バッチ*します。
- `Texture2D` 実行時にイメージ オブジェクトを表します。 `Texture2D` インスタンスは、これらも作成できますが動的に実行時に多くの場合、.png、.bmp などのファイル形式から作成されます。 `Texture2D` 表示するときにインスタンスが使用される`SpriteBatch`インスタンス。
- `Vector2` ビジュアル オブジェクトの配置のよく使用されている 2D 座標システムでの位置を表します。 MonoGame も含まれています。`Vector3`と`Vector4`にのみ使用が`Vector2`このチュートリアルでします。
- `Rectangle` – 位置、幅、および高さで 4 方向の領域。 使用するこののどの部分を定義する、`Texture2D`アニメーションで作業を始めるときに表示するためにします。

MonoGame がその名前空間に関係なく保持 microsoft – されません私たちも注意してください。 `Microsoft.Xna` MonoGame に既存の XNA プロジェクトを移行するが容易に MonoGame で名前空間が使用されます。

## <a name="rendering-our-first-sprite"></a>最初、スプライトをレンダリング

次に、1 つのスプライトを描画 MonoGame で 2D レンダリングを実行する方法を表示する画面にします。

### <a name="creating-a-texture2d"></a>Texture2D を作成します。

作成する必要があります、`Texture2D`スプライトをレンダリングするときに使用するインスタンス。 ゲームの内容をすべてが最終的にという名前のフォルダーに含まれている**コンテンツ、** プラットフォームに固有のプロジェクトに存在します。 コンテンツがプラットフォームに固有のビルド アクションを使用する必要があります、共有 MonoGame プロジェクトでは、コンテンツを含めることはできません。 CocosSharp の開発者は使い慣れた概念にコンテンツ フォルダーに検索-CocosSharp と MonoGame の両方のプロジェクト内の同じ場所に配置されています。 IOS のプロジェクト、および Android プロジェクトの Assets フォルダー内、コンテンツのフォルダーを確認できます。

ゲームのコンテンツを追加するを右クリックし、**コンテンツ**フォルダーと選択**追加 > ファイルを追加しています.** Content.zip ファイルが抽出された場所に移動し、選択、 **charactersheet.png**ファイル。 場合は、ファイルをフォルダーに追加する方法についてよく寄せられるを選択する必要があります、**コピー**オプション。

![](part2-images/image1.png "フォルダーにファイルを追加する方法について要求された場合は、コピー オプションを選択します。")

コンテンツのフォルダーには、今すぐ charactersheet.png ファイルが含まれています。

![](part2-images/image2.png "コンテンツのフォルダーが charactersheet.png ファイルを含むようになりました")

次に、読み込んで charactersheet.png ファイルを作成するためのコードを追加、`Texture2D`します。 開き、`Game1.cs`ファイルを開き、次のフィールドを Game1.cs クラスに追加します。

```csharp
Texture2D characterSheetTexture;
```

次に、作成が、`characterSheetTexture`で、`LoadContent`メソッド。 変更する前に`LoadContent`メソッドは次のようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

既定のプロジェクトが既にインスタンス化することに注意する必要があります、`spriteBatch`のインスタンス。 使用するこの後で、私たちを準備するためのコードが追加されませんが、`spriteBatch`使用します。 その一方で、当社`spriteSheetTexture`ため、初期化後は、初期化を必要とは、`spriteBatch`が作成されます。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

あるので、`SpriteBatch`インスタンスと`Texture2D`レンダリング コードを追加できますのインスタンス、`Game1.Draw`メソッド。

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

ここで、ゲームを実行するには、charactersheet.png から作成されたテクスチャを表示する 1 つのスプライトは示しています。

![](part2-images/image3.png "Charactersheet.png から作成されたテクスチャを表示する 1 つのスプライトを表示、ゲームを実行するようになりました")

## <a name="creating-the-characterentity"></a>CharacterEntity を作成します。

これまでに画面に 1 つのスプライトをレンダリングするコードを追加しましたただし、他の任意のクラスを作成することがなく完全なゲームを開発する場合、組織のコードの問題に実行が。

### <a name="what-is-an-entity"></a>エンティティとは何ですか。

ゲームのコードを整理するための一般的なパターンは、各ゲームの新しいクラスを作成する*エンティティ*型。 ゲーム開発のエンティティは、(必要なすべてではなく、)、次の特性の一部を含めることができるオブジェクトを示します。

- スプライト、テキスト、または 3D モデルなどのビジュアル要素
- 物理運動またはパスの設定または入力に応答して player 文字哨戒単位などのすべてのフレーム動作
- 作成し、破棄などの電源を使用して、プレーヤーによって収集されるデータを動的にできます

エンティティの組織のシステムが複雑になることができ、多くのゲーム エンジンは、エンティティを管理するためにクラスを提供します。 私たちがするシステムを実装する非常に単純なエンティティ、ため、完全なゲームは、開発者の複数の組織を通常必要がありますに注意します。


### <a name="defining-the-characterentity"></a>CharacterEntity を定義します。

このエンティティを呼び出すこと`CharacterEntity`、必要で、次の特性があります。

- 独自にロードする機能 `Texture2D`
- ウォークのアニメーションを更新するメソッドを呼び出し元を格納しているなど、自身をレンダリングする機能
- `X` および Y プロパティを文字の位置を制御します。
- 具体的には – 自体を更新する、タッチからの値は、画面し、位置を適切に調整を読み取る権限です。

追加する、 `CharacterEntity` 、ゲーム、右クリックして、またはコントロールをクリックして、 **WalkingGame**順に選択して**追加 > 新しいファイル.**.選択、**空のクラス**オプションし、新しいファイルに名前を**CharacterEntity**、 をクリックし、**新規**します。

最初の機能を追加します、`CharacterEntity`を読み込む、`Texture2D`も自体を描画します。 新しく追加された変更`CharacterEntity.cs`ファイルの次のようにします。

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

上記のコードは、配置、描画、およびコンテンツの読み込みの役割を追加します。、`CharacterEntity`します。 上記のコードで実行されるいくつかの考慮事項を指摘してみましょう。

### <a name="why-are-x-and-y-floats"></a>X である理由と Y の浮動小数点数。

ゲームのプログラミングに慣れていない開発者はなぜだろうことがあります、`float`とは対照的に使用されている型`int`または`double`します。 理由は、32 ビット値が低レベル レンダリング コードでの配置に最も一般的なことです。 Double 型では、精度は、オブジェクトの位置を指定する必要はほとんどなく 64 ビットを占有します。 32 ビットの違いは、意味のないように思える可能性があります、多くの最新のゲームは (30、または 1 秒あたり 60 回) は、各フレーム数万の位置の値の処理を必要とするグラフィックスを含めます。 メモリの量を切り取り、移動する必要がありますグラフィックを半分のパイプラインは、ゲームのパフォーマンスに大きな影響を与えることができます。

`int`データ型は、適切な位置を指定すると、測定単位を指定できますが、そのエンティティが配置されている方法での制限事項を配置できます。 たとえば、整数値を使用してより困難ズームまたは 3D カメラをサポートするゲームの動きが滑らかに実装するために (で許可されている`SpriteBatch`)。

最後に、ゲームのタイミング値を使用してこれを文字を画面に移動して、ロジックでが表示されます。 使用する必要があります、特定のフレームにどれ時間だけが経過して速度を乗算した結果は、整数で結果ことはほとんどありません`float`の`X`と`Y`します。

### <a name="why-is-charactersheettexture-static"></a>CharacterSheetTexture を理由には静的ですか?

`characterSheetTexture` `Texture2D`インスタンスが、charactersheet.png ファイルのランタイム表現。 各ピクセルの色の値を含む、つまり、ソースから抽出された**charactersheet.png**ファイル。 ゲームが 2 つ作成する場合`CharacterEntity`インスタンスそれぞれ必要なアクセス情報を charactersheet.png します。 このような状況で charactersheet.png を 2 回、読み込みのパフォーマンス コストが発生する必要はありません。 また ram に格納されている重複するテクスチャのメモリが必要な場合します。 使用して、`static`フィールドでは、同じ共有により`Texture2D`すべて`CharacterEntity`インスタンス。

使用して`static`コンテンツのランタイム表現のメンバーは、単純なソリューションとアンロードのコンテンツなどの大規模なゲームで 1 つのレベル間を移動するときに発生する問題は対応していません。 このチュートリアルの範囲を超えているより高度なソリューションには、MonoGame のコンテンツ パイプラインを使用して、またはカスタムのコンテンツ管理システムを作成するが含まれます。

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>なぜ、引数 Instead of によってインスタンス化、エンティティとして SpriteBatch に渡されるですか。

`Draw`メソッド上では実装されている、`SpriteBatch`は対照的に、引数、`characterSheetTexture`によってインスタンス化、`CharacterEntity`します。

この理由は、最も効率的なレンダリングする可能性があります同じ`SpriteBatch`すべてのインスタンスが使用される`Draw`呼び出しとタイミングすべて`Draw`呼び出しに対して 1 つの間で行われた`Begin`と`End`呼び出し。 もちろん、ゲームには、1 つのエンティティのインスタンスにはのみが含まれますより複雑なゲームが複数のエンティティを使用して、同じパターンから得られる`SpriteBatch`インスタンス。


## <a name="adding-characterentity-to-the-game"></a>ゲームに CharacterEntity を追加します。

これで追加されました、`CharacterEntity`自体をレンダリングするためのコードでは、コードを置き換えることができます`Game1.cs`この新しいエンティティのインスタンスを使用します。 置き換えることでこれを行う、`Texture2D`フィールドに、`CharacterEntity`フィールドに`Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

次に、このエンティティのインスタンス化を追加`Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

削除する必要もあります、`Texture2D`から作成`LoadContent`はここでの内部で処理されるため、`CharacterEntity`します。 `Game1.LoadContent` このようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

値しますを名前とは異なり、`LoadContent`メソッドは、コンテンツを読み込むことができます: コンテンツの多くのゲーム実装またはコンテンツとして、更新プログラムのコードであっても、Initialize メソッドでの読み込みは必要ありません、プレーヤーに到達するまでは、唯一の場所ではありません、ゲームの特定の位置。

最後に、Draw メソッドを次のように変更できます。


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

ゲームを実行した場合に、文字表示されます。 X と Y を 0 に既定では後、は、画面の左上隅に対して文字が配置します。

![](part2-images/image4.png "X と Y を 0 に既定では後の文字が画面の左上隅に対して配置します。")

## <a name="creating-the-animation-class"></a>アニメーション クラスを作成します。

現在、`CharacterEntity`完全な表示**charactersheet.png**ファイル。 この 1 つのファイルで複数のイメージの配置と呼びます、*スプライト シート*します。 通常、スプライトはスプライト シートの部分のみに表示されます。 変更、`CharacterEntity`これの一部を表示するために**charactersheet.png**、徒歩アニメーションを表示する時間の経過と共にこの部分が変更されます。

作成、`Animation`ロジックと CharacterEntity アニメーションの状態を制御するクラス。 アニメーションのクラスだけでなく、任意のエンティティに対して使用できる一般的なクラスになります`CharacterEntity`アニメーション。 Ultimate、`Animation`クラスが用意されて、`Rectangle`を`CharacterEntity`自体を描画するときに使用されます。 作成して、`AnimationFrame`アニメーションを定義するために使用するクラス。


### <a name="defining-animationframe"></a>AnimationFrame を定義します。

`AnimationFrame` アニメーションに関連するすべてのロジックは含まれません。 使用する、データを格納するだけです。 追加する、`AnimationFrame`クラス、右クリックして、またはコントロールをクリックして、 **WalkingGame**共有プロジェクトと選択**追加 > 新しいファイル.** 名前を入力します**AnimationFrame**  をクリックし、**新規**ボタンをクリックします。 変更します。、`AnimationFrame.cs`次のコードが含まれているように、ファイル。


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame`クラスには、2 つの情報が含まれています。

- `SourceRectangle` – の領域を定義する、`Texture2D`によって表示される、`AnimationFrame`します。 この値は、上部左側が (0, 0) をピクセル単位で測定されます。
- `Duration` – 定義どのくらいの期間、`AnimationFrame`で使用されるときに表示される、`Animation`します。

### <a name="defining-animation"></a>アニメーションを定義します。

`Animation`クラスが含まれて、`List<AnimationFrame>`をどれ時間だけが経過に従って現在表示されているどのフレームを切り替えるロジックとします。

追加する、`Animation`クラス、右クリックして、またはコントロールをクリックして、 **WalkingGame**共有プロジェクトと選択**追加 > 新しいファイル.**.名前を入力します**アニメーション** をクリックし、**新規**ボタンをクリックします。 変更します。、`Animation.cs`ファイルの次のコードが含まれるようにします。


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

いくつかの詳細を見てみましょう、`Animation`クラス。

### <a name="frames-list"></a>フレーム一覧

`frames`メンバーは、アニメーションのデータを格納とします。 アニメーションをインスタンス化するコードは追加`AnimationFrame`インスタンスを`frames`を一覧表示、`AddFrame`メソッド。 詳細な実装を提供できます`public`メソッドまたはプロパティの変更を`frames`が、このチュートリアルではフレームを追加するための機能が制限されています。

### <a name="duration"></a>存続期間

実行時間の合計継続時間を返します、`Animation,`が格納されているすべての期間を追加することによって取得される`AnimationFrame`インスタンス。 場合、この値をキャッシュしたり`AnimationFrame`変更不可のオブジェクトが AnimationFrame をアニメーションに追加された後に変更できるクラスとして実装しましたので、プロパティがアクセスされるたびに、この値を計算する必要があります。

### <a name="update"></a>更新

`Update`メソッドにすべてのフレーム (つまり、毎回ゲーム全体の更新) を呼び出す必要があります。 その目的は、増やすこと、`timeIntoAnimation`メンバーを現在表示されているフレームを返すために使用します。 ロジックでは、`Update`により、`timeIntoAnimation`がこれまで、アニメーション全体の期間よりも大きい。

使用するため、`Animation`クラスをこのアニメーションの末尾に達した場合に、繰り返しが必要な徒歩アニメーションを表示します。 これをチェックすることで実現しましたできます、`timeIntoAnimation`よりも大きい、`Duration`値。 その場合、先頭に戻る、ループのアニメーションで、残りの部分が保持されます。

### <a name="obtaining-the-current-frames-rectangle"></a>現在のフレームの四角形を取得します。

目的、`Animation`を提供するクラスは、最終的に、`Rectangle`スプライトを描画するときに使用できます。 現在、`Animation`クラスには変更するためのロジックが含まれています、`timeIntoAnimation`を使用して、取得するメンバー`Rectangle`が表示されます。 作成してこれは、`CurrentRectangle`プロパティ、`Animation`クラス。 このプロパティにコピーして、`Animation`クラス。

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>CharacterEntity への最初のアニメーションの追加

`CharacterEntity`ウォーキングし継続、だけでなく、現在への参照のアニメーションを含む`Animation`表示されています。

最初に、最初が追加`Animation,`これまでに記述するように、機能をテストするのに使用します。 CharacterEntity クラスに次のメンバーを追加しましょう。

```csharp
Animation walkDown;
Animation currentAnimation;
```

次に、定義、`walkDown`アニメーション。 この変更を行う、`CharacterEntity`次のようにコンス トラクター。

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

呼び出す必要があります前に述べた`Animation.Update`時間ベースのアニメーションを再生します。 割り当てる必要もあります、`currentAnimation`します。 ようになりましたを割り当てます、`currentAnimation`に`walkDown`が、私たちが置き換えられるこのコード後で、移動ロジックを実装します。 追加、`Update`メソッドを`CharacterEntity`次のようにします。


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

あるので、`currentAnimation`されて割り当てられ、更新、当社の描画の実行に使用できます。 変更します。`CharacterEntity.Draw`次のようにします。

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

取得する最後の手順、`CharacterEntity`をアニメーション化は、新しく追加した`Update`メソッドから`Game1`します。 変更`Game1.Update`次のようにします。

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

これで、`CharacterEntity`再生はその`walkDown`アニメーション。

![](part2-images/image5.gif "CharacterEntity がその walkDown アニメーションを再生するようになりました")

## <a name="adding-movement-to-the-character"></a>文字への移動の追加

次を追加する予定の動きをタッチ コントロールを使用して、文字。 ユーザーが画面をタッチすると、文字を画面にタッチ点に向かって移動します。 タッチが検出されない場合、文字は、インプレース スタンドは。

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput を定義します。

使用する MonoGame の`TouchPanel`クラスで、タッチ スクリーンの現在の状態に関する情報を提供します。 チェックするメソッドを追加してみましょう、`TouchPanel`戻って、文字の必要なベロシティ。


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState`メソッドを返します。 を`TouchCollection`、ユーザーが画面に触れて、に関する情報が含まれます。 ユーザーは、画面に触れていない場合、`TouchCollection`は空にする、文字が進むべきではない場合。

場合は、ユーザーが画面に触れるに移動に対する最初のタッチ文字言い換えると、`TouchLocation`インデックス 0 位置にあります。 最初に、文字の場所と最初のタッチの位置の違いと同じに必要な速度を設定します。

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

数値演算と同じ速度で移動する文字を保持するを次に示します。 これが重要な理由を説明するには、ユーザーが画面の 500 ピクセル、文字がある場所から離れた場所をタッチが状況について考えてみましょう。 最初の行位置`desiredVelocity.X`はセットで 500 の値を割り当てるとします。 ただし、文字から 100 単位の距離で画面を接触していた場合は、次に、`desiredVelocity.X `は 100 に設定されます。 結果は、文字の移動速度は応答する方法を離れたタッチ ポイントは、文字からになります。 常に同じ速度で移動する、文字、たいので、desiredVelocity を変更する必要があります。

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)`ユーザーが文字の現在の位置と同じ場所に触れていないことを確認するをチェックしている場合は、velocity 非 0 – つまり、ステートメントを確認します。 タッチを離れた方法に関係なく定数である文字の速度を設定する必要がありますし、そうでない場合です。 長さが 1 になることで結果が速度ベクターを正規化することでこれを実現します。 1 は、文字が 1 秒あたり 1 ピクセルに移動する速度ベクター。 200 の必要な速度で値を乗算して速度します。


### <a name="applying-velocity-to-position"></a>位置に Velocity を適用します。

返された速度`GetDesiredVelocityFromInput`文字に適用する必要がある`X`と`Y`実行時に影響する値。 変更します。、`Update`メソッドとして、次のとおりです。

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

ここで実装したと呼びます*時間に基づく*移動 (ではなく*フレーム ベース*移動) します。 時間ベースの移動速度の値を乗算します (ここで、値が格納されている、`velocity`変数) に格納されている最後の更新からどれ時間だけが経過して`gameTime.ElapsedGameTime.TotalSeconds`します。 ゲームは、1 秒あたりのフレーム数が少ないで実行する場合は、フレーム間の経過時間が上昇 – 最終的な結果は、フレーム レートに関係なく同じ速度で移動に時間を使用してオブジェクトが常に移動します。

ゲームを今すぐ実行文字が、タッチ位置に向かっています表示されます。

![](part2-images/image6.gif "文字がタッチの場所に移動します。")

## <a name="matching-movement-and-animation"></a>一致する移動とアニメーション

移動し、1 つのアニメーションを再生する、文字取得したら、アニメーションの残りの部分を定義し、キャラクターの動きを反映するために使用できます。 ときに完了に、8 つのアニメーションの合計。

- ウォーク下、左、および右のアニメーション
- 継続のアニメーションがまだし、上向きにして下、左、および右

### <a name="defining-the-rest-of-the-animations"></a>アニメーションの残りの部分を定義します。

最初に追加します、`Animation`インスタンスを`CharacterEntity`、アニメーションを追加、同じ場所でのすべてのクラス`walkDown`します。 この後、`CharacterEntity`次のものが`Animation`メンバー。

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

アニメーションを定義しますので、`CharacterEntity`次のようにコンス トラクター。

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

上記のコードに追加されたことに注意する必要があります、`CharacterEntity`このチュートリアルの短いコンス トラクター。 ゲーム通常をキャラクターのアニメーションに独自のクラスの定義を分割または XML や JSON などのデータ形式からこの情報をロードします。

次に、文字が単に停止した場合、文字を移動すると、方向、または最後のアニメーションに従って、アニメーションを使用するロジックが調整されます。 これを行うには、変更しますが、`Update`メソッド。


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

アニメーションを切り替えるコードは、2 つのブロックに分割されます。 最初は、velocity が等しくないかどうかをチェック`Vector2.Zero`– 文字が移動するかどうかを表します。 かどうかには、文字が、移動を見て、`velocity.X`と`velocity.Y`徒歩を再生するアニメーションを決定する値。

文字が移動していないかどうかは、文字を設定する必要`currentAnimation`継続アニメーション – に場合のみ行っていますが、`currentAnimation`をウォークは、アニメーションやアニメーションが設定されていないかどうか。 場合、`currentAnimation`が 4 つのウォークのアニメーションのいずれかを変更する必要はありませんので、文字が立って既に`currentAnimation`します。

このコードの結果は、こと、文字は正しくウォーク、ときにアニメーション化して、最後に停止するときに、ウォーキングが方向に直面しには。

![](part2-images/image7.gif "このコードの結果は、文字が正しくウォーク、ときにアニメーション化して最後に停止するときに、ウォーキングが方向に直面し、")

## <a name="summary"></a>まとめ

このチュートリアルは、スプライト、オブジェクトの移動、入力検出、およびアニメーションのゲームのクロスプラット フォームを作成する MonoGame を使用する方法を説明しました。 汎用のアニメーション クラスの作成について説明します。 また、コード ロジックを整理するための文字エンティティを作成する方法も示しました。

## <a name="related-links"></a>関連リンク

- [CharacterSheet イメージ リソース (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [ウォーク ゲーム完了 (サンプル)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
