---
title: Xamarin で tvOS のボタンを使用
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリのボタンを使用する方法について説明します。 ボタン コードと、ストーリー ボードを操作する方法について説明し、ボタンのスタイルを設定する方法を調べます。
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/07/2017
ms.openlocfilehash: 6d8fc1daaced24dccead78c4f9d0e5d0959b3755
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116343"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Xamarin で tvOS のボタンを使用

インスタンスを使用して、 `UIButton` tvOS ウィンドウにフォーカスを設定できる、選択可能なボタンを作成するクラス。 ターゲット オブジェクトに、アクション メッセージを送信、ユーザーがボタンを選択、Xamarin.tvOS アプリの応答をユーザーの入力を許可します。

[![](buttons-images/buttons01.png "ボタンの例")](buttons-images/buttons01.png#lightbox)

フォーカスのある操作と Siri のリモート内を移動する方法の詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

<a name="About-Buttons" />

## <a name="about-buttons"></a>ボタンの概要

TvOS、ボタンはアプリに固有の操作に使用され、タイトル、アイコン、またはその両方を含めることができます。 アプリのユーザー インターフェイスを使用して、ユーザーが移動したときに、 [Siri のリモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)テキストと背景色を変更することが、指定したボタンにフォーカスを移動します。 影は、ユーザー インターフェイスの残りの部分を超えるように 3D 効果の追加 ボタンにも適用されます。

[![](buttons-images/buttons01.png "ボタンの例")](buttons-images/buttons01.png#lightbox)

Apple では、ボタンを使用するための次の推奨事項があります。

- **タイトルまたはアイコンを使用**- ながら、両方のアイコンとボタンのタイトルを含めることが、スペースが限られていない点に両方を組み合わせます。
- **明確にマークの破壊的なボタン**- 破壊的なボタンを実行している場合 (ファイルの削除) などのアクションが明確にマーク テキストまたはアイコンを使用するようにします。 破壊的な操作は常に存在する必要があります、[アラート](~/ios/tvos/user-interface/alerts.md)アクションを制限するように求めます。
- **戻るボタンの使用をしない**-Siri のリモートでのメニュー ボタンを使用すると、前の画面に戻ります。 このルールの 1 つの例外は、アプリ内購入または破壊的な操作の場所、**キャンセル**ボタンが表示されます。

フォーカスおよびナビゲーションの操作方法の詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)ドキュメント。

<a name="Button-Icons" />

### <a name="button-icons"></a>ボタンのアイコン

Apple では、ボタンのアイコンは単純な非常にわかりやすいイメージを使用することをお勧めします。 過度に複雑なアイコンでは、ソファ、部屋のテレビ画面に認識、全体で把握する最も簡単な表現を使用しようとするために困難です。 可能であれば、標準を使用して、既知の検索の虫眼鏡) (アイコン用イメージ。

<a name="Button-Titles" />

### <a name="button-titles"></a>ボタンのタイトル

Apple は、ボタンのタイトルを作成するときに次の推奨事項があります。

- **わかりやすいテキストの下のアイコン ボタンを表示する**- 可能であれば、アイコンの下、明確でわかりやすいテキストのみの各ボタンをさらに get 間で、ボタンの目的である場合。
- **タイトルの動詞または動詞句を使用して**-ユーザーがボタンをクリックしたときに、明確に状態のアクションが実行されるに配置します。
- **タイトル スタイルの大文字と小文字を使用して、** - 記事や接続詞前置詞を除き (4 文字以下)、ボタンのタイトルのすべての単語を大文字にする必要があります。
- **Short、ポイントにタイトルを使用して、** -最短の可能な表現を使用して、ボタンのアクションを記述します。

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>ボタンとストーリー ボード

Xamarin.tvOS アプリ内のボタンを使用する最も簡単な方法では、Xamarin のデザイナーを使用して iOS 用アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**ボタン**から、**ライブラリ**し、ビューにドロップします。 

    [![](buttons-images/storyboard01.png "ボタン")](buttons-images/storyboard01.png#lightbox)
1. **プロパティ エクスプ ローラー**などのボタンのいくつかのプロパティを調整することができます、**タイトル**と**テキストの色**: 

    [![](buttons-images/storyboard02.png "ボタンのプロパティ")](buttons-images/storyboard02.png#lightbox)
1. 次に、スイッチ、**イベント タブ**に接続し、**イベント**から、**ボタン**、呼び出す`ButtonPressed`: 

    [![](buttons-images/storyboard03.png "[イベント] タブ")](buttons-images/storyboard03.png#lightbox)
1. 自動的に切り替わりますが、`ViewController.cs`ビューのコードを使用して、新しいアクションを配置することができます、**を**と**ダウン**方向キー。 

    [![](buttons-images/storyboard04.png "コードで新しいアクションを配置します。")](buttons-images/storyboard04.png#lightbox)
1. キーを押して、 **Enter**場所を選択します。 

    [![](buttons-images/storyboard05.png "コード エディター")](buttons-images/storyboard05.png#lightbox)
1. すべてのファイルに変更を保存します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**ボタン**から、**ライブラリ**し、ビューにドロップします。 

    [![](buttons-images/storyboard01vs.png "ボタン")](buttons-images/storyboard01vs.png#lightbox)
1. **プロパティ エクスプ ローラー**などのボタンのいくつかのプロパティを調整することができます、**タイトル**と**テキストの色**: 

    [![](buttons-images/storyboard02vs.png "プロパティ エクスプ ローラー")](buttons-images/storyboard02vs.png#lightbox)
1. 次に、スイッチ、**イベント タブ**に接続し、**イベント**から、**ボタン**、呼び出す`ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "[イベント] タブ")](buttons-images/storyboard03vs.png#lightbox)
1. すべてのファイルに変更を保存します。



ビュー コント ローラーの編集 (例`ViewController.cs`) ファイルを開き、ボタンが選択されているを処理するために次のコードを追加します。


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

ボタンの限り`Enabled`プロパティは`true`と別のコントロールまたはビューを対象になっていない、Siri リモコンを使用して、フォーカス設定項目ことができます。 ユーザーがボタンを選択し、タッチ画面をクリックした場合、`ButtonPressed`上記で定義されたアクションは実行されます。

> [!IMPORTANT]
> などのアクションを割り当てることはできますが`TouchUpInside`を`UIButton`iOS デザイナーを作成するときに、**イベント ハンドラー**、Apple TV がタッチ画面またはタッチ イベントをサポートしていないためは呼び出されません。 既定値を常に使用する必要があります**アクションの種類**の作成時に**アクション**の tvOS ユーザー インターフェイス要素です。




ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>ボタンとコード

必要に応じて、`UIButton`で作成できますC#コードおよび tvOS アプリのビューに追加します。 例えば:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

新規に作成するときに`UIButton`指定するコードでは、その`UIButtonType`として、次のいずれか。

- **システム**-が tvOS によって提示されるボタンの標準の種類で、最も頻繁に使用する型です。
- **DetailDisclosure** -詳細な情報を表示または非表示に使用されるボタンの"有効にする の型を表示します。
- **InfoDark** -濃い円に、ボタン、"i"が表示される情報を詳しく説明します。
- **InfoLight** -ライト円に、ボタン、"i"が表示される情報を詳しく説明します。
- **AddContact** -メンバーの追加ボタンとしてボタンを表示します。
- **カスタム**- ボタンのいくつかの特性をカスタマイズすることができます。

次に、画面サイズを定義し、ボタンの位置。 例:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

次に、ボタンのタイトルを設定します。 `UIButtons` ほとんどとは異なる`UIKit`コントロールだけことはできませんので、状態を持ち、タイトルを変更する、この値を変更する必要は、指定された`UIControlState`します。 例えば:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

次に、使用、`AllEvents`イベントをユーザーがボタンをクリックしたときに参照してください。 例:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

最後に、それを表示するビューに、ボタンを追加します。

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> などのアクションを割り当てることはできますが`TouchUpInside`を`UIButton`、Apple TV がタッチ画面またはタッチ イベントをサポートしていないためは呼び出されません。 など、常にイベントを使用する必要があります**AllEvents**または**PrimaryActionTriggered**します。




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>ボタンのスタイル設定

tvOS のいくつかのプロパティを提供する、`UIButton`を使用して、そのタイトルを提供し、背景色とイメージのようなものでスタイルを設定できます。

<a name="Button-Titles" />

### <a name="button-titles"></a>ボタンのタイトル

、上記のとおり`UIButtons`ほとんどとは異なる`UIKit`コントロールだけことはできませんので、状態を持ち、タイトルを変更する、この値を変更する必要は、指定された`UIControlState`します。 例えば:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

使用して、ボタンのタイトルの色を設定することができます、`SetTitleColor`メソッド。 例えば:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

タイトルを調整してシャドウを使用して、`SetTitleShadowColor`します。 例えば:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

変更するタイトルの影を設定する*Engraved*に*浮き出し*次のコードを使用して、ボタンが強調表示するとします。

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

さらに、ボタンのタイトルとして属性付きのテキストを使用することができます。 例えば:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>ボタンのイメージ

A`UIButton`にアタッチされているイメージがあることができ、その背景としてイメージを使用することができます。

ボタンの背景イメージの設定を指定された`UIControlState`、次のコードを使用します。

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

設定、`AdjustsImageWhenHiglighted`プロパティを`true`ボタンが強調表示すると、軽量のイメージを描画するために (これは、既定値です)。 設定、`AdjustsImageWhenDisabled`プロパティを`true`ボタンが無効にした場合は、暗いイメージを描画する (ここでも、これは、既定値)。

ボタンに表示されるイメージを設定するには、次のコードを使用します。

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

使用して、`TintColor`タイトルと、ボタンのイメージの両方に適用される色の濃淡を設定するプロパティ。 ボタン、`Custom`の種類、このプロパティは影響を与えません、実装する必要があります、`TintColor`動作自分でします。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と Xamarin.tvOS アプリ内でボタンの操作について説明しました。 IOS Designer でボタンを使用する方法とボタンを作成する方法を示しましたC#コード。 最後に、ボタンのタイトルを変更し、そのスタイルと外観を変更する方法を示しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
