---
title: パート2–ゲームの実装
description: このチュートリアルでは、ゲームロジックとコンテンツを空のモノゲームプロジェクトに追加して、タッチ入力で移動するアニメーションスプライトのデモを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 2c290ac7d66147342087342bda5e5a19b4e6e6f7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763621"
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

コードの記述を開始する前に、ゲーム*コンテンツ*を解凍します。 多くの場合、ゲーム開発者は、*コンテンツ*という用語を使用して、通常はビジュアルアーティスト、ゲームデザイナー、またはオーディオデザイナーによって作成された非コードファイルを参照します。 一般的なコンテンツの種類には、ビジュアルの表示、サウンドの再生、人工知能 (AI) の動作の制御に使用されるファイルが含まれます。 ゲーム開発チームの観点から見たコンテンツは、通常、プログラマ以外が作成します。

ここで使用するコンテンツについては、 [GitHub を](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)参照してください。 これらのファイルは、このチュートリアルの後半でアクセスする場所にダウンロードする必要があります。

## <a name="monogame-class-overview"></a>モノゲームクラスの概要

ここでは、基本的なレンダリングで使用されるモノゲームクラスについて説明します。

- `SpriteBatch`–画面に2D グラフィックスを描画するために使用されます。 *スプライト*は、画面上にイメージを表示するために使用される2d ビジュアル要素です。 オブジェクト`SpriteBatch`は、メソッド`Begin`と`End`メソッドの間で一度に1つのスプライトを描画できます。また、複数のスプライトをグループ化または*バッチ*処理することもできます。
- `Texture2D`–実行時にイメージオブジェクトを表します。 `Texture2D`インスタンスは、多くの場合、.png や .bmp などのファイル形式から作成されますが、実行時に動的に作成することもできます。 `Texture2D`インスタンスは、インスタンスを使用`SpriteBatch`して表示するときに使用されます。
- `Vector2`–2D 座標系の位置を表します。これは、多くの場合、ビジュアルオブジェクトの配置に使用されます。 モノゲームにも`Vector3`が`Vector4`含まれています`Vector2`が、このチュートリアルでのみ使用します。
- `Rectangle`–位置、幅、および高さを持つ4つの辺の領域。 ここでは、 `Texture2D`アニメーションの使用を開始するときにレンダリングするの部分を定義します。

また、モノの名前空間にかかわらず、モノゲームは Microsoft によって管理されていないことにも注意してください。 この`Microsoft.Xna`名前空間は、既存の XNA プロジェクトをモノゲームに簡単に移行できるようにするために、モノゲームで使用されています。

## <a name="rendering-our-first-sprite"></a>最初のスプライトをレンダリングする

次に、1つのスプライトを画面に描画して、モノゲームで2D レンダリングを実行する方法を示します。

### <a name="creating-a-texture2d"></a>Texture2D の作成

スプライトを表示するとき`Texture2D`に使用するインスタンスを作成する必要があります。 すべてのゲームコンテンツは、最終的にプラットフォーム固有のプロジェクトにある**コンテンツ**という名前のフォルダーに格納されます。 モノゲームの共有プロジェクトには、プラットフォーム固有のビルドアクションを使用する必要があるため、コンテンツを含めることはできません。 コンテンツフォルダーは、iOS プロジェクトと、Android プロジェクトの Assets フォルダー内にあります。

ゲームのコンテンツを追加するには、 **[コンテンツ]** フォルダーを右クリックし、[**追加] > [ファイルの追加**] の順に選択します。コンテンツ .zip ファイルが抽出された場所に移動し、文字**シート .png**ファイルを選択します。 ファイルをフォルダーに追加する方法を確認するメッセージが表示されたら、 **[コピー]** オプションを選択します。

![ファイルをフォルダーに追加する方法を確認するメッセージが表示されたら、[コピー] オプションを選択します。](part2-images/image1.png)

コンテンツフォルダーには、文字シート .png ファイルが含まれるようになりました。

