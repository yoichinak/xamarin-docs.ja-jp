---
title: "IOS デザイナーでテーブルを操作します。"
description: "前のセクションでは、テーブルを使用して開発おについて説明しました。 これには、5 番目および最後のセクションおは、これまで説明した集計し、ストーリー ボードを使用して基本的な面倒な作業一覧アプリケーションを作成します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c59ddde44b0e47122865c55a7964707f106d2691
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tables-in-the-ios-designer"></a>IOS デザイナーでテーブルを操作します。

ストーリー ボード WYSIWYG iOS アプリケーションを作成する方法は、Mac および Windows で Visual Studio 内ではサポートされます。 ストーリー ボードの詳細についてを参照してください、[の概要をストーリー ボード](~/ios/user-interface/storyboards/index.md)ドキュメント。 ストーリー ボードのセルのレイアウトを編集することも*で*テーブルでは、テーブルおよびセルでの開発を簡略化

テーブル ビューのプロパティを構成する iOS デザイナーで、ときに 2 種類のセルのコンテンツから選択できます:**動的**または**静的**プロトタイプ コンテンツ。

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>プロトタイプの動的なコンテンツ

A`UITableView`プロトタイプでコンテンツ通常は、データの一覧を表示する場所プロトタイプ セル (またはセルとを定義できます 2 つ以上)、リスト内の各項目の再利用されます。 セルをインスタンス化する必要はありませんで取得された、`GetView`メソッドを呼び出して、`DequeueReusableCell`のメソッド、`UITableViewSource`です。

 <a name="Static_Content" />


## <a name="static-content"></a>静的コンテンツ

`UITableView`静的コンテンツとは、デザイン サーフェイスを右に設計するテーブルを使用できます。 セルをテーブルにドラッグし、プロパティを変更して、コントロールを追加してカスタマイズします。

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>ストーリー ボード ドリブン アプリケーションの作成

