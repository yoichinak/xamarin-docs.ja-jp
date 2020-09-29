---
title: パート2–ゲームの実装
description: このチュートリアルでは、ゲームロジックとコンテンツを空のモノゲームプロジェクトに追加して、タッチ入力で移動するアニメーションスプライトのデモを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 835beca6866f5d3f82ae48d1c99557ceafe360ba
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91429775"
---
# <a name="part-2--implementing-the-walkinggame"></a>パート2–ゲームの実装

_このチュートリアルでは、ゲームロジックとコンテンツを空のモノゲームプロジェクトに追加して、タッチ入力で移動するアニメーションスプライトのデモを作成する方法について説明します。_

このチュートリアルの前の部分では、空のモノゲームプロジェクトを作成する方法を示しました。 ここでは、単純なゲームデモを作成して、これらの前の部分について説明します。 この記事は、次のセクションで構成されています。

- ゲームコンテンツの解凍
- モノゲームクラスの概要
- 最初のスプライトをレンダリングする
- 文字エンティティの作成
- キャラクターエンティティをゲームに追加する
- Animation クラスの作成
- 最初のアニメーションを文字エンティティに追加する
- 文字への移動の追加
- 一致する移動とアニメーション

## <a name="unzipping-our-game-content"></a>ゲームコンテンツの解凍

コードの記述を開始する前に、ゲーム *コンテンツ*を解凍します。 多くの場合、ゲーム開発者は、 *コンテンツ* という用語を使用して、通常はビジュアルアーティスト、ゲームデザイナー、またはオーディオデザイナーによって作成された非コードファイルを参照します。 一般的なコンテンツの種類には、ビジュアルの表示、サウンドの再生、人工知能 (AI) の動作の制御に使用されるファイルが含まれます。 ゲーム開発チームの観点から見たコンテンツは、通常、プログラマ以外が作成します。

ここで使用するコンテンツについては、 [GitHub を](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)参照してください。 これらのファイルは、このチュートリアルの後半でアクセスする場所にダウンロードする必要があります。

## <a name="monogame-class-overview"></a>モノゲームクラスの概要

ここでは、基本的なレンダリングで使用されるモノゲームクラスについて説明します。

- `SpriteBatch` –画面に2D グラフィックスを描画するために使用されます。 *スプライト* は、画面上にイメージを表示するために使用される2d ビジュアル要素です。 オブジェクトは、 `SpriteBatch` メソッドとメソッドの間で一度に1つのスプライトを描画できます `Begin` `End` 。また、複数のスプライトをグループ化または *バッチ*処理することもできます。
- `Texture2D` –実行時にイメージオブジェクトを表します。 `Texture2D` インスタンスは、多くの場合、.png や .bmp などのファイル形式から作成されますが、実行時に動的に作成することもできます。 `Texture2D` インスタンスは、インスタンスを使用して表示するときに使用され `SpriteBatch` ます。
- `Vector2` –2D 座標系の位置を表します。これは、多くの場合、ビジュアルオブジェクトの配置に使用されます。 モノゲームにもが含まれてい `Vector3` `Vector4` ますが、 `Vector2` このチュートリアルでのみ使用します。
- `Rectangle` –位置、幅、および高さを持つ4つの辺の領域。 ここでは、アニメーションの使用を開始するときにレンダリングするの部分を定義し `Texture2D` ます。

また、モノの名前空間にかかわらず、モノゲームは Microsoft によって管理されていないことにも注意してください。 `Microsoft.Xna`この名前空間は、既存の XNA プロジェクトをモノゲームに簡単に移行できるようにするために、モノゲームで使用されています。

## <a name="rendering-our-first-sprite"></a>最初のスプライトをレンダリングする

次に、1つのスプライトを画面に描画して、モノゲームで2D レンダリングを実行する方法を示します。

### <a name="creating-a-texture2d"></a>Texture2D の作成

`Texture2D`スプライトを表示するときに使用するインスタンスを作成する必要があります。 すべてのゲームコンテンツは、最終的にプラットフォーム固有のプロジェクトにある **コンテンツ** という名前のフォルダーに格納されます。 モノゲームの共有プロジェクトには、プラットフォーム固有のビルドアクションを使用する必要があるため、コンテンツを含めることはできません。 コンテンツフォルダーは、iOS プロジェクトと、Android プロジェクトの Assets フォルダー内にあります。

