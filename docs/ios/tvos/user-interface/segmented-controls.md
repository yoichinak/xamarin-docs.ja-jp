---
title: Xamarin で tvOS のセグメント化されたコントロールの操作
description: このドキュメントでは、Xamarin でビルドされたアプリでのセグメント化されたコントロールを tvOS を操作する方法について説明します。 セグメントのアイコンとテキスト、コントロールの外観の変更イベントがについて説明します。
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 98a770d05014e0498b805ed9ffa0c84314efc765
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61375037"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Xamarin で tvOS のセグメント化されたコントロールの操作

セグメント化されたコントロールでは、アイコンやテキストを含めることができ、ユーザーに関連する選択肢を提供するために使用の線形の要素のセットを提供します。

[![](segmented-controls-images/segment01.png "セグメントのコントロールのサンプル")](segmented-controls-images/segment01.png#lightbox)

Apple では、セグメント化されたコントロールを操作するための次の推奨事項があります。

- **十分な領域を確保**-ケアの間で十分な空き領域を受け取る必要があります[フォーカスを設定できる項目](~/ios/tvos/app-fundamentals/navigation-focus.md)とセグメント化されたコントロール。 (されませんがクリックされた) 場合、フォーカス設定は、実際には、現在のセグメントにフォーカスを設定できるもう 1 つの項目を選択するときに、ユーザーはセグメントを変更誤ってできるときに、個々 のセグメントが選択されます。
- **分割ビューを使用して、コンテンツ フィルターの**-セグメント付きコントロールがコンテンツのコンテンツと、フィルターの間に簡単に移動用に設計された分割ビューのフィルター処理の適切な選択肢を作成しません。
- **7 つのセグメントの最大数に制限**-(8) として以下の 8 つのセグメントの最大数を保持しようとする必要があります部屋のソファでかつ容易に移動しますから解析を容易になります。
- **一貫性のあるセグメントのコンテンツのサイズを使用して、** - すべてのセグメントを持つ同じ幅、可能であれば、しようとする必要があります、同じサイズの各セグメントにコンテンツを保持します。 これにより、セグメントのコントロールを視覚的に心地よいによりだけでなくが一目で読みやすくなります。
- **混在のアイコンとテキストを避けるため**-個々 の各セグメントは、アイコンやテキストの両方ではなくいずれかに含めることができます。 アイコンと同じセグメント化されたコントロール内のテキストの両方を混在させることはできますが、これは回避する必要があります。

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>セグメントのアイコンの詳細

Apple では、検索の虫眼鏡などのセグメントのアイコンのシンプルで認識可能なイメージの使用をお勧めします。 過度に複雑なアイコンは、単純な形式にアイコンを制限することをお勧めしているために、部屋のテレビ画面に気付きにくい。

テキストとアイコンを指定されたセグメントの両方を混ぜることはできませんし、1 つのセグメント化されたコントロールのアイコンやテキストの混在を回避する必要があります。 すべてのアイコンまたはすべてのテキストのいずれかが必要です。

<a name="Summary" />

## <a name="segment-text"></a>セグメントのテキスト

Apple では、セグメントのテキストを操作する次の推奨事項。

- **短く、わかりやすい名詞を使用して、** -、セグメントのタイトルは、特定のセグメントを選択するときに期待されるコンテンツの種類を明確に記述する必要があります。 例:音楽またはビデオ。
- **タイトル文字の大文字と小文字を使用して、** -記事、接続詞前置詞より小さい 4 つの文字を除くセグメントのタイトルのすべての単語を大文字にする必要があります。
- **使用して、簡単に言えば、フォーカスされたタイトル**-タイトル、短期保存とセグメントが選択されているときに想定されるコンテンツの種類に焦点を保持します。

ここでも、テキストとアイコンを指定されたセグメントの両方を混ぜることはできませんし、1 つのセグメント化されたコントロールのアイコンやテキストの混在を回避する必要があります。

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>セグメントのコントロールとストーリー ボード

Xamarin.tvOS アプリでのセグメントのコントロールを操作する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**セグメント コントロール**から、**ツールボックス**し、ビューにドロップします。 

    [![](segmented-controls-images/segment02.png "セグメントの制御")](segmented-controls-images/segment02.png#lightbox)
1. **ウィジェット タブ**の**プロパティ パッド**など、セグメントのコントロールのいくつかのプロパティを調整することができます、**スタイル**と**状態**: 

    [![](segmented-controls-images/segment03.png "[ウィジェット] タブ")](segmented-controls-images/segment03.png#lightbox)
1. 使用して、**セグメント**フィールドをコント ローラー内のセグメントの数を制御します。
1. 特定のセグメントを選択、**セグメント ドロップダウン**などの個々 のプロパティを調整する**タイトル**または**イメージ**場合は、特定のセグメントを制御する**有効になっている**または**選択**コントロールが表示されます。
1. 最後に、割り当てる**名**コントロール内に応答できるようにC#コード。 例: 

    [![](segmented-controls-images/segment04.png "名前を割り当てる")](segmented-controls-images/segment04.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**セグメント コントロール**から、**ツールボックス**し、ビューにドロップします。 

    [![](segmented-controls-images/segment02-vs.png "セグメントの制御")](segmented-controls-images/segment02-vs.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**など、セグメントのコントロールのいくつかのプロパティを調整することができます、**スタイル**と**状態**: 

    [![](segmented-controls-images/segment03-vs.png "[ウィジェット] タブ")](segmented-controls-images/segment03-vs.png#lightbox)
1. 使用して、**セグメント**フィールドをコント ローラー内のセグメントの数を制御します。
1. 特定のセグメントを選択、**セグメント ドロップダウン**などの個々 のプロパティを調整する**タイトル**または**イメージ**場合は、特定のセグメントを制御する**有効になっている**または**選択**コントロールが表示されます。
1. 最後に、割り当てる**名**コントロール内に応答できるようにC#コード。 例: 

    [![](segmented-controls-images/segment04-vs.png "名前を割り当てる")](segmented-controls-images/segment04-vs.png#lightbox)
1. 変更内容を保存します。
    
-----

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>セグメント化されたコントロールの操作

セグメント化されたコントロールには、線形の要素のセットが用意されています、s 前述のよう、それぞれのアイコンまたはテキストを含めることができ、ユーザーに関連する選択肢を提供するために使用します。

セグメント付きコントロールを使用するには、Xamarin.tvOS アプリでいくつかのさまざまな方法はあります。

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>名前とイベントとして公開

インターフェイス デザイナーで、セグメントの制御を作成し、という名前のコントロールとイベントとして、公開されている場合は、セグメントの変化に対応する次のコードを使用できます。

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

として公開されているセグメントのコントロール上の例の場合、`PlayerCount`名と`PlayerCountChanged`イベント アクション。 アクションとアウトレットの操作方法の詳細についてを参照してください、 [outlet と action を使用してコードを記述](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code)のセクション、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。

`SelectedSegment`プロパティを取得またはベースのゼロ (0) として現在選択されているセグメントのインデックスを設定します。 5 つのセグメントがある場合は、最初のセグメントがゼロ (0) と最後のインデックスのインデックス、4 (4)。

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>セグメントの変更

いつでも、数と、セグメント化されたコントロールのコンテンツの両方を変更できます。 セグメントの新しいアイコンを挿入するのにには、次のコードを使用します。

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

2 番目のパラメーターを定義、セグメントがされる、0 (0) から始まるインデックスを使用して挿入します。 最後のパラメーターは場合`true`挿入がアニメーション化します。

指定されたセグメントを削除するには、次の手順を使用します。

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

または、次のすべてのセグメントを削除する:

```csharp
SegmentedControl.RemoveAllSegments();
```

ここでも、最後のパラメーターがある場合`true`削除、アニメーション化します。 使用して、`NumberOfSegments`プロパティを現在のセグメント数を返します。

取得する、**タイトル**または**アイコン**指定されたセグメントに対して、次を使用します。

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

および**有効/無効**セグメントを指定するには、次を使用して。

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>セグメント化されたコントロールの外観を変更します。

イメージに指定されたセグメントの背景を変更するのには、次のコードを使用できます。

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

場所`UIControlState`として画像を設定するコントロールの状態を指定します。

- 標準
- 強調表示されています。
- 無効
- 選択済み
- フォーカスされている

`UIBarMetrics`として使用するメトリックを指定します。

- 既定値
- Compact
- DefaultPrompt
- CompactPrompt

さらを使用してセグメント間の境界線を設定できます。

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

最初の`UIControlState`区分線と、2 つ目の左のセグメントの状態を指定します`UIControlState`右側のセグメントの状態を指定します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と Xamarin.tvOS アプリ内でセグメント化されたコントロールの操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
