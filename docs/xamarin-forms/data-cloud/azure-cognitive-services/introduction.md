---
title: Xamarin. Forms と Azure Cognitive Services の概要
description: この記事では、Microsoft 認知サービス Api の一部を呼び出す方法を示すサンプルアプリケーションの概要について説明します。
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 52774b387644b14e3d4612dffa6d3c3b28a37f25
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68652313"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Xamarin. Forms と Azure Cognitive Services の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services は、顔認識、音声認識、言語の理解などの機能を追加することで、開発者がアプリケーションをよりインテリジェントにするために使用できる Api、Sdk、およびサービスのセットです。この記事では、Microsoft 認知サービス Api の一部を呼び出す方法を示すサンプルアプリケーションの概要について説明します。_

## <a name="overview"></a>概要

付属のサンプルは、次の機能を提供する todo リストアプリケーションです。

- タスクの一覧を表示します。
- ソフトキーボードを使用するか、Microsoft Speech API で音声認識を実行して、タスクを追加および編集します。 音声認識を実行する方法の詳細については、「 [Microsoft Speech API を使用した音声認識](speech-recognition.md)」を参照してください。
- Bing Spell Check API を使用して、スペルチェックタスクを行います。 詳細については、「 [Bing Spell Check API を使用したスペルチェック](spell-check.md)」を参照してください。
- Translator API を使用して、タスクを英語からドイツ語に変換します。 詳細については、「 [TRANSLATOR API を使用したテキスト変換](text-translation.md)」を参照してください。
- タスクを削除します。
- タスクの状態を [完了] に設定します。
- Face API を使用して、アプリケーションを感情認識で評価します。 詳細については、「 [Face API を使用した感情認識](emotion-recognition.md)」を参照してください。

タスクは、ローカルの SQLite データベースに格納されます。 ローカルの SQLite データベースの使用方法の詳細については、「[ローカルデータベースの操作](~/xamarin-forms/data-cloud/data/databases.md)」を参照してください。

アプリケーションの起動時に `TodoListPage` が表示されます。 このページには、ローカルデータベースに格納されているタスクの一覧が表示され、ユーザーは新しいタスクを作成したり、アプリケーションを評価したりすることができます。

![](introduction-images/sample-application-1.png "TodoListPage")

新しい項目を作成するには、[ *+* ] ボタンをクリックします。このボタンをクリックすると、`TodoItemPage` に移動します。 このページには、タスクを選択して移動することもできます。

![](introduction-images/sample-application-2.png "TodoItemPage")

@No__t_0 を使用すると、タスクの作成、編集、スペルチェック、翻訳、保存、および削除を行うことができます。 音声認識は、タスクを作成または編集するために使用できます。 これを実現するには、マイクボタンを押して録音を開始し、もう一度同じボタンを押して記録を停止します。これにより、記録が Bing Speech 認識 API に送信されます。

@No__t_0 の [smilies] ボタンをクリックすると、`RateAppPage` に移動します。これは、顔式の画像で感情認識を実行するために使用されます。

![](introduction-images/sample-application-3.png "RateAppPage")

@No__t_0 を使用すると、ユーザーは顔の写真を撮ることができます。これは、返された感情が表示された Face API に送信されます。

## <a name="understand-the-application-anatomy"></a>アプリケーションの構造を理解する

サンプルアプリケーションの共有コードプロジェクトは、次の5つの主要なフォルダーで構成されています。

|フォルダー|目的|
|--- |--- |
|モデル|アプリケーションのデータモデルクラスが含まれています。 これには、アプリケーションによって使用される1つのデータ項目をモデル化する `TodoItem` クラスが含まれます。 このフォルダーには、さまざまな Microsoft 認知サービス Api から返された JSON 応答のモデル化に使用されるクラスも含まれています。|
|保管|データベース操作を実行するために使用される `ITodoItemRepository` インターフェイスおよび `TodoItemRepository` クラスが含まれています。|
|Services|さまざまな Microsoft 認知サービス Api にアクセスするために使用されるインターフェイスとクラス、およびプラットフォームプロジェクトでインターフェイスを実装するクラスを検索するために `DependencyService` クラスによって使用されるインターフェイスが含まれています。|
|utils|@No__t_0 クラスが含まれています。このクラスは、9分ごとに JWT アクセストークンを更新するために `AuthenticationService` クラスによって使用されます。|
|Views|アプリケーションのページが含まれています。|

