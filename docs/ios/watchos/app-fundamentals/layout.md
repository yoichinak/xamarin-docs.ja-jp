---
title: Xamarin での watchOS レイアウトの使用
description: このドキュメントでは、Xamarin を使用して watchOS レイアウトを作成する方法について説明します。 インターフェイスコントローラー、グループ、区切り記号、およびコンテンツコントロールについて説明します。
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 568d1e354d0ee840aeed980d6e8cc6b83068a1c8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001539"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Xamarin での watchOS レイアウトの使用

Apple Watch[画面サイズ](~/ios/watchos/app-fundamentals/screen-sizes.md)のレイアウトを設計すると、独自の課題が生じます。

## <a name="design-tips"></a>デザインのヒント

重要な点は、ユーザーインターフェイスを読み取り可能にして、大きな指で小さな watch 画面で使用できるようにすることです。 **IOS シミュレーター** (非常に大きい) と**マウスポインター** (小さなタッチターゲットで動作) の設計のトラップには含まれません。

- 黒の背景を使用します。これは、ウォッチの黒いベゼルを使用して、大きな画面の錯覚を作成します。

- 画面レイアウトの周囲に埋め込まないでください。ベゼルは、自然な視覚的余白を形成します。

- 読みやすさに重点を置いてください。 フォントサイズと色を慎重に使用して、テキストを判読できるようにします。 組み込みのテキストスタイルを使用して、動的な型の自動サポートを実現します。

![](layout-images/type.png "Example of Dynamic Type support")

- タッチターゲットのサイズに注目します。 ボタン/tappable テーブルの行をテキストラベルで表示するには、画面全体にまたがる必要があります。 Apple では、"2 つ以上の項目を並べて表示しない" と表示され、テキストラベルではなくアイコンを使用していることが示されています。

- [`Menu` コントロール](~/ios/watchos/user-interface/menu.md)を使用して、アプリの設計を明確かつ簡潔に保つために使用頻度の低い機能を公開します。

## <a name="implementation"></a>実装

Watch Kit には、魅力的なウォッチアプリのレイアウトを作成するための次のコントロールが含まれています。

### <a name="interface-controller"></a>インターフェイスコントローラー

`WKInterfaceController` は、すべてのシーンの基本クラスです。

インターフェイスコントローラーのデザインサーフェイスは、垂直方向の**グループ**のように動作します。他のコントロールをインターフェイスコントローラーにドラッグすると、他のコントロールの上に自動的に配置されます。

![](layout-images/controller-scene.png "Controls are automatically laid-out one above the other")

各コントロールの**Position**プロパティと**Size**プロパティを設定して、外観を制御できます。

![](layout-images/positionsize-attributes.png "Set the Position and Size properties on each control")

サイズが**コンテナーに対して相対的**に設定されている場合は、比例値とオフセット調整を指定できます。 このスクリーンショットは、ウォッチ画面の幅 (**0.8**) の80% を使用するように設定されたボタンを示しています。

![](layout-images/button-attributes.png "Provide a proportional value and an offset adjustment")

### <a name="group"></a>グループ化

`WKInterfaceGroup` は、垂直方向または水平方向にコントロールを積み重ねるように構成できる単純なレイアウトコンテナーです。 既定では、各コントロールの間隔が含まれますが、 **[属性]** インスペクターで間隔 (およびインセット) を変更できます。

![](layout-images/group-attributes.png "Modify the spacing and insets in the Attributes inspector")

グループは、その周囲のコントロールを基準にしてサイズを指定し、配置することができます。また、グループを入れ子にして、複雑なレイアウトを作成することもできます。

![](layout-images/group-scene.png "Groups can be nested to create complex layouts")

### <a name="separator"></a>区切り記号

区切り記号コントロールは、レイアウトの視覚的なガイダンスを提供するためのものです。 区切り記号 (または背景色や画像) を使用して、画面上でどのコンテンツが関連しているかをユーザーが理解できるようにします。

![](layout-images/separator-scene.png "Example of Separator usage")

注: 画面の全幅を使用しない青と緑の区切り記号は、**固定**または**コンテナーサイズからの相対**値で構成されています。

### <a name="content-controls"></a>コンテンツ コントロール

`Label`、`Image`、`Button`、`Switch`、`Slider`、`Map`、および[その他のコントロール](~/ios/watchos/user-interface/index.md)を使用せずにレイアウトを完了することはできません。
これらは、**グループ**、または各コントロールの位置とサイズの設定を使用して、レイアウトに配置できます。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のレイアウトリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple の色 & タイポグラフィリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
