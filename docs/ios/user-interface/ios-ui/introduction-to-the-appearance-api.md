---
title: Xamarin.iOS で API の外観
description: iOS では、アプリケーションでそのコントロールのすべてのインスタンスに変更が適用されるように、個々 のオブジェクトではなく、静的クラス レベルでのビジュアル プロパティの設定を適用できます。
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 02b33550451506ef4756f0f7d4400b4f98cef368
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790254"
---
# <a name="appearance-api-in-xamarinios"></a>Xamarin.iOS で API の外観

_iOS では、アプリケーションでそのコントロールのすべてのインスタンスに変更が適用されるように、個々 のオブジェクトではなく、静的クラス レベルでのビジュアル プロパティの設定を適用できます。_

この機能は、静的なを介して Xamarin.iOS で公開`Appearance`サポートされているすべての UIKit コントロールのプロパティです。 アプリケーションの外観を一貫した外観 (濃淡の色と背景のイメージとしてプロパティなど) は簡単にカスタマイズしたがってことができます。 外観 API は 5、iOS で導入され、iOS 9 で廃止されたことの一部がまだ一部のスタイルと Xamarin.iOS アプリでテーマの効果を実現することをお勧めします。

## <a name="overview"></a>概要

iOS では、標準のコントロールをアプリケーションに適用するブランドに準拠する多くの UIKit コントロールの外観のカスタマイズを使用できます。

カスタムの外観を適用する 2 つの方法があります。

- **コントロールのインスタンスに直接**– ツールバー、ナビゲーション バー、ボタンとスライダーを含む多くのコントロールで、濃淡の色、背景イメージとタイトルの位置 (およびその他のいくつかの属性) を設定することができます。

- **外観の静的プロパティの既定値の設定**– コントロールごとにカスタマイズ可能な属性がを介して公開される、`Appearance`静的プロパティです。 これらのプロパティに適用するすべてのカスタマイズは、プロパティを設定した後に作成されるその型の任意のコントロールの既定値として使用されます。

外観サンプル アプリケーションは、これらのスクリーン ショットに示すように、すべての 3 つの方法を示しています。

 [![](introduction-to-the-appearance-api-images/appearance01.png "外観のサンプル アプリケーションは、すべての 3 つの方法を示します")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

8、iOS の時点では、外観プロキシが TraitCollections に拡張されました。
 `AppearanceForTraitCollection` 特定の特徴であるコレクションの既定の外観を設定するために使用します。 詳細を読み取ることができますにこれは、[ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ガイドです。


## <a name="setting-appearance-properties"></a>外観のプロパティの設定

最初の画面で、静的外観クラスを使用して、ボタンと次のように黄色/オレンジ色要素のスタイルを設定します。

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

緑の要素のスタイルに設定は、次のように、`ViewDidLoad`既定値を上書きする方法、および*外観*静的クラス。

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

外観 API をする場合に役立ちます[iOS アプリのスタイル設定](~/xamarin-forms/platform/ios/theme.md#uiappearance)Xamarin.Forms ソリューションでします。 いくつかの行で、`AppDelegate`を作成しなくても、特定のカラー スキームを実装するクラスが役立つことが、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)です。


### <a name="custom-themes-and-uiappearance"></a>カスタム テーマと UIAppearance

によりユーザーの多くの視覚属性「テーマ」を使用するインターフェイスの制御、iOS、 *UIAppearance*同じ外観にする特定のコントロールのすべてのインスタンスを強制するための Api です。 これは、多くのユーザー インターフェイス コントロール クラスではなく、コントロールの個々 のインスタンスの外観プロパティとして公開されます。 静的に表示プロパティを設定`Appearance`プロパティは、アプリケーションでは、その型のすべてのコントロールに影響します。

概念を理解するのには、例を検討してください。

特定の変更を`UISegmentedControl`マゼンタ濃淡には、特定のコントロールで次のように、画面上で参照`ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

または、デザイナーのプロパティ パッドで、値を設定します。 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "パッド濃淡のプロパティ")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

次の図では、濃淡 'sg1' をという名前のコントロールのみで設定を示しています。

 [![](introduction-to-the-appearance-api-images/image53.png "個々 のコントロール濃淡の設定")](introduction-to-the-appearance-api-images/image53.png#lightbox)

この方法で多くのコントロールを設定できない完全に効率的な静的代わりにセットアップできるように`Appearance`クラス自体のプロパティです。 これは、次のコードで示されます。

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

次の図は、両方のセグメント化されたコントロールを示していますマゼンタに設定の外観のようになりました。

 [![](introduction-to-the-appearance-api-images/image54.png "外観の制御濃淡の設定")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` プロパティ設定する必要があるアプリケーションのライフ サイクルの早い段階でなど AppDelegate の`FinishedLaunching`イベント、または影響を受けるコントロールを表示する前に、ViewController にします。


参照してください、[外観 API の概要について](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)詳細についてはします。


## <a name="related-links"></a>関連リンク

- [外観 (サンプル)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [UIAppearance プロトコル リファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin.Forms での外観](~/xamarin-forms/platform/ios/theme.md#uiappearance)
