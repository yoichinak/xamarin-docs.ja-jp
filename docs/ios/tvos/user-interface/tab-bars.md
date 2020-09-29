---
title: Xamarin での tvOS タブバーコントローラーの使用
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでタブバーコントローラーを操作する方法について説明します。 タブバーの高度なビューが用意されており、タブバー項目、ストーリーボード統合、およびタブバー項目について説明しています。
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 813a4de1966aa53e9c1447804deda8f55e5672f3
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430282"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>Xamarin での tvOS タブバーコントローラーの使用

多くの種類の tvOS アプリでは、メインナビゲーションは画面の上部で実行されるタブバーとして表示されます。 ユーザーは、使用可能なカテゴリの一覧と、ユーザーの選択内容を反映するように変更されたコンテンツ領域のスワイプを左右します。

[![サンプルタブバー](tab-bars-images/tab01.png)](tab-bars-images/tab01.png#lightbox)

タブバーは既定では半透明であり、常に画面の上部に表示されます。 フォーカスが移動すると、タブバーは画面の上位140ピクセルに対応しますが、フォーカスが下のコンテンツ領域に移ったときにすぐにスライドします。

<a name="Tab-Bars-in-tvOS"></a>

## <a name="tab-bars-in-tvos"></a>TvOS のタブバー

は `UITabViewController` 同様の方法で動作し、tvOS では、iOS と同様の目的で機能します。主な違いは次のとおりです。

- 画面の下部に表示される iOS のタブバーとは異なり、tvOS のタブバーは画面の上部の140ピクセルを占め、既定では半透明になっています。
- 下のコンテンツ領域のタブバーがフォーカスを移動すると、タブバーが画面の上部からすぐにスライドし、非表示になります。 ユーザーはメニューボタンを1回タップするか、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) でスワイプして、もう一度タブバーを表示することができます。
- Siri リモートでスワイプすると、タブバーの下のコンテンツ領域にフォーカスが移動し、表示されているコンテンツ内のフォーカスがある最初の [項目](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) に移動します。 ここでも、フォーカスが移動するとタブバーが非表示になります。
- タブバーに表示されるカテゴリをクリックして選択すると、そのカテゴリのコンテンツに切り替わり、フォーカスはそのビュー内のフォーカスが設定された最初の項目に切り替わります。
- タブバーに表示されるカテゴリの数は固定する必要があり、すべてのカテゴリに常にアクセスできるようにする必要があります。特定のカテゴリを無効にすることはできません。
- タブバーは、tvOS のカスタマイズをサポートしていません。 また、タブバーに表示されるよりも多くのカテゴリがある場合は、 **より多く** のカテゴリ (iOS など) は表示されません。

Apple には、タブバーの操作に関する次のような推奨事項があります。

- **タブバーを使用してコンテンツを論理的に整理する** -タブバーを使用して、tvOS アプリが動作するコンテンツを論理的に整理します。 たとえば、おすすめの上位のグラフ、購入して検索します。
- **新しいコンテンツをユーザーに通知するバッジを追加** します。必要に応じて、バッジ (白い数字や感嘆符を含む赤い楕円) を表示して、カテゴリ内の新しいコンテンツをユーザーに通知することができます。
- **バッジは控えめに使用** します。バッジでタブバーを乱雑にせず、ユーザーに重要な情報を提供している場所だけを表示します。
- **カテゴリの数を制限** する-複雑さを軽減し、アプリを管理しやすくするために、カテゴリを使用してタブバーをオーバーロードし、すべてのカテゴリが表示され、混雑していないことを確認します。 簡単で、短いタイトルが最適です。
- **カテゴリを無効** にしない-すべてのタブ (カテゴリ) が常に表示され、常に有効になっている必要があります。 特定のタブにコンテンツがない場合は、その理由をユーザーに説明します。 たとえば、ユーザーが購入を行っていない場合、[購入] タブは空になります。

<a name="Tab-Bar-Items"></a>

## <a name="tab-bar-items"></a>タブバー項目

タブバーの各カテゴリ (タブ) は、タブバー項目 () で表され `UITabBarItem` ます。 Apple には、タブバー項目を操作するための次のような推奨事項があります。

- **テキストベースのタブを使用** する-タブバー項目をアイコンとして表示できますが、簡潔なタイトルはアイコンよりも簡単に解釈できるため、Apple ではテキストのみを使用することをお勧めします。
- **短い、意味のある名詞または動詞を使用** する-タブバー項目は、含まれているコンテンツを明確にリレーし、単純な名詞 (写真、映画、音楽など) または動詞 (検索や再生など) である場合に最適に機能します。

<a name="Tab-Bars-and-Storyboards"></a>

## <a name="tab-bars-and-storyboards"></a>タブバーとストーリーボード

