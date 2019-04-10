---
title: ASP.NET Web サービス (ASMX) の使用
description: この記事では、Xamarin.Forms アプリケーションから ASMX SOAP サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1fa2fee36619a2a443d84b861b2473a1f616ed0e
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055142"
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>ASP.NET Web サービス (ASMX) の使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)

_ASMX では、簡易オブジェクト アクセス プロトコル (SOAP) を使用してメッセージを送信する web サービスをビルドする機能を提供します。SOAP は、構築、および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。のみの SOAP メッセージを送受信する方法を理解する必要があります。この記事では、Xamarin.Forms アプリケーションから ASMX SOAP サービスを使用する方法を示します。_

SOAP メッセージは、次の要素を含む XML ドキュメントを示します。

- という名前のルート要素*エンベロープ*SOAP メッセージとして XML ドキュメントを識別します。
- 省略可能な*ヘッダー*認証データなどのアプリケーションに固有の情報を含む要素。 場合、*ヘッダー*要素が存在するは、最初の子要素があります、*エンベロープ*要素。
- 必要な*本文*SOAP メッセージの受信者のためのものを含む要素。
- 省略可能な*フォールト*をエラー メッセージを示すために使用される要素。 場合、*フォールト*要素が存在するの子要素があります、*本文*要素。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポート プロトコルで操作できます。 ただし、ASMX サービスは HTTP 経由でのみ操作できます。 Xamarin プラットフォームは、HTTP 経由で SOAP 1.1 の標準的な実装をサポートしていて、標準の ASMX サービスの構成の多くのサポートが含まれます。

ASMX サービスの設定方法については、サンプル アプリケーションに付属する readme ファイルで確認できます。 ただし、サンプル アプリケーションを実行すると、それに接続する ASMX の Xamarin でホストされるサービス、データへの読み取り専用アクセスを提供する次のスクリーン ショットに示すように。

![](asmx-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="consuming-the-web-service"></a>Web サービスの使用

ASMX サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータ モデルの詳細については、[、データのモデリング](~/xamarin-forms/data-cloud/walkthrough.md)を参照してください。

> [!NOTE]
> サンプル アプリケーションでは、web サービスへの読み取り専用アクセスを提供する、Xamarin でホストされる ASMX サービスを消費します。 そのため、作成、更新、およびデータを削除する操作では、アプリケーションで使用されるデータは変更されません。 ただし、ASMX サービスのホスト可能なバージョンが記載されて、 **TodoASMXService**付属のサンプル アプリケーション内のフォルダー。 完全な ASMX サービスの許可ホスト可能なこのバージョンで、作成、更新、読み取り、およびデータへのアクセスを削除します。

A*プロキシ*あり、アプリケーション、サービスに接続する ASMX サービスの使用を生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 プラットフォーム固有のプロジェクトに web サービスの web 参照を追加することで、プロキシが構築されます。

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 たとえば、`EndGetTodoItems`メソッドのコレクションを返します`TodoItem`インスタンス。 *EndOperationName*メソッドも含まれています、`IAsyncResult`パラメーターに対応する呼び出しによって返されるインスタンスに設定する必要がありますが、 *BeginOperationName*メソッド。

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`TaskFactory.FromAsync`メソッド。

APM の詳細については、[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn を参照してください。

### <a name="creating-the-todoservice-object"></a>TodoService オブジェクトを作成します。

生成されたプロキシ クラスには、`TodoService`クラスは、HTTP 経由で ASMX サービスと通信するために使用します。 識別されたサービス インスタンスの URI からの非同期操作は、web サービス メソッドを呼び出すために、機能を提供します。 非同期操作の詳細については、[非同期サポートの概要](~/cross-platform/platform/async.md)を参照してください。

`TodoService`次のコード例に示すように、ASMX サービスを使用するアプリケーションに必要な限りのオブジェクトが存在するために、クラス レベルでインスタンスが宣言されています。

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

`TodoService`コンス トラクターは、ASMX サービス インスタンスの URL を指定する省略可能な文字列パラメーターを受け取ります。 これにより、パブリッシュされた複数のインスタンスがあること、アプリケーション、ASMX サービスの異なるインスタンスに接続します。

### <a name="creating-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラス。 格納する、`TodoItem`最初生成されたプロキシに変換する必要がある、web サービス内の項目`TodoItem`型。 これは、`ToASMXServiceTodoItem`メソッドを次のコード例に示すようにします。

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

このメソッドは単に新しい作成`ASMService.TodoItem`インスタンス、および各プロパティから同じプロパティを設定して、`TodoItem`インスタンス。

同様に、web サービスからデータを取得すると、ときにする必要があります変換する必要が生成されたプロキシから`TodoItem`型、`TodoItem`インスタンス。 これを実行、`FromASMXServiceTodoItem`メソッドを次のコード例に示すようにします。

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

このメソッドは、生成されたプロキシからのデータを取得する単に`TodoItem`を入力し、新しく作成設定`TodoItem`インスタンス。

### <a name="retrieving-data"></a>データの取得

`TodoService.BeginGetTodoItems`と`TodoService.EndGetTodoItems`メソッド呼び出しを使用して、 `GetTodoItems` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndGetTodoItems`メソッドを 1 回、`TodoService.BeginGetTodoItems`メソッドが完了したらで、`null`パラメーターにデータが渡されていないことを示す、`BeginGetTodoItems`デリゲートします。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

`TodoService.EndGetTodoItems`メソッドの配列を返します`ASMXService.TodoItem`に変換し、インスタンス、`List`の`TodoItem`表示のインスタンス。

### <a name="creating-data"></a>データの作成

`TodoService.BeginCreateTodoItem`と`TodoService.EndCreateTodoItem`メソッド呼び出しを使用して、 `CreateTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndCreateTodoItem`メソッドを 1 回、`TodoService.BeginCreateTodoItem`メソッドが完了したらで、`todoItem`パラメーターに渡されるデータ、 `BeginCreateTodoItem` を指定するデリゲート`TodoItem` web サービスによって作成されます。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`SoapException`作成に失敗した場合、`TodoItem`をアプリケーションでこれを処理します。

### <a name="updating-data"></a>データの更新

`TodoService.BeginEditTodoItem`と`TodoService.EndEditTodoItem`メソッド呼び出しを使用して、 `EditTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndEditTodoItem`メソッドを 1 回、`TodoService.BeginCreateTodoItem`メソッドが完了したらで、`todoItem`パラメーターに渡されるデータ、 `BeginEditTodoItem` を指定するデリゲート`TodoItem` web サービスで更新します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`SoapException`を探すか、更新に失敗した場合、 `TodoItem`、アプリケーションでこれを処理します。

### <a name="deleting-data"></a>データの削除

`TodoService.BeginDeleteTodoItem`と`TodoService.EndDeleteTodoItem`メソッド呼び出しを使用して、 `DeleteTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndDeleteTodoItem`メソッドを 1 回、`TodoService.BeginDeleteTodoItem`メソッドが完了したらで、`id`パラメーターに渡されるデータ、 `BeginDeleteTodoItem` を指定するデリゲート`TodoItem` web サービスによって削除されます。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`SoapException`を探すか、削除に失敗した場合、`TodoItem`をアプリケーションでこれを処理します。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションから ASMX web サービスを使用する方法を示しました。 ASMX では、SOAP を使用して HTTP 経由でメッセージを送信する web サービスをビルドする機能を提供します。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。 のみの SOAP メッセージを送受信する方法を理解する必要があります。


## <a name="related-links"></a>関連リンク

- [TodoASMX (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
