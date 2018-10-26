---
title: CCTextureCache を使用して、テクスチャ キャッシュ
description: CocosSharp の CCTextureCache クラスでは、整理、キャッシュ、およびコンテンツをアンロードする標準的な方法を提供します。 大規模なゲーム RAM、グループ化、およびテクスチャの破棄のプロセスを簡略化する完全に適合しない可能性に特に便利です。
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 232363d6ce1cb93499716b2c1247c48403cf6cea
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117383"
---
# <a name="texture-caching-using-cctexturecache"></a>CCTextureCache を使用して、テクスチャ キャッシュ

_CocosSharp の CCTextureCache クラスでは、整理、キャッシュ、およびコンテンツをアンロードする標準的な方法を提供します。大規模なゲーム RAM、グループ化、およびテクスチャの破棄のプロセスを簡略化する完全に適合しない可能性が特に便利です。_

`CCTextureCache`クラスは CocosSharp ゲーム開発の重要な部分です。 ほとんどの CocosSharp ゲームを使用して、`CCTextureCache`オブジェクト、任意の数の CocosSharp メソッドが内部的に使用して明示的に、場合でも、*共有*テクスチャのキャッシュ。

このガイドでは、`CCTextureCache`と理由がゲーム開発のために重要です。 具体的に説明します。

 - なぜテクスチャのキャッシュの問題
 - テクスチャの有効期間
 - SharedTextureCache を使用します。
 - 遅延読み込みと AddImage の事前読み込み
 - テクスチャの破棄


## <a name="why-texture-caching-matters"></a>なぜテクスチャのキャッシュの問題

テクスチャの読み込みが時間のかかる操作であるテクスチャには実行時に大量の RAM が必要なゲーム開発で重要な考慮事項は、テクスチャのキャッシュします。

すべてのファイル操作とは、ディスクからのテクスチャの読み込みは、コストの高い操作に指定できます。 テクスチャの読み込みで、読み込まれているファイル (png および jpg イメージの場合と同様) の圧縮を解除されているなどの処理を必要とする場合、余分な時間がかかる場合があります。 テクスチャのキャッシュと、時間、アプリケーションがディスクからファイルを読み込む必要がありますの数を減らすことができます。

前述のように、テクスチャも大量のランタイムのメモリを占有します。 たとえば PNG ファイルがサイズが数キロバイトのみである場合でも、背景画像は iPhone 6 (1344 x 750) の解像度のサイズが 4 メガバイトの RAM – を占有するは。 テクスチャのキャッシュには、アプリ内のテクスチャの参照を共有する方法とも異なるゲームの状態間を遷移するときに、すべてのコンテンツをアンロードする簡単な方法が提供します。


## <a name="texture-lifespan"></a>テクスチャの有効期間

CocosSharp のテクスチャは、アプリの実行の全体の長さのメモリに保持する可能性があります。 または短期的な場合があります。 メモリを最小限に抑えるテクスチャが不要になったときにアプリの使用状況を破棄する必要があります。 もちろんはテクスチャの破棄し、読み込み時間を増やすか、読み込み中にパフォーマンスが低下することができる後で再度読み込むことがあることを意味します。 

メモリ使用量と負荷の時間の間にトレードオフが必要ですテクスチャの多くの場合、読み込み/実行時のパフォーマンス。 少量のテクスチャのメモリを使用するゲームは、必要に応じて、メモリ内ですべてのテクスチャを保持できますが、大規模なゲームが領域を解放するテクスチャをアンロードする必要があります。

次の図は、必要に応じて、テクスチャの読み込みと実行の全体の長さのメモリに保持の簡単なゲームを示しています。

![](texture-cache-images/image1.png "この図では、必要に応じて、テクスチャの読み込みと実行の全体の長さのメモリに保持の簡単なゲームを示しています。")

最初の 2 つのバーは、ゲームの実行時にすぐに必要なテクスチャを表します。 次の 3 つのバーは、テクスチャの各レベルで、必要に応じて読み込むを表します。

ゲームが大きい場合は、十分なデバイスや OS によって提供されるすべての RAM を入力するための十分なテクスチャ最終的にロードします。 これを解決するには、不要になったときに、ゲームはテクスチャ データをアンロードすることがあります。 たとえば、次の図は、必要がなくなったらその後、次のレベルに対する負荷を Level2Texture Level1Texture をアンロードするゲームを示します。 特定の時点のみの 3 つのテクスチャをメモリに保持されることを最終的には。 

![](texture-cache-images/image2.png "最終的な結果は 3 つのみのテクスチャは常にメモリに保持されます。")


上記の図は、アンロード、テクスチャのメモリ使用量を削減することができますが、レベルを再生するプレーヤーが決定した場合、追加の読み込み時間がこの必要がありますを示します。 UITexture と MainCharacter テクスチャが読み込まれ、アンロードされないことに注目すべきです。 これは、ため、常にメモリに保持されているように、すべてのレベルでこれらのテクスチャが必要です。 


## <a name="using-sharedtexturecache"></a>SharedTextureCache を使用します。