ゲームのコンテンツを追加するには、[ **コンテンツ** ] フォルダーを右クリックし、[ **追加] > [ファイルの追加** ] の順に選択します。content.zip ファイルが抽出された場所に移動し、 **charactersheet.png** ファイルを選択します。 ファイルをフォルダーに追加する方法を確認するメッセージが表示されたら、[ **コピー** ] オプションを選択します。

![ファイルをフォルダーに追加する方法を確認するメッセージが表示されたら、[コピー] オプションを選択します。](part2-images/image1.png)

コンテンツフォルダーに charactersheet.png ファイルが含まれるようになりました。

![コンテンツフォルダーに charactersheet.png ファイルが含まれるようになりました](part2-images/image2.png)

次に、charactersheet.png ファイルを読み込み、を作成するコードを追加し `Texture2D` ます。 これを行うには、ファイルを開き、 `Game1.cs` 次のフィールドを Game1.cs クラスに追加します。

```csharp
Texture2D characterSheetTexture;
```

次に、 `characterSheetTexture` メソッドでを作成し `LoadContent` ます。 変更方法は、次の `LoadContent` ようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

既定のプロジェクトでは、インスタンスが既にインスタンス化されていることに注意してください `spriteBatch` 。 これは後で使用しますが、を使用するための準備として追加のコードは追加されません `spriteBatch` 。 一方、は `spriteSheetTexture` 初期化を必要とするため、の作成後に初期化され `spriteBatch` ます。

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

インスタンスとインスタンスが用意できたので、 `SpriteBatch` `Texture2D` 次のように表示コードをメソッドに追加し `Game1.Draw` ます。

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

ゲームを実行すると、charactersheet.png から作成されたテクスチャを表示するスプライトが1つ表示されるようになりました。

![ゲームの実行中に、から作成されたテクスチャを表示するスプライトが1つ表示されるようになりました charactersheet.png](part2-images/image3.png)

## <a name="creating-the-characterentity"></a>文字エンティティの作成

ここまでで、1つのスプライトを画面に表示するコードを追加しました。ただし、他のクラスを作成せずにゲーム全体を開発した場合は、コード編成の問題が発生します。

### <a name="what-is-an-entity"></a>エンティティとは何ですか。

ゲームコードを整理するための一般的なパターンは、game *エンティティ* 型ごとに新しいクラスを作成することです。 ゲーム開発のエンティティは、次の特性のいくつかを含むことができるオブジェクトです (すべてが必須ではありません)。

- スプライト、テキスト、3D モデルなどのビジュアル要素
- 物理またはすべてのフレームの動作 (単位 patrolling 設定パスまたはプレーヤー文字が入力に応答する)
- 電源が表示され、プレーヤーによって収集されるなど、動的に作成および破棄できます。

エンティティ組織のシステムは複雑になる可能性があり、多くのゲームエンジンでは、エンティティの管理に役立つクラスを提供しています。 非常に単純なエンティティシステムを実装します。そのため、完全なゲームでは通常、開発者の作業に多くの組織が必要であることに注意してください。

### <a name="defining-the-characterentity"></a>文字エンティティの定義

私たちが呼び出すエンティティは、 `CharacterEntity` 次の特性を持ちます。

- 独自のものを読み込む機能 `Texture2D`
- ウォークアニメーションを更新する呼び出しメソッドを含む、それ自体をレンダリングする機能
- `X` および Y プロパティを指定して、文字の位置を制御します。
- 自分自身を更新する機能。具体的には、タッチスクリーンから値を読み取り、位置を適切に調整します。

ゲームにを追加するには、 `CharacterEntity` [プレイ] プロジェクトを右クリックまた**WalkingGame**はクリックし、[**新しいファイルの追加 >**] を選択します。[**空のクラス**] オプションを選択し、新しいファイルに「文字**エンティティ**」という名前を指定し、[**新規作成**] をクリックします。

まず、を `CharacterEntity` 読み込み、 `Texture2D` それ自体を描画する機能を追加します。 新しく追加されたファイルを次のように変更し `CharacterEntity.cs` ます。

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

