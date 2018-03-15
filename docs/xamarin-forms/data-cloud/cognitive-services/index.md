---
title: "認識サービスとインテリジェンスを追加します。"
description: "マイクロソフトの知的サービスでは、Api、Sdk、および開発者は、顔認識、音声認識、および言語理解などの機能を追加することで、アプリケーションをより高度な利用できるサービスのセットです。 この記事では、Microsoft 認知サービス Api の一部を呼び出す方法を説明するサンプル アプリケーションに紹介します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: c309fb6936296dc181e499c91770ab8891121e9c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>認識サービスとインテリジェンスを追加します。

_マイクロソフトの知的サービスでは、Api、Sdk、および開発者は、顔認識、音声認識、および言語理解などの機能を追加することで、アプリケーションをより高度な利用できるサービスのセットです。この記事では、Microsoft 認知サービス Api の一部を呼び出す方法を説明するサンプル アプリケーションに紹介します。_

## <a name="overview"></a>概要

付属するサンプルは、機能を提供する todo リスト アプリケーションです。

- タスクの一覧を表示します。
- 追加し、ソフト キーボード、または Bing Speech API で音声認識を実行することによってタスクを編集します。 音声認識を実行する方法の詳細については、次を参照してください。 [Bing Speech API を使用する音声認識](speech-recognition.md)です。
- スペルをチェック タスクの Bing スペル チェック API を使用します。 詳細については、次を参照してください。[スペル チェック Bing スペル チェック API を使用して](spell-check.md)です。
- 変換 API を使用してドイツ語に英語からタスクを変換します。 詳細については、次を参照してください。[トランスレーター API を使用してテキストの翻訳](text-translation.md)です。
- タスクを削除します。
- 'Done' に、タスクの状態を設定します。
- Emotion API を使用して、emotion 認識を使用してアプリケーションを評価します。 詳細については、次を参照してください。 [Emotion API を使用して Emotion 認識](emotion-recognition.md)です。

タスクは、ローカル SQLite データベースに格納されます。 詳細については、SQLite ローカル データベースを使用して、次を参照してください。[ローカル データベースで作業](~/xamarin-forms/app-fundamentals/databases.md)です。

`TodoListPage`が、アプリケーションを起動するときに表示されます。 このページは、ローカル データベースに格納されているすべてのタスクの一覧を表示し、ユーザーまたはアプリケーションを評価する新しいタスクを作成することができます。

![](images/sample-application-1.png "TodoListPage")

をクリックして新しい項目を作成することができます、  *+* に移動するボタン、`TodoItemPage`です。 このページは、タスクを選択してに移動することもできます。

![](images/sample-application-2.png "TodoItemPage")

`TodoItemPage`変換、保存すると、および削除にタスクを作成、編集、スペル チェックを実行できます。 作成または編集するタスクは、音声認識を使用できます。 これは、ボタンを押して、マイクを録音の開始とボタンを押して、同じ 2 つ目の時間、記録を停止する Bing Speech 認識 API に、記録を送信します。

の顔文字 ボタンをクリックして、`TodoListPage`に移動、 `RateAppPage`、顔の式のイメージに対して emotion 認識を実行に使用されます。

![](images/sample-application-3.png "RateAppPage")

`RateAppPage`写真の面では、表示されている、返された感情を Emotion API に送信されるを実行することができます。

## <a name="understanding-the-application-anatomy"></a>アプリケーション構造を理解します。

5 つのメイン フォルダーのサンプル アプリケーションについては、ポータブル クラス ライブラリ (PCL) プロジェクトで構成されます。

|フォルダー|目的|
|--- |--- |
|モデル|アプリケーションのデータ モデル クラスを含みます。 これが含まれています、`TodoItem`クラスは、アプリケーションによって使用されるデータの単一の項目をモデル化します。 フォルダーには、別の Microsoft 認知サービス Api から返されたモデルの JSON 応答に使用されるクラスも含まれています。|
|リポジトリ|含まれています、`ITodoItemRepository`インターフェイスと`TodoItemRepository`データベース操作を実行に使用されるクラスです。|
|サービス|Api にアクセスするさまざまな Microsoft 認知サービスで使用されるインターフェイスと一緒に使用されるクラスとインターフェイスが含まれています、`DependencyService`プラットフォーム プロジェクトでは、インターフェイスを実装するクラスを検索対象のクラスです。|
|ユーティリティ|含まれています、`Timer`によって使用されるクラス、 `AuthenticationService` 9 分ごと、JWT アクセス トークンを更新するクラス。|
|ビュー|アプリケーション ページが含まれます。|

