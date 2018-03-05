---
title: "行の操作の操作"
description: "このガイドは UISwipeActionsConfiguration または UITableViewRowAction でテーブルの行のカスタム スワイプしてアクションを作成する方法を示します"
ms.topic: article
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: e9d3e2eecd4c03e7b3046e1ad86dd8a0d70a7f73
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-row-actions"></a>行の操作の操作

_このガイドは UISwipeActionsConfiguration または UITableViewRowAction でテーブルの行のカスタム スワイプしてアクションを作成する方法を示します_

![行方向にスワイプ操作のデモンストレーション](row-action-images/action02.png)

iOS には、テーブルに対する操作を行う 2 つの方法が用意されています:`UISwipeActionsConfiguration`と`UITableViewRowAction`です。

`UISwipeActionsConfiguration` iOS 11 で導入され、のセットを定義するために使用に実行するアクションは配置時にユーザー スワイプといった_どちらの方向に_テーブル ビュー内の行にします。 この動作はネイティブのある Mail.app のと似ています 

`UITableViewRowAction`ユーザー スワイプといったのテーブル ビュー内の行の水平方向にままの場合で行われるアクションを定義するクラスを使用します。
たとえば、行の左にスワイプ、テーブルの編集を表示するとき、**削除**既定ボタンをクリックします。 複数のインスタンスをアタッチすることにより、`UITableViewRowAction`クラスを`UITableView`、複数のカスタム動作を定義することができます、それぞれ独自のテキスト、書式設定に動作します。


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

スワイプしてアクションを実装するために必要な 3 つの手順がある`UISwipeActionsConfiguration`:

1. オーバーライド`GetLeadingSwipeActionsConfiguration`や`GetTrailingSwipeActionsConfiguration`メソッドです。 これらのメソッドが返す、`UISwipeActionsConfiguration`です。 
2. インスタンスを作成、`UISwipeActionsConfiguration`を指定します。 このクラスの配列を受け取る`UIContextualAction`です。
3. `UIContextualAction` を作成します。

これらは、次のセクションで詳しく説明します。

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1.SwipeActionsConfigurations メソッドの実装

`UITableViewController` (、さらに`UITableViewSource`と`UITableViewDelegate`) 2 つのメソッドが含まれます:`GetLeadingSwipeActionsConfiguration`と`GetTrailingSwipeActionsConfiguration`、テーブル ビューの行にスワイプしてアクションのセットを実装する使用されています。 先頭の方向にスワイプ アクションは、左から右への言語で、画面の左側にあるおよび右から左への言語で、画面の右側から、スワイプを指します。 

次の例 (から、 [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)サンプル) 先頭方向にスワイプ構成の実装を示しています。 2 つの操作が説明されているコンテキストのアクションから作成された[下](#create-uicontextualaction)です。 これらのアクションに渡されてで新たに初期化された[ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations)、戻り値として使用されます。


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

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2.インスタンスを作成します。 `UISwipeActionsConfiguration`

インスタンスを作成、`UISwipeActionsConfiguration`を使用して、`FromActions`の新しい配列を追加するメソッドを`UIContextualAction`s、次のコード スニペットで示すようにします。

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

操作が表示される順序は、配列に渡される方法に依存する重要です。 たとえば、先頭のカードの上に、コード アクションが表示されるため。

![テーブルの行に表示される読み取り操作の先頭](row-action-images/action03.png)

スワイプといった、末尾の次の図に示すように、アクションが表示されます。

![テーブルの行に表示される後続の方向にスワイプ アクション](row-action-images/action04.png)

このコード スニペットも使用する新しい`PerformsFirstActionWithFullSwipe`プロパティです。 既定では、このプロパティに設定が true の場合、ユーザーが行を完全に読み取るときに、配列内の最初のアクションが発生することを意味します。 破壊的なできないアクションがある場合 (たとえば"Delete"、理想的な動作はないかもしれませんおよびに設定する必要がありますので`false`です。

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>`UIContextualAction` の作成

コンテキストのアクションは、ユーザーがテーブルの行を読み取るときに表示されるアクションを実際に作成します。

指定する必要がありますアクションを初期化するために、 `UIContextualActionStyle`、タイトル、および`UIContextualActionHandler`です。 `UIContextualActionHandler` 3 つのパラメーターを受け取る: アクション、アクションが、表示されていたビューと、完了ハンドラー。

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

背景色またはアクションのイメージなどのさまざまな視覚的プロパティを編集できます。 上記のコード スニペットは、アクションにイメージを追加して、その背景色を青に設定を示します。

初期化に使用できるコンテキストのアクションを作成した後、`UISwipeActionsConfiguration`で、`GetLeadingSwipeActionsConfiguration`メソッドです。

## <a name="uitableviewrowaction"></a>UITableViewRowAction

1 つまたは複数の行のカスタム アクションを定義する、`UITableView`のインスタンスを作成する必要があります、`UITableViewDelegate`クラスし、オーバーライド、`EditActionsForRow`メソッドです。 例:

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

静的`UITableViewRowAction.Create`メソッドの作成に使用する新しい`UITableViewRowAction`を表示する、 **Hi**ユーザー カードは、テーブル内の行の水平方向にままの場合 ボタンをクリックします。 以降の新しいインスタンス、`TableDelegate`が作成されに接続されている、`UITableView`です。 例:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

上記のコードを実行すると、ユーザー スワイプといった左テーブルの行に、 **Hi**の代わりにボタンが表示されます、**削除**既定で表示されるボタン。

[ ![](row-action-images/action01.png "[削除] ボタンの代わりに表示されている、Hi ボタン")](row-action-images/action01.png)

ユーザーがタップした場合、 **Hi**ボタン、`Hello World!`が書き込まれる Visual Studio でのコンソールへの Mac または Visual Studio アプリケーションをデバッグ モードで実行するとします。



## <a name="related-links"></a>関連リンク

- [TableSwipeActions (サンプル)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
