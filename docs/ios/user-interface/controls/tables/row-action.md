---
title: Xamarin.iOS での行のアクションの使用
description: このガイドは UISwipeActionsConfiguration または UITableViewRowAction テーブル行のカスタムのスワイプ操作を作成する方法を示します
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: 6d41f37d4a63db710bb04e35e6e1a4be0dd4f7a4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408282"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Xamarin.iOS での行のアクションの使用

_このガイドは UISwipeActionsConfiguration または UITableViewRowAction テーブル行のカスタムのスワイプ操作を作成する方法を示します_

![行に示すのスワイプ操作](row-action-images/action02.png)

iOS には、テーブルに対する操作を行う 2 つの方法が用意されています:`UISwipeActionsConfiguration`と`UITableViewRowAction`します。

`UISwipeActionsConfiguration` iOS 11 で導入され、のセットを定義するために使用に実行するアクションを配置ときにユーザー スワイプといった_どちらの方向に_テーブル ビュー内の行。 この動作はネイティブ Mail.app のと同様です。 

`UITableViewRowAction`テーブル ビュー内の行をユーザーのカードが水平方向に左するときに実行されるアクションを定義するクラスを使用します。
たとえば、テーブル、行の左にスワイプの編集が表示されたら、**削除**既定のボタン。 複数のインスタンスに接続することによって、`UITableViewRowAction`クラスを`UITableView`、複数のカスタム動作を定義することができます、それぞれに独自のテキスト、書式設定と動作します。


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

スワイプ操作での実装に必要な 3 つのステップ`UISwipeActionsConfiguration`:

1. オーバーライド`GetLeadingSwipeActionsConfiguration`や`GetTrailingSwipeActionsConfiguration`メソッド。 これらのメソッドを返す、`UISwipeActionsConfiguration`します。 
2. インスタンスを作成、`UISwipeActionsConfiguration`が返されます。 このクラスの配列には、`UIContextualAction`します。
3. `UIContextualAction` を作成します。

これらは、次のセクションで詳しく説明します。

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1.SwipeActionsConfigurations メソッドを実装します。

`UITableViewController` (また`UITableViewSource`と`UITableViewDelegate`) 2 つのメソッドが含まれます:`GetLeadingSwipeActionsConfiguration`と`GetTrailingSwipeActionsConfiguration`、テーブル ビューの行の一連のスワイプ操作を実装するために使用されます。 先頭のスワイプ操作は、左から右の言語で、画面の左側にあると、右から左の言語では、画面の右側から、スワイプを指します。 

次の例 (から、 [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)サンプル)、先頭のスワイプ構成の実装例を示します。 2 つのアクションが説明されているコンテキストのアクションから作成された[下](#create-uicontextualaction)します。 これらのアクションはで新しく初期化に渡す[ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations)、戻り値として使用します。


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

インスタンス化、`UISwipeActionsConfiguration`を使用して、`FromActions`の新しい配列を追加するメソッドを`UIContextualAction`s、次のコード スニペットに示すようにします。

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

操作の表示順序は、配列に渡される方法に依存する重要です。 たとえば、先頭のカードの上のコードはアクションのため表示します。

![テーブルの行に表示される先頭のスワイプ操作](row-action-images/action03.png)

スワイプといった、末尾の次の図に示すように、アクションが表示されます。

![テーブルの行に表示される後続のスワイプ操作](row-action-images/action04.png)

このコード スニペットの新しい利用も`PerformsFirstActionWithFullSwipe`プロパティ。 既定では、このプロパティに設定が true の場合、行をユーザーが完全にスワイプするときに、配列の最初のアクションが発生することを意味します。 破壊的な操作がある場合 (たとえば"Delete"、理想的な動作できない可能性がありますこれおよびに設定する必要がありますので`false`します。

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>`UIContextualAction` の作成

コンテキストのアクションは、ユーザー テーブルの行をスワイプするときに表示されるアクションを実際に作成します。

提供する必要がありますアクションを初期化するために、`UIContextualActionStyle`タイトル、および`UIContextualActionHandler`します。 `UIContextualActionHandler`は 3 つのパラメーターを受け取ります。 アクション、アクションは、表示されていたビュー、および完了ハンドラー。

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

背景色やアクションの画像などのさまざまなビジュアル プロパティを編集できます。 上記のコード スニペットは、アクションにイメージを追加し、その背景色を青の設定を示します。

コンテキスト アクションが作成されたら、初期化するために使用できます、`UISwipeActionsConfiguration`で、`GetLeadingSwipeActionsConfiguration`メソッド。

## <a name="uitableviewrowaction"></a>UITableViewRowAction

1 つまたは複数の行のカスタム アクションを定義する、`UITableView`のインスタンスを作成する必要があります、`UITableViewDelegate`クラスし、オーバーライド、`EditActionsForRow`メソッド。 例:

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

静的な`UITableViewRowAction.Create`メソッドは、新たに作成するために使用`UITableViewRowAction`を表示する、 **Hi**ユーザー カードは、テーブル内の行を水平方向にままにするとボタンをクリックします。 後での新しいインスタンス、`TableDelegate`が作成されに接続されている、`UITableView`します。 例:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

上記のコードの実行し、ユーザー スワイプといった左テーブルの行に、 **Hi**の代わりにボタンが表示されます、**削除**既定で表示されるボタン。

[![](row-action-images/action01.png "削除ボタンの代わりに表示されている Hi ボタン")](row-action-images/action01.png#lightbox)

ユーザーがタップした場合、 **Hi**ボタン、`Hello World!`が書き込まれる Visual Studio でコンソールに Mac または Visual Studio のデバッグ モードでアプリケーションを実行するとします。



## <a name="related-links"></a>関連リンク

- [TableSwipeActions (サンプル)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
