---
title: このサンプルを理解します。
description: このトピックでは、別の web サービスと通信する方法については、Xamarin.Forms サンプル アプリケーションのチュートリアルを提供します。 各 web サービスは、別個のサンプル アプリケーションを使用するときに、機能的によく似た、一般的なクラスを共有します。
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: e9738a766762dd64cdfbb034d4eaa54f76aca311
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="understanding-the-sample"></a>このサンプルを理解します。

_このトピックでは、別の web サービスと通信する方法については、Xamarin.Forms サンプル アプリケーションのチュートリアルを提供します。各 web サービスは、別個のサンプル アプリケーションを使用するときに、機能的によく似た、一般的なクラスを共有します。_

以下に示すサンプルの to do リスト アプリケーションは、Xamarin.Forms を使用した web サービスのバックエンドのさまざまな種類にアクセスする方法を示すために使用されます。 機能を提供します。

- タスクの一覧を表示します。
- 追加、編集、およびタスクを削除します。
- 'Done' に、タスクの状態を設定します。
- タスクの名前とメモ フィールドを話します。

すべての場合、タスクは、web サービスからアクセスするバックエンドに格納されます。

アプリケーションを起動すると、web サービスから取得したすべてのタスクを一覧表示して、新しいタスクを作成するユーザーをできるように、ページが表示されます。 タスクをクリックすると、2 番目のページを使用している場所タスクでく編集、保存、削除、および読み上げにアプリケーションが移動します。 最終的なアプリケーションは、次のとおりです。

![](walkthrough-images/app-example-1.png "Todo アプリケーション - 最初のページ")
![](walkthrough-images/app-example-2.png "Todo アプリケーション - 2 ページ目")

このガイドの各トピックを提供するダウンロード リンク、*異なる*を web サービスのバックエンドの特定の種類を示すアプリケーションのバージョン。 各 web サービス スタイルに関連するページに関連するサンプル コードをダウンロードします。

## <a name="understanding-the-application-anatomy"></a>アプリケーション構造を理解します。

各サンプル アプリケーションについては、PCL プロジェクトは、次の 3 つのメイン フォルダーで構成されます。

|フォルダー|目的|
|--- |--- |
|データ|データ項目を管理し、web サービスとの通信に使用するインターフェイスとクラスが含まれています。 少なくとも、これが含まれています、`TodoItemManager`のプロパティによって公開されるクラス、 `App` web サービス操作の呼び出しにクラスです。|
|モデル|アプリケーションのデータ モデル クラスを含みます。 少なくとも、これが含まれています、`TodoItem`クラスは、アプリケーションによって使用されるデータの単一の項目をモデル化します。 フォルダーには、ユーザー データのモデルで使用されるその他のクラスも指定できます。|
|ビュー|アプリケーション ページが含まれます。 通常から成る、`TodoListPage`と`TodoItemPage`クラス、および認証の目的で使用されるその他のクラスです。|

重要なファイルの数もアプリケーションごとに PCL プロジェクトの構成します。

|ファイル|目的|
|--- |--- |
|Constants.cs|`Constants`クラスは、web サービスと通信するために、アプリケーションで使用される任意の定数を指定します。 これらの定数は、プロバイダーに対して作成された個人のバックエンド サービスにアクセスする更新が必要です。|
|ITextToSpeech.cs|`ITextToSpeech`ことを指定するインターフェイス、`Speak`メソッドを実装する任意のクラスによって提供される必要があります。|
|Todo.cs|`App`を各プラットフォームでのアプリケーションによって表示される両方の最初のページをインスタンス化するクラスと`TodoItemManager`web サービス操作の呼び出しに使用されるクラスです。|

### <a name="viewing-pages"></a>ページの表示

サンプル アプリケーションの大部分には、少なくとも 2 つのページが含まれています。

- **TodoListPage** – このページの一覧を表示する`TodoItem`インスタンス、およびのチェック マーク アイコン場合、`TodoItem.Done`プロパティは`true`します。 移動するアイテムをクリックすると、`TodoItemPage`です。 新しい項目を作成してをクリックするとさらに、 *+*シンボル。
- **TodoItemPage** – このページには、選択した詳細が表示されます。 `TodoItem`、し、編集、保存、削除、および読み上げすることができます。

さらに、いくつかのサンプル アプリケーションは、ユーザーの認証プロセスの管理に使用される追加のページを含んでいます。

### <a name="modeling-the-data"></a>データをモデリング

各サンプル アプリケーションを使用して、`TodoItem`表示され、記憶域用の web サービスに送信されるデータをモデル化するクラス。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

`ID`プロパティを使用して、それぞれを一意に識別`TodoItem`、インスタンス化し、各 web サービスによって更新または削除するデータを識別するために使用します。

### <a name="invoking-web-service-operations"></a>Web サービスの操作を呼び出す

Web サービス操作にはを通じてアクセス、`TodoItemManager`クラス、およびクラスのインスタンスを通じてアクセスできる、`App.TodoManager`プロパティです。 `TodoItemManager`クラスは、web サービス操作の呼び出しに次のメソッドを提供します。

- **GetTasksAsync** – このメソッドは、設定に使用されます、`ListView`の control 権限、`TodoListPage`で、 `TodoItem` web サービスからインスタンスを取得します。
- **SaveTaskAsync** – このメソッドを作成または更新に使用、 `TodoItem` web サービスのインスタンス。
- **DeleteTaskAsync** – このメソッドを使用して、削除、 `TodoItem` web サービスのインスタンス。

さらに、サンプル アプリケーションに含める追加のメソッド、`TodoItemManager`クラスは、ユーザーの認証プロセスを管理するために使用します。

Web サービスの操作を直接呼び出すのではなく、`TodoItemManager`メソッドに組み込まれている依存クラスのメソッドを呼び出し、`TodoItemManager`コンス トラクターです。 たとえば、1 つのサンプル アプリケーションでは挿入、`SimpleDBStorage`にクラス、 `TodoItemManager` Amazon の SimpleDB サービスに対して操作を呼び出す実装を提供するコンス トラクターです。

### <a name="translating-text-to-speech"></a>Text to Speech を変換します。

サンプル アプリケーションの大部分には、値を話すに音声合成 (TTS) 機能が含まれて、`TodoItem.Name`と`TodoItem.Notes`プロパティです。 これは、`OnSpeakActivated`内のイベント ハンドラー、`TodoItemPage`クラスに、次のコード例に示すようにします。

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

このメソッドを呼び出すだけ、`Speak`プラットフォーム固有の仕様によって実装されるメソッド`Speech`クラスです。 各`Speech`クラスが実装する、`ITextToSpeech`インターフェイス、プラットフォーム固有のスタートアップ コードは、のインスタンスを作成し、`Speech`経由でアクセスできるクラス、`App.Speech`プロパティです。

## <a name="summary"></a>まとめ

このトピックでは、別の web サービスと通信する方法を示すために使用される Xamarin.Forms サンプル アプリケーションのチュートリアルが用意されています。 各 web サービスで別のサンプル アプリケーションを使用して、すべてに基づいている同じユーザー インターフェイスとビジネス ロジック前述 - web サービス データ ストレージ機構のみが異なる。


## <a name="related-links"></a>関連リンク

- [ASMX バージョン (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [WCF のバージョン (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [REST バージョン (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Azure のバージョン (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
- [Amazon Web Services バージョン (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS)
