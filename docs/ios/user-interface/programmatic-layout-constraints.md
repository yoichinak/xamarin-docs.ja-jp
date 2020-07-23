---
title: Xamarin のプログラムによるレイアウトの制約
description: このガイドでは、ios デザイナーで作成するのではなく、C# コードで iOS の自動レイアウトの制約を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: d8dcf64f9a905557bea69402bcaf7fcfb51fb10b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928576"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Xamarin のプログラムによるレイアウトの制約

_このガイドでは、ios デザイナーで作成するのではなく、C# コードで iOS の自動レイアウトの制約を使用する方法について説明します。_

自動レイアウト ("アダプティブレイアウト" とも呼ばれます) は、応答性の高いデザインアプローチです。 移動レイアウトシステムとは異なり、各要素の位置は画面上のポイントにハードコーディングされています。 "自動レイアウト" は、*リレーションシップ*(デザインサーフェイス上の他の要素に対する要素の相対的な位置) に関するものです。 自動レイアウトの中核となるのは、画面上の他の要素のコンテキストにおける要素または要素のセットの配置を定義する制約または規則の概念です。 要素は画面上の特定の位置に関連付けられていないため、制約は、さまざまな画面サイズとデバイスの向きに適したアダプティブレイアウトを作成するのに役立ちます。

通常、iOS で自動レイアウトを使用する場合は、iOS デザイナーを使用して、UI 項目にレイアウトの制約をグラフィカルに配置します。 ただし、C# コードで制約を作成して適用することが必要になる場合があります。 たとえば、動的に作成された UI 要素をに追加した場合など `UIView` です。

このガイドでは、iOS Designer でグラフィカルに作成するのではなく、C# コードを使用して制約を作成および操作する方法について説明します。

<a name="Creating-Constraints-Programmatically"></a>

## <a name="creating-constraints-programmatically"></a>プログラムによる制約の作成

前述のように、通常は iOS Designer で自動レイアウト制約を使用します。 プログラムを使用して制約を作成する必要がある場合は、次の3つの選択肢があります。

