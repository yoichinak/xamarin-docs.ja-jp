---
title: "ラベル"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 0fdeecc4465aa5709b452ef0b591ec4e5c262e3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="labels"></a>ラベル

`UILabel`コントロールが単一および複数の行を表示するための読み取り専用のテキスト。 

## <a name="implementing-a-label"></a>ラベルを実装します。

新しいラベルがインスタンス化して作成された、 [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>ラベルとストーリー ボード

IOS デザイナーを使用する場合を UI にラベルを追加することもできます。 検索**ラベル**で、**ツールボックス**し、ビューにドラッグします。

![[ツールボックス] 内のラベルします。](labels-images/image3.png)

プロパティ パッド上、次のプロパティを調整できます。

![ラベルのプロパティ パネル](labels-images/image2.png)

- **テキストのコンテキスト**-プレーン テキストまたは属性です。 プレーン テキストでは、設定することができます、[書式属性](#Formatting_Text_and_Label)文字列全体にします。 属性付きのテキストでは、別の文字または文字列内の単語に書式を設定することができます。
- **色、フォント、アラインメント**– のラベルに適用できる書式設定の属性です。
- **行**– 行数を設定、ラベルにまたがることができます。 この設定を必要に応じて多くの行を使用するラベルを許可する 0 にします。
- **動作**– 有効] または [強調表示のいずれかに設定することができます。 有効になっている既定の設定、無効なテキストが表示されます明るいグレー色にします。 強調表示されている既定で無効にでき、ユーザーが選択されている強調表示された状態で再描画されるラベルです。
- **Baselane と改行**– 
    - ベースラインは、テキストがされる方法を決定フォント サイズが指定されたものと異なる場合に配置します。
    - 改行は、文字列のラップまたはも 1 つの行を超える場合は切り捨てを決定します。
- **Autoshrink** – 必要に応じて、ラベル内で、フォント サイズを最小化する方法を決定します。
- **強調表示されている、影のオフセット**– 強調表示されて、影の色を設定することができ、影のオフセットします。

## <a name="truncating-and-wrapping"></a>切り捨てとラッピング

行の使用の詳細については、iOS で区切りを参照してください、 [Truncate およびテキストを折り返す](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/)レシピします。

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>テキストとラベルの書式設定

ラベルに使用する文字列の書式設定を設定するか、書式文字列全体で属性ことも属性付きの文字列を使用することができます。 次の例では、これらを実装する方法を示します。

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

スタイル設定のテキストを使用する方法について`NSAttributedString`を参照してください、[テキストのスタイル設定](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/)レシピします。

ラベルが既定では、`Enabled`は true に設定に設定するには、可能な限り無効になっているユーザーに提供する、特定のコントロールが無効になっているヒント。

```csharp
label.Enabled = false;
```

IOS での制限の画面の次の例の画像に示すように、ラベルを明るいグレー色では、この設定します。

![IOS で無効にされたボタン](labels-images/image1.png)

他の効果のラベル テキストにシャドウと強調表示テキストの色を設定することもできます。

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

これには、次のようにテキストが表示されます。

![シャドウと強調表示をテキストに設定](labels-images/image4.png)

UILabel のフォントを変更する方法の詳細についてを参照してください、 [、フォントの変更](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/)レシピします。





