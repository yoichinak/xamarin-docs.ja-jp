---
title: "プログラムによるレイアウト制約"
description: "このガイドは、iOS デザイナー内でそれらを作成する代わりに c# コードでの自動レイアウトの制約を iOS での作業を表示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 774d6e6ecdb081650c6f008b1ac83c397f788d5b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="programmatic-layout-constraints"></a>プログラムによるレイアウト制約

_このガイドは、iOS デザイナー内でそれらを作成する代わりに c# コードでの自動レイアウトの制約を iOS での作業を表示します。_

自動レイアウト (「アダプティブ レイアウト」とも呼ばれます) は、レスポンシブ デザイン方法です。 各要素の場所は、画面上の点にハードコード、過渡期のレイアウト システムとは異なり、自動レイアウトがに関するは*リレーションシップ*-デザイン サーフェイス上の他の要素に対して要素の位置。 自動レイアウトの中核には、制約または画面の他の要素のコンテキストで要素の配置または要素のセットを定義するルールの設定を勧めします。 要素が画面上の特定位置に関連付けられていないために、制約はさまざまな画面サイズと向きがデバイスには良く見えるアダプティブ レイアウトの作成に役立ちます。

通常、iOS では、自動レイアウトを使用する場合に使用する iOS デザイナー UI の項目のレイアウトの制約をグラフィカルに配置します。 ただし、作成し、c# コード内の制約を適用する必要がある場合もあります。 たとえば、作成時に動的に使用する UI 要素に追加、`UIView`です。

このガイドでは、c# コードを使用して iOS デザイナーでグラフィカルに作成する代わりに、制約を作成および操作する方法を示します。

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>制約をプログラムで作成します。

前述のように、通常を操作する自動レイアウトの制約を含む iOS デザイナーでします。 プログラムで、制約を作成する必要も、選択できる 3 つのオプションがあります。