![コンテンツフォルダーには、文字シート .png ファイルが含まれるようになりました。](part2-images/image2.png)

次に、文字シート .png ファイルを読み込んでを`Texture2D`作成するコードを追加します。 これを行うには`Game1.cs` 、ファイルを開き、次のフィールドを Game1.cs クラスに追加します。

```csharp
Texture2D characterSheetTexture;
```

次に、 `characterSheetTexture` `LoadContent`メソッドでを作成します。 変更`LoadContent`方法は、次のようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

既定のプロジェクトでは、インスタンスが`spriteBatch`既にインスタンス化されていることに注意してください。 これは後で使用しますが、 `spriteBatch`を使用するための準備として追加のコードは追加されません。 一方`spriteSheetTexture` 、は初期化を必要とするため、の`spriteBatch`作成後に初期化されます。

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

`SpriteBatch`インスタンス`Game1.Draw`とインスタンスが用意できたので、次のように表示コードをメソッドに`Texture2D`追加します。

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

ゲームを実行すると、文字シートから作成されたテクスチャを表示するスプライトが1つ表示されるようになりました。

![ゲームを実行すると、文字シートから作成されたテクスチャを表示するスプライトが1つ表示されるようになりました。](part2-images/image3.png)

## <a name="creating-the-characterentity"></a>文字エンティティの作成

ここまでで、1つのスプライトを画面に表示するコードを追加しました。ただし、他のクラスを作成せずにゲーム全体を開発した場合は、コード編成の問題が発生します。

### <a name="what-is-an-entity"></a>エンティティとは何ですか。

ゲームコードを整理するための一般的なパターンは、game*エンティティ*型ごとに新しいクラスを作成することです。 ゲーム開発のエンティティは、次の特性のいくつかを含むことができるオブジェクトです (すべてが必須ではありません)。

- スプライト、テキスト、3D モデルなどのビジュアル要素
- 物理またはすべてのフレームの動作 (単位 patrolling 設定パスまたはプレーヤー文字が入力に応答する)
- 電源が表示され、プレーヤーによって収集されるなど、動的に作成および破棄できます。

エンティティ組織のシステムは複雑になる可能性があり、多くのゲームエンジンでは、エンティティの管理に役立つクラスを提供しています。 非常に単純なエンティティシステムを実装します。そのため、完全なゲームでは通常、開発者の作業に多くの組織が必要であることに注意してください。

### <a name="defining-the-characterentity"></a>文字エンティティの定義

私たちが呼び出す`CharacterEntity`エンティティは、次の特性を持ちます。

- 独自のものを読み込む機能`Texture2D`
- ウォークアニメーションを更新する呼び出しメソッドを含む、それ自体をレンダリングする機能
- `X`および Y プロパティを指定して、文字の位置を制御します。
- 自分自身を更新する機能。具体的には、タッチスクリーンから値を読み取り、位置を適切に調整します。

ゲーム`CharacterEntity`にを追加するには、 **[プレイ] プロジェクトを**右クリックまたはクリックし、 **[新しいファイルの追加 >]** を選択します。 **[空のクラス]** オプションを選択し、新しいファイルに「文字**エンティティ**」という名前を指定し、 **[新規作成]** をクリックします。

まず、 `CharacterEntity`を`Texture2D`読み込み、それ自体を描画する機能を追加します。 新しく追加さ`CharacterEntity.cs`れたファイルを次のように変更します。

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

上のコードは、コンテンツの配置、表示、およびへ`CharacterEntity`の読み込みの責任を追加します。 上のコードでいくつかの考慮事項について説明します。

### <a name="why-are-x-and-y-floats"></a>X と Y がフローティングするのはなぜですか。

