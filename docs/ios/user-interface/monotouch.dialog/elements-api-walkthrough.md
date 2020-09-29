---
title: Elements API を使用して Xamarin iOS アプリケーションを作成する
description: この記事は、「Monotouch.dialog の概要」の記事に記載されている情報に基づいています。 ここでは、Monotouch.dialog (MT) の使用方法を示すチュートリアルを示します。D) Elements API を使用して、MT を使用したアプリケーションの構築をすぐに始めることができます。
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: b95066379e7b6845bf1265b43681aec83b130aa4
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436755"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Elements API を使用して Xamarin iOS アプリケーションを作成する

_この記事は、「Monotouch.dialog の概要」の記事に記載されている情報に基づいています。ここでは、Monotouch.dialog (MT) の使用方法を示すチュートリアルを示します。D) Elements API を使用して、MT を使用したアプリケーションの構築をすぐに始めることができます。_

このチュートリアルでは、MT を使用します。D Elements API は、タスク一覧を表示するアプリケーションのマスター/詳細スタイルを作成します。 ユーザーがナビゲーションバーでボタンを選択すると、 **+** タスクのテーブルに新しい行が追加されます。 行を選択すると、次に示すように、タスクの説明と期限を更新できる詳細画面に移動します。

[![行を選択すると、詳細画面に移動し、タスクの説明と期日を更新することができます。](elements-api-walkthrough-images/01-task-list-app.png)](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

## <a name="setting-up-mtd"></a>MT を設定しています。A

MT.D は、Xamarin. iOS と共に配布されます。 これを使用するには、Visual Studio 2017 または Visual Studio for Mac で Xamarin. iOS プロジェクトの [ **参照** ] ノードを右クリックし、 **monotouch.dialog** アセンブリへの参照を追加します。 次に、 `using MonoTouch.Dialog` 必要に応じて、ソースコードにステートメントを追加します。

## <a name="elements-api-walkthrough"></a>Elements API のチュートリアル

Monotouch.dialog の [概要ダイアログ](~/ios/user-interface/monotouch.dialog/index.md) の記事では、MT のさまざまな部分について十分に理解しています。 Elements API を使用して、それらをすべてまとめてアプリケーションに配置しましょう。

## <a name="setting-up-the-multi-screen-application"></a>マルチスクリーンアプリケーションの設定

画面作成プロセスを開始するために、Monotouch.dialog はを作成し、 `DialogViewController` を追加します `RootElement` 。

Monotouch.dialog を使用してマルチスクリーンアプリケーションを作成するには、次のことを行う必要があります。

1. 認証要求の処理に使用する `UINavigationController.`
1. 認証要求の処理に使用する `DialogViewController.`
1. を、の `DialogViewController` ルートとして追加します。  `UINavigationController.` 
1. をに追加し `RootElement` ます。  `DialogViewController.`
1. `Sections`と `Elements` をに追加します。`RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>UINavigationController を使用する

ナビゲーションスタイルのアプリケーションを作成するには、を作成して、の `UINavigationController` メソッドのとして追加する必要があり `RootViewController` `FinishedLaunching` `AppDelegate` ます。 Monotouch.dialog を操作するには、次のようにを `UINavigationController` に追加します。 `DialogViewController` `UINavigationController`

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

上のコードでは、のインスタンスを作成し、そのインスタンスをに `RootElement` 渡し `DialogViewController` ます。 は、 `DialogViewController` 常に `RootElement` 階層の最上位にあります。 この例では、は、 `RootElement` ナビゲーションコントローラーのナビゲーションバーでタイトルとして機能する "To Do List" という文字列を使用して作成されます。 この時点で、アプリケーションを実行すると、次のような画面が表示されます。

 [![アプリケーションを実行すると、ここに表示される画面が表示されます。](elements-api-walkthrough-images/02-to-do-list-screen-.png)](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

との Monotouch.dialog の階層構造を使用して `Sections` 、画面をさらに追加する方法を見てみましょう。 `Elements`

### <a name="creating-the-dialog-screens"></a>ダイアログ画面の作成

は、 `DialogViewController` `UITableViewController` monotouch.dialog が画面を追加するために使用するサブクラスです。 Monotouch.dialog は、 `RootElement` 上に示したように、をに追加することによって画面を作成します。 `DialogViewController` は、 `RootElement` `Section` テーブルのセクションを表すインスタンスを持つことができます。
これらのセクションは、要素、他のセクション、またはその他のセクションで構成されてい `RootElements` ます。 `RootElements`Monotouch.dialog を入れ子にすると、次に示すように、ナビゲーションスタイルのアプリケーションが自動的に作成されます。

### <a name="using-dialogviewcontroller"></a>診断ビューコントローラーの使用

サブクラスであるは、 `DialogViewController` `UITableViewController` `UITableView` ビューとしてを持ちます。 この例では、ボタンがタップされるたびに、テーブルに項目を追加し **+** ます。 は `DialogViewController` に追加されたため `UINavigationController` 、 `NavigationItem` 次に示すように、のプロパティを使用し `RightBarButton` **+** てボタンを追加できます。

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

以前にを作成したときに、ユーザーがボタンをタップしたときに `RootElement` 要素を追加できるように、1つのインスタンスに渡しました `Section` **+** 。 次のコードを使用して、ボタンのイベントハンドラーでこれを実現できます。

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

このコードでは `Task` 、ボタンがタップされるたびに新しいオブジェクトが作成されます。 クラスの単純な実装を次に示し `Task` ます。

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

タスクの `Name` プロパティは、のキャプションと、 `RootElement` `n` 新しいタスクごとにインクリメントされるという名前のカウンター変数を作成するために使用されます。 Monotouch.dialog を追加すると、要素がに追加された行に変換されます。 `TableView` `taskElement`

## <a name="presenting-and-managing-dialog-screens"></a>ダイアログ画面の表示と管理

`RootElement`Monotouch.dialog がタスクの詳細ごとに新しい画面を自動的に作成し、行を選択したときに移動するようにを使用しました。

タスクの詳細画面自体は、次の2つのセクションで構成されています。これらの各セクションには、1つの要素が含まれています。 最初の要素は、 `EntryElement` タスクのプロパティの編集可能な行を提供するために、から作成され `Description` ます。 要素を選択すると、次のようにテキスト編集用のキーボードが表示されます。

 [![要素を選択すると、テキスト編集用のキーボードが表示されます。](elements-api-walkthrough-images/03-create-task.png)](elements-api-walkthrough-images/03-create-task.png#lightbox)

2番目のセクションには、タスクのプロパティを管理するためのが含まれてい `DateElement` `DueDate` ます。 日付を選択すると、次のように日付の選択が自動的に読み込まれます。

 [![日付を選択すると、日付の選択が自動的に読み込まれます。](elements-api-walkthrough-images/04-date-picker.png)](elements-api-walkthrough-images/04-date-picker.png#lightbox)

との両方 `EntryElement` の `DateElement` ケース (または monotouch.dialog の任意のデータ入力要素) では、値に対する変更は自動的に保持されます。 これを確認するには、日付を編集してから、ルート画面とさまざまなタスクの詳細を前後に移動します。詳細画面の値は保持されます。

## <a name="summary"></a>まとめ

この記事では、Monotouch.dialog Elements API の使用方法を示すチュートリアルについて説明しました。 MT でマルチスクリーンアプリケーションを作成するための基本的な手順について説明しています。D。を使用する方法、 `DialogViewController` および要素とセクションを追加して画面を作成する方法を含みます。 また、MT の使用方法についても説明しました。D との組み合わせ `UINavigationController` 。

## <a name="related-links"></a>関連リンク

- [MTDWalkthrough (サンプル)](/samples/xamarin/ios-samples/mtdwalkthrough)
- [Monotouch.dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github の Monotouch.dialog ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスの参照](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)