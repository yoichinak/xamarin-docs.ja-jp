---
title: Xamarin. iOS のボタン
description: UIButton クラスは、iOS の画面のさまざまな異なるスタイルのボタンを表すために使用されます。 このガイドでは、iOS でボタンを操作するためのさまざまなオプションについて説明します。
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/11/2018
ms.openlocfilehash: ce0c4579f13311811106a00390f95a20a0abf979
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768448"
---
# <a name="buttons-in-xamarinios"></a>Xamarin. iOS のボタン

IOS では、 `UIButton`クラスはボタンコントロールを表します。

ボタンのプロパティは、プログラムによって、または iOS デザイナーの**Properties Pad**で変更できます。

![IOS デザイナーの Properties Pad](buttons-images/properties.png "IOS デザイナーの Properties Pad")

## <a name="creating-a-button-programmatically"></a>プログラムによるボタンの作成

は`UIButton`数行のコードでのみ作成できます。

- ボタンをインスタンス化し、その型を指定します。

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  ボタンの型は、 `UIButtonType`によって指定されます。

  - `UIButtonType.System`-汎用ボタン
  - `UIButtonType.DetailDisclosure`-詳細情報が使用可能かどうかを示します。通常、テーブル内の特定のアイテムに関する情報が表示されます。
  - `UIButtonType.InfoDark`-構成情報が使用可能かどうかを示します。濃い色
  - `UIButtonType.InfoLight`-構成情報が使用可能かどうかを示します。薄い色
  - `UIButtonType..AddContact`-連絡先を追加できることを示します。
  - `UIButtonType.Custom`-カスタマイズ可能なボタン

  さまざまなボタンの種類の詳細については、次を参照してください。
  
  - このドキュメントの「[カスタムボタンの種類](#custom-button-types)」セクション
  - [ボタンの種類](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons)のレシピ
  - Apple の[IOS ヒューマンインターフェイスガイドライン](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)。

- ボタンのサイズと位置を定義します。

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- ボタンのテキストを設定します。 次のように、テキストと値を`UIControlState`必要とするメソッドを使用します。`SetTitle`

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  ボタンのスタイル設定とそのテキストの設定の詳細については、次を参照してください。

  - このドキュメントの「[ボタンのスタイルを](#styling-a-button)設定する」セクション
  - [設定ボタンのテキスト](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text)レシピ。

## <a name="handling-a-button-tap"></a>ボタンのタップの処理

ボタンのタップに応答するには、ボタンの`TouchUpInside`イベントのハンドラーを指定します。

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside`は、使用可能な唯一のボタンイベントではありません。 `UIButton`はの`UIControl`子クラスであり、さまざま[なイベント](xref:UIKit.UIControlEvent)を定義します。

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>IOS Designer を使用してボタンイベントハンドラーを指定する

**Properties Pad**の **[イベント]** タブを使用して、ボタンのさまざまなイベントのイベントハンドラーを指定します。

適切なイベントについて、新しいイベントハンドラーの名前を入力するか、一覧から1つを選択します。 この操作を行うと、ボタンのビューコントローラーのコードにイベントハンドラーが作成されます。

![Properties Pad の [イベント] タブ](buttons-images/image1.png "Properties Pad の [イベント] タブ")

## <a name="styling-a-button"></a>ボタンのスタイル設定

`UIButton`コントロールは`UIControlState` 、それぞれ`Normal`値`Disabled` (、、`Highlighted`、など) によって指定されるさまざまな状態に存在することがあります。 `Focused`各状態には、プログラムまたは iOS デザイナーで指定した一意のスタイルを指定できます。

> [!NOTE]
> すべて`UIControlState`の値の完全な一覧については、「」を参照してください。[`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> 書.

たとえば、タイトルの色と影の色`UIControlState.Normal`を設定するには、次のようにします。

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

次のコードでは、ボタンのタイトルをと`UIControlState.Normal` `UIControlState.Highlighted`の属性付きの (定型) 文字列に設定しています。

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>カスタムボタンの種類

がのボタン`UIButtonType`に`Custom`は、既定のスタイルはありません。 ただし、さまざまな状態のイメージを設定して、ボタンの外観を構成することはできます。

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

ユーザーがボタンにタッチしているかどうかに応じて、次のいずれかの画像 (`UIControlState.Normal`、 `UIControlState.Highlighted` 、 `UIControlState.Selected`およびの状態) が表示されます。

![UIControlState](buttons-images/image22.png "UIControlState")![](buttons-images/image23.png "UIControlState UIControlState")![]UIControlState(buttons-images/image24.png "UIControlState. selected") .
 


カスタムボタンの操作の詳細については、「[ボタンレシピのイメージの使用」](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button)を参照してください。
