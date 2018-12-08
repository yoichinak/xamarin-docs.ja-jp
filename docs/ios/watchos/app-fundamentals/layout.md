---
title: WatchOS で Xamarin のレイアウトの操作
description: このドキュメントでは、Xamarin を使用して、watchOS レイアウトを作成する方法について説明します。 インターフェイス コント ローラー、グループ、区切り記号、およびコンテンツ コントロールがについて説明します。
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 9a9bec364990f683a59e6ddce1a536950cdf3861
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059614"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>WatchOS で Xamarin のレイアウトの操作

Apple Watch のレイアウトを設計[画面サイズ](~/ios/watchos/app-fundamentals/screen-sizes.md)固有の課題を表示します。

## <a name="design-tips"></a>デザインに関するヒント

重要なポイント: 読み取り可能で、大規模な本の指での小さなウォッチ画面では、使用可能なユーザー インターフェイスを作成します。 デザインするのに陥る可能性はありません、 **iOS シミュレーター** (これは非常に大きな表示) と**マウス ポインター** (これは小さなタッチのターゲットを持つ動作します)。

- 黒の背景を使用して、-、ウォッチのベゼルを黒より大きな画面の効果を作成します。

- 画面のレイアウトの周りに埋め込む操作を行います - ドアが自然なビジュアルのパディングを形成します。

- 読みやすさに注目します。 テキストが読み取り可能であることを確認するのに、フォント サイズや色を慎重に使用します。 自動の動的な型のサポートを受けるには、組み込みのテキストのスタイルを使用します。

![](layout-images/type.png "動的な型のサポートの例")

- タッチのターゲットのサイズに注目します。 テキスト ラベルが付いたボタン/tappable テーブルの行は、画面全体にまたがる必要があります。 Apple では、「配置してはいけない以上 3 つの項目で並列」、およびアイコンとテキスト ラベルではなく使用するかどうかを述べています。

- 使用して、 [ `Menu`コントロール](~/ios/watchos/user-interface/menu.md)明確かつ簡潔なアプリの設計を維持する公開の使用頻度の低い機能にします。


## <a name="implementation"></a>実装

ウォッチ キットには、魅力的の watch アプリのレイアウトを構築するための次のコントロールが含まれています。

### <a name="interface-controller"></a>インターフェイス コント ローラー

`WKInterfaceController`シーンのすべての基本クラスです。

インターフェイス コント ローラーのデザイン サーフェイスに垂直方向のように動作**グループ**: インターフェイス コント ローラーにその他のコントロールをドラッグして、上下に並べて配置を自動的に 1 つになります。

![](layout-images/controller-scene.png "コントロールは、上下に並べて配置を自動的に 1 つです。")

設定することができます、**位置**と**サイズ**の外観を制御するには、各コントロールのプロパティ。

![](layout-images/positionsize-attributes.png "各コントロールの位置とサイズのプロパティを設定します。")

サイズを設定すると**コンテナーからの相対**比例値とオフセットの調整を行うことができます。 このスクリーン ショットは、ウォッチ画面の幅の 80% を使用して設定されているボタンを示しています (**0.8**)。

![](layout-images/button-attributes.png "比例値とオフセットの調整を提供します。")


### <a name="group"></a>グループ化

`WKInterfaceGroup` 垂直方向または水平方向にスタックを構成できるシンプルなレイアウト コンテナーが制御します。 既定では、各コントロール間の間隔が含まれていますでの間隔 (およびくぼみ) を変更することができます、**属性**インスペクター。

![](layout-images/group-attributes.png "Attributes inspector のくぼみと間隔を変更します。")

グループできます自体は、サイズ、周囲のコントロールに対して相対的に配置されているし、複雑なレイアウトを作成するグループを入れ子にすることができます。

![](layout-images/group-scene.png "複雑なレイアウトを作成するグループを入れ子にすることができます。")


### <a name="separator"></a>区切り記号

区切り記号コントロールはレイアウトでは、視覚的なガイダンスを提供するためのものです。 区切り記号 (または背景色またはイメージ)、画面に関連するコンテンツをわかりやすくを使用します。

![](layout-images/separator-scene.png "区切り記号の使用方法の例")

画面の幅全体を使用していない青と緑の区切り記号は、いずれかで構成されているに注意してください。**固定**または**コンテナーからの相対**サイズ。

### <a name="content-controls"></a>コンテンツ コントロール

レイアウトは完璧とはせず、 `Label`、 `Image`、 `Button`、 `Switch`、 `Slider`、 `Map`、および[他のコントロール](~/ios/watchos/user-interface/index.md)します。
これらを使用して、レイアウトに配置できる**グループ**または各コントロールの位置とサイズを設定します。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のレイアウトのリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple の色と文字体裁を参照します。](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
