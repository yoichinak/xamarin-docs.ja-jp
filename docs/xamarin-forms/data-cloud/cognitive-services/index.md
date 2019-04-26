---
title: Cognitive Services とインテリジェンスの追加
description: この記事では、いくつかの Microsoft Cognitive Service Api を呼び出す方法を示すサンプル アプリケーションを紹介します。
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 0c09063b55a14f9f22feb91d2a6f9d3f9417ecee
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61330898"
---
# <a name="adding-intelligence-with-cognitive-services"></a>Cognitive Services とインテリジェンスの追加

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Microsoft Cognitive Services では、Api、Sdk、および開発者は、顔認識、音声認識、および言語の理解などの機能を追加することで、アプリケーションをよりインテリジェントに利用可能なサービスのセットです。この記事では、いくつかの Microsoft Cognitive Service Api を呼び出す方法を示すサンプル アプリケーションを紹介します。_

## <a name="overview"></a>概要

付属のサンプルでは、機能を提供する todo リスト アプリケーションを示します。

- タスクの一覧を表示します。
- 追加し、ソフト キーボード、または Microsoft Speech API を使用した音声認識を実行することによって、タスクを編集します。 音声認識の実行の詳細については、次を参照してください。 [Microsoft Speech API を使用して、音声認識](speech-recognition.md)します。
- スペルをチェック タスクの Bing Spell Check API を使用します。 詳細については、次を参照してください。[スペル チェック、Bing Spell Check API を使用して](spell-check.md)します。
- Translator API を使用してドイツ語、英語からタスクを変換します。 詳細については、次を参照してください。 [Translator API を使用してテキストの翻訳](text-translation.md)します。
- タスクを削除します。
- タスクの状態 'done' に設定します。
- Face API を使用して、感情認識を使用してアプリケーションを評価します。 詳細については、次を参照してください。 [Face API を使用して、感情認識](emotion-recognition.md)します。

タスクは、ローカルの SQLite データベースに格納されます。 詳細については、ローカルの SQLite データベースを使用して、次を参照してください。[ローカル Database](~/xamarin-forms/app-fundamentals/databases.md)。

`TodoListPage`が、アプリケーションを起動するときに表示されます。 このページは、ローカルのデータベースに格納されているすべてのタスクの一覧を表示し、により、新しいタスクを作成したり、アプリケーションの評価。

![](images/sample-application-1.png "TodoListPage")

クリックして新しい項目を作成することができます、 *+* ボタンに移動するため、`TodoItemPage`します。 このページはタスクを選択してに移動することもできます。

![](images/sample-application-2.png "TodoItemPage")

`TodoItemPage`タスクを作成、編集、スペル チェックを翻訳、保存、および削除できます。 作成または編集するタスクは、音声認識を使用できます。 これは、ボタンを押して、同じをもう一度、記録を停止して、記録を開始するには、あるマイク ボタンを押して、Bing Speech Recognition API に、記録を送信します。

顔文字 ボタンをクリックすると、`TodoListPage`に移動、`RateAppPage`表情のイメージの感情認識の実行に使用します。

![](images/sample-application-3.png "RateAppPage")

`RateAppPage`の面では、表示されている、返された emotion で Face API に送信されるの写真を撮影できます。

## <a name="understanding-the-application-anatomy"></a>アプリケーションの構造を理解します。

サンプル アプリケーションについては、ポータブル クラス ライブラリ (PCL) プロジェクトは、5 つのメイン フォルダーで構成されます。

|フォルダー|目的|
|--- |--- |
|モデル|アプリケーションのデータ モデル クラスが含まれています。 これが含まれています、`TodoItem`クラスは、アプリケーションによって使用されるデータの 1 つの項目をモデル化します。 フォルダーには、別の Microsoft Cognitive Service Api から返される JSON 応答をモデルに使用されるクラスも含まれています。|
|リポジトリ|含まれています、`ITodoItemRepository`インターフェイスと`TodoItemRepository`データベース操作の実行に使用されるクラス。|
|Services|インターフェイスとさまざまな Microsoft Cognitive Service Api で使用されるインターフェイスと共にへのアクセスに使用されるクラスが含まれています、`DependencyService`プラットフォーム プロジェクトにインターフェイスを実装するクラスを検索するクラス。|
|Utils|含まれています、`Timer`クラスで使用される、 `AuthenticationService` 9 分ごとに、JWT アクセス トークンを更新するクラス。|
|Views|アプリケーションのページが含まれています。|

