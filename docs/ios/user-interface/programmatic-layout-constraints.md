---
title: Xamarin.iOS でプログラムによるレイアウトの制約
description: このガイドは iOS の操作での自動レイアウトの制約C#iOS Designer で作成するのではなくコード。
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 3d8e69af7f790415343abf464ea2bb22e879e025
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106962"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Xamarin.iOS でプログラムによるレイアウトの制約

_このガイドは iOS の操作での自動レイアウトの制約C#iOS Designer で作成するのではなくコード。_

自動レイアウトの (「アダプティブ レイアウト」とも呼ばれます) は、レスポンシブ デザイン手法です。 各要素の場所は、画面上のポイントにハード コードされた、過渡期のレイアウト システムとは異なり、自動レイアウトがの詳細については*リレーションシップ*-デザイン サーフェイス上の他の要素を基準として要素の位置。 自動レイアウトの中核が制約または画面上の他の要素のコンテキストで要素の配置または要素のセットを定義するルールの考え方です。 要素が画面上の特定位置に関連付けられていないため、制約はさまざまな画面サイズと向きがデバイス上で優れた外観アダプティブのレイアウトの作成に役立ちます。

通常は iOS に自動レイアウトを使用する場合は、グラフィカル UI 項目のレイアウトの制約を配置する iOS Designer を使用します。 ただし、する可能性がありますを作成し、内の制約を適用する必要がある生じるC#コード。 たとえばを動的に使用して作成されたときに追加の UI 要素を`UIView`します。

このガイドを使用して制約を作成および操作する方法を示しますC#iOS Designer でグラフィカルに作成するのではなくコード。

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>制約をプログラムで作成します。

前述のように、通常しますで操作することで自動レイアウトの制約 iOS Designer。 制約をプログラムで作成するには時間の選択できる 3 つのオプションがあります。

