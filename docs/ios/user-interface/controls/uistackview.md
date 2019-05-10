---
title: Xamarin.iOS でスタック ビュー
description: この記事では、水平方向または垂直方向に配置後のスタックのいずれかでサブビューのセットを管理する Xamarin.iOS アプリで新しい UIStackView コントロールの使用について説明します。
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e9c18920386cb58f152d7631c52240b4b5b72ff9
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977845"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin.iOS でスタック ビュー

_この記事では、水平方向または垂直方向に配置後のスタックのいずれかでサブビューのセットを管理する Xamarin.iOS アプリで新しい UIStackView コントロールの使用について説明します。_

> [!IMPORTANT]
> StackView iOS Designer ではサポートされていますが、発生するユーザビリティ バグ、安定チャネルを使用する場合に注意してください。 ベータ版またはアルファ チャネルを切り替えると、この問題を軽減する必要があります。 Xcode を使用するために必要な修正プログラムは、安定チャネルで実装されるまで、このチュートリアルを提示することにしました。

スタック ビュー コントロール (`UIStackView`) iOS デバイスの向きや画面サイズに動的に応答する水平方向または垂直方向に、サブビューのスタックを管理するには、自動レイアウトとサイズ クラスの機能を利用しています。

スタック ビューにアタッチされているすべてのサブビューのレイアウトは、軸、配布、配置、および間隔などの開発者は定義されたプロパティに基づいて、管理されます。

