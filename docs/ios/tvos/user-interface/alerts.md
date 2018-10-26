---
title: Xamarin で tvOS のアラートの使用
description: このドキュメントでは、Xamarin で tvOS のアラートを操作する方法について説明します。 これは、テキスト フィールド、およびヘルパー クラスを追加、アラートの表示について説明します。
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 13c5ae3fac76ec1ec1a0ade135d5919403066226
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111292"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Xamarin で tvOS のアラートの使用

_この記事では UIAlertController Xamarin.tvOS でユーザーに警告メッセージを表示する操作について説明します。_

使用して、警告メッセージを表示するには tvOS ユーザーの注意を引くか (ファイルを削除する) などの破壊的な操作を実行するアクセス許可を依頼する必要がある場合、 `UIAlertViewController`:

[![](alerts-images/alert01.png "UIAlertViewController 例")](alerts-images/alert01.png#lightbox)

かどうか、メッセージを表示するときにさらに、追加できますボタンとテキスト フィールドの操作に応答し、フィードバックを提供するユーザーを許可するためにアラートにします。

<a name="About-Alerts" />

## <a name="about-alerts"></a>アラートについて

前述のように、アラートを使用して、ユーザーの注意を引くし、通知するアプリまたは要求に関するフィードバックの状態のことをします。 アラートは、タイトルを表示する必要があります、メッセージと 1 つ以上のボタンまたはテキスト フィールドがあることができます必要に応じて。

[![](alerts-images/alert04.png "アラートの例")](alerts-images/alert04.png#lightbox)

Apple では、アラートを操作するための次の推奨事項があります。

- **控えめにアラートを使用して、** -アラートは、アプリのユーザーのフローが中断されるとユーザー エクスペリエンスを中断しなど、エラー通知、アプリ内購入破壊的な行為など重要な場合にのみ使用する必要があります。 
- **便利な選択肢を提供**- アラートは、ユーザーにオプションを提示している場合、各オプションが重要な情報を提供し、実行するユーザーの役に立つアクションを提供するようにする必要があります。

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>アラートのタイトルとメッセージ

Apple では、アラートのタイトルと省略可能なメッセージを表示するための次の推奨事項があります。

- **Multiword タイトルを使用する**-アラートのタイトルは、単純なが残っているときに明確にで、状況のポイントを取得する必要があります。 1 つの単語のタイトルは、ほとんど十分な情報を提供します。
- **メッセージを必要としない、わかりやすいタイトルを使用する**- 可能な限り、アラートのタイトルの説明は省略可能なメッセージのテキストを表示するために必要なことを検討します。 
- **メッセージをようにするには、Short、完全な文**- 省略可能なメッセージが、ポイントの間でアラートを取得し、できるだけ単純にする、適切な大文字と小文字および句読点で完全な文をように必要な場合は。

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>アラートのボタン

Apple では、アラートにボタンを追加するための次の提案があります。

- **2 つのボタンに制限**- 可能な限り、2 つのボタンの最大値にアラートを制限します。 1 つのボタン アラートは、情報がアクションを提供しません。 2 つのボタンのアラートの提供、単純なアクションのはい/いいえの選択肢です。
- **Succinct、論理ボタンのタイトルを使用して、** -単純なボタンのアクションを明確に記述する 1 ~ 2 ワード ボタンのタイトルが最適に動作します。 詳細については、次を参照してください。 この[ボタンの](~/ios/tvos/user-interface/buttons.md)ドキュメント。
- **明確にマークの破壊的なボタン**- (ファイルを削除する) などの破壊的な操作を実行するボタンが明確にでそれらをマークするために、`UIAlertActionStyle.Destructive`スタイル。

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>通知を表示します。

インスタンスを作成するアラートを表示する、`UIAlertViewController`とアクション (ボタン) を追加して、アラートのスタイルを選択してアプリケーションを構成します。 たとえば、次のコードでは、[ok]/[キャンセル] アラートが表示されます。

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

詳細にこのコードを見てをみましょう。 最初に、指定されたタイトルとメッセージの新しいアラートを作成します。

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

次に、各アラートに表示するボタンのボタン、そのスタイルとボタンが押された場合に実行するアクションのタイトルを定義するアクションを作成します。

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle`列挙型が、次のいずれかとして、ボタンのスタイルを設定することを許可します。

- **既定**-、ボタンは、アラートが表示されるときに選択されている既定のボタンになります。
- **キャンセル**-ボタンは、アラートの [キャンセル] ボタンをクリックします。
- **破壊的な**-ファイルを削除するなどの破壊的な操作として、ボタンが強調表示されます。 現時点では、tvOS では、背景が赤の破壊的なボタンをレンダリングします。

`AddAction`メソッドに指定されたアクションの追加、`UIAlertViewController`と最後に、`PresentViewController (alertController, true, null)`メソッドが、ユーザーに指定された警告を表示します。

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>テキスト フィールドを追加します。

アラートにアクション (ボタン) を追加するだけでは、ユーザー Id やパスワードなどの情報を入力するユーザーを許可するアラートをテキスト フィールドを追加できます。

[![](alerts-images/alert02.png "アラートのテキスト フィールド")](alerts-images/alert02.png#lightbox)

ユーザーは、テキスト フィールドを選択する場合、フィールドの値を入力することができます標準 tvOS キーボードが表示されます。

[![](alerts-images/alert03.png "テキストを入力します。")](alerts-images/alert03.png#lightbox)

次のコードは、値を入力するため、1 つのテキスト フィールドで [ok]/[キャンセル]、アラートが表示されます。

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

`AddTextField`メソッドは、テキスト (フィールドが空のときに表示されるテキスト)、既定のテキスト値、およびキーボードの種類など、プレース ホルダーのプロパティを設定して構成することができますし、アラートに新しいテキスト フィールドを追加します。 例えば:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

私たちは、後で、テキスト フィールドの値を操作できる、ように次のコードを使用してのコピーも節約します。

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

使用できる、ユーザーがテキスト フィールドに値を入力した後、`field`その値にアクセスする変数。

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>アラート ビュー コント ローラー ヘルパー クラス

使用してアラートの簡単な一般的な型を表示するため、`UIAlertViewController`により、重複するコードのかなり、ヘルパー クラスを使用すると、繰り返しのコードの量を減らすことができます。 例えば:

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

このクラスを使用して、表示、および単純なアラートへの対応は次のように実行できます。

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

この記事では、操作をカバーされて`UIAlertController`Xamarin.tvOS でユーザーに警告メッセージを表示します。 まず、簡単な警告を表示し、ボタンを追加する方法を示しました。 次に、アラートをテキスト フィールドを追加する方法を示しました。 最後に、ヘルパー クラスを使用して、アラートを表示するために必要な反復的なコードの量を削減する方法を示しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
