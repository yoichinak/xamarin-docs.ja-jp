---
title: iOS Designer でのテーブルの操作
description: 前のセクションでテーブルを使用した開発について学習しました。 これは、5 番目と最後のセクションはこれまでに学習した内容を集計し、ストーリー ボードを使用して基本的な面倒な作業一覧アプリケーションを作成します。
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 303c96ae6cdbc9f5b327c971f962d6eac75a6fa1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227554"
---
# <a name="working-with-tables-in-the-ios-designer"></a>iOS Designer でのテーブルの操作

ストーリー ボードは、WYSIWYG iOS アプリケーションを作成する方法および Mac および Windows で Visual Studio 内でサポートされます。 ストーリー ボードの詳細についてを参照してください、[の概要をストーリー ボード](~/ios/user-interface/storyboards/index.md)ドキュメント。 ストーリー ボードのセルのレイアウトを編集することも*で*テーブルでは、テーブルとセルを使用した開発を簡略化

IOS Designer のテーブル ビューのプロパティを構成するときに、セルの内容から選択することができますの 2 つの種類があります。**動的**または**静的**プロトタイプ コンテンツ。

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>プロトタイプの動的なコンテンツ

A`UITableView`プロトタイプ コンテンツは、通常データの一覧を表示するため、プロトタイプのセル (または、セルを定義できます 1 つ以上)、リストの各項目の再利用されます。 セルがインスタンス化する必要はありませんで取得された、`GetView`メソッドを呼び出して、`DequeueReusableCell`メソッドの`UITableViewSource`します。

 <a name="Static_Content" />


## <a name="static-content"></a>静的コンテンツ

`UITableView`静的コンテンツを使用すると、テーブルをデザイン サーフェイスの右にデザインできます。 セルは、テーブルにドラッグしてプロパティを変更して、コントロールを追加してカスタマイズできます。

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>ストーリー ボード駆動型アプリを作成します。

