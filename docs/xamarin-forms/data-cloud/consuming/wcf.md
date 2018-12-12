---
title: Windows Communication Foundation (WCF) Web サービスを使用
description: この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 7e8acc6e8aaf8b8e0e8cec7d5d0f3e28cf60073a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055596"
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) Web サービスを使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF は、サービス指向アプリケーションを構築するための Microsoft の統一されたフレームワークです。セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。_

WCF では、次を含むさまざまなコントラクトのさまざまなサービスについて説明します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を形成するデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **フォールト コントラクト**– を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**– と、メッセージが各操作と対話するために必要なサービスをサポートする操作を指定します。 また、各サービスでの操作に関連付けることができる任意のカスタム エラー動作を指定します。

ASP.NET Web サービス (ASMX) と WCF では、違いがありますが、WCF が、同じ ASMX が提供する機能: HTTP 経由の SOAP メッセージをサポートしているかを理解することが重要です。 ASMX サービスの使用に関する詳細については、次を参照してください。[消費の ASP.NET Web サービス (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)します。

一般に、Xamarin プラットフォームでは、Silverlight ランタイムに付属する WCF のクライアント側のサブセットが同じをサポートしています。 WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます: HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`クラス。 さらに、WCF のサポートには、プロキシを生成する Windows 環境でのみ使用できるツールの使用が必要です。

WCF サービスの設定方法については、サンプル アプリケーションに付属する readme ファイルで確認できます。 ただし、サンプル アプリケーションの実行時に、次のスクリーン ショットに示すように、データへの読み取り専用アクセスを提供する Xamarin でホストされる WCF サービスに接続には。

![](wcf-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)します。

## <a name="consuming-the-web-service"></a>Web サービスの使用

WCF サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータ モデルの詳細については、次を参照してください。 [、データのモデリング](~/xamarin-forms/data-cloud/walkthrough.md)します。

> [!NOTE]
> サンプル アプリケーションでは、web サービスへの読み取り専用アクセスを提供する、Xamarin でホストされる WCF サービスを消費します。 そのため、作成、更新、およびデータを削除する操作では、アプリケーションで使用されるデータは変更されません。 ただし、ASMX サービスのホスト可能なバージョンが記載されて、 **TodoWCFService**付属のサンプル アプリケーション内のフォルダー。 このホスト可能なバージョンの WCF サービスが完全な作成、更新、読み取り、およびデータへのアクセスを削除します。

A*プロキシ*により、アプリケーションは、サービスに接続する WCF サービスを使用する生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 Visual Studio 2017 で .NET Standard ライブラリを web サービスのサービス参照を追加する Microsoft WCF Web Service Reference Provider を使用して、プロキシを構築できます。 Visual Studio 2017 での Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、次を参照してください。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)します。

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 たとえば、`EndGetTodoItems`メソッドのコレクションを返します`TodoItem`インスタンス。 *EndOperationName*メソッドも含まれています、`IAsyncResult`パラメーターに対応する呼び出しによって返されるインスタンスに設定する必要がありますが、 *BeginOperationName*メソッド。

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`TaskFactory.FromAsync`メソッド。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn です。

### <a name="creating-the-todoserviceclient-object"></a>TodoServiceClient オブジェクトを作成します。

生成されたプロキシ クラスには、`TodoServiceClient`クラスは、HTTP 経由で WCF サービスと通信するために使用します。 識別されたサービス インスタンスの URI からの非同期操作は、web サービス メソッドを呼び出すために、機能を提供します。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)します。

`TodoServiceClient`次のコード例に示すように、WCF サービスを使用するアプリケーションに必要な限りのオブジェクトが存在するために、クラス レベルでインスタンスが宣言されています。

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient`インスタンスはバインド情報とエンドポイント アドレスで構成します。 バインディングを使用して、トランスポート、エンコーディング、およびアプリケーションとサービスが互いに通信するために必要なプロトコルの詳細を指定します。 `BasicHttpBinding`テキストでエンコードされた SOAP メッセージは、HTTP トランスポート プロトコル経由で送信されることを指定します。 パブリッシュされた複数のインスタンスがあること、WCF サービスの異なるインスタンスに接続するアプリケーションをエンドポイント アドレスの指定できます。

サービス参照を構成する方法の詳細については、次を参照してください。[サービス参照を構成する](~/cross-platform/data-cloud/web-services/index.md#wcf)します。

### <a name="creating-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラス。 格納する、`TodoItem`最初生成されたプロキシに変換する必要がある、web サービス内の項目`TodoItem`型。 これは、`ToWCFServiceTodoItem`メソッドを次のコード例に示すようにします。

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

このメソッドは単に新しい作成`TodoWCFService.TodoItem`インスタンス、および各プロパティから同じプロパティを設定して、`TodoItem`インスタンス。

同様に、web サービスからデータを取得すると、ときにする必要があります変換する必要が生成されたプロキシから`TodoItem`型、`TodoItem`インスタンス。 これを実行、`FromWCFServiceTodoItem`メソッドを次のコード例に示すようにします。

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
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

`TodoServiceClient.BeginGetTodoItems`と`TodoServiceClient.EndGetTodoItems`メソッド呼び出しを使用して、 `GetTodoItems` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndGetTodoItems`メソッドを 1 回、`TodoServiceClient.BeginGetTodoItems`メソッドが完了したらで、`null`パラメーターにデータが渡されていないことを示す、`BeginGetTodoItems`デリゲートします。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

`TodoServiceClient.EndGetTodoItems`メソッドを返します、`ObservableCollection`の`TodoWCFService.TodoItem`に変換し、インスタンス、`List`の`TodoItem`表示のインスタンス。

### <a name="creating-data"></a>データの作成

`TodoServiceClient.BeginCreateTodoItem`と`TodoServiceClient.EndCreateTodoItem`メソッド呼び出しを使用して、 `CreateTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndCreateTodoItem`メソッドを 1 回、`TodoServiceClient.BeginCreateTodoItem`メソッドが完了したらで、`todoItem`パラメーターに渡されるデータ、 `BeginCreateTodoItem` を指定するデリゲート`TodoItem` web サービスによって作成されます。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`FaultException`作成に失敗した場合、`TodoItem`をアプリケーションでこれを処理します。

### <a name="updating-data"></a>データの更新

`TodoServiceClient.BeginEditTodoItem`と`TodoServiceClient.EndEditTodoItem`メソッド呼び出しを使用して、 `EditTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndEditTodoItem`メソッドを 1 回、`TodoServiceClient.BeginCreateTodoItem`メソッドが完了したらで、`todoItem`パラメーターに渡されるデータ、 `BeginEditTodoItem` を指定するデリゲート`TodoItem` web サービスで更新します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`FaultException`を探すか、更新に失敗した場合、 `TodoItem`、アプリケーションでこれを処理します。

### <a name="deleting-data"></a>データの削除

`TodoServiceClient.BeginDeleteTodoItem`と`TodoServiceClient.EndDeleteTodoItem`メソッド呼び出しを使用して、 `DeleteTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndDeleteTodoItem`メソッドを 1 回、`TodoServiceClient.BeginDeleteTodoItem`メソッドが完了したらで、`id`パラメーターに渡されるデータ、 `BeginDeleteTodoItem` を指定するデリゲート`TodoItem` web サービスによって削除されます。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`FaultException`を探すか、削除に失敗した場合、`TodoItem`をアプリケーションでこれを処理します。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションから WCF SOAP サービスを使用する方法を示しました。 一般に、Xamarin プラットフォームでは、Silverlight ランタイムに付属する WCF のクライアント側のサブセットが同じをサポートしています。 WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます: HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`クラス。 さらに、WCF のサポートには、プロキシを生成する Windows 環境でのみ使用できるツールの使用が必要です。


## <a name="related-links"></a>関連リンク

- [TodoWCF (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
