---
title: セグメント化されたコントロールの操作
description: この記事では、設計と Xamarin.tvOS アプリ内でセグメント化されたコントロールの操作について説明します。
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d4eac932c7fad628a0a65127bceb641f34ea5d79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-segmented-controls"></a>セグメント化されたコントロールの操作

_この記事では、設計と Xamarin.tvOS アプリ内でセグメント化されたコントロールの操作について説明します。_


セグメント化されたコントロールは、それぞれのアイコンまたはテキストを含めることができ、ユーザーに関連する選択肢を提供するための線形の要素のセットを提供します。

[![](segmented-controls-images/segment01.png "セグメント コントロールのサンプル")](segmented-controls-images/segment01.png#lightbox)

Apple では、セグメント化されたコントロールの操作の次の方法があります。

- **十分な領域を用意**-注意は、十分な領域の間で提供されるまでにかかるをする必要があります[フォーカス可能な項目](~/ios/tvos/app-fundamentals/navigation-focus.md)およびセグメント化された制御します。 フォーカスが置かれてときではなくがクリックされた) あり、場合に、ユーザーが誤って変更セグメント実際には、現在のセグメントで別のフォーカス可能なアイテムを選択するときに、個々 のセグメントをオンになります。
- **分割ビューを使用してコンテンツ フィルター** -セグメント化されたコントロールのコンテンツのコンテンツと、フィルターの間で簡単に操作用に設計された分割ビューのフィルター処理用の適切な選択しないでください。
- **7 つのセグメントの最大数に制限**-(8) として 8 個の下のセグメントの最大数を保持しようとする必要がありますこの部屋の反対側のソファにかつ容易に移動する解析元に簡単です。
- **一貫性のあるセグメントのコンテンツ サイズを使用して**- すべてのセグメントを持つ同じ幅、可能であれば、しようとする必要があります、同じサイズの各セグメントにコンテンツを保持します。 これにより、セグメントのコントロールを視覚的に心地よいによりだけでなくが一目で読みやすくなります。
- **混在のアイコンとテキストを避けるため**-個々 の各セグメントは、アイコンとテキストの両方ではなくはどちらかを含めることができます。 アイコンと同じセグメント化されたコントロール内のテキストの両方を混在させることはできますが、これを回避する必要があります。

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>セグメントのアイコンの詳細

Apple では、セグメントのアイコン、検索の虫眼鏡などのシンプルで認識可能なイメージの使用を提案します。 過度に複雑なアイコンすることは難しい認識テレビ画面で、部屋のため、単純な形式にアイコンを制限することをお勧めします。

テキストと特定のセグメント上のアイコンの両方を混在させることはできませんし、アイコンと 1 つのセグメント化されたコントロール内のテキストの混合を避ける必要があります。 すべてのアイコンまたはすべてのテキストのいずれかになります。

<a name="Summary" />

## <a name="segment-text"></a>セグメントのテキスト

Apple では、セグメントのテキストを操作する次の提案を加えます。

- **Short、わかりやすい名詞を使用して**-、セグメントのタイトルが特定のセグメントを選択するときに期待されるコンテンツの種類を明確に記述する必要があります。 例: 音楽またはビデオです。
- **タイトル文字の大文字と小文字を使用して**-セグメントのタイトルのすべての単語は記事、接続詞前置詞より小さい 4 (4) 文字を除く大文字で入力する必要があります。
- **Short、フォーカスされたタイトルを使用して**-短いセグメントが選択されている場合に予想されるコンテンツの種類に焦点を当てており、タイトルを保持します。

再び、テキストと特定のセグメント上のアイコンの両方を混在させることはできませんし、アイコンと 1 つのセグメント化されたコントロール内のテキストの混合を避ける必要があります。

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>セグメントのコントロールとストーリー ボード