* [レイアウトのアンカー](#Layout-Anchors) -この API は、アンカー プロパティへのアクセスを提供します (など`TopAnchor`、`BottomAnchor`または`HeightAnchor`) の制約されている UI 項目。
* [レイアウトの制約](#Layout-Constraints)-直接を使用して制約を作成することができます、`NSLayoutConstraint`クラス。
* [ビジュアルの書式設定言語](#Visual-Format-Language)の制約を定義するメソッドと同様に、ASCII アートを提供します。

次のセクションでは、各オプションの詳細に見ていきます。

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>レイアウトのアンカー

使用して、`NSLayoutAnchor`クラス制約されている UI 項目のアンカー プロパティに基づく制約を作成するための fluent インターフェイスがあります。 ビュー コント ローラーの上部と下部のレイアウトの公開ガイドなど、 `TopAnchor`、`BottomAnchor`と`HeightAnchor`ビューは、edge、center、サイズ、および基準のプロパティを公開します。 中にプロパティを固定します。

> [!IMPORTANT]
> IOS ビューだけでなくアンカー プロパティの標準セットを含めることも、`LayoutMarginsGuides`と`ReadableContentGuide`プロパティ。 これらのプロパティを公開`UILayoutGuide`ビューの余白と読み取り可能な操作のオブジェクトをそれぞれガイド コンテンツします。

レイアウトのアンカーは、わかりやすく、コンパクトな形式で制約を作成するためのいくつかのメソッドを提供します。

- **ConstraintEqualTo** -のリレーションシップを定義、`first attribute = second attribute + [constant]`をオプションで指定された`constant`オフセット値。
- **ConstraintGreaterThanOrEqualTo** -のリレーションシップを定義、`first attribute >= second attribute + [constant]`をオプションで指定された`constant`オフセット値。
- **ConstraintLessThanOrEqualTo** -のリレーションシップを定義、`first attribute <= second attribute + [constant]`をオプションで指定された`constant`オフセット値。

例えば:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

一般的なレイアウトの制約は、単純に直線の式として表現できます。 次の例を参照してください。

[![](programmatic-layout-constraints-images/graph01.png "直線の式として表されるレイアウト制約")](programmatic-layout-constraints-images/graph01.png#lightbox)

これが、次の行に変換されるC#レイアウトのアンカーを使用するコードします。

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

場所の部分、C#コードは次のように、式の指定された構成要素に対応します。

|式|コード|
|---|---|
|項目 1|PurpleView|
|属性 1|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|乗数|既定値は 1.0 のように指定されていません|
|項目 2|OrangeView|
|属性 2|TrailingAnchor|
|定数|10.0|

特定のレイアウトの制約式を解決するために必要なパラメーターのみを提供するだけでなくの各レイアウト アンカー メソッドに渡されるパラメーターのタイプ セーフを適用します。 制約が、水平アンカーなど`LeadingAnchor`または`TrailingAnchor`のみ使用できますの他の水平アンカーを持つ型と乗数のみに提供されるサイズの制約。

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>レイアウトの制約

自動レイアウトの制約を手動で追加するには直接作成することにより、`NSLayoutConstraint`でC#コード。 レイアウトのアンカーを使用するとは異なり、定義されている制約に影響はない場合でも、すべてのパラメーターの値を指定する必要があります。 その結果、非常に長い時間、読み取り、定型コードを生成することになります。 例えば:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

場所、`NSLayoutAttribute`列挙型は、ビューの余白の値を定義およびに対応しています、`LayoutMarginsGuide`などのプロパティ`Left`、 `Right`、`Top`と`Bottom`と`NSLayoutRelation`列挙型の関係を定義します。として指定された属性の間で成される`Equal`、`LessThanOrEqual`または`GreaterThanOrEqual`します。

異なりレイアウト アンカー API を使用して、`NSLayoutConstraint`作成方法には、特定の制約の重要な側面が強調表示されないと、コンパイルがない時間の制約に対して実行されるチェックします。 結果として、実行時に例外をスローする無効な制約を作成しやすいです。

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>ビジュアルな形式の言語

Visual 言語の形式を使用すると、作成される制約の視覚的表現を提供する文字列のような ASCII アートを使用して制約を定義できます。 これは、次の長所と短所があります。

- Visual 言語の形式は、有効な制約のみの作成を強制します。
 - 自動レイアウトでは、デバッグ メッセージの制約を作成するために使用するコードのようになりますので、ビジュアルの書式の言語を使用してコンソールに制約を出力します。
 - Visual 言語の形式を使用すると、非常にコンパクトな表現で同時に複数の制約を作成できます。
 - Visual 言語の書式設定文字列のコンパイル側の検証がないため、問題は実行時にのみ検出できます。
 - Visual 言語の形式は、完全を期すために視覚化を強調ために、比率) などをいくつかの制約の種類を作成することはできません。

Visual 言語の形式を使用して制約を作成するときに、次の手順を実行します。

1. 作成、`NSDictionary`オブジェクトの表示およびレイアウト ガイドと形式を定義するときに使用される文字列のキーを格納しています。
2. 必要に応じて作成、`NSDictionary`キーと値のセットを定義する (`NSNumber`) 制約の定数の値として使用します。
3. 1 つの列または行の項目をレイアウトする書式指定文字列を作成します。
4. 呼び出す、`FromVisualFormat`のメソッド、`NSLayoutConstraint`制約を生成するクラス。
5. 呼び出す、`ActivateConstraints`のメソッド、`NSLayoutConstraint`クラスをアクティブ化および制約を適用します。

たとえば、ビジュアルの書式の言語で、先頭と末尾の制約の両方を作成するにする可能性があります、次のように使用します。

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Visual 言語の形式には、常に 0 個の既定の間隔を使用する場合は、親ビューの余白にアタッチされているポイントの制約が作成されます、ために、このコードには、上記の例を同一の結果が生成されます。

1 つの行に複数の子ビューなどのより複雑な UI デザインは、Visual 言語の形式は、左右の間隔と垂直方向の配置の両方を指定します。 指定する上記の例のように、 `AlignAllTop` `NSLayoutFormatOptions`のすべての行または列の上部でビューを揃えます。

Apple を参照してください。 [Visual 形式の言語の付録](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)な例については、一般的な制約とビジュアルの書式設定文字列文法を指定します。

<a name="Summary" />

## <a name="summary"></a>まとめ

このガイドでの自動レイアウトの制約の作成と操作の表示C#iOS Designer でグラフィカルに作成するとは対照的です。 レイアウトのアンカーを使用する調べると、最初に、(`NSLayoutAnchor`) 自動レイアウトを処理します。 次に、レイアウトの制約を使用する方法を示しました (`NSLayoutConstraint`)。 最後に、自動レイアウトのビジュアルの書式の言語を使用して表示されます。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS デザイン可能なコントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [IOS 用の Xamarin のデザイナーを使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple の制約をプログラムで作成します。](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple のビジュアルな形式の言語の付録](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
