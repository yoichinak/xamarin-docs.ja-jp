---
title: Xamarin での tvOS 積み上げビューの使用
description: このドキュメントでは、Xamarin でビルドされたアプリで tvOS 積み上げビューを操作する方法について説明します。 ここでは、積み上げ表示の概要について説明します。また、[自動レイアウト]、[積み上げビューの位置とサイズの設定]、[一般的な用途]、[ストーリーボードとの統合] などについても説明します。
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 9f2c8fb235603c5dac37fc0c25be2f070d7df98e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022157"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Xamarin での tvOS 積み上げビューの使用

スタックビューコントロール (`UIStackView`) は、自動レイアウトクラスとサイズクラスの機能を活用して、サブビューのスタックを水平方向または垂直方向に管理します。これにより、Apple TV デバイスのコンテンツの変更と画面のサイズに動的に応答します。

スタックビューにアタッチされているすべてのサブビューのレイアウトは、軸、分布、配置、スペースなどの開発者が定義したプロパティに基づいて、it 部門によって管理されます。

[![](stacked-views-images/stacked01.png "Subview layout diagram")](stacked-views-images/stacked01.png#lightbox)

TvOS アプリで `UIStackView` を使用する場合、開発者は、iOS デザイナーのストーリーボード内でサブビューを定義するか、またはコードでC#サブビューを追加および削除することができます。

## <a name="about-stacked-view-controls"></a>積み上げビューコントロールの概要

`UIStackView` は、非レンダリングコンテナービューとして設計されているため、`UIView`の他のサブクラスと同様にキャンバスに描画されることはありません。 `BackgroundColor` や `DrawRect` のオーバーライドなどのプロパティを設定しても、視覚効果はありません。

スタックビューでサブビューのコレクションをどのように配置するかを制御するプロパティがいくつかあります。

- **Axis** –スタックビューでサブビューを**水平**または**垂直**のどちらに整列させるかを決定します。
- **Alignment** –スタックビュー内でサブビューをどのように配置するかを制御します。
- **Distribution** –スタックビュー内でサブビューのサイズを設定する方法を制御します。
- **スペーシング**–スタックビュー内の各サブビュー間の最小限の空白を制御します。
- **ベースライン相対**– `true`場合、各サブビューの上下の間隔は、そのベースラインから派生します。
- **[相対レイアウト]** -標準レイアウトの余白を基準にサブビューを配置します。

通常は、少数のサブビューを配置するためにスタックビューを使用します。 1つ以上のスタックビューを相互に入れ子にすることで、より複雑なユーザーインターフェイスを作成できます。

サブビューに制約を追加することで、Ui の外観をさらに細かく調整できます (たとえば、高さや幅を制御します)。 ただし、スタックビュー自体によって導入された制約に競合する制約を含めないように注意する必要があります。

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>自動レイアウトとサイズのクラス

サブビューがスタックビューに追加されると、そのレイアウトは、[自動レイアウト] と [サイズ] クラスを使用して、配置されたビューの位置とサイズを示す、そのスタックビューによって完全に制御されます。

スタックビューでは、コレクション内の最初と最後のサブビューが、垂直スタックビューの**上端**と**下端**、および水平スタックビューの**左端**と**右端**に_固定_されます。 [`LayoutMarginsRelativeArrangement`] プロパティを [`true`] に設定すると、ビューでは、そのサブビューがエッジではなく関連する余白にピン留めされます。

スタックビューは、定義された `Axis` (`FillEqually Distribution`を除く) に沿ってサブビューのサイズを計算するときに、サブビューの `IntrinsicContentSize` プロパティを使用します。 `FillEqually Distribution` では、すべてのサブビューのサイズが同じになるようにサイズを変更します。これにより、`Axis`に沿ってスタックビューが塗りつぶされます。

`Fill Alignment`を除き、スタックビューでは、サブビューの `IntrinsicContentSize` プロパティを使用して、指定された `Axis`に対して垂直にビューのサイズを計算します。 `Fill Alignment`では、すべてのサブビューのサイズが変更され、特定の `Axis`に垂直方向にスタックビューが表示されます。

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>スタックビューの配置とサイズ変更

スタックビューには、サブビューのレイアウト全体の制御があります (`Axis` や `Distribution`などのプロパティに基づいています) が、[自動レイアウト] と [サイズ] クラスを使用して、その親ビュー内でスタックビュー (`UIStackView`) を配置する必要があります。

一般に、これは、展開とコントラクトを行うためにスタックビューの少なくとも2つの端を固定して、その位置を定義することを意味します。 追加の制約がなければ、次のように、すべてのサブビューに合わせてスタックビューのサイズが自動的に変更されます。

- `Axis` のサイズは、すべてのサブビューのサイズに加え、各サブビュー間に定義されているすべての領域の合計になります。
- `LayoutMarginsRelativeArrangement` プロパティが `true`の場合、スタックビューのサイズには余白用の領域も含まれます。
- `Axis` に垂直なサイズは、コレクション内の最大のサブビューに設定されます。

また、スタックビューの**高さ**と**幅**に対して制約を指定することもできます。 この場合、`Distribution` と `Alignment` のプロパティによって決定されるように、スタックビューで指定された領域を埋めるようにサブビューがレイアウト (サイズ設定) されます。

`BaselineRelativeArrangement` プロパティが `true`の場合、**上**、**下**、または **中央*- **Y**位置を使用するのではなく、最初または最後のサブビューのベースラインに基づいてサブビューがレイアウトされます。 これらは、次のようにスタックビューのコンテンツで計算されます。

- 垂直スタックビューでは、最初のベースラインの最初のサブビューと最後のサブビューが返されます。 これらのサブビューがそれ自体のスタックビューである場合は、その最初または最後のベースラインが使用されます。
- 水平スタックビューでは、最初と最後のベースラインの両方に対して最も高いサブビューが使用されます。 最も高いビューがスタックビューでもある場合は、ベースラインとして最も基本的なサブビューが使用されます。

> [!IMPORTANT]
> ベースラインの配置は、拡張または圧縮されたサブビューのサイズに対しては機能しません。これは、ベースラインが間違った位置に計算されるためです。 ベースラインの配置では、サブビューの**高さ**が組み込みコンテンツビューの**高さ**と一致していることを確認します。

<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>一般的なスタックビューの使用

スタックビューコントロールでは、いくつかのレイアウトの種類に対応しています。 Apple によると、一般的な使用方法がいくつかあります。

- **軸に沿ってサイズを定義**します。スタックビューの `Axis` と隣接する端のどちらかに沿って両方の端を固定すると、そのサブビューで定義されている領域に合わせて、軸に沿ってスタックビューが拡大されます。
- サブ**ビューの位置を定義**します。これは、親ビューのスタックビューの隣接する端にピン留めすることによって、両方のディメンションで、サブビューが含まれていることに合わせてスタックビューを拡大します。
- スタック**のサイズと位置を定義**する–スタックビューの4つのすべての辺を親ビューに固定することで、スタックビューで定義されている領域に基づいてサブビューが配置されます。
- **軸の垂直方向のサイズを定義**します。これにより、スタックビューの `Axis` に垂直方向にピン留めし、軸に沿って位置を設定することにより、そのサブビューで定義されている空間に合わせて、スタックビューが軸に対して垂直方向に拡大されます。

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>スタックビューとストーリーボード

TvOS アプリでスタックビューを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、`Main.storyboard` ファイルをダブルクリックして開き、編集します。
1. スタックビューに追加する個々の要素のレイアウトをデザインします。

    [![](stacked-views-images/layout01.png "Element layout example")](stacked-views-images/layout01.png#lightbox)
1. 要素に必要な制約を追加して、それらが正しく拡張されるようにします。 要素がスタックビューに追加されると、この手順が重要になります。
1. 必要な数のコピー (この場合は4つ) を作成します。

    [![](stacked-views-images/layout02.png "The required number of copies")](stacked-views-images/layout02.png#lightbox)
1. **[ツールボックス]** から**スタックビュー**をドラッグし、ビューにドロップします。

    [![](stacked-views-images/layout03.png "A Stack View")](stacked-views-images/layout03.png#lightbox)
1. [スタック] ビューを選択し、 **Properties Pad**の [**ウィジェット] タブ**で、 **[配置]** の **[塗りつぶし]** を選択し、 **[分布]** に**均等**に入力して、 **[間隔]** に「`25`」と入力します。

    [![](stacked-views-images/layout04.png "The Widget Tab")](stacked-views-images/layout04.png#lightbox)
1. 必要に応じて、画面上にスタックビューを配置し、制約を追加して必要な場所に保持します。
1. 個々の要素を選択し、スタックビューにドラッグします。

    [![](stacked-views-images/layout05.png "The individual elements in the Stack View")](stacked-views-images/layout05.png#lightbox)
1. レイアウトが調整され、上で設定した属性に基づいて、要素がスタックビューに配置されます。
1. コードC#内で UI コントロールを操作するには、**プロパティエクスプローラー**の [**ウィジェット] タブ**で**名前**を割り当てます。
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、`Main.storyboard` ファイルをダブルクリックして開き、編集します。
1. スタックビューに追加する個々の要素のレイアウトをデザインします。

    [![](stacked-views-images/layout01.png "Example element layout")](stacked-views-images/layout01.png#lightbox)
1. 要素に必要な制約を追加して、それらが正しく拡張されるようにします。 要素がスタックビューに追加されると、この手順が重要になります。
1. 必要な数のコピー (この場合は4つ) を作成します。

    [![](stacked-views-images/layout02.png "The required number of copies")](stacked-views-images/layout02.png#lightbox)
1. **[ツールボックス]** から**スタックビュー**をドラッグし、ビューにドロップします。

    [![](stacked-views-images/layout03-vs.png "A Stack View")](stacked-views-images/layout03-vs.png#lightbox)
1. スタックビューを選択し、**プロパティエクスプローラー**の [**ウィジェット] タブ**で、 **[配置]** の **[塗りつぶし]** を選択し、**配布**に**均等**に入力して、 **[間隔]** に「`25`」と入力します。

    [![](stacked-views-images/layout04-vs.png "The Widget Tab")](stacked-views-images/layout04-vs.png#lightbox)
1. 必要に応じて、画面上にスタックビューを配置し、制約を追加して必要な場所に保持します。
1. 個々の要素を選択し、スタックビューにドラッグします。

    [![](stacked-views-images/layout05-vs.png "The individual elements in the Stack View")](stacked-views-images/layout05-vs.png#lightbox)
1. レイアウトが調整され、上で設定した属性に基づいて、要素がスタックビューに配置されます。
1. コードC#内で UI コントロールを操作するには、**プロパティエクスプローラー**の [**ウィジェット] タブ**で**名前**を割り当てます。
1. 変更内容を保存します。

-----

> [!IMPORTANT]
> イベントハンドラーの作成時に iOS デザイナーの UI 要素 (`UIButton`など) に `TouchUpInside` などのアクションを割り当てることはできますが、Apple TV はタッチスクリーンやサポートタッチイベントを持っていないため、呼び出されません。 TvOS ユーザーインターフェイス要素のアクションを作成する場合は、常に既定の `Action Type` を使用する必要があります。

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

この例では、セグメントコントロールのアウトレットとアクション、および各 "プレーヤーカード" のアウトレットを公開しています。 コードでは、現在のセグメントに基づいてプレーヤーを非表示にして表示します。 (例:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

アプリを実行すると、次の4つの要素がスタックビューに均等に配分されます。

[![](stacked-views-images/layout06.png "When the app is run, the four elements will equally be distributed in our Stack View")](stacked-views-images/layout06.png#lightbox)

プレーヤーの数が減少している場合は、未使用のビューが非表示になり、スタックビューによってレイアウトが調整されます。

[![](stacked-views-images/layout07.png "If the number of players is decreased, the unused views are hidden and the Stack View adjust the layout to fit")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>コードからスタックビューを設定する

IOS デザイナーでスタックビューの内容とレイアウトを完全に定義するだけでなく、コードからC#動的に作成および削除することもできます。

次の例では、スタックビューを使用してレビューの "星" を処理しています (1 ~ 5)。

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

このコードのいくつかの部分を詳しく見てみましょう。 まず、`if` ステートメントを使用して、5つ以上の "星" または0未満であることを確認します。

新しい "star" を追加するには、イメージを読み込み、その**コンテンツモード**を **[縦横合わせる]** に設定します。

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

これにより、"星" アイコンがスタックビューに追加されるときに、そのアイコンがゆがんでしまいます。

次に、新しい "star" アイコンをスタックビューのサブビューのコレクションに追加します。

```csharp
RatingView.AddArrangedSubview(icon);
```

`UIImageView` は、`SubView`ではなく、`UIStackView`の `ArrangedSubviews` プロパティに追加されていることがわかります。 スタックビューでレイアウトを制御するビューは、`ArrangedSubviews` プロパティに追加する必要があります。

スタックビューからサブビューを削除するには、まず、削除するサブビューを取得します。

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

次に、`ArrangedSubviews` コレクションとスーパービューの両方から削除する必要があります。

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

`ArrangedSubviews` コレクションだけからサブビューを削除すると、スタックビューのコントロールから除外されますが、画面からは削除されません。

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>コンテンツの動的な変更

サブビューが追加、削除、または非表示になるたびに、スタックビューによってサブビューのレイアウトが自動的に調整されます。 また、スタックビューの任意のプロパティ (`Axis`など) が調整された場合にも、レイアウトが調整されます。

レイアウトの変更は、アニメーションブロック内に配置することによってアニメーション化できます。次に例を示します。

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

スタックビューのプロパティの多くは、ストーリーボード内のサイズクラスを使用して指定できます。 これらのプロパティは、サイズまたは向きの変化に対する応答として自動的にアニメーション化されます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、tvOS アプリ内の積み上げビューの設計と操作について説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
