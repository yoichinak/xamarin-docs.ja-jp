---
title: パート 2 –、WalkingGame を実装します。
description: このチュートリアルでは、ゲーム ロジックおよびコンテンツの移動、アニメーション化されたスプライトのデモを作成する空の MonoGame プロジェクト タッチ入力を追加する方法を示します。
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bc4ab2e77bfce9c9ba6043533bcfda5a359d322e
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="part-2--implementing-the-walkinggame"></a>パート 2 –、WalkingGame を実装します。

_このチュートリアルでは、ゲーム ロジックおよびコンテンツの移動、アニメーション化されたスプライトのデモを作成する空の MonoGame プロジェクト タッチ入力を追加する方法を示します。_

このチュートリアルの前の部分では、空の MonoGame プロジェクトを作成する方法を示しました。 ここではゲームの簡単なデモをすることでこれらの前の部分に構築します。 この記事は、次のセクションで構成されています。

- 解凍、ゲームの内容
- MonoGame クラスの概要
- 最初、スプライトのレンダリング
- CharacterEntity を作成します。
- ゲームに CharacterEntity を追加します。
- アニメーションのクラスを作成します。
- CharacterEntity への最初のアニメーションの追加
- 文字への移動の追加
- 一致する動きとアニメーション


## <a name="unzipping-our-game-content"></a>解凍、ゲームの内容

コードの記述を始める前にたいと考えて、ゲームを解凍*コンテンツ*です。 ゲームの開発者は多くの場合、用語を使用して*コンテンツ*には、通常 visual アーティスト、ゲームのデザイナーやオーディオ デザイナーによって作成されるコード以外のファイルを参照してください。 一般的なコンテンツの種類には、ビジュアルを表示、音を再生または人工知能 (AI) の動作を制御するために使用するファイルが含まれます。 ゲームの開発からチームのパースペクティブのコンテンツは通常プログラマ以外が作成されます。

ここで使用されるコンテンツが見つかります[github](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)です。 これらのファイルにこのチュートリアルの後半でアクセスできる場所にダウンロードする必要があります。

## <a name="monogame-class-overview"></a>MonoGame クラスの概要

まず基本的なレンダリングで使用される MonoGame クラスについて学びます。

- `SpriteBatch` – 2 次元グラフィックスを画面に描画するために使用します。 *スプライト*が画面にイメージを表示するために使用する 2D のビジュアル要素。 `SpriteBatch`オブジェクト間で一度に単一スプライトを描画できるその`Begin`と`End`メソッド、または複数のスプライトまとめることができ、または*バッチ*です。
- `Texture2D` 実行時にイメージ オブジェクトを表します。 `Texture2D` インスタンスは、それらも作成できますが動的に実行時に多くの場合、.png、.bmp などのファイル形式から作成されます。 `Texture2D` 表示するときにインスタンスが使用される`SpriteBatch`インスタンス。
- `Vector2` – visual オブジェクトの配置のよく使用されている 2D 座標システムでの位置を表します。 MonoGame も含まれています。`Vector3`と`Vector4`にのみ使用が`Vector2`このチュートリアルでします。
- `Rectangle` – 位置、幅、および高さで 4 方向の領域。 使用するこののどの部分を定義する、`Texture2D`アニメーションを使用して作業を始めるときに表示するためにします。

MonoGame がその名前空間に関係なく維持 microsoft – されませんおも注意してください。 `Microsoft.Xna`名前空間に移行する既存のプロジェクトが XNA MonoGame やすく MonoGame で使用します。

## <a name="rendering-our-first-sprite"></a>最初、スプライトのレンダリング

次に単一のスプライトお描画 MonoGame で 2D レンダリングを実行する方法を表示する画面にします。

### <a name="creating-a-texture2d"></a>テクスチャ 2d の作成

作成する必要があります、`Texture2D`スプライトを表示するときに使用するインスタンス。 すべてのゲームの内容は最終的にという名前のフォルダーに含まれている**コンテンツ、**プラットフォーム固有のプロジェクトに存在します。 コンテンツは、プラットフォームに固有のビルド アクションを使用する必要があります、MonoGame 共有プロジェクトは、コンテンツを含めることはできません。 CocosSharp 開発者は使い慣れた概念にコンテンツ フォルダーに検索 – ファイルは CocosSharp と MonoGame の両方のプロジェクトと同じ場所に配置します。 プロジェクトで、iOS、Android プロジェクトの Assets フォルダー内のコンテンツのフォルダーを確認できます。