* [レイアウトのアンカー](#Layout-Anchors) -この API は、アンカー プロパティへのアクセスを提供 (など`TopAnchor`、`BottomAnchor`または`HeightAnchor`) 制約されている UI 項目の。
* [レイアウト制約](#Layout-Constraints)の制約を使用して直接作成したり、`NSLayoutConstraint`クラスです。
* [ビジュアルの書式設定言語](#Visual-Format-Language)-、制約を定義するメソッドと同様に、ASCII アートを提供します。

次のセクションでは、各オプションの詳細に見ていきます。

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>レイアウトのアンカー

使用して、`NSLayoutAnchor`クラス制約されている UI 項目のアンカー プロパティに基づく制約を作成するための fluent インターフェイスがあります。 ビュー コント ローラーの上部と下部のレイアウトが公開をガイドするなど、 `TopAnchor`、`BottomAnchor`と`HeightAnchor`ビュー端、中心、サイズおよび基準のプロパティを公開するときに、プロパティを固定します。

> [!IMPORTANT]
> IOS のビューにもは、標準のアンカー プロパティ セットに加えて、`LayoutMarginsGuides`と`ReadableContentGuide`プロパティです。 これらのプロパティを公開`UILayoutGuide`コンテンツ ガイドのそれぞれのビューの余白と読み取り可能な作業するときのオブジェクト。

レイアウトのアンカーは、読みやすい、コンパクトな形式で制約を作成するためのいくつかのメソッドを提供します。

- **ConstraintEqualTo** -のリレーションシップを定義、`first attribute = second attribute + [constant]`オプションで指定されたと`constant`オフセット値。
- **ConstraintGreaterThanOrEqualTo** -のリレーションシップを定義、`first attribute >= second attribute + [constant]`オプションで指定されたと`constant`オフセット値。
- **ConstraintLessThanOrEqualTo** -のリレーションシップを定義、`first attribute <= second attribute + [constant]`オプションで指定されたと`constant`オフセット値。

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

一般的なレイアウト制約は、単に線形の式として表現できます。 次の例を参照してください。

[![](programmatic-layout-constraints-images/graph01.png "直線の式で表されるレイアウト制約")](programmatic-layout-constraints-images/graph01.png#lightbox)

レイアウトのアンカーを使用して c# コードの次の行に変換されます。

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

ここで c# コードの部分は式の指定された構成要素に次のように対応します。

|数式|コード|
|---|---|
|アイテム 1|PurpleView|
|属性 1|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|乗数|既定値は 1.0 ように指定されていません|
|項目 2|OrangeView|
|属性 2|TrailingAnchor|
|定数|10.0|

レイアウトの制約式を解決するために必要なパラメーターのみを提供するだけでなくのレイアウトのアンカー メソッドに渡されるパラメーターの型の安全性を適用します。 など、水平方向の制約を固定`LeadingAnchor`または`TrailingAnchor`のみ使用できます他の水平方向のアンカーを持つ型と乗数だけに提供されたサイズの制限。

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>レイアウトの制約

自動レイアウトの制約を手動で追加するには直接構築することによって、 `NSLayoutConstraint` c# コードです。 レイアウトのアンカーを使用するとは異なり、定義されている制約には影響はない場合でも、すべてのパラメーターの値を指定する必要があります。 その結果にかなりの読み取り、定型的なコードを生成するは終了します。 例えば:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

場所、`NSLayoutAttribute`列挙型は、ビューの余白の値を定義しに対応している、`LayoutMarginsGuide`などのプロパティ`Left`、 `Right`、`Top`と`Bottom`と`NSLayoutRelation`列挙型の関係を定義します。として指定された属性の間で作成されます`Equal`、`LessThanOrEqual`または`GreaterThanOrEqual`です。

異なり、レイアウト アンカー API を使用して、`NSLayoutConstraint`作成方法を選択しないで、特定の制約の重要な側面とコンパイルがない制約に対して実行されるチェックの時刻します。 その結果、実行時に例外をスローする無効な制約を作成しやすいです。

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>ビジュアルな形式の言語

ビジュアルの言語の形式では、作成される制約を示すビジュアル表現を提供する文字列のような ASCII アートを使用する制約を定義することができます。 これは、次の長所と短所があります。

- ビジュアルの言語の形式は、有効な制約のみの作成を強制します。
 - 自動レイアウトでは、デバッグ メッセージの制約を作成するために使用するコードのようになりますので、ビジュアルの言語形式を使用してコンソールに制約を出力します。
 - ビジュアルの言語の形式では、非常にコンパクト式で同時に複数の制約を作成することができます。
 - ビジュアルの言語の形式の文字列のコンパイル側の検証しないため、問題は実行時にのみ検出できます。
 - Visual の言語の形式は、完全を期す経由での視覚エフェクトを強調するため、(比) など、いくつかの制約の種類を関連付け作成できません。

Visual の言語の形式を使用して制約を作成する場合は、以下の手順を実行します。

1. 作成、`NSDictionary`オブジェクトの表示およびレイアウト ガイドと形式を定義するときに使用される文字列のキーを格納しています。
2. 必要に応じて、作成、`NSDictionary`キーと値のセットを定義する (`NSNumber`) 制約の定数値として使用します。
3. 1 つの列またはアイテムの 1 行のレイアウトを書式指定文字列を作成します。
4. 呼び出す、`FromVisualFormat`のメソッド、`NSLayoutConstraint`制約を生成するクラス。
5. 呼び出す、`ActivateConstraints`のメソッド、`NSLayoutConstraint`クラスをアクティブ化および制約を適用します。

たとえば、Visual の言語の形式で、先頭と末尾の制約の両方を作成するにすることが、次のように使用します。

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

作成するため、ビジュアルな形式言語常にゼロ ポイント制約の既定の間隔を使用する場合は、親ビューの余白にアタッチされている、このコードは、上記の例と同じ結果を生成します。

1 つの行に複数の子ビューなどのより複雑な UI のデザインは、Visual の言語の形式は、左右の間隔と縦方向の配置の両方を指定します。 これが指定されている上記の例のように、 `AlignAllTop` `NSLayoutFormatOptions`のすべての行または列の上部でビューを揃えます。

Apple を参照してください[Visual 形式言語付録](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)な例については、一般的な制約とビジュアルの書式設定文字列文法を指定します。

<a name="Summary" />

## <a name="summary"></a>まとめ

このガイドでは、iOS デザイナーでグラフィカルに作成するのではなく、c# での自動レイアウト制約の使用の作成と表示されます。 最初に、レイアウトのアンカーを使用して検索する (`NSLayoutAnchor`) を自動レイアウトを処理します。 次に、レイアウトの制約を使用する方法を示した (`NSLayoutConstraint`)。 最後に、自動レイアウトのビジュアルの言語形式を使用して表示されます。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS 設計可能なコントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [IOS 用の Xamarin デザイナーに自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple の制約をプログラムで作成します。](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple のビジュアルな形式の言語の付録](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
