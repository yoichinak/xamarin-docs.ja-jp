---
title: iOS Designer でのテーブルの操作
description: 前のセクションでは、テーブルを使用した開発について説明します。 この5番目のセクションでは、これまでに学習した内容を集計し、ストーリーボードを使用して基本的な作業リストアプリケーションを作成します。
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 57347336bb91757c9c54f7279f386f15e07c9cd7
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573643"
---
# <a name="working-with-tables-in-the-ios-designer"></a>iOS Designer でのテーブルの操作

ストーリーボードは、iOS アプリケーションを作成するための WYSIWYG 的な方法であり、Mac および Windows 上の Visual Studio 内でサポートされています。 ストーリーボードの詳細については、「[ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)」ドキュメントを参照してください。 また、テーブル*内*のセルレイアウトを編集して、テーブルとセルを使用した開発を簡素化することもできます。

IOS デザイナーでテーブルビューのプロパティを構成するときに選択できるセルコンテンツには、**動的**または**静的**なプロトタイプコンテンツの2種類があります。

<a name="Prototype_Content"></a>

## <a name="dynamic-prototype-content"></a>動的プロトタイプのコンテンツ

`UITableView`プロトタイプコンテンツを持つは、通常、リスト内の各項目に対してプロトタイプセル (または複数定義可能なセル) を再利用するデータの一覧を表示することを目的としています。 これらのセルは、インスタンス化する必要はありません `GetView` 。メソッドでは、のメソッドを呼び出すことによって取得され `DequeueReusableCell` `UITableViewSource` ます。

 <a name="Static_Content"></a>

## <a name="static-content"></a>静的なコンテンツ

`UITableView`の静的コンテンツを使用すると、デザイン画面上にテーブルを適切にデザインできます。 セルはテーブルにドラッグし、プロパティを変更してコントロールを追加することによってカスタマイズできます。

 <a name="Creating_a_Storyboard-driven_app"></a>

## <a name="creating-a-storyboard-driven-app"></a>ストーリーボード駆動型アプリの作成