上のコードは、コンテンツの配置、表示、およびへの読み込みの責任を追加し `CharacterEntity` ます。 上のコードでいくつかの考慮事項について説明します。

### <a name="why-are-x-and-y-floats"></a>X と Y がフローティングするのはなぜですか。

ゲームプログラミングを初めて使用する開発者は、 `float` 型がまたはではなく使用されている理由を気にすることがあり `int` `double` ます。 これは、低レベルのレンダリングコードでの配置では、32ビットの値が最も一般的であるためです。 Double 型は、オブジェクトの配置に必要なことがほとんどない、精度の64ビットを占有します。 32ビットの違いは意味がないように思えるかもしれませんが、多くの最新のゲームには、フレームごとに数十の位置値 (1 秒あたり30または60回) を処理する必要があるグラフィックスが含まれています。 グラフィックスパイプライン内を半分ずつ移動する必要があるメモリ量を減らすことは、ゲームのパフォーマンスに大きな影響を与える可能性があります。

`int`データ型は、配置に適した測定単位にすることができますが、エンティティの配置方法に制限を設定できます。 たとえば、整数値を使用すると、ズームインまたは3D カメラ (で許可されている) をサポートするゲームのスムーズな移動を実装することが難しくなり `SpriteBatch` ます。

最後に、ゲームのタイミング値を使用して、画面の周りで文字を移動するロジックについて説明します。 ベロシティと特定のフレームに渡された時間を乗算した結果、ほとんどの場合、整数にならないことが多いため、とにはを使用する必要があり `float` `X` `Y` ます。

### <a name="why-is-charactersheettexture-static"></a>文字は、静的なのはなぜですか。

`characterSheetTexture` `Texture2D` インスタンスは、charactersheet.png ファイルのランタイム表現です。 つまり、ソース **charactersheet.png** ファイルから抽出された各ピクセルの色の値が含まれます。 ゲームで2つのインスタンスを作成する場合は `CharacterEntity` 、charactersheet.png から情報にアクセスする必要があります。 このような状況では、charactersheet.png を2回読み込む場合のパフォーマンスコストは発生しません。また、ram に重複するテクスチャメモリを格納する必要もありません。 フィールドを使用 `static` すると、すべてのインスタンスで同じを共有でき `Texture2D` `CharacterEntity` ます。

`static`コンテンツのランタイム表現にメンバーを使用することは、単純なソリューションであり、あるレベルから別のレベルへの移動時にコンテンツをアンロードするなど、大規模なゲームで発生した問題に対処するものではありません。 このチュートリアルの範囲を超えた高度なソリューションには、モノゲームのコンテンツパイプラインの使用、またはカスタムコンテンツ管理システムの作成が含まれます。

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>SpriteBatch がエンティティによってインスタンス化されるのではなく、引数として渡されるのはなぜですか。

`Draw`上記で実装されたメソッドは引数を受け取りますが、はに `SpriteBatch` `characterSheetTexture` よってインスタンス化され `CharacterEntity` ます。

これは、すべての呼び出しに同じインスタンスを使用する場合と、 `SpriteBatch` `Draw` `Draw` 1 つのセットとの呼び出しの間ですべての呼び出しが行われる場合に `Begin` 、最も効率的なレンダリングが可能であるためです `End` 。 もちろん、ゲームに含まれるエンティティインスタンスは1つだけですが、より複雑なゲームでは、複数のエンティティで同じインスタンスを使用できるパターンを利用でき `SpriteBatch` ます。

## <a name="adding-characterentity-to-the-game"></a>キャラクターエンティティをゲームに追加する

これで、をレンダリングする `CharacterEntity` ためのコードを追加したので、 `Game1.cs` この新しいエンティティのインスタンスを使用するようにのコードを置き換えることができます。 これを行うには、 `Texture2D` フィールドをのフィールドに置き換えます `CharacterEntity` `Game1` 。

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

次に、このエンティティのインスタンス化をに追加し `Game1.Initialize` ます。

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

また、から作成したを削除する必要があり `Texture2D` `LoadContent` ます。これは、の内部で処理されるためです `CharacterEntity` 。 `Game1.LoadContent` 次のようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

