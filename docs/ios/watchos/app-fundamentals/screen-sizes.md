---
title: 画面サイズの使用
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 600b3de6cbf31bd4c3221eb1bf81eda7b4678c09
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-screen-sizes"></a>画面サイズの使用

Apple Watch では、次の 2 つの画面サイズで使用できます。

- **38mm**
  - 136 x 170 論理ピクセル (272 x 340 物理的なピクセル)

- **42mm**
  - 156 x 195 論理ピクセル (312 x 390 物理的なピクセル) です。

設計と、アプリをテストするときに画面のサイズを行う必要があります。

## <a name="watchos-interface-designer"></a>watchOS インターフェイス デザイナー

既定では、Visual Studio for Mac デザイナーが表示されますのインターフェイスのコント ローラーを視聴**Any Apple Watch**です。

![](screen-sizes-images/screen-any-sml.png "デザイナーが表示 Any Apple Watch でのインターフェイスのコント ローラーを監視します。")

サイズ メニューを使用して、編集およびプレビューの使用可能な画面サイズのどちらでも、ストーリー ボード: **38 mm**または**42 mm**:

![](screen-sizes-images/screen-menu-sml.png "38 mm または 42 mm のサイズを選択します。")

大きな画面のサイズが小さい画面に切り捨てられます/非表示になるコンテンツのレンダリング場合があります。
両方のサイズをテストすることを確認します。


### <a name="interface-design"></a>インターフェイスのデザイン

アプリ必要があります画面で、サイズに関係なく、同じコンテンツを表示し、必要がありますを展開または閉じるとして適切な要素です。 属性 Inspector 内での Mac デザイナー用の Visual Studio を使用して、**コンテナーに相対**または**サイズに合わせてコンテンツを**方が優先的固定サイズです。

![](screen-sizes-images/sizeattributepanel-sml.png "コンテナーまたは固定サイズ方が優先的には、コンテンツに合わせてサイズを基準に使用します。")

ウォッチ画面は、黒のベゼルで囲まれている、ため、インターフェイスの周りの余白を提供することはお勧めしません。 要素の自然な枠線が、アプリを形成ベゼルをさせて、画面の端と接することができます。


## <a name="watchos-simulator"></a>watchOS シミュレーター

ときに、シミュレーターでテストを簡単に切り替えるを使用して 2 つの画面サイズ、**ハードウェア > デバイス**メニュー。

![](screen-sizes-images/simulator.png "シミュレーターは、ハードウェア デバイス メニューを使用して 2 つの画面サイズ間で切り替えることができます。")


## <a name="image-resources"></a>イメージ リソース

1 つのアセットにさまざまなサイズでも適切に表示されない場合、複数のイメージ アセットを使用する必要があります。 サイズごとに指定する別のビットマップのイメージ資産カタログを使用します。

![](screen-sizes-images/images-xcassets.png "イメージ資産カタログ エディター")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

また、画面のサイズを決定し、さまざまなイメージの読み込みを完全にコードを使用します。

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

詳しくを使用して、[イメージ コントロール](~/ios/watchos/user-interface/image.md)です。



## <a name="related-links"></a>関連リンク

- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
