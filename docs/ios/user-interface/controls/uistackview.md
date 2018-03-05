---
title: "スタック ビュー"
description: "この記事では、水平方向または垂直方向に整列スタック内のいずれかでサブビューのセットを管理する Xamarin.iOS アプリで新しい UIStackView コントロールを使用してについて説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4555906512ecc36e3387f1b2483753e7f50a51ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="stack-view"></a>スタック ビュー

_この記事では、水平方向または垂直方向に整列スタック内のいずれかでサブビューのセットを管理する Xamarin.iOS アプリで新しい UIStackView コントロールを使用してについて説明します。_

> [!IMPORTANT]
> StackView iOS デザイナーではサポートされていますがときに、発生するユーザビリティ バグを安定したチャネルの使用に注意してください。 切り替え、ベータ版またはアルファ チャネルは、この問題を軽減する必要があります。 Xcode を使用して、必要な修正プログラムは、安定したチャネルに実装されてまでこのチュートリアルを提示することにしました。

スタック ビュー コントロール (`UIStackView`)、iOS デバイスの向きや画面のサイズを動的に応答する水平方向または垂直方向にサブビューのスタックを管理するには、自動レイアウトおよびサイズ クラスの機能を利用しています。

スタック ビューにアタッチされているすべてのサブビューのレイアウトは、軸、配布、調整間隔などの開発者が定義されているプロパティに基づいてによって管理されます。

[ ![](uistackview-images/stacked01.png "スタック ビュー レイアウトの図")](uistackview-images/stacked01.png)

使用する場合、 `UIStackView` Xamarin.iOS アプリで、開発者はかサブビュー iOS デザイナー、または削除して、追加のサブビュー c# コードでストーリー ボード内のいずれかを定義します。

このドキュメントは、2 つの部分で構成されています: に、最初の履歴を表示する実装され、いくつかの技術的詳細に関するそのしくみに役立つクイック スタート。

## <a name="uistackview-quickstart"></a>UIStackView クイック スタート

簡単な説明として、`UIStackView`コントロール、しようとして評価を 1 から 5 まで入力をユーザーに許可する単純なインターフェイスを作成します。 使用する 2 つのスタック ビュー: デバイスの画面とを画面全体で水平方向に 1 ~ 5 の評価アイコンを整列する 1 つの垂直方向にインターフェイスを配置する 1 つです。

### <a name="define-the-ui"></a>UI を定義します。

新しい Xamarin.iOS プロジェクトを開始し、編集、 **Main.storyboard** Xcode のインターフェイスのビルダー内のファイルです。 最初に、1 つをドラッグ**垂直スタック ビュー**上、**ビュー コント ローラー**:

[ ![](uistackview-images/quick01.png "ビュー コント ローラーの 1 つの垂直スタック ビューにドラッグします。")](uistackview-images/quick01.png)

**属性インスペクター**、次のオプションを設定します。

[ ![](uistackview-images/quick02.png "スタック ビューのオプションを設定します。")](uistackview-images/quick02.png)

この場合、

- **軸**– スタック ビューがかサブビューを整列するかどうかを判断**水平方向に**または**垂直方向に**です。
- **配置**– サブビュー スタック ビュー内で整列する方法を制御します。
- **配布**– サブビューにスタック ビュー内でサイズが設定する方法を制御します。
- **間隔**– スタック ビューでは、各サブビュー間の最小限の空白を制御します。
- **ベースライン相対**– 各サブビューの上下の間隔をそのベースラインから導き出されますオンにした場合。
- **レイアウトの余白相対**– サブビュー標準レイアウトの余白を基準に配置します。

スタック ビューを使用するときに考えることができます、**配置**として、 **X**と**Y**サブビューの場所と**配布**として、**高さ**と**幅**です。

> [!IMPORTANT]
> **注:** `UIStackView`ように設計されたの他のサブクラスと同様に、キャンバスに描画されていない非表示コンテナーのビューとして、そのため、`UIView`です。 などのプロパティの設定、`BackgroundColor`やオーバーライド`DrawRect`ビジュアル効果はありません。

次のように、ラベル、ImageView、2 つのボタンおよび水平方向のスタック ビューを追加することによって、アプリのインターフェイスをレイアウトし続けます。

[ ![](uistackview-images/quick03.png "スタック ビュー UI のレイアウト")](uistackview-images/quick03.png)

次のオプションを水平方向のスタック ビューを構成します。

[ ![](uistackview-images/quick04.png "水平方向のスタック ビューのオプションを構成します。")](uistackview-images/quick04.png)

サイズを拡大する評価を各「ポイント」を表すアイコンをたくないのでが加わった時点を水平方向のスタック ビューに設定しています、**配置**に**Center**と**配布**に**均等に塗りつぶし**です。