ゲームの内容を追加するを右クリックし、**コンテンツ**フォルダーと選択**追加 > ファイルを追加しています.**Content.zip ファイルが抽出された場所に移動し、選択、 **charactersheet.png**ファイル。 フォルダーにファイルを追加する方法の場合を選択する必要があります、**コピー**オプション。

![](part2-images/image1.png "フォルダーにファイルを追加する方法の場合、[コピー] オプションを選択します。")

コンテンツのフォルダーには、今すぐ charactersheet.png ファイルが含まれています。

![](part2-images/image2.png "コンテンツのフォルダーに今すぐ charactersheet.png ファイルが含まれています")

次に、charactersheet.png ファイルを読み込み、作成するコードを追加、`Texture2D`です。 開いて、`Game1.cs`ファイルおよび Game1.cs クラスに次のフィールドを追加します。

```csharp
Texture2D characterSheetTexture;
```

次に、作成を`characterSheetTexture`で、`LoadContent`メソッドです。 変更を加える前に`LoadContent`メソッドは、次のようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

既定のプロジェクトが既にインスタンス化することに注意する必要があります、`spriteBatch`ご利用の米国のインスタンス。 使用するこの後は準備するためのコードが追加されませんが、`spriteBatch`使用します。 その一方で、当社`spriteSheetTexture`初期化、しなお初期化後は、`spriteBatch`が作成されます。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

これでが、`SpriteBatch`インスタンスおよび`Texture2D`レンダリングようコードを追加できるインスタンス、`Game1.Draw`メソッド。

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

ここで、ゲームを実行するには、charactersheet.png から作成されたテクスチャを表示する 1 つスプライトは示しています。

![](part2-images/image3.png "Charactersheet.png から作成されたテクスチャを表示する 1 つスプライトを示しています、ゲームを実行するようになりました")

## <a name="creating-the-characterentity"></a>CharacterEntity を作成します。

これまでに画面に単一スプライトをレンダリングするコードが追加されましたただし、他のクラスを作成することがなく完全ゲームを開発する場合、コードの問題を組織に実行はします。

### <a name="what-is-an-entity"></a>エンティティとは何ですか。

各ゲームの新しいクラスを作成するのには、ゲーム コードを整理するための一般的なパターン*エンティティ*型です。 ゲーム開発のエンティティは、(必要なすべてではありませんが)、次の特性の一部を含めることができるオブジェクトを示します。

- スプライト、テキスト、または 3D モデルなどの視覚的要素
- 物理またはパスを設定または入力に応答して player 文字哨戒単位などのすべてのフレーム動作
- 作成し、破棄を動的になどの電源投入が表示されると、player によって収集されることができます

多くのゲーム エンジンは、エンティティを管理するためにクラスを提供して、エンティティの組織のシステムを複雑なことができます。 おされますを実装する非常に単純なエンティティ システムでは、完全ゲームは、開発者の多くの組織を通常が必要な注目に値しますようにします。


### <a name="defining-the-characterentity"></a>CharacterEntity を定義します。

このエンティティは、とします`CharacterEntity`、次の特性を持ちます。

- 独自に読み込む機能 `Texture2D`
- 含まれているウォークのアニメーションを更新する呼び出し元のメソッドを含む自体を表示する機能
- `X` および Y プロパティを文字の位置を制御します。
- – 自ら更新を具体的には、タッチからの値が画面や位置を適切に調整を読み取る権限です。

追加する、 `CharacterEntity` 、ゲーム、右クリックまたはコントロールをクリックする、 **WalkingGame**プロジェクトし、選択**追加 > 新しいファイル.**.選択、**空のクラス**オプションし、新しいファイルの名前を付けます**CharacterEntity**、順にクリックして**新規**です。

まず追加の機能、`CharacterEntity`を読み込む、`Texture2D`も、自身で描画します。 修正するので、新たに追加した`CharacterEntity.cs`ファイルの次のようにします。

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

