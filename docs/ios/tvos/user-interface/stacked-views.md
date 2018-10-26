---
title: Xamarin で tvOS 積み上げビューを使用します。
description: このドキュメントでは、Xamarin でビルドされたアプリでの積み上げビューの tvOS 協力する方法について説明します。 スタック ビューの概要を提供し、自動レイアウト、位置およびサイズ積み上げ表示、一般的な使用して、ストーリー ボードとの統合などについて説明します。
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f51ed3d6dbbfc8a7e430c2949485838a7471e545
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110766"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Xamarin で tvOS 積み上げビューを使用します。

スタック ビュー コントロール (`UIStackView`) 機能を水平方向または垂直方向に、サブビューのスタックを管理するには、自動レイアウトとサイズ クラスの内容は変化し、Apple TV のデバイスの画面サイズに動的に応答を活用しています。

スタック ビューにアタッチされているすべてのサブビューのレイアウトは、軸、配布、配置、および間隔などの開発者は定義されたプロパティに基づいて、管理されます。

[![](stacked-views-images/stacked01.png "サブビュー レイアウトの図")](stacked-views-images/stacked01.png#lightbox)

使用する場合、 `UIStackView` Xamarin.tvOS アプリケーションでは、開発者は、iOS Designer、または追加および削除のサブビューのストーリー ボード内のいずれかのサブビューを定義C#コード。

## <a name="about-stacked-view-controls"></a>スタック ビュー コントロールの概要 

`UIStackView`いますの他のサブクラスのようにキャンバスに描画されていない非表示コンテナー ビューとして、そのため、`UIView`します。 などのプロパティを設定`BackgroundColor`オーバーライドまたは`DrawRect`視覚効果はありません。

スタック ビューがそのサブビューのコレクションを配置する方法を制御するいくつかのプロパティがあります。

- **軸**– スタック ビューが、サブビューを配置するかどうかを決定します**水平方向に**または**垂直方向に**します。
- **配置**– スタック ビュー内で、サブビューを配置する方法を制御します。
- **配布**– サブビューのスタック ビュー内でサイズはどのように制御します。
- **間隔**– スタック ビューには、各サブビューの間の最小限のスペースを制御します。
- **ベースライン相対**– `true`、各サブビューの上下の間隔をそのベースラインから導き出されます。
- **レイアウトの余白相対**– 標準的なレイアウトの余白を基準とした、サブビューを配置します。

一般的にはスタック ビューは、サブビューの数が少ないを配置するために使用します。 複雑なユーザー インターフェイスは、互いの内部で 1 つ以上の履歴ビューを入れ子にして作成できます。

(たとえば、コントロールの高さまたは幅) にサブビューを追加の制約を追加することで、Ui の外観をさらに微調整できます。 ただし、注意してください自体スタック ビューで導入されたものに競合する制約を含める必要はありません。

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>自動レイアウトとサイズ クラス

サブビューがスタック ビューに追加されたときにそのレイアウトが完全に自動レイアウトとサイズ クラスの位置し、サイズ、配置後のビューを使用してそのスタック ビューによって制御されます。

スタック ビューは_pin_をそのコレクション内の最初と最後のサブビュー、**上部**と**下部**スタック ビューを垂直方向の端または**を左**と**右**スタック ビューを水平方向のエッジ。 設定した場合、`LayoutMarginsRelativeArrangement`プロパティを`true`ビュー、サブビュー、edge の代わりに関連する余白をピン留めします。

スタック ビューで使用する、サブビューの`IntrinsicContentSize`プロパティの定義に沿ってサブビューのサイズを計算するときに`Axis`(を除き、 `FillEqually Distribution`)。 `FillEqually Distribution`されるので、同じサイズでは、すべてのサブビューのサイズを変更したがってに沿った履歴ビューを入力、`Axis`します。

例外として、 `Fill Alignment`、スタック ビューで使用する、サブビューの`IntrinsicContentSize`に垂直ビューのサイズを計算するためのプロパティ、指定された`Axis`。 `Fill Alignment`、に対して垂直スタック ビューを入力するようにすべてのサブビューのサイズは、指定された`Axis`します。

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>位置およびサイズ スタック ビュー

スタック ビューにサブビューのレイアウトを完全に制御があるときに (などのプロパティに基づいた`Axis`と`Distribution`)、スタックのビューを配置する必要があります (`UIStackView`) 自動レイアウトとサイズ クラスを使用して親ビュー内で。

つまり、通常の拡大/縮小、その位置を定義するためにスタック ビューに少なくとも 2 つの端をピン留めします。 せず、追加の制約は、サブビューのすべてに適合するように、スタック ビューをサイズ変更に自動的には。

* に沿ってサイズ、`Axis`サブビューのすべてのサイズと各サブビューの間で定義されているすべての領域の合計になります。
* 場合、`LayoutMarginsRelativeArrangement`プロパティは`true`、スタック ビュー サイズには、余白の領域も含まれます。
* サイズを垂直、`Axis`はコレクション内の最大のサブビューに設定されます。

スタック ビューの制約を指定するさらに、**高さ**と**幅**します。 この場合は、サブビューをレイアウトする (サイズ) によって決定される履歴ビューで指定された領域を埋める、`Distribution`と`Alignment`プロパティ。

場合、`BaselineRelativeArrangement`プロパティは`true`、サブビューをレイアウトするを使用してではなく、最初と最後のサブビューの基準に基づいて、**上部**、**下部**または **Center*-  **Y**位置。 スタック ビューのコンテンツに次のように計算これらされます。

* 最初の基準の最初のサブビューと最後の最後には、垂直方向のスタック ビューを返します。 これらのサブビューのいずれかが、スタック ビュー自体が場合は、その初または最後の基準が使用されます。
* 水平方向のスタック ビュー最初と最後の両方の基準に、最も高いサブビューが使用されます。 Tallest ビューも履歴ビューである場合は、tallest サブビューをベースラインとして使用します。

> [!IMPORTANT]
> ベースライン配置では動作しませんサブビューを拡張または圧縮サイズとして、間違った位置に、基準が計算されます。 ベースラインの配置、ようにサブビューの**高さ**組み込みのコンテンツ ビューと一致する**高さ**します。




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>スタック ビューの一般的な使用

スタック ビュー コントロールと連動するいくつかのレイアウトの種類があります。 に従って、Apple は、次にいくつかの一般的な使用方法を示します。

- **定義、軸に沿ったサイズ**– 両方の端に沿ってスタック ビューのピン留めして`Axis`サブビューによって定義された領域に合わせて軸に沿ったビューのサイズが増加するスタックの位置を設定する隣接辺の 1 つ。
*  **サブビューの位置を定義**– に合わせて格納サブビューの寸法の両方で、親ビューにスタック ビューの隣接辺に固定してスタック ビューが拡張されます。
- **スタックの位置とサイズを定義する**– スタック ビューを親ビューの履歴ビューのすべての 4 辺をピン留めするには、スタック ビュー内で定義される領域に応じてサブビューを配置します。
*  **サイズの垂直軸を定義**– エッジに対して垂直スタック ビューの両方をピン留めして`Axis`1 つのビューがによって定義された領域に合わせて axis に対して垂直な拡張スタックの位置を設定する軸に沿った端とそのサブビューします。

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>スタック ビューとストーリー ボード

Xamarin.tvOS アプリでスタック ビューを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**、ダブルクリック、`Main.storyboard`ファイルし、編集用に開きます。
1. スタック ビューに追加しようとする、個々 の要素のレイアウトをデザインするには。 

    [![](stacked-views-images/layout01.png "要素のレイアウトの例")](stacked-views-images/layout01.png#lightbox)
1. 正常にスケールできるようにする要素に必要な制約を追加します。 スタック ビューに、要素が追加されると、この手順が重要です。
1. 必要な数 (この場合は 4 つ) のコピーを行います。 

    [![](stacked-views-images/layout02.png "必要なコピー数")](stacked-views-images/layout02.png#lightbox)
1. ドラッグ、**スタック ビュー**から、**ツールボックス**し、ビューにドロップします。 

    [![](stacked-views-images/layout03.png "スタック ビュー")](stacked-views-images/layout03.png#lightbox)
1. スタック ビューを選択、**ウィジェット タブ**の**Properties Pad**選択**入力**の**配置**、**塗りつぶし同様に**の**配布**入力`25`の**間隔**: 

    [![](stacked-views-images/layout04.png "[ウィジェット] タブ")](stacked-views-images/layout04.png#lightbox)
1. 場所を選択し、必要な場所をそのままに制約を追加の画面にスタック ビューを配置します。
1. 個々 の要素を選択し、スタック ビューにドラッグします。 

    [![](stacked-views-images/layout05.png "スタック ビューには、個々 の要素")](stacked-views-images/layout05.png#lightbox)
1. レイアウトが調整され、上記で設定した属性に基づくスタック ビュー内の要素に配置されます。
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ エクスプ ローラー**で UI コントロールを使用するC#コード。
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプ ローラー**、ダブルクリック、`Main.storyboard`ファイルし、編集用に開きます。
1. スタック ビューに追加しようとする、個々 の要素のレイアウトをデザインするには。 

    [![](stacked-views-images/layout01.png "要素のレイアウトの例")](stacked-views-images/layout01.png#lightbox)
1. 正常にスケールできるようにする要素に必要な制約を追加します。 スタック ビューに、要素が追加されると、この手順が重要です。
1. 必要な数 (この場合は 4 つ) のコピーを行います。 

    [![](stacked-views-images/layout02.png "必要なコピー数")](stacked-views-images/layout02.png#lightbox)
1. ドラッグ、**スタック ビュー**から、**ツールボックス**し、ビューにドロップします。 

    [![](stacked-views-images/layout03-vs.png "スタック ビュー")](stacked-views-images/layout03-vs.png#lightbox)
1. スタック ビューを選択、**ウィジェット タブ**の**プロパティ エクスプ ローラー**選択**入力**の**配置**、**塗りつぶし同様に**の**配布**入力`25`の**間隔**: 

    [![](stacked-views-images/layout04-vs.png "[ウィジェット] タブ")](stacked-views-images/layout04-vs.png#lightbox)
1. 場所を選択し、必要な場所をそのままに制約を追加の画面にスタック ビューを配置します。
1. 個々 の要素を選択し、スタック ビューにドラッグします。 

    [![](stacked-views-images/layout05-vs.png "スタック ビューには、個々 の要素")](stacked-views-images/layout05-vs.png#lightbox)
1. レイアウトが調整され、上記で設定した属性に基づくスタック ビュー内の要素に配置されます。
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ エクスプ ローラー**で UI コントロールを使用するC#コード。
1. 変更内容を保存します。

-----

> [!IMPORTANT]
> などのアクションを割り当てることはできますが`TouchUpInside`UI 要素に (など、 `UIButton`) iOS Designer のイベント ハンドラーを作成するときに、これは呼び出されません Apple TV がタッチ画面またはタッチ イベントをサポートしていないためです。 既定値を常に使用する必要があります`Action Type`tvOS ユーザー インターフェイス要素のアクションを作成するときにします。

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。

この例の場合を公開しています、コンセントとセグメントのコントロールのアクションとアウトレット カードごとに"player"。 コードでは非表示にし、プレーヤーの現在のセグメントに基づくを示します。 例えば:

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

アプリを実行すると、4 つの要素は、スタック ビューには分散均等に。

[![](stacked-views-images/layout06.png "4 つの要素がスタック ビューに均等分散するアプリを実行すると、")](stacked-views-images/layout06.png#lightbox)

選手の数が低下している場合は、未使用のビューが非表示し、スタック ビューに合わせてレイアウトを調整します。

[![](stacked-views-images/layout07.png "選手の数が低下している場合は、未使用のビューが非表示し、スタック ビューに合わせてレイアウトを調整します。")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>コードからの履歴ビューを設定します。

IOS Designer で、内容、およびスタック ビューのレイアウトを完全に定義、に加えてを作成および削除から動的にC#コード。

スタック ビューを使用してレビュー (1 ~ 5) の「星」を処理する次の例を実行します。

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

詳細、いくつかのこのコードを見てをみましょう。 まず、使用、 `if` 「星」の 5 つ以上が含まれていないことを確認するステートメント 0 よりも小さい。

そのイメージを読み込み新しい"star"を追加する設定とその**コンテンツ モード**に**縦横に合わせてスケール**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

これにより、「スター」アイコンが変形スタック ビューに追加されたとき。

次に、新しい「スター」アイコンは、サブビューのスタック ビューのコレクションに追加します。

```csharp
RatingView.AddArrangedSubview(icon);
```

追加されていますがわかります、`UIImageView`を`UIStackView`の`ArrangedSubviews`プロパティなく、`SubView`します。 スタック ビューのレイアウトを制御する任意のビューに追加する必要があります、`ArrangedSubviews`プロパティ。

スタック ビューから、サブビューを削除するにまず、サブビューを削除します。

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

両方から削除する必要があり、`ArrangedSubviews`コレクションとスーパー ビュー。

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

サブビューを削除する`ArrangedSubviews`コレクション スタック ビューのコントロール、外はこれが画面からは削除されません。

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>コンテンツを動的に変更します。

スタック ビュー、サブビューを追加、削除または非表示にするたびに、サブビューのレイアウトは自動的に調整します。 スタック ビューの任意のプロパティを調整する場合、レイアウトが調整も (など、 `Axis`)。

たとえば、アニメーション ブロック内で配置してレイアウトの変更をアニメーション化することができます。

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

スタック ビューのプロパティの多くは、ストーリー ボード内のサイズ クラスを使用して指定できます。 これらのプロパティは場合、自動的にサイズまたは向きの変更への応答は、アニメーション化します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計とビューの積み上げ Xamarin.tvOS アプリ内での操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