共有コードプロジェクトには、いくつかの重要なファイルも含まれています。

|ファイル|目的|
|--- |--- |
|Constants.cs|@No__t_0 クラス。呼び出される Microsoft 認知サービス Api の API キーとエンドポイントを指定します。 API キー定数は、さまざまな認知サービス Api にアクセスするために更新する必要があります。|
|App.xaml.cs|@No__t_0 クラスは、各プラットフォームでアプリケーションによって表示される最初のページと、データベース操作を呼び出すために使用される `TodoManager` クラスの両方をインスタンス化します。|

### <a name="nuget-packages"></a>NuGet パッケージ

サンプルアプリケーションでは、次の NuGet パッケージを使用します。

- `Newtonsoft.Json` – .NET 用の JSON フレームワークを提供します。
- `PCLStorage` –クロスプラットフォームのローカルファイル IO Api のセットを提供します。
- `sqlite-net-pcl` – SQLite データベースストレージを提供します。
- `Xam.Plugin.Media` –クロスプラットフォームの写真を取得および選択する Api を提供します。

また、これらの NuGet パッケージでは、独自の依存関係もインストールされます。

### <a name="model-the-data"></a>データのモデル化

このサンプルアプリケーションでは、`TodoItem` クラスを使用して、ローカルの SQLite データベースに表示および格納されるデータをモデル化します。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

@No__t_0 プロパティは、各 `TodoItem` インスタンスを一意に識別するために使用され、SQLite 属性で修飾されます。この属性によって、データベースの主キーが自動インクリメントされます。

### <a name="invoke-database-operations"></a>データベース操作の呼び出し

@No__t_0 クラスはデータベース操作を実装します。クラスのインスタンスには、`App.TodoManager` プロパティを使用してアクセスできます。 @No__t_0 クラスには、データベース操作を呼び出すための次のメソッドが用意されています。

- **GetAllItemsAsync** –ローカルの SQLite データベースからすべての項目を取得します。
- **GetItemAsync** –指定された項目をローカルの SQLite データベースから取得します。
- **Saveitemasync** –ローカルの SQLite データベースの項目を作成または更新します。
- **Deleteitemasync** –指定された項目をローカルの SQLite データベースから削除します。

### <a name="platform-project-implementations"></a>プラットフォームプロジェクトの実装

共有コードプロジェクトの `Services` フォルダーには、プラットフォームプロジェクトでインターフェイスを実装するクラスを検索するために `DependencyService` クラスによって使用される `IFileHelper` および `IAudioRecorderService` インターフェイスが含まれています。

@No__t_0 インターフェイスは、各プラットフォームプロジェクトの `FileHelper` クラスによって実装されます。 このクラスは、`GetLocalFilePath`、SQLite データベースを格納するためのローカルファイルパスを返す1つのメソッドで構成されます。

@No__t_0 インターフェイスは、各プラットフォームプロジェクトの `AudioRecorderService` クラスによって実装されます。 このクラスは、`StartRecording`、`StopRecording`、およびサポートメソッドで構成されています。このメソッドは、プラットフォーム Api を使用してデバイスのマイクからオーディオを録音し、wav ファイルとして格納します。 IOS では、`AudioRecorderService` は `AVFoundation` API を使用してオーディオを記録します。 Android では、`AudioRecordService` は `AudioRecord` API を使用してオーディオを記録します。 ユニバーサル Windows プラットフォーム (UWP) では、`AudioRecorderService` は `AudioGraph` API を使用してオーディオを記録します。

### <a name="invoke-cognitive-services"></a>認識サービスの呼び出し

サンプルアプリケーションは、次の Microsoft Cognitive Services を呼び出します。

- Microsoft Speech API。 詳細については、「 [Microsoft Speech API を使用した音声認識](speech-recognition.md)」を参照してください。
- Bing Spell Check API。 詳細については、「 [Bing Spell Check API を使用したスペルチェック](spell-check.md)」を参照してください。
- 変換 API。 詳細については、「 [TRANSLATOR API を使用したテキスト変換](text-translation.md)」を参照してください。
- Face API。 詳細については、「 [Face API を使用した感情認識](emotion-recognition.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Microsoft Cognitive Services のドキュメント](https://www.microsoft.com/cognitive-services/documentation)
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
