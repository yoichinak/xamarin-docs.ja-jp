---
title: Xamarin での tvOS Page コントロールの使用
description: このドキュメントでは、Xamarin でビルドされたアプリで tvOS page コントロールを操作する方法について説明します。 ページコントロールの概要を説明し、ストーリーボードで設定する方法について説明し、ページ変更イベントに応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 12fe9645ab832db1db37e36b0342664bbd2fe9f8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030413"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>Xamarin での tvOS Page コントロールの使用

場合によっては、tvOS アプリに一連のページまたはイメージを表示する必要があります。 ページコントロールは、ユーザーが最大ページ数を超えているページを明確に示すように設計されています。 ページコントロールには、暗い楕円形の背景に対して一連のドットが表示されます。 現在のページには塗りつぶされたドットが表示され、他のすべてのページは中空点として表示されます。 ページコントロールは、背景領域に収まりきらない場合に、最も外側のドットをクリップします。

[![](page-controls-images/page01.png "Sample Page control")](page-controls-images/page01.png#lightbox)

ユーザーのみにフィードバックを提供するように設計された非対話型の要素のページコントロール。 現在のページ番号 (ジェスチャやボタンなど) を変更するには、他のコントロールを追加する必要があります。

ページコントロールを使用する場合、Apple には次の推奨事項があります。

- **完全なコレクションでのみ使用**する-ページコントロールは全画面環境で最適に機能し、1つのコレクションに存在する複数のページを表示します。
- ページ**の数を制限**します。ページコントロールは、10 (10) 以下のページ、および最大で 20 (20) ページに最適です。 20ページを超える場合は、[コレクションビュー](~/ios/tvos/user-interface/collection-views.md)を使用し、グリッドにページを表示することを検討してください。

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>ページコントロールとストーリーボード

TvOS アプリでページコントロールを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、`Main.storyboard` ファイルをダブルクリックして開き、編集します。
1. **ツールボックス**から**ページコントロール**をドラッグし、ビューにドロップします。

    [![](page-controls-images/page02.png "A Page Control")](page-controls-images/page02.png#lightbox)
1. **Properties Pad**の [**ウィジェット] タブ**では、ページコントロールの**現在のページ**や**ページの**数など、いくつかのプロパティを調整できます。

    [![](page-controls-images/page03.png "The Widget Tab")](page-controls-images/page03.png#lightbox)
1. 次に、コントロールまたはジェスチャをビューに追加して、ページのコレクションを前後に移動します。
1. 最後に、コントロールに**名前**を割り当てて、コードでC#それらに応答できるようにします。 (例:

    [![](page-controls-images/page04.png "Name the control")](page-controls-images/page04.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、`Main.storyboard` ファイルをダブルクリックして開き、編集します。
1. **ツールボックス**から**ページコントロール**をドラッグし、ビューにドロップします。

    [![](page-controls-images/page02-vs.png "A Page Control")](page-controls-images/page02-vs.png#lightbox)
1. **プロパティエクスプローラー**の [**ウィジェット] タブ**では、**現在のページ**やページ**の**数など、ページコントロールのいくつかのプロパティを調整できます。

    [![](page-controls-images/page03-vs.png "The Widget tab")](page-controls-images/page03-vs.png#lightbox)
1. 次に、コントロールまたはジェスチャをビューに追加して、ページのコレクションを前後に移動します。
1. 最後に、コントロールに**名前**を割り当てて、コードでC#それらに応答できるようにします。 (例:

    [![](page-controls-images/page04-vs.png "Name the control")](page-controls-images/page04-vs.png#lightbox)
1. 変更内容を保存します。

-----

> [!IMPORTANT]
> `TouchUpInside` などのイベントを iOS デザイナーの UI 要素 (UIButton など) に割り当てることはできますが、Apple TV にタッチスクリーンやタッチイベントのサポートがないため、呼び出されません。 TvOS ユーザーインターフェイス要素のイベントハンドラーを作成するときは、常に `Primary Action` イベントを使用する必要があります。

ビューコントローラー (例 `ViewController.cs`) ファイルを編集し、変更されているページを処理するコードを追加します。 (例:

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

ページコントロールの2つのプロパティについて詳しく見ていきましょう。 まず、ページの最大数を指定するには、次のように指定します。

```csharp
PageView.Pages = 6;
```

現在のページ番号を変更するには、次のコードを使用します。

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` プロパティはゼロ (0) に基づいているため、最初のページはゼロになり、最後のページはページの最大数を引いたものになります。

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、tvOS アプリ内でのページコントロールの設計と操作について説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
