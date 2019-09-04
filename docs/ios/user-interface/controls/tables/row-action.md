---
title: Xamarin の行アクションの操作 (iOS)
description: このガイドでは、Uiswipeactions Configuration または Uiswipeactionsconfiguration を使用してテーブル行のカスタムスワイプ操作を作成する方法を示します。
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: e90e108e6b02055a585129b6412641a726afaab4
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226298"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Xamarin の行アクションの操作 (iOS)

_このガイドでは、Uiswipeactions Configuration または Uiswipeactionsconfiguration を使用してテーブル行のカスタムスワイプ操作を作成する方法を示します。_

![行に対するスワイプ操作のデモンストレーション](row-action-images/action02.png)

iOS には`UISwipeActionsConfiguration` 、テーブルに対して操作を実行する`UITableViewRowAction`2 つの方法 (と) が用意されています。

`UISwipeActionsConfiguration`は、iOS 11 で導入され、ユーザーがテーブルビューの行に対して_いずれかの方向で_スワイプしたときに実行する一連のアクションを定義するために使用されます。 この動作は、ネイティブのメールアプリの動作と似ています。

`UITableViewRowAction`クラスは、ユーザーがテーブルビューの行で水平にスワイプたときに実行されるアクションを定義するために使用されます。
たとえば、テーブルを編集するときに、行を左にスワイプすると、既定で **[削除]** ボタンが表示されます。 `UITableViewRowAction`クラスの複数のインスタンスを`UITableView`にアタッチすることにより、複数のカスタムアクションを定義し、それぞれに独自のテキスト、書式設定、および動作を設定できます。


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

スワイプ操作`UISwipeActionsConfiguration`を実装するには、次の3つの手順が必要です。

1. `GetTrailingSwipeActionsConfiguration`メソッドとメソッドをオーバーライド`GetLeadingSwipeActionsConfiguration`します。 これらのメソッドは`UISwipeActionsConfiguration`を返します。
2. 返される`UISwipeActionsConfiguration`をインスタンス化します。 このクラスは、の`UIContextualAction`配列を受け取ります。
3. `UIContextualAction` を作成します。

これらの詳細については、次のセクションで詳しく説明します。

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. SwipeActionsConfigurations メソッドの実装

`UITableViewController`(およびも`UITableViewSource` `UITableViewDelegate` )に`GetTrailingSwipeActionsConfiguration`は、テーブルビュー行にスワイプアクションのセットを実装するために使用されるとという2つのメソッドが含まれています。`GetLeadingSwipeActionsConfiguration` 先頭のスワイプ操作では、画面の左側から左から右の言語でスワイプを参照し、右から左に記述する言語で画面の右側からスワイプを参照します。

次の例では、(「行[Wipeactions](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions)サンプル」から) リーディングスワイプ構成を実装する方法を示しています。 コンテキストアクションからは、[次](#create-uicontextualaction)に説明する2つのアクションが作成されます。 これらのアクションは、戻り値として[`UISwipeActionsConfiguration`](#create-uiswipeactionsconfigurations)使用される、新しく初期化されたに渡されます。


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });

    leadingSwipe.PerformsFirstActionWithFullSwipe = false;

    return leadingSwipe;
}
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. をインスタンス化する`UISwipeActionsConfiguration`

次の`UISwipeActionsConfiguration`コードスニペットに`FromActions`示すように、メソッドを使用`UIContextualAction`してをインスタンス化し、の新しい配列を追加します。

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

アクションが表示される順序は、配列に渡される方法によって異なります。 たとえば、先頭のスワイプのコードでは、次のようにアクションが表示されます。

![テーブルの行に表示される先頭方向のスワイプ操作](row-action-images/action03.png)

後続のスワイプでは、次の図のようにアクションが表示されます。

![テーブル行に表示される最後のスワイプ操作](row-action-images/action04.png)

このコードスニペットでは、新しい`PerformsFirstActionWithFullSwipe`プロパティも使用します。 既定では、このプロパティは true に設定されています。これは、ユーザーが行を完全にスワイプしたときに、配列の最初のアクションが行われることを意味します。 破壊的でないアクション (たとえば "Delete") がある場合は、これが理想的な動作ではない可能性があるため、 `false`これをに設定する必要があります。

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>`UIContextualAction` の作成

コンテキストアクションでは、ユーザーがテーブル行をスワイプしたときに表示されるアクションを実際に作成します。

アクションを初期化するには`UIContextualActionStyle`、、タイトル、 `UIContextualActionHandler`およびを指定する必要があります。 は`UIContextualActionHandler` 、アクション、アクションが表示されたビュー、および完了ハンドラーの3つのパラメーターを受け取ります。

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null));
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);

                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

アクションの背景色や画像など、さまざまな視覚プロパティを編集できます。 上記のコードスニペットは、アクションに画像を追加し、背景色を青に設定する方法を示しています。

コンテキストアクションが作成されると、を使用して`UISwipeActionsConfiguration` `GetLeadingSwipeActionsConfiguration`メソッドでを初期化できます。

## <a name="uitableviewrowaction"></a>UITableViewRowAction

に対して1つ以上の`UITableView`カスタム行アクションを定義するには、 `UITableViewDelegate`クラスのインスタンスを作成し、 `EditActionsForRow`メソッドをオーバーライドする必要があります。 例えば:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

静的`UITableViewRowAction.Create`メソッドを使用して新しい`UITableViewRowAction`を作成し、ユーザーがテーブルの行の水平方向にスワイプたときに **[Hi]** ボタンが表示されるようにします。 後で、 `TableDelegate`の新しいインスタンスが作成され、 `UITableView`にアタッチされます。 例えば:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

上のコードを実行し、ユーザーがテーブルの行にスワイプすると、既定で表示される **[削除]** ボタンの代わりに **[Hi]** ボタンが表示されます。

[![](row-action-images/action01.png "[削除] ボタンの代わりに表示される [Hi] ボタン")](row-action-images/action01.png#lightbox)

ユーザーが **[こんにちは]** ボタンをタップ`Hello World!`すると、アプリケーションがデバッグモードで実行されるときに、Visual Studio for Mac または Visual Studio のコンソールにが出力されます。



## <a name="related-links"></a>関連リンク

- [個の Wipeactions (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions)
- [WorkingWithTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
