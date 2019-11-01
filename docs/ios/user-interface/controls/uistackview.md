---
title: Xamarin. iOS のスタックビュー
description: この記事では、Xamarin. iOS アプリで新しい UIStackView コントロールを使用して、水平方向または垂直方向に配置されたスタックでサブビューのセットを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b4a8507d4d1497964f6b60307622ca3e1dc4cd90
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021802"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin. iOS のスタックビュー

_この記事では、Xamarin. iOS アプリで新しい UIStackView コントロールを使用して、水平方向または垂直方向に配置されたスタックでサブビューのセットを管理する方法について説明します。_

> [!IMPORTANT]
> StackView は iOS Designer でサポートされていますが、安定チャネルを使用するとユーザビリティのバグが発生する可能性があることに注意してください。 ベータチャネルまたはアルファチャネルを切り替えると、この問題が軽減されます。 このチュートリアルでは、必要な修正が安定したチャネルに実装されるまで、Xcode を使用してこのチュートリアルを提示することを決定しました。

スタックビューコントロール (`UIStackView`) は、自動レイアウトクラスとサイズクラスの機能を活用して、iOS デバイスの向きと画面サイズに動的に応答するサブビューのスタックを、水平方向または垂直方向に管理します。

スタックビューにアタッチされているすべてのサブビューのレイアウトは、軸、分布、配置、スペースなどの開発者が定義したプロパティに基づいて、it 部門によって管理されます。

