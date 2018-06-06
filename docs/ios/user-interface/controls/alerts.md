---
title: Xamarin.iOS にアラートを表示します。
description: このドキュメントでは、Xamarin.iOS UIAlertController iOS 8 で導入された Api を使用して、アラートを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 88233cb1ac31b2669fdc38bbc9b0835a45c6b0ce
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789597"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin.iOS にアラートを表示します。

IOS 8 以降、UIAlertController が完了した交換 UIActionSheet とうち UIAlertView 両方は使用されなくなりました。

UIAlertController とは異なり、このクラスに置き換え、UIView のサブクラスであり、UIViewController のサブクラスです。

使用して`UIAlertControllerStyle`を表示するアラートの種類を指定します。 これらのアラートの種類は次のとおりです。

- **UIAlertControllerStyleActionSheet**
    * 前の iOS 8 これが、UIActionSheet
- **UIAlertControllerStyleAlert**
    * 前の iOS 8 これが見込めた UIAlertView 

アラートのコント ローラーを作成するときに実行するために必要な手順は次の 3 つです。

- 作成し、構成アラート a:
    * タイトル
    * message
    * preferredStyle
    
- (省略可能)テキスト フィールドを追加します。
- 必要なアクションを追加します。
- ビューのコント ローラーを表示します。

最も単純なアラートには、このスクリーン ショットに示すように 1 つのボタンが含まれています。

 ![アラートを 1 つのボタン](alerts-images/alert1.png)

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

複数のオプションでは、通知を表示する同様の方法で行われますが、2 つのアクションを追加します。 たとえば、次のスクリーン ショットは、2 つのボタンのアラートを示します。

 ![ 2 つのボタンを持つアラートします。](alerts-images/alert2.png)

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

 ![アクションのシートのアラート](alerts-images/alert3.png)

ボタンのアラートに追加されて、`AddAction`メソッド。

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

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
- [アラートのコント ローラー](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