- [レイアウトのアンカー](#Layout-Anchors) -この API は、 `TopAnchor` `BottomAnchor` `HeightAnchor` 制約されている UI 項目のアンカープロパティ (、、など) へのアクセスを提供します。
- [レイアウトの制約](#Layout-Constraints)-クラスを使用して直接制約を作成でき `NSLayoutConstraint` ます。
- [Visual の書式設定言語](#Visual-Format-Language)-制約を定義するためのメソッドとして ASCII を使用します。

次のセクションでは、各オプションについて詳しく説明します。

<a name="Layout-Anchors"></a>

### <a name="layout-anchors"></a>アンカーのレイアウト

クラスを使用 `NSLayoutAnchor` すると、制限されている UI 項目のアンカープロパティに基づいて制約を作成するための fluent インターフェイスを使用できます。 たとえば、ビューコントローラーの上部および下部のレイアウトガイドでは、 `TopAnchor` 、、 `BottomAnchor` およびアンカープロパティが公開されていますが、 `HeightAnchor` ビューではエッジ、中心、サイズ、およびベースラインのプロパティが公開されています。

> [!IMPORTANT]
> アンカープロパティの標準セットに加えて、iOS ビューにはプロパティとプロパティも含まれて `LayoutMarginsGuides` `ReadableContentGuide` います。 これらのプロパティ `UILayoutGuide` は、それぞれ、ビューの余白と読み取り可能なコンテンツガイドを操作するためのオブジェクトを公開します。

レイアウトアンカーには、読みやすい、コンパクトな形式で制約を作成するためのいくつかのメソッドが用意されています。

- **ConstraintEqualTo** - `first attribute = second attribute + [constant]` 必要に応じてオフセット値が指定されているリレーションシップを定義 `constant` します。
- **ConstraintGreaterThanOrEqualTo** - `first attribute >= second attribute + [constant]` 必要に応じてオフセット値が指定されているリレーションシップを定義 `constant` します。
- **ConstraintLessThanOrEqualTo** - `first attribute <= second attribute + [constant]` 必要に応じてオフセット値が指定されているリレーションシップを定義 `constant` します。

次に例を示します。

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

一般的なレイアウトの制約は、単に線形式として表現できます。 次に例を示します。

[![線形式として表現されたレイアウト制約](programmatic-layout-constraints-images/graph01.png)](programmatic-layout-constraints-images/graph01.png#lightbox)

これは、レイアウトアンカーを使用して次の C# コード行に変換されます。

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

ここで、C# コードの部分は、次のように式の指定した部分に対応します。

|下付き|コード|
|---|---|
|アイテム 1|ビューの表示|
|属性1|リードのアンカー|
|リレーションシップ|ConstraintEqualTo|
|×|既定値は1.0 であるため、指定されていません|
|項目 2|OrangeView|
|属性2|TrailingAnchor|
|定数|10.0|

指定されたレイアウト制約式を解決するために必要なパラメーターだけでなく、各レイアウトアンカーメソッドに渡されるパラメーターのタイプセーフも適用されます。 したがって、やなどの水平方向の制約アンカーは、 `LeadingAnchor` `TrailingAnchor` 他の水平アンカーの種類でのみ使用でき、乗数はサイズ制約にのみ提供されます。

<a name="Layout-Constraints"></a>

### <a name="layout-constraints"></a>レイアウトの制約

C# コードでを直接構築することで、自動レイアウト制約を手動で追加でき `NSLayoutConstraint` ます。 レイアウトアンカーを使用する場合とは異なり、定義されている制約に影響がない場合でも、すべてのパラメーターに値を指定する必要があります。 結果として、定型コードがかなり大量に読み込まれることになります。 次に例を示します。

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

`NSLayoutAttribute`列挙体はビューの余白の値を定義し、、、などのプロパティに対応 `LayoutMarginsGuide` `Left` `Right` `Top` `Bottom` し `NSLayoutRelation` ます。列挙型は、指定された属性の間に作成されるリレーションシップを `Equal` 、 `LessThanOrEqual` または `GreaterThanOrEqual` として定義します。

レイアウトアンカー API とは異なり、作成方法では、 `NSLayoutConstraint` 特定の制約の重要な側面は強調表示されず、制約に対して実行されるコンパイル時のチェックはありません。 その結果、実行時に例外をスローする無効な制約を簡単に作成できます。

<a name="Visual-Format-Language"></a>

### <a name="visual-format-language"></a>ビジュアル形式の言語

Visual Format Language を使用すると、作成される制約を視覚的に表現する文字列など、ASCII アートを使用して制約を定義できます。 これには、次のような長所と短所があります。

- Visual フォーマット言語では、有効な制約のみの作成が強制されます。
- 自動レイアウトでは、Visual Format Language を使用して制約がコンソールに出力されるため、デバッグメッセージは、制約の作成に使用されるコードと似ています。
- Visual Format Language を使用すると、非常にコンパクトな式で複数の制約を同時に作成できます。
- Visual Format 言語の文字列はコンパイル側で検証されないため、問題は実行時にのみ検出されます。
- ビジュアルの書式設定言語では、不完全に対する視覚エフェクトが強調されているため、一部の制約の種類を使用して作成することはできません (比率など)。

Visual Format Language を使用して制約を作成する場合は、次の手順を実行します。

1. `NSDictionary`ビューオブジェクトとレイアウトガイド、および形式を定義するときに使用される文字列キーを含むを作成します。
2. 必要に応じて、 `NSDictionary` `NSNumber` 制約の定数値として使用されるキーと値のセット () を定義するを作成します。
3. 書式指定文字列を作成して、1つまたは複数の項目の行をレイアウトします。
4. `FromVisualFormat`クラスのメソッドを呼び出して `NSLayoutConstraint` 、制約を生成します。
5. `ActivateConstraints`クラスのメソッドを呼び出し `NSLayoutConstraint` て、制約をアクティブ化して適用します。

たとえば、Visual フォーマット言語で先頭と末尾の両方の制約を作成するには、次のように指定します。

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

既定の間隔を使用する場合、ビジュアル形式言語では常に親ビューの余白に関連付けられたゼロポイント制約が作成されるため、このコードでは上記の例と同じ結果が生成されます。

複数の子ビューを1行に表示するなど、より複雑な UI デザインの場合、Visual Format Language は水平方向の間隔と垂直方向の配置の両方を指定します。 上の例のように、を指定すると、 `AlignAllTop` `NSLayoutFormatOptions` 行または列のすべてのビューが上部に配置されます。

一般的な制約と Visual 書式文字列の文法を指定する例については、「Apple の[Visual フォーマット言語付録](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)」を参照してください。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

このガイドでは、iOS Designer でグラフィックをグラフィカルに作成するのではなく、自動レイアウトの制約を C# で作成して使用する方法について説明します。 まず、レイアウトアンカー () を使用して自動レイアウトを処理する方法を見てきました `NSLayoutAnchor` 。 次に、レイアウトの制約を使用する方法 () について説明しました `NSLayoutConstraint` 。 最後に、自動レイアウト用のビジュアル形式言語を使用して説明します。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS のデザイン可能コントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Xamarin Designer for iOS を使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple-プログラムによる制約の作成](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple-ビジュアル形式言語付録](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
