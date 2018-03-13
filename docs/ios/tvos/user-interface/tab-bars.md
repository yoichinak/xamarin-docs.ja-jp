---
title: "タブ バー コント ローラーの使用"
description: "この記事では、設計と Xamarin.tvOS アプリ内でタブ バーのコント ローラーの操作について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fc9765b3f6a77f47fdce32dbc3805dd7bd70a08a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-tab-bar-controller"></a>タブ バー コント ローラーの使用

_この記事では、設計と Xamarin.tvOS アプリ内でタブ バーのコント ローラーの操作について説明します。_

多くの種類の tvOS アプリでは、プライマリ ナビゲーション、画面の一番上に実行されているタブ バーとして表示されます。 ユーザーは、可能なカテゴリと、ユーザーの選択を反映するように、コンテンツの下の領域の変更の一覧で、左と右を読み取ります。

[![](tab-bars-images/tab01.png "サンプル タブ バー")](tab-bars-images/tab01.png#lightbox)

タブ バーでは、既定では半透明し、常に、画面の上部に表示されます。 、フォーカスがあるときタブ バーは画面の最上位の 140 ピクセルを取り上げますはすばやくを移すと休暇から次のコンテンツ エリアにフォーカスを移動します。

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>TvOS のタブ バー

`UITabViewController`同様の方法で動作を次の主な違いと、iOS で行われるように、tvOS のと同様の目的を担うとします。

- 異なり、画面の下部に表示される iOS のタブ バー、tvOS のタブ バー位置を埋めるため、画面の最上位の 140 ピクセルは既定半透明です。
- コンテンツ領域の下のタブ バーがフォーカスをタブ バーは画面の上端からスライドはすばやくを表示されません。 ユーザーのメニュー ボタンを 1 回タップまたはで上方向にスワイプできる、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)タブ バーを再度表示します。
- Siri リモート ダウン スワイプにフォーカスを移動、コンテンツの下の領域、最初のタブ バー[フォーカス可能な項目](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection)表示されている内容にします。 もう一度、タブ バーを非表示フォーカスを移動したらこれされます。
- クリックしてタブ バーに表示されるカテゴリを選択するカテゴリのコンテンツとフォーカスに切り替わります最初のフォーカス可能な項目のビューに切り替わります。
- タブ バーに表示されるカテゴリの数を固定する必要がありますとすべてのカテゴリは、常にアクセスする必要があります、1 つのカテゴリを無効にすることはありません。
- タブ バーは、tvOS のカスタマイズをサポートしていません。 さらに、それらを表示しない、**詳細**(iOS) などのカテゴリではより多くのカテゴリがある場合、タブ バーに収まることができます。

Apple では、タブ バーを操作するための以下の推奨事項があります。

- **使用してタブ バーを表示するコンテンツを論理的に編成**-タブ バーを使用して、tvOS アプリに使用するコンテンツを論理的に編成します。 たとえば、おすすめ、上部のグラフ、Purchased および検索です。
- **ユーザーの新しいコンテンツに通知するバッジを追加**-カテゴリの新しいコンテンツのユーザーに通知するバッジ (赤、白の数字または感嘆符で oval) を必要に応じて表示できます。
- **多用バッジ**- しないバッジとタブ バーの煩雑化、およびのみ常に提供される重要な情報をユーザーにそれらを表示します。
- **カテゴリの数を制限する**- を複雑さを軽減し、アプリを続行して管理しやすい、しないオーバー ロードのカテゴリで、タブ バーおよびすべてのカテゴリが表示されないいっぱいになったことを確認してください。 単純で短いタイトルが最適です。
- **カテゴリを無効にしない**-すべてのタブ (カテゴリ) は常に表示して有効に常にします。 指定したタブには、コンテンツがなければ、説明については、ユーザーに提供理由。 たとえば、購入 タブは、ユーザーに購入記録が行われなかった場合は空になります。

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>タブ バー項目

タブ バー項目によって表されるタブ バー内の各カテゴリ (タブ) (`UITabBarItem`)。 Apple では、タブ バー アイテムの操作の次の方法があります。

- **テキスト ベースのタブを使用して**-Apple 提案中に、タブ バーの項目がアイコンとして表現されていること、簡潔なタイトルはアイコンよりを解釈しやすくためにのみにテキストを使用します。
- **Short、わかりやすい名詞または動詞を使用して**-A タブ バー項目が含まれており、(写真、ビデオや音楽) などの単純な名詞または (検索や Play) などの動詞である場合に適しているコンテンツを中継明確にする必要があります。

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>タブ バーとストーリー ボード

