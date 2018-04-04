---
title: レイアウトの操作
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 38e26544a907ffcafdec38e3e2758d9bdac7b977
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-layout"></a>レイアウトの操作

Apple Watch のレイアウトを設計[サイズを画面](~/ios/watchos/app-fundamentals/screen-sizes.md)特異な課題を表示します。

## <a name="design-tips"></a>デザインに関するヒント

重要なポイント: 読み取り可能で、ウォッチ式の小さな画面では、大規模な指で、使用可能なユーザー インターフェイスを作成します。 設計のトラップに入らない、 **iOS シミュレーター** (表示される非常に大きな) および**マウス ポインター** (これは小さなタッチのターゲットを持つ動作します)。

- 黒の背景を使用して - ウォッチの黒いベゼルのより大きな画面のイメージを作成します。

- 画面のレイアウトの周囲の余白を埋めるはありません - ベゼルが自然なビジュアル パディングを形成します。

- 読みやすさに注目します。 テキストが読み取り可能なことを確認するのに、フォント サイズ、および色を注意深く使用します。 自動の動的な型のサポートを受けるには、組み込みのテキストのスタイルを使用します。

![](layout-images/type.png "動的な型のサポートの例")

- タッチのターゲット サイズに注目します。 テキスト ラベルが付いたボタン/tappable テーブルの行は、画面全体にまたがる必要があります。 「には配置しないで 3 つの項目サイド バイ サイドよりも多く」、およびアイコンとラベルではテキストを使用するかどうかは、Apple が表示されます。

- 使用して、 [ `Menu`コントロール](~/ios/watchos/user-interface/menu.md)明確かつ簡潔なアプリケーション設計を保持するために使用頻度の低い公開機能にします。


## <a name="implementation"></a>実装

ウォッチ キットには、ウォッチ式の魅力的なアプリのレイアウトを構築するための次のコントロールが含まれています。

### <a name="interface-controller"></a>コント ローラーをインターフェイスします。

`WKInterfaceController`は、シーンのすべての基本クラスです。

インターフェイス コント ローラーのデザイン サーフェイスが垂直方向のように動作**グループ**: インターフェイス コント ローラーに他のコントロールをドラッグして、上下に並べて配置を自動的に 1 つになります。

![](layout-images/controller-scene.png "コントロールは、上下に並べて配置を自動的に 1 つです。")

設定することができます、**位置**と**サイズ**の外観を制御するには、各コントロールのプロパティ。

![](layout-images/positionsize-attributes.png "各コントロールの位置とサイズ プロパティを設定します。")

サイズを設定すると**コンテナーに相対**比例値とオフセットの調整を指定することができます。 このスクリーン ショットは、監視画面の幅の値の 80% を使用して設定されているボタンを示しています (**0.8**)。

![](layout-images/button-attributes.png "比例値とオフセットの調整")


### <a name="group"></a>グループ化

`WKInterfaceGroup` スタックを構成できるシンプルなレイアウト コンテナー上下左右の方向を制御します。 既定では、各コントロール間のスペースが含まれていますで、間隔 (およびインセット) を変更することができます、**属性**インスペクターです。

![](layout-images/group-attributes.png "属性インスペクターでインセットと間隔を変更します。")

グループ自体サイズ設定して、その周囲のコントロールに対して相対的に配置および複雑なレイアウトを作成するグループを入れ子にすることができます。

![](layout-images/group-scene.png "複雑なレイアウトを作成するグループを入れ子にすることができます。")


### <a name="separator"></a>区切り記号

区切り記号コントロールは、レイアウトで視覚的なガイダンスを提供するのに役立つためのものです。 区切り記号 (または背景色またはイメージ)、画面に関連するコンテンツを理解するユーザーを支援するを使用します。

![](layout-images/separator-scene.png "区切り記号の使用方法の例")

画面の幅全体を使用していない青と緑の区切り記号は、いずれかで構成されているに注意してください**固定**または**コンテナーに相対**サイズ。

### <a name="content-controls"></a>コンテンツ コントロール

レイアウトすることが完成せず、 `Label`、 `Image`、 `Button`、 `Switch`、 `Slider`、 `Map`、および[他のコントロール](~/ios/watchos/user-interface/index.md)です。
これらを使用して、レイアウトに配置できます**グループ**または各コントロールの位置とサイズの設定。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple のレイアウトの参照](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple の色 (&) 文字体裁を参照します。](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
