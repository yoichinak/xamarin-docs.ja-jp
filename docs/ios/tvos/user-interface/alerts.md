---
title: Xamarin で tvOS アラートの使用
description: このドキュメントでは、Xamarin で tvOS アラートを操作する方法について説明します。 これは、テキスト フィールド、およびヘルパー クラスを追加する、アラートの表示について説明します。
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b5125f150a4d57ed27041da2944f4c161434cf93
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789084"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Xamarin で tvOS アラートの使用

_この記事では、Xamarin.tvOS でユーザーに警告メッセージを表示する UIAlertController の扱いについて説明します。_

使用して、警告メッセージを表示するには、tvOS のユーザーの注目を取得または (ファイルを削除する) などの破壊的な操作を実行するアクセス許可を依頼する必要がある場合、 `UIAlertViewController`:

[![](alerts-images/alert01.png "たとえば UIAlertViewController")](alerts-images/alert01.png#lightbox)

かどうかのメッセージの表示をさらに、追加できますボタンとテキスト フィールドを操作に応答し、フィードバックを提供できるようにするアラートにします。

<a name="About-Alerts" />

## <a name="about-alerts"></a>通知について

前述のように、ユーザーの注意を引くために連絡して、アプリまたは要求に関するフィードバックの状態のアラートが使用されます。 アラートは、タイトルを表示する必要があります、できます必要に応じてがメッセージ 1 つまたは複数のボタンやテキスト フィールド。

[![](alerts-images/alert04.png "例アラート")](alerts-images/alert04.png#lightbox)

Apple では、アラートの操作の次の方法があります。

- **多用しないアラート**-アラートは、アプリのユーザーのフローが中断されると、ユーザー エクスペリエンスを中断し、ような場合、エラー通知、アプリ内購入および破壊的な操作などの重要な場合にのみ使用する必要があります。 
- **便利な選択肢を提供**-、アラート、ユーザーにオプションを表示する場合、各オプションが重要な情報を提供し、ユーザーが実行するための便利なアクションは、ことを確認する必要があります。

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>アラートのタイトルとメッセージ

Apple では、アラートのタイトルと省略可能なメッセージを表すための以下の推奨事項があります。

- **Multiword タイトルを使用する**-警告のタイトルは、単純なが残っているときに明確に間で、この状況のポイントを取得する必要があります。 1 つの単語のタイトルは、まれに十分な情報を提供します。
- **メッセージを必要としないわかりやすいタイトルを使用する**- 可能な限り、アラートのタイトルの説明は省略可能なメッセージのテキストを表示するために必要なことを検討します。 
- **メッセージをようにするには、短い、完全な文**- 省略可能なメッセージが全体で、アラートのポイントを取得し、できるだけ単純にする、適切な大文字と小文字および句読点と完全な文章を行う必要がある場合。

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>アラートのボタン

Apple では、アラートにボタンを追加するため、次の修正案があります。

- **2 つのボタンに制限**可能な限り、-、最大で 2 つのボタンのアラートを制限します。 1 つのボタン アラートは、情報がアクションを提供しません。 2 つのボタン アラート提供、単純なアクションのはい/いいえの選択肢です。
- **Succinct、論理ボタン タイトルを使用して**-単純なボタンのアクションを明確に記述する 1 ~ 2 ワード ボタン タイトルが最適に動作します。 詳細については、次を参照してください。 この[ボタンの](~/ios/tvos/user-interface/buttons.md)ドキュメント。
- **明確にマークの破壊的なボタン**– (ファイルを削除する) などの破壊的な操作を実行するボタン マークすることで、`UIAlertActionStyle.Destructive`スタイル。

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>通知を表示します。

インスタンスを作成するアラートを表示する、`UIAlertViewController`アクション (ボタン) を追加して、アラートのスタイルを選択して構成します。 たとえば、次のコードでは、[ok]/[キャンセル] アラートが表示されます。

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

このコードを見るを詳しく見てみましょう。 最初に、新しいアラートを指定されたタイトルとメッセージを使用して作成します。

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

次に、アラートの表示にする各ボタンのボタン、そのスタイル、およびボタンが押された場合に実行するアクションのタイトルを定義するアクションを作成します。

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle`列挙型が、次の 1 つとして、ボタンのスタイルを設定することを許可します。

- **既定**- ボタンは、アラートが表示されたら、選択されている既定のボタンになります。
- **キャンセル**- ボタンは、アラートの キャンセル ボタンをクリックします。
- **破壊的な**-ファイルを削除するなど、破壊的な操作として、ボタンを強調表示します。 現在、tvOS は、背景が赤の破壊的なボタンをレンダリングします。

`AddAction`メソッドに指定されたアクションの追加、`UIAlertViewController`し、最後に、`PresentViewController (alertController, true, null)`メソッドは、ユーザーに指定された警告を表示します。

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>テキスト フィールドを追加します。

に、アラートをアクション (ボタン) を追加するだけでなく、この警告を使用するユーザー Id やパスワードなどの情報を入力するユーザーにテキスト フィールドを追加できます。

[![](alerts-images/alert02.png "アラートのテキスト フィールド")](alerts-images/alert02.png#lightbox)

ユーザーは、テキスト フィールドを選択する場合、フィールドの値を入力すること、tvOS の標準キーボードが表示されます。

[![](alerts-images/alert03.png "テキストを入力します。")](alerts-images/alert03.png#lightbox)

次のコードは、値を入力するため、[ok]/[キャンセル] アラートを 1 つのテキスト フィールドが表示されます。

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField`メソッドは、テキスト (フィールドが空のときに表示されるテキスト)、既定のテキスト値とキーボードの種類など、プレース ホルダーのプロパティを設定して構成することができますし、アラートに新しいテキスト フィールドを追加します。 例えば:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

おは、後で、テキスト フィールドの値を操作できる、ようにおの次のコードを使用してコピーも保存します。

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

使用して、ユーザーがテキスト フィールドに値を入力した後、`field`その値にアクセスする変数。

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>アラート ビュー コント ローラーのヘルパー クラス

使用してアラートの種類の単純な一般的な表示ため`UIAlertViewController`相当量の重複したコードで発生することができます、繰り返し出現するコードの量を削減するためのヘルパー クラスを使用することができます。 例えば:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

このクラスを使用して、表示、および単純なアラートに対応して、次のように実行できます。

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、操作をカバーされて`UIAlertController`Xamarin.tvOS でユーザーに警告メッセージを表示します。 最初に、単純なアラートを表示し、ボタンを追加する方法を示しました。 次に、アラートをテキスト フィールドを追加する方法を示しました。 最後に、アラートを表示するために必要な繰り返し出現するコードの量を削減するためのヘルパー クラスを使用する方法を示しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
