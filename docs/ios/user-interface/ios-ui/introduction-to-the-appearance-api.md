---
title: Xamarin.iOS で外観 API
description: iOS では、アプリケーションでそのコントロールのすべてのインスタンスに変更が適用されるように、個々 のオブジェクトではなく、静的クラス レベルでのビジュアルのプロパティの設定を適用できます。
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0dd9832a2e4dd0803f92d6e3923fe178252211f4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103570"
---
# <a name="appearance-api-in-xamarinios"></a>Xamarin.iOS で外観 API

_iOS では、アプリケーションでそのコントロールのすべてのインスタンスに変更が適用されるように、個々 のオブジェクトではなく、静的クラス レベルでのビジュアルのプロパティの設定を適用できます。_

この機能は、静的なを使用して Xamarin.iOS で公開`Appearance`プロパティをサポートしているすべての UIKit コントロール。 アプリケーションの外観を一貫性のある視覚的な外観 (濃淡の色と背景のイメージとしてプロパティなど) は簡単にカスタマイズしたがってことができます。 外観 API は iOS 5 で導入され、いくつかのスタイルとテーマ Xamarin.iOS アプリの効果を実現するために適切な方法ではまだ中の一部が iOS 9 で非推奨とされました。

## <a name="overview"></a>概要

iOS では、標準的なコントロールをアプリケーションに適用するブランド化に準拠している多くの UIKit コントロールの外観をカスタマイズして使用できます。

カスタムの外観を適用する 2 つのさまざまな方法はあります。

- **コントロールのインスタンスに対して直接**– ツールバー、ナビゲーション バー、ボタンやスライダーなど多くのコントロールで、濃淡の色、背景イメージとタイトルの位置 (およびその他のいくつかの属性) を設定することができます。

- **外観の静的プロパティの既定値の設定**– 経由で公開される各コントロールのカスタマイズ可能な属性、`Appearance`静的プロパティ。 これらのプロパティに適用するカスタマイズは、プロパティを設定した後に作成されるその型の任意のコントロールの既定として使用されます。

外観のサンプル アプリケーションではこれらのスクリーン ショットに示すようにすべての 3 つの方法を示しています。

 [![](introduction-to-the-appearance-api-images/appearance01.png "外観のサンプル アプリケーションは、次の 3 つすべての方法を示しています。")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

IOS 8 の時点で外観のプロキシは TraitCollections に拡張されました。
 `AppearanceForTraitCollection` 特定の特徴であるコレクションの既定の外観を設定するために使用します。 詳細については、これには、[ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ガイド。


## <a name="setting-appearance-properties"></a>外観のプロパティの設定

最初の画面で外観の静的クラスを使用して、ボタンと次のように黄色/オレンジ色要素のスタイル設定にします。

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

緑の要素のスタイルはで、このような設定、`ViewDidLoad`メソッドの既定値を上書きして、*外観*静的クラス。

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Xamarin.Forms で UIAppearance の使用

外観 API されます[iOS アプリのスタイル設定](~/xamarin-forms/platform/ios/theme.md#uiappearance)Xamarin.Forms ソリューションです。 いくつかの行で、`AppDelegate`を作成しなくても、特定のカラー スキームを実装するクラスが役立つことができます、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)します。


### <a name="custom-themes-and-uiappearance"></a>カスタム テーマと UIAppearance

により、ユーザーの多くの視覚属性「テーマ」を使用することのインターフェイス コントロール、iOS、 *UIAppearance*同じ外観にする特定のコントロールのすべてのインスタンスを強制する Api。 これは、多くのユーザー インターフェイス コントロール クラスでは、コントロールの個々 のインスタンスではなく、外観プロパティとして公開されます。 静的に表示プロパティを設定`Appearance`プロパティが、アプリケーションでは、その型のすべてのコントロールに影響します。

概念を理解するには、例を検討します。

特定の変更を`UISegmentedControl`マゼンタ濃淡には、特定のコントロールで次のように、画面上で参照`ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

または、デザイナーの [プロパティ] タブで、値を設定します。 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "プロパティ パッド濃淡")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

次の図は、'sg1' という名前のコントロールのみで、濃淡設定ことを示しています。

 [![](introduction-to-the-appearance-api-images/image53.png "個々 のコントロールの濃淡の設定")](introduction-to-the-appearance-api-images/image53.png#lightbox)

この方法で多くのコントロールを設定するのには効率的ではありません完全に静的な代わりにセットアップできるように`Appearance`クラス自体のプロパティ。 これは、次のコードに示されます。

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

次の図は、マゼンタに設定の外観のようになりました両方のセグメント化されたコントロールを示します。

 [![](introduction-to-the-appearance-api-images/image54.png "外観の制御濃淡の設定")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` プロパティは、アプリケーションのライフ サイクルの早い段階でなどで設定 AppDelegate の`FinishedLaunching`イベント、または影響を受けるコントロールを表示する前に、ViewController でします。


参照してください、[外観 API の概要について](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)詳細な情報。


## <a name="related-links"></a>関連リンク

- [外観 (サンプル)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [UIAppearance プロトコルのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin.Forms での外観](~/xamarin-forms/platform/ios/theme.md#uiappearance)
