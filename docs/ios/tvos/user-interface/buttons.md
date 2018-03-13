---
title: "ボタンの操作"
description: "この記事では、設計と Xamarin.tvOS アプリ内でのボタンの操作について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 4b2a470d7fe2a1f9d4b8df40836c934547adf614
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-buttons"></a>ボタンの操作

_この記事では、設計と Xamarin.tvOS アプリ内でのボタンの操作について説明します。_


インスタンスを使用して、 `UIButton` tvOS ウィンドウにフォーカスを設定できる、選択可能なボタンを作成するクラス。 ターゲット オブジェクトに、アクション メッセージを送信、ユーザーがボタンを選択すると、ユーザーに、Xamarin.tvOS アプリの応答の入力を許可します。

[![](buttons-images/buttons01.png "ボタンの例")](buttons-images/buttons01.png#lightbox)

フォーカスのある操作と Siri リモコンを使用した移動の詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[Siri リモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

<a name="About-Buttons" />

## <a name="about-buttons"></a>ボタンの概要

TvOS、ボタンは、アプリ固有のアクションの使用され、タイトル、アイコン、またはその両方を含めることがあります。 アプリのユーザー インターフェイスを使用して、ユーザーが移動したときに、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)テキスト色と背景色を変更することが、指定したボタンにフォーカスを移動します。 影は、ユーザー インターフェイスの残りの部分を超えるように表示する 3D 効果の追加 ボタンにも適用されます。

[![](buttons-images/buttons01.png "ボタンの例")](buttons-images/buttons01.png#lightbox)

Apple では、ボタンの操作の次の方法があります。

- **タイトル、またはアイコンを使用する**- ながら、両方のアイコンとボタンにタイトルを追加したり、スペースが限られてして両方を結合します。
- **明確にマークの破壊的なボタン**- ボタン実行破壊された場合 (ファイルを削除する) などのアクションを明確にマーク テキストまたはアイコンを使用するようにします。 破壊的な操作は常に存在する必要があります、[アラート](~/ios/tvos/user-interface/alerts.md)アクションを工夫するユーザーかを確認します。
- **戻るボタンの使用をしない**-前の画面に戻るには Siri リモコンのメニュー ボタンを使用します。 アプリ内購入または破壊的な操作はこの規則に 1 つの例外です。 ここで、**キャンセル**ボタンが表示されます。

フォーカスおよびナビゲーションの使用の詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)ドキュメント。

<a name="Button-Icons" />

### <a name="button-icons"></a>ボタンのアイコン

Apple では、ボタンのアイコンには、単純な非常にわかりやすい画像を使用することをお勧めします。 過度に複雑なアイコンは、ハードをソファに部屋のテレビ画面に認識するを把握するために、全体で可能な最も単純な形式を使用するようにしてください。 可能な限り、標準を使用して、(検索の虫眼鏡) などのアイコン用イメージを well 知る。

<a name="Button-Titles" />

### <a name="button-titles"></a>ボタンのタイトル

Apple には、ボタンのタイトルを作成するときに、以下の推奨事項。

- **わかりやすいテキストの下のアイコンのボタンを表示する**- 可能な限り、アイコンの下、明確でわかりやすいテキストのみの各ボタンをさらに get 間でのボタンの用途。
- **タイトルの動詞または動詞句を使用して**-ユーザーがボタンをクリックしたときに、明確に状態のアクションが実行に配置します。
- **タイトル スタイルの大文字と小文字を使用して**- アーティクル、接続詞または前置詞を除き (4 文字以内)、ボタンのタイトルのすべての単語を大文字で入力する必要があります。
- **短い、ポイントにタイトルを使用して**-最短の考えられる文字化けしますを使用して、ボタンのアクションを説明します。

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>ボタンとストーリー ボード

Xamarin.tvOS アプリ内のボタンを使用する最も簡単な方法では、Xamarin デザイナーを使用して、iOS 用アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**ボタン**から、**ライブラリ**し、ビュー上にドロップします。 

    [![](buttons-images/storyboard01.png "ボタン")](buttons-images/storyboard01.png#lightbox)
1. **プロパティ エクスプ ローラー**などのボタンのいくつかのプロパティを調整することができます、**タイトル**と**テキストの色**: 

    [![](buttons-images/storyboard02.png "ボタンのプロパティ")](buttons-images/storyboard02.png#lightbox)
1. 次に、スイッチ、**イベント タブ**およびワイヤ アップ、**イベント**から、**ボタン**および呼び出し`ButtonPressed`: 

    [![](buttons-images/storyboard03.png "[イベント] タブ")](buttons-images/storyboard03.png#lightbox)
1. 自動的に切り替わりますが、`ViewController.cs`ビューを使用して、コードで新しいアクションを配置することができます、**を**と**ダウン**方向キー。 

    [![](buttons-images/storyboard04.png "コードで、新しいアクションを配置します。")](buttons-images/storyboard04.png#lightbox)
1. キーを押して、 **Enter**場所を選択します。 

    [![](buttons-images/storyboard05.png "コード エディター")](buttons-images/storyboard05.png#lightbox)
1. すべてのファイルに変更を保存します。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**ボタン**から、**ライブラリ**し、ビュー上にドロップします。 

    [![](buttons-images/storyboard01vs.png "ボタン")](buttons-images/storyboard01vs.png#lightbox)
1. **プロパティ エクスプ ローラー**などのボタンのいくつかのプロパティを調整することができます、**タイトル**と**テキストの色**: 

    [![](buttons-images/storyboard02vs.png "プロパティ エクスプ ローラー")](buttons-images/storyboard02vs.png#lightbox)
1. 次に、スイッチ、**イベント タブ**およびワイヤ アップ、**イベント**から、**ボタン**および呼び出し`ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "[イベント] タブ")](buttons-images/storyboard03vs.png#lightbox)
1. すべてのファイルに変更を保存します。



編集ビュー コント ローラー (例`ViewController.cs`) ファイルし、選択されているボタンを処理する次のコードを追加します。


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

ボタンの限り`Enabled`プロパティは`true`と別のコントロールまたはビューでは対応できない、Siri リモコンを使用して、フォーカス設定項目にできます。 ユーザーがボタンを選択して、タッチ画面をクリックすると、`ButtonPressed`上記で定義されたアクションが実行されます。

> [!IMPORTANT]
> **注:**などのアクションを割り当てることができますが`TouchUpInside`を`UIButton`iOS デザイナーを作成するときに、**イベント ハンドラー**、Apple TV はタッチ スクリーンやサポートがあるないためには呼び出すことはありませんタッチ イベント。 既定値を常に使用する必要があります**アクションの種類**作成するときに**アクション**の tvOS ユーザー インターフェイス要素です。




ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>ボタンとコード

必要に応じて、 `UIButton` c# コードで作成され、tvOS アプリのビューに追加されたことができます。 例:

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

新規に作成するときに`UIButton`を指定するコードでは、その`UIButtonType`として、次のいずれか。

- **システム**-tvOS によって提示されるボタンの標準の種類で、最も頻繁に使用する型。
- **DetailDisclosure** -詳細な情報を表示または非表示に使用されるボタンの「有効にする」の型を表示します。
- **InfoDark** -dark 詳細情報 ボタンは、円で囲んだ"i"を表示します。
- **InfoLight** -簡単な詳細情報 ボタンは、円で囲んだ"i"を表示します。
- **AddContact** -メンバーの追加ボタンとして、ボタンを表示します。
- **カスタム**- ボタンのいくつかの特性をカスタマイズすることができます。

次に、画面に表示されるサイズを定義し、ボタンの場所。 例:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

次に、ボタンのタイトルを設定します。 `UIButtons` ほとんどとは異なる`UIKit`コントロールにまったくことはできませんので、状態がある点では、タイトルを変更する必要がありますを変更するため、指定された`UIControlState`です。 例:

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

最後に、表示するビューに、ボタンを追加します。

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> **注:**などのアクションを割り当てることができますが`TouchUpInside`を`UIButton`、Apple TV はタッチ画面またはタッチ イベントのサポートがあるないためには呼び出すことはありません。 常にイベントを使用するように**AllEvents**または**PrimaryActionTriggered**です。




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>ボタンのスタイルを設定

tvOS のいくつかのプロパティを提供する、`UIButton`を使用して、タイトルを提供し、背景色と画像のようなものでスタイルを設定できます。

<a name="Button-Titles" />

### <a name="button-titles"></a>ボタンのタイトル

上記に説明したよう`UIButtons`ほとんどとは異なる`UIKit`コントロールにまったくことはできませんので、状態がある点では、タイトルを変更する必要がありますを変更するため、指定された`UIControlState`です。 例:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

使用して、ボタンのタイトルの色を設定することができます、`SetTitleColor`メソッドです。 例:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

タイトルを調整してシャドウを使用して、`SetTitleShadowColor`です。 例:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

変更するタイトルの影を設定することができます*Engraved*に*浮き出し*次のコードを使用して、ボタンが強調表示するとします。

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

さらに、ボタンのタイトルとして属性付きのテキストを使用することができます。 例:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>ボタン イメージ

A`UIButton`が付属しているイメージがあることができ、画像の背景として使用できます。

ボタンの背景画像を設定する、指定された`UIControlState`、次のコードを使用します。

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

設定、`AdjustsImageWhenHiglighted`プロパティを`true`ボタンが強調表示すると、明るいイメージを描画する (これは、既定値です)。 設定、`AdjustsImageWhenDisabled`プロパティを`true`ボタンが無効にすると、濃いほうのイメージを描画する (ここでも、これは、既定値)。

ボタンに表示されるイメージを設定するには、次のコードを使用します。

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

使用して、`TintColor`タイトルとボタンのイメージの両方に適用される色の濃淡を設定するプロパティです。 ボタン、`Custom`タイプ、このプロパティは影響を与えません、実装する必要があります、`TintColor`動作自分でします。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でのボタンの操作について説明しました。 C# コードでボタンを作成する方法と、iOS デザイナー内のボタンを使用する方法を示しました。 最後に、ボタンのタイトルを変更し、そのスタイルと外観を変更する方法を示しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