次の項目をネットワーク上での最後に、**コンセント**と**アクション**:

[ ![](uistackview-images/quick05.png "スタック ビュー コンセントとアクション")](uistackview-images/quick05.png)

### <a name="populate-a-uistackview-from-code"></a>コードから UIStackView への追加します。

Mac と編集の Visual Studio に戻り、 **ViewController.cs**ファイルし、次のコードを追加します。

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

### <a name="testing-the-ui"></a>UI のテスト

すべての必須の UI 要素やコードか所で、ここで実行し、インターフェイスをテストすることができます。 UI が表示されたら、垂直方向のスタック ビュー内の要素のすべてが等間隔に配置する上から下にします。

ユーザーがタップしたときに、**レーティングを上げる**ボタン、もう 1 つの「スター」が (最大 5)、画面に追加。

[ ![](uistackview-images/intro01.png "実行するサンプル アプリ")](uistackview-images/intro01.png)

「スター」は自動的に中央揃えし、水平方向のスタック ビューに均等に分散します。 ユーザーがタップしたときに、**レーティングを下げる**(none のままにする) まで、ボタン、「スター」を削除します。

## <a name="stack-view-details"></a>スタックの詳細の表示

新機能についての一般的な方針ができました、`UIStackView`コントロールは、しくみ、一部の機能と詳細情報について詳しく説明を見てみましょう。

### <a name="auto-layout-and-size-classes"></a>自動レイアウトおよびサイズ クラス

上で見たようサブビューが加わった時点をスタック ビュー レイアウト位置および配置のビューのサイズに自動レイアウトおよびサイズ クラスを使用してそのスタック ビューによって完全に制御されます。

スタック ビューは_pin_ためには、そのコレクション内の最初と最後のサブビュー、**上部**と**下部**スタック ビューを垂直方向のエッジまたは**を左**と**右**スタック ビューを水平方向のエッジ。 設定した場合、`LayoutMarginsRelativeArrangement`プロパティを`true`ビュー、エッジではなく、関連する余白にサブビューをピン留めし、します。

スタック ビューが使用されたサブビューの`IntrinsicContentSize`プロパティの定義に沿ってサブビュー サイズを計算するときに`Axis`(を除き、 `FillEqually Distribution`)。 `FillEqually Distribution`サブビューがすべてのサイズを変更して得られるよう、同じサイズに沿った履歴ビューを埋めるため、`Axis`です。

例外を除いて、 `Fill Alignment`、スタック ビューが使用されたサブビューの`IntrinsicContentSize`に垂直なビューのサイズを計算するためのプロパティ、指定された`Axis`です。 `Fill Alignment`に垂直スタック ビューを入力するようにサブビューがすべてのサイズは、指定された`Axis`です。

### <a name="positioning-and-sizing-the-stack-view"></a>配置やスタック ビューをサイズ変更

スタック ビューにサブビューのレイアウトを完全に制御があるときに (などのプロパティに基づく`Axis`と`Distribution`)、スタック ビューを配置する必要があります (`UIStackView`) 自動レイアウトおよびサイズのクラスを使用して親ビュー内です。

一般に、つまり、スタック ビューを展開し、コントラクト、ため、その位置を定義するのには、少なくとも 2 つのエッジをピン留めします。 せず、追加の制約は、すべてを収めるそのサブビューの次のように、スタック ビューをサイズ変更自動的には。

 - に沿ってサイズ、`Axis`サブビューのすべてのサイズと各サブビュー間で定義されているすべての領域の合計になります。
 - 場合、`LayoutMarginsRelativeArrangement`プロパティは`true`、スタック ビュー サイズには、余白の領域も含まれます。
 - サイズを垂直、`Axis`コレクション内で最大サブビューに設定されます。

スタック ビューの制約を指定するさらに、**高さ**と**幅**です。 この場合、サブビューをレイアウトする (規模) によって決定されるスタック ビューで指定された領域を埋める、`Distribution`と`Alignment`プロパティです。

場合、`BaselineRelativeArrangement`プロパティは`true`、サブビューをレイアウトするを使用せずに、最初または最サブビューの基準に基づいて、**上部**、**下部**または **Center*-  **Y**位置。 スタック ビューのコンテンツに対するよう計算これらされます。

 - 垂直スタック ビューは、最初の基準の最初のサブビューと最後の最後に戻ります。 スタック ビュー自体これらサブビューのいずれかの場合は、最初と最後のベースラインが使用されます。
 - 水平方向のスタック ビュー最初と最後の両方の基準に、最も高いサブビューが使用されます。 最も高いビューは、スタック ビューではまた場合、は、最も高いサブビューをベースラインとして使用します。