ゲームプログラミングを初めて使用する開発者は、 `float`型が`int`または`double`ではなく使用されている理由を気にすることがあります。 これは、低レベルのレンダリングコードでの配置では、32ビットの値が最も一般的であるためです。 Double 型は、オブジェクトの配置に必要なことがほとんどない、精度の64ビットを占有します。 32ビットの違いは意味がないように思えるかもしれませんが、多くの最新のゲームには、フレームごとに数十の位置値 (1 秒あたり30または60回) を処理する必要があるグラフィックスが含まれています。 グラフィックスパイプライン内を半分ずつ移動する必要があるメモリ量を減らすことは、ゲームのパフォーマンスに大きな影響を与える可能性があります。

データ`int`型は、配置に適した測定単位にすることができますが、エンティティの配置方法に制限を設定できます。 たとえば、整数値を使用すると、ズームインまたは3D カメラ (で許可されて`SpriteBatch`いる) をサポートするゲームのスムーズな移動を実装することが難しくなります。

最後に、ゲームのタイミング値を使用して、画面の周りで文字を移動するロジックについて説明します。 ベロシティと特定のフレームに渡された時間を乗算した結果、ほとんどの場合、整数にならないことが多い`float`ため`X` 、 `Y`とにはを使用する必要があります。

### <a name="why-is-charactersheettexture-static"></a>文字は、静的なのはなぜですか。

`characterSheetTexture` インスタンスは、文字シート.pngファイル`Texture2D`のランタイム表現です。 つまり、ソース文字**シート .png**ファイルから抽出された各ピクセルの色の値が含まれます。 ゲームで2つ`CharacterEntity`のインスタンスを作成する場合は、それぞれが文字シート .png からの情報にアクセスできる必要があります。 このような状況では、2回の文字を読み込む場合のパフォーマンスコストは発生しません。また、ram に重複するテクスチャメモリを格納する必要もありません。 フィールドを使用すると、すべて`CharacterEntity`のインスタンス`Texture2D`で同じを共有できます。 `static`

コンテンツ`static`のランタイム表現にメンバーを使用することは、単純なソリューションであり、あるレベルから別のレベルへの移動時にコンテンツをアンロードするなど、大規模なゲームで発生した問題に対処するものではありません。 このチュートリアルの範囲を超えた高度なソリューションには、モノゲームのコンテンツパイプラインの使用、またはカスタムコンテンツ管理システムの作成が含まれます。

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>SpriteBatch がエンティティによってインスタンス化されるのではなく、引数として渡されるのはなぜですか。

上記`Draw`で実装`characterSheetTexture`されたメソッド`SpriteBatch`は引数を受け取り`CharacterEntity`ますが、はによってインスタンス化されます。

これは`SpriteBatch` 、すべて`Draw`の呼び出しに同じインスタンスを使用する場合と、1つのセットと`End`の`Begin`呼び出しの間で`Draw`すべての呼び出しが行われる場合に、最も効率的なレンダリングが可能であるためです。 もちろん、ゲームに含まれるエンティティインスタンスは1つだけですが、より複雑なゲームでは、複数のエンティティで同じ`SpriteBatch`インスタンスを使用できるパターンを利用できます。

## <a name="adding-characterentity-to-the-game"></a>キャラクターエンティティをゲームに追加する

これで、をレンダリングする`CharacterEntity`ためのコードを追加したので、この新しい`Game1.cs`エンティティのインスタンスを使用するようにのコードを置き換えることができます。 これを行うには、 `Texture2D`フィールドをの`CharacterEntity` `Game1`フィールドに置き換えます。

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

次に、このエンティティのインスタンス化をに`Game1.Initialize`追加します。

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

また、から`Texture2D` `LoadContent`作成したを削除する必要があり`CharacterEntity`ます。これは、の内部で処理されるためです。 `Game1.LoadContent`次のようになります。

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

名前に関係なく、 `LoadContent`メソッドは、コンテンツを読み込むための唯一の場所ではありません。多くのゲームでは、初期化メソッドでコンテンツの読み込みを実装しています。または、コンテンツとしての更新コードでも、プレーヤーがに到達するまでは必要ない可能性があります。ゲームの特定のポイント。

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