TvOS アプリのタブバーを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. 新しい tvOS アプリを開始し、[ **tvOS**  >  **app**]  >  **タブ付きアプリ**を選択します。 

    [![タブ付きアプリの選択](tab-bars-images/tab02.png)](tab-bars-images/tab02.png#lightbox)
1. すべてのプロンプトに従って、新しい tvOS ソリューションを作成します。
1. **Solution Pad**で、ファイルをダブルクリックし `Main.storyboard` て開き、編集します。
1. 特定のカテゴリの**アイコン**または**タイトル**を変更するには、[**ドキュメントアウトライン**] の**ビューコントローラー**の**タブバー項目**を選択します。

    [![ドキュメントアウトラインのビューコントローラーのタブバー項目](tab-bars-images/tab03a.png)](tab-bars-images/tab03a.png#lightbox)
1. 次に、**プロパティエクスプローラー**の [**ウィジェット] タブ**で、必要なプロパティを設定します。 

    [![[ウィジェット] タブ](tab-bars-images/tab03.png)](tab-bars-images/tab03.png#lightbox)
1. 新しいカテゴリ (タブ) を追加するには、 **ビューコントローラー** をデザインサーフェイスにドロップします。 

    [![ビューコントローラー](tab-bars-images/tab04.png)](tab-bars-images/tab04.png#lightbox)
1. コントロールをクリックし、 **タブビューコントローラー** から新しい **ビューコントローラー**にドラッグします。
1. ポップアップから [コントローラーの **表示** ] を選択して、新しいビューをタブ (カテゴリ) として追加します。 

    [![タブの選択](tab-bars-images/tab05.png)](tab-bars-images/tab05.png#lightbox)
1. IOS デザイナーで UI 要素を追加することによって、各 Caterogies コンテンツ領域の UI のレイアウトを通常どおりにデザインします。
1. C# コードで UI コントロールを操作するために必要なイベントを公開します。
1. C# コードで公開する UI コントロールの名前を指定します。
1. 変更を保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. 新しい tvOS アプリを開始し、[ **tvOS**  >  **app**]  >  **タブ付きアプリ**を選択します。 

    [![タブ付きアプリの選択](tab-bars-images/tab02vs.png)](tab-bars-images/tab02vs.png#lightbox)
1. すべてのプロンプトに従って、新しい tvOS ソリューションを作成します。
1. **ソリューションエクスプローラー**で、ファイルをダブルクリックし `Main.storyboard` て開き、編集します。
1. 特定のカテゴリの**アイコン**または**タイトル**を変更するには、[**ドキュメントアウトライン**] の**ビューコントローラー**の**タブバー項目**を選択します。

    [![ドキュメントアウトラインのビューコントローラー](tab-bars-images/tab03avs.png)](tab-bars-images/tab03avs.png#lightbox)
1. 次に、**プロパティエクスプローラー**の [**ウィジェット] タブ**で、必要なプロパティを設定します。 

    [![[ウィジェット] タブ](tab-bars-images/tab03vs.png)](tab-bars-images/tab03vs.png#lightbox)
1. 新しいカテゴリ (タブ) を追加するには、[**ツールボックス**] から**ビューコントローラー**をドラッグし、デザインサーフェイスにドロップします。 

    [![ビューコントローラー](tab-bars-images/tab04vs.png)](tab-bars-images/tab04vs.png#lightbox)
1. コントロールをクリックし、 **タブビューコントローラー** から新しい **ビューコントローラー**にドラッグします。
1. ポップアップから [コントローラーの **表示** ] を選択して、新しいビューをタブ (カテゴリ) として追加します。 

    [![タブの選択](tab-bars-images/tab05vs.png)](tab-bars-images/tab05vs.png#lightbox)
1. IOS Designer で UI 要素を追加することによって、各 Caterogies コンテンツ領域の UI のレイアウトを通常どおりにデザインします。
1. C# コードで UI コントロールを操作するために必要なイベントを公開します。
1. C# コードで公開する UI コントロールの名前を指定します。
1. 変更を保存します。

-----

> [!IMPORTANT]
> などのイベントを `TouchUpInside` IOS デザイナーで UI 要素 (など) に割り当てることはでき `UIButton` ますが、Apple TV にタッチスクリーンやタッチイベントのサポートがないために呼び出されることはありません。 `Primary Action`TvOS ユーザーインターフェイス要素のイベントハンドラーを作成するときは、常にイベントを使用する必要があります。

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。 

<a name="Working-with-Tab-Bars"></a>

## <a name="working-with-tab-bars"></a>タブバーの操作

のプロパティを使用して `Items` 、に `UITabBar` 格納されているのコレクションに、 `UITabBarItems` ゼロ (0) のインデックス付き配列としてアクセスします。 プロパティは、 `SelectedItem` 現在選択されているタブ (カテゴリ) をとして返し `UITabBarItem` ます。

<a name="Working-with-Tab-Bar-Items"></a>

## <a name="working-with-tab-bar-items"></a>タブバー項目の操作

特定のタブ (白いテキストを含む赤い楕円) にバッジを表示するには、次のコードを使用します。

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

実行すると、次の結果が生成されます。

[![バッジが付いたタブバー項目](tab-bars-images/tab06.png)](tab-bars-images/tab06.png#lightbox)

のプロパティを使用して、 `Title` `UITabBarItem` タイトルとプロパティを変更し、 `Image` アイコンを変更します。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、tvOS アプリ内でのタブバーコントローラーの設計と操作について説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)