> [!IMPORTANT]
> **注:**ベースラインの配置では、間違った位置に、基準が計算されますに、拡張または圧縮されたサブビュー サイズで動作しません。 ベースライン配置のことを確認サブビューの**高さ**組み込みのコンテンツ ビューと一致する**高さ**です。

### <a name="common-stack-view-uses"></a>スタック ビューの一般的な使用方法

スタック ビュー コントロールでうまく機能をいくつかのレイアウトの種類があります。 Apple にに従っていくつかの一般的な使用方法を次に示します。

- **に沿って、軸のサイズを定義**– に沿ってスタック ビューの両方の端をピン留めする`Axis`と 1 つの隣接する端をそのサブビューによって定義された領域に合わせて軸に沿って表示が大きくなり、スタックの位置を設定します。
 -  **サブビューの位置を定義**– 両方のディメンションを含むサブビューに合わせて親ビューにスタック ビューの隣接する端に固定、スタック ビューが拡大します。
- **スタックの位置とサイズを定義する**– 親ビューにスタック ビューのすべての 4 つのエッジをピン留めするスタック ビューがスタック ビュー内で定義されている領域に基づいたサブビューを整列します。
 -  **サイズの垂直軸を定義する**– エッジに対して垂直スタック ビューの両方をピン留めする`Axis`ビューがによって定義された領域に合わせて軸と垂直に大きくなり、スタックの位置を設定する軸に沿ったのエッジのいずれかとそのサブビューです。

### <a name="managing-the-appearance"></a>外観を管理します。

`UIStackView`ように設計された他のサブクラスのようなキャンバスに描画されていない非表示コンテナーのビューとして、そのため、`UIView`です。 などのプロパティを設定`BackgroundColor`やオーバーライド`DrawRect`ビジュアル効果はありません。

スタック ビューがそのサブビューのコレクションを配置する方法を制御するいくつかのプロパティがあります。

- **軸**– スタック ビューがかサブビューを整列するかどうかを判断**水平方向に**または**垂直方向に**です。
- **配置**– サブビュー スタック ビュー内で整列する方法を制御します。
- **配布**– サブビューにスタック ビュー内でサイズが設定する方法を制御します。
- **間隔**– スタック ビューでは、各サブビュー間の最小限の空白を制御します。
- **ベースライン相対**– `true`、各サブビューの上下の間隔をそのベースラインから導き出されます。
- **レイアウトの余白相対**– サブビュー標準レイアウトの余白を基準に配置します。

通常はサブビューの数が少ないを配置するのにスタック ビューを使用します。 互いに内部の 1 つ以上のスタック ビューを入れ子にして複雑なユーザー インターフェイスを作成することができます (行ったように、 [UIStackView クイック スタート](#UIStackView-Quickstart)上)。

サブビュー (たとえばにコントロールの高さまたは幅) に追加の制約を追加することで、Ui の外観をさらに細かく調整できます。 ただし、注意が必要自体スタック ビューで導入されたものに、競合する制約を含める必要はありません。

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>ビューおよび Sub ビューの整合性を維持配置

スタック表示することでその`ArrangedSubviews`プロパティは、のサブセットでは常にその`Subviews`次の規則を使用してプロパティ。

 - サブビューが追加された場合、 `ArrangedSubviews` 、コレクションに自動的に追加されます、`Subviews`コレクションは既にそのコレクションの一部)。
 - サブビューはから削除された場合、 `Subviews` (表示から削除)、コレクションからも削除は、`ArrangedSubviews`コレクション。
 - 削除からサブビュー、`ArrangedSubviews`からあるコレクションは削除されません、`Subviews`コレクション。 これは不要になったレイアウト スタック ビューによってが、画面に表示されます。

`ArrangedSubviews`コレクションは、のサブセットでは常に、`Subview`コレクション、独立した、次のように管理された各コレクション内で個々 のサブビューの順序は、ただし。

 - 内でサブビューの順序、`ArrangedSubviews`コレクションは、スタック内のそれらの表示順序を決定します。
 - 内でサブビューの順序、`Subview`コレクション ビュー内で、前後の Z オーダー (または重ね順) を決定します。

### <a name="dynamically-changing-content"></a>コンテンツを動的に変更します。

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

## <a name="summary"></a>まとめ

この記事は、新しいカバーされて`UIStackView`サブビュー Xamarin.iOS アプリではスタックが水平方向または垂直方向の配置内のセットを管理する (iOS 9) を制御します。
スタック ビューを使用して、UI を作成する簡単な例で始まるし、スタック ビューとそのプロパティおよび機能の詳細について作業を完了します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [新機能 iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [紹介 UIStackView (ビデオ)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