PCL プロジェクトには、いくつかの重要なファイルも含まれています。

|ファイル|目的|
|--- |--- |
|Constants.cs|`Constants`クラスは、呼び出される Microsoft Cognitive Service Api の API キーとエンドポイントを指定します。 API キーの定数は、さまざまな Cognitive Service Api にアクセスする更新が必要です。|
|App.xaml.cs|`App`クラスは、各プラットフォームでアプリケーションによって表示される両方の最初のページをインスタンス化を担当し、`TodoManager`データベース操作の呼び出しに使用されるクラスです。|

### <a name="nuget-packages"></a>NuGet パッケージ

サンプル アプリケーションでは、次の NuGet パッケージを使用します。

- `Newtonsoft.Json` – .NET 用の JSON フレームワークを提供します。
- `PCLStorage` -クロス プラットフォームのローカル ファイル IO Api のセットを提供します。
- `sqlite-net-pcl` -SQLite データベース ストレージを提供します。
- `Xam.Plugin.Media` -クロス プラットフォームの写真の取得と Api の選択を提供します。

さらに、これらの NuGet パッケージは、独自の依存関係もインストールします。

### <a name="modeling-the-data"></a>データのモデリング

サンプル アプリケーションを使用して、`TodoItem`を表示し、ローカルの SQLite データベースに格納されるデータをモデル化するクラス。 次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID`プロパティは、それぞれを一意に識別するために使用`TodoItem`インスタンスし、は、プロパティを自動インクリメントの主キー、データベースに SQLite 属性で修飾されます。

### <a name="invoking-database-operations"></a>データベース操作を呼び出す

`TodoItemRepository`クラスは、データベース操作を実装し、クラスのインスタンスを介してアクセスできる、`App.TodoManager`プロパティ。 `TodoItemRepository`クラスは、データベース操作を呼び出す次のメソッドを提供します。

- **GetAllItemsAsync** – ローカルの SQLite データベースからすべての項目を取得します。
- **GetItemAsync** – ローカルの SQLite データベースから指定した項目を取得します。
- **SaveItemAsync** – 作成するか、ローカルの SQLite データベース内の項目を更新します。
- **DeleteItemAsync** – ローカルの SQLite データベースから指定した項目を削除します。

### <a name="platform-project-implementations"></a>プラットフォーム プロジェクトの実装

`Services` PCL プロジェクトのフォルダーが含まれています、`IFileHelper`と`IAudioRecorderService`インターフェイスで使用される、`DependencyService`プラットフォーム プロジェクトにインターフェイスを実装するクラスを検索するクラス。

`IFileHelper`インターフェイスによって実装されます、`FileHelper`各プラットフォーム プロジェクトにクラス。 このクラスは、1 つのメソッドの`GetLocalFilePath`、SQLite データベースを格納するためのローカル ファイル パスが返されます。

`IAudioRecorderService`インターフェイスによって実装されます、`AudioRecorderService`各プラットフォーム プロジェクトにクラス。 このクラスから成る`StartRecording`、 `StopRecording`、プラットフォーム Api を使用して、デバイスのマイクからオーディオを録音し、wav ファイルとして保存するには、メソッドをサポートしているとします。 Ios では、`AudioRecorderService`を使用して、`AVFoundation`オーディオを録音する API。 Android の場合、`AudioRecordService`を使用して、`AudioRecord`オーディオを録音する API。 ユニバーサル Windows プラットフォーム (UWP) で、`AudioRecorderService`を使用して、`AudioGraph`オーディオを録音する API。

### <a name="invoking-cognitive-services"></a>Cognitive Services を呼び出す

サンプル アプリケーションは、次の Microsoft Cognitive Services を呼び出します。

- Microsoft Speech API。 詳細については、次を参照してください。 [Microsoft Speech API を使用して、音声認識](speech-recognition.md)します。
- Bing Spell Check API。 詳細については、次を参照してください。[スペル チェック、Bing Spell Check API を使用して](spell-check.md)します。
- API を変換します。 詳細については、次を参照してください。 [Translator API を使用してテキストの翻訳](text-translation.md)します。
- Face API。 詳細については、次を参照してください。 [Face API を使用して、感情認識](emotion-recognition.md)します。

## <a name="related-links"></a>関連リンク

- [Microsoft Cognitive Services のドキュメント](https://www.microsoft.com/cognitive-services/documentation)
- [Todo Cognitive Services (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
