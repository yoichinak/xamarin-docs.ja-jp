---
title: Xamarin. iOS のボタン
description: UIButton クラスは、iOS の画面のさまざまな異なるスタイルのボタンを表すために使用されます。 このガイドでは、iOS でボタンを操作するためのさまざまなオプションについて説明します。
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2018
ms.openlocfilehash: d763d4433ef0979343de059c5113314f553142fa
ms.sourcegitcommit: 01ccefd54c0ced724784dbe1aec9ecfc9b00e633
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92630219"
---
# <a name="buttons-in-xamarinios"></a>Xamarin. iOS のボタン

IOS では、 `UIButton` クラスはボタンコントロールを表します。

ボタンのプロパティは、プログラムで、または iOS デザイナーの [ **プロパティ] ウィンドウまたは [埋め込み]** で変更できます。

![IOS デザイナーの Properties Pad](buttons-images/properties.png "IOS デザイナーの Properties Pad")

## <a name="creating-a-button-programmatically"></a>プログラムによるボタンの作成

は `UIButton` 数行のコードでのみ作成できます。

- ボタンをインスタンス化し、その型を指定します。

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  ボタンの型は、プロパティによって指定され `UIButtonType` ます。

  - `UIButtonType.System` -汎用ボタン
  - `UIButtonType.DetailDisclosure` -詳細情報が使用可能かどうかを示します。通常、テーブル内の特定のアイテムに関する情報が表示されます。
  - `UIButtonType.InfoDark` -構成情報が使用可能かどうかを示します。濃い色
  - `UIButtonType.InfoLight` -構成情報が使用可能かどうかを示します。薄い色
  - `UIButtonType..AddContact` -連絡先を追加できることを示します。
  - `UIButtonType.Custom` -カスタマイズ可能なボタン

  さまざまなボタンの種類の詳細については、次を参照してください。
  
  - このドキュメントの「 [カスタムボタンの種類](#custom-button-types) 」セクション
  - [ボタンの種類](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons)のレシピ
  - Apple の [IOS ヒューマンインターフェイスガイドライン](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)。

- ボタンのサイズと位置を定義します。

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- ボタンのテキストを設定します。 `SetTitle`メソッドを使用します。これには、テキストと `UIControlState` ボタンの状態の値が必要です。

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```
  
  ボタンの状態の種類を次に示します。
  
  - `UIControlState.Normal`
  - `UIControlState.Highlighted`
  - `UIControlState.Disabled`
  - `UIControlState.Selected`
  - `UIControlState.Focused`
  - `UIControlState.Application`
  - `UIControlState.Reserved`
  
  ボタンのスタイル設定とそのテキストの設定の詳細については、次を参照してください。

  - このドキュメントの「 [ボタンのスタイルを](#styling-a-button) 設定する」セクション
  - [設定ボタンのテキスト](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text)レシピ。

## <a name="handling-a-button-tap"></a>ボタンのタップの処理

ボタンのタップに応答するには、ボタンのイベントのハンドラーを指定し `TouchUpInside` ます。

```csharp
myButton.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` は、使用可能な唯一のボタンイベントではありません。 `UIButton` はの子クラスで `UIControl` あり、さまざま [なイベント](xref:UIKit.UIControlEvent)を定義します。

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>IOS Designer を使用してボタンイベントハンドラーを指定する

**Properties Pad** の [ **イベント** ] タブを使用して、ボタンのさまざまなイベントのイベントハンドラーを指定します。

適切なイベントについて、新しいイベントハンドラーの名前を入力するか、一覧から1つを選択します。 この操作を行うと、ボタンのビューコントローラーのコードにイベントハンドラーが作成されます。

![Properties Pad の [イベント] タブ](buttons-images/image1.png "Properties Pad の [イベント] タブ")

## <a name="styling-a-button"></a>ボタンのスタイル設定

`UIButton` コントロールは、それぞれ値 (、、、など) によって指定されるさまざまな状態に存在することがあり `UIControlState` `Normal` `Disabled` `Focused` `Highlighted` ます。各状態には、プログラムまたは iOS デザイナーで指定した一意のスタイルを指定できます。

> [!NOTE]
> すべての値の完全な一覧については、「」を参照してください。 `UIControlState`[`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> ドキュメントを参照してください。

たとえば、タイトルの色と影の色を設定するには、次のようにし `UIControlState.Normal` ます。

```csharp
myButton.SetTitleColor(UIColor.White, UIControlState.Normal);
myButton.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

次のコードでは、ボタンのタイトルをとの属性付きの (定型) 文字列に設定してい `UIControlState.Normal` `UIControlState.Highlighted` ます。

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>カスタムボタンの種類

がのボタンには、 `UIButtonType` `Custom` 既定のスタイルはありません。 ただし、さまざまな状態のイメージを設定して、ボタンの外観を構成することはできます。

```csharp
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

ユーザーがボタンにタッチしているかどうかに応じて、次のいずれかの画像 ( `UIControlState.Normal` 、、およびの状態) が表示され `UIControlState.Highlighted` `UIControlState.Selected` ます。

![UIControlState](buttons-images/image22.png "UIControlState") 
![UIControlState](buttons-images/image23.png "UIControlState") 
![UIControlState](buttons-images/image24.png "UIControlState")

カスタムボタンの操作の詳細については、「 [ボタンレシピのイメージの使用」](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) を参照してください。