PCL プロジェクトには、いくつかの重要なファイルも含まれています。

|ファイル|目的|
|--- |--- |
|Constants.cs|`Constants`クラスは、呼び出される Microsoft 認知サービス Api の API キーとエンドポイントを指定します。 API キー定数は、さまざまな認知サービス Api にアクセスする更新が必要です。|
|App.xaml.cs|`App`クラスは、各プラットフォームでのアプリケーションによって表示される両方の最初のページをインスタンス化して、`TodoManager`データベース操作の呼び出しに使用されるクラスです。|

### <a name="nuget-packages"></a>NuGet パッケージ

サンプル アプリケーションは、次の NuGet パッケージを使用します。

- `Microsoft.Net.Http` – 提供、 `HttpClient` HTTP 経由で要求を行うためのクラスです。
- `Newtonsoft.Json` – .NET の JSON フレームワークを提供します。
- `Microsoft.ProjectOxford.Emotion` – Emotion API にアクセスするためのクライアント ライブラリです。
- `PCLStorage` – クロスプラット フォームのローカル ファイル IO Api のセットを提供します。
- `sqlite-net-pcl` – SQLite データベース ストレージを提供します。
- `Xam.Plugin.Media` – クロスプラット フォームの写真の作成と Api の取得を提供します。

さらに、これらの NuGet パッケージは、独自の依存関係もインストールします。

### <a name="modeling-the-data"></a>データをモデリング

サンプル アプリケーションを使用して、`TodoItem`表示され、ローカル SQLite データベースに格納されるデータをモデル化するクラス。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID`をそれぞれを一意に識別するプロパティを使用`TodoItem`インスタンス、およびデータベースで自動インクリメントの主キーのプロパティを構成する SQLite 属性で装飾されています。

### <a name="invoking-database-operations"></a>呼び出し元のデータベース操作

`TodoItemRepository`クラスは、データベース操作を実装し、クラスのインスタンスからアクセスできます、`App.TodoManager`プロパティです。 `TodoItemRepository`クラスは、データベース操作の呼び出しに次のメソッドを提供します。

- **GetAllItemsAsync** – ローカル SQLite データベースからすべての項目を取得します。
- **GetItemAsync** – ローカル SQLite データベースから、指定した項目を取得します。
- **SaveItemAsync** – 作成するか、ローカルの SQLite データベース内の項目を更新します。
- **DeleteItemAsync** – 指定した項目をローカルの SQLite データベースから削除します。

### <a name="platform-project-implementations"></a>プラットフォームのプロジェクトの実装

`Services` PCL プロジェクト フォルダーに含まれています、`IFileHelper`と`IAudioRecorderService`によって使用されているインターフェイス、`DependencyService`プラットフォーム プロジェクトでは、インターフェイスを実装するクラスを検索対象のクラスです。

`IFileHelper`インターフェイスは、`FileHelper`各プラットフォームのプロジェクト内のクラスです。 このクラスは、1 つのメソッドの`GetLocalFilePath`、SQLite データベースを格納するためのローカル ファイル パスが返されます。

`IAudioRecorderService`インターフェイスは、`AudioRecorderService`各プラットフォームのプロジェクト内のクラスです。 このクラスから成ります`StartRecording`、 `StopRecording`、プラットフォーム Api を使用して、デバイスのマイクからオーディオを録音をおよび、wav ファイルとして保存する方法をサポートするとします。 Ios の場合、`AudioRecorderService`を使用して、`AVFoundation`オーディオを録音する API。 Android で、`AudioRecordService`を使用して、`AudioRecord`オーディオを録音する API。 ユニバーサル Windows プラットフォーム (UWP) に、`AudioRecorderService`を使用して、`AudioGraph`オーディオを録音する API。

### <a name="invoking-cognitive-services"></a>認知サービスを呼び出す

サンプル アプリケーションは、次の Microsoft 認知サービスを呼び出します。

- Bing Speech API. 詳細については、次を参照してください。 [Bing Speech API を使用する音声認識](speech-recognition.md)です。
- Bing のスペル チェック API です。 詳細については、次を参照してください。[スペル チェック Bing スペル チェック API を使用して](spell-check.md)です。
- API を変換します。 詳細については、次を参照してください。[トランスレーター API を使用してテキストの翻訳](text-translation.md)です。
- Emotion API です。 詳細については、次を参照してください。 [Emotion API を使用して Emotion 認識](emotion-recognition.md)です。


## <a name="related-links"></a>関連リンク

- [Microsoft 認知 Services のドキュメント](https://www.microsoft.com/cognitive-services/documentation)
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