Xamarin.tvOS アプリでは、セグメントのコントロールを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**セグメント コントロール**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](segmented-controls-images/segment02.png "セグメントの制御")](segmented-controls-images/segment02.png#lightbox)
1. **ウィジェット タブ**の**プロパティ パッド**など、セグメント コントロールのいくつかのプロパティを調整することができます、**スタイル**と**状態**: 

    [![](segmented-controls-images/segment03.png "ウィジェット タブ")](segmented-controls-images/segment03.png#lightbox)
1. 使用して、**セグメント**コント ローラーのセグメントの数を制御するフィールドです。
1. 指定されたセグメントを選択、**セグメント ドロップダウン**など、その個々 のプロパティを調整する**タイトル**または**イメージ**とコントロールの場合は、指定されたセグメントに**有効になっている**または**選択**コントロールが表示される場合。
1. 最後に、割り当てる**名**コントロールに c# コードでそれらに応答できるようにします。 例えば: 

    [![](segmented-controls-images/segment04.png "名前を割り当てる")](segmented-controls-images/segment04.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**セグメント コントロール**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](segmented-controls-images/segment02-vs.png "セグメントの制御")](segmented-controls-images/segment02-vs.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**など、セグメント コントロールのいくつかのプロパティを調整することができます、**スタイル**と**状態**: 

    [![](segmented-controls-images/segment03-vs.png "ウィジェット タブ")](segmented-controls-images/segment03-vs.png#lightbox)
1. 使用して、**セグメント**コント ローラーのセグメントの数を制御するフィールドです。
1. 指定されたセグメントを選択、**セグメント ドロップダウン**など、その個々 のプロパティを調整する**タイトル**または**イメージ**とコントロールの場合は、指定されたセグメントに**有効になっている**または**選択**コントロールが表示される場合。
1. 最後に、割り当てる**名**コントロールに c# コードでそれらに応答できるようにします。 例えば: 

    [![](segmented-controls-images/segment04-vs.png "名前を割り当てる")](segmented-controls-images/segment04-vs.png#lightbox)
1. 変更内容を保存します。
    
-----

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>セグメント化されたコントロールの操作

前述のよう、s セグメント化されたコントロールには、線形の要素のセットが用意されていますそれぞれのアイコンまたはテキストを含めることができ、ユーザーに関連する選択肢を提供するために使用します。

セグメント化されたコントロールを使用するには、Xamarin.tvOS アプリでいくつかのさまざまな方法はあります。

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>名とイベントとして公開されています。

インターフェイス デザイナーで、セグメントのコントロールを作成し、という名前のコントロールとイベントとして公開する場合は、セグメントの変化に対応する次のコードを使用できます。

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

として公開されているセグメント コントロール上の例の場合、`PlayerCount`名前および`PlayerCountChanged`イベント アクション。 アクションおよびコンセントと操作の詳細についてを参照してください、[コンセントとアクションを使用してコードを記述](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code)のセクションで、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。

`SelectedSegment`プロパティを取得またはベースのゼロ (0) と現在選択されているセグメントのインデックスを設定します。 5 (5) セグメントがある場合は、最初のセグメントがゼロ (0) と最後のインデックスのインデックス、4 (4)。

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>セグメントの変更

いつでも数と、セグメント化されたコントロールのコンテンツの両方を変更することができます。 新しいアイコン セグメントを挿入するのにには、次のコードを使用します。

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

2 番目のパラメーターは、セグメントをする場所を定義、ゼロ (0) から始まるインデックスを使用して挿入します。 最後のパラメーターは場合`true`挿入がアニメーション化します。

特定のセグメントを削除するには次の手順に従います。

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

または、次のすべてのセグメントを削除する:

```csharp
SegmentedControl.RemoveAllSegments();
```

もう一度、最後のパラメーターは場合`true`削除がアニメーション化します。 使用して、`NumberOfSegments`プロパティを現在のセグメント数を返します。

取得する、**タイトル**または**アイコン**特定のセグメントを次を使用します。

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

変更して、**タイトル**または**アイコン**次の使用します。

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

指定されたセグメントが**有効**次の使用します。

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

および**有効/無効**セグメントを指定するには、次を使用します。

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>セグメント化されたコントロールの外観の変更

次のコードを使用して、イメージに指定されたセグメントの背景を変更することができます。

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

ここで`UIControlState`として画像を設定するコントロールの状態を指定します。

- 標準
- 強調表示されます。
- 無効
- 選択済み
- フォーカスされている

`UIBarMetrics`として使用する基準を指定します。

- 既定値
- Compact
- DefaultPrompt
- CompactPrompt

さらを使用してセグメント間の境界線を設定することができます。

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

ここで、最初`UIControlState`区分線と 2 番目の左側にセグメントの状態を指定`UIControlState`右側のセグメントの状態を指定します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でセグメント化されたコントロールの使い方について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
