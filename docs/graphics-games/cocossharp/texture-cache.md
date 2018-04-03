---
title: CCTextureCache を使用してテクスチャ キャッシュ
description: CocosSharp の CCTextureCache クラスは、整理、キャッシュ、およびコンテンツをアンロードする標準的な方法を提供します。 大規模なゲーム RAM、グループ化とテクスチャの破棄のプロセスを簡略化に完全に合わない可能性がありますが特に便利です。
ms.topic: article
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 350a454bc94c796b34cfeeb319481919b18d334f
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="texture-caching-using-cctexturecache"></a>テクスチャ キャッシュ CCTextureCache を使用します。

_CocosSharp の CCTextureCache クラスは、整理、キャッシュ、およびコンテンツをアンロードする標準的な方法を提供します。大規模なゲーム RAM、グループ化とテクスチャの破棄のプロセスを簡略化に完全に合わない可能性がありますが特に便利です。_

`CCTextureCache`クラスは CocosSharp ゲーム開発の不可欠な一部です。 ほとんどの CocosSharp ゲームを使用して、`CCTextureCache`オブジェクトを CocosSharp の数のメソッドを内部的に使用明示されていない場合でも、*共有*テクスチャのキャッシュします。

ここでは、`CCTextureCache`およびゲーム開発の重要である理由。 具体的に説明します。

 - テクスチャ キャッシュの問題の原因
 - テクスチャの有効期間
 - SharedTextureCache を使用します。
 - 遅延 AddImage に事前読み込みと読み込み
 - テクスチャを破棄しています。


## <a name="why-texture-caching-matters"></a>テクスチャ キャッシュの問題の原因

テクスチャ キャッシュはゲーム開発の重要な考慮事項のテクスチャの読み込み時間のかかる操作は、テクスチャには、実行時に大量の RAM が必要があります。

同様に、ファイルの操作は、ディスクからのテクスチャの読み込みは、コストの高い操作に指定できます。 テクスチャの読み込みで、読み込まれているファイル (png、jpg イメージの場合と同様) を圧縮解除されているような処理が必要な場合、余分な時間がかかる場合があります。 テクスチャ キャッシュと、アプリケーションがディスクからファイルを読み込む必要がありますを時間の数を削減できます。

前述のように、テクスチャもランタイム メモリの消費量を占有します。 たとえば PNG ファイルがサイズで数キロバイトのみである場合でも、iPhone 6 (1344 x 750) の解決にサイズの背景イメージが 4 メガバイトの RAM – を占有するは。 テクスチャのキャッシュは、さまざまなゲーム状態間を遷移するときに、すべてのコンテンツをアンロードする簡単な方法と、アプリ内のテクスチャの参照を共有する方法を提供します。


## <a name="texture-lifespan"></a>テクスチャの有効期間

CocosSharp テクスチャは、アプリの実行の全体にわたってメモリに保持される場合があります、または短期的な場合があります。 メモリを最小限に抑えるテクスチャ不要になったときにアプリを使用する必要があります破棄します。 もちろんはテクスチャの破棄し、再度読み込んだ後で、読み込み時間を増やすことも時の負荷のパフォーマンスが低下する可能性があることを意味します。 

メモリ使用量と読み込み時間の間にトレードオフ テクスチャが多くの場合に読み込む必要があります/実行時のパフォーマンスです。 少量のテクスチャのメモリを使用するゲームは、必要に応じて、メモリ内ですべてのテクスチャを保つことができますが、大規模なゲームが領域を解放するテクスチャをアンロードする必要があります。

次の図は、必要に応じて、テクスチャの読み込みと実行の全体にわたってメモリに保持する単純なゲームを示しています。

![](texture-cache-images/image1.png "この図では、必要に応じて、テクスチャの読み込みと実行の全体にわたってメモリに保持する単純なゲームを示しています。")

最初の 2 つのバーでは、ゲームの実行時にすぐに必要なテクスチャを表します。 次の 3 つのバーは、必要に応じて読み込まれて、それぞれのレベルのテクスチャを表します。

ゲームが大きい場合は十分なことをデバイスと OS によって提供されるすべてのメモリを埋めるための十分なテクスチャ最終的にロードします。 これを解決するには、必要がなくなったときに、ゲームはテクスチャ データをアンロードすることがあります。 たとえば、次の図は、不要になった、その後、次のレベルについて Level2Texture を負荷すると、Level1Texture をアンロードするゲームを示します。 最終結果は、のみの 3 つのテクスチャは特定の時点でメモリに保持されます。 

![](texture-cache-images/image2.png "最終結果は、3 つだけのテクスチャは特定の時点でメモリに保持されます。")


上記の図は、アンロード、テクスチャのメモリ使用量を削減することができますが、追加のロード時間が必要、レベルを再生するプレーヤーが決定した場合を示します。 注目に値します UITexture と MainCharacter テクスチャが読み込まれてし、アンロードされません。 つまり、これらのテクスチャが必要であるすべてのレベルのメモリ内で常に保持されているためです。 


## <a name="using-sharedtexturecache"></a>SharedTextureCache を使用します。

