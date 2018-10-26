---
title: Xamarin.iOS でのアラートの表示
description: このドキュメントでは、Xamarin.iOS で UIAlertController iOS 8 で導入された Api を使用してアラートを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 6071381daa7eedf4fa4b076ea60f2748865cf002
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109641"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin.iOS でのアラートの表示

IOS 8 以降、UIAlertController が完成した置換 UIActionSheet とうち UIAlertView 両方が非推奨となりました。

UIAlertController とは異なり、このクラスに置き換え、UIView のサブクラスであり、UIViewController のサブクラスです。

使用`UIAlertControllerStyle`を表示するアラートの種類を示します。 これらのアラートの種類は次のとおりです。

- **UIAlertControllerStyleActionSheet**
    * 前の iOS 8 を UIActionSheet をされたこれは
- **UIAlertControllerStyleAlert**
    * 前の iOS 8 この UIAlertView にされている場合 

アラートのコント ローラーの作成時に実行するために必要な 3 つの手順があります。

- 作成し、アラートを a: 構成
    * タイトル
    * message
    * preferredStyle
    
- (省略可能)テキスト フィールドを追加します。
- 必要なアクションを追加します。
- ビュー コント ローラーを表示します。

最も単純なアラートには、このスクリーン ショットで示すように 1 つのボタンが含まれています。

 ![1 つのボタンをアラートします。](alerts-images/alert1.png)

単純なアラートを表示するコードは次のとおりです。

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

複数のオプションを使用して通知を表示する同様の方法で行われますが、2 つのアクションを追加します。 たとえば、次のスクリーン ショットでは、2 つのボタンでアラートが表示されます。

 ![ 2 つのボタンをアラートします。](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

アラートは、次のスクリーン ショットのようなアクション シートを表示もできます。

 ![アクション シートのアラート](alerts-images/alert3.png)

ボタンのアラートに追加されて、`AddAction`メソッド。

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
- [アラートのコント ローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