上記のコードを追加、配置、表示、およびコンテンツの読み込みの責任において、`CharacterEntity`です。 上記のコードで実行されるいくつかの考慮事項を指摘する時点を見てみましょう。

### <a name="why-are-x-and-y-floats"></a>理由は X と Y の浮動小数点値しますか?

ゲームのプログラミングに慣れている開発者が不思議可能性があります、`float`はなく型が使用されている`int`または`double`です。 その理由は、32 ビット値が低水準のレンダリング コードでの配置に最も一般的なことです。 Double 型では、64 ビットの有効桁数は、オブジェクトを配置する必要はほとんどなくを占有します。 32 ビットの違いは、意味のないように思える可能性があります、多くの最新のゲームは 30 (1 秒あたり 60 時間) は、各フレーム数万人の位置の値の処理を必要とするグラフィックスを含めます。 メモリの量を切り取り、移動する必要がありますグラフィックを半分のパイプラインは、ゲームのパフォーマンスに大きな影響を与えることができます。

`int`データ型は、適切な位置の測定単位を指定できますが、エンティティが配置されている方法に制限を加えることできます。 たとえば、整数値を使用してより難しくズーム インまたは 3D のカメラをサポートするゲーム用の動きが滑らかを実装する (で許可されている`SpriteBatch`)。

最後に、こと、ロジック、文字、画面の周りに移動するためには、ゲームのタイミング値を使用して表示されます。 フレーム内にどのくらい時間が経過してベロシティを乗算した結果まれになります、整数を使用する必要がありますので`float`の`X`と`Y`です。

### <a name="why-is-charactersheettexture-static"></a>CharacterSheetTexture をなぜは静的ですか?

`characterSheetTexture` `Texture2D`インスタンス charactersheet.png ファイルのランタイム表現であります。 つまりが含まれている各ピクセルの色の値のソースから抽出された**charactersheet.png**ファイル。 ゲームが 2 つ作成する場合`CharacterEntity`インスタンスを 1 つずつとする必要がありますアクセス情報 charactersheet.png からです。 このような状況で charactersheet.png を 2 回、読み込みのパフォーマンス コストが発生する必要はありません。 また ram に格納されている重複したテクスチャ メモリが必要なは。 使用して、`static`フィールドにより、同じ共有を`Texture2D`すべてにわたって`CharacterEntity`インスタンス。

使用して`static`コンテンツのランタイム表現のメンバーは、単純なソリューションと、1 つのレベル間を移動するときにアンロード コンテンツなどの大規模なゲームで発生する問題は対応していません。 高度なソリューションは、このチュートリアルの範囲を超えているなど MonoGame のコンテンツのパイプラインを使用してカスタムのコンテンツ管理システムを作成します。

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>なぜ、引数 Instead of によってインスタンス化、エンティティとして SpriteBatch が渡されるですか。

`Draw`メソッド上では実装されている、`SpriteBatch`がこれに対し、引数、`characterSheetTexture`によってインスタンス化、`CharacterEntity`です。

その理由は、最も効率的なレンダリング可能性があるため場合に、同じ`SpriteBatch`インスタンスはすべての使用`Draw`呼び出しとタイミングすべて`Draw`1 セットの間の呼び出しが作成されている`Begin`と`End`呼び出しです。 もちろん、ゲームには、1 つのエンティティのインスタンスにはのみが含まれますより複雑なゲームは、同じを使用する複数のエンティティを許可するパターンを利用`SpriteBatch`インスタンス。


## <a name="adding-characterentity-to-the-game"></a>ゲームに CharacterEntity を追加します。

追加されました。 これで、`CharacterEntity`自体を表示するためのコードでは、内のコードを置き換えることができます`Game1.cs`この新しいエンティティのインスタンスを使用します。 置き換えることでこれを行うには、`Texture2D`フィールドに、`CharacterEntity`フィールドに`Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

次に、追加でこのエンティティのインスタンス化`Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

削除する必要があります、`Texture2D`から作成`LoadContent`内の処理するようになりましたので、`CharacterEntity`です。 `Game1.LoadContent` 次のようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

