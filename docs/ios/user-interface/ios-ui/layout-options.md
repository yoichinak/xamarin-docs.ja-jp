---
title: Xamarin. iOS のレイアウトオプション
description: このドキュメントでは、Xamarin. iOS でユーザーインターフェイスをレイアウトするさまざまな方法について説明します。 自動サイズ調整と自動レイアウトについて説明します。
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 2514287fe06216d62b994cf19ba8f0901dd36c49
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003321"
---
# <a name="layout-options-in-xamarinios"></a>Xamarin. iOS のレイアウトオプション

ビューのサイズ変更時または回転時にレイアウトを制御するには、次の2つの異なるメカニズムがあります。

- **自動サイズ調整**–デザイナーの自動サイズ調整 inspector は、`AutoresizingMask` のプロパティを設定する方法を提供します。 これにより、コントロールをコンテナーの端に固定したり、サイズを修正したりすることができます。 自動サイズ調整は、iOS のすべてのバージョンで動作します。 詳細については、以下を参照してください。
- **自動レイアウト**– iOS 6 で導入され、UI コントロールの関係をきめ細かく制御できる機能です。 これにより、デザインサーフェイス上の他の要素を基準とした要素の位置を制御できます。 このトピックの詳細については、「 [Xamarin IOS Designer を使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)」を参照してください。

## <a name="autosizing"></a>自動サイズ調整

ユーザーがウィンドウのサイズを変更すると (デバイスが回転して向きが変化した場合など)、自動サイズ調整の規則に従って、ウィンドウ内のビューのサイズが自動的に変更されます。 これらのルールは、次C#に示すように、`UIView`の`AutoresizingMask`プロパティを使用して、または iOS デザイナーの**Properties Pad**で設定できます。

 [![](layout-options-images/image41.png "Visual Studio for Mac Designer")](layout-options-images/image41.png#lightbox)

コントロールを選択すると、コントロールの位置とサイズを手動で指定できるだけでなく、**自動サイズ調整**の動作を選択することもできます。 次のスクリーンショットに示すように、自動サイズ調整コントロールのスプリングと struts を使用して、選択したビューとその親との関係を定義できます。

 [![](layout-options-images/image42.png "Visual Studio for Mac Designer")](layout-options-images/image42.png#lightbox)

*Spring*を調整すると、親ビューの幅または高さに基づいて、ビューのサイズが変更されます。 *Strut*を調整すると、ビューはその特定のエッジ上で、その親ビューとその親ビューの間の一定の距離を維持します。

これらの設定は、コードで設定することもできます。

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```

自動サイズ調整の設定をテストするには、プロジェクトのオプションで、**サポートされている**さまざまなデバイスの向きを有効にします。

 [![](layout-options-images/image43a.png "Autosizing Settings")](layout-options-images/image43a.png#lightbox)

このコードでは、次のコードを使用できます。これにより、2つのテキストコントロールが水平方向にサイズ変更されます。

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```

デザイナーを使用してコントロールを調整することもできます。 次に示すように struts を選択すると、ビューの下部からクリップされずに画像が右に固定されます。

 [![](layout-options-images/autoresize.png "Autorotation")](layout-options-images/autoresize.png#lightbox)

これらのスクリーンショットは、画面の回転時にコントロールのサイズや位置を変更する方法を示しています。

 [![](layout-options-images/image44a.png "Autorotation")](layout-options-images/image44a.png#lightbox)

`FlexibleWidth` の設定により、テキストビューとテキストフィールドの両方が拡張され、同じ左右の余白が維持されることに注意してください。 画像は、上下左右の余白を維持します。つまり、画面の回転時に画像を表示したままにします。 通常、複雑なレイアウトでは、表示されているすべてのコントロールに対してこれらの設定を組み合わせて、ユーザーインターフェイスの一貫性を保ち、ビューの境界が変更されたとき (回転やその他のサイズ変更イベントによって) コントロールの重なりを防止する必要があります。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