[![](uistackview-images/stacked01.png "スタック ビュー レイアウトの図")](uistackview-images/stacked01.png#lightbox)

使用する場合、 `UIStackView` Xamarin.iOS アプリケーションでは、開発者はいずれかで iOS Designer、または追加し、c# コードでサブビューを削除するには、ストーリー ボード内のいずれかのサブビューを定義します。

このドキュメントは、2 つの部分で構成されています。 最初のスタックを表示する実装し、その動作についていくつか技術的な詳細から簡単に開始します。

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView ビデオ**

## <a name="uistackview-quickstart"></a>UIStackView クイック スタート

概要として、`UIStackView`コントロール、ここ 1 から 5 までの評価の入力をユーザーに許可する単純なインターフェイスを作成します。 使用する 2 つのスタック ビュー: デバイスの画面、画面全体で水平方向に 1 ~ 5 の評価アイコンの配置に 1 つの垂直方向にインターフェイスを配置する 1 つ。

### <a name="define-the-ui"></a>UI を定義します。

新しい Xamarin.iOS プロジェクトを開始し、編集、 **Main.storyboard** Xcode の Interface Builder でのファイル。 最初に、1 つをドラッグ**垂直スタック ビュー**上、**ビュー コント ローラー**:

[![](uistackview-images/quick01.png "ビュー コント ローラーの 1 つの垂直スタック ビューをドラッグします。")](uistackview-images/quick01.png#lightbox)

**属性インスペクター**、次のオプションを設定します。

[![](uistackview-images/quick02.png "スタック ビューのオプションを設定します。")](uistackview-images/quick02.png#lightbox)

この場合、

- **軸**– スタック ビューが、サブビューを配置するかどうかを決定します**水平方向に**または**垂直方向に**します。
- **配置**– スタック ビュー内で、サブビューを配置する方法を制御します。
- **配布**– サブビューのスタック ビュー内でサイズはどのように制御します。
- **間隔**– スタック ビューには、各サブビューの間の最小限のスペースを制御します。
- **ベースライン相対**– 各サブビューの上下の間隔をそのベースラインから派生はオンにした場合。
- **レイアウトの余白相対**– 標準的なレイアウトの余白を基準とした、サブビューを配置します。

スタック ビューを使用する場合の考えることができます、**配置**として、 **X**と**Y**サブビューの場所と**配布**として、**高さ**と**幅**します。

> [!IMPORTANT]
> `UIStackView` いますの他のサブクラスのようにキャンバスに描画されていない非表示コンテナー ビューとして、そのため、`UIView`します。 などのプロパティの設定、`BackgroundColor`オーバーライドまたは`DrawRect`視覚効果はありません。

次のように、ラベル、ImageView、2 つのボタンおよび水平方向のスタック ビューを追加することで、アプリのインターフェイスをレイアウトに進みます。

[![](uistackview-images/quick03.png "スタック ビューの UI のレイアウト")](uistackview-images/quick03.png#lightbox)

次のオプションを使用して、水平方向のスタック ビューを構成します。

[![](uistackview-images/quick04.png "水平方向のスタックの表示オプションを構成します。")](uistackview-images/quick04.png#lightbox)

拡張の対象としてなしの評価"point"を表すアイコンをたくないので追加ときに、水平方向のスタック ビューに設定しています、**配置**に**Center** 、 **配布**に**均等に塗りつぶし**します。

次の項目を最後に、ワイヤ**Outlet**と**アクション**:

[![](uistackview-images/quick05.png "スタック ビュー Outlet と Action")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>コードからの UIStackView を設定します。

Mac および編集用の Visual Studio に戻り、 **ViewController.cs**ファイルを開き、次のコードを追加します。

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

### <a name="testing-the-ui"></a>UI をテスト

すべての必要な UI 要素とコードで今すぐ実行し、インターフェイスをテストできます。 UI が表示されたら、垂直方向のスタック ビュー内の要素のすべてが等間隔に配置する上から下にします。

ユーザーがタップしたときに、**レーティングを上げる**ボタン、もう 1 つの"star"が (最大 5)、画面に追加されます。

[![](uistackview-images/intro01.png "サンプル アプリの実行")](uistackview-images/intro01.png#lightbox)

「星印」は自動的に中央揃えし、スタック ビューを水平方向に均等に分散します。 ユーザーがタップしたときに、**レーティングを下げる**(none が残されます) まで、ボタン、"star"は削除されます。

## <a name="stack-view-details"></a>スタックの詳細を表示

どのような一般的な考えが作成できた、`UIStackView`コントロールし、どのように機能は、その機能と詳細の一部について詳しく説明を見てみましょう。

### <a name="auto-layout-and-size-classes"></a>自動レイアウトとサイズ クラス

上記のとおり、サブビューが追加されたときにスタック ビュー レイアウトは、完全に自動レイアウトとサイズ クラスの位置し、サイズ、配置後のビューを使用してそのスタック ビューによって制御されます。

スタック ビューは_pin_をそのコレクション内の最初と最後のサブビュー、**上部**と**下部**スタック ビューを垂直方向の端または**を左**と**右**スタック ビューを水平方向のエッジ。 設定した場合、`LayoutMarginsRelativeArrangement`プロパティを`true`ビュー、サブビュー、edge の代わりに関連する余白をピン留めします。

スタック ビューで使用する、サブビューの`IntrinsicContentSize`プロパティの定義に沿ってサブビューのサイズを計算するときに`Axis`(を除き、 `FillEqually Distribution`)。 `FillEqually Distribution`されるので、同じサイズでは、すべてのサブビューのサイズを変更したがってに沿った履歴ビューを入力、`Axis`します。

例外として、 `Fill Alignment`、スタック ビューで使用する、サブビューの`IntrinsicContentSize`に垂直ビューのサイズを計算するためのプロパティ、指定された`Axis`。 `Fill Alignment`、に対して垂直スタック ビューを入力するようにすべてのサブビューのサイズは、指定された`Axis`します。

### <a name="positioning-and-sizing-the-stack-view"></a>位置およびサイズ スタック ビュー

スタック ビューにサブビューのレイアウトを完全に制御があるときに (などのプロパティに基づいた`Axis`と`Distribution`)、スタックのビューを配置する必要があります (`UIStackView`) 自動レイアウトとサイズ クラスを使用して親ビュー内で。

つまり、通常の拡大/縮小、その位置を定義するためにスタック ビューに少なくとも 2 つの端をピン留めします。 せず、追加の制約は、サブビューのすべてに適合するように、スタック ビューをサイズ変更に自動的には。

 - に沿ってサイズ、`Axis`サブビューのすべてのサイズと各サブビューの間で定義されているすべての領域の合計になります。
 - 場合、`LayoutMarginsRelativeArrangement`プロパティは`true`、スタック ビュー サイズには、余白の領域も含まれます。
 - サイズを垂直、`Axis`はコレクション内の最大のサブビューに設定されます。

スタック ビューの制約を指定するさらに、**高さ**と**幅**します。 この場合は、サブビューをレイアウトする (サイズ) によって決定される履歴ビューで指定された領域を埋める、`Distribution`と`Alignment`プロパティ。

場合、`BaselineRelativeArrangement`プロパティは`true`、サブビューをレイアウトするを使用してではなく、最初と最後のサブビューの基準に基づいて、**上部**、**下部**または**Center**-  **Y**位置。 スタック ビューのコンテンツに次のように計算これらされます。

 - 最初の基準の最初のサブビューと最後の最後には、垂直方向のスタック ビューを返します。 これらのサブビューのいずれかが、スタック ビュー自体が場合は、その初または最後の基準が使用されます。
 - 水平方向のスタック ビュー最初と最後の両方の基準に、最も高いサブビューが使用されます。 Tallest ビューも履歴ビューである場合は、tallest サブビューをベースラインとして使用します。

> [!IMPORTANT]
> ベースライン配置では動作しませんサブビューを拡張または圧縮サイズとして、間違った位置に、基準が計算されます。 ベースラインの配置、ようにサブビューの**高さ**組み込みのコンテンツ ビューと一致する**高さ**します。

### <a name="common-stack-view-uses"></a>スタック ビューの一般的な使用

スタック ビュー コントロールと連動するいくつかのレイアウトの種類があります。 に従って、Apple は、次にいくつかの一般的な使用方法を示します。

- **定義、軸に沿ったサイズ**– 両方の端に沿ってスタック ビューのピン留めして`Axis`サブビューによって定義された領域に合わせて軸に沿ったビューのサイズが増加するスタックの位置を設定する隣接辺の 1 つ。
 -  **サブビューの位置を定義**– に合わせて格納サブビューの寸法の両方で、親ビューにスタック ビューの隣接辺に固定してスタック ビューが拡張されます。
- **スタックの位置とサイズを定義する**– スタック ビューを親ビューの履歴ビューのすべての 4 辺をピン留めするには、スタック ビュー内で定義される領域に応じてサブビューを配置します。
 -  **サイズの垂直軸を定義**– エッジに対して垂直スタック ビューの両方をピン留めして`Axis`1 つのビューがによって定義された領域に合わせて axis に対して垂直な拡張スタックの位置を設定する軸に沿った端とそのサブビューします。

### <a name="managing-the-appearance"></a>外観を管理します。

`UIStackView`いますの他のサブクラスのようにキャンバスに描画されていない非表示コンテナー ビューとして、そのため、`UIView`します。 などのプロパティを設定`BackgroundColor`オーバーライドまたは`DrawRect`視覚効果はありません。

スタック ビューがそのサブビューのコレクションを配置する方法を制御するいくつかのプロパティがあります。

- **軸**– スタック ビューが、サブビューを配置するかどうかを決定します**水平方向に**または**垂直方向に**します。
- **配置**– スタック ビュー内で、サブビューを配置する方法を制御します。
- **配布**– サブビューのスタック ビュー内でサイズはどのように制御します。
- **間隔**– スタック ビューには、各サブビューの間の最小限のスペースを制御します。
- **ベースライン相対**– `true`、各サブビューの上下の間隔をそのベースラインから導き出されます。
- **レイアウトの余白相対**– 標準的なレイアウトの余白を基準とした、サブビューを配置します。

一般的にはスタック ビューは、サブビューの数が少ないを配置するために使用します。 1 つ以上のスタック ビューどうしを入れ子にしてより複雑なユーザー インターフェイスを作成できます (で行ったよう、 [UIStackView クイック スタート](#uistackview-quickstart)上)。

(たとえば、コントロールの高さまたは幅) にサブビューを追加の制約を追加することで、Ui の外観をさらに微調整できます。 ただし、注意してください自体スタック ビューで導入されたものに競合する制約を含める必要はありません。

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>ビューとビューの整合性をサブの配置を維持

スタック ビューによりその`ArrangedSubviews`プロパティは、のサブセットでは常にその`Subviews`次の規則を使用してプロパティ。

 - サブビューに追加された場合、`ArrangedSubviews`コレクション、それが自動的に追加されます、`Subviews`コレクション (ない場合は既にそのコレクションの一部)。
 - サブビューはから削除する場合、 `Subviews` (表示から削除)、コレクションからも外されます、`ArrangedSubviews`コレクション。
 - サブビューの削除、`ArrangedSubviews`からあるコレクションは削除されません、`Subviews`コレクション。 そのため、スタック ビューをレイアウトする不要になったが、画面に表示されます。

`ArrangedSubviews`コレクションがのサブセットでは常に、`Subview`コレクション、しかし各コレクション内の個々 のサブビューの順序は別であり、次によって制御されます。

 - 内のサブビューの順序、`ArrangedSubviews`コレクションは、スタック内には、その表示順序を決定します。
 - 内のサブビューの順序、`Subview`コレクションが前面にビュー内で、Z オーダー (またはレイヤー) を決定します。

### <a name="dynamically-changing-content"></a>コンテンツを動的に変更します。

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

## <a name="summary"></a>まとめ

この記事では、新しいについて説明しました`UIStackView`Xamarin.iOS アプリで水平方向または垂直方向に配置後のスタックにサブビューのセットを管理する (iOS 9) を制御します。
スタック ビューを使用して、UI を作成する簡単な例を開始し、スタック ビューとそのプロパティおよび機能の詳細を見て完了します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 を新します。](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [UIStackView (ビデオ) の概要](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
