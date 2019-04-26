---
title: Xamarin.iOS でのラベル
description: このドキュメントでは、Xamarin.iOS でラベルを使用する方法について説明します。 これには、プログラムを使用して、iOS Designer では、ラベルを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: cca74ac74e5077822193f6dd97a69f8d9b823561
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227831"
---
# <a name="labels-in-xamarinios"></a>Xamarin.iOS でのラベル

`UILabel`コントロールが単一および複数の行を表示するための読み取り専用のテキスト。 

## <a name="implementing-a-label"></a>ラベルを実装します。

新しいラベルがインスタンス化によって作成された、 [ `UILabel` ](xref:UIKit.UILabel):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>ラベルとストーリー ボード

IOS Designer を使用する場合に、UI ラベルを追加できます。 検索**ラベル**で、**ツールボックス**ビューにドラッグします。

![ツールボックス内のラベルします。](labels-images/image3.png)

プロパティ パッドで、次のプロパティを調整することができます。

![ラベルのプロパティ パネル](labels-images/image2.png)

- **テキストのコンテキスト**– プレーンまたは属性。 プレーン テキストでは、設定、[書式属性](#Formatting_Text_and_Label)文字列全体でします。 属性付きのテキストを別の文字または文字列内の単語の書式設定することができます。
- **色、フォント、アラインメント**– ラベルに適用できる属性の書式設定します。
- **行**– ラベルにまたがることができる行の数を設定します。 この設定を 0 に、必要に応じて多くの行を使用するラベルを許可します。
- **動作**– 有効] または [反転表示されたに設定することができます。 有効になっている既定の設定を無効になっているテキストは明るいグレー色で表示されます。 強調表示されている既定で無効にでき、ユーザーが選択されている強調表示された状態で再描画されるラベル。
- **Baselane と改行**– 
    - ベースラインは、テキストがされる方法を決定します。 フォント サイズが指定されたものと異なる場合に配置します。
    - 改行は、文字列のラップまたは 1 つの行より長い場合は切り捨てを決定します。
- **Autoshrink** – 必要に応じて、ラベル内サイズのフォントの最小化する方法を決定します。
- **強調表示、影のオフセット**: 強調表示されて、影の色を設定することができ、影のオフセットします。

## <a name="truncating-and-wrapping"></a>切り捨てと折り返し

行の使用方法については、iOS で区切りを参照してください、[切り捨てし、テキストを折り返す](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text)レシピです。

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>テキストとラベルの書式設定

ラベルで使用する文字列書式設定文字列全体の属性を書式設定するか、または属性付きの文字列を使用することができます。 次の例では、これらを実装する方法を示します。

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

スタイル設定を使用するテキストの詳細については`NSAttributedString`を参照してください、[テキストのスタイル設定](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text)レシピです。

ラベルが既定では、`Enabled`が true に設定に設定するには、可能な限り無効になっているユーザーに特定のコントロールが無効になっているヒントを指定します。

```csharp
label.Enabled = false;
```

IOS での制限の画面の次の図に示すように、ラベルを明るいグレー色では、この設定します。

![IOS で無効にされたボタン](labels-images/image1.png)

ラベル テキストの追加の効果をシャドウと強調表示のテキストの色を設定することもできます。

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

これには、このようなテキストが表示されます。

![テキストの強調表示とシャドウ設定](labels-images/image4.png)

UILabel のフォントを変更する方法の詳細についてを参照してください、 [、フォントの変更](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font)レシピです。





