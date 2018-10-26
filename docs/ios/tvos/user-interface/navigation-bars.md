---
title: TvOS Xamarin でのナビゲーション バーの操作
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリのナビゲーション バーを操作する方法について説明します。 ストーリー ボードのナビゲーション バーを設定し、これらのボタンからのイベントに応答がについて説明します。
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 81e6cfe1e532bcfa7616e35adb28b314587bafc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106950"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>TvOS Xamarin でのナビゲーション バーの操作

ナビゲーション バーは、タイトルと省略可能なナビゲーション バーのボタンを表示するビューの上部に追加できます。 通常、ユーザーがテーブルのビュー、コレクションまたは選択した項目の詳細を示すサブビューをメニューなどのメイン ページから移動するときに使用します。

[![](navigation-bars-images/navbar01.png "サンプルのナビゲーション バー")](navigation-bars-images/navbar01.png#lightbox)

タイトル (を中心に表示されます) をさらに、ナビゲーション バーには 1 つまたは複数のナビゲーション バーのボタンも含めることができます (`UIBarButtonItem`) バーの左側および右側にします。

> [!IMPORTANT]
> ナビゲーション バーは、既定では完全に透過的です。 ナビゲーション バーのコンテンツがその下にあるコンテンツを読み取り可能な続けることに注意してください。 テーブル ビューまたはコレクション内のコンテンツ スクロールその下にある場合など、表示します。

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>ナビゲーション バーとストーリー ボード

Xamarin.tvOS アプリでのナビゲーション バーを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**、ダブルクリックして`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**ナビゲーション バー**から、**ツールボックス**し、画面の上部にあるビューにドロップします。 

    [![](navigation-bars-images/navbar02.png "ナビゲーション バー")](navigation-bars-images/navbar02.png#lightbox)
1. ダブルクリックして、**ナビゲーション バー**を選択する**ナビゲーション項目**します。 **ウィジェット**のタブ、 **Properties Pad**を設定することができます、**タイトル**: 

    [![](navigation-bars-images/navbar03.png "タイトルを設定します。")](navigation-bars-images/navbar03.png#lightbox)
1. 次に、1 つまたは複数を追加**バー ボタン項目**バーのいずれかの端に。 

    [![](navigation-bars-images/navbar04.png "バーのボタンの項目 A")](navigation-bars-images/navbar04.png#lightbox)
1. 最後に、接続、**バー ボタン項目**のアクションに、**イベント**のタブ、**プロパティ エクスプ ローラー**: 

    [![](navigation-bars-images/navbar05.png "ボタンの項目操作バー")](navigation-bars-images/navbar05.png#lightbox)
1. 変更内容を保存します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. **ソリューション エクスプ ローラー**、ダブルクリックして`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**ナビゲーション バー**から、**ツールボックス**し、画面の上部にあるビューにドロップします。 

    [![](navigation-bars-images/navbar02-vs.png "ナビゲーション バー")](navigation-bars-images/navbar02-vs.png#lightbox)
1. ダブルクリックして、**ナビゲーション バー**を選択する**ナビゲーション項目**します。 **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を設定することができます、**タイトル**: 

    [![](navigation-bars-images/navbar03-vs.png "タイトルを設定します。")](navigation-bars-images/navbar03-vs.png#lightbox)
1. 次に、1 つまたは複数を追加**バー ボタン項目**バーのいずれかの端に。 

    [![](navigation-bars-images/navbar04-vs.png "バーのボタン項目 A")](navigation-bars-images/navbar04-vs.png#lightbox)
1. 最後に、接続、**バー ボタン項目**のアクションに、**イベント**のタブ、**プロパティ エクスプ ローラー**: 

    [![](navigation-bars-images/navbar05-vs.png "ボタンのアイテムの操作バー")](navigation-bars-images/navbar05-vs.png#lightbox)
1. 変更内容を保存します。


-----

> [!IMPORTANT]
> などのイベントを割り当てることはできますが`TouchUpInside`iOS Designer の UI 要素 (、UIButton) など、これは呼び出されません Apple TV がタッチ画面またはタッチ イベントをサポートしていないためです。 常に使用する必要があります、 `Primary Action` tvOS 用のイベント ハンドラーのユーザー インターフェイス要素を作成するときにイベント。

次のコードは、次の 3 つの異なる BarButtonItems でイベント ハンドラーの例を示します: `ShowFirstHotel`、 `ShowSecondHotel`、および`ShowThirdHotel`します。 各項目がクリックされたとき、背景画像`HotelImage`が変更されました。 これは、ビュー コント ローラーで編集 (例`ViewController.cs`) ファイル。

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

ボタンの限り`Enabled`プロパティは`true`と別のコントロールまたはビューを対象になっていない、Siri リモコンを使用して、フォーカス設定項目ことができます。

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。 

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と Xamarin.tvOS アプリ内でのナビゲーション バーの操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
