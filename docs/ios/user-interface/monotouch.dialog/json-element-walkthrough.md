---
title: JSON を使用して Xamarin.iOS でのユーザー インターフェイスを作成するには
description: MonoTouch.Dialog (mt.D) JSON データを使用して、UI の動的生成をサポートしています。 このチュートリアルで、JSONElement を使用して、アプリケーションでは、含まれている、またはリモートの Url から読み込まれた JSON からユーザー インターフェイスを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 94cef78bb7eedc03192071f17af765ebb702e260
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038496"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>JSON を使用して Xamarin.iOS でのユーザー インターフェイスを作成するには

_MonoTouch.Dialog (mt.D) JSON データを使用して、UI の動的生成をサポートしています。このチュートリアルで、JSONElement を使用して、アプリケーションでは、含まれている、またはリモートの Url から読み込まれた JSON からユーザー インターフェイスを作成する方法について説明します。_

MT.D には、JSON で宣言されているユーザー インターフェイスの作成がサポートされています。 要素は、山の JSON を使用して宣言された場合D するは、関連付けられている要素を自動的に作成されます。 ローカル ファイルで、解析されたか、JSON を読み込むことが`JsonObject`インスタンス、またはリモートの Url も使用できます。

MT.D には、さまざまな JSON を使用する場合は、要素 API で使用できる機能がサポートされています。 たとえば、次のスクリーン ショットで、アプリケーションが完全を使用して宣言 JSON:

[![](json-element-walkthrough-images/01-load-from-file.png "たとえば、このスクリーン ショットでは、アプリケーションが完全に宣言されている JSON を使用して")](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ](json-element-walkthrough-images/01-load-from-file.png "など、このスクリーン ショットでは、アプリケーションが完全を使用して宣言JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

例では、ここでも、[要素 API チュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)JSON を使用してタスクの詳細画面を追加する方法を示すチュートリアル。

## <a name="setting-up-mtd"></a>山の設定D

MT.D は、Xamarin.iOS で分散されます。 これを使用するを右クリックし、**参照**、Xamarin.iOS のノードは、Visual Studio 2017 または Visual Studio for Mac プロジェクトし、への参照を追加、 **MonoTouch.Dialog 1**アセンブリ。 次に、追加`using MonoTouch.Dialog`ソース内のステートメントは、必要に応じてコードします。

## <a name="json-walkthrough"></a>JSON のチュートリアル

このチュートリアルの例は、タスクを作成できます。 最初の画面でタスクを選択すると、詳細画面が示すように表示されます。

 [![](json-element-walkthrough-images/03-task-list.png "示すように、詳細画面が表示される最初の画面でタスクを選択すると、")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON の作成

この例では、という名前のプロジェクト内のファイルから、JSON をロードしましょう`task.json`します。 MT.D は、要素の API を反映した構文に準拠するように JSON を想定しています。 コードから要素 API を使用して、同様のセクションでは宣言 JSON を使用する場合とセクション内で要素を追加します。 セクションでは、JSON 内の要素を宣言するには文字列「セクション」と「要素」それぞれキーとして使用します。 使用して、関連付けられている要素の型を設定の各要素に対して、`type`キー。 その他のすべての要素のプロパティは、プロパティ名をキーとして設定されます。

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

上記の JSON に注意してくださいには、各要素の id が含まれています。 任意の要素には、実行時に参照する、id を含めることができます。 これをコードで JSON を読み込む方法を表示するとすぐには、使用する方法が表示されます。

## <a name="loading-the-json-in-code"></a>コードで JSON の読み込み

JSON を定義した後に山に読み込む必要があります。D を使用して、`JsonElement`クラス。 上記で作成した JSON ファイルと仮定すると名前 sample.json をプロジェクトに追加して読み込み、コンテンツのビルド アクションを指定、`JsonElement`は次のコード行を呼び出すことだけです。

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

これを追加する、必要に応じて毎回タスクを作成したことはので、次のように、以前の要素の API の例からクリックされたボタンを変更できます。

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>実行時に要素にアクセスします。

JSON ファイルで宣言するときに、両方の要素に id を追加しましたを思い出してください。 実行時のコードでは、そのプロパティを変更するのには、各要素にアクセスするのに id プロパティを使用できます。 たとえば、次のコードは、タスク オブジェクトから値を設定するエントリと日付の要素を参照します。

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

## <a name="loading-json-from-a-url"></a>Url からの JSON の読み込み

MT.コンス トラクターに、Url を渡すだけで、外部の Url からの JSON を動的な読み込みにもサポート D、`JsonElement`します。 MT.D は画面間を移動すると、必要に応じて、JSON で宣言された階層を展開します。 たとえば、ローカル web サーバーのルートにある次などの JSON ファイルを検討します。

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

私たちはこれを使用して読み込む、`JsonElement`次のコードのようにします。

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

ファイルの取得し、山によって解析の実行時に次のスクリーン ショットに示すように、ユーザーが 2 つ目のビューに移動したときに D:

 [![](json-element-walkthrough-images/04-json-web-example.png "ファイルが取得され、山によって解析ユーザーが 2 つ目のビューに移動すると、D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>まとめ

この記事を使用して作成する方法を示しました山とのインターフェイスJSON から D. アプリケーションと共に、リモート Url からのファイルに含まれる JSON を読み込む方法を示しました。 実行時に JSON で説明する要素にアクセスする方法も示しました。

## <a name="related-links"></a>関連リンク

- [MTDJsonDemo (sample)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [MonoTouch.Dialog とスクリーン キャスト - Miguel de Icaza の作成、iOS のログイン画面](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [要素の API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
