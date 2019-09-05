---
title: Xamarin での tvOS セグメント化されるコントロールの操作
description: このドキュメントでは、Xamarin でビルドされたアプリで tvOS セグメント化されたコントロールを操作する方法について説明します。 ここでは、セグメントのアイコンとテキスト、イベント、コントロールの外観の変更などについて説明します。
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 9ca931058f15c2ff6f1b46810196d9ea2bb964e0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291393"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Xamarin での tvOS セグメント化されるコントロールの操作

セグメント化されたコントロールは、一連の線形要素を提供します。各要素にはアイコンまたはテキストを含めることができ、関連する一連の選択肢をユーザーに提供するために使用されます。

[![](segmented-controls-images/segment01.png "サンプルセグメントコントロール")](segmented-controls-images/segment01.png#lightbox)

次に、セグメント化されたコントロールの操作に関する推奨事項を示します。

- 他のフォーカス可能な[項目](~/ios/tvos/app-fundamentals/navigation-focus.md)とセグメント化されたコントロールの間に十分な領域を確保するには、**十分な領域**を確保する必要があります。 個々のセグメントは、フォーカスがあるときに選択され (クリックされたときではなく)、ユーザーが現在のセグメントでフォーカスできる別の項目を実際に選択するときに、誤ってセグメントを変更することができます。
- **コンテンツのフィルター処理用に分割ビューを使用する**-セグメント化されたコントロールでは、コンテンツのフィルター処理に適した選択が行われません。分割ビューは、コンテンツとフィルターを簡単に移動できるように設計されています。
- 最大**7 つのセグメント**の最大数に制限します。これは、ソファの部屋から見やすく、移動しやすくなるため、セグメントの最大数を 8 (8) 以下に保つようにしてください。
- **一貫したセグメントコンテンツサイズの使用**-すべてのセグメントの幅が同じで、可能であれば、各セグメントのコンテンツを同じサイズに保つようにしてください。 これにより、セグメントコントロールが視覚的に美しくなるだけでなく、ひとめで読みやすくなります。
- **アイコンとテキストが混在しないよう**にする-各セグメントにはアイコンまたはテキストを含めることができますが、両方を含めることはできません。 同じセグメント化されたコントロールにアイコンとテキストの両方を混在させることができますが、これは避けてください。

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>セグメントアイコンについて

Apple では、検索のための虫眼鏡など、セグメントアイコンにわかりやすいイメージを使用することを提案しています。 非常に複雑なアイコンは、部屋のテレビ画面で認識するのは困難です。そのため、アイコンを単純な表現に限定することをお勧めします。

特定のセグメントにテキストとアイコンの両方を混在させることはできません。また、1つのセグメント化されたコントロールにアイコンとテキストが混在しないようにする必要があります。 すべてのアイコンまたはすべてのテキストを指定する必要があります。

<a name="Summary" />

## <a name="segment-text"></a>セグメントテキスト

Apple では、セグメントテキストを操作するために次の提案を行います。

- **わかりやすい、意味**のある名詞を使用する-セグメントタイトルは、指定されたセグメントを選択するときにユーザーが期待するコンテンツの種類を明確に示す必要があります。 例えば:音楽またはビデオ。
- **タイトル**の大文字と小文字を区別する-セグメントタイトルのすべての単語は、記事、接続詞、および前置詞の4文字未満の部分を除いて大文字にする必要があります。
- **短いタイトルのタイトルを使用**します。タイトルは短くし、セグメントを選択したときに想定されるコンテンツの種類に重点を置いています。

ここでも、特定のセグメントにテキストとアイコンの両方を混在させることはできません。また、1つのセグメント化されたコントロールにアイコンとテキストが混在しないようにする必要があります。

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>セグメントコントロールとストーリーボード

TvOS アプリでセグメントコントロールを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、 `Main.storyboard`ファイルをダブルクリックして開き、編集します。
1. **[ツールボックス]** から**セグメントコントロール**をドラッグし、ビューにドロップします。 

    [![](segmented-controls-images/segment02.png "セグメントコントロール")](segmented-controls-images/segment02.png#lightbox)
1. **プロパティパッド**の [**ウィジェット] タブ**では、**スタイル**や**状態**など、セグメントコントロールのいくつかのプロパティを調整できます。 

    [![](segmented-controls-images/segment03.png "[ウィジェット] タブ")](segmented-controls-images/segment03.png#lightbox)
1. **[セグメント]** フィールドを使用して、コントローラー内のセグメントの数を制御します。
1. [**セグメント] ドロップダウン**から特定のセグメントを選択し、 **[タイトル]** や **[画像]** などの個々のプロパティを調整して、コントロールの表示時に特定のセグメントが有効に**なって**いるか**選択さ**れているかを制御します。
1. 最後に、コントロールに**名前**を割り当てて、コードでC#それらに応答できるようにします。 例えば: 

    [![](segmented-controls-images/segment04.png "名前を割り当てる")](segmented-controls-images/segment04.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックして開き、編集します。
1. **[ツールボックス]** から**セグメントコントロール**をドラッグし、ビューにドロップします。 

    [![](segmented-controls-images/segment02-vs.png "セグメントコントロール")](segmented-controls-images/segment02-vs.png#lightbox)
1. **プロパティエクスプローラー**の [**ウィジェット] タブ**では、 **[スタイル]** や **[状態]** など、セグメントコントロールのいくつかのプロパティを調整できます。 

    [![](segmented-controls-images/segment03-vs.png "[ウィジェット] タブ")](segmented-controls-images/segment03-vs.png#lightbox)
1. **[セグメント]** フィールドを使用して、コントローラー内のセグメントの数を制御します。
1. [**セグメント] ドロップダウン**から特定のセグメントを選択し、 **[タイトル]** や **[画像]** などの個々のプロパティを調整して、コントロールの表示時に特定のセグメントが有効に**なって**いるか**選択さ**れているかを制御します。
1. 最後に、コントロールに**名前**を割り当てて、コードでC#それらに応答できるようにします。 例えば: 

    [![](segmented-controls-images/segment04-vs.png "名前を割り当てる")](segmented-controls-images/segment04-vs.png#lightbox)
1. 変更内容を保存します。

-----

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>セグメント化されるコントロールの操作

前述のように、のセグメント化されたコントロールには、一連の線形要素が用意されています。各要素にはアイコンまたはテキストを含めることができ、関連する一連の選択肢をユーザーに提供するために使用されます。

TvOS アプリでセグメント化されたコントロールを操作するには、いくつかの方法があります。

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>名前とイベントとして公開

インターフェイスデザイナーでセグメントコントロールを作成し、名前付きコントロールとイベントとして公開した場合は、次のコードを使用して、変更するセグメントに応答できます。

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

上の例の場合、セグメントコントロールは`PlayerCount`名前`PlayerCountChanged`とイベントアクションとして公開されていました。 アクションとアウトレットの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」の「[アウトレットとアクションを使用したコードの記述](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code)」を参照してください。

プロパティ`SelectedSegment`は、現在選択されているセグメントを0から始まるインデックスとして取得または設定します。 したがって、5つのセグメントがある場合、最初のセグメントのインデックスはゼロ (0) で、最後のインデックスは 4 (4) になります。

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>セグメントの変更

セグメント化されたコントロールの数とコンテンツの両方をいつでも変更できます。 次のコードを使用して、新しいアイコンセグメントを挿入します。

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

2番目のパラメーターは、0から始まるインデックスを使用して、セグメントが挿入される場所を定義します。 最後のパラメーターが`true`の場合、挿入はアニメーション化されます。

特定のセグメントを削除するには、次のように指定します。

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

すべてのセグメントを削除するには、次のようにします。

```csharp
SegmentedControl.RemoveAllSegments();
```

ここでも、最後のパラメーター `true`がの場合、削除はアニメーション化されます。 現在の`NumberOfSegments`セグメント数を取得するには、プロパティを使用します。

特定のセグメントの**タイトル**または**アイコン**を取得するには、次のように指定します。

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

**タイトル**または**アイコン**を変更するには、次のようにします。

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

特定のセグメントが**有効になっ**ているかどうかを確認するには、次のようにします。

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

また、特定のセグメントを**有効または無効**にするには、次のようにします。

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>セグメント化されたコントロールの外観の変更

次のコードを使用して、特定のセグメントの背景をイメージに変更できます。

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

ここ`UIControlState`では、イメージを設定するコントロールの状態をとして指定します。

- 標準
- '95'5c
- Disabled
- 選択済み
- フォーカスされている

と`UIBarMetrics`は、次のように使用するメトリックを指定します。

- 既定値
- コンパクト
- DefaultPrompt
- CompactPrompt

また、次を使用してセグメント間に区分線を設定することもできます。

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

最初`UIControlState`のは、区分線の左側にあるセグメントの状態を指定し、2 `UIControlState`番目のは右側のセグメントの状態を指定します。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS アプリ内のセグメント化されたコントロールの設計と操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
