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
ms.openlocfilehash: a926ae3aa91b7c1dbe13ba78c28f4df63228e151
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436485"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin. iOS のスタックビュー

_この記事では、Xamarin. iOS アプリで新しい UIStackView コントロールを使用して、水平方向または垂直方向に配置されたスタックでサブビューのセットを管理する方法について説明します。_

> [!IMPORTANT]
> StackView は iOS Designer でサポートされていますが、安定チャネルを使用するとユーザビリティのバグが発生する可能性があることに注意してください。 ベータチャネルまたはアルファチャネルを切り替えると、この問題が軽減されます。 このチュートリアルでは、必要な修正が安定したチャネルに実装されるまで、Xcode を使用してこのチュートリアルを提示することを決定しました。

スタックビューコントロール () は、 `UIStackView` 自動レイアウトクラスとサイズクラスの機能を活用して、マルチビューのスタックを水平方向または垂直方向に管理します。これは、iOS デバイスの向きと画面サイズに動的に反応します。

スタックビューにアタッチされているすべてのサブビューのレイアウトは、軸、分布、配置、スペースなどの開発者が定義したプロパティに基づいて、it 部門によって管理されます。

[![スタックビューのレイアウトダイアグラム](uistackview-images/stacked01.png)](uistackview-images/stacked01.png#lightbox)

Xamarin iOS アプリでを使用する場合、 `UIStackView` 開発者は、Ios デザイナーのストーリーボード内でサブビューを定義するか、C# コードでサブビューを追加および削除することができます。

このドキュメントは、2つの部分で構成されています。最初のスタックビューを実装するのに役立つクイックスタートと、そのしくみについての技術的な詳細について説明します。

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView ビデオ**

## <a name="uistackview-quickstart"></a>UIStackView クイックスタート

コントロールを簡単に紹介するために `UIStackView` 、ユーザーが 1 ~ 5 の評価を入力できる単純なインターフェイスを作成します。 ここでは、2つのスタックビューを使用します。1つは、デバイスの画面にインターフェイスを垂直方向に配置し、もう1つは画面全体で1-5 の評価アイコンを上下に配置します。

### <a name="define-the-ui"></a>UI を定義する

新しい Xamarin. iOS プロジェクトを開始し、Xcode の Interface Builder の **メインのストーリーボード** ファイルを編集します。 まず、**ビューコントローラー**に1つの**垂直方向のスタックビュー**をドラッグします。

[![ビューコントローラーに1つの垂直方向のスタックビューをドラッグする](uistackview-images/quick01.png)](uistackview-images/quick01.png#lightbox)

**属性インスペクター**で、次のオプションを設定します。

[![スタックビューのオプションを設定する](uistackview-images/quick02.png)](uistackview-images/quick02.png#lightbox)

各値の説明:

- **Axis** –スタックビューでサブビューを **水平** または **垂直**のどちらに整列させるかを決定します。
- **Alignment** –スタックビュー内でサブビューをどのように配置するかを制御します。
- **Distribution** –スタックビュー内でサブビューのサイズを設定する方法を制御します。
- **スペーシング** –スタックビュー内の各サブビュー間の最小限の空白を制御します。
- **[ベースラインの相対** ] –このチェックボックスをオンにすると、各サブビューの上下の間隔がそのベースラインから派生します。
- [**相対レイアウト**]-標準レイアウトの余白を基準にサブビューを配置します。

スタックビューを使用する場合、**配置**は、サブビューの**X**および**Y**位置、および**高さ**と**幅**とし**て考える**ことができます。

> [!IMPORTANT]
> `UIStackView` は非レンダリングコンテナービューとして設計されているため、の他のサブクラスと同様にキャンバスに描画されることはありません `UIView` 。 そのため、やのオーバーライドなどのプロパティを設定しても `BackgroundColor` `DrawRect` 視覚効果はありません。

次のように、ラベル、ImageView、2つのボタン、水平スタックビューを追加して、アプリのインターフェイスのレイアウトを続けます。

[![スタックビュー UI のレイアウト](uistackview-images/quick03.png)](uistackview-images/quick03.png#lightbox)

次のオプションを使用して、水平方向のスタックビューを構成します。

[![水平方向のスタックビューのオプションを構成する](uistackview-images/quick04.png)](uistackview-images/quick04.png#lightbox)

横方向のスタックビューに追加したときに、評価の各 "ポイント" を表すアイコンを拡大する必要がないので、 **配置** を [ **中央** 揃え] に設定し、 **分布** を **均等に塗りつぶす**ように設定しました。

最後に、次の **アウトレット** と **アクション**を接続します。

[![スタックビューのコンセントとアクション](uistackview-images/quick05.png)](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>コードから UIStackView にデータを設定する

Visual Studio for Mac に戻り、 **ViewController.cs** ファイルを編集して、次のコードを追加します。

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

このコードのいくつかの部分を詳しく見てみましょう。 まず、ステートメントを使用し `if` て、5つ以上の "星" または0未満の値があることを確認します。

新しい "star" を追加するには、イメージを読み込み、その **コンテンツモード** を [ **縦横合わせる**] に設定します。

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

これにより、"星" アイコンがスタックビューに追加されるときに、そのアイコンがゆがんでしまいます。

次に、新しい "star" アイコンをスタックビューのサブビューのコレクションに追加します。

```csharp
RatingView.AddArrangedSubview(icon);
```

は、にではなく、のプロパティに追加されていることがわかり `UIImageView` `UIStackView` `ArrangedSubviews` `SubView` ます。 スタックビューでレイアウトを制御するビューをプロパティに追加する必要があり `ArrangedSubviews` ます。

スタックビューからサブビューを削除するには、まず、削除するサブビューを取得します。

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

次に、コレクションとスーパービューの両方から削除する必要があり `ArrangedSubviews` ます。

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

コレクションだけからサブビューを削除すると、 `ArrangedSubviews` スタックビューのコントロールから除外されますが、画面からは削除されません。

### <a name="testing-the-ui"></a>UI のテスト

必要なすべての UI 要素とコードを配置して、インターフェイスを実行およびテストできるようになりました。 UI が表示されると、垂直スタックビュー内のすべての要素が、上から下に均等に配置されます。

ユーザーが [評価の **拡大** ] ボタンをタップすると、別の "星" が画面に追加されます (最大5個まで)。

[![サンプルアプリの実行](uistackview-images/intro01.png)](uistackview-images/intro01.png#lightbox)

"星" は、水平スタックビューで自動的に中央揃えになり、均等に配分されます。 ユーザーが [評価を **下げる** ] ボタンをタップすると、"star" が削除されます (いずれかが残っている場合)。

## <a name="stack-view-details"></a>スタックビューの詳細

これで、コントロールの概要と動作方法がわかりました。次は、 `UIStackView` その機能と詳細について詳しく見ていきましょう。

### <a name="auto-layout-and-size-classes"></a>自動レイアウトとサイズのクラス

前に説明したように、ビューがスタックビューに追加されると、[自動レイアウト] と [サイズ] クラスを使用して、配置されたビューの位置とサイズを示す、そのスタックビューによってレイアウトが全体的に制御されます。

スタックビューでは、コレクション内の最初と最後のサブビューが、垂直スタックビューの**上端**と**下端**、および水平スタックビューの**左端**と**右端**に_固定_されます。 プロパティをに設定した場合、 `LayoutMarginsRelativeArrangement` `true` ビューでは、境界線がエッジではなく関連する余白にピン留めされます。

スタックビューは、 `IntrinsicContentSize` 定義されたに沿ってサブビューのサイズを計算するときにサブビューのプロパティを使用し `Axis` ます (を除く `FillEqually Distribution` )。 では、すべてのサブビューの `FillEqually Distribution` サイズが同じになるようにサイズが変更されるため、に沿ってスタックビューが塗りつぶされ `Axis` ます。

を除き、 `Fill Alignment` スタックビューはサブビューのプロパティを使用して `IntrinsicContentSize` 、指定されたに対するビューのサイズを垂直に計算し `Axis` ます。 では、すべてのサブビューのサイズが変更され、指定されたに対して `Fill Alignment` スタックビューが垂直になり `Axis` ます。

### <a name="positioning-and-sizing-the-stack-view"></a>スタックビューの配置とサイズ変更

スタックビューには、サブビューのレイアウト全体の制御があります (やなどのプロパティに基づいてい `Axis` `Distribution` ます) が、 `UIStackView` Auto Layout クラスと Size クラスを使用して、その親ビュー内でスタックビュー () を配置する必要があります。

一般に、これは、展開とコントラクトを行うためにスタックビューの少なくとも2つの端を固定して、その位置を定義することを意味します。 追加の制約がなければ、次のように、すべてのサブビューに合わせてスタックビューのサイズが自動的に変更されます。

- のサイズは、 `Axis` すべてのサブビューのサイズに加え、各サブビュー間に定義されているすべての領域の合計になります。
- プロパティがの場合は `LayoutMarginsRelativeArrangement` `true` 、スタックビューのサイズに余白用の領域も含まれます。
- に垂直のサイズが、 `Axis` コレクション内の最大のサブビューに設定されます。

また、スタックビューの **高さ** と **幅**に対して制約を指定することもできます。 この場合、サブビューは、およびプロパティによって決定されるスタックビューで指定された領域を埋めるようにレイアウト (サイズ設定) され `Distribution` `Alignment` ます。

プロパティがの場合、サブビューは、 `BaselineRelativeArrangement` `true` **上**、**下**、または**中央**の Y 位置を使用するのではなく、最初または最後のサブビューのベースラインに基づいてレイアウトされ -  **Y**ます。 これらは、次のようにスタックビューのコンテンツで計算されます。

- 垂直スタックビューでは、最初のベースラインの最初のサブビューと最後のサブビューが返されます。 これらのサブビューがそれ自体のスタックビューである場合は、その最初または最後のベースラインが使用されます。
- 水平スタックビューでは、最初と最後のベースラインの両方に対して最も高いサブビューが使用されます。 最も高いビューがスタックビューでもある場合は、ベースラインとして最も基本的なサブビューが使用されます。

> [!IMPORTANT]
> ベースラインの配置は、拡張または圧縮されたサブビューのサイズに対しては機能しません。これは、ベースラインが間違った位置に計算されるためです。 ベースラインの配置では、サブビューの **高さ** が組み込みコンテンツビューの **高さ**と一致していることを確認します。

### <a name="common-stack-view-uses"></a>一般的なスタックビューの使用

スタックビューコントロールでは、いくつかのレイアウトの種類に対応しています。 Apple によると、一般的な使用方法がいくつかあります。

- **軸に沿ってサイズを定義** します。スタックビューの端と隣接する端のどちらかを固定して位置を設定することにより、そのサブビュー `Axis` で定義されている領域に合わせて、スタックビューが軸に沿って拡大されます。
- サブ**ビューの位置を定義**します。これは、親ビューのスタックビューの隣接する端にピン留めすることによって、両方のディメンションで、サブビューが含まれていることに合わせてスタックビューを拡大します。
- スタック**のサイズと位置を定義**する–スタックビューの4つのすべての辺を親ビューに固定することで、スタックビューで定義されている領域に基づいてサブビューが配置されます。
- **軸の垂直方向のサイズを定義** します。スタックビューに垂直方向にピン留めすることにより、軸に沿って位置を設定することによって、スタックビューは、サブビュー `Axis` で定義されている領域に合わせて、軸に対して垂直方向に拡大されます。

### <a name="managing-the-appearance"></a>外観の管理

は `UIStackView` 非レンダリングコンテナービューとして設計されているため、の他のサブクラスと同様にキャンバスに描画されることはありません `UIView` 。 やのオーバーライドなどのプロパティを設定しても `BackgroundColor` 、 `DrawRect` 視覚効果はありません。

スタックビューでサブビューのコレクションをどのように配置するかを制御するプロパティがいくつかあります。

- **Axis** –スタックビューでサブビューを **水平** または **垂直**のどちらに整列させるかを決定します。
- **Alignment** –スタックビュー内でサブビューをどのように配置するかを制御します。
- **Distribution** –スタックビュー内でサブビューのサイズを設定する方法を制御します。
- **スペーシング** –スタックビュー内の各サブビュー間の最小限の空白を制御します。
- **ベースライン相対** –の場合 `true` 、各サブビューの上下の間隔は、そのベースラインから派生します。
- [**相対レイアウト**]-標準レイアウトの余白を基準にサブビューを配置します。

通常は、少数のサブビューを配置するためにスタックビューを使用します。 1つ以上のスタックビューを相互に入れ子にすることにより、より複雑なユーザーインターフェイスを作成できます (上記の [Uistackview クイックスタート](#uistackview-quickstart) で行ったように)。

サブビューに制約を追加することで、Ui の外観をさらに細かく調整できます (たとえば、高さや幅を制御します)。 ただし、スタックビュー自体によって導入された制約に競合する制約を含めないように注意する必要があります。

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>配置されたビューとサブビューの一貫性の維持

スタックビューは、 `ArrangedSubviews` `Subviews` 次の規則を使用して、プロパティが常にそのプロパティのサブセットであることを確認します。

- サブビューがコレクションに追加されると、コレクションに `ArrangedSubviews` 自動的に追加されます (コレクションに含まれてい `Subviews` ない場合)。
- サブビューがコレクションから削除された場合 `Subviews` (表示から削除された場合)、コレクションからも削除され `ArrangedSubviews` ます。
- コレクションからサブビューを削除しても、 `ArrangedSubviews` コレクションからは削除されません `Subviews` 。 スタックビューによってレイアウトされることはなくなりますが、画面に表示されます。

コレクションは `ArrangedSubviews` 常にコレクションのサブセットですが、 `Subview` 各コレクション内の個々のサブビューの順序は、次のように分けられ、制御されます。

- コレクション内のサブビューの順序によって、 `ArrangedSubviews` スタック内での表示順序が決まります。
- コレクション内のサブビューの順序によって、 `Subview` ビュー内のそれぞれの Z オーダー (またはレイヤー) が手前に戻ります。

### <a name="dynamically-changing-content"></a>コンテンツの動的な変更

サブビューが追加、削除、または非表示になるたびに、スタックビューによってサブビューのレイアウトが自動的に調整されます。 また、スタックビューの任意のプロパティ (など) が調整された場合にも、レイアウトが調整され `Axis` ます。

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

この記事では、 `UIStackView` Xamarin ios アプリで水平方向または垂直方向に配置された一連のサブビューを管理する新しいコントロール (iOS 9 用) について説明しました。
ここでは、スタックビューを使用して UI を作成する簡単な例から始めて、スタックビューとそのプロパティと機能について詳しく見ていきました。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [UIStackView の概要 (ビデオ)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)