---
title: "チュートリアル - 要素 API を使用してアプリケーションを作成します。"
description: "この記事は、MonoTouch ダイアログ記事の概要に表示される情報を基に構築します。 MonoTouch.Dialog (山を使用する方法を示すチュートリアルを示しますD) 要素 API を簡単に開始山したアプリケーションのビルドD."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 19e1ab4000e473aa773bf75015ff520a1f9a96d8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>チュートリアル - 要素 API を使用してアプリケーションを作成します。

_この記事は、MonoTouch ダイアログ記事の概要に表示される情報を基に構築します。MonoTouch.Dialog (山を使用する方法を示すチュートリアルを示しますD) 要素 API を簡単に開始山したアプリケーションのビルドD._

このチュートリアルでは、山を使用しますタスク一覧を表示するアプリケーションのマスター/詳細形式のスタイルを作成する D 要素 API です。 ユーザーが選択すると、 <span class="ui"> + </span>ボタン ナビゲーション バーで、新しい行をタスクのテーブルに追加されます。 行を選択するとは、以下に示すように、タスクの説明と期限の日付を更新することを許可する詳細画面に移動します。

 [![](elements-api-walkthrough-images/01-task-list-app.png "タスクの説明と、期限を更新することができる詳細画面に移動は、行を選択")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>要素の API のチュートリアル

[MonoTouch ダイアログ ボックスに概要](~/ios/user-interface/monotouch.dialog/index.md)機械翻訳のさまざまな部分を確実に理解を得記事、D. アプリケーションにまとめて配置するのに要素 API を使用してみましょう。

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>複数の画面アプリケーションを設定します。

MonoTouch.Dialog を作成、画面の作成プロセスを開始するには`DialogViewController`、し、追加、`RootElement`です。

MonoTouch.Dialog でマルチ画面アプリケーションを作成するには、する必要があります。

1.  作成します。  `UINavigationController.`
1.  作成します。  `DialogViewController.`
1.  追加、`DialogViewController`のルートとして、  `UINavigationController.` 
1.  追加、`RootElement`に、  `DialogViewController.`
1.  追加`Sections`と`Elements`に、  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>使用して、UINavigationController

ナビゲーション スタイル アプリケーションを作成する必要がありますを作成する、 `UINavigationController`、として追加し、`RootViewController`で、`FinishedLaunching`のメソッド、`AppDelegate`です。 させる、`UINavigationController`追加 MonoTouch.Dialog、使用、`DialogViewController`を`UINavigationController`次のように。

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

上記のコードがのインスタンスを作成、`RootElement`に渡すと、`DialogViewController`です。 `DialogViewController`が常に、`RootElement`その階層の最上位にします。 この例では、 `RootElement` 「作業の一覧、」ナビゲーション コント ローラーのナビゲーション バーのタイトルとして機能する文字列を作成します。 この時点では、アプリケーションを実行するには、次に示す画面があります。

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "次に示す画面が表示されます、アプリケーションを実行します。")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

MonoTouch.Dialog の階層構造を使用する方法を見てみましょう`Sections`と`Elements`を他の画面を追加します。

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>ダイアログの画面を作成します。

A`DialogViewController`は、 `UITableViewController` MonoTouch.Dialog を使用して画面を追加するサブクラスです。 MonoTouch.Dialog を追加することで画面を作成、`RootElement`を`DialogViewController`上で見たようです。 `RootElement`持てる`Section`テーブルのセクションを表すインスタンス。
セクションが要素の構成、他のセクションでは、または他の`RootElements`します。 入れ子により`RootElements`MonoTouch.Dialog は、次おわかりのように自動的にナビゲーション スタイル アプリケーションを作成します。

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>DialogViewController を使用します。

`DialogViewController`、中、`UITableViewController`サブクラスが、`UITableView`そのビューとします。 この例ではおに追加する項目の表に、毎回、 <span class="ui"> + </span>  ボタンをタップします。 以降、`DialogViewController`に追加された、`UINavigationController`を使用して、`NavigationItem`の`RightBarButton`プロパティを追加する、 <span class="ui"> + </span>  ボタン、次のように。

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

作成したときに、`RootElement`前に、渡されたが、1 つ`Section`インスタンスの要素として追加できるように、 <span class="ui"> + </span>ユーザーがボタンをタップします。 次のコードを使用して、このイベントのハンドラーをボタンを実行するおことができます。

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

このコードでは、新しい`Task`オブジェクトごとに、ボタンをタップします。 次の単純な実装を示しています、`Task`クラス。

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

 []()

タスクの`Name`プロパティを使用して、作成、`RootElement`のという名前のカウンター変数と共にキャプション`n`は、新しいタスクごとに増加します。 MonoTouch.Dialog に追加される行に要素をオンに、`TableView`ときに各`taskElement`を追加します。

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>表示と選択 ダイアログ ボックスを管理します。

使用して、 `RootElement` MonoTouch.Dialog は自動的に各タスクの詳細用の新しい画面を作成し、行が選択されているときに、移動できるようにします。

タスクの詳細画面自体は 2 つのセクションで構成されます。これらのセクションには、1 つの要素が含まれています。 最初の要素が作成された、`EntryElement`のタスクの編集可能な行を提供する`Description`プロパティです。 要素を選択すると、次に示すようにテキストを編集するためのキーボードが表示されます。

 [![](elements-api-walkthrough-images/03-create-task.png "ように、要素が選択されているときにテキストを編集するためのキーボードが表示されます。")](elements-api-walkthrough-images/03-create-task.png#lightbox)

2 番目のセクションが含まれています、`DateElement`ことにより、管理タスクの`DueDate`プロパティです。 ように、日付選択カレンダーをロード日付を自動的に選択します。

 [![](elements-api-walkthrough-images/04-date-picker.png "日付を自動的に選択として日付選択カレンダーを読み込みます")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

両方で、`EntryElement`と`DateElement`場合 (または MonoTouch.Dialog 内のデータ入力要素の) 値に変更が自動的に保持されます。 これは、日付を編集して、ルート画面と詳細画面内の値が保持されます、さまざまなタスクの詳細の間で前後の移動おデモンストレーションできます。

 <a name="Summary" />


## <a name="summary"></a>まとめ

この記事では、MonoTouch.Dialog 要素 API を使用する方法を示しましたを説明するチュートリアルが表示されます。 山とマルチ画面アプリケーションを作成する基本的な手順を説明しました使用する方法を含む、D、`DialogViewController`と画面を作成するには、要素とセクションを追加する方法です。 山を使用する方法を示しましたがさらに、組み合わせて D、`UINavigationController`です。


## <a name="related-links"></a>関連リンク

- [MTDWalkthrough (sample)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [-スクリーン キャストである Miguel de Icaza により iOS ログイン画面 MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
