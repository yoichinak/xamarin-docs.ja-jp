---
title: Xamarin での tvOS アラートの使用
description: このドキュメントでは、Xamarin で tvOS アラートを使用する方法について説明します。 ここでは、アラートの表示、テキストフィールドの追加、およびヘルパークラスについて説明します。
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 76a9af2a3d845ce3f93b02358901cda8d9d02294
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030514"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Xamarin での tvOS アラートの使用

_この記事では、UIAlertController を使用して tvOS のユーザーに警告メッセージを表示する方法について説明します。_

TvOS ユーザーに注意を喚起する必要がある場合、または破壊的な操作 (ファイルの削除など) を実行するアクセス許可を要求する場合は、`UIAlertViewController`を使用して警告メッセージを表示できます。

[![](alerts-images/alert01.png "An example UIAlertViewController")](alerts-images/alert01.png#lightbox)

メッセージを表示するだけでなく、ボタンやテキストフィールドを警告に追加して、ユーザーがアクションに応答してフィードバックを提供できるようにすることもできます。

<a name="About-Alerts" />

## <a name="about-alerts"></a>アラートについて

前述のように、アラートを使用してユーザーの注意を受け取り、アプリの状態またはフィードバックの要求を通知します。 アラートにはタイトルを指定する必要があります。また、必要に応じて、メッセージと1つ以上のボタンまたはテキストフィールドを含めることができます。

[![](alerts-images/alert04.png "An example alert")](alerts-images/alert04.png#lightbox)

Apple では、アラートの使用に関して次のような提案があります。

- **アラートを控えめに使用する**-アラートはアプリとのユーザーのフローを中断し、ユーザーエクスペリエンスを中断します。そのため、エラー通知、アプリ内購入、破壊的な操作などの重要な状況でのみ使用してください。
- **便利な選択肢を提供**します。アラートによってユーザーにオプションが表示される場合は、各オプションが重要な情報を提供し、ユーザーが実行するのに役立つアクションを提供する必要があります。

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>アラートのタイトルとメッセージ

Apple では、アラートのタイトルとオプションのメッセージを表示するための次の提案があります。

- **複数の単語のタイトルを使用する**-アラートのタイトルは、単純なままで、状況を明確に把握する必要があります。 1つの単語のタイトルでは、十分な情報が得られない場合があります。
- **メッセージを必要としない説明のタイトルを使用**する: 可能な場合は、警告のタイトルに必要なメッセージテキストが不要であることを確認してください。
- **メッセージを短い、完全な文**にします。アラートのポイントを取得するためにオプションのメッセージが必要な場合は、できるだけ単純なものにして、大文字と小文字を正しく設定した完全な文にします。

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>アラートボタン

Apple では、アラートにボタンを追加する際に次のことを提案しています。

- **2 つのボタンに制限する**: 可能な限り、アラートを最大2つのボタンに制限します。 1つのボタンのアラートは情報を提供しますが、アクションは提供しません。 2つのボタンのアラートは、単純な [はい]、[いいえ] のいずれかのアクションを提供します。
- **簡潔で論理的なボタンタイトルを使用**します。ボタンのアクションが最適であることを明確に説明する2つの単語ボタンのタイトルです。 詳細については、[ボタンの操作に](~/ios/tvos/user-interface/buttons.md)関するドキュメントを参照してください。
- **明確に破壊ボタンをマーク**する-破壊的な操作 (ファイルの削除など) を実行するボタンの場合は、`UIAlertActionStyle.Destructive` スタイルで明示的にマークします。

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>アラートの表示

アラートを表示するには、`UIAlertViewController` のインスタンスを作成し、アクション (ボタン) を追加して警告のスタイルを選択して構成します。 たとえば、次のコードでは、[OK] および [キャンセル] アラートが表示されます。

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

このコードを詳しく見てみましょう。 まず、指定されたタイトルとメッセージを使用して新しいアラートを作成します。

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

次に、アラートに表示する各ボタンについて、ボタンのタイトル、スタイル、およびボタンが押されたときに実行するアクションを定義するアクションを作成します。

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ =>
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle` 列挙型を使用すると、次のいずれかの方法でボタンのスタイルを設定できます。

- **[既定]** : このボタンは、警告が表示されたときに選択された既定のボタンになります。
- **[キャンセル**]: このボタンは、アラートの [キャンセル] ボタンです。
- **破壊的**-ファイルの削除など、破壊的なアクションとしてボタンを強調表示します。 現在、tvOS は、背景が赤の破壊的ボタンをレンダリングします。

`AddAction` メソッドは、指定されたアクションを `UIAlertViewController` に追加し、最後に `PresentViewController (alertController, true, null)` メソッドによって、指定された警告をユーザーに表示します。

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>テキストフィールドの追加

アラートにアクション (ボタン) を追加するだけでなく、テキストフィールドをアラートに追加して、ユーザーがユーザー Id やパスワードなどの情報を入力できるようにすることができます。

[![](alerts-images/alert02.png "Text Field in an alert")](alerts-images/alert02.png#lightbox)

ユーザーがテキストフィールドを選択すると、標準の tvOS キーボードが表示され、フィールドの値を入力できます。

[![](alerts-images/alert03.png "Entering text")](alerts-images/alert03.png#lightbox)

次のコードでは、値を入力するための1つのテキストフィールドを含む、OK/キャンセルアラートを表示します。

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

`AddTextField` メソッドは、新しいテキストフィールドを警告に追加します。このフィールドは、プレースホルダーテキスト (フィールドが空のときに表示されるテキスト)、既定のテキスト値、キーボードの種類などのプロパティを設定することによって構成できます。 (例:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

後でテキストフィールドの値を操作できるように、次のコードを使用してのコピーも保存します。

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

ユーザーがテキストフィールドに値を入力した後、`field` 変数を使用してその値にアクセスできます。

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>アラートビューコントローラーヘルパークラス

`UIAlertViewController` を使用してシンプルで一般的な種類のアラートを表示すると、コードが重複して生成される可能性があるため、ヘルパークラスを使用して繰り返しコードの量を減らすことができます。 (例:

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

このクラスを使用すると、次のように単純な警告の表示と応答を行うことができます。

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

この記事では、`UIAlertController` を使用して、Xamarin. tvOS のユーザーにアラートメッセージを表示する方法について説明しました。 まず、簡単なアラートを表示し、ボタンを追加する方法を示しました。 次に、テキストフィールドを警告に追加する方法について説明しました。 最後に、ヘルパークラスを使用して、アラートを表示するために必要な反復的なコードの量を減らす方法を説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