名前にかかわらず、 `LoadContent` メソッドがコンテンツを読み込むための唯一の場所ではないことに注意してください。多くのゲームは、初期化メソッドでコンテンツの読み込みを実装します。または、コンテンツとしての更新コードでも、プレーヤーがゲームの特定のポイントに到達するまでは必要ない可能性があります。

最後に、次のように Draw メソッドを変更できます。

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

ゲームを実行すると、文字が表示されるようになります。 X と Y の既定値は0であるため、文字は画面の左上隅に配置されます。

![X と Y の既定値は0であるため、文字は画面の左上隅に配置されます。](part2-images/image4.png)

## <a name="creating-the-animation-class"></a>Animation クラスの作成

現在、 `CharacterEntity` **charactersheet.png** ファイル全体が表示されています。 このような複数のイメージを1つのファイルに配置することを、 *スプライトシート*と呼びます。 通常、スプライトはスプライトシートの一部だけを表示します。 `CharacterEntity`この**charactersheet.png**の一部をレンダリングするようにを変更します。この部分は、時間の経過と共に変化して、ウォーキングアニメーションを表示します。

ここでは、クラスを作成して、文字 `Animation` エンティティアニメーションのロジックと状態を制御します。 Animation クラスは、アニメーションだけでなくすべてのエンティティに使用できる一般的なクラスになります `CharacterEntity` 。 Ultimate `Animation` クラスは `Rectangle` 、を描画するときにが使用するを提供し `CharacterEntity` ます。 また `AnimationFrame` 、アニメーションの定義に使用されるクラスも作成します。

### <a name="defining-animationframe"></a>定義 (アニメーションフレームを)

`AnimationFrame` には、アニメーションに関連するロジックは含まれません。 データを格納するためだけに使用します。 クラスを追加するに `AnimationFrame` は、[ **プレイ** ] の共有プロジェクトを右クリックまたはクリックし、[ **新しいファイルの追加 >** ] を選択します。「 **アニメーションフレーム** 」という名前を入力し、[ **新規** ] ボタンをクリックします。 `AnimationFrame.cs`次のコードが含まれるようにファイルを変更します。

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

クラスには、 `AnimationFrame` 次の2つの情報が含まれています。

- `SourceRectangle` –によって表示されるの領域を定義し `Texture2D` `AnimationFrame` ます。 この値はピクセル単位で計測され、左上は (0, 0) になります。
- `Duration` – `AnimationFrame` での使用時にが表示される期間を定義 `Animation` します。

### <a name="defining-animation"></a>アニメーションの定義

クラスには `Animation` 、 `List<AnimationFrame>` が経過した時間に従って現在表示されているフレームを切り替えるロジックと共に、が含まれます。

クラスを追加するに `Animation` は、[ **プレイ** ] の共有プロジェクトを右クリックまたはクリックし、[ **新しいファイルの追加 >**] を選択します。名前の **アニメーション** を入力し、[ **新規** ] ボタンをクリックします。 `Animation.cs`次のコードが含まれるようにファイルを変更します。

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

クラスの詳細をいくつか見てみましょう `Animation` 。

### <a name="frames-list"></a>フレーム一覧

`frames`メンバーは、アニメーションのデータを格納するものです。 アニメーションをインスタンス化するコードは、 `AnimationFrame` メソッドを使用してリストにインスタンスを追加し `frames` `AddFrame` ます。 より完全な実装では `public` 、変更のためのメソッドまたはプロパティを提供できます `frames` が、このチュートリアルのフレームを追加するように機能を制限します。

### <a name="duration"></a>Duration

Duration は、含まれている `Animation,` すべてのインスタンスの期間を加算することによって取得されるの合計期間を返し `AnimationFrame` ます。 が変更できないオブジェクトの場合、この値はキャッシュされる可能性 `AnimationFrame` がありますが、アニメーションに追加した後に変更できるクラスとして Animation frame が実装されているため、プロパティにアクセスするたびにこの値を計算する必要があります。

### <a name="update"></a>更新

メソッドは、 `Update` すべてのフレーム (つまり、ゲーム全体が更新されるたびに) を呼び出す必要があります。 その目的は、 `timeIntoAnimation` 現在表示されているフレームを返すために使用されるメンバーを増やすことです。 のロジックにより `Update` 、が `timeIntoAnimation` アニメーション全体の継続時間よりも大きくなるのを防ぐことができます。

