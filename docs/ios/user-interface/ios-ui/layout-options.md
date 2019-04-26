---
title: Xamarin.iOS でのレイアウト オプション
description: このドキュメントでは、Xamarin.iOS でのユーザー インターフェイスをレイアウトするさまざまな方法について説明します。 自動サイズ調整と自動レイアウトがについて説明します。
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b35149028763691c17fe526673d023cc9b707c28
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61246061"
---
# <a name="layout-options-in-xamarinios"></a>Xamarin.iOS でのレイアウト オプション

これには、ビューがサイズ変更、または回転したときに、レイアウトを制御するための 2 つの異なるメカニズムがあります。

-  **自動サイズ調整**– デザイナーで、自動サイズ調整インスペクターを設定する方法を提供する、`AutoresizingMask`プロパティ。 これで、コントロールがコンテナーの端に固定するや、そのサイズを修正します。 自動サイズ調整は、iOS のすべてのバージョンで動作します。 これは、以下で詳しく説明されています。
-  **自動レイアウト**– UI コントロールの関係の詳細に制御を許可する iOS 6 で導入された機能です。 デザイン サーフェイス上の他の要素を基準として要素の位置をコントロールできるようになります。 このトピックで詳しく説明については、 [Xamarin iOS Designer での自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)ガイド。

## <a name="autosizing"></a>自動サイズ調整

ユーザーがデバイスの回転と向きの変更など、ウィンドウをサイズ変更時に、システムは自動サイズ調整の規則に従って、そのウィンドウ内のビューに自動的にサイズ変更されます。 これらの規則を設定できるC#を使用して、`AutoresizingMask`のプロパティ、`UIView`または、 **Properties Pad** iOS デザイナー、下図の。

 [![](layout-options-images/image41.png "Visual Studio for Mac のデザイナー")](layout-options-images/image41.png#lightbox)

これにより、コントロールのサイズと場所を手動で指定することで、コントロールが選択したときに選択すると**自動サイズ調整**動作します。 次のスクリーン ショットのように、ここで使用できます springs と struts 自動サイズ調整コントロールの親を選択したビューのリレーションシップを定義します。

 [![](layout-options-images/image42.png "Visual Studio for Mac のデザイナー")](layout-options-images/image42.png#lightbox)

調整を*spring*幅または高さの親ビューに基づくサイズを変更するビューが発生されます。 調整を*見せつけて*自体とその特定のエッジに、親ビューの間の距離が一定の管理ビューになります。

これらの設定は、コードでも設定できます。

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


自動サイズ調整の設定をテストするさまざまなを有効にする**サポートされているデバイスの向き**プロジェクトのオプション。

 [![](layout-options-images/image43a.png "自動サイズ調整の設定")](layout-options-images/image43a.png#lightbox)

分離コードは、次のコードは、水平方向にサイズを変更する 2 つのテキスト コントロールを使用できます。

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


デザイナーを使用してコントロールを調整もできます。 以下が示すように、struts を選択すると、ビューの一番下からクリッピングなしの右揃えを維持するイメージが発生します。

 [![](layout-options-images/autoresize.png "オートローテーション")](layout-options-images/autoresize.png#lightbox)

これらのスクリーン ショットは、コントロールのサイズを変更または再配置、画面を回転したときに表示します。

 [![](layout-options-images/image44a.png "オートローテーション")](layout-options-images/image44a.png#lightbox)

テキスト ビューとテキスト フィールドを両方同じのまま維持する拡張し、理由のための余白を右に注意してください、`FlexibleWidth`設定します。 イメージは、上および左余白、柔軟なは、下と右余白 – 画面を回転したときに、イメージをビューに維持することが維持されます。 複雑なレイアウトには、これらの設定の組み合わせすべて表示されているコントロールのユーザー インターフェイスの整合性を維持し、コントロールがビューの範囲を変更 (回転、またはその他のサイズ変更イベント) が原因と重複していることを防ぐために通常必要があります。





## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