StoryboardTable 例には、ストーリー ボードの UITableView の両方の種類を使用する単純なマスター-詳細アプリが含まれています。 このセクションの残りの部分では、完了すると、次のようになりますが小規模の to do リストの例を構築する方法について説明します。

 [![例の画面](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

ユーザー インターフェイスは、ストーリー ボードをビルドし、両方の画面を UITableView が使用されます。 メイン画面を使用して*プロトタイプ コンテンツ*画面を使用するには、行のレイアウトと詳細*静的コンテンツ*カスタム セルのレイアウトを使用してデータ エントリ フォームを作成します。

## <a name="walkthrough"></a>チュートリアル

Visual Studio を使用して新しいソリューションを作成 **(作成) の新しいプロジェクト > 単一ビュー アプリ (C#)**、それを呼び出すと_StoryboardTables_します。

 [![新しいプロジェクト ダイアログを作成します。](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

一部のソリューションが開きますC#ファイルと`Main.storyboard`ファイルが既に作成されています。 ダブルクリックして、 `Main.storyboard` iOS Designer で開くファイル。

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>ストーリー ボードを変更します。

3 つの手順では、ストーリー ボードを編集します。

-  最初に、レイアウトに必要なコント ローラーを表示し、そのプロパティを設定します。
-  次に、ドラッグ アンド ドロップ、ビュー上にオブジェクトによって、UI を作成します。
-  最後に、それぞれのビューに必要なの UIKit クラスを追加し、コードで参照できるように、さまざまなコントロールに名前を付けます。


ストーリー ボードが完了すると、正常に動作するコードを追加することができます。

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>レイアウト ビュー コント ローラー

ストーリー ボードには、最初の変更が既存の詳細ビューを削除して、UITableViewController に置き換えます。 この場合は、以下の手順に従ってください。

1.  ビュー コント ローラーの下部にあるバーを選択し、それを削除します。
2.  ドラッグ、**ナビゲーション コント ローラー**と**テーブル ビュー コント ローラー**ツールボックスからストーリー ボード上にします。 
3.  追加されたテーブル ビューの 2 番目のコント ローラーをルート ビュー コント ローラーからのセグエを作成します。 セグエ、コントロールの作成 + ドラッグ*詳細セルから*UITableViewController を新たに追加します。 オプションを選択**表示**  **セグエ選択**します。 
4.  選択して、新しい作成したセグエし、参照をコードでこのセグエに識別子を取得します。 セグエをクリックし、入力`TaskSegue`の**識別子**で、 **Properties Pad**、次のように。    
  [![名前付けプロパティ パネルのセグエ](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. 次を選択し、[プロパティ] タブを使用して 2 つのテーブル ビューを構成します。 ビューとビュー コント ローラー以外を選択してください – 選択を支援するドキュメント アウトラインを使用することができます。

6.  変更するルート ビュー コント ローラー**コンテンツ。動的なプロトタイプ**(デザイン サーフェイス上のビューがラベル付けする**プロトタイプ コンテンツ**)。

    [![動的なプロトタイプへのコンテンツのプロパティの設定](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  新しい変更**UITableViewController**する**コンテンツ。静的セル**します。 


8. 新しい UITableViewController は、そのクラスの名前と識別子の設定が必要です。 ビュー コント ローラーと種類を選択_TaskDetailViewController_の**クラス**で、 **Properties Pad** – これは、新しい作成`TaskDetailViewController.cs`ソリューション内のファイルパッドのです。 入力、 **StoryboardID**として_詳細_次の例で示すようにします。 これは、後で使用では、このビューを読み込むC#コード。  

    [![ストーリー ボード ID の設定](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. このようなストーリー ボードのデザイン画面なります (ルート ビュー コント ローラーのナビゲーション項目のタイトルが「面倒な作業ボード」に変更されました)。

    [![デザイン画面](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>UI を作成する

これで、ビュー セグエとはユーザー インターフェイス要素は、構成を追加する必要があります。

#### <a name="root-view-controller"></a>ルート ビュー コントローラー

最初に、マスター ビュー コント ローラーで、プロトタイプのセルを選択し、設定、**識別子**として_taskcell_、下図のようにします。 これは、コードの後半でこの UITableViewCell のインスタンスの取得に使用されます。

 [![セル識別子の設定](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

次に、以下に示すように、新しいタスクを追加するボタンを作成する必要があります。

[![バーのナビゲーション バーのボタンの項目](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

次の手順で行います。 

-  ドラッグ、**バー ボタンの項目**をツールボックスから、_のナビゲーション バーの右側にある_します。
-  **Properties Pad** **バー ボタンの項目**選択**識別子。追加**(させる、 *+* + ボタン)。 
-  コードの後の段階で識別できるように、これに名前を付けます。 ルート ビュー コント ローラー クラス名を提供する必要がありますに注意してください (たとえば**ItemViewController**) バー ボタンの項目の名前を設定するようにします。


#### <a name="taskdetail-view-controller"></a>TaskDetail ビュー コント ローラー

詳細ビューでは、多くの作業が必要です。 テーブル セルの表示は、ビューにドラッグし、そのラベル、テキスト ビュー、およびボタンが設定する必要があります。 次のスクリーン ショットは、2 つのセクションで完成した UI を示しています。 1 つのセクションには 3 つのセル、3 つのラベル、2 番目のセクションに 2 つのボタンを 1 つのセルがあるときに、2 つのテキスト フィールドと 1 つスイッチします。

 [![詳細ビューのレイアウト](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

完全なレイアウトを作成する手順は次のとおりです。

テーブル ビューを選択し、開く、**プロパティ パッド**します。 次のプロパティを更新します。

-  **セクション**:_2_ 
-  **スタイル**:_グループ化_
-  **区切り記号**:_[なし]_
-  **選択**:_選択されていません_

最上部のセクションを選択し、**プロパティ > テーブルのビュー セクション**変更**行**に_3_以下に示すように。


 [![最上部のセクションを 3 つの行に設定します。](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

開いている各セルの**Properties Pad**設定。

-  **スタイル**:_Custom_
-  **識別子**:(例: 各セルの一意の識別子を選択します。 "_タイトル_「,」_ノート_「,」_完了_")。
-  スクリーン ショットに示すようにレイアウトを生成するために必要なコントロールをドラッグします (配置**UILabel**、 **UITextField**と**UISwitch**正しいのセルとラベルを設定適切に、ie します。タイトル、ノートと完了)。


2 番目のセクションでは、次のように設定します。**行**に_1_高さが大きくするセルの下のサイズ変更ハンドルを取得してください。

-  **識別子の設定**: 一意の値 (例。 「保存」)。 
-  **背景を設定する**:_色をオフに_します。
-  2 つのボタンをセルにドラッグし、タイトルを適切に設定 (つまり_保存_と_削除_) の下に示すように。

   [![下部のセクションで 2 つのボタンの設定](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

この時点で、セルとコントロールをアダプティブのレイアウトに制約を設定することがありますもします。

### <a name="adding-uikit-class-and-naming-controls"></a>UIKit クラスを追加して、コントロールの名前を付ける

ストーリー ボードを作成するのには、最後のいくつかの手順があります。 最初名前を付けてください、コントロールの各  **Identity > 名**後でコードで使用できるようにします。 次のように、これら名前を付けます。

-  **タイトルの UITextField** :_TitleText_
-  **ノート UITextField** :_NotesText_
-  **UISwitch** :_DoneSwitch_
-  **削除 UIButton** :_DeleteButton_
-  **保存 UIButton** :_SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>コードを追加します。

作業の残りの部分は、Mac または Windows での Visual Studio で行われますC#します。 コードで使用されるプロパティの名前が、上記のチュートリアルでの設定を反映させることに注意してください。

作成する最初の`Chores`クラスを取得し、ID、名前、ノートおよび、完了ブール値、値を設定する方法が提供されるアプリケーション全体でこれらの値を使用できるようにします。

`Chores`クラスは、次のコードを追加します。

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

次に、作成、`RootTableSource`クラスから継承する`UITableViewSource`します。 

非ストーリー ボードのテーブル ビューとの違いは、`GetView`メソッドは、– セルが任意のインスタンスを作成する必要がある`theDequeueReusableCell`メソッドは常に (一致する識別子) を持つプロトタイプ セルのインスタンスを返します。

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

使用する、`RootTableSource`クラスで新しいコレクションを作成、`ItemViewController`のコンス トラクター。

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

`ViewWillAppear`ソースに、コレクションを渡すし、テーブル ビューに割り当てます。

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

アプリを実行する場合は、メイン画面ようになりましたロードを 2 つのタスクの一覧を表示します。 タスクが操作されたときに、セグエをストーリー ボードが定義されていると、詳細画面を表示するが、現時点でデータは表示されません。

'パラメーター 'セグエを送信するにはオーバーライド、`PrepareForSegue`メソッドのプロパティを設定し、 `DestinationViewController` (、`TaskDetailViewController`この例では)。 対象ビュー コント ローラー クラスをインスタンス化されているがまだ – ユーザーに表示されます。 つまり、クラスのプロパティを設定できますが、UI コントロールを変更しません。

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

`TaskDetailViewController` 、 `SetTask` ViewWillAppear で参照できるように、メソッドがプロパティにそのパラメーターを割り当てます。 コントロールのプロパティは変更できません`SetTask`ためにが存在しないときに`PrepareForSegue`が呼び出されます。

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

セグエは今すぐ詳細画面を開くし、選択したタスクの情報を表示します。 残念ながらの実装はありません、**保存**と**削除**ボタン。 ボタンを実装する前に、これらのメソッドを追加**ItemViewController.cs**を基になるデータを更新し、詳細画面を閉じます。

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

次に、ボタンの追加する必要があります`TouchUpInside`イベント ハンドラーを`ViewDidLoad`メソッドの**TaskDetailViewController.cs**します。 `Delegate`プロパティへの参照を`ItemViewController`を呼び出し、具体的には、作成された`SaveTask`と`DeleteTask`、その操作の一部としてこのビューを閉じるを。

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

ビルドする機能の最後の残りの部分は、新しいタスクの作成です。 **ItemViewController.cs**追加新しいを作成するメソッドは、タスクし、詳細ビューが開きます。 ストーリー ボードの使用からビューをインスタンス化する、`InstantiateViewController`メソッドを`Identifier`'詳細' となるこの例では、そのビューの。

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

接続のナビゲーション バーにボタンをクリックし、最後に、 **ItemViewController.cs**の`ViewDidLoad`メソッドを呼び出します。

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

ストーリー ボードの使用例 – 次のように完成したアプリは完了します。

[![完成したアプリ](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

例を示します。

-  プロトタイプ コンテンツ データのリストを表示する再利用のセルが定義されているテーブルを作成します。 
-  入力のフォームを作成する静的なコンテンツのテーブルを作成しています。 これには、テーブルのスタイルを変更して、セクションでは、セルおよび UI コントロールの追加が含まれています。 
-  セグエを作成し、オーバーライドする方法、`PrepareForSegue`に必要なすべてのパラメーターのターゲット ビューを通知するメソッド。 
-  ストーリー ボードのビューを直接を読み込み、`Storyboard.InstantiateViewController`メソッド。



## <a name="related-links"></a>関連リンク

- [StoryboardTable (サンプル)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
