---
title: ASP.NET Web サービス (ASMX) の使用
description: Asmx サービスは、簡易オブジェクト アクセス プロトコル (SOAP) を使用してメッセージを送信する web サービスをビルドする機能を提供します。 SOAP は、構築および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスの実装に使用するプログラミング言語に関する知識は必要はありません。 のみ、SOAP メッセージを送受信する方法を理解する必要があります。 この記事では、Xamarin.Forms アプリケーションから、ASMX SOAP サービスを利用する方法を示します。
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: c45f0de039abc3f98b7c269f183e2883a495910b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>ASP.NET Web サービス (ASMX) の使用

_Asmx サービスは、簡易オブジェクト アクセス プロトコル (SOAP) を使用してメッセージを送信する web サービスをビルドする機能を提供します。SOAP は、構築および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスの実装に使用するプログラミング言語に関する知識は必要はありません。のみ、SOAP メッセージを送受信する方法を理解する必要があります。この記事では、Xamarin.Forms アプリケーションから、ASMX SOAP サービスを利用する方法を示します。_

SOAP メッセージは、次の要素を含む XML ドキュメントを示します。

- という名前のルート要素*エンベロープ*SOAP メッセージとして XML ドキュメントを識別します。
- 省略可能な*ヘッダー*認証データなどのアプリケーションに固有の情報を格納する要素。 場合、*ヘッダー*要素が存在の最初の子要素があります、*エンベロープ*要素。
- 必要な*本文*受信者のためのもので、SOAP メッセージを格納する要素。
- 省略可能な*フォールト*エラー メッセージを示すために使用される要素。 場合、*フォールト*要素が存在する必要がありますの子要素、*本文*要素。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポート プロトコルで動作できます。 ただし、ASMX サービスは HTTP 経由で操作のみが可能です。 Xamarin プラットフォームが over HTTP を標準 SOAP 1.1 の実装をサポートし、これには、標準の ASMX サービス構成の多くのサポートが含まれます。

ASMX サービス設定手順については、サンプル アプリケーションに付属している readme ファイルで確認できます。 ただし、実行時に、サンプル アプリケーションは、接続、データへの読み取り専用のアクセスを提供する ASMX を Xamarin でホストされるサービスに次のスクリーン ショットに示すように。

![](asmx-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="consuming-the-web-service"></a>Web サービスの使用

ASMX サービスでは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|作業アイテムの一覧を取得します。|
|CreateTodoItem|新しい作業項目を作成します。|TodoItem が XML にシリアル化されます。|
|EditTodoItem|作業項目を更新します。|TodoItem が XML にシリアル化されます。|
|DeleteTodoItem|作業項目を削除します。|TodoItem が XML にシリアル化されます。|

アプリケーションで使用されるデータ モデルの詳細については、次を参照してください。[データ モデリング](~/xamarin-forms/data-cloud/walkthrough.md)です。

> [!NOTE]
> サンプル アプリケーションは、web サービスへの読み取り専用のアクセスを提供する Xamarin でホストされる ASMX サービスを使用します。 そのため、作成、更新、およびデータを削除する操作では、アプリケーションで使用するデータは変更されません。 ただし、ASMX サービスのホスト可能なバージョンは利用で、 **TodoASMXService**付随するサンプル アプリケーション内のフォルダーです。 このホスト可能なバージョン完全 ASMX サービス許可の作成、更新、読み取り、およびデータへのアクセスを削除します。

A*プロキシ*により、アプリケーションは、サービスに接続する、ASMX サービスを使用するを生成する必要があります。 メソッドと関連付けられているサービス構成を定義するサービス メタデータを使用して、プロキシが構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 プロキシは、プラットフォーム固有のプロジェクトに web サービスの web 参照を追加することによって作成されています。

生成されたプロキシ クラスでは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*、開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始しを実装するオブジェクトを返します、`IAsyncResult`インターフェイスです。 呼び出した後*BeginOperationName*、スレッド プールのスレッドで非同期操作の実行中のスレッドの呼び出しに関する命令を実行するアプリケーションを続行できます。

呼び出しごとに*BeginOperationName*、アプリケーションを呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*同期 web サービス メソッドによって返される同じ型です。 たとえば、`EndGetTodoItems`メソッドのコレクションを返します`TodoItem`インスタンス。 *EndOperationName*メソッドも含まれています、`IAsyncResult`パラメーターに対応する呼び出しによって返されるインスタンスに設定する必要がありますが、 *BeginOperationName*メソッドです。

タスク並列ライブラリ (TPL) は同じで、非同期操作をカプセル化して APM 開始/終了メソッドのペアを使用するプロセスを簡略化できます`Task`オブジェクト。 このカプセル化はの複数のオーバー ロードによって提供される、`TaskFactory.FromAsync`メソッドです。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn です。

### <a name="creating-the-todoservice-object"></a>TodoService オブジェクトを作成します。

生成されたプロキシ クラスを提供、 `TodoService` ASMX サービスと HTTP 経由の通信に使用されるクラスです。 メソッドを呼び出す web サービス URI から操作を非同期にサービス インスタンスが識別された機能を提供します。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)です。

