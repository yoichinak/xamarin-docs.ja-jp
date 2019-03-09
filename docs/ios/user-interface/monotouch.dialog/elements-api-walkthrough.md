---
title: 要素の API を使用して Xamarin.iOS アプリケーションを作成します。
description: この記事では、MonoTouch ダイアログの記事の概要に表示される情報に構築します。 MonoTouch.Dialog (MT を使用する方法を示すチュートリアルを表示しますD) 要素 API をすぐに開始山とアプリケーションの構築D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 14711f9cc2c34d72765e28db158379bc2a26849b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670290"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>要素の API を使用して Xamarin.iOS アプリケーションを作成します。

_この記事では、MonoTouch ダイアログの記事の概要に表示される情報に構築します。MonoTouch.Dialog (MT を使用する方法を示すチュートリアルを表示しますD) 要素 API をすぐに開始山とアプリケーションの構築D._

このチュートリアルでは、MT を使用しますタスク一覧を表示するアプリケーションのマスター/詳細のスタイルを作成する D 要素 API です。 ユーザーが選択すると、 <span class="ui"> + </span>ボタン タスクのテーブルへのナビゲーション バーで新しい行が追加されます。 行を選択するとは、以下に示すように、タスクの説明と、期日の更新を許可する詳細画面に移動します。

 [![](elements-api-walkthrough-images/01-task-list-app.png "タスクの説明と、期限を更新できるようにする詳細画面に移動しますが、行を選択")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>山の設定D

MT.D は、Xamarin.iOS で分散されます。 これを使用するを右クリックし、**参照**、Xamarin.iOS のノードは、Visual Studio 2017 または Visual Studio for Mac プロジェクトし、への参照を追加、 **MonoTouch.Dialog 1**アセンブリ。 次に、追加`using MonoTouch.Dialog`ソース内のステートメントは、必要に応じてコードします。

## <a name="elements-api-walkthrough"></a>API の要素のチュートリアル

[MonoTouch ダイアログの概要](~/ios/user-interface/monotouch.dialog/index.md)記事では、MT のさまざまな部分を十分に理解を得D. 要素の API を使用して、アプリケーションにまとめて配置しましょう。

## <a name="setting-up-the-multi-screen-application"></a>マルチ スクリーン アプリケーションの設定

MonoTouch.Dialog の作成画面の作成プロセスを開始する、 `DialogViewController`、し、追加、`RootElement`します。

MonoTouch.Dialog のマルチ スクリーン アプリケーションを作成する必要があります。

1.  
  `UINavigationController.` の作成
1.  
  `DialogViewController.` の作成
1.  追加、`DialogViewController`のルートとして、  `UINavigationController.` 
1.  追加、`RootElement`に、  `DialogViewController.`
1.  追加`Sections`と`Elements`に、  `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>UINavigationController を使用します。

ナビゲーション スタイルのアプリケーションを作成する必要がありますを作成する、 `UINavigationController`、として追加し、`RootViewController`で、`FinishedLaunching`のメソッド、`AppDelegate`します。 させる、`UINavigationController`追加 MonoTouch.Dialog、使用、`DialogViewController`を`UINavigationController`次に示します。

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

上記のコードのインスタンスを作成する、`RootElement`に渡すと、`DialogViewController`します。 `DialogViewController`が常に、`RootElement`その階層の最上位にします。 この例で、 `RootElement` "To Do List、"ナビゲーション コント ローラーのナビゲーション バーのタイトルとして機能する文字列が作成されます。 この時点では、アプリケーションを実行するには、次に示す画面があります。

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "ここで示すように画面が表示されます、アプリケーションを実行します。")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

MonoTouch.Dialog の階層構造を使用する方法を見てみましょう`Sections`と`Elements`をさらに画面を追加します。

### <a name="creating-the-dialog-screens"></a>ダイアログ画面の作成

A`DialogViewController`は、 `UITableViewController` MonoTouch.Dialog を使用して画面を追加するサブクラスです。 MonoTouch.Dialog を追加して画面を作成します、`RootElement`を`DialogViewController`、上記のとおりです。 `RootElement`が`Section`テーブルの各セクションを表すインスタンス。
要素のセクションが構成されますが、その他のセクションで、または他にも`RootElements`します。 入れ子で`RootElements`、次に表示されるように MonoTouch.Dialog が、ナビゲーション スタイルのアプリケーションに自動的に作成されます。

### <a name="using-dialogviewcontroller"></a>DialogViewController を使用します。

`DialogViewController`、中、 `UITableViewController` 、サブクラスを`UITableView`そのビューとします。 たびに、テーブルに項目を追加する、この例では、 <span class="ui"> + </span>  ボタンをタップします。 `DialogViewController`に追加された、`UINavigationController`を使用して、`NavigationItem`の`RightBarButton`プロパティを追加、 <span class="ui"> + </span>ボタンを次に示すよう。

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

作成したときに、`RootElement`以前を渡して、1 つ`Section`インスタンスとしての要素を追加することができるように、 <span class="ui"> + </span>ユーザーがボタンをタップします。 このイベントのボタンのハンドラーを実行するのに次のコードを使用できます。

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

このコードを作成する新しい`Task`オブジェクトごとに、ボタンをタップします。 単純な実装を次に示します、`Task`クラス。

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

タスクの`Name`プロパティを使用して、作成、`RootElement`のというカウンター変数と共にキャプション`n`を新しいタスクごとにインクリメントされます。 MonoTouch.Dialog に追加される行に要素をオンに、`TableView`ときに各`taskElement`が追加されます。

## <a name="presenting-and-managing-dialog-screens"></a>表示およびダイアログ ボックスを管理します。

使用して、 `RootElement` MonoTouch.Dialog の各タスクの詳細については新しい画面を作成し、行が選択されているときに、そこに移動が自動的にようにします。

タスクの詳細画面自体は 2 つのセクションで構成されます。各セクションには、1 つの要素が含まれています。 最初の要素が作成された、`EntryElement`タスクのため、編集可能な行を提供する`Description`プロパティ。 要素を選択すると、次に示すようにテキストを編集するためのキーボードが表示されます。

 [![](elements-api-walkthrough-images/03-create-task.png "示すようにテキストを編集するためのキーボードが表示される要素を選択すると、")](elements-api-walkthrough-images/03-create-task.png#lightbox)

2 番目のセクションが含まれています、`DateElement`できる管理タスクの`DueDate`プロパティ。 日付を選択すると、自動的にに示すように日付の選択を読み込みます。

 [![](elements-api-walkthrough-images/04-date-picker.png "日付を選択すると、自動的としての日付の選択を読み込みます")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

両方で、`EntryElement`と`DateElement`場合 (または MonoTouch.Dialog の任意のデータ入力要素の) 値への変更は自動的に保持されます。 これを示します、日付を編集して、ルート画面と詳細画面内の値が保持されます、さまざまなタスクの詳細の前後へ移動することができます。

## <a name="summary"></a>まとめ

この記事では、MonoTouch.Dialog 要素 API を使用する方法を説明したチュートリアルが表示されます。 山でマルチ スクリーンのアプリケーションを作成する基本的な手順を説明しました使用する方法など、D、`DialogViewController`と画面を作成するには、要素とセクションを追加する方法。 さらに、mt. を使用する方法を示しました組み合わせて D、`UINavigationController`します。

## <a name="related-links"></a>関連リンク

- [MTDWalkthrough (サンプル)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [MonoTouch.Dialog とスクリーン キャスト - Miguel de Icaza の作成、iOS のログイン画面](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