現在のところ、完全な文字**シート .png**ファイルが表示されています。`CharacterEntity` このような複数のイメージを1つのファイルに配置することを、*スプライトシート*と呼びます。 通常、スプライトはスプライトシートの一部だけを表示します。 この文字**シート**の`CharacterEntity`一部をレンダリングするようにを変更します。この部分は、時間の経過と共に変更され、ウォーキングアニメーションが表示されます。

ここでは、 `Animation`クラスを作成して、文字エンティティアニメーションのロジックと状態を制御します。 Animation クラスは、アニメーションだけ`CharacterEntity`でなくすべてのエンティティに使用できる一般的なクラスになります。 Ultimate クラスは`Rectangle` 、`CharacterEntity`を描画するときにが使用するを提供します。 `Animation` また、アニメーションの定義`AnimationFrame`に使用されるクラスも作成します。

### <a name="defining-animationframe"></a>定義 (アニメーションフレームを)

`AnimationFrame`には、アニメーションに関連するロジックは含まれません。 データを格納するためだけに使用します。 `AnimationFrame`クラスを追加するには、 **[プレイ]** の共有プロジェクトを右クリックまたはクリックし、 **[新しいファイルの追加 >]** を選択します。「**アニメーションフレーム**」という名前を入力し、 **[新規]** ボタンをクリックします。 次のコードが`AnimationFrame.cs`含まれるようにファイルを変更します。

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

クラス`AnimationFrame`には、次の2つの情報が含まれています。

- `SourceRectangle`–に`Texture2D` `AnimationFrame`よって表示されるの領域を定義します。 この値はピクセル単位で計測され、左上は (0, 0) になります。
- `Duration``AnimationFrame` –`Animation`での使用時にが表示される期間を定義します。

### <a name="defining-animation"></a>アニメーションの定義

クラス`Animation`には、が`List<AnimationFrame>`経過した時間に従って現在表示されているフレームを切り替えるロジックと共に、が含まれます。

`Animation`クラスを追加するには、 **[プレイ]** の共有プロジェクトを右クリックまたはクリックし、 **[新しいファイルの追加 >]** を選択します。名前の**アニメーション**を入力し、 **[新規]** ボタンをクリックします。 次のコードが`Animation.cs`含まれるようにファイルを変更します。

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

`Animation`クラスの詳細をいくつか見てみましょう。

### <a name="frames-list"></a>フレーム一覧

`frames`メンバーは、アニメーションのデータを格納するものです。 アニメーションをインスタンス化するコードは、 `AnimationFrame` `AddFrame`メソッドを使用`frames`してリストにインスタンスを追加します。 より完全な実装では`public` 、変更`frames`のためのメソッドまたはプロパティを提供できますが、このチュートリアルのフレームを追加するように機能を制限します。

### <a name="duration"></a>存続期間

Duration は、 `Animation,`含ま`AnimationFrame`れているすべてのインスタンスの期間を加算することによって取得されるの合計期間を返します。 が変更できないオブジェクトの`AnimationFrame`場合、この値はキャッシュされる可能性がありますが、アニメーションに追加した後に変更できるクラスとして animation frame が実装されているため、プロパティにアクセスするたびにこの値を計算する必要があります。

### <a name="update"></a>更新

`Update`メソッドは、すべてのフレーム (つまり、ゲーム全体が更新されるたびに) を呼び出す必要があります。 その目的は、現在表示`timeIntoAnimation`されているフレームを返すために使用されるメンバーを増やすことです。 のロジック`Update`により、 `timeIntoAnimation`がアニメーション全体の継続時間よりも大きくなるのを防ぐことができます。

`Animation`クラスを使用してウォーキングアニメーションを表示するため、このアニメーションが最後に達したときに繰り返します。 これは、 `timeIntoAnimation`が`Duration`値より大きいかどうかを確認することで実現できます。 そのような場合は、最初に戻って残りの部分を保持し、ループアニメーションになります。

