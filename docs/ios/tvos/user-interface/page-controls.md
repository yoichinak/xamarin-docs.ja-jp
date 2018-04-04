---
title: ページ コントロールの操作
description: この記事では、設計と Xamarin.tvOS アプリ内でページ コントロールの使い方について説明します。
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1cb07c050aeb2ee72e34048baab991df2d5775d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-page-control"></a>ページ コントロールの操作

_この記事では、設計と Xamarin.tvOS アプリ内でページ コントロールの使い方について説明します。_

場合によっては、Xamarin.tvOS アプリで、一連のページやイメージを表示する必要があります。 ページ コントロールは、明確に、ユーザーがページの最大数外でどのページを表示するように設計されました。 ページ コントロールには、一連の dark、楕円形の背景に対してドットが表示されます。 黒点を現在のページが表示されますが、他のすべてのページが中空のドットで表示されます。 その背景領域に合わせてが多すぎますがある場合、ページ コントロールは外側のほとんどのドットをクリップします。

[![](page-controls-images/page01.png "ページ コントロールをサンプルします。")](page-controls-images/page01.png#lightbox)

ユーザーのみにフィードバックを提供する非対話型要素内のページ コントロール。 (ジェスチャ ボタンなど) の現在のページ番号を変更するには、その他のコントロールを追加する必要があります。

ページ コントロールを使用する場合、Apple から以下の推奨事項があります。

- **完全コレクションのみで使用する**のページ コントロールは、単一のコレクションに存在する複数のページを表示する、全画面表示の環境で最も適切に機能します。
- **ページの数を制限する**-10 個以下のページ、および 20 (20) ページの最大数に最適なページ コントロール。 20 を超えるページは、使用を検討して、[コレクション ビュー](~/ios/tvos/user-interface/collection-views.md)をグリッドにページを表示するとします。

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>ページ コントロールとストーリー ボード

Xamarin.tvOS アプリでのページ コントロールを操作する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**ページ コントロール**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](page-controls-images/page02.png "ページ コントロール")](page-controls-images/page02.png#lightbox)
1. **ウィジェット タブ**の**プロパティ パッド**などいくつかのページ コントロールのプロパティを調整することができます、**現在のページ**と**のページ数**: 

    [![](page-controls-images/page03.png "ウィジェット タブ")](page-controls-images/page03.png#lightbox)
1. 次に、後ろに移動し、ページのコレクションを転送するビューにコントロールやジェスチャを追加します。
1. 最後に、割り当てる**名**コントロールに c# コードでそれらに応答できるようにします。 例えば: 

    [![](page-controls-images/page04.png "コントロールの名前")](page-controls-images/page04.png#lightbox)
1. 変更内容を保存します。
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**ページ コントロール**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](page-controls-images/page02-vs.png "ページ コントロール")](page-controls-images/page02-vs.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**などいくつかのページ コントロールのプロパティを調整することができます、**現在のページ**と**のページ数**: 

    [![](page-controls-images/page03-vs.png "ウィジェット タブ")](page-controls-images/page03-vs.png#lightbox)
1. 次に、後ろに移動し、ページのコレクションを転送するビューにコントロールやジェスチャを追加します。
1. 最後に、割り当てる**名**コントロールに c# コードでそれらに応答できるようにします。 例えば: 

    [![](page-controls-images/page04-vs.png "コントロールの名前")](page-controls-images/page04-vs.png#lightbox)
1. 変更内容を保存します。
    

-----

> [!IMPORTANT]
> などのイベントを割り当てることができますが`TouchUpInside`UI 要素に (など、UIButton)、ios デザイナーには決して呼び出されません Apple TV はタッチ画面またはタッチ イベントのサポートがあるないためです。 常に使用する必要があります、 `Primary Action` tvOS のイベント ハンドラーのユーザー インターフェイス要素を作成するときにイベント。




編集ビュー コント ローラー (例`ViewController.cs`) ファイルし、変更されているページを処理するコードを追加します。 例えば:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

ページ コントロールの 2 つのプロパティについて詳しく見てをみましょう。 最初に、ページの最大数を指定するには、次のように使用します。

```csharp
PageView.Pages = 6;
```

現在のページ番号を変更するには、次のコードを使用します。

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage`プロパティがゼロ (0) ベースで最初のページ 0 になり、最後には、最大ページ数を引いた数いずれかになります。

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。 

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でページ コントロールの使い方について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