Xamarin.tvOS アプリでは、タブ バーを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. 新しい Xamarin.tvOS アプリを起動し、選択**tvOS** > **アプリ** > **タブ付きのアプリ**: 

    [![](tab-bars-images/tab02.png "タブ付きのアプリを選択します")](tab-bars-images/tab02.png#lightbox)
1. すべての新しい Xamarin.tvOS ソリューションを作成する画面の指示に従います。
1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. 変更する、**アイコン**または**タイトル**1 つのカテゴリを選択、**タブ バー項目**の**ビュー コント ローラー**で、 **ドキュメント アウトライン**:

    [![](tab-bars-images/tab03a.png "ドキュメント アウトライン ビュー コント ローラーの見出しバーの項目")](tab-bars-images/tab03a.png#lightbox)
1. 必要なプロパティを設定、**ウィジェット タブ**の**プロパティ エクスプ ローラー**: 

    [![](tab-bars-images/tab03.png "ウィジェット タブ")](tab-bars-images/tab03.png#lightbox)
1. 新しいカテゴリ (タブ) を追加、削除、**ビュー コント ローラー**デザイン サーフェイスに。 

    [![](tab-bars-images/tab04.png "ビューのコント ローラー")](tab-bars-images/tab04.png#lightbox)
1. コントロールをクリックしてからドラッグ、  **タブのビューのコント ローラー**を新しい**ビュー コント ローラー**です。
1. ポップアップ ウィンドウから次のように選択します。**コント ローラーを表示**(カテゴリ) のタブとして、新しいビューを追加します。 

    [![](tab-bars-images/tab05.png "タブを選択します。")](tab-bars-images/tab05.png#lightbox)
1. IOS デザイナーで UI 要素を追加することで、通常どおりに各 Caterogies コンテンツ エリアの UI のレイアウトをデザインします。
1. C# コードで UI コントロールを使用する必要なイベントを公開します。
1. C# コードに公開するすべての UI コントロールの名前を付けます。
1. 変更内容を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. 新しい Xamarin.tvOS アプリを起動し、選択**tvOS** > **アプリ** > **タブ付きのアプリ**: 

    [![](tab-bars-images/tab02vs.png "タブ付きのアプリを選択します")](tab-bars-images/tab02vs.png#lightbox)
1. すべての新しい Xamarin.tvOS ソリューションを作成する画面の指示に従います。
1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. 変更する、**アイコン**または**タイトル**1 つのカテゴリを選択、**タブ バー項目**の**ビュー コント ローラー**で、 **ドキュメント アウトライン**:

    [![](tab-bars-images/tab03avs.png "ドキュメント アウトライン ビュー コント ローラー")](tab-bars-images/tab03avs.png#lightbox)
1. 必要なプロパティを設定、**ウィジェット タブ**の**プロパティ エクスプ ローラー**: 

    [![](tab-bars-images/tab03vs.png "ウィジェット タブ")](tab-bars-images/tab03vs.png#lightbox)
1. 新しいカテゴリ (タブ) を追加するには、次のようにドラッグします。、**ビュー コント ローラー**から、**ツールボックス**し、デザイン サーフェイスにドロップします。 

    [![](tab-bars-images/tab04vs.png "ビューのコント ローラー")](tab-bars-images/tab04vs.png#lightbox)
1. コントロールをクリックしてからドラッグ、  **タブのビューのコント ローラー**を新しい**ビュー コント ローラー**です。
1. ポップアップ ウィンドウから次のように選択します。**コント ローラーを表示**(カテゴリ) のタブとして、新しいビューを追加します。 

    [![](tab-bars-images/tab05vs.png "タブを選択します。")](tab-bars-images/tab05vs.png#lightbox)
1. IOS デザイナーで UI 要素を追加することで、通常どおりに各 Caterogies コンテンツ エリアの UI のレイアウトをデザインします。
1. C# コードで UI コントロールを使用する必要なイベントを公開します。
1. C# コードに公開するすべての UI コントロールの名前を付けます。
1. 変更内容を保存します。
    
-----

> [!IMPORTANT]
> **注:**などのイベントを割り当てることができますが`TouchUpInside`UI 要素 (など、 `UIButton`)、ios デザイナーには決して呼び出されません Apple TV はタッチ画面またはタッチ イベントのサポートがあるないためです。 常に使用する必要があります、 `Primary Action ` tvOS のイベント ハンドラーのユーザー インターフェイス要素を作成するときにイベント。

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>タブ バーの操作

使用して、`Items`のプロパティ、`UITabBar`のコレクションにアクセスする`UITabBarItems`をゼロ (0) のインデックス付き配列としてが含まれています。 `SelectedItem`として、現在選択されているタブ (カテゴリ) が返される、`UITabBarItem`です。


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>タブ バー アイテムの操作

(白いテキストと赤い oval) の指定したタブのバッジを表示するには、次のコードを使用します。

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

実行時に、次の結果が生成されるされます。

[![](tab-bars-images/tab06.png "バッジとタブ バー項目")](tab-bars-images/tab06.png#lightbox)

使用して、`Title`のプロパティ、`UITabBarItem`タイトルを変更して、`Image`アイコンを変更するプロパティです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でタブ バーのコント ローラーの操作について説明しました。




## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