StoryboardTable 例には、ストーリー ボードで UITableView の両方の種類を使用する単純なマスター/詳細アプリが含まれています。 このセクションの残りの部分では、完了すると、次のようになります小さな to do リストの例をビルドする方法について説明します。

 [ ![例の画面](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png)

、ストーリー ボードとユーザー インターフェイスがビルドされ、両方の画面、UITableView が使用されます。 メイン画面を使用して*プロトタイプ コンテンツ*画面を使用するには、行のレイアウトと詳細情報の*静的なコンテンツ*セルのカスタム レイアウトを使用してデータ エントリ フォームを作成します。

## <a name="walkthrough"></a>チュートリアル

Visual Studio を使用して、新しいソリューションを作成する**(作成) の新しいプロジェクト > 1 つのビュー App(C#)**、および呼び出し_StoryboardTables_です。

 [ ![新しいプロジェクト ダイアログ ボックスを作成します。](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png)

一部の c# ファイルが開き、ソリューションおよび`Main.storyboard`ファイルが既に作成します。 ダブルクリックして、 `Main.storyboard` iOS デザイナーで開くファイル。

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>ストーリー ボードを変更します。

ストーリー ボードは、3 つの手順で編集されます。

-  最初に、レイアウト、必要なコント ローラーを表示し、それらのプロパティを設定します。
-  次に、ドラッグ アンド ドロップするオブジェクトのビューをドラッグして、UI を作成します。
-  最後に、各ビューに必要な UIKit クラスを追加し、コードで参照できるように、さまざまなコントロールに名前を付けます。


ストーリー ボードが完了したら、コードが正常に動作を追加できます。

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>コント ローラーの表示のレイアウト

ストーリー ボードには、最初の変更が既存の詳細ビューを削除して、UITableViewController に置き換えます。 この場合は、以下の手順に従ってください。

1.  ビューのコント ローラーの下部にあるバーを選択し、それを削除します。
2.  ドラッグ、**ナビゲーション コント ローラー**と**ビュー コント ローラーの表に**ツールボックスからストーリー ボード上にします。 
3.  追加されたテーブルのビューの 2 番目のコント ローラーに、ルート ビュー コント ローラーから、segue を作成します。 Segue、コントロールを作成すると、ドラッグ*詳細セルから*新しく追加された UITableViewController にします。 オプションを選択**表示*** **話題選択**です。 
4.  選択して、新しい話題作成したしコードでこの話題参照に識別子を取得します。 Segue をクリックし、入力`TaskSegue`の**識別子**で、**プロパティ パッド**、次のようにします。    
  [ ![プロパティ パネルの話題名前付け](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png) 

5. 次を選択し、プロパティ パッドを使用する 2 つのテーブルのビューを構成します。 ビューとビューのコント ローラー以外を選択することを確認 – 選択に役立てるためにドキュメント アウトラインを使用することができます。

6.  ルート ビュー コント ローラーを変更する**コンテンツ: 動的プロトタイプ**(デザイン サーフェイスにビューのラベル付けするには**プロトタイプ コンテンツ**)。

    [ ![動的なプロトタイプにコンテンツのプロパティの設定](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png)

7.  新しい変更**UITableViewController**する**コンテンツ: 静的セル**です。 


8. 新しい UITableViewController は、クラス名と識別子の設定が必要です。 コント ローラーのビューと種類を選択_TaskDetailViewController_の**クラス**で、**プロパティ パッド**– これは、新しい作成`TaskDetailViewController.cs`ソリューション内のファイルパッドです。 入力、 **StoryboardID**として_詳細_次の例で示すようにします。 これは、c# コードでは、このビューの読み込みに後で使用されます。  

    [ ![ストーリー ボード ID を設定](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png)

9. ストーリー ボードのデザイン画面は次のようになります (このルート ビュー コント ローラーのナビゲーション項目のタイトルは「面倒ボード」に変更されました)。

    [ ![デザイン画面](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>UI を作成する

これで、ビュー segues とはユーザー インターフェイス要素は、構成を追加する必要があります。

#### <a name="root-view-controller"></a>ルート ビュー コントローラー

最初に、マスター ビュー コント ローラーでのプロトタイプのセルを選択し、設定、**識別子**として_taskcell_下図のように、します。 これは、コードの後でこの UITableViewCell のインスタンスの取得に使用されます。

 [ ![セル識別子の設定](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png)

次に、以下に示すように、新しいタスクを追加するボタンを作成する必要があります。

[ ![バーのナビゲーション バーでボタン項目](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png)

次の手順で行います。 

-  ドラッグ、**バー ボタン項目**をツールボックスから、_ナビゲーション バーの右側にある_です。
-  **プロパティ パッド****バー ボタンの項目**選択**識別子: 追加**(やすく、  *+* プラス ボタン)。 
-  名前を付けます後の段階でのコードに識別できるようにします。 ルート ビュー コント ローラー クラス名を指定する必要がありますに注意してください (たとえば**ItemViewController**) バー ボタンの項目の名前を設定することにします。


#### <a name="taskdetail-view-controller"></a>TaskDetail ビュー コント ローラー

詳細ビューには、はるかに多く作業が必要です。 テーブル セルの表示は、ビューにドラッグしても、ラベル、テキスト ビュー、およびボタンを使用し、設定する必要があります。 次のスクリーン ショットは、2 つのセクションで完成した UI を示しています。 1 つのセクションが 3 つのセル、ラベルを 3 つ、2 つのテキスト フィールドと 1 つ切り替えるには、2 番目のセクションに 2 つのボタンを含む 1 つのセルがあるときに。

 [ ![詳細ビューのレイアウト](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png)

完全なレイアウトを作成する手順は次のとおりです。

テーブル ビューを選択し、開く、**プロパティ パッド**です。 次のプロパティを更新します。

-  **セクションでは**: _2_ 
-  **スタイル**:_グループ化_
-  **区切り記号**:_なし_
-  **選択範囲**:_選択がありません_

上部のセクションを選択し、**プロパティ > テーブルのビュー セクション**変更**行**に_3_下図のように。


 [ ![上部のセクションを 3 つの行に設定します。](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png)

開いている各セルの**プロパティ パッド**設定。

-  **スタイル**:_カスタム_
-  **識別子**: (各セルの一意の識別子 "_タイトル_「,」_ノート_「,」_完了_") です。
-  スクリーン ショットに示すようにレイアウトの作成に必要なコントロールをドラッグして (配置**UILabel**、 **UITextField**と**UISwitch**正しいセルのラベルを設定し、適切に ie です。タイトル、ノート行います)。


2 番目のセクションでは、次のように設定します。**行**に_1_高さが大きくするセルの下部にあるサイズ変更ハンドルを取得します。

-  **識別子を設定**: 一意の値。 [保存])。 
-  **背景を設定する**:_色をオフに_です。
-  セルに 2 つのボタンをドラッグし、タイトルを適切に設定 (つまり_保存_と_削除_)、次のように。

   [ ![下部のセクションに 2 つのボタンの設定](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png)

この時点ですることも、セルとアダプティブ レイアウトを確認するコントロールに制約を設定します。

### <a name="adding-uikit-class-and-naming-controls"></a>UIKit クラスを追加して、コントロールの名前を付ける

このストーリー ボードを作成するのには、最後のいくつかの手順があります。 最初は名前を付けてください、コントロールの各  **Identity > 名前**以降にコードで使用できるようにします。 これらの次のように、名前します。

-  **Title UITextField** : _TitleText_
-  **ノート UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **削除 UIButton** : _DeleteButton_
-  **保存 UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>コードを追加します。

作業の残りの部分は、Mac または c# での Windows で Visual Studio で実行されます。 上記のチュートリアルで設定されているコードで使用されるプロパティの名前を示すことに注意してください。

作成するとまず、`Chores`おアプリケーション全体でこれらの値を使用できるように取得し、ID、名前、ノートおよび、実行のブール値、値を設定する方法を提供するクラス。

`Chores`クラスは、次のコードを追加します。

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

次に、作成、`RootTableSource`から継承するクラスを`UITableViewSource`です。 

これとストーリー ボード以外のテーブル ビューの違いは、`GetView`メソッドが任意のセルをインスタンス化する必要がある`theDequeueReusableCell`メソッドは常に (一致する識別子) を持つプロトタイプ セルのインスタンスを返します。

次のコードは、`RootTableSource.cs`ファイル。

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

使用する、`RootTableSource`クラス、新しいコレクションを作成、`ItemViewController`のコンス トラクター。

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

`ViewWillAppear`ソース コレクションを渡すし、テーブルのビューに割り当て。

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

アプリを実行する場合は、メイン画面は今すぐを読み込んで 2 つのタスクの一覧を表示します。 タスクの影響を受けるときにストーリー ボードが定義されている segue によって詳細画面に表示されるが現時点ですべてのデータは表示されません。

'で送信する、パラメーター '、segue、オーバーライド、`PrepareForSegue`メソッドのプロパティを設定し、 `DestinationViewController` (、`TaskDetailViewController`この例では)。 移行先のビューのコント ローラー クラスはインスタンス化されたがまだユーザーに表示されるつまり、クラスのプロパティを設定できますが、UI コントロールを変更します。

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

`TaskDetailViewController` 、 `SetTask` ViewWillAppear で参照できるようにメソッドがプロパティにはそのパラメーターを割り当てます。 コントロールのプロパティは変更できません`SetTask`ためにが存在しないときに`PrepareForSegue`と呼びます。

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

Segue は今すぐ詳細画面を開くし、選択したタスクの情報を表示します。 残念ながらの実装はありません、**保存**と**削除**ボタン。 ボタンを実装する前にこれらのメソッドを追加**ItemViewController.cs**を基になるデータを更新し、詳細画面を閉じます。

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

次に、ボタンの追加する必要があります`TouchUpInside`イベント ハンドラーを`ViewDidLoad`メソッドの**TaskDetailViewController.cs**です。 `Delegate`をプロパティ リファレンス、`ItemViewController`呼び出せるように具体的には作成`SaveTask`と`DeleteTask`、コマンドレットの操作の一部としてこのビューを閉じたりします。

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

構築する機能の最後の残りの部分は、新しいタスクの作成です。 **ItemViewController.cs**新しいを作成するメソッドは、タスクおよび詳細ビューが開きます。 を追加します。 ストーリー ボードを使用してから、ビューをインスタンス化する、`InstantiateViewController`メソッドを`Identifier`'detail' になります。 この例では、そのビューの。

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

ナビゲーション バーのボタンをネットワーク上での最後に、 **ItemViewController.cs**の`ViewDidLoad`それを呼び出すメソッド。

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

ストーリー ボードの使用例 – 次のように完成したアプリの外観を完了するとします。

[ ![完成したアプリ](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png)

この例を示しています。

-  プロトタイプ コンテンツ データの一覧を表示する再利用するためのセルが定義されているテーブルを作成しています。 
-  入力フォームを作成する静的なコンテンツのテーブルを作成しています。 これには、テーブルのスタイルを変更して、セクションでは、セルと UI コントロールの追加が含まれます。 
-  Segue を作成し、オーバーライドする方法、`PrepareForSegue`に必要なすべてのパラメーターの対象のビューを通知するメソッド。 
-  ストーリー ボード ビューを直接を読み込み、`Storyboard.InstantiateViewController`メソッドです。



## <a name="related-links"></a>関連リンク

- [StoryboardTable (サンプル)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)