### <a name="obtaining-the-current-frames-rectangle"></a>現在のフレームの四角形を取得する

`Animation`クラスの目的は、スプライトを描画するとき`Rectangle`に使用できるを提供することです。 現在、 `Animation`クラスには、表示する`timeIntoAnimation` `Rectangle`必要があるメンバーを取得するために使用するメンバーを変更するためのロジックが含まれています。 これを行うには、 `CurrentRectangle` `Animation`クラスでプロパティを作成します。 次のプロパティをクラス`Animation`にコピーします。

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

に`CharacterEntity`は、現在`Animation`表示されているへの参照だけでなく、ウォーキングのためのアニメーションも含まれます。

まず、これまでに記述し`Animation,`た機能をテストするために使用する最初のを追加します。 次のメンバーを文字エンティティクラスに追加してみましょう。

```csharp
Animation walkDown;
Animation currentAnimation;
```

次に、アニメーションを`walkDown`定義します。 これを行うには`CharacterEntity` 、次のようにコンストラクターを変更します。

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

既に説明したように、 `Animation.Update`時間ベースのアニメーションを再生するには、を呼び出す必要があります。 また、 `currentAnimation`を割り当てる必要もあります。 ここでは、 `currentAnimation`をに割り当てますが、このコードは後で、移動ロジックを`walkDown`実装するときに置き換えます。 次のように`Update` 、メソッド`CharacterEntity`をに追加します。

