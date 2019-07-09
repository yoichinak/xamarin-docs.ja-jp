---
title: Xamarin.Forms の Web サービスの概要
description: このガイドでは、別の web サービスと通信する方法について説明する Xamarin.Forms のサンプル アプリケーションのチュートリアルを提供します。 別のサンプル アプリケーションを使用すると、各 web サービスが機能的に似ており共通クラスを共有します。
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: b34381ad587d623e66af1b581200094448851bf6
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658989"
---
# <a name="xamarinforms-web-services-introduction"></a>Xamarin.Forms の Web サービスの概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)

_このトピックでは、別の web サービスと通信する方法について説明する Xamarin.Forms のサンプル アプリケーションのチュートリアルを示します。別のサンプル アプリケーションを使用すると、各 web サービスが機能的に似ており共通クラスを共有します。_

以下に示すサンプルの to-do list アプリケーションを使用してを Xamarin.Forms での web サービスのバックエンドのさまざまな種類にアクセスする方法を示します。 機能を提供します。

- タスクの一覧を表示します。
- 追加、編集、およびタスクを削除します。
- タスクの状態 'done' に設定します。
- タスクの名前とメモ フィールドを読み上げます。

すべてのケースで、タスクは、web サービス経由でアクセスするバックエンドに格納されます。

アプリケーションが起動され、web サービスから取得したすべてのタスク一覧が表示され、新しいタスクを作成するユーザーは、ページが表示されます。 タスクをクリックすると、アプリケーションを 2 番目のページでタスクことができますが編集、保存、削除され、音声が移動します。 最終的なアプリケーションは、次のとおりです。

![](introduction-images/app-example-1.png "Todo アプリケーションの最初のページ")
![](introduction-images/app-example-2.png "Todo アプリケーションの 2 番目のページ")

このガイド内の各トピックへのダウンロード リンクを提供する、*異なる*web サービスのバックエンドの特定の種類を示す、アプリケーションのバージョン。 各 web サービス スタイルに関連するページに関連するサンプル コードをダウンロードします。

## <a name="understand-the-application-anatomy"></a>アプリケーションの構造を理解します。

各サンプル アプリケーション用の共有コード プロジェクトは、3 つのメイン フォルダーで構成されます。

|フォルダー|目的|
|--- |--- |
|データ|クラスとデータ項目を管理し、web サービスと通信するために使用するインターフェイスが含まれています。 ここには、少なくとも、`TodoItemManager`クラスのプロパティによって公開される、 `App` web サービスの操作を呼び出すクラス。|
|モデル|アプリケーションのデータ モデル クラスが含まれています。 ここには、少なくとも、`TodoItem`クラスは、アプリケーションによって使用されるデータの 1 つの項目をモデル化します。 フォルダーは、ユーザー データのモデリングに使用されるその他のクラスも含めることができます。|
|Views|アプリケーションのページが含まれています。 通常から成る、`TodoListPage`と`TodoItemPage`クラス、および認証のために使用されるその他のクラス。|

各アプリケーション用の共有コード プロジェクトは、多数の重要なファイルで構成されます。

|ファイル|目的|
|--- |--- |
|Constants.cs|`Constants`クラスは、web サービスと通信するために、アプリケーションで使用される任意の定数を指定します。 これらの定数は、プロバイダー上に作成、個人のバックエンド サービスにアクセスする更新が必要です。|
|ITextToSpeech.cs|`ITextToSpeech`インターフェイスでは、ことを指定します、`Speak`メソッドを実装するクラスによって提供される必要があります。|
|Todo.cs|`App`は、各プラットフォームでアプリケーションによって表示される両方の最初のページをインスタンス化を担当するクラスと`TodoItemManager`web サービス操作の呼び出しに使用されるクラスです。|

### <a name="view-pages"></a>ページの表示

サンプル アプリケーションの大部分には、少なくとも 2 つのページが含まれています。

- **TodoListPage** – このページの一覧を表示する`TodoItem`インスタンス、およびのチェック マーク アイコン場合、`TodoItem.Done`プロパティは`true`します。 移動する項目をクリックすると、`TodoItemPage`します。 新しい項目を作成してをクリックしてさらに、 *+* シンボル。
- **TodoItemPage** – このページには、選択した詳細が表示されます。 `TodoItem`、し、編集、保存、削除、および話されるようになります。

さらに、一部のサンプル アプリケーションには、ユーザーの認証プロセスの管理に使用される追加のページが含まれます。

### <a name="model-the-data"></a>データをモデルします。

各サンプル アプリケーションを使用して、`TodoItem`を表示し、記憶域の web サービスに送信されるデータをモデル化するクラス。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

`ID`プロパティは、それぞれを一意に識別するために使用`TodoItem`インスタンス、および更新または削除するデータを識別するために各 web サービスによって使用されます。

### <a name="invoke-web-service-operations"></a>Web サービスの操作を呼び出す

Web サービスの操作を使用してアクセスされる、`TodoItemManager`クラスやクラスのインスタンスにアクセスできる、`App.TodoManager`プロパティ。 `TodoItemManager`クラスは、web サービスの操作を呼び出す次のメソッドを提供します。

- **GetTasksAsync** – このメソッドを使用して設定を`ListView`の control 権限、`TodoListPage`で、`TodoItem`インスタンスは、web サービスから取得します。
- **SaveTaskAsync** – このメソッドを使用して作成または更新を`TodoItem`web サービスのインスタンス。
- **DeleteTaskAsync** – このメソッドを使用して、削除、 `TodoItem` web サービスのインスタンス。

さらに、一部のサンプル アプリケーションに含める追加のメソッドで、`TodoItemManager`クラスは、ユーザーの認証プロセスを管理するために使用します。

Web サービスの操作を直接呼び出すのではなく、`TodoItemManager`メソッドに挿入される依存クラスのメソッドを呼び出し、`TodoItemManager`コンス トラクター。 たとえば、1 つのサンプル アプリケーションを挿入、`RestService`にクラス、 `TodoItemManager` REST Api を使用して、データにアクセスする実装を提供するコンス トラクター。

## <a name="related-links"></a>関連リンク

- [ASMX (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [WCF (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [REST (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