クラスを使用して `Animation` ウォーキングアニメーションを表示するため、このアニメーションが最後に達したときに繰り返します。 これは、が値より大きいかどうかを確認することで実現でき `timeIntoAnimation` `Duration` ます。 そのような場合は、最初に戻って残りの部分を保持し、ループアニメーションになります。

### <a name="obtaining-the-current-frames-rectangle"></a>現在のフレームの四角形を取得する

クラスの目的 `Animation` は、スプライトを描画するときに使用できるを提供する `Rectangle` ことです。 現在、クラスには、 `Animation` 表示する `timeIntoAnimation` 必要があるメンバーを取得するために使用するメンバーを変更するためのロジックが含まれてい `Rectangle` ます。 これを行うには `CurrentRectangle` 、クラスでプロパティを作成し `Animation` ます。 次のプロパティをクラスにコピーし `Animation` ます。

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime = new TimeSpan();
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

## <a name="adding-the-first-animation-to-characterentity"></a>最初のアニメーションを文字エンティティに追加する

に `CharacterEntity` は、現在表示されているへの参照だけでなく、ウォーキングのためのアニメーションも含まれ `Animation` ます。

まず、これ `Animation,` までに記述した機能をテストするために使用する最初のを追加します。 次のメンバーを文字エンティティクラスに追加してみましょう。

```csharp
Animation walkDown;
Animation currentAnimation;
```

次に、アニメーションを定義し `walkDown` ます。 これを行うには、次のようにコンストラクターを変更し `CharacterEntity` ます。

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

既に説明したように、 `Animation.Update` 時間ベースのアニメーションを再生するには、を呼び出す必要があります。 また、を割り当てる必要もあり `currentAnimation` ます。 ここでは、 `currentAnimation` をに割り当てます `walkDown` が、このコードは後で、移動ロジックを実装するときに置き換えます。 次のよう `Update` に、メソッドをに追加し `CharacterEntity` ます。

```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

が割り当てられ、更新されたので、それを使用して `currentAnimation` 描画を実行できます。 次のように変更 `CharacterEntity.Draw` します。

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

アニメーションを取得する最後の手順 `CharacterEntity` は、から新しく追加したメソッドを呼び出すことです `Update` `Game1` 。 次のように変更 `Game1.Update` します。

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

これで、は `CharacterEntity` アニメーションを再生し `walkDown` ます。

![これで、文字エンティティが再生ダウンアニメーションを再生します。](part2-images/image5.gif)

## <a name="adding-movement-to-the-character"></a>文字への移動の追加

次に、タッチコントロールを使用して、文字への移動を追加します。 ユーザーが画面に触れると、文字は画面に触れる位置に移動します。 何も検出されない場合は、文字が配置されます。

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput の定義

ここでは、 `TouchPanel` タッチスクリーンの現在の状態に関する情報を提供する、モノゲームのクラスを使用します。 を確認 `TouchPanel` し、文字の目的の速度を返すメソッドを追加してみましょう。

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

メソッドは、 `TouchPanel.GetState` `TouchCollection` ユーザーが画面に触れる場所に関する情報を格納しているを返します。 ユーザーが画面に触れていない場合、は `TouchCollection` 空になります。この場合、文字は移動できません。

ユーザーが画面に触れている場合は、最初のタッチに向かって文字を移動します (つまり、 `TouchLocation` インデックス0の)。 最初に、文字の位置と最初のタッチの位置の差に等しいように、必要なベロシティを設定します。

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

次に示すのは、文字を同じ速度で移動させるための少しの数値演算です。 なぜこれが重要なのかを説明するために、文字がどこから離れているかをユーザーが500ピクセルに接している状況を考えてみましょう。 が設定されている最初の行では、 `desiredVelocity.X` 値500が割り当てられます。 ただし、ユーザーが文字の100単位の距離で画面にタッチしていた場合、は `desiredVelocity.X` 100 に設定されます。 結果として、文字の移動速度が文字からのタッチポイントの距離に応答することになります。 文字を常に同じ速度で移動させるため、desiredVelocity を変更する必要があります。

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)`このステートメントでは、ベロシティが0以外であるかどうかが確認されています。つまり、ユーザーが文字の現在位置と同じ位置に触れていないことを確認しています。 そうでない場合は、タッチの距離に関係なく、文字の速度が一定になるように設定する必要があります。 これを実現するには、ベロシティベクターを正規化します。これにより、長さは1になります。 速度ベクトル1は、文字が1ピクセル/秒で移動することを意味します。 この値には、200の必要な速度を乗算することで、速度を上げます。