注意が必要する名前とは異なり、`LoadContent`メソッドは、コンテンツを読み込むことができます: コンテンツ多くのゲームを実装またはコンテンツとその更新プログラムのコード内で、Initialize メソッドでの読み込みは必要ありません、プレーヤーに到達するまでは、唯一の場所ではありません、ゲームのポイントが特定されます。

最後に描画メソッドをよう変更おことができます。


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

ゲームを実行する場合は、文字に表示されます。 X と Y の既定で 0 に後、画面の左上隅に対して文字が位置指定します。

![](part2-images/image4.png "X と Y の既定で 0 に後の文字が、画面の左上隅に対して配置します。")

## <a name="creating-the-animation-class"></a>アニメーションのクラスを作成します。

現在、`CharacterEntity`全体が表示されます**charactersheet.png**ファイル。 この 1 つのファイルに複数のイメージの配置と呼びます、*スプライト シート*です。 通常、スプライト スプライト シートの部分のみで表示されます。 修正するので、`CharacterEntity`のこの部分を表示するために**charactersheet.png**、ウォーク アニメーションを表示する時間の経過と共にこの部分は変更します。

作成、`Animation`ロジックおよび CharacterEntity アニメーションの状態を制御するクラス。 アニメーションなるだけでなく、エンティティを使用できる一般的なクラス`CharacterEntity`アニメーション。 Ultimate、`Animation`クラスが用意されて、`Rectangle`を`CharacterEntity`自体を描画するときに使用されます。 作成して、`AnimationFrame`アニメーションを定義するために使用するクラス。


### <a name="defining-animationframe"></a>AnimationFrame を定義します。

