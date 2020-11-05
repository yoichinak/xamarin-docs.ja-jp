---
title: Xamarin.Forms Web サービスの概要
description: このガイドでは、 Xamarin.Forms さまざまな web サービスと通信する方法を示すサンプルアプリケーションのチュートリアルを提供します。 各 web サービスは個別のサンプルアプリケーションを使用しますが、機能的に類似した共通クラスを共有します。
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7d1cb1d9fa418cd16cb25519680e526864c9917c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367726"
---
# <a name="no-locxamarinforms-web-services-introduction"></a>Xamarin.Forms Web サービスの概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_このトピックでは、 Xamarin.Forms さまざまな web サービスと通信する方法を示すサンプルアプリケーションのチュートリアルについて説明します。各 web サービスは個別のサンプルアプリケーションを使用しますが、機能的に類似した共通クラスを共有します。_

次に示すサンプルの to do list アプリケーションは、を使用してさまざまな種類の web サービスバックエンドにアクセスする方法を示すために使用され Xamarin.Forms ます。 次の機能を提供します。

- タスクの一覧を表示します。
- タスクを追加、編集、および削除します。
- タスクの状態を [完了] に設定します。
- タスクの [名前] フィールドと [メモ] フィールドを読み上げます。

どのような場合でも、タスクは web サービスを通じてアクセスされるバックエンドに格納されます。

アプリケーションが起動すると、web サービスから取得したすべてのタスクを一覧表示するページが表示され、ユーザーは新しいタスクを作成できます。 タスクをクリックすると、アプリケーションが2番目のページに移動します。このページでは、タスクを編集、保存、削除、および読み上げることができます。 最終的なアプリケーションは、次のとおりです。

![Todo アプリケーション-最初 ](introduction-images/app-example-1.png)
 ![ のページ todo アプリケーション-2 ページ](introduction-images/app-example-2.png)

このガイドの各トピックでは、特定の種類の web サービスバックエンドを示す、 *別* のバージョンのアプリケーションへのダウンロードリンクを提供します。 各 web サービススタイルに関連するページで、関連するサンプルコードをダウンロードします。

## <a name="understand-the-application-anatomy"></a>アプリケーションの構造を理解する

各サンプルアプリケーションの共有コードプロジェクトは、次の3つの主要なフォルダーで構成されています。

|Folder|目的|
|--- |--- |
|データ|データ項目を管理し、web サービスと通信するために使用されるクラスとインターフェイスが含まれています。 これには、少なくともクラスが含まれ `TodoItemManager` ます。これは、 `App` web サービス操作を呼び出すためにクラスのプロパティを介して公開されます。|
|モデル|アプリケーションのデータモデルクラスが含まれています。 少なくとも、 `TodoItem` アプリケーションによって使用される1つのデータ項目をモデル化するクラスが含まれています。 このフォルダーには、ユーザーデータのモデル化に使用する追加のクラスを含めることもできます。|
|ビュー|アプリケーションのページが含まれています。 これは通常、 `TodoListPage` クラスと `TodoItemPage` クラス、および認証のために使用される追加のクラスで構成されます。|

各アプリケーションの共有コードプロジェクトも、いくつかの重要なファイルで構成されています。

|ファイル|目的|
|--- |--- |
|Constants.cs|`Constants`クラス。アプリケーションが web サービスと通信するために使用する定数を指定します。 これらの定数は、プロバイダーで作成された個人用バックエンドサービスにアクセスするために更新する必要があります。|
|ITextToSpeech.cs|`ITextToSpeech`インターフェイス。実装するクラスがメソッドを提供する必要があることを指定し `Speak` ます。|
|Todo.cs|`App`各プラットフォームでアプリケーションによって表示される最初のページと、 `TodoItemManager` web サービス操作を呼び出すために使用されるクラスの両方をインスタンス化するクラス。|

### <a name="view-pages"></a>ページの表示

サンプルアプリケーションの大部分には、少なくとも2つのページが含まれています。

- **TodoListPage** –このページには、インスタンスの一覧と、プロパティがの場合はティックアイコンが表示され `TodoItem` `TodoItem.Done` `true` ます。 項目をクリックすると、に移動 `TodoItemPage` します。 また、シンボルをクリックして新しい項目を作成することもでき *+* ます。
- **TodoItemPage** –このページには、選択したの詳細が表示され、 `TodoItem` 編集、保存、削除、および読み上げを行うことができます。

また、一部のサンプルアプリケーションには、ユーザー認証プロセスを管理するために使用される追加のページが含まれています。

### <a name="model-the-data"></a>データをモデリングする

各サンプルアプリケーションでは、クラスを使用して、 `TodoItem` ストレージ用に web サービスに送信されるデータをモデル化します。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

プロパティは、 `ID` 各インスタンスを一意に識別するために使用され、 `TodoItem` 更新または削除するデータを識別するために各 web サービスによって使用されます。

### <a name="invoke-web-service-operations"></a>Web サービス操作の呼び出し

Web サービスの操作には、 `TodoItemManager` クラスを介してアクセスします。クラスのインスタンスには、プロパティを使用してアクセスでき `App.TodoManager` ます。 クラスには、 `TodoItemManager` web サービス操作を呼び出すための次のメソッドが用意されています。

- **Gettasksasync** –このメソッドは `ListView` 、 `TodoListPage` `TodoItem` web サービスから取得したインスタンスを使用してのコントロールを設定するために使用されます。
- **Savetaskasync** –このメソッドは、web サービスのインスタンスを作成または更新するために使用され `TodoItem` ます。
- **Deletetaskasync** –このメソッドは、web サービスのインスタンスを削除するために使用され `TodoItem` ます。

また、一部のサンプルアプリケーションには、 `TodoItemManager` ユーザー認証プロセスを管理するために使用されるクラスの追加のメソッドが含まれています。

メソッドは、web サービス操作を直接呼び出すのではなく、 `TodoItemManager` コンストラクターに挿入される依存クラスのメソッドを呼び出し `TodoItemManager` ます。 たとえば、1つのサンプルアプリケーションは、クラスをコンストラクターに挿入して、 `RestService` `TodoItemManager` REST api を使用してデータにアクセスする実装を提供します。

## <a name="related-links"></a>関連リンク

- [ASMX (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [WCF (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [REST (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-todorest)