`TodoService`次のコード例のように、ASMX サービスを使用するアプリケーションに必要な限りのオブジェクトが存在するように、クラス レベルでインスタンスが宣言されています。

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

`TodoService`コンス トラクターは、ASMX サービス インスタンスの URL を指定する省略可能な文字列パラメーターを受け取ります。 パブリッシュされた複数のインスタンスがあること、ASMX サービスの異なるインスタンスに接続するアプリケーションが有効にします。

### <a name="creating-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラスです。 格納する、`TodoItem`生成されたプロキシを変換する必要がありますまず web サービス内の項目`TodoItem`型です。 これは、`ToASMXServiceTodoItem`メソッドを次のコード例に示すようにします。

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

このメソッドは単に新しい作成`ASMService.TodoItem`インスタンスし、同一のプロパティを各プロパティを設定、`TodoItem`インスタンス。

同様に、データは、web サービスから取得された、変換する必要あります生成されたプロキシから`TodoItem`型を`TodoItem`インスタンス。 これを使用、`FromASMXServiceTodoItem`メソッドを次のコード例に示すようにします。

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

このメソッドは、生成されたプロキシからのデータを取得するだけで`TodoItem`を入力し、新しく作成された設定`TodoItem`インスタンス。

### <a name="retrieving-data"></a>データの取得

`TodoService.BeginGetTodoItems`と`TodoService.EndGetTodoItems`メソッドが呼び出すために使用される、 `GetTodoItems` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndGetTodoItems`メソッドを 1 回、`TodoService.BeginGetTodoItems`メソッドが完了するで、`null`パラメーターにデータが渡されるないことを示す、`BeginGetTodoItems`を委任します。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

`TodoService.EndGetTodoItems`メソッドの配列を返します`ASMXService.TodoItem`のインスタンスに変換し、`List`の`TodoItem`表示のインスタンス。

### <a name="creating-data"></a>データの作成

`TodoService.BeginCreateTodoItem`と`TodoService.EndCreateTodoItem`メソッドが呼び出すために使用される、 `CreateTodoItem` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndCreateTodoItem`メソッドを 1 回、`TodoService.BeginCreateTodoItem`メソッドが完了すると、使用、`todoItem`パラメーターに渡されるデータ、 `BeginCreateTodoItem` を指定するデリゲート`TodoItem` web サービスによって作成されます。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

Web サービスがスロー、`SoapException`作成に失敗した場合、`TodoItem`が、アプリケーションによって処理されます。

### <a name="updating-data"></a>データの更新

`TodoService.BeginEditTodoItem`と`TodoService.EndEditTodoItem`メソッドが呼び出すために使用される、 `EditTodoItem` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndEditTodoItem`メソッドを 1 回、`TodoService.BeginCreateTodoItem`メソッドが完了すると、使用、`todoItem`パラメーターに渡されるデータ、 `BeginEditTodoItem` を指定するデリゲート`TodoItem` web サービスで更新します。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

Web サービスがスロー、`SoapException`探すか、更新に失敗した場合、`TodoItem`が、アプリケーションで処理します。

### <a name="deleting-data"></a>データの削除

`TodoService.BeginDeleteTodoItem`と`TodoService.EndDeleteTodoItem`メソッドが呼び出すために使用される、 `DeleteTodoItem` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoService.EndDeleteTodoItem`メソッドを 1 回、`TodoService.BeginDeleteTodoItem`メソッドが完了すると、使用、`id`パラメーターに渡されるデータ、 `BeginDeleteTodoItem` を指定するデリゲート`TodoItem` web サービスによって削除されます。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

Web サービスがスロー、`SoapException`探すか、削除に失敗した場合、`TodoItem`が、アプリケーションによって処理されます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションからの ASMX web サービスを利用する方法を示しました。 Asmx サービスは、SOAP を使用して HTTP 経由でメッセージを送信する web サービスをビルドする機能を提供します。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスの実装に使用するプログラミング言語に関する知識は必要はありません。 のみ、SOAP メッセージを送受信する方法を理解する必要があります。


## <a name="related-links"></a>関連リンク

- [TodoASMX (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