StoryboardTable の例には、ストーリーボードで両方の種類の UITableView を使用する単純なマスター/詳細アプリが含まれています。 このセクションの残りの部分では、完了時に次のような小さな to do リストの例を構築する方法について説明します。

 [![画面の例](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

ユーザーインターフェイスはストーリーボードを使用して作成され、両方の画面で UITableView が使用されます。 メイン画面は*プロトタイプコンテンツ*を使用して行をレイアウトし、詳細画面は*静的なコンテンツ*を使用して、カスタムセルレイアウトを使用してデータ入力フォームを作成します。

## <a name="walkthrough"></a>チュートリアル

(Create) [新しいプロジェクトの作成] を使用して Visual Studio で新しいソリューションを作成**し (C#) >**、 _StoryboardTables_を呼び出します。

 [![[新しいプロジェクトの作成] ダイアログ](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

ソリューションは、C# ファイルと `Main.storyboard` ファイルが既に作成されている状態で開きます。 ファイルをダブルクリックし `Main.storyboard` て、IOS Designer で開きます。

<a name="Modifying_the_Storyboard"></a>

## <a name="modifying-the-storyboard"></a>ストーリーボードの変更

ストーリーボードは、次の3つの手順で編集されます。

- まず、必要なビューコントローラーをレイアウトし、それらのプロパティを設定します。
- 次に、オブジェクトをビューにドラッグアンドドロップして UI を作成します。
- 最後に、必要な UIKit クラスを各ビューに追加し、コードで参照できるように、さまざまなコントロールに名前を付けます。

ストーリーボードが完成したら、コードを追加してすべての機能を実現できます。

<a name="Layout_The_View_Controllers"></a>

### <a name="layout-the-view-controllers"></a>ビューコントローラーのレイアウト

ストーリーボードへの最初の変更は、既存の詳細ビューを削除し、UITableViewController に置き換えることです。 次の手順に従います。

1. ビューコントローラーの下部にあるバーを選択して削除します。
2. **ナビゲーションコントローラー**と**テーブルビューコントローラー**を、ツールボックスからストーリーボードにドラッグします。 
3. ルートビューコントローラーから、先ほど追加した2番目のテーブルビューコントローラーまでのセグエを作成します。 セグエを作成するには、*詳細セルから*新しく追加された UITableViewController にコントロールをドラッグします。 [**セグエの選択**] の下にある [**表示**] を選択します。 
4. 作成した新しいセグエを選択し、コードでこのセグエを参照するための識別子を指定します。 セグエをクリックし、次のように Properties Pad の識別子として「」と入力し `TaskSegue` ます。 **Identifier** **Properties Pad**    
  [![プロパティパネルでのセグエの名前付け](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. 次に、2つのテーブルビューを選択し、Properties Pad を使用して構成します。 [ビューコントローラーを表示せずに表示する] を選択してください。ドキュメントアウトラインを使用して、選択に役立てることができます。

6. ルートビューコントローラーをコンテンツに変更し**ます: 動的プロトタイプ**(デザインサーフェイスのビューは、**プロトタイプコンテンツ**としてラベル付けされます):

    [![コンテンツプロパティを動的プロトタイプに設定する](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7. 新しい**Uitableviewcontroller**を**Content: Static cell**に変更します。 

8. 新しい UITableViewController には、クラス名と識別子が設定されている必要があります。 ビューコントローラーを選択し、 **Properties Pad**の**クラス**に「 _task詳細 viewcontroller_ 」と入力します。これにより、Solution Pad に新しいファイルが作成され `TaskDetailViewController.cs` ます。 次の例に示すように、 **StoryboardID**を_詳細_として入力します。 これは後で C# コードでこのビューを読み込むために使用されます。  

    [![ストーリーボード ID の設定](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. ストーリーボードのデザインサーフェイスは次のようになります (ルートビューコントローラーのナビゲーション項目のタイトルが "面倒なボード" に変更されています)。

    [![デザイン画面](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  

<a name="Create_the_UI"></a>

### <a name="create-the-ui"></a>UI を作成する

ビューとセグエが構成されたので、ユーザーインターフェイスの要素を追加する必要があります。

#### <a name="root-view-controller"></a>ルート ビュー コントローラー

まず、次に示すように、マスタービューコントローラーで [プロトタイプ] セルを選択し、[**識別子**] を_taskcell_に設定します。 この UITableViewCell のインスタンスを取得するために、後でコード内で使用されます。

 [![セル識別子の設定](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

次に、次に示すように、新しいタスクを追加するボタンを作成する必要があります。

[![ナビゲーションバーのバーボタン項目](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

次の操作を行います。 

- ツールボックスから_ナビゲーションバーの右側_に**バーボタンの項目**をドラッグします。
- **Properties Pad**の [**バーボタン項目**] で、[**識別子: 追加**] を選択し *+* ます (プラスボタンにします)。 
- 後のステージでコード内で識別できるように名前を付けます。 バーボタン項目の名前を設定できるようにするには、ルートビューコントローラーにクラス名 (たとえば、 **Itemviewcontroller**) を指定する必要があることに注意してください。

#### <a name="taskdetail-view-controller"></a>TaskDetail ビューコントローラー

詳細ビューでは、さらに多くの作業が必要です。 テーブルビューのセルをビューにドラッグして、ラベル、テキストビュー、およびボタンを設定する必要があります。 次のスクリーンショットは、2つのセクションを含む完成した UI を示しています。 1つのセクションには3つのセル、3つのラベル、2つのテキストフィールド、1つのスイッチがあり、2番目のセクションには2つのボタンを持つ1つのセルがあります。

 [![詳細ビューのレイアウト](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

レイアウト全体を作成する手順は次のとおりです。

テーブルビューを選択し、**プロパティパッド**を開きます。 次のプロパティを更新します。

- **セクション**: _2_ 
- **スタイル**:_グループ化_
- **区切り記号**:_なし_
- **選択**:_選択なし_

上のセクションを選択し、[**プロパティ > テーブルビュー] セクション**で、次に示すように**行**を_3_に変更します。

 [![上のセクションを3行に設定する](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

各セルに対して**Properties Pad**を開き、次のように設定します。

- **スタイル**:_カスタム_
- **識別子**: 各セルの一意の識別子を選択します (例: "_title_"、"_note_"、"_done_")。
- 必要なコントロールをドラッグして、スクリーンショットに示されているレイアウトを生成します (適切なセルに**UILabel**、 **uitextfield** 、 **uisスイッチ**を配置し、ラベルを適切に設定します)。タイトル、メモ、完了)。

2番目のセクションでは、**行**を_1_に設定し、セルの下部のサイズ変更ハンドルを取得して高さを変更します。

- **識別子**を一意の値に設定します (例: [保存])。 
- **背景を設定**します。_色をクリア_します。
- 次に示すように、2つのボタンをセルにドラッグし、タイトルを適切に設定します (つまり、_保存_と_削除_)。

   [![下部セクションの2つのボタンの設定](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

この時点で、セルとコントロールに対して制約を設定して、アダプティブレイアウトを確保することもできます。

### <a name="adding-uikit-class-and-naming-controls"></a>UIKit クラスと名前付けコントロールの追加

ストーリーボードを作成するための最後の手順がいくつかあります。 まず、後でコードで使用できるように、各コントロールに**Identity > name**の下に名前を付けておく必要があります。 次のように名前を付けます。

- **Title UITextField: タイトル**_テキスト_
- **メモ UITextField** : _NotesText_
- **Uiswitch** : _DoneSwitch_
- **UIButton の削除**: _deletebutton_
- **[Uibutton の保存]** : _savebutton_

<a name="Adding_Code"></a>

## <a name="adding-code"></a>追加 (コードを)

残りの作業は、Mac または Windows の Visual Studio で C# を使用して実行されます。 コードで使用されるプロパティ名は、上記のチュートリアルで設定されているものと同じであることに注意してください。

まず、クラスを作成します。これにより、 `Chores` アプリケーション全体でこれらの値を使用できるように、ID、Name、note、Done ブール値を取得して設定する方法が提供されます。

クラスで `Chores` 、次のコードを追加します。

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

次に、 `RootTableSource` から継承するクラスを作成 `UITableViewSource` します。 

このと非ストーリーボードテーブルビューの違いは、 `GetView` メソッドがセルをインスタンス化する必要がないことです `theDequeueReusableCell` 。メソッドは常に、(一致する識別子を持つ) プロトタイプセルのインスタンスを返します。

次のコードは、ファイルからのものです `RootTableSource.cs` 。

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

クラスを使用するには、 `RootTableSource` のコンストラクターに新しいコレクションを作成し `ItemViewController` ます。

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

で `ViewWillAppear` ソースにコレクションを渡し、テーブルビューに割り当てます。

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

アプリをすぐに実行すると、メイン画面に2つのタスクの一覧が読み込まれて表示されるようになります。 タスクがタッチされると、ストーリーボードによって定義されたセグエによって詳細画面が表示されますが、その時点ではデータは表示されません。

セグエでパラメーターを送信するには、メソッドをオーバーライド `PrepareForSegue` し、のプロパティを設定し `DestinationViewController` `TaskDetailViewController` ます (この例では)。 ターゲットビューコントローラークラスはインスタンス化されていますが、まだユーザーに表示されていません。つまり、クラスのプロパティを設定することはできますが、UI コントロールを変更することはできません。

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

`TaskDetailViewController`メソッドでは、 `SetTask` viewwillappear 中で参照できるように、パラメーターをプロパティに割り当てます。 `SetTask`が呼び出されたときにが存在しない可能性があるので、コントロールのプロパティをで変更することはできません `PrepareForSegue` 。

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

セグエで詳細画面が開き、選択したタスク情報が表示されます。 残念ながら、[**保存**] ボタンと [**削除**] ボタンは実装されていません。 ボタンを実装する前に、これらのメソッドを**ItemViewController.cs**に追加して、基になるデータを更新し、詳細画面を閉じます。

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

次に、ボタンの `TouchUpInside` イベントハンドラーを `ViewDidLoad` **TaskDetailViewController.cs**のメソッドに追加する必要があります。 `Delegate`へのプロパティ参照は、 `ItemViewController` とを呼び出すことができるように特別に作成されました。これにより、 `SaveTask` `DeleteTask` このビューは操作の一部として閉じられます。

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

最後にビルドする機能は、新しいタスクを作成することです。 **ItemViewController.cs**で、新しいタスクを作成するメソッドを追加し、詳細ビューを開きます。 ストーリーボードからビューをインスタンス化するには、 `InstantiateViewController` そのビューのでメソッドを使用し `Identifier` ます。この例では、"detail" になります。

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

最後に、 **ItemViewController.cs**のメソッドのナビゲーションバーにあるボタンをクリックし `ViewDidLoad` て呼び出します。

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

ストーリーボードの例を完了すると、完成したアプリは次のようになります。

[![完成したアプリ](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

例を次に示します。

- データの一覧を表示するために再利用するためにセルが定義されているプロトタイプコンテンツを含むテーブルを作成します。 
- 静的なコンテンツを含むテーブルを作成し、入力フォームを構築します。 これには、テーブルのスタイルの変更や、セクション、セル、および UI コントロールの追加が含まれます。 
- セグエを作成し、メソッドをオーバーライドして、 `PrepareForSegue` 必要なパラメーターのターゲットビューを通知する方法。 
- メソッドを使用して、ストーリーボードビューを直接読み込み `Storyboard.InstantiateViewController` ます。

## <a name="related-links"></a>関連リンク

- [StoryboardTable (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