`AnimationFrame` アニメーションに関連する任意のロジックは含まれません。 使用することのみデータを格納します。 追加する、`AnimationFrame`クラス、右クリックまたはコントロールをクリック、 **WalkingGame**共有プロジェクトと選択**追加 > 新しいファイル.**名前を入力します**AnimationFrame**  をクリックし、**新規**ボタンをクリックします。 変更して、`AnimationFrame.cs`ファイルに次のコードが含まれています。


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame`クラスには、2 つ情報にはが含まれています。

- `SourceRectangle` – の領域を定義する、`Texture2D`によって表示される、`AnimationFrame`です。 この値は、上部左中 (0, 0) をピクセル単位で計測されます。
- `Duration` – を定義する時間、`AnimationFrame`で使用されるときに表示される、`Animation`です。

### <a name="defining-animation"></a>アニメーションを定義します。

`Animation`クラスに含まれる、`List<AnimationFrame>`どのくらい時間が経過に従って現在表示されているフレームを切り替えるには、ロジックとします。

追加する、`Animation`クラス、右クリックまたはコントロールをクリック、 **WalkingGame**共有プロジェクトと選択**追加 > 新しいファイル.**.名前を入力します**アニメーション** をクリックし、**新規**ボタンをクリックします。 変更して、`Animation.cs`ファイルの次のコードが含まれているようにします。


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

詳細のいくつか見ておきましょう、`Animation`クラスです。

### <a name="frames-list"></a>フレーム リスト

`frames`メンバーは、アニメーション用のデータを格納する新機能です。 アニメーションをインスタンス化するコードを追加`AnimationFrame`インスタンスを`frames`を一覧表示、`AddFrame`メソッドです。 完全な実装を提供できます`public`メソッドまたはプロパティを変更する`frames`、このチュートリアル用のフレームを追加するための機能が制限されていますが、します。

### <a name="duration"></a>存続期間

返しますの総実行時間の期間、`Animation,`が格納されているすべての期間を追加することによって取得される`AnimationFrame`インスタンス。 場合、この値をキャッシュできなかった`AnimationFrame`されましたが、変更できないオブジェクトがアニメーションに追加された後に変更することができますをクラスとして AnimationFrame を実装しましたので、プロパティにアクセスするたびに、この値を計算する必要があります。

### <a name="update"></a>更新

`Update`メソッドに (つまり、たびにゲーム全体が更新される) すべてのフレームを呼び出す必要があります。 その目的を増やすには、`timeIntoAnimation`現在表示されているフレームを返すために使用するメンバー。 ロジックでは、`Update`により、`timeIntoAnimation`を持ち、全体のアニメーションの時間よりも大きいです。

使用するので、`Animation`クラスをこのアニメーションの末尾に到達した場合に繰り返し表示が必要なウォーク アニメーションを表示します。 これを行う場合にチェックして、`timeIntoAnimation`よりも大きい、`Duration`値。 そのような場合が先頭に戻ると、ループのアニメーションで結果の剰余を保持します。

### <a name="obtaining-the-current-frames-rectangle"></a>現在のフレームの四角形を取得します。

目的、`Animation`を提供するクラスが最終的には、`Rectangle`スプライトを描画するときに使用できます。 現在、`Animation`クラスには、変更するためのロジックが含まれています、`timeIntoAnimation`を取得する際に使用するメンバー`Rectangle`が表示されます。 それでは、作成することで、`CurrentRectangle`プロパティに、`Animation`クラスです。 このプロパティにコピーして、`Animation`クラス。

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>CharacterEntity への最初のアニメーションの追加

`CharacterEntity`ウォーキング継続との参照を現在のアニメーションが含まれます`Animation`表示されています。

最初に、追加最初`Animation,`これまでに書き込まれるように機能をテストする際に使用します。 みましょう CharacterEntity クラスに次のメンバーを追加します。

```csharp
Animation walkDown;
Animation currentAnimation;
```

次に、定義、`walkDown`アニメーション。 この変更を行うには、`CharacterEntity`次のようにコンス トラクター。

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

前述のようを呼び出す必要があります`Animation.Update`時間ベースのアニメーションを再生します。 またを割り当てる必要があります、`currentAnimation`です。 割り当てます今すぐに、`currentAnimation`に`walkDown`、おありますによって置き換えられる次のコード後で、移動ロジックを実装しましたが、します。 追加、`Update`メソッドを`CharacterEntity`次のようにします。


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

これでが、`currentAnimation`されて割り当てられ、更新、これを使用して、描画を実行します。 コラム`CharacterEntity.Draw`次のようにします。

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

最後の手順を取得する、`CharacterEntity`を呼び出して、新しく追加したがアニメーション化`Update`メソッドから`Game1`です。 変更`Game1.Update`次のようにします。

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

これで、`CharacterEntity`再生がその`walkDown`アニメーション。

![](part2-images/image5.gif "CharacterEntity がその walkDown アニメーションを再生するようになりました")

## <a name="adding-movement-to-the-character"></a>文字への移動の追加

次に、追加されます。 移動タッチ コントロールを使用して、文字にします。 ユーザーが画面に触れたときに文字が画面がタッチされるポイントに移動します。 仕上げが検出されない場合、文字はインプレース スタンドされます。

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput を定義します。

使用する MonoGame の`TouchPanel`クラスで、タッチ スクリーンの現在の状態に関する情報を提供します。 チェックするメソッドを追加してみましょう。、`TouchPanel`し、文字の必要なベロシティを返します。


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState`メソッドを返します、`TouchCollection`ユーザーが画面に触れて場所に関する情報が含まれます。 ユーザーは、画面に触れていない場合、`TouchCollection`は空になります、後者文字移動しないでください。

場合は、ユーザーは、画面に触れることに移動最初のタッチに向かって文字言い換えると、`TouchLocation`インデックス 0 にします。 最初に、必要なベロシティに同じ文字の場所と最初のタッチの場所との違いを設定します。

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

