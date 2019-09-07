---
title: Xamarin での watchOS 画面サイズの操作
description: このドキュメントでは、さまざまな watchOS 画面サイズを操作する方法について説明します。 WatchOS インターフェイスデザイナー、watchOS シミュレーター、およびイメージリソースについて説明します。
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: e9c87b76dc6845962450b8cb6fab921ea1748832
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768324"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>Xamarin での watchOS 画面サイズの操作

Apple Watch は、次の2つの画面サイズで使用できます。

- **38mm**
  - 136 x 170 論理ピクセル (272 x 340 物理ピクセル)

- **42mm**
  - 156 x 195 論理ピクセル (312 x 390 物理ピクセル)。

アプリの設計とテストを行うときは、画面のサイズを考慮する必要があります。

## <a name="watchos-interface-designer"></a>watchOS インターフェイスデザイナー

既定では、Visual Studio for Mac デザイナーでは、**任意の Apple Watch**に watch interface controller が表示されます。

![](screen-sizes-images/screen-any-sml.png "デザイナーでは、watch インターフェイスコントローラーが任意の Apple Watch に表示されます。")

[サイズ] メニューを使用して、使用可能な画面サイズのいずれかでストーリーボードを編集およびプレビューします。**38mm**または**42 mm**:

![](screen-sizes-images/screen-menu-sml.png "38mm または 42 mm のサイズを選択します。")

画面のサイズが大きいほど、小さい画面では、切り詰められたり非表示になるコンテンツが表示されることがあります。
必ず両方のサイズでテストしてください。

### <a name="interface-design"></a>インターフェイスのデザイン

アプリでは、サイズに関係なく同じコンテンツを画面に表示する必要があります。また、必要に応じて、要素を展開またはコントラクトします。 Visual Studio for Mac デザイナーの属性インスペクターでは、コンテンツを固定サイズに**合わせるために**、コンテナーまたはサイズ**に対して相対的**なを使用する必要があります。

![](screen-sizes-images/sizeattributepanel-sml.png "コンテンツを固定サイズに合わせるために、コンテナーまたはサイズに対して相対的に使用します")

[ウォッチ] 画面は黒いベゼルで囲まれているため、インターフェイスの周囲に埋め込みを指定することはお勧めしません。 要素を画面の端に対して残し、ベゼルがアプリの周りを自然に境界線にするようにします。

## <a name="watchos-simulator"></a>watchOS シミュレーター

シミュレーターでテストする場合、 **[ハードウェア > デバイス]** メニューを使用して、2つの画面サイズを簡単に切り替えることができます。

![](screen-sizes-images/simulator.png "シミュレーターでは、[ハードウェアデバイス] メニューを使用して、2つの画面サイズを切り替えることができます。")

## <a name="image-resources"></a>イメージリソース

1つのアセットがさまざまなサイズで適切に表示されない場合は、複数のイメージアセットを使用する必要があります。 イメージ資産カタログを使用すると、サイズごとに個別のビットマップを指定できます。

![](screen-sizes-images/images-xcassets.png "イメージ資産カタログエディター")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

または、コードを使用して画面サイズを決定し、異なるイメージをまとめて読み込みます。

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

詳細については、「[イメージコントロール](~/ios/watchos/user-interface/image.md)の使用」を参照してください。

## <a name="related-links"></a>関連リンク

- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
