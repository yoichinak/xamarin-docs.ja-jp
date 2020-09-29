---
title: Xamarin での tvOS ナビゲーションバーの使用
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでナビゲーションバーを操作する方法について説明します。 ここでは、ストーリーボードでのナビゲーションバーの設定と、これらのボタンからのイベントへの応答について説明します。
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 3ee159eaaf4a124f652b083e4dd7341909f52d82
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433118"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Xamarin での tvOS ナビゲーションバーの使用

ナビゲーションバーをビューの上部に追加して、タイトルとオプションのナビゲーションバーボタンを表示できます。 通常、ユーザーがメインページから移動したときに使用されます。これには、テーブルビュー、コレクション、メニューなど、選択した項目の詳細を示すサブビューが表示されます。

[![サンプルナビゲーションバー](navigation-bars-images/navbar01.png)](navigation-bars-images/navbar01.png#lightbox)

ナビゲーションバーには、(中央に表示される) タイトルに加えて、バーの左右に1つまたは複数のナビゲーションバーボタン () を含めることができ `UIBarButtonItem` ます。

> [!IMPORTANT]
> ナビゲーションバーは、既定では完全に透明です。 ナビゲーションバーのコンテンツは、その下のコンテンツを読み取れるようにする必要があります。 たとえば、テーブルビューまたはコレクション内のコンテンツがその下でスクロールする場合です。

<a name="Navigation-Bars-and-Storyboards"></a>

## <a name="navigation-bars-and-storyboards"></a>ナビゲーションバーとストーリーボード

TvOS アプリのナビゲーションバーを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、[ファイル] をダブルクリックし、 `Main.storyboard` 編集用に開きます。
1. **ツールボックス**から**ナビゲーションバー**をドラッグし、画面の上部にあるビューにドロップします。

    [![ナビゲーションバー](navigation-bars-images/navbar02.png)](navigation-bars-images/navbar02.png#lightbox)
1. ナビゲーション **バー** をダブルクリックして、[ **ナビゲーション項目**] を選択します。 **Properties Pad**の [**ウィジェット**] タブで、次のように**タイトル**を設定できます。

    [![タイトルの設定](navigation-bars-images/navbar03.png)](navigation-bars-images/navbar03.png#lightbox)
1. 次に、バーの両端に1つまたは複数の **バーボタン項目** を追加できます。

    [![バーボタンの項目](navigation-bars-images/navbar04.png)](navigation-bars-images/navbar04.png#lightbox)
1. 最後に、**プロパティエクスプローラー**の [**イベント**] タブで、**バーボタンの項目**を操作に接続します。

    [![バーボタンの項目のアクション](navigation-bars-images/navbar05.png)](navigation-bars-images/navbar05.png#lightbox)
1. 変更を保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、[ファイル] をダブルクリックし、 `Main.storyboard` 編集用に開きます。
1. **ツールボックス**から**ナビゲーションバー**をドラッグし、画面の上部にあるビューにドロップします。

    [![ナビゲーションバー](navigation-bars-images/navbar02-vs.png)](navigation-bars-images/navbar02-vs.png#lightbox)
1. ナビゲーション **バー** をダブルクリックして、[ **ナビゲーション項目**] を選択します。 **プロパティエクスプローラー**の [**ウィジェット**] タブで、次のように**タイトル**を設定できます。

    [![タイトルの設定](navigation-bars-images/navbar03-vs.png)](navigation-bars-images/navbar03-vs.png#lightbox)
1. 次に、バーの両端に1つまたは複数の **バーボタン項目** を追加できます。

    [![バーボタン項目](navigation-bars-images/navbar04-vs.png)](navigation-bars-images/navbar04-vs.png#lightbox)
1. 最後に、**プロパティエクスプローラー**の [**イベント**] タブで、**バーボタンの項目**を操作に接続します。

    [![バーボタン項目のアクション](navigation-bars-images/navbar05-vs.png)](navigation-bars-images/navbar05-vs.png#lightbox)
1. 変更を保存します。

-----

> [!IMPORTANT]
> などのイベントを、 `TouchUpInside` IOS デザイナーの UI 要素 (UIButton など) に割り当てることはできますが、APPLE TV にタッチスクリーンやタッチイベントのサポートがないために呼び出されることはありません。 `Primary Action`TvOS ユーザーインターフェイス要素のイベントハンドラーを作成するときは、常にイベントを使用する必要があります。

次のコードは、3つの異なる BarButtonItems (、、および) 上のイベントハンドラーの例を示して `ShowFirstHotel` `ShowSecondHotel` `ShowThirdHotel` います。 各項目がクリックされると、背景イメージ `HotelImage` が変更されます。 これは、ビューコントローラー (例) ファイルで編集されてい `ViewController.cs` ます。

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

ボタンの `Enabled` プロパティがで `true` あり、他のコントロールまたはビューでカバーされていない場合は、Siri リモートを使用してフォーカスを設定された項目にすることができます。

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、tvOS アプリ内のナビゲーションバーの設計と操作について説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)