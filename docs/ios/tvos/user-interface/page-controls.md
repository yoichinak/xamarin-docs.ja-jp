---
title: TvOS Xamarin でのページ コントロールの操作
description: このドキュメントでは、Xamarin でビルドされたアプリでの tvOS ページ コントロールを操作する方法について説明します。 ページ コントロールの概要を説明して、ストーリー ボードを設定する方法について説明します、およびページ変更イベントに応答する方法を説明します。
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 173bc7713b5b8c330d4d4c5863bef24be8bdcb52
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179669"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>TvOS Xamarin でのページ コントロールの操作

場合によって、Xamarin.tvOS アプリで、一連のページやイメージを表示する必要があります。 ページ コントロールは、明らかにユーザーが上にページの最大数のうちどのページを表示する設計されました。 ページ コントロールには、一連の濃い、楕円形の背景に対してドットが表示されます。 塗りつぶされたドットを現在のページが表示されます、白抜きのドットで他のすべてのページを表示します。 その背景領域に合わせてが多すぎますがある場合、ページ コントロールは外側のほとんどのドットをクリップします。

[![](page-controls-images/page01.png "サンプル ページ コントロール")](page-controls-images/page01.png#lightbox)

ユーザーのみにフィードバックを提供する非対話型要素内のページ コントロール。 (ジェスチャ、ボタンなど) は、現在のページ番号を変更するには、その他のコントロールを追加する必要があります。

Apple では、ページ コントロールを使用する場合に次の推奨事項があります。

- **完全なコレクションのみで使用**-ページ コントロールが 1 つのコレクションに存在する複数のページを表示する全画面表示の環境で最適に動作します。
- **ページの数を制限する**-10 人以下のページ、および 20 (20) ページの最大数に最適なページ コントロール。 20 を超えるページの場合は、使用を検討して、[コレクション ビュー](~/ios/tvos/user-interface/collection-views.md)をグリッドにページを表示するとします。

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>ページ コントロールとストーリー ボード

Xamarin.tvOS アプリでページ コントロールを操作する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    
1. **Solution Pad**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**ページ コントロール**から、**ツールボックス**し、ビューにドロップします。 

    [![](page-controls-images/page02.png "ページ コントロール")](page-controls-images/page02.png#lightbox)
1. **ウィジェット タブ**の**Properties Pad**など、ページ コントロールのいくつかのプロパティを調整することができます、**現在のページ**と**のページ数**: 

    [![](page-controls-images/page03.png "[ウィジェット] タブ")](page-controls-images/page03.png#lightbox)
1. 次に、後ろに移動し、ページのコレクションを転送するには、ビューにコントロールやジェスチャを追加します。
1. 最後に、割り当てる**名**コントロール内に応答できるようにC#コード。 例えば: 

    [![](page-controls-images/page04.png "コントロールに名前")](page-controls-images/page04.png#lightbox)
1. 変更内容を保存します。
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**ページ コントロール**から、**ツールボックス**し、ビューにドロップします。 

    [![](page-controls-images/page02-vs.png "ページ コントロール")](page-controls-images/page02-vs.png#lightbox)
1. **ウィジェット タブ**の**プロパティ エクスプ ローラー**など、ページ コントロールのいくつかのプロパティを調整することができます、**現在のページ**と**のページ数**: 

    [![](page-controls-images/page03-vs.png "[ウィジェット] タブ")](page-controls-images/page03-vs.png#lightbox)
1. 次に、後ろに移動し、ページのコレクションを転送するには、ビューにコントロールやジェスチャを追加します。
1. 最後に、割り当てる**名**コントロール内に応答できるようにC#コード。 例: 

    [![](page-controls-images/page04-vs.png "コントロールに名前")](page-controls-images/page04-vs.png#lightbox)
1. 変更内容を保存します。
    

-----

> [!IMPORTANT]
> などのイベントを割り当てることはできますが`TouchUpInside`iOS Designer の UI 要素 (、UIButton) など、これは呼び出されません Apple TV がタッチ画面またはタッチ イベントをサポートしていないためです。 常に使用する必要があります、 `Primary Action` tvOS 用のイベント ハンドラーのユーザー インターフェイス要素を作成するときにイベント。

ビュー コント ローラーの編集 (例`ViewController.cs`) ファイルし、変更されているページを処理するコードを追加します。 例:

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

`CurrentPage`プロパティがゼロ (0) のベースなので、最初のページは 0 にして、最後には、ページの最大数-いずれかになります。

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。 

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と Xamarin.tvOS アプリ内でページのコントロールの操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