CocosSharp はテクスチャを自動的にキャッシュしてからの読み込み時に、`CCSprite`コンス トラクター。 たとえば、次のコードは、テクスチャの 1 つのインスタンスのみを作成します。


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp を自動的にキャッシュ、`star.png`を多数作成の負荷の高い代替手段を避けるためにテクスチャと同じ`CCTexture2D`インスタンス。 これは`AddImage`呼び出されている共有で`CCTextureCache`具体的には、インスタンス`CCTextureCache.SharedTextureCache.Shared`します。 理解する方法、`SharedTextureCache`使用のコードを呼び出すことと同じである次のコードを見て、`CCSprite`文字列パラメーターを持つコンス トラクター。


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` 場合にチェック引数ファイル (ここで`star.png`) 既に読み込まれています。 そうである場合は、キャッシュされたインスタンスが返されます。 ファイル システムから読み込まれてしない場合はのテクスチャへの参照が内部的に格納されている後続`AddImage`呼び出し。 つまり、`star.png`イメージは、1 回のみ読み込むし、追加のディスクへのアクセスまたはその他のテクスチャ メモリの後続の呼び出しは必要ありません。


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>遅延読み込みと AddImage の事前読み込み

`AddImage` により、コードを同じを記述するかどうか、要求されたテクスチャは既に読み込まれていますか。 必要になるまで、つまり、そのコンテンツは読み込まれませんただし、予期しないコンテンツの読み込みのための実行時にパフォーマンスの問題を合わせたこれこともできます。

たとえば、プレイヤーの武器をアップグレードできるゲームを検討してください。 アップグレードすると、武器、弾丸が視覚的は変更され、使用されている新しいテクスチャでします。 コンテンツが遅延読み込みの場合、アップグレードされた武器に関連付けられているテクスチャは読み込まれません最初に、プレーヤーが、アップグレードを獲得する場合、後でなく。 

ゲームをする可能性がこのゲームプレイ mid 読み込み*pop*実行中の短いも顕著な固定であります。 これを防ぐためには、コードが、どのテクスチャは、前面のセットアップし、それらを事前に読み込む必要になる可能性を予測できます。 たとえば、次にことができますのテクスチャを事前に読み込みます。


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

この事前読み込みは、メモリが無駄につながるし、起動時間を増やすことができます。 たとえば、プレーヤー可能性がありますによって表される電源を実際には取得、`powerup3.png`テクスチャ、不必要に読み込まれるようにします。 もちろん、ゲームプレイの潜在的な pop を避けるため、メモリに収まる場合にコンテンツをプリロードすることをお勧めに料金を支払うために必要なコストがあります。


## <a name="disposing-textures"></a>テクスチャの破棄

ゲームには最小限の仕様のデバイスで使用可能なテクスチャ メモリが必要としない場合、テクスチャを破棄する必要はありません。 その一方で、大規模なゲームでは、新しいコンテンツを確保するためにテクスチャ メモリを解放する必要があります。 たとえば、ゲームでは、大量のメモリが環境のテクスチャを格納するを使用できます。 環境は、特定のレベルでのみ使用しするかどう読み込まれたレベルが終了するとします。


### <a name="disposing-a-single-texture"></a>1 つのテクスチャの破棄

1 つのテクスチャを削除する呼び出しが必要です最初、`Dispose`メソッドでは後から手動で削除、`CCTextureCache`します。

次に、そのテクスチャと共に背景スプライトを完全に削除する方法を示します。


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

テクスチャを直接破棄有効にできる、少数のテクスチャが、これを扱うことができますになったときにエラーが発生しやすい大きなテクスチャ セットを処理するとき。

テクスチャは、カスタムの (非共有) に分類できます`CCTextureCache`テクスチャのクリーンアップを簡略化するインスタンス。

たとえば、例が、特定のレベルを使用して、コンテンツがプリロードされた`CCTextureCache`インスタンス。 `CCTextureCache`レベルを定義するクラスのインスタンスを定義することがあります (なる可能性がある、`CCLayer`または`CCScene`)。


```csharp
CCTextureCache levelTextures; 
```

`levelTextures`レベルに固有のテクスチャをプリロードするインスタンスを使用することができます。 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

最後に、レベルの終了時にテクスチャできますを一度にすべて破棄、 `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Dispose メソッドは、これらのテクスチャで使用されるメモリを消去するすべての内部テクスチャで破棄されます。 結合`CCTextureCache.Shared`レベルまたはゲーム モードの特定の`CCTextureCache`結果で一部のテクスチャ全体のゲームといくつかのようにこのガイドの先頭に表示されるダイアグラムのように、レベル終了がアンロードされる永続化のインスタンス。 

![](texture-cache-images/image2.png "CCTextureCache.Shared を組み合わせる、レベル、またはゲーム モード固有 CCTextureCache インスタンスの結果で一部のテクスチャ全体のゲームといくつかのようにこのガイドの先頭に表示されるダイアグラムのように、レベル終了がアンロードされる永続化します。")




## <a name="summary"></a>まとめ

このガイドは、使用する方法を示します、`CCTextureCache`分散メモリの使用状況と実行時のパフォーマンスをクラス。 `CCTexturCache.SharedTextureCache` 明示的にしたり、読み込みおよびアプリケーションのライフ サイクルのテクスチャのキャッシュを暗黙的に使用するには、while`CCTextureCache`メモリ使用量を削減するテクスチャをアンロードするインスタンスを使用できます。

## <a name="related-links"></a>関連リンク

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
