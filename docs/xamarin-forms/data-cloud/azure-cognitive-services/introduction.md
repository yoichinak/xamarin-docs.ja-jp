---
title: Xamarin.Formsおよび Azure Cognitive Services の概要
description: この記事では、Microsoft 認知サービス Api の一部を呼び出す方法を示すサンプルアプリケーションの概要について説明します。
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cce5b0fc9c3d1d04c20b1be242197e3bc9e4f901
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929338"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Xamarin.Formsおよび Azure Cognitive Services の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services は、顔認識、音声認識、言語の理解などの機能を追加することで、開発者がアプリケーションをよりインテリジェントにするために使用できる Api、Sdk、およびサービスのセットです。この記事では、Microsoft 認知サービス Api の一部を呼び出す方法を示すサンプルアプリケーションの概要について説明します。_

## <a name="overview"></a>概要

付属のサンプルは、次の機能を提供する todo リストアプリケーションです。

- タスクの一覧を表示します。
- ソフトキーボードを使用するか、Microsoft Speech API で音声認識を実行して、タスクを追加および編集します。
- Bing Spell Check API を使用して、スペルチェックタスクを行います。 詳細については、「 [Bing Spell Check API を使用したスペルチェック](spell-check.md)」を参照してください。
- Translator API を使用して、タスクを英語からドイツ語に変換します。 詳細については、「 [TRANSLATOR API を使用したテキスト変換](text-translation.md)」を参照してください。
- タスクを削除します。
- タスクの状態を [完了] に設定します。
- Face API を使用して、アプリケーションを感情認識で評価します。 詳細については、「 [Face API を使用した感情認識](emotion-recognition.md)」を参照してください。

> [!WARNING]
> Bing Speech API は、Azure Speech サービスを優先するために非推奨とされました。 Azure Speech Service 専用のサンプルについては、「speech[サービス API による音声認識](~/xamarin-forms/data-cloud/azure-cognitive-services/speech-recognition.md)」を参照してください。

タスクは、ローカルの SQLite データベースに格納されます。 ローカルの SQLite データベースの使用方法の詳細については、「[ローカルデータベースの操作](~/xamarin-forms/data-cloud/data/databases.md)」を参照してください。

は、 `TodoListPage` アプリケーションの起動時に表示されます。 このページには、ローカルデータベースに格納されているタスクの一覧が表示され、ユーザーは新しいタスクを作成したり、アプリケーションを評価したりすることができます。

![TodoListPage](introduction-images/sample-application-1.png)

新しい項目を作成するには、[] ボタンをクリックします。このボタンをクリックすると *+* 、に移動し `TodoItemPage` ます。 このページには、タスクを選択して移動することもできます。

![TodoItemPage](introduction-images/sample-application-2.png)

では、 `TodoItemPage` タスクの作成、編集、スペルチェック、翻訳、保存、および削除を行うことができます。 音声認識は、タスクを作成または編集するために使用できます。 これを実現するには、マイクボタンを押して録音を開始し、もう一度同じボタンを押して記録を停止します。これにより、記録が Bing Speech 認識 API に送信されます。

の [smilies] ボタンをクリックすると、 `TodoListPage` `RateAppPage` 顔式のイメージに対して感情認識を実行するために使用されるに移動します。

![RateAppPage](introduction-images/sample-application-3.png)

を `RateAppPage` 使用すると、ユーザーは顔の写真を撮ることができます。これは、返された感情が表示された状態で Face API に送信されます。

## <a name="understand-the-application-anatomy"></a>アプリケーションの構造を理解する

サンプルアプリケーションの共有コードプロジェクトは、次の5つの主要なフォルダーで構成されています。

|Folder|目的|
|--- |--- |
|モデル|アプリケーションのデータモデルクラスが含まれています。 これには、 `TodoItem` アプリケーションによって使用される1つのデータ項目をモデル化するクラスが含まれます。 このフォルダーには、さまざまな Microsoft 認知サービス Api から返された JSON 応答のモデル化に使用されるクラスも含まれています。|
|リポジトリ|`ITodoItemRepository` `TodoItemRepository` データベース操作を実行するために使用されるインターフェイスとクラスが含まれています。|
|サービス|さまざまな Microsoft 認知サービス Api にアクセスするために使用されるインターフェイスとクラス、およびプラットフォームプロジェクトでインターフェイスを実装するクラスを検索するためにクラスによって使用されるインターフェイスが含まれてい `DependencyService` ます。|
|Utils|クラスを含み `Timer` ます。このクラスは、 `AuthenticationService` 9 分ごとに JWT アクセストークンを更新するためにクラスによって使用されます。|
|Views|アプリケーションのページが含まれています。|

