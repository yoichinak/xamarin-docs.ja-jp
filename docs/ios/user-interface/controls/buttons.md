---
title: Xamarin.iOS のボタン
description: UIButton クラスは、iOS 画面のボタンのさまざまな異なるスタイルを表すために使用されます。 このガイドでは、iOS のボタンを使用するためのさまざまなオプションについて説明します。
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2018
ms.openlocfilehash: a98ddc2622682f2c105a6aff32e94bd92a5b11f2
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233251"
---
# <a name="buttons-in-xamarinios"></a>Xamarin.iOS のボタン

Ios で、`UIButton`クラスは、ボタン コントロールを表します。

プログラムから、または、ボタンのプロパティを変更できる、 **Properties Pad** iOS Designer の。

![IOS Designer の Properties Pad](buttons-images/properties.png "iOS Designer の Properties Pad")

## <a name="creating-a-button-programmatically"></a>ボタンをプログラムで作成します。

A`UIButton`のみ、わずか数行のコードで作成できます。

- ボタンのインスタンスを作成し、その型を指定します。

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  ボタンの種類が指定された、 `UIButtonType`:

  - `UIButtonType.System` 汎用-ボタン
  - `UIButtonType.DetailDisclosure` 詳細については、テーブル内の特定の項目の詳細については、通常の可用性を示します
  - `UIButtonType.InfoDark` -情報の構成の可用性をことを示します。暗い色
  - `UIButtonType.InfoLight` -情報の構成の可用性をことを示します。明るい色
  - `UIButtonType..AddContact` -連絡先を追加できることを示します
  - `UIButtonType.Custom` -カスタマイズ可能なボタン

  ボタンのさまざまな種類の詳細についてを参照してください。
  
  - [カスタム ボタンの種類](#custom-button-types)このドキュメントの「
  - [ボタンの種類](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons)レシピ
  - Apple の[iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)します。

- ボタンのサイズと位置を定義します。

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- ボタンのテキストを設定します。 使用して、`SetTitle`メソッドでは、テキストが必要ですが、および`UIControlState`値。

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  ボタンのスタイルを設定し、そのテキストを設定する方法の詳細についてを参照してください。

  - [ボタンをスタイル設定](#styling-a-button)このドキュメントの「
  - [ボタン設定テキスト](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text)レシピです。

## <a name="handling-a-button-tap"></a>ボタンのタップの処理

ボタンのハンドラーを提供するボタンのタップに対応する`TouchUpInside`イベント。

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` 使用できる唯一のボタン イベントはありません。 `UIButton` 子クラスは、`UIControl`を定義する[さまざまなイベント](xref:UIKit.UIControlEvent)します。

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>IOS Designer を使用して、ボタンのイベント ハンドラーを指定するには

使用して、**イベント**のタブ、 **Properties Pad**ボタンのさまざまなイベントのイベント ハンドラーを指定します。

適切なイベントには、新しいイベント ハンドラーの名前を入力するか、一覧から 1 つを選択します。 これを行うと、ボタンのビュー コント ローラーのコードにイベント ハンドラーが作成されます。

![Properties Pad の [イベント] タブ](buttons-images/image1.png "Properties Pad の [イベント] タブ")

## <a name="styling-a-button"></a>ボタンのスタイル設定

`UIButton` コントロールができるさまざまな種類の状態で存在する各で指定された、`UIControlState`値 – `Normal`、 `Disabled`、 `Focused`、`Highlighted`など。各状態には、プログラムから、または iOS Designer で指定された一意のスタイルを指定できます。

> [!NOTE]
> すべての完全な一覧については`UIControlState`を値で見て、 [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> ドキュメントです。

たとえば、タイトルの色と影の色を設定する`UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

次のコードの属性 (スタイル) 文字列にボタンのタイトルを設定する`UIControlState.Normal`と`UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>カスタム ボタンの種類

ボタン、`UIButtonType`の`Custom`ある既定のスタイルはありません。 ただし、そのさまざまな状態のイメージを設定してボタンの外観を構成することは。

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

次のイメージの 1 つとして表示するかどうかどうかは、ユーザーに触れると、ボタンには、によって (`UIControlState.Normal`、`UIControlState.Highlighted`と`UIControlState.Selected`状態、それぞれ)。

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted")
![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

カスタム ボタンの使用方法の詳細についてを参照してください、[ボタンのイメージを使用して](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button)レシピです。

