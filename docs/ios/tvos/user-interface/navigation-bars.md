---
title: Xamarin での tvOS ナビゲーションバーの使用
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでナビゲーションバーを操作する方法について説明します。 ここでは、ストーリーボードでのナビゲーションバーの設定と、これらのボタンからのイベントへの応答について説明します。
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 73474aaeb138d52536dd8ad5a7dca9be566475af
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769088"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Xamarin での tvOS ナビゲーションバーの使用

ナビゲーションバーをビューの上部に追加して、タイトルとオプションのナビゲーションバーボタンを表示できます。 通常、ユーザーがメインページから移動したときに使用されます。これには、テーブルビュー、コレクション、メニューなど、選択した項目の詳細を示すサブビューが表示されます。

[![](navigation-bars-images/navbar01.png "サンプルナビゲーションバー")](navigation-bars-images/navbar01.png#lightbox)

ナビゲーションバーには、(中央に表示される) タイトルに加えて、バーの左右に1つまたは`UIBarButtonItem`複数のナビゲーションバーボタン () を含めることができます。

> [!IMPORTANT]
> ナビゲーションバーは、既定では完全に透明です。 ナビゲーションバーのコンテンツは、その下のコンテンツを読み取れるようにする必要があります。 たとえば、テーブルビューまたはコレクション内のコンテンツがその下でスクロールする場合です。

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>ナビゲーションバーとストーリーボード

TvOS アプリのナビゲーションバーを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、[ファイル] を`Main.storyboard`ダブルクリックし、編集用に開きます。
1. **ツールボックス**から**ナビゲーションバー**をドラッグし、画面の上部にあるビューにドロップします。

    [![](navigation-bars-images/navbar02.png "ナビゲーションバー")](navigation-bars-images/navbar02.png#lightbox)
1. ナビゲーション**バー**をダブルクリックして、 **[ナビゲーション項目]** を選択します。 **Properties Pad**の **[ウィジェット]** タブで、次のように**タイトル**を設定できます。

    [![](navigation-bars-images/navbar03.png "タイトルの設定")](navigation-bars-images/navbar03.png#lightbox)
1. 次に、バーの両端に1つまたは複数の**バーボタン項目**を追加できます。

    [![](navigation-bars-images/navbar04.png "バーボタンの項目")](navigation-bars-images/navbar04.png#lightbox)
1. 最後に、**プロパティエクスプローラー**の **[イベント]** タブで、**バーボタンの項目**を操作に接続します。

    [![](navigation-bars-images/navbar05.png "バーボタンの項目のアクション")](navigation-bars-images/navbar05.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、[ファイル] を`Main.storyboard`ダブルクリックし、編集用に開きます。
1. **ツールボックス**から**ナビゲーションバー**をドラッグし、画面の上部にあるビューにドロップします。

    [![](navigation-bars-images/navbar02-vs.png "ナビゲーションバー")](navigation-bars-images/navbar02-vs.png#lightbox)
1. ナビゲーション**バー**をダブルクリックして、 **[ナビゲーション項目]** を選択します。 **プロパティエクスプローラー**の **[ウィジェット]** タブで、次のように**タイトル**を設定できます。

    [![](navigation-bars-images/navbar03-vs.png "タイトルの設定")](navigation-bars-images/navbar03-vs.png#lightbox)
1. 次に、バーの両端に1つまたは複数の**バーボタン項目**を追加できます。

    [![](navigation-bars-images/navbar04-vs.png "バーボタン項目")](navigation-bars-images/navbar04-vs.png#lightbox)
1. 最後に、**プロパティエクスプローラー**の **[イベント]** タブで、**バーボタンの項目**を操作に接続します。

    [![](navigation-bars-images/navbar05-vs.png "バーボタン項目のアクション")](navigation-bars-images/navbar05-vs.png#lightbox)
1. 変更内容を保存します。

-----

> [!IMPORTANT]
> など`TouchUpInside`のイベントを、iOS デザイナーの UI 要素 (uibutton など) に割り当てることはできますが、Apple TV にタッチスクリーンやタッチイベントのサポートがないために呼び出されることはありません。 TvOS ユーザーインターフェイス要素の`Primary Action`イベントハンドラーを作成するときは、常にイベントを使用する必要があります。

次のコードは、3つの異なる barbuttonitems `ShowFirstHotel`( `ShowSecondHotel`、、および`ShowThirdHotel`) 上のイベントハンドラーの例を示しています。 各項目がクリックされると、背景`HotelImage`イメージが変更されます。 これは、ビューコントローラー (例`ViewController.cs`) ファイルで編集されています。

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

ボタンの`Enabled`プロパティが`true`であり、他のコントロールまたはビューでカバーされていない場合は、siri リモートを使用してフォーカスを設定された項目にすることができます。

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS アプリ内のナビゲーションバーの設計と操作について説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
