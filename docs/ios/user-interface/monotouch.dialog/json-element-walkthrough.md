---
title: JSON を使用して、Xamarin.iOS でユーザー インターフェイスを作成するには
description: MonoTouch.Dialog (mt.D) には、JSON データを使用してダイナミック UI 生成のサポートが含まれています。 このチュートリアルでは、JSONElement を使用して、アプリケーションに含まれているか、リモート Url から読み込まれたを JSON からユーザー インターフェイスを作成する方法を説明します。
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f9ba2cce1650260aa889e8282c091012ef8bbddc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790654"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>JSON を使用して、Xamarin.iOS でユーザー インターフェイスを作成するには

_MonoTouch.Dialog (mt.D) には、JSON データを使用してダイナミック UI 生成のサポートが含まれています。このチュートリアルでは、JSONElement を使用して、アプリケーションに含まれているか、リモート Url から読み込まれたを JSON からユーザー インターフェイスを作成する方法を説明します。_

MT.D は、JSON で宣言されているユーザー インターフェイスの作成をサポートします。 JSON、山を使用して要素の宣言時にD は、関連付けられている要素を自動的に作成されます。 ローカル ファイルで、解析されたか、JSON を読み込むことができる`JsonObject`インスタンス、またはリモートの Url もします。

MT.D には、さまざまな JSON を使用する場合、要素 API で使用できる機能がサポートされています。 たとえば、次のスクリーン ショットでは、アプリケーションが完全を使用して宣言 JSON:

[![](json-element-walkthrough-images/01-load-from-file.png "たとえば、このスクリーン ショットで、アプリケーションが完全に宣言されている JSON を使用して")](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ](json-element-walkthrough-images/01-load-from-file.png "など、このスクリーン ショットで、アプリケーションが完全を使用して宣言JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

みましょうから例を見直し、[要素 API チュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)チュートリアルでは、JSON を使用してタスクの詳細画面を追加する方法を示すです。

## <a name="json-walkthrough"></a>JSON のチュートリアル

このチュートリアルの例ではタスクを作成します。 最初の画面でタスクを選択すると、詳細画面が示すように表示されます。

 [![](json-element-walkthrough-images/03-task-list.png "ように詳細画面が表示される最初の画面でタスクを選択すると、")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON を作成します。

たとえば、という名前のプロジェクト内のファイルから、JSON を読み込むことが`task.json`です。 MT.D は、要素の API を反映した構文に準拠するように、JSON を想定しています。 コードからの要素の API を使用すると同じように JSON を使用する場合のセクションでは宣言セクション内でお要素を追加します。 セクションおよび JSON 内の要素を宣言するには文字列"sections"および「要素」それぞれキーとして使用します。 各要素に対して、関連付けられている要素の型が設定を使用して、`type`キー。 その他のすべての要素のプロパティは、キーとしてプロパティ名に設定されます。

たとえば、次の JSON は、セクションとタスクの詳細の要素について説明します。

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

上記の JSON の通知には、各要素の id が含まれています。 いずれかの要素には、実行時に参照する、id を含めることができます。 このコードで JSON を読み込む方法を表示現時点での使用方法が表示されます。

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>コードでの JSON の読み込み

JSON を定義した後、山に読み込む必要があります。D を使用して、`JsonElement`クラスです。 上で作成した JSON でのファイル名 sample.json でプロジェクトに追加して読み込みコンテンツのビルド アクションを指定、`JsonElement`次のコード行を呼び出すことと同じくらい簡単には。

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

これを追加おオンデマンドたびに、タスクが作成される、ので、以前の要素の API 例から次のようにクリックしたボタンを変更できます。

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>実行時の要素へのアクセス

JSON ファイルで宣言したときに、両方の要素に id を追加おに注意してください。 Id プロパティを使用して実行時のコードでそのプロパティを変更するのには各要素にアクセスすることができます。 たとえば、次のコードは、タスク オブジェクトから値を設定するエントリと日付の要素を参照します。

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Url からの JSON の読み込み

MT.JSON の外部 Url からのコンス トラクターに Url を渡すだけで動的な読み込みにもサポート D、`JsonElement`です。 MT.D は画面間を移動すると、要求時に、JSON で宣言された階層を展開します。 たとえば、次のような JSON ファイルのローカル web サーバーのルートにあります。

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

読み込みますこれを使用して、`JsonElement`次のコードのようにします。

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

ファイルの取得および山によって解析の実行時に、次のスクリーン ショットに示すように、ユーザーが 2 つ目のビューに移動したときに D:

 [![](json-element-walkthrough-images/04-json-web-example.png "ファイルが取得され、山によって解析ユーザーが 2 つ目のビューに移動するときに D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>まとめ

この記事では使用して作成する方法を示しました山を持つインターフェイスJSON から D. リモート Url から、アプリケーションと共にファイルに含まれる JSON を読み込む方法を示しました。 また、実行時に、JSON で説明されている要素にアクセスする方法を示しました。


## <a name="related-links"></a>関連リンク

- [MTDJsonDemo (sample)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [-スクリーン キャストである Miguel de Icaza により iOS ログイン画面 MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [要素の API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
