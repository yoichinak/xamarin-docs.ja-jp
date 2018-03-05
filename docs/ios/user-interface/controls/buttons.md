---
title: "ボタン"
description: "UIButton クラスは、iOS の画面でボタンのさまざまな種類のスタイルを表すために使用します。 このセクションでは、iOS でのボタンを使用するためのさまざまなオプションについて説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: ab7a77c44412bd22427e17a696eb43278ff7dd94
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="buttons"></a>ボタン

_UIButton クラスは、iOS の画面でボタンのさまざまな種類のスタイルを表すために使用します。このセクションでは、iOS でのボタンを使用するためのさまざまなオプションについて説明します。_

`UIButton`クラスは、iOS でボタン コントロールを表します。 

ボタンのプロパティを編集することができます、 `Properties Pad` iOS デザイナーの。


![](buttons-images/properties.png "IOS デザイナーのプロパティの埋め込み")

## <a name="creating-a-button"></a>ボタンを作成します。

のみ、数行のコード経由で、UIButton を作成できます。

最初に、新しいボタンをインスタンス化し、必要があるボタンの種類を指定します。

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

次の 1 つとして、UIButtonType を指定する必要があります。

- **システム**-これは、iOS で使用される標準のボタンの種類は、最も頻繁に使用する型。
- **DetailDisclosure** -詳細な情報を表示または非表示に使用されるボタンの「有効にする」の型を表示します。
- **InfoDark** -dark 詳細情報 ボタンは、円で囲んだ"i"を表示します。
- **InfoLight** -簡単な詳細情報 ボタンは、円で囲んだ"i"を表示します。
- **AddContact** -メンバーの追加ボタンとして、ボタンを表示します。
- **カスタム**- ボタンのいくつかの特性をカスタマイズすることができます。

ボタンの種類の詳細については含まれて、[ボタンの種類](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/)レシピします。

次に、画面に表示されるサイズを定義し、ボタンの場所。 例:

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

ボタンのテキストを変更するには、使用、`SetTitle`プロパティ、ボタンのテキスト文字列を設定する必要があります、`UIControlStyle`です。 例:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

詳細については、ユーザーの (通信に状態のそれぞれに異なるプロパティを設定できます。 作成するテキストの色無効状態の灰色)。 IOS デザイナーを使用して各状態の間で切り替えることができます、またはプログラムによって行うことができます。 ボタンのテキストに設定し、状態の詳細についてを参照してください、[設定 ボタンのテキスト](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/)レシピします。

## <a name="dealing-with-user-interactions"></a>ユーザーの操作を処理します。


ボタンがクリックされたときに何かそうでない限りいない非常に便利です! 

ボタンのイベントは、ほとんどの場合、iOS で使用がこれを変更することによって、画面上のボタンと対話するので、イベントをタッチします。 すべての可能な < イベントの一覧が表示されます[ここ](https://developer.apple.com/documentation/uikit/uicontrolevents)、iOS で最もよく使用されるイベントですが、`TouchUpInside`です。 処理を行う、ボタンが押された後、イベント ハンドラーを作成できます。


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>IOS デザイナー内でイベントを追加します。
 
コントロールにイベントを追加するのにプロパティ パッドで、[イベント] タブを使用します。

イベントを選択し、新しいイベント ハンドラーまたは一覧からいずれかの選択の名前を入力するか。 これを行うと、ビューのコント ローラー クラスに新しい部分メソッドが作成されます。

![[イベント] タブ](buttons-images/image1.png)

## <a name="styling-a-button"></a>ボタンのスタイルを設定

UIButtons はほとんど UIKit 制御単タイトルを変更することはできませんので、状態がある点で異なって、ごとに変更する必要がある`UIControlState`です。 タイトルの色と影の色の設定は、同様の方法で行います。

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

さらに、ボタンのタイトルとして属性付きのテキストを使用することができます。 例:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>カスタム ボタンの種類


設定すると、`Custom`ボタンの種類、オブジェクトには、既定のレンダリングがありません。 ボタンの外観を構成するには、さまざまな状態のイメージを設定します。 たとえば、次のコードには、ごとに異なるイメージを追加する方法を示しています。、 `Normal`、`Highlighted`と`Selected`状態。


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


かどうかユーザーがボタンか、触れてに応じてにスクリプトが表示は次のイメージの 1 つとして (`Normal`、`Highlighted`と`Selected`それぞれを示します)。


![](buttons-images/image22.png "UIButton 状態が 正常")
![](buttons-images/image23.png "強調表示されている UIButton 状態")
![](buttons-images/image24.png "UIButton 状態の選択")

カスタム ボタンの操作の詳細についてを参照してください、[ボタンのイメージを使用して](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/)です。


## <a name="related-links"></a>関連リンク

- [UIButton ブック](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
