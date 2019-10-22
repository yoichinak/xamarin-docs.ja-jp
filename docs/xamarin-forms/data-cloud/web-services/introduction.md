---
title: Xamarin. Forms Web Services の概要
description: このガイドでは、さまざまな web サービスとの通信方法を示す Xamarin サンプルアプリケーションのチュートリアルを提供します。 各 web サービスは個別のサンプルアプリケーションを使用しますが、機能的に類似した共通クラスを共有します。
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: bbeab6a6ab0d4a9d0e3a962240317fc0d54f9e25
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656638"
---
# <a name="xamarinforms-web-services-introduction"></a>Xamarin. Forms Web Services の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_このトピックでは、さまざまな web サービスとの通信方法を示す Xamarin サンプルアプリケーションのチュートリアルを提供します。各 web サービスは個別のサンプルアプリケーションを使用しますが、機能的に類似した共通クラスを共有します。_

次に示すサンプルの to do list アプリケーションは、Xamarin. Forms でさまざまな種類の web サービスバックエンドにアクセスする方法を示すために使用されます。 次の機能を提供します。

- タスクの一覧を表示します。
- タスクを追加、編集、および削除します。
- タスクの状態を [完了] に設定します。
- タスクの [名前] フィールドと [メモ] フィールドを読み上げます。

どのような場合でも、タスクは web サービスを通じてアクセスされるバックエンドに格納されます。

アプリケーションが起動すると、web サービスから取得したすべてのタスクを一覧表示するページが表示され、ユーザーは新しいタスクを作成できます。 タスクをクリックすると、アプリケーションが2番目のページに移動します。このページでは、タスクを編集、保存、削除、および読み上げることができます。 最終的なアプリケーションは、次のとおりです。

![](introduction-images/app-example-1.png "Todo application - first page")
![](introduction-images/app-example-2.png "Todo application - second page")

このガイドの各トピックでは、特定の種類の web サービスバックエンドを示す、*別*のバージョンのアプリケーションへのダウンロードリンクを提供します。 各 web サービススタイルに関連するページで、関連するサンプルコードをダウンロードします。

## <a name="understand-the-application-anatomy"></a>アプリケーションの構造を理解する

各サンプルアプリケーションの共有コードプロジェクトは、次の3つの主要なフォルダーで構成されています。

|フォルダー|目的|
|--- |--- |
|データ|データ項目を管理し、web サービスと通信するために使用されるクラスとインターフェイスが含まれています。 これには、少なくとも、web サービス操作を呼び出すために `App` クラスのプロパティを介して公開される `TodoItemManager` クラスが含まれます。|
|モデル|アプリケーションのデータモデルクラスが含まれています。 これには、少なくとも、アプリケーションによって使用される1つのデータ項目をモデル化する `TodoItem` クラスが含まれます。 このフォルダーには、ユーザーデータのモデル化に使用する追加のクラスを含めることもできます。|
|Views|アプリケーションのページが含まれています。 これは通常、`TodoListPage` クラスと `TodoItemPage` クラスと、認証のために使用される追加のクラスで構成されます。|

各アプリケーションの共有コードプロジェクトも、いくつかの重要なファイルで構成されています。

|ファイル|目的|
|--- |--- |
|Constants.cs|@No__t_0 クラス。アプリケーションが web サービスと通信するために使用する定数を指定します。 これらの定数は、プロバイダーで作成された個人用バックエンドサービスにアクセスするために更新する必要があります。|
|ITextToSpeech.cs|@No__t_0 インターフェイス。実装するクラスが `Speak` メソッドを提供する必要があることを指定します。|
|Todo.cs|各プラットフォームでアプリケーションによって表示される最初のページと、web サービス操作を呼び出すために使用される `TodoItemManager` クラスの両方をインスタンス化するための `App` クラス。|

### <a name="view-pages"></a>ページの表示

サンプルアプリケーションの大部分には、少なくとも2つのページが含まれています。

- **TodoListPage** –このページには `TodoItem` インスタンスの一覧と、`TodoItem.Done` プロパティが `true` 場合のティックアイコンが表示されます。 項目をクリックすると、`TodoItemPage` に移動します。 また、 *+* 記号をクリックして新しい項目を作成することもできます。
- **TodoItemPage** –このページには、選択した `TodoItem` の詳細が表示され、編集、保存、削除、および読み上げを行うことができます。

また、一部のサンプルアプリケーションには、ユーザー認証プロセスを管理するために使用される追加のページが含まれています。

### <a name="model-the-data"></a>データのモデル化

各サンプルアプリケーションは、`TodoItem` クラスを使用して、表示され、web サービスに送信されるデータをストレージ用にモデル化します。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

@No__t_0 プロパティは、各 `TodoItem` インスタンスを一意に識別するために使用され、更新または削除するデータを識別するために各 web サービスによって使用されます。

### <a name="invoke-web-service-operations"></a>Web サービス操作の呼び出し

Web サービス操作には `TodoItemManager` クラスを使用してアクセスします。クラスのインスタンスには、`App.TodoManager` プロパティを使用してアクセスできます。 @No__t_0 クラスには、web サービス操作を呼び出すための次のメソッドが用意されています。

- **Gettasksasync** –このメソッドは、web サービスから取得した `TodoItem` インスタンスを使用して `TodoListPage` の `ListView` コントロールを設定するために使用されます。
- **Savetaskasync** –このメソッドは、web サービス上の `TodoItem` インスタンスを作成または更新するために使用されます。
- **Deletetaskasync** –このメソッドは、web サービス上の `TodoItem` インスタンスを削除するために使用されます。

また、一部のサンプルアプリケーションには、ユーザー認証プロセスを管理するために使用される、`TodoItemManager` クラスの追加のメソッドが含まれています。

@No__t_0 メソッドは、web サービス操作を直接呼び出すのではなく、`TodoItemManager` コンストラクターに挿入される依存クラスのメソッドを呼び出します。 たとえば、1つのサンプルアプリケーションは、`RestService` クラスを `TodoItemManager` コンストラクターに挿入して、REST Api を使用してデータにアクセスする実装を提供します。

## <a name="related-links"></a>関連リンク

- [ASMX (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [WCF (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [REST (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