共有コードプロジェクトには、いくつかの重要なファイルも含まれています。

|ファイル|目的|
|--- |--- |
|Constants.cs|`Constants`クラス。呼び出される Microsoft 認知サービス api の api キーとエンドポイントを指定します。 API キー定数は、さまざまな認知サービス Api にアクセスするために更新する必要があります。|
|App.xaml.cs|クラスは、 `App` 各プラットフォームでアプリケーションによって表示される最初のページと、 `TodoManager` データベース操作を呼び出すために使用されるクラスの両方をインスタンス化します。|

### <a name="nuget-packages"></a>NuGet パッケージ

サンプルアプリケーションでは、次の NuGet パッケージを使用します。

- `Newtonsoft.Json`– .NET 用の JSON フレームワークを提供します。
- `PCLStorage`–クロスプラットフォームのローカルファイル IO Api のセットを提供します。
- `sqlite-net-pcl`– SQLite データベースストレージを提供します。
- `Xam.Plugin.Media`–クロスプラットフォームの写真を取得および選択する Api を提供します。

また、これらの NuGet パッケージでは、独自の依存関係もインストールされます。

### <a name="model-the-data"></a>データをモデリングする

このサンプルアプリケーションでは、クラスを使用して、ローカルの SQLite データベースに表示され、格納されて `TodoItem` いるデータをモデル化します。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

プロパティは、 `ID` 各インスタンスを一意に識別するために使用され、 `TodoItem` SQLite 属性で修飾されます。この属性により、データベースの主キーが自動インクリメントされます。

### <a name="invoke-database-operations"></a>データベース操作の呼び出し

`TodoItemRepository`クラスはデータベース操作を実装します。クラスのインスタンスには、プロパティを使用してアクセスでき `App.TodoManager` ます。 クラスには、 `TodoItemRepository` データベース操作を呼び出すための次のメソッドが用意されています。

- **GetAllItemsAsync** –ローカルの SQLite データベースからすべての項目を取得します。
- **GetItemAsync** –指定された項目をローカルの SQLite データベースから取得します。
- **Saveitemasync** –ローカルの SQLite データベースの項目を作成または更新します。
- **Deleteitemasync** –指定された項目をローカルの SQLite データベースから削除します。

### <a name="platform-project-implementations"></a>プラットフォームプロジェクトの実装

`Services`共有コードプロジェクトのフォルダーには、 `IFileHelper` `IAudioRecorderService` `DependencyService` プラットフォームプロジェクトでインターフェイスを実装するクラスを検索するためにクラスによって使用されるインターフェイスとインターフェイスが含まれています。

この `IFileHelper` インターフェイスは、 `FileHelper` 各プラットフォームプロジェクトのクラスによって実装されます。 このクラスは、 `GetLocalFilePath` SQLite データベースを格納するためのローカルファイルパスを返す1つのメソッドで構成されます。

この `IAudioRecorderService` インターフェイスは、 `AudioRecorderService` 各プラットフォームプロジェクトのクラスによって実装されます。 このクラスは `StartRecording` 、、 `StopRecording` 、およびサポートメソッドで構成されています。このメソッドは、プラットフォーム api を使用してデバイスのマイクからオーディオを録音し、wav ファイルとして格納します。 IOS では、は API を使用して `AudioRecorderService` `AVFoundation` オーディオを記録します。 Android では、は API を使用して `AudioRecordService` `AudioRecord` オーディオを記録します。 ユニバーサル Windows プラットフォーム (UWP) では、は `AudioRecorderService` API を使用して `AudioGraph` オーディオを記録します。

### <a name="invoke-cognitive-services"></a>認識サービスの呼び出し

サンプルアプリケーションは、次の Microsoft Cognitive Services を呼び出します。

- Microsoft Speech API。 詳細については、「 [Microsoft Speech API を使用した音声認識](speech-recognition.md)」を参照してください。
- Bing Spell Check API。 詳細については、「 [Bing Spell Check API を使用したスペルチェック](spell-check.md)」を参照してください。
- 変換 API。 詳細については、「 [TRANSLATOR API を使用したテキスト変換](text-translation.md)」を参照してください。
- Face API。 詳細については、「 [Face API を使用した感情認識](emotion-recognition.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Speech サービス API を使用した音声認識](~/xamarin-forms/data-cloud/azure-cognitive-services/speech-recognition.md)
- [Microsoft Cognitive Services のドキュメント](https://www.microsoft.com/cognitive-services/documentation)
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