CocosSharp はテクスチャを自動的にキャッシュを使用して読み込むときに、`CCSprite`コンス トラクターです。 たとえば、次のコードでは、1 つのテクスチャ インスタンスのみを作成します。


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp が自動的にキャッシュされる、`star.png`を多数作成の高価な代替手段を回避するのにテクスチャと同じ`CCTexture2D`インスタンス。 これを行う`AddImage`呼び出されている共有で`CCTextureCache`具体的には、インスタンス`CCTextureCache.SharedTextureCache.Shared`です。 理解する方法、`SharedTextureCache`使用コードを呼び出すことと同じである次のコードを見ていきましょう、`CCSprite`文字列パラメーターを持つコンス トラクター。


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` 場合にチェック引数ファイル (このケースで`star.png`) 既に読み込まれています。 その場合、キャッシュされたインスタンスが返されます。 ファイル システムから読み込まれていない、しのテクスチャへの参照が内部的に格納されている場合後続`AddImage`呼び出しです。 つまり、`star.png`イメージが一度読み込まれるだけと、追加のディスクへのアクセスやその他のテクスチャ メモリの後続の呼び出しは必要ありません。


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>遅延 AddImage に事前読み込みと読み込み

`AddImage` により、コードを同じを記述するかどうか、要求されたテクスチャは既に読み込まれていないか。 必要になるまで、このコンテンツは、意味は読み込まれませんただし、予期しないコンテンツの読み込みのための実行時にパフォーマンスの問題これさせることができます。

たとえば、プレーヤーの武器をアップグレードするゲームを検討してください。 アップグレードする際、武器と弾丸は視覚的に変更され、使用されている新しいにテクスチャでします。 コンテンツが遅延読み込みの場合、アップグレードされた武器に関連付けられているテクスチャは読み込まれません最初に、わけではなく、後で、プレーヤーが、アップグレードを獲得する場合。 

このゲーム プレイ中旬読み込みには、ゲームをする可能性があります*pop*実行中の短いも顕著な固定であります。 これを防ぐためには、コードが、どのテクスチャは、前面の設定し、それらを事前に読み込む必要になる可能性を予測できます。 たとえば、次は使用できますにテクスチャを事前に読み込みます。


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

この事前読み込みでは、無駄なメモリにより、開始時間を増やすことができます。 たとえば、プレーヤーが実際にを取得する電源投入によって表される、`powerup3.png`テクスチャ、不必要に読み込まれるようにします。 もちろん、ゲームの潜在的な pop を回避するため、メモリに収まる場合にコンテンツをプリロードすることをお勧めする費用のために必要なコストがあります。


## <a name="disposing-textures"></a>テクスチャを破棄しています。

ゲームには、最低限の仕様のデバイスで使用できるよりも多くのテクスチャ メモリが必要としない場合、テクスチャを破棄する必要はありません。 一方、大規模なゲームは、新しいコンテンツを確保するためにテクスチャ メモリを解放する必要があります。 たとえば、ゲームは環境のテクスチャを格納するメモリの消費量を使用する可能性があります。 環境は、特定のレベルでのみ使用し、そのするかどう読み込まれたレベルが終了するとします。


### <a name="disposing-a-single-texture"></a>1 つのテクスチャを破棄しています。

1 つのテクスチャを削除する呼び出しは必要最初、`Dispose`メソッド後から手動で削除、`CCTextureCache`です。

バック グラウンド スプライトと共にそのテクスチャを完全に削除する方法を次に示します。


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

テクスチャを直接破棄できる効果的である少数のテクスチャが、これを処理することができますになったときにエラーが発生しやすいサイズの大きいテクスチャのセットを処理する場合。

テクスチャ グループ分けできます (共有されない) カスタム`CCTextureCache`テクスチャのクリーンアップを簡略化するインスタンス。

たとえば、例を特定のレベルを使用してコンテンツをプリロード`CCTextureCache`インスタンス。 `CCTextureCache`インスタンスは、レベルを定義するクラスで定義される場合があります (である、`CCLayer`または`CCScene`)。


```csharp
CCTextureCache levelTextures; 
```

`levelTextures`インスタンス レベルに固有のテクスチャを事前に使用できます。 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

最後に、レベルの終了時にテクスチャできますを一度にすべて破棄、 `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Dispose メソッドは、これらのテクスチャによって使用されるメモリをオフにすると、すべての内部テクスチャを破棄します。 結合`CCTextureCache.Shared`レベルまたはゲーム モードの特定`CCTextureCache`ゲーム全体については、およびいくつかのレベルで、終了には、このガイドの先頭に表示されるようなアンロードされる永続化する一部のテクスチャで結果インスタンス。 

![](texture-cache-images/image2.png "結果を含む、レベル、またはゲーム モード固有 CCTextureCache インスタンス レベルで、終了には、このガイドの先頭に表示されるようなアンロードされている一部のゲーム全体については、して永続化する一部のテクスチャで CCTextureCache.Shared を組み合わせること。")




## <a name="summary"></a>まとめ

このガイドを使用する方法を示しています、`CCTextureCache`残高メモリの使用状況と実行時のパフォーマンスへのクラスです。 `CCTexturCache.SharedTextureCache` ときに暗黙的に読み込んで、アプリケーションの有効期間のテクスチャをキャッシュするために使用、または明示的にできる`CCTextureCache`インスタンスは、メモリ使用量を削減するテクスチャをアンロードするために使用できます。

## <a name="related-links"></a>関連リンク

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
