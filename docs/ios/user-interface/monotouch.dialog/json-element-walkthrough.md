---
title: JSON を使用した Xamarin でのユーザーインターフェイスの作成
description: Monotouch.dialog (MT.D) では、JSON データを使用した動的な UI 生成がサポートされています。 このチュートリアルでは、JSONElement を使用して、アプリケーションに含まれているか、リモート Url から読み込まれた JSON からユーザーインターフェイスを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 8ad1bf746c129cfb5cfbc8b3e04b871224df486d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936579"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>JSON を使用した Xamarin でのユーザーインターフェイスの作成

_Monotouch.dialog (MT.D) では、JSON データを使用した動的な UI 生成がサポートされています。このチュートリアルでは、JSONElement を使用して、アプリケーションに含まれているか、リモート Url から読み込まれた JSON からユーザーインターフェイスを作成する方法について説明します。_

MT.D では、JSON で宣言されたユーザーインターフェイスの作成をサポートしています。 要素が JSON (MT) を使用して宣言されている場合。関連する要素が自動的に作成されます。 JSON は、ローカルファイル、解析さ `JsonObject` れたインスタンス、またはリモート Url から読み込むことができます。

MT.D は、JSON を使用するときに Elements API で使用できるすべての機能をサポートしています。 たとえば、次のスクリーンショットのアプリケーションは、JSON を使用して完全に宣言されています。

[ ![ たとえば、このスクリーンショットのアプリケーションは json を使用して完全に宣言さ](json-element-walkthrough-images/01-load-from-file.png)](json-element-walkthrough-images/01-load-from-file.png#lightbox)れています。 [ ![ たとえば、このスクリーンショットのアプリケーションは json を使用して完全に宣言されてい](json-element-walkthrough-images/01-load-from-file.png)](json-element-walkthrough-images/01-load-from-file.png#lightbox)ます。

ここでは、JSON を使用してタスクの詳細画面を追加する方法を示す[ELEMENTS API チュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)のチュートリアルの例について説明します。

## <a name="setting-up-mtd"></a>MT を設定しています。A

MT.D は、Xamarin. iOS と共に配布されます。 これを使用するには、Visual Studio 2017 または Visual Studio for Mac で Xamarin. iOS プロジェクトの [**参照**] ノードを右クリックし、 **monotouch.dialog**アセンブリへの参照を追加します。 次に、 `using MonoTouch.Dialog` 必要に応じて、ソースコードにステートメントを追加します。

## <a name="json-walkthrough"></a>JSON チュートリアル

このチュートリアルの例では、タスクを作成できます。 最初の画面でタスクを選択すると、次のような詳細画面が表示されます。

 [![最初の画面でタスクを選択すると、詳細画面が表示されます。](json-element-walkthrough-images/03-task-list.png)](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON の作成

この例では、という名前のプロジェクトのファイルから JSON を読み込み `task.json` ます。 MT.D は、JSON が Elements API を反映する構文に準拠していることを想定しています。 コードから Elements API を使用する場合と同様に、JSON を使用する場合は、セクションを宣言し、これらのセクション内で要素を追加します。 JSON でセクションと要素を宣言するには、文字列 "sections" と "elements" をそれぞれキーとして使用します。 要素ごとに、キーを使用して、関連付けられている要素の型が設定され `type` ます。 その他のすべての elements プロパティは、キーとしてプロパティ名を使用して設定されます。

たとえば、次の JSON では、タスクの詳細のセクションと要素について説明しています。

```json
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

上記の JSON には、各要素の id が含まれています。 要素には、実行時に参照するための id を含めることができます。 コードで JSON を読み込む方法を説明するときに、この方法がどのように使用されるかをご紹介します。

## <a name="loading-the-json-in-code"></a>コードでの JSON の読み込み

JSON を定義したら、それを MT に読み込む必要があります。D クラスを使用する `JsonElement` 。 前の手順で作成した JSON のファイルがプロジェクトに追加され、そのファイルの名前が sample.jsであり、コンテンツのビルドアクションが指定されている場合、を読み込むの `JsonElement` は、次のコード行を呼び出した場合と同じです。

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

ここでは、タスクが作成されるたびにこの要求を追加するため、次のように、前の Elements API の例でクリックしたボタンを変更できます。

```csharp
_addButton.Clicked += (sender, e) => {
    ++n;

    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

    var taskElement = JsonElement.FromFile ("task.json");

    _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>実行時の要素へのアクセス

ここでは、JSON ファイルで宣言したときに、両方の要素に id を追加しました。 Id プロパティを使用して、実行時に各要素にアクセスし、コードでプロパティを変更できます。 たとえば、次のコードでは、task オブジェクトの値を設定するために、entry 要素と date 要素を参照しています。

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

## <a name="loading-json-from-a-url"></a>Url から JSON を読み込んでいます

MT.また、のコンストラクターに Url を渡すだけで、外部 Url からの JSON の動的な読み込みもサポートさ `JsonElement` れます。 MT.D は、画面間を移動するときに、オンデマンドで JSON で宣言された階層を拡張します。 たとえば、次のような JSON ファイルをローカル web サーバーのルートに配置するとします。

```json
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

これは、次のコードのようにを使用して読み込むことができ `JsonElement` ます。

```csharp
_rootElement = new RootElement ("Json Example") {
    new Section ("") {
        new JsonElement ("Load from url", "http://localhost/sample.json")
    }
};
```

実行時には、ファイルが MT によって取得および解析されます。次のスクリーンショットに示すように、ユーザーが2番目のビューに移動したとき。

 [![ファイルは MT によって取得および解析されます。ユーザーが2番目のビューに移動したとき](json-element-walkthrough-images/04-json-web-example.png)](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、MT で using インターフェイスを作成する方法について説明しました。JSON からの D。 このチュートリアルでは、ファイルに含まれる JSON をアプリケーションとリモート Url から読み込む方法を説明しました。 また、実行時に JSON で記述された要素にアクセスする方法も示しました。

## <a name="related-links"></a>関連リンク

- [MTDJsonDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdjsondemo)
- [Monotouch.dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [Elements API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [リフレクション API のチュートリアル](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github の Monotouch.dialog ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスの参照](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