同じ速度で移動文字を保存する数学のビットは、次に示します。 これが重要な理由を説明するには、ユーザーが、500 ピクセルの文字がある場所から離れた場所に触れては状況について考えてみましょう。 最初の行 where`desiredVelocity.X`はセットが 500 の値を割り当てるとします。 ただし、ユーザーは、文字から数えた文字数は 100 個単位の距離にある画面に触れてされた場合は、次に、 `desiredVelocity.X `100 に設定するとします。 結果は、文字の移動速度が応答する方法を遠く離れたタッチ ポイントが、文字からになります。 常に同じ速度で移動する文字を desiredVelocity を変更する必要があります。

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)`ステートメントでは、ユーザーが文字の現在の位置と同じ場所に触れていないことを確認してくださいをチェックしている場合は、ベロシティ非ゼロ – 言い換えると、確認しています。 タッチ遠く離れた方法に関係なく定数である文字の速度を設定する必要がありますし、されていない場合です。 このためには結果的にれると、長さ 1 のベロシティ ベクターを正規化します。 1 の場合、文字が 1 秒あたり 1 ピクセルに移動するようにベロシティ ベクトル。 これをスピードアップ 200 の必要な速度で値を乗算することによってです。


### <a name="applying-velocity-to-position"></a>位置にベロシティを適用します。

返されたベロシティ`GetDesiredVelocityFromInput`文字に適用する必要がある`X`と`Y`を実行時に有効値。 変更して、`Update`メソッドを次のようにします。

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

ここで実装した機能が呼び出された*時間に基づく*移動 (はなく*フレーム ベース*移動) します。 時間ベースの移動速度の値を乗算し (ここに値が格納されている、`velocity`変数) に格納されている最後の更新以降にどれ時間だけが経過して`gameTime.ElapsedGameTime.TotalSeconds`です。 ゲームの実行で 1 秒あたりのフレーム数が少ない場合は、フレームまでの経過時間を上へ移動 – 最終結果は、時間ベースの動きを使用して、オブジェクトは、フレーム レートに関係なく同じ速度で常に移動します。

ゲームを今すぐ実行お文字がタッチの場所に向かっていることをおわかります。

![](part2-images/image6.gif "文字は、タッチの場所へ移動します。")

## <a name="matching-movement-and-animation"></a>一致する動きとアニメーション

移動し、1 つのアニメーションを再生する文字を作成したら、お、アニメーションの残りの部分を定義し、文字の移動を反映するために使用できます。 ときに終了が 8 つのアニメーションの合計。

- ウォークを左、下と右のアニメーション
- 継続のアニメーションがまだおよび、左右の上向き、下向き

### <a name="defining-the-rest-of-the-animations"></a>アニメーションの残りの部分を定義します。

最初に追加されます、`Animation`インスタンスを`CharacterEntity`おが追加された同じ場所に、アニメーションのすべてのクラス`walkDown`です。 これを行うと、`CharacterEntity`次のものは`Animation`メンバー。

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

アニメーションを定義しこれで、`CharacterEntity`次のようにコンス トラクター。

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

ここに上記のコードが追加されたことに注意してください、`CharacterEntity`短いこのチュートリアルを保持するコンス トラクターです。 ゲーム通常は別の文字のアニメーションが独自のクラス定義や XML や JSON などのデータ形式からこの情報を読み込みます。

次に、文字が停止した場合、文字を移動すると、方向、または最後のアニメーションに従ってアニメーションを使用するためのロジックを調整します。 これを行うには変更してみます、`Update`メソッド。


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

アニメーションを切り替えるには、コードは、2 つのブロックに分割されます。 ベロシティと等しくないかどうか、最初のチェック`Vector2.Zero`– 文字が移動するかどうかを表します。 かどうかには、文字が、移動、見ることができます、`velocity.X`と`velocity.Y`の値を調べてどのウォーク アニメーションを再生します。

文字を移動していない場合に、文字を設定する`currentAnimation`継続アニメーション – 場合のみ行うことが、`currentAnimation`ウォーキングはアニメーションやアニメーションが設定されていないかどうか。 場合、`currentAnimation`は 4 つのウォークのアニメーションのいずれかの文字は、既に従来からのために変更する必要はありませんし、`currentAnimation`です。

このコードの結果は、文字が正しく、調べているときのアニメーション化しを停止すると、ウォーキングが最後の方向を直面には。

![](part2-images/image7.gif "このコードの結果は、文字が正しく、調べているときのアニメーション化し、し、停止すると、ウォーキングが最後の方向に直面されます。")

## <a name="summary"></a>まとめ

このチュートリアルは、スプライト、オブジェクトの移動、入力の検出、およびアニメーションのゲーム MonoGame クロスプラット フォームの作成に使用する方法を説明しました。 汎用的なアニメーション クラスの作成について説明します。 また、コード ロジックを整理するための文字エンティティを作成する方法を示しました。

## <a name="related-links"></a>関連リンク

- [CharacterSheet イメージ リソース (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [ウォーキング ゲーム完了 (サンプル)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
