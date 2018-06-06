---
title: Xamarin.iOS でレイアウト オプション
description: このドキュメントでは、Xamarin.iOS 内のユーザー インターフェイスをレイアウトするさまざまな方法について説明します。 これは、自動と自動レイアウトについて説明します。
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bad29eae308c8ca9f7228a1cbdfd69940894cf34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790117"
---
# <a name="layout-options-in-xamarinios"></a>Xamarin.iOS でレイアウト オプション

これには、ビューがサイズ変更、または回転したときに、レイアウトを制御するための 2 つの異なるメカニズムがあります。

-  **自動**– デザイナーで、自動インスペクターを設定する方法を提供する、`AutoresizingMask`プロパティです。 これにより、コントロールがコンテナーの端に固定や、そのサイズを修正、ことです。 サイズの自動変更は、iOS のすべてのバージョンで機能します。 詳細については、以下に記載されて
-  **レイアウトを自動**–、UI コントロールのリレーションシップの粒度の細かい制御を許可する iOS 6 で導入された機能です。 デザイン サーフェイス上の他の要素に対して要素の位置のコントロールが許可されます。 このトピックで詳しく説明については、 [Xamarin iOS デザイナーで自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)ガイドです。

## <a name="autosizing"></a>サイズの自動変更

ユーザーがデバイスを回転するときに、向きの変更など、ウィンドウのサイズと、システムは自動的にサイズを変更の自動規則に従ってそのウィンドウ内のビュー。 これらの規則を設定することができます (C#) を使用して、`AutoresizingMask`のプロパティ、`UIView`または、**プロパティ パッド**デザイナー、下図のように、iOS の。

 [![](layout-options-images/image41.png "Visual Studio for Mac デザイナー")](layout-options-images/image41.png#lightbox)

これにより、コントロールのサイズと場所を手動で指定する、コントロールが選択されている場合に選択するだけでなく**自動**動作します。 次のスクリーン ショットに示すようにおおよび使用できます、springs 脚柱自動コントロール内の親を選択したビューのリレーションシップを定義します。

 [![](layout-options-images/image42.png "Visual Studio for Mac デザイナー")](layout-options-images/image42.png#lightbox)

調整する、 *spring*親ビューの高さまたは幅に基づくサイズを変更するビューが発生されます。 調整、*見せつけて*自体とその特定のエッジに、親ビューの間の距離が一定の管理ビューを作成します。

これらの設定は、コードでも設定できます。

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


自動設定をテストするには、有効にするさまざまな**サポートされているデバイスの向き**プロジェクトのオプション で。

 [![](layout-options-images/image43a.png "自動設定")](layout-options-images/image43a.png#lightbox)

分離コードで次のコードは、水平方向にサイズを変更する 2 つのテキスト コントロールを使用できます。

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


デザイナーを使用してコントロールを調整することもできます。 以下が示すように、脚柱を選択すると、ビューの一番下から切り抜かれてなく右揃えを維持するイメージが発生します。

 [![](layout-options-images/autoresize.png "オートローテーション")](layout-options-images/autoresize.png#lightbox)

これらのスクリーン ショットは、コントロールのサイズを変更または画面を回転したときに再配置を示します。

 [![](layout-options-images/image44a.png "オートローテーション")](layout-options-images/image44a.png#lightbox)

テキスト ビューとテキスト フィールドの両方にストレッチすると、同じのまま維持いて理由のための余白を右、`FlexibleWidth`設定します。 イメージに存在、上と左余白柔軟なので、保存、下と右余白 – 画面を回転したときにビューで、イメージを維持することを意味します。 複雑なレイアウトには、これらの設定の組み合わせすべて表示されているコントロールのユーザー インターフェイスの一貫性を維持して、ビューの境界を変更 (回転やその他のサイズ変更イベント) のためと重複しないようにするコントロールを防ぐために通常必要とします。





## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
