---
title: Xamarin. iOS の外観 API
description: iOS では、個々のオブジェクトではなく静的クラスレベルで、アプリケーション内のそのコントロールのすべてのインスタンスに変更が適用されるように、ビジュアルプロパティの設定を適用できます。
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/15/2018
ms.openlocfilehash: 7ba6eca8f74c10254ae93b95725bc73ae100be70
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433850"
---
# <a name="appearance-api-in-xamarinios"></a>Xamarin. iOS の外観 API

_iOS では、個々のオブジェクトではなく静的クラスレベルで、アプリケーション内のそのコントロールのすべてのインスタンスに変更が適用されるように、ビジュアルプロパティの設定を適用できます。_

この機能は `Appearance` 、その機能をサポートするすべての UIKit コントロールの静的プロパティを介して、Xamarin で公開されます。 このため、外観 (濃淡の色や背景画像などのプロパティ) を簡単にカスタマイズして、アプリケーションの外観を統一することができます。 外観 API は iOS 5 で導入されましたが、一部の部分は iOS 9 では非推奨とされていますが、Xamarin の iOS アプリでは、いくつかのスタイルとテーマの効果を実現するのにも適しています。

## <a name="overview"></a>概要

iOS では、多くの UIKit コントロールの外観をカスタマイズして、標準コントロールをアプリケーションに適用するブランドに準拠させることができます。

カスタムの外観を適用するには、次の2つの方法があります。

- **コントロールインスタンスで直接** 実行する–ツールバー、ナビゲーションバー、ボタン、およびスライダーを含む多くのコントロールで、着色色、背景画像、タイトル位置 (およびその他の属性) を設定できます。

- **外観の静的プロパティに既定値を設定** する–各コントロールのカスタマイズ可能な属性は静的プロパティを介して公開され `Appearance` ます。 これらのプロパティに適用したカスタマイズは、プロパティの設定後に作成されるその型のコントロールの既定値として使用されます。

外観サンプルアプリケーションは、次のスクリーンショットに示すように、3つのすべての方法を示しています。

[![外観サンプルアプリケーションでは、3つの方法すべてを示しています。](introduction-to-the-appearance-api-images/appearance01-sml.png)](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

IOS 8 では、表示プロキシは TraitCollections に拡張されています。
 `AppearanceForTraitCollection` 特定の特徴コレクションの既定の外観を設定するために使用できます。 詳細については、「 [ストーリーボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md) 」ガイドを参照してください。

## <a name="setting-appearance-properties"></a>外観プロパティの設定

最初の画面では、静的外観クラスを使用して、次のようにボタンと黄色/オレンジの要素のスタイルを選択します。

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

緑の要素スタイルは、次のように、 `ViewDidLoad` 既定値と *外観* 静的クラスをオーバーライドするメソッドで設定されます。

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Xamarin での UIAppearance の使用

外観 API は、Xamarin. Forms ソリューションで [iOS アプリのスタイル](~/xamarin-forms/platform/ios/formatting.md#uiappearance-api) を設定するときに役立ちます。 クラスのいくつかの行は、 `AppDelegate` [カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成しなくても、特定の配色を実装するのに役立ちます。

### <a name="custom-themes-and-uiappearance"></a>カスタムテーマと UIAppearance

iOS では、特定のコントロールのすべてのインスタンスの外観を強制的に設定するために、 *Uiappearance* api を使用して、ユーザーインターフェイスコントロールの多くの視覚属性を "テーマ" にすることができます。 これは、コントロールの個々のインスタンスではなく、多くのユーザーインターフェイスコントロールクラスの外観プロパティとして公開されます。 静的プロパティの表示プロパティを設定 `Appearance` すると、アプリケーション内のその種類のすべてのコントロールに影響します。

概念について理解を深めるために、例を考えてみましょう。

特定のを `UISegmentedControl` マゼンタの濃淡に変更するには、次のように、画面で特定のコントロールを参照し `ViewDidLoad` ます。

```csharp
sg1.TintColor = UIColor.Magenta;
```

または、デザイナーの [プロパティ] パッドで値を設定します。

[![Properties Pad 濃淡](introduction-to-the-appearance-api-images/propertiespadtint.png)](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

次の画像は、' sg1 ' という名前のコントロールに対してのみ濃淡を設定することを示しています。

[![個々のコントロールの濃淡の設定](introduction-to-the-appearance-api-images/image53.png)](introduction-to-the-appearance-api-images/image53.png#lightbox)

このように多くのコントロールを設定すると、完全に非効率的になります。そのため、 `Appearance` クラス自体で静的プロパティを設定できます。 これを次のコードに示します。

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

次の図は、外観がマゼンタに設定されたセグメント化されたコントロールの両方を示しています。

[![外観コントロールの濃淡の設定](introduction-to-the-appearance-api-images/image54.png)](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` プロパティは、AppDelegate のイベントなど、アプリケーションのライフサイクルの早い段階で設定する `FinishedLaunching` か、影響を受けるコントロールが表示される前に ViewController で設定する必要があります。

詳細については、「 [外観 API の概要](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) 」を参照してください。

## <a name="related-links"></a>関連リンク

- [外観 (サンプル)](/samples/xamarin/ios-samples/appearance)
- [UIAppearance プロトコルのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin. Forms での外観](~/xamarin-forms/platform/ios/formatting.md#uiappearance-api)