[![](uistackview-images/stacked01.png "Stack View layout diagram")](uistackview-images/stacked01.png#lightbox)

Xamarin iOS アプリで `UIStackView` を使用する場合、開発者は iOS デザイナーのストーリーボード内でサブビューを定義するか、またはコードでC#サブビューを追加および削除することができます。

このドキュメントは、2つの部分で構成されています。最初のスタックビューを実装するのに役立つクイックスタートと、そのしくみについての技術的な詳細について説明します。

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView ビデオ**

## <a name="uistackview-quickstart"></a>UIStackView クイックスタート

`UIStackView` コントロールの簡単な概要として、ユーザーが 1 ~ 5 の評価を入力できる単純なインターフェイスを作成します。 ここでは、2つのスタックビューを使用します。1つは、デバイスの画面にインターフェイスを垂直方向に配置し、もう1つは画面全体で1-5 の評価アイコンを上下に配置します。

### <a name="define-the-ui"></a>UI を定義する

新しい Xamarin. iOS プロジェクトを開始し、Xcode の Interface Builder の**メインのストーリーボード**ファイルを編集します。 まず、**ビューコントローラー**に1つの**垂直方向のスタックビュー**をドラッグします。

[![](uistackview-images/quick01.png "Drag a single Vertical Stack View on the View Controller")](uistackview-images/quick01.png#lightbox)

**属性インスペクター**で、次のオプションを設定します。

[![](uistackview-images/quick02.png "Set the Stack View options")](uistackview-images/quick02.png#lightbox)

この場合、

- **Axis** –スタックビューでサブビューを**水平**または**垂直**のどちらに整列させるかを決定します。
- **Alignment** –スタックビュー内でサブビューをどのように配置するかを制御します。
- **Distribution** –スタックビュー内でサブビューのサイズを設定する方法を制御します。
- **スペーシング**–スタックビュー内の各サブビュー間の最小限の空白を制御します。
- **[ベースラインの相対**] –このチェックボックスをオンにすると、各サブビューの上下の間隔がそのベースラインから派生します。
- **[相対レイアウト]** -標準レイアウトの余白を基準にサブビューを配置します。

スタックビューを使用する場合、**配置**は、サブビューの**X**および**Y**位置、および**高さ**と**幅**とし**て考える**ことができます。

> [!IMPORTANT]
> `UIStackView` は、非レンダリングコンテナービューとして設計されているため、`UIView`の他のサブクラスと同様にキャンバスに描画されることはありません。 したがって、`BackgroundColor` や `DrawRect` のオーバーライドなどのプロパティを設定しても視覚効果はありません。

次のように、ラベル、ImageView、2つのボタン、水平スタックビューを追加して、アプリのインターフェイスのレイアウトを続けます。

[![](uistackview-images/quick03.png "Laying out the Stack View UI")](uistackview-images/quick03.png#lightbox)

次のオプションを使用して、水平方向のスタックビューを構成します。

[![](uistackview-images/quick04.png "Configure the Horizontal Stack View options")](uistackview-images/quick04.png#lightbox)

横方向のスタックビューに追加したときに、評価の各 "ポイント" を表すアイコンを拡大する必要がないので、**配置**を [**中央**揃え] に設定し、**分布**を**均等に塗りつぶす**ように設定しました。

最後に、次の**アウトレット**と**アクション**を接続します。

[![](uistackview-images/quick05.png "The Stack View Outlets and Actions")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>コードから UIStackView にデータを設定する

Visual Studio for Mac に戻り、 **ViewController.cs**ファイルを編集して、次のコードを追加します。

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

### <a name="testing-the-ui"></a>UI のテスト

必要なすべての UI 要素とコードを配置して、インターフェイスを実行およびテストできるようになりました。 UI が表示されると、垂直スタックビュー内のすべての要素が、上から下に均等に配置されます。

ユーザーが [評価の**拡大**] ボタンをタップすると、別の "星" が画面に追加されます (最大5個まで)。

[![](uistackview-images/intro01.png "The sample app run")](uistackview-images/intro01.png#lightbox)

"星" は、水平スタックビューで自動的に中央揃えになり、均等に配分されます。 ユーザーが [評価を**下げる**] ボタンをタップすると、"star" が削除されます (いずれかが残っている場合)。

## <a name="stack-view-details"></a>スタックビューの詳細

`UIStackView` コントロールの概要と動作方法についての概要を説明したので、その機能と詳細について詳しく見ていきましょう。

### <a name="auto-layout-and-size-classes"></a>自動レイアウトとサイズのクラス

前に説明したように、ビューがスタックビューに追加されると、[自動レイアウト] と [サイズ] クラスを使用して、配置されたビューの位置とサイズを示す、そのスタックビューによってレイアウトが全体的に制御されます。

スタックビューでは、コレクション内の最初と最後のサブビューが、垂直スタックビューの**上端**と**下端**、および水平スタックビューの**左端**と**右端**に_固定_されます。 [`LayoutMarginsRelativeArrangement`] プロパティを [`true`] に設定すると、ビューでは、そのサブビューがエッジではなく関連する余白にピン留めされます。

スタックビューは、定義された `Axis` (`FillEqually Distribution`を除く) に沿ってサブビューのサイズを計算するときに、サブビューの `IntrinsicContentSize` プロパティを使用します。 `FillEqually Distribution` では、すべてのサブビューのサイズが同じになるようにサイズを変更します。これにより、`Axis`に沿ってスタックビューが塗りつぶされます。

`Fill Alignment`を除き、スタックビューでは、サブビューの `IntrinsicContentSize` プロパティを使用して、指定された `Axis`に対して垂直にビューのサイズを計算します。 `Fill Alignment`では、すべてのサブビューのサイズが変更され、特定の `Axis`に垂直方向にスタックビューが表示されます。

### <a name="positioning-and-sizing-the-stack-view"></a>スタックビューの配置とサイズ変更

スタックビューには、サブビューのレイアウト全体の制御があります (`Axis` や `Distribution`などのプロパティに基づいています) が、[自動レイアウト] と [サイズ] クラスを使用して、その親ビュー内でスタックビュー (`UIStackView`) を配置する必要があります。

一般に、これは、展開とコントラクトを行うためにスタックビューの少なくとも2つの端を固定して、その位置を定義することを意味します。 追加の制約がなければ、次のように、すべてのサブビューに合わせてスタックビューのサイズが自動的に変更されます。

- `Axis` のサイズは、すべてのサブビューのサイズに加え、各サブビュー間に定義されているすべての領域の合計になります。
- `LayoutMarginsRelativeArrangement` プロパティが `true`の場合、スタックビューのサイズには余白用の領域も含まれます。
- `Axis` に垂直なサイズは、コレクション内の最大のサブビューに設定されます。

また、スタックビューの**高さ**と**幅**に対して制約を指定することもできます。 この場合、`Distribution` と `Alignment` のプロパティによって決定されるように、スタックビューで指定された領域を埋めるようにサブビューがレイアウト (サイズ設定) されます。

`BaselineRelativeArrangement` プロパティが `true`の場合、**上**、**下**、または**中央**- **Y**位置を使用するのではなく、最初または最後のサブビューのベースラインに基づいてサブビューがレイアウトされます。 これらは、次のようにスタックビューのコンテンツで計算されます。

- 垂直スタックビューでは、最初のベースラインの最初のサブビューと最後のサブビューが返されます。 これらのサブビューがそれ自体のスタックビューである場合は、その最初または最後のベースラインが使用されます。
- 水平スタックビューでは、最初と最後のベースラインの両方に対して最も高いサブビューが使用されます。 最も高いビューがスタックビューでもある場合は、ベースラインとして最も基本的なサブビューが使用されます。

> [!IMPORTANT]
> ベースラインの配置は、拡張または圧縮されたサブビューのサイズに対しては機能しません。これは、ベースラインが間違った位置に計算されるためです。 ベースラインの配置では、サブビューの**高さ**が組み込みコンテンツビューの**高さ**と一致していることを確認します。

### <a name="common-stack-view-uses"></a>一般的なスタックビューの使用

スタックビューコントロールでは、いくつかのレイアウトの種類に対応しています。 Apple によると、一般的な使用方法がいくつかあります。

- **軸に沿ってサイズを定義**します。スタックビューの `Axis` と隣接する端のどちらかに沿って両方の端を固定すると、そのサブビューで定義されている領域に合わせて、軸に沿ってスタックビューが拡大されます。
- サブ**ビューの位置を定義**します。これは、親ビューのスタックビューの隣接する端にピン留めすることによって、両方のディメンションで、サブビューが含まれていることに合わせてスタックビューを拡大します。
- スタック**のサイズと位置を定義**する–スタックビューの4つのすべての辺を親ビューに固定することで、スタックビューで定義されている領域に基づいてサブビューが配置されます。
- **軸の垂直方向のサイズを定義**します。これにより、スタックビューの `Axis` に垂直方向にピン留めし、軸に沿って位置を設定することにより、そのサブビューで定義されている空間に合わせて、スタックビューが軸に対して垂直方向に拡大されます。

### <a name="managing-the-appearance"></a>外観の管理

`UIStackView` は、非レンダリングコンテナービューとして設計されているため、`UIView`の他のサブクラスと同様にキャンバスに描画されることはありません。 `BackgroundColor` や `DrawRect` のオーバーライドなどのプロパティを設定しても、視覚効果はありません。

スタックビューでサブビューのコレクションをどのように配置するかを制御するプロパティがいくつかあります。

- **Axis** –スタックビューでサブビューを**水平**または**垂直**のどちらに整列させるかを決定します。
- **Alignment** –スタックビュー内でサブビューをどのように配置するかを制御します。
- **Distribution** –スタックビュー内でサブビューのサイズを設定する方法を制御します。
- **スペーシング**–スタックビュー内の各サブビュー間の最小限の空白を制御します。
- **ベースライン相対**– `true`場合、各サブビューの上下の間隔は、そのベースラインから派生します。
- **[相対レイアウト]** -標準レイアウトの余白を基準にサブビューを配置します。

通常は、少数のサブビューを配置するためにスタックビューを使用します。 1つ以上のスタックビューを相互に入れ子にすることにより、より複雑なユーザーインターフェイスを作成できます (上記の[Uistackview クイックスタート](#uistackview-quickstart)で行ったように)。

サブビューに制約を追加することで、Ui の外観をさらに細かく調整できます (たとえば、高さや幅を制御します)。 ただし、スタックビュー自体によって導入された制約に競合する制約を含めないように注意する必要があります。

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>配置されたビューとサブビューの一貫性の維持

スタックビューは、次の規則を使用して、その `ArrangedSubviews` プロパティが常にその `Subviews` プロパティのサブセットであることを確認します。

- サブビューが `ArrangedSubviews` コレクションに追加されると、そのサブビューは `Subviews` コレクションに自動的に追加されます (コレクションに含まれていない場合)。
- サブビューが `Subviews` コレクションから削除された場合 (表示から削除された場合)、`ArrangedSubviews` コレクションからも削除されます。
- `ArrangedSubviews` コレクションからサブビューを削除しても、`Subviews` コレクションからは削除されません。 スタックビューによってレイアウトされることはなくなりますが、画面に表示されます。

`ArrangedSubviews` コレクションは常に `Subview` コレクションのサブセットですが、各コレクション内の個々のサブビューの順序は、次のように分けられ、制御されます。

- `ArrangedSubviews` コレクション内のサブビューの順序によって、スタック内での表示順序が決まります。
- `Subview` コレクション内のサブビューの順序によって、ビュー内のそれぞれの Z オーダー (またはレイヤー) が前面に戻されます。

### <a name="dynamically-changing-content"></a>コンテンツの動的な変更

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

## <a name="summary"></a>まとめ

この記事では、Xamarin iOS アプリで水平方向または垂直方向に配置された一連のサブビューを管理するための新しい `UIStackView` コントロール (iOS 9 用) について説明しました。
ここでは、スタックビューを使用して UI を作成する簡単な例から始めて、スタックビューとそのプロパティと機能について詳しく見ていきました。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [UIStackView の概要 (ビデオ)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
