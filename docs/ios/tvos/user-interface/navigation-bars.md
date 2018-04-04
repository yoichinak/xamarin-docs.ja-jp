---
title: ナビゲーションのコント ローラーの使用
description: この記事では、設計と Xamarin.tvOS アプリ内でのナビゲーション バーの操作について説明します。
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8a9a1c852137a2bcc0d46615e69eef0a245a9768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-navigation-controllers"></a>ナビゲーションのコント ローラーの使用

_この記事では、設計と Xamarin.tvOS アプリ内でのナビゲーション バーの操作について説明します。_

ナビゲーション バーは、タイトルと省略可能なナビゲーション バーのボタンを表示するビューの先頭に追加できます。 通常、ユーザーがテーブルのビュー、コレクションまたは選択した項目の詳細を示すサブビュー メニューのように、メイン ページから移動するときに使用します。

[![](navigation-bars-images/navbar01.png "サンプルのナビゲーション バー")](navigation-bars-images/navbar01.png#lightbox)

ナビゲーション バーの見出し (中央に表示されます) をさらに、1 つまたは複数のナビゲーション バーのボタンを含めることができます (`UIBarButtonItem`) 左およびバーの右側にします。

> [!IMPORTANT]
> ナビゲーション バーは、既定では完全に透過的です。 下にあるコンテンツをナビゲーション バーの内容が読み取り可能なことに注意してください。 たとえば、ときにテーブルのビューまたはコレクション内のコンテンツがスクロールします。




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>ナビゲーション バーとストーリー ボード

Xamarin.tvOS アプリでのナビゲーション バーを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. **ソリューション パッド**、ダブルクリック`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**ナビゲーション バー**から、**ツールボックス**し、画面の上部にあるビュー上にドロップします。 

    [![](navigation-bars-images/navbar02.png "ナビゲーション バー")](navigation-bars-images/navbar02.png#lightbox)
1. ダブルクリックして、**ナビゲーション バー**に選択する**ナビゲーション項目**です。 **ウィジェット**のタブ、**プロパティ パッド**、設定することができます、**タイトル**: 

    [![](navigation-bars-images/navbar03.png "タイトルを設定します。")](navigation-bars-images/navbar03.png#lightbox)
1. 次に、1 つまたは複数を追加できる**バー ボタン項目**バーのいずれかの端に。 

    [![](navigation-bars-images/navbar04.png "バーのボタン アイテム A")](navigation-bars-images/navbar04.png#lightbox)
1. 最後に、ネットワーク上、**バー ボタン項目**のアクションに、**イベント**のタブ、**プロパティ エクスプ ローラー**: 

    [![](navigation-bars-images/navbar05.png "バーのボタン アイテム操作 A")](navigation-bars-images/navbar05.png#lightbox)
1. 変更内容を保存します。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **ソリューション エクスプ ローラー**、ダブルクリック`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**ナビゲーション バー**から、**ツールボックス**し、画面の上部にあるビュー上にドロップします。 

    [![](navigation-bars-images/navbar02-vs.png "ナビゲーション バー")](navigation-bars-images/navbar02-vs.png#lightbox)
1. ダブルクリックして、**ナビゲーション バー**に選択する**ナビゲーション項目**です。 **ウィジェット**のタブ、**プロパティ エクスプ ローラー**、設定することができます、**タイトル**: 

    [![](navigation-bars-images/navbar03-vs.png "タイトルを設定します。")](navigation-bars-images/navbar03-vs.png#lightbox)
1. 次に、1 つまたは複数を追加できる**バー ボタン項目**バーのいずれかの端に。 

    [![](navigation-bars-images/navbar04-vs.png "バーのボタン アイテム A")](navigation-bars-images/navbar04-vs.png#lightbox)
1. 最後に、ネットワーク上、**バー ボタン項目**のアクションに、**イベント**のタブ、**プロパティ エクスプ ローラー**: 

    [![](navigation-bars-images/navbar05-vs.png "バーのボタンの動作のアイテム A")](navigation-bars-images/navbar05-vs.png#lightbox)
1. 変更内容を保存します。


-----

> [!IMPORTANT]
> などのイベントを割り当てることができますが`TouchUpInside`UI 要素に (など、UIButton)、ios デザイナーには決して呼び出されません Apple TV はタッチ画面またはタッチ イベントのサポートがあるないためです。 常に使用する必要があります、 `Primary Action` tvOS のイベント ハンドラーのユーザー インターフェイス要素を作成するときにイベント。




次のコードは、次の 3 つの異なる BarButtonItems のイベント ハンドラーの例を示します: `ShowFirstHotel`、 `ShowSecondHotel`、および`ShowThirdHotel`です。 各アイテムがクリックされたとき、背景画像`HotelImage`を変更します。 これはビュー コント ローラーで編集 (例`ViewController.cs`) ファイル。

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

ボタンの限り`Enabled`プロパティは`true`と別のコントロールまたはビューでは対応できない、Siri リモコンを使用して、フォーカス設定項目にできます。

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。 

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でのナビゲーション バーの操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
