---
title: WatchOS で Xamarin の画面サイズの使用
description: このドキュメントでは、さまざまな watchOS 画面サイズに対応する方法について説明します。 WatchOS インターフェイス デザイナー、watchOS シミュレーター、について説明し、イメージ リソース。
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: b2f4cc71c1993e51ed55b51edd7c50d393e60873
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61412909"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>WatchOS で Xamarin の画面サイズの使用

Apple Watch の 2 つの画面サイズで提供されています。

- **38mm**
  - 136 x 170 論理ピクセル (272 x 340 物理ピクセル単位)

- **42mm**
  - 156 x 195 論理ピクセル (312 x 390 物理ピクセル単位) です。

設計とアプリをテストするときに、画面のサイズを行う必要があります。

## <a name="watchos-interface-designer"></a>watchOS インターフェイス デザイナー

既定では、Visual Studio for Mac のデザイナーが表示されますでインターフェイス コント ローラーを見る**Any Apple Watch**します。

![](screen-sizes-images/screen-any-sml.png "デザイナーが表示されますが、Apple Watch でのインターフェイス コント ローラーをご覧ください。")

編集およびプレビューで利用可能な画面サイズのいずれかのストーリー ボード メニューのサイズを使用します。**38 mm**または**42 mm**:

![](screen-sizes-images/screen-menu-sml.png "38 mm または 42 mm のサイズを選択します。")

大きな画面サイズは、小さい画面で切り捨て/非表示になるコンテンツをレンダリングして場合があります。
必ず両方のサイズでテストしてください。


### <a name="interface-design"></a>インターフェイスのデザイン

アプリする必要があります画面で、サイズに関係なく、同じコンテンツを表示しする必要がありますを展開または閉じるとして適切な要素。 属性インスペクターでの Mac のデザイナーの Visual Studio で使用する必要があります**コンテナーからの相対**または**サイズに合わせてコンテンツを**方が優先的固定サイズです。

![](screen-sizes-images/sizeattributepanel-sml.png "固定サイズよりもコンテナーへの相対パスまたはコンテンツに合わせてサイズを使用します。")

ウォッチ画面は、黒のベゼルで囲まれている、ため、インターフェイスの周囲にパディングを提供することは推奨されません。 要素のフォーム アプリの境界線を自然なドアを行い、画面の端と接することができます。


## <a name="watchos-simulator"></a>watchOS シミュレーター

ときに、シミュレーターでテストを簡単に切り替えるを使用して 2 つの画面サイズ、**ハードウェア > デバイス**メニュー。

![](screen-sizes-images/simulator.png "シミュレーターは、ハードウェア デバイス メニューを使用して 2 つの画面サイズを切り替えることができます。")


## <a name="image-resources"></a>イメージ リソース

1 つのアセットにさまざまなサイズでも適切に表示されない場合は、複数のイメージ アセットを使用してください。 各サイズに指定する別のビットマップのイメージ資産カタログを許可します。

![](screen-sizes-images/images-xcassets.png "イメージの資産カタログ エディター")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

または、画面サイズを決定し、完全に異なるイメージを読み込むコードを使用します。

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

詳細についてを使用して、[イメージ コントロール](~/ios/watchos/user-interface/image.md)します。



## <a name="related-links"></a>関連リンク

- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
