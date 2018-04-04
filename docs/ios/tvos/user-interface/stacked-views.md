---
title: 積み上げビューの操作
description: この記事では、設計とビューの積み上げ Xamarin.tvOS アプリ内での操作について説明します。
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: a6300e4da47022199c0503e6be63b0c90f15654d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-stacked-view"></a>積み上げビューの操作

_この記事では、設計とビューの積み上げ Xamarin.tvOS アプリ内での操作について説明します。_


スタック ビュー コントロール (`UIStackView`) および Apple TV のデバイスの画面のサイズのコンテンツの変更を動的に応答する水平方向または垂直方向にサブビューのスタックを管理するには、自動レイアウトおよびサイズ クラスの機能を利用しています。

スタック ビューにアタッチされているすべてのサブビューのレイアウトは、軸、配布、調整間隔などの開発者が定義されているプロパティに基づいてによって管理されます。

[![](stacked-views-images/stacked01.png "サブビュー レイアウトの図")](stacked-views-images/stacked01.png#lightbox)

使用する場合、 `UIStackView` Xamarin.tvOS アプリで、開発者はかサブビュー iOS デザイナー、または削除して、追加のサブビュー c# コードでストーリー ボード内のいずれかを定義します。

## <a name="about-stacked-view-controls"></a>積み上げビュー コントロールの概要 

`UIStackView`ように設計された他のサブクラスのようなキャンバスに描画されていない非表示コンテナーのビューとして、そのため、`UIView`です。 などのプロパティを設定`BackgroundColor`やオーバーライド`DrawRect`ビジュアル効果はありません。

スタック ビューがそのサブビューのコレクションを配置する方法を制御するいくつかのプロパティがあります。

- **軸**– スタック ビューがかサブビューを整列するかどうかを判断**水平方向に**または**垂直方向に**です。
- **配置**– サブビュー スタック ビュー内で整列する方法を制御します。
- **配布**– サブビューにスタック ビュー内でサイズが設定する方法を制御します。
- **間隔**– スタック ビューでは、各サブビュー間の最小限の空白を制御します。
- **ベースライン相対**– `true`、各サブビューの上下の間隔をそのベースラインから導き出されます。
- **レイアウトの余白相対**– サブビュー標準レイアウトの余白を基準に配置します。

通常はサブビューの数が少ないを配置するのにスタック ビューを使用します。 複雑なユーザー インターフェイスは、互いの内部に 1 つ以上のスタック ビューを入れ子にして作成できます。

サブビュー (たとえばにコントロールの高さまたは幅) に追加の制約を追加することで、Ui の外観をさらに細かく調整できます。 ただし、注意が必要自体スタック ビューで導入されたものに、競合する制約を含める必要はありません。

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>自動レイアウトおよびサイズ クラス

サブビューがスタック ビューに追加されたときに、完全にによって制御されるレイアウト位置および配置のビューのサイズに自動レイアウトおよびサイズ クラスを使用してそのスタック ビュー。

スタック ビューは_pin_ためには、そのコレクション内の最初と最後のサブビュー、**上部**と**下部**スタック ビューを垂直方向のエッジまたは**を左**と**右**スタック ビューを水平方向のエッジ。 設定した場合、`LayoutMarginsRelativeArrangement`プロパティを`true`ビュー、エッジではなく、関連する余白にサブビューをピン留めし、します。

スタック ビューが使用されたサブビューの`IntrinsicContentSize`プロパティの定義に沿ってサブビュー サイズを計算するときに`Axis`(を除き、 `FillEqually Distribution`)。 `FillEqually Distribution`サブビューがすべてのサイズを変更して得られるよう、同じサイズに沿った履歴ビューを埋めるため、`Axis`です。

例外を除いて、 `Fill Alignment`、スタック ビューが使用されたサブビューの`IntrinsicContentSize`に垂直なビューのサイズを計算するためのプロパティ、指定された`Axis`です。 `Fill Alignment`に垂直スタック ビューを入力するようにサブビューがすべてのサイズは、指定された`Axis`です。

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>配置やスタック ビューをサイズ変更

スタック ビューにサブビューのレイアウトを完全に制御があるときに (などのプロパティに基づく`Axis`と`Distribution`)、スタック ビューを配置する必要があります (`UIStackView`) 自動レイアウトおよびサイズのクラスを使用して親ビュー内です。

一般に、つまり、スタック ビューを展開し、コントラクト、ため、その位置を定義するのには、少なくとも 2 つのエッジをピン留めします。 せず、追加の制約は、すべてを収めるそのサブビューの次のように、スタック ビューをサイズ変更自動的には。

* に沿ってサイズ、`Axis`サブビューのすべてのサイズと各サブビュー間で定義されているすべての領域の合計になります。
* 場合、`LayoutMarginsRelativeArrangement`プロパティは`true`、スタック ビュー サイズには、余白の領域も含まれます。
* サイズを垂直、`Axis`コレクション内で最大サブビューに設定されます。

スタック ビューの制約を指定するさらに、**高さ**と**幅**です。 この場合、サブビューをレイアウトする (規模) によって決定されるスタック ビューで指定された領域を埋める、`Distribution`と`Alignment`プロパティです。

場合、`BaselineRelativeArrangement`プロパティは`true`、サブビューをレイアウトするを使用せずに、最初または最サブビューの基準に基づいて、**上部**、**下部**または **Center*-  **Y**位置。 スタック ビューのコンテンツに対するよう計算これらされます。

* 垂直スタック ビューは、最初の基準の最初のサブビューと最後の最後に戻ります。 スタック ビュー自体これらサブビューのいずれかの場合は、最初と最後のベースラインが使用されます。
* 水平方向のスタック ビュー最初と最後の両方の基準に、最も高いサブビューが使用されます。 最も高いビューは、スタック ビューではまた場合、は、最も高いサブビューをベースラインとして使用します。

> [!IMPORTANT]
> ベースライン配置機能しない. 拡張または圧縮されたサブビュー サイズで間違った位置に、基準が計算されます。 ベースライン配置のことを確認サブビューの**高さ**組み込みのコンテンツ ビューと一致する**高さ**です。




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>スタック ビューの一般的な使用方法

スタック ビュー コントロールでうまく機能をいくつかのレイアウトの種類があります。 Apple にに従っていくつかの一般的な使用方法を次に示します。

- **に沿って、軸のサイズを定義**– に沿ってスタック ビューの両方の端をピン留めする`Axis`と 1 つの隣接する端をそのサブビューによって定義された領域に合わせて軸に沿って表示が大きくなり、スタックの位置を設定します。
*  **サブビューの位置を定義**– 両方のディメンションを含むサブビューに合わせて親ビューにスタック ビューの隣接する端に固定、スタック ビューが拡大します。
- **スタックの位置とサイズを定義する**– 親ビューにスタック ビューのすべての 4 つのエッジをピン留めするスタック ビューがスタック ビュー内で定義されている領域に基づいたサブビューを整列します。
*  **サイズの垂直軸を定義する**– エッジに対して垂直スタック ビューの両方をピン留めする`Axis`ビューがによって定義された領域に合わせて軸と垂直に大きくなり、スタックの位置を設定する軸に沿ったのエッジのいずれかとそのサブビューです。

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>スタック ビューとストーリー ボード

Xamarin.tvOS アプリでは、スタック ビューを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション パッド**、ダブルクリック、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. スタック ビューに追加しようとする、個々 の要素のレイアウトをデザインします。 

    [![](stacked-views-images/layout01.png "要素のレイアウトの例")](stacked-views-images/layout01.png#lightbox)
1. 正しくスケーリングすることを確認する要素に必要な制約を追加します。 要素がスタック ビューに追加されると、このステップは重要です。
1. 必要な部数 (ここでは 4 つ) を行います。 

    [![](stacked-views-images/layout02.png "必要なコピー数")](stacked-views-images/layout02.png#lightbox)
1. ドラッグ、**スタック ビュー**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](stacked-views-images/layout03.png "スタック ビュー")](stacked-views-images/layout03.png#lightbox)
1. スタック ビューを選択、**ウィジェット タブ**の**プロパティ パッド**選択**塗りつぶし**の**配置**、**塗りつぶし均等に**の**配布**入力と`25`の**間隔**: 

    [![](stacked-views-images/layout04.png "ウィジェット タブ")](stacked-views-images/layout04.png#lightbox)
1. 場所を選択し、その場所に保管して、必要な制約を追加、画面上のスタック表示を配置します。
1. 個々 の要素を選択し、スタック ビューにドラッグします。 

    [![](stacked-views-images/layout05.png "スタック ビュー内の個々 の要素")](stacked-views-images/layout05.png#lightbox)
1. レイアウトが調整され、設定済みの属性に基づいたスタック ビュー内の要素に配置されます。
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ エクスプ ローラー** c# コードで UI コントロールを使用します。
1. 変更内容を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプ ローラー**、ダブルクリック、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. スタック ビューに追加しようとする、個々 の要素のレイアウトをデザインします。 

    [![](stacked-views-images/layout01.png "要素のレイアウトの例")](stacked-views-images/layout01.png#lightbox)
1. 正しくスケーリングすることを確認する要素に必要な制約を追加します。 要素がスタック ビューに追加されると、このステップは重要です。
1. 必要な部数 (ここでは 4 つ) を行います。 

    [![](stacked-views-images/layout02.png "必要なコピー数")](stacked-views-images/layout02.png#lightbox)
1. ドラッグ、**スタック ビュー**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](stacked-views-images/layout03-vs.png "スタック ビュー")](stacked-views-images/layout03-vs.png#lightbox)
1. スタック ビューを選択、**ウィジェット タブ**の**プロパティ エクスプ ローラー**選択**塗りつぶし**の**配置**、**塗りつぶし均等に**の**配布**入力と`25`の**間隔**: 

    [![](stacked-views-images/layout04-vs.png "ウィジェット タブ")](stacked-views-images/layout04-vs.png#lightbox)
1. 場所を選択し、その場所に保管して、必要な制約を追加、画面上のスタック表示を配置します。
1. 個々 の要素を選択し、スタック ビューにドラッグします。 

    [![](stacked-views-images/layout05-vs.png "スタック ビュー内の個々 の要素")](stacked-views-images/layout05-vs.png#lightbox)
1. レイアウトが調整され、設定済みの属性に基づいたスタック ビュー内の要素に配置されます。
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ エクスプ ローラー** c# コードで UI コントロールを使用します。
1. 変更内容を保存します。

-----

> [!IMPORTANT]
> などのアクションを割り当てることができますが`TouchUpInside`UI 要素 (など、 `UIButton`)、ios では、イベント ハンドラーを作成するときに、デザイナー、これは決して呼び出されません Apple TV はタッチ画面またはタッチ イベントのサポートがあるないためです。 既定値を常に使用する必要があります`Action Type`ユーザー インターフェイス要素 tvOS のアクションを作成する場合。

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。

この例の場合、コンセントとセグメント コントロールのアクションと「player カードごと」コンセントを公開おしました。 コードではおを非表示にされ、現在のセグメントに基づくプレーヤーを表示します。 例えば:

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

アプリの実行時に 4 つの要素は、スタック ビュー内に均等に分散します。

[![](stacked-views-images/layout06.png "4 つの要素が、スタック ビューに均等に分散するアプリの実行時に")](stacked-views-images/layout06.png#lightbox)

プレーヤー数が低下している場合、未使用のビューが非表示になり、スタック ビューに合わせてレイアウトを調整します。

[![](stacked-views-images/layout07.png "プレーヤー数が低下している場合、未使用のビューが非表示になりスタック ビューに合わせてレイアウトを調整します。")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>コードからスタック ビューへの追加します。

まったく内容とスタック ビューのレイアウトを定義する、iOS デザイナーで、他は、作成し、c# コードから動的に削除することができます。

スタック ビューを使用して、レビュー (1 ~ 5) で「星」を処理する次の例を実行します。

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

詳しくは、このコードの部分をいくつか見てを見てみましょう。 最初に、使用、`if`以上 5「星」がないことを確認するステートメント 0 よりも小さいです。

新しい「スター」を追加するには、そのイメージを読み込むことを設定およびその**コンテンツ モード**に**スケール縦横に合わせて**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

これにより、スタック ビューに追加されたときに音がひずむされてから、「スター型」のアイコンが保持されます。

次にサブビューのスタック ビューのコレクションに新しい「スター」アイコンを追加します。

```csharp
RatingView.AddArrangedSubview(icon);
```

追加お気づき、`UIImageView`を`UIStackView`の`ArrangedSubviews`プロパティとしないように、`SubView`です。 スタック ビューのレイアウトを制御する任意のビューに追加する必要があります、`ArrangedSubviews`プロパティです。

スタック ビューからサブビューを削除するに最初に取得サブビューを削除します。

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

両方から削除する必要があり、`ArrangedSubviews`コレクションとスーパー ビュー。

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

だけからサブビューを削除する`ArrangedSubviews`コレクションは、スタック ビューのコントロールから受け取るが、画面からは削除されません。

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>コンテンツを動的に変更します。

スタック ビューでは、サブビューが追加、削除または非表示にするたびに、サブビューのレイアウトは自動的に調整します。 レイアウトは、スタック ビューの任意のプロパティを調整する場合も調整される (など、 `Axis`)。

たとえば、アニメーション ブロック内で挿入して、レイアウトの変更をアニメーション化できます。

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

スタック ビューのプロパティの多くは、ストーリー ボード内のサイズのクラスを使用して指定できます。 これらのプロパティが自動的にされますサイズまたは向きの変更に対する応答は、アニメーション化します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計とビューの積み上げ Xamarin.tvOS アプリ内での操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