```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

が割り当てられ、 `currentAnimation`更新されたので、それを使用して描画を実行できます。 次のよう`CharacterEntity.Draw`に変更します。

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

`CharacterEntity`アニメーションを取得する最後の手順は、から`Game1`新しく追加`Update`したメソッドを呼び出すことです。 次`Game1.Update`のように変更します。

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

これで`CharacterEntity` 、は`walkDown`アニメーションを再生します。

![これで、文字エンティティが再生ダウンアニメーションを再生します。](part2-images/image5.gif)

## <a name="adding-movement-to-the-character"></a>文字への移動の追加

次に、タッチコントロールを使用して、文字への移動を追加します。 ユーザーが画面に触れると、文字は画面に触れる位置に移動します。 何も検出されない場合は、文字が配置されます。

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput の定義

ここでは、タッチスクリーンの`TouchPanel`現在の状態に関する情報を提供する、モノゲームのクラスを使用します。 を確認`TouchPanel`し、文字の目的の速度を返すメソッドを追加してみましょう。

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

メソッド`TouchPanel.GetState`は、ユーザー `TouchCollection`が画面に触れる場所に関する情報を格納しているを返します。 ユーザーが画面に触れていない場合、は`TouchCollection`空になります。この場合、文字は移動できません。

ユーザーが画面に触れている場合は、最初のタッチに向かって文字を移動します (つまり`TouchLocation` 、インデックス0の)。 最初に、文字の位置と最初のタッチの位置の差に等しいように、必要なベロシティを設定します。

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

次に示すのは、文字を同じ速度で移動させるための少しの数値演算です。 なぜこれが重要なのかを説明するために、文字がどこから離れているかをユーザーが500ピクセルに接している状況を考えてみましょう。 が設定され`desiredVelocity.X`ている最初の行では、値500が割り当てられます。 ただし、ユーザーが文字の100単位の距離で画面にタッチしていた場合、は`desiredVelocity.X` 100 に設定されます。 結果として、文字の移動速度が文字からのタッチポイントの距離に応答することになります。 文字を常に同じ速度で移動させるため、desiredVelocity を変更する必要があります。

この`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)`ステートメントでは、ベロシティが0以外であるかどうかが確認されています。つまり、ユーザーが文字の現在位置と同じ位置に触れていないことを確認しています。 そうでない場合は、タッチの距離に関係なく、文字の速度が一定になるように設定する必要があります。 これを実現するには、ベロシティベクターを正規化します。これにより、長さは1になります。 速度ベクトル1は、文字が1ピクセル/秒で移動することを意味します。 この値には、200の必要な速度を乗算することで、速度を上げます。

### <a name="applying-velocity-to-position"></a>位置にベロシティを適用する

から`GetDesiredVelocityFromInput`返されたベロシティを、実行時に影響`X`を`Y`与える文字の値と値に適用する必要があります。 次のように`Update`メソッドを変更します。

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

ここで実装した内容は、(*フレームベースの移動で*はなく)*時間ベースの*移動と呼ばれます。 時間ベースの移動は、に格納されている最後の`velocity` `gameTime.ElapsedGameTime.TotalSeconds`更新以降に経過した時間によって、ベロシティ値 (この場合は変数に格納されている値) を乗算します。 ゲームの実行時間が1秒あたりのフレーム数が少なくなると、フレーム間の経過時間が上がります。最終的には、時間ベースの移動を使用するオブジェクトは、フレームレートに関係なく、常に同じ速度で移動します。

ここでゲームを実行すると、文字がタッチの場所に移動していることがわかります。

![文字がタッチ位置に移動しています](part2-images/image6.gif)

## <a name="matching-movement-and-animation"></a>一致する移動とアニメーション

1つのアニメーションの移動と再生を行った後、アニメーションの残りの部分を定義し、それらを使用して文字の移動を反映することができます。 完了すると、合計8つのアニメーションが作成されます。

- 上向き、下向き、左、右のアニメーション
- 静止していて、上、下、左、右に移動するためのアニメーション

### <a name="defining-the-rest-of-the-animations"></a>アニメーションの残りの部分を定義する

最初に、追加し`Animation`た`walkDown`のと`CharacterEntity`同じ場所にあるすべてのアニメーションのクラスにインスタンスを追加します。 これを行うと、に`CharacterEntity`は次`Animation`のメンバーが含まれます。

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

次のように、 `CharacterEntity`コンストラクターでアニメーションを定義します。

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

このチュートリアルを短くするために、上記のコード`CharacterEntity`がコンストラクターに追加されていることに注意してください。 通常、ゲームでは、文字アニメーションの定義を独自のクラスに分割するか、XML や JSON などのデータ形式からこの情報を読み込みます。

次に、文字が移動する方向に従ってアニメーションを使用するようにロジックを調整します。または、文字が停止したばかりの場合は、最後のアニメーションに従います。 これを行うには、メソッドを`Update`次のように変更します。

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

アニメーションを切り替えるコードは、2つのブロックに分割されます。 最初のチェックでは、ベロシティがに`Vector2.Zero`等しくないかどうかが確認されます。これは、文字が移動しているかどうかを示します。 文字が移動中の場合は、との`velocity.X`値を確認して`velocity.Y` 、再生する再生アニメーションを決定できます。

文字が移動していない場合は、その文字`currentAnimation`を継続アニメーションに設定する必要がありますが、は、 `currentAnimation`がウォーキングアニメーションの場合、またはアニメーションが設定されていない場合にのみ実行します。 が4つのウォーキングアニメーションのいずれでもない場合、文字は既に存在するため、変更`currentAnimation`する必要はありません。 `currentAnimation`

このコードの結果として、検索時に文字が適切にアニメーション化され、停止時にその最後の方向に直面することになります。

![このコードの結果として、検索時に文字が適切にアニメーション化され、停止時にその最後の方向に直面することになります。](part2-images/image7.gif)

## <a name="summary"></a>Summary

このチュートリアルでは、モノゲームを使って、スプライト、オブジェクトの移動、入力検出、アニメーションを使用してクロスプラットフォームのゲームを作成する方法を説明しました。 汎用アニメーションクラスの作成について説明します。 また、コードロジックを整理するための文字エンティティを作成する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [文字シートイメージリソース (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [ゲーム完了のウォーク (サンプル)](https://docs.microsoft.com/samples/xamarin/mobile-samples/walkinggamemg/)