### <a name="applying-velocity-to-position"></a>位置にベロシティを適用する

から返されたベロシティを、 `GetDesiredVelocityFromInput` `X` 実行時に影響を与える文字の値と値に適用する必要があり `Y` ます。 次のように `Update` メソッドを変更します。

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

ここで実装した内容は、(*フレームベースの移動で*はなく)*時間ベースの*移動と呼ばれます。 時間ベースの移動は、 `velocity` に格納されている最後の更新以降に経過した時間によって、ベロシティ値 (この場合は変数に格納されている値) を乗算し `gameTime.ElapsedGameTime.TotalSeconds` ます。 ゲームの実行時間が1秒あたりのフレーム数が少なくなると、フレーム間の経過時間が上がります。最終的には、時間ベースの移動を使用するオブジェクトは、フレームレートに関係なく、常に同じ速度で移動します。

ここでゲームを実行すると、文字がタッチの場所に移動していることがわかります。

![文字がタッチ位置に移動しています](part2-images/image6.gif)

## <a name="matching-movement-and-animation"></a>一致する移動とアニメーション

1つのアニメーションの移動と再生を行った後、アニメーションの残りの部分を定義し、それらを使用して文字の移動を反映することができます。 完了すると、合計8つのアニメーションが作成されます。

- 上向き、下向き、左、右のアニメーション
- 静止していて、上、下、左、右に移動するためのアニメーション

### <a name="defining-the-rest-of-the-animations"></a>アニメーションの残りの部分を定義する

最初に、 `Animation` `CharacterEntity` 追加したのと同じ場所にあるすべてのアニメーションのクラスにインスタンスを追加 `walkDown` します。 これを行うと、には `CharacterEntity` 次のメンバーが含まれ `Animation` ます。

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

次のように、コンストラクターでアニメーションを定義し `CharacterEntity` ます。

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

`CharacterEntity`このチュートリアルを短くするために、上記のコードがコンストラクターに追加されていることに注意してください。 通常、ゲームでは、文字アニメーションの定義を独自のクラスに分割するか、XML や JSON などのデータ形式からこの情報を読み込みます。

次に、文字が移動する方向に従ってアニメーションを使用するようにロジックを調整します。または、文字が停止したばかりの場合は、最後のアニメーションに従います。 これを行うには、メソッドを次のように変更し `Update` ます。

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

アニメーションを切り替えるコードは、2つのブロックに分割されます。 最初のチェックでは、ベロシティがに等しくないかどうかが確認 `Vector2.Zero` されます。これは、文字が移動しているかどうかを示します。 文字が移動中の場合は、との値を確認し `velocity.X` て、 `velocity.Y` 再生する再生アニメーションを決定できます。

文字が移動していない場合は、その文字を継続アニメーションに設定する必要 `currentAnimation` がありますが、は、 `currentAnimation` がウォーキングアニメーションの場合、またはアニメーションが設定されていない場合にのみ実行します。 `currentAnimation`が4つのウォーキングアニメーションのいずれでもない場合、文字は既に存在するため、変更する必要はありません `currentAnimation` 。

このコードの結果として、検索時に文字が適切にアニメーション化され、停止時にその最後の方向に直面することになります。

![このコードの結果として、検索時に文字が適切にアニメーション化され、停止時にその最後の方向に直面することになります。](part2-images/image7.gif)

## <a name="summary"></a>まとめ

このチュートリアルでは、モノゲームを使って、スプライト、オブジェクトの移動、入力検出、アニメーションを使用してクロスプラットフォームのゲームを作成する方法を説明しました。 汎用アニメーションクラスの作成について説明します。 また、コードロジックを整理するための文字エンティティを作成する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [文字シートイメージリソース (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [ゲーム完了のウォーク (サンプル)](/samples/xamarin/mobile-samples/walkinggamemg/)