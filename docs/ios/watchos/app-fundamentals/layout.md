---
title: Xamarin での watchOS レイアウトの使用
description: このドキュメントでは、Xamarin を使用して watchOS レイアウトを作成する方法について説明します。 インターフェイスコントローラー、グループ、区切り記号、およびコンテンツコントロールについて説明します。
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 1fdb7a10bd767085ba8758fa2e026cc36c93639a
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436918"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Xamarin での watchOS レイアウトの使用

Apple Watch [画面サイズ](~/ios/watchos/app-fundamentals/screen-sizes.md) のレイアウトを設計すると、独自の課題が生じます。

## <a name="design-tips"></a>デザインのヒント

重要な点は、ユーザーインターフェイスを読み取り可能にして、大きな指で小さな watch 画面で使用できるようにすることです。 **IOS シミュレーター** (非常に大きい) と**マウスポインター** (小さなタッチターゲットで動作) の設計のトラップには含まれません。

- 黒の背景を使用します。これは、ウォッチの黒いベゼルを使用して、大きな画面の錯覚を作成します。

- 画面レイアウトの周囲に埋め込まないでください。ベゼルは、自然な視覚的余白を形成します。

- 読みやすさに重点を置いてください。 フォントサイズと色を慎重に使用して、テキストを判読できるようにします。 組み込みのテキストスタイルを使用して、動的な型の自動サポートを実現します。

![動的な型のサポートの例](layout-images/type.png)

- タッチターゲットのサイズに注目します。 ボタン/tappable テーブルの行をテキストラベルで表示するには、画面全体にまたがる必要があります。 Apple では、"2 つ以上の項目を並べて表示しない" と表示され、テキストラベルではなくアイコンを使用していることが示されています。

- [ `Menu` コントロール](~/ios/watchos/user-interface/menu.md)を使用して、アプリの設計を明確かつ簡潔に保つために使用頻度の低い機能を公開します。

## <a name="implementation"></a>実装

Watch Kit には、魅力的なウォッチアプリのレイアウトを作成するための次のコントロールが含まれています。

### <a name="interface-controller"></a>インターフェイスコントローラー

は、 `WKInterfaceController` すべてのシーンの基本クラスです。

インターフェイスコントローラーのデザインサーフェイスは、垂直方向の **グループ**のように動作します。他のコントロールをインターフェイスコントローラーにドラッグすると、他のコントロールの上に自動的に配置されます。

![コントロールは、他のコントロールの上に自動的に配置されます。](layout-images/controller-scene.png)

各コントロールの **Position** プロパティと **Size** プロパティを設定して、外観を制御できます。

![各コントロールの Position プロパティと Size プロパティの設定](layout-images/positionsize-attributes.png)

サイズが **コンテナーに対して相対的** に設定されている場合は、比例値とオフセット調整を指定できます。 このスクリーンショットは、ウォッチ画面の幅 (**0.8**) の80% を使用するように設定されたボタンを示しています。

![比例値とオフセット調整を指定する](layout-images/button-attributes.png)

### <a name="group"></a>Group

`WKInterfaceGroup` は、コントロールを垂直方向または水平方向に積み重ねるように構成できる単純なレイアウトコンテナーです。 既定では、各コントロールの間隔が含まれますが、[ **属性** ] インスペクターで間隔 (およびインセット) を変更できます。

![属性インスペクターの間隔とインセットを変更する](layout-images/group-attributes.png)

グループは、その周囲のコントロールを基準にしてサイズを指定し、配置することができます。また、グループを入れ子にして、複雑なレイアウトを作成することもできます。

![グループを入れ子にして複雑なレイアウトを作成することができます](layout-images/group-scene.png)

### <a name="separator"></a>区切り記号

区切り記号コントロールは、レイアウトの視覚的なガイダンスを提供するためのものです。 区切り記号 (または背景色や画像) を使用して、画面上でどのコンテンツが関連しているかをユーザーが理解できるようにします。

![区切り記号の使用例](layout-images/separator-scene.png)

注: 画面の全幅を使用しない青と緑の区切り記号は、 **固定** または **コンテナーサイズからの相対** 値で構成されています。

### <a name="content-controls"></a>コンテンツ コントロール

、、、、、 `Label` `Image` `Button` `Switch` `Slider` `Map` 、および [その他のコントロール](~/ios/watchos/user-interface/index.md)を使用せずにレイアウトを完了することはできません。
これらは、 **グループ** 、または各コントロールの位置とサイズの設定を使用して、レイアウトに配置できます。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のレイアウトリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple の色 & タイポグラフィリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)