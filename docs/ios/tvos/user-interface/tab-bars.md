---
title: TvOS Xamarin でのタブ バー コント ローラーの操作
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリ内のタブ バー コント ローラーを使用する方法について説明します。 ビューのタブの横棒の上、高レベルを提供し、タブ バー項目、ストーリー ボードの統合、およびタブ バー項目について説明します。
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: a0efc30fd9814e4da858c4e3e4e99990eccf102e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119871"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>TvOS Xamarin でのタブ バー コント ローラーの操作

多くの種類の tvOS アプリでは、プライマリ ナビゲーション画面の一番上で実行されているタブ バーとして表示されます。 ユーザーは、可能なカテゴリと、ユーザーの選択を反映するように、変更の下のコンテンツ領域の一覧で、左側と右側を読み取ります。

[![](tab-bars-images/tab01.png "サンプルのタブ バー")](tab-bars-images/tab01.png#lightbox)

タブ バーは既定で半透明し、常に、画面の上部に表示されます。 、フォーカスがあるときは、タブ バーは画面の最上位の 140 ピクセルを取り上げますが、以下のコンテンツ領域にフォーカスが移動したときにスライドはすばやく。

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>TvOS のタブ バー

`UITabViewController`同様の方法で動作し、次の主な違いと、iOS では、同様の目的を tvOS で機能します。

- IOS 画面の下部に表示されるタブ バーとは異なり tvOS のタブ バーは画面の最上位の 140 ピクセルを占有し、既定で半透明します。
- フォーカスが下のコンテンツ領域のタブ バーを離れるときにタブ バーは画面の上端からスライドはすばやくを非表示にします。 ユーザーのメニュー ボタンを 1 回タップまたはの上方向にスワイプできる、 [Siri のリモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)タブ バーを再び表示します。
- 最初のタブ バーの下のコンテンツ領域にフォーカスを移動は Siri のリモートで下へスワイプ[フォーカスを設定できる項目](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection)で表示されているコンテンツ。 ここでも、タブ バーを非表示にフォーカスを移動するとこれは。
- タブ バーに表示されるカテゴリを選択する をクリックするのカテゴリのコンテンツとフォーカスをそのビューにフォーカスを設定できる最初の項目に切り替えが切り替えられます。
- タブ バーに表示されるカテゴリ数を固定する必要があり、すべてのカテゴリは、常にアクセスできる必要がある、特定のカテゴリを無効にすることはありません。
- タブ バーでは、tvOS のカスタマイズはサポートされていません。 さらに、それらを表示しない、**詳細**(iOS) などのカテゴリではより多くのカテゴリがある場合、タブ バーを格納できます。

Apple では、タブ バーを操作するための次の推奨事項があります。

- **コンテンツを論理的に整理するタブ バーを使用して**-タブ バーを使用して、tvOS アプリのコンテンツを論理的に整理します。 たとえば、おすすめ、上部のグラフ、Purchased および検索です。
- **バッジを新しいコンテンツのユーザーの通知に追加**-新しいコンテンツのカテゴリのユーザーに通知するバッジ (白の数字、感嘆符の赤い楕円) を必要に応じて表示できます。
- **控えめにバッジを使用して、** - しないバッジとタブ バーを整理し、ユーザーに重要な情報を提供することのみが表示されます。
- **カテゴリの数を制限する**でに複雑さを軽減し保持するアプリ管理しやすい、しないカテゴリで、タブ バーをオーバー ロードしてすべてのカテゴリが表示されないいっぱいになったことを確認します。 単純で短いタイトルに最適です。
- **カテゴリを無効にしない**-すべてのタブ (カテゴリ) は常に表示して有効に常にします。 指定したタブに内容が含まれない場合、理由をユーザーに、理由を説明します。 たとえば、購入 タブは、ユーザーが購入行われなかった場合に空になります。

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>タブ バー項目

タブ バー項目によって表されるタブ バー内の各カテゴリ (タブ) (`UITabBarItem`)。 Apple では、タブ バー項目を操作するための次の推奨事項があります。

- **テキスト ベースのタブを使用して**-Apple 提案、タブ バー項目中には、アイコンとして表されることは、簡潔なタイトルがアイコンよりを解釈しやすくするためにのみ、テキストを使用します。
- **短期、わかりやすい名詞と動詞を使用して、** -あるタブ バー項目を含まれており、(写真、ビデオや音楽) などの単純な名詞や動詞 (検索や再生) がある場合に最適に動作するコンテンツを中継明確にする必要があります。

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>タブ バーとストーリー ボード

Xamarin.tvOS アプリでタブ バーを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. 新しい Xamarin.tvOS アプリを起動し、選択**tvOS** > **アプリ** > **タブ付きアプリ**: 

    [![](tab-bars-images/tab02.png "タブ付きアプリを選択します。")](tab-bars-images/tab02.png#lightbox)
1. すべての新しい Xamarin.tvOS ソリューションを作成する、画面の指示に従います。
1. **Solution Pad**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. 変更する、**アイコン**または**タイトル**特定のカテゴリを選択、**タブ バー項目**の**ビュー コント ローラー**で、 **ドキュメント アウトライン**:

    [![](tab-bars-images/tab03a.png "ドキュメント アウトライン ビュー コント ローラーのタブ バーの項目")](tab-bars-images/tab03a.png#lightbox)
1. 必要なプロパティを設定、**ウィジェット タブ**の**プロパティ エクスプ ローラー**: 

    [![](tab-bars-images/tab03.png "[ウィジェット] タブ")](tab-bars-images/tab03.png#lightbox)
1. 新しいカテゴリ (タブ) を追加するには、削除、**ビュー コント ローラー**デザイン サーフェイスに。 

    [![](tab-bars-images/tab04.png "ビュー コント ローラー")](tab-bars-images/tab04.png#lightbox)
1. コントロールをクリックしてからドラッグ、 **タブのビューのコント ローラー**を新しい**ビュー コント ローラー**です。
1. ポップアップから次のように選択します。**コントローラの表示** タブ (カテゴリ) として、新しいビューを追加します。 

    [![](tab-bars-images/tab05.png "タブを選択します")](tab-bars-images/tab05.png#lightbox)
1. IOS Designer の UI 要素を追加することで、通常どおり各 Caterogies コンテンツ領域用の UI のレイアウトをデザインします。
1. UI コントロールを使用する必要なイベントも公開C#コード。
1. 公開するすべての UI コントロールの名前を付けますC#コード。
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 新しい Xamarin.tvOS アプリを起動し、選択**tvOS** > **アプリ** > **タブ付きアプリ**: 

    [![](tab-bars-images/tab02vs.png "タブ付きアプリを選択します。")](tab-bars-images/tab02vs.png#lightbox)
1. すべての新しい Xamarin.tvOS ソリューションを作成する、画面の指示に従います。
1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. 変更する、**アイコン**または**タイトル**特定のカテゴリを選択、**タブ バー項目**の**ビュー コント ローラー**で、 **ドキュメント アウトライン**:

    [![](tab-bars-images/tab03avs.png "ドキュメント アウトライン ビュー コント ローラー")](tab-bars-images/tab03avs.png#lightbox)
1. 必要なプロパティを設定、**ウィジェット タブ**の**プロパティ エクスプ ローラー**: 

    [![](tab-bars-images/tab03vs.png "[ウィジェット] タブ")](tab-bars-images/tab03vs.png#lightbox)
1. (タブ) の新しいカテゴリを追加するには、ドラッグ、**ビュー コント ローラー**から、**ツールボックス**し、デザイン画面にドロップします。 

    [![](tab-bars-images/tab04vs.png "ビュー コント ローラー")](tab-bars-images/tab04vs.png#lightbox)
1. コントロールをクリックしてからドラッグ、 **タブのビューのコント ローラー**を新しい**ビュー コント ローラー**です。
1. ポップアップから次のように選択します。**コントローラの表示** タブ (カテゴリ) として、新しいビューを追加します。 

    [![](tab-bars-images/tab05vs.png "タブを選択します")](tab-bars-images/tab05vs.png#lightbox)
1. IOS Designer の UI 要素を追加することで、通常どおり各 Caterogies コンテンツ領域用の UI のレイアウトをデザインします。
1. UI コントロールを使用する必要なイベントも公開C#コード。
1. 公開するすべての UI コントロールの名前を付けますC#コード。
1. 変更内容を保存します。
    
-----

> [!IMPORTANT]
> などのイベントを割り当てることはできますが`TouchUpInside`UI 要素に (など、 `UIButton`)、ios デザイナーには呼び出されません Apple TV がタッチ画面またはタッチ イベントをサポートしていないためです。 常に使用する必要があります、 `Primary Action ` tvOS 用のイベント ハンドラーのユーザー インターフェイス要素を作成するときにイベント。

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>タブ バーの操作

使用して、`Items`のプロパティ、`UITabBar`のコレクションにアクセスする`UITabBarItems`として 0 (0) のインデックス付き配列が含まれています。 `SelectedItem`として現在選択されているタブ (カテゴリ) が返される、`UITabBarItem`します。


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>タブ バー項目の操作

バッジを (赤い楕円と白いテキスト) の指定したタブに表示するには、次のコードを使用します。

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

実行時に、次の結果を生成する場合します。

[![](tab-bars-images/tab06.png "バッジとタブ バー項目")](tab-bars-images/tab06.png#lightbox)

使用して、`Title`のプロパティ、`UITabBarItem`タイトルを変更して、`Image`アイコンを変更するプロパティ。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計とタブ バー コント ローラー Xamarin.tvOS アプリ内での操作について説明しました。




## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
