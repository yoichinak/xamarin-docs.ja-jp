---
title: Elements API を使用して Xamarin iOS アプリケーションを作成する
description: この記事は、「Monotouch.dialog の概要」の記事に記載されている情報に基づいています。 ここでは、Monotouch.dialog (MT) の使用方法を示すチュートリアルを示します。D) Elements API を使用して、MT を使用したアプリケーションの構築をすぐに始めることができます。A.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 2258b2e8451f896aff59c89478833976ef7086e3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002364"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Elements API を使用して Xamarin iOS アプリケーションを作成する

_この記事は、「Monotouch.dialog の概要」の記事に記載されている情報に基づいています。ここでは、Monotouch.dialog (MT) の使用方法を示すチュートリアルを示します。D) Elements API を使用して、MT を使用したアプリケーションの構築をすぐに始めることができます。A._

このチュートリアルでは、MT を使用します。D Elements API は、タスク一覧を表示するアプリケーションのマスター/詳細スタイルを作成します。 ユーザーがナビゲーションバーの [ **+** ] ボタンを選択すると、そのタスクのテーブルに新しい行が追加されます。 行を選択すると、次に示すように、タスクの説明と期限を更新できる詳細画面に移動します。

[![](elements-api-walkthrough-images/01-task-list-app.png "Selecting the row will navigate to the detail screen that allows us to update the task description and the due date")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

## <a name="setting-up-mtd"></a>MT を設定しています。A

MT.D は、Xamarin. iOS と共に配布されます。 これを使用するには、Visual Studio 2017 または Visual Studio for Mac で Xamarin. iOS プロジェクトの **[参照]** ノードを右クリックし、 **monotouch.dialog**アセンブリへの参照を追加します。 次に、必要に応じて、ソースコードに `using MonoTouch.Dialog` ステートメントを追加します。

## <a name="elements-api-walkthrough"></a>Elements API のチュートリアル

Monotouch.dialog の[概要ダイアログ](~/ios/user-interface/monotouch.dialog/index.md)の記事では、MT のさまざまな部分について十分に理解しています。A. Elements API を使用して、それらをすべてまとめてアプリケーションに配置しましょう。

## <a name="setting-up-the-multi-screen-application"></a>マルチスクリーンアプリケーションの設定

画面作成プロセスを開始するために、Monotouch.dialog によって `DialogViewController`が作成され、`RootElement`が追加されます。

Monotouch.dialog を使用してマルチスクリーンアプリケーションを作成するには、次のことを行う必要があります。

1. `UINavigationController.` の作成
1. `DialogViewController.` の作成
1. `DialogViewController` を `UINavigationController.` のルートとして追加します。 
1. `DialogViewController.` に `RootElement` を追加する
1. `Sections` と `Elements` を `RootElement.` に追加する 

### <a name="using-a-uinavigationcontroller"></a>UINavigationController を使用する

ナビゲーションスタイルのアプリケーションを作成するには、`UINavigationController`を作成し、`AppDelegate`の `FinishedLaunching` メソッドの `RootViewController` として追加する必要があります。 `UINavigationController` が Monotouch.dialog で動作するようにするには、次に示すように `UINavigationController` に `DialogViewController` を追加します。

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
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

上記のコードでは、`RootElement` のインスタンスを作成し、`DialogViewController`に渡します。 `DialogViewController` には、常に、その階層の最上位に `RootElement` があります。 この例では、`RootElement` は、ナビゲーションコントローラーのナビゲーションバーでタイトルとして機能する "To Do List" という文字列を使用して作成されます。 この時点で、アプリケーションを実行すると、次のような画面が表示されます。

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Running the application will present the screen shown here")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

`Sections` と `Elements` の Monotouch.dialog の階層構造を使用して、画面をさらに追加する方法を見てみましょう。

### <a name="creating-the-dialog-screens"></a>ダイアログ画面の作成

`DialogViewController` は、Monotouch.dialog が画面を追加するために使用する `UITableViewController` サブクラスです。 Monotouch.dialog は、上に示したように、`DialogViewController`に `RootElement` を追加することによって画面を作成します。 `RootElement` には、テーブルのセクションを表す `Section` インスタンスを含めることができます。
これらのセクションは、要素、他のセクション、またはその他の `RootElements`で構成されています。 `RootElements`を入れ子にすることにより、Monotouch.dialog は次に示すように、ナビゲーションスタイルのアプリケーションを自動的に作成します。

### <a name="using-dialogviewcontroller"></a>診断ビューコントローラーの使用

`UITableViewController` サブクラスである `DialogViewController`には、`UITableView` がビューとして含まれています。 この例では、[ **+** ] ボタンがタップされるたびに、テーブルに項目を追加します。 `DialogViewController` が `UINavigationController`に追加されたため、次に示すように、`NavigationItem`の `RightBarButton` プロパティを使用して **+** ボタンを追加できます。

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

前に `RootElement` を作成したときに、ユーザーが **+** ボタンをタップしたときに要素を追加できるように、1つの `Section` インスタンスに渡しました。 次のコードを使用して、ボタンのイベントハンドラーでこれを実現できます。

```csharp
_addButton.Clicked += (sender, e) => {                
    ++n;
                
    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
    var taskElement = new RootElement (task.Name) {
        new Section () {
            new EntryElement (task.Name, "Enter task description", task.Description)
        },
        new Section () {
            new DateElement ("Due Date", task.DueDate)
        }
    };
    _rootElement [0].Add (taskElement);
};
```

このコードは、ボタンがタップされるたびに新しい `Task` オブジェクトを作成します。 `Task` クラスの単純な実装を次に示します。

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

タスクの `Name` プロパティは、新しいタスクごとにインクリメントされる `n` という名前のカウンター変数と共に `RootElement`のキャプションを作成するために使用されます。 Monotouch.dialog は、各 `taskElement` が追加されたときに `TableView` に追加される行に要素を変換します。

## <a name="presenting-and-managing-dialog-screens"></a>ダイアログ画面の表示と管理

Monotouch.dialog を使用して `RootElement`、タスクの詳細ごとに新しい画面が自動的に作成され、行が選択されたときに移動するようにしました。

タスクの詳細画面自体は、次の2つのセクションで構成されています。これらの各セクションには、1つの要素が含まれています。 最初の要素は、タスクの `Description` プロパティの編集可能な行を提供するために、`EntryElement` から作成されます。 要素を選択すると、次のようにテキスト編集用のキーボードが表示されます。

 [![](elements-api-walkthrough-images/03-create-task.png "When the element is selected, a keyboard for text editing is presented as shown")](elements-api-walkthrough-images/03-create-task.png#lightbox)

2番目のセクションには、タスクの `DueDate` プロパティを管理するための `DateElement` が含まれています。 日付を選択すると、次のように日付の選択が自動的に読み込まれます。

 [![](elements-api-walkthrough-images/04-date-picker.png "Selecting the date automatically loads a date picker as")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

`EntryElement` と `DateElement` の両方のケース (または Monotouch.dialog の任意のデータ入力要素) では、値に対する変更はすべて自動的に保持されます。 これを確認するには、日付を編集してから、ルート画面とさまざまなタスクの詳細を前後に移動します。詳細画面の値は保持されます。

## <a name="summary"></a>まとめ

この記事では、Monotouch.dialog Elements API の使用方法を示すチュートリアルについて説明しました。 MT でマルチスクリーンアプリケーションを作成するための基本的な手順について説明しています。`DialogViewController` の使用方法、および要素とセクションを追加して画面を作成する方法を含む D。 また、MT の使用方法についても説明しました。D `UINavigationController`と組み合わせて使用します。

## <a name="related-links"></a>関連リンク

- [MTDWalkthrough (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdwalkthrough)
- [Monotouch.dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github の Monotouch.dialog ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスの参照](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
