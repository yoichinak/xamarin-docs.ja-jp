---
title: "Windows Communication Foundation (WCF) Web サービスを使用"
description: "WCF は、サービス指向アプリケーションを構築するための Microsoft の統一フレームワークです。 セキュリティで保護された、信頼性、トランザクション、および相互運用可能な分散アプリケーションを構築する開発者ができるようにします。 この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 5cf194dce9bf4d0af23ba663ab00cf94a8a1766c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) Web サービスを使用

_WCF は、サービス指向アプリケーションを構築するための Microsoft の統一フレームワークです。セキュリティで保護された、信頼性、トランザクション、および相互運用可能な分散アプリケーションを構築する開発者ができるようにします。この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。_

WCF では、さまざまな異なるコントラクトで、次のサービスについて説明します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を成してデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **コントラクトをエラー** – を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**: サービスをサポートする操作を指定し、各操作と対話するために必要なメッセージです。 また、各サービスでの操作に関連付けることができるすべてのカスタム エラー動作を指定します。

ASP.NET Web サービス (ASMX) と、WCF の違いがあるが、WCF が、同じ機能を備えた ASMX – HTTP 経由の SOAP メッセージをサポートしていることを理解することが重要です。 ASMX サービスの使用の詳細については、次を参照してください。[消費 ASP.NET Web サービス (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)です。

一般に、Xamarin プラットフォームでは、同じクライアント側 Silverlight ランタイムに付属する WCF のサブセットをサポートしています。 これには、WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます: HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`クラスです。 さらに、WCF のサポートには、プロキシを生成するための Windows 環境でのみ使用できるツールの使用が必要です。

WCF サービスの設定方法は、サンプル アプリケーションに付属している readme ファイルで確認できます。 ただし、サンプル アプリケーションの実行時に、接続、データへの読み取り専用のアクセスを提供する Xamarin でホストされる WCF サービスに次のスクリーン ショットに示すようにします。

![](wcf-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="consuming-the-web-service"></a>Web サービスの使用

WCF サービスでは、次の操作を提供します。

<table>
  <thead>
    <tr>
      <th>操作</th>
      <th>説明</th>
      <th>パラメーター</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GetTodoItems</td>
      <td>作業アイテムの一覧を取得します。</td>
      <td></td>
    </tr>
    <tr>
      <td>CreateTodoItem</td>
      <td>新しい作業項目を作成します。</td>
      <td>シリアル化された XML <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>EditTodoItem</td>
      <td>作業項目を更新します。</td>
      <td>シリアル化された XML <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>DeleteTodoItem</td>
      <td>作業項目を削除します。</td>
      <td>シリアル化された XML <code>TodoItem</code></td>
    </tr>
  </tbody>
</table>

アプリケーションで使用されるデータ モデルの詳細については、次を参照してください。[データ モデリング](~/xamarin-forms/data-cloud/walkthrough.md)です。

> [!NOTE]
> サンプル アプリケーションは、web サービスへの読み取り専用のアクセスを提供する Xamarin でホストされる WCF サービスを使用します。 そのため、作成、更新、およびデータを削除する操作では、アプリケーションで使用するデータは変更されません。 ただし、ASMX サービスのホスト可能なバージョンは利用で、 **TodoWCFService**付随するサンプル アプリケーション内のフォルダーです。 このホスト可能なバージョン完全 WCF サービスの許可の作成、更新、読み取り、およびデータへのアクセスを削除します。

A*プロキシ*を使用した WCF サービス、により、アプリケーション、サービスへの接続を生成する必要があります。 メソッドと関連付けられているサービス構成を定義するサービス メタデータを使用して、プロキシが構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 .NET 標準ライブラリに、web サービスのサービス参照を追加する Visual Studio 2017 で Microsoft WCF Web サービス参照のプロバイダーを使用して、プロキシを構築できます。 Visual Studio 2017 で Microsoft WCF Web サービス参照のプロバイダーを使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、次を参照してください。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)です。

生成されたプロキシ クラスでは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 という名前の 2 つの方法として、このパターンで非同期操作が実装されている*BeginOperationName*と*EndOperationName*、これを開始および非同期の操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始しを実装するオブジェクトを返します、`IAsyncResult`インターフェイスです。 呼び出した後*BeginOperationName*、スレッド プールのスレッドで非同期操作の実行中のスレッドの呼び出しに関する命令を実行するアプリケーションを続行できます。

呼び出しごとに*BeginOperationName*、アプリケーションを呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*同期 web サービス メソッドによって返される同じ型です。 たとえば、`EndGetTodoItems`メソッドのコレクションを返します`TodoItem`インスタンス。 *EndOperationName*メソッドも含まれています、`IAsyncResult`パラメーターに対応する呼び出しによって返されるインスタンスに設定する必要がありますが、 *BeginOperationName*メソッドです。

タスク並列ライブラリ (TPL) は同じで、非同期操作をカプセル化して APM 開始/終了メソッドのペアを使用するプロセスを簡略化できます`Task`オブジェクト。 このカプセル化はの複数のオーバー ロードによって提供される、`TaskFactory.FromAsync`メソッドです。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn です。

### <a name="creating-the-todoserviceclient-object"></a>TodoServiceClient オブジェクトを作成します。

生成されたプロキシ クラスを提供、 `TodoServiceClient` HTTP 経由で WCF サービスと通信するために使用されるクラスです。 メソッドを呼び出す web サービス URI から操作を非同期にサービス インスタンスが識別された機能を提供します。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)です。

`TodoServiceClient`次のコード例に示すように WCF サービスを利用するアプリケーションに必要な限りのオブジェクトが存在するように、クラス レベルでインスタンスが宣言されています。

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

`TodoServiceClient`インスタンスは、バインディング情報およびエンドポイント アドレスを構成します。 バインドを使用して、トランスポート、エンコーディング、およびアプリケーションとサービスが互いに通信するために必要なプロトコルの詳細を指定します。 `BasicHttpBinding`テキストでエンコードされた SOAP メッセージを HTTP トランスポート プロトコルを介して送信されることを指定します。 パブリッシュされた複数のインスタンスがあること、WCF サービスの異なるインスタンスに接続するアプリケーションをエンドポイント アドレスの指定できます。

サービス参照の構成の詳細については、次を参照してください。[サービス参照を構成する](~/cross-platform/data-cloud/web-services/index.md#wcf)です。

### <a name="creating-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラスです。 格納する、`TodoItem`生成されたプロキシを変換する必要がありますまず web サービス内の項目`TodoItem`型です。 これは、`ToWCFServiceTodoItem`メソッドを次のコード例に示すようにします。

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

このメソッドは単に新しい作成`TodoWCFService.TodoItem`インスタンスし、同一のプロパティを各プロパティを設定、`TodoItem`インスタンス。

同様に、データは、web サービスから取得された、変換する必要あります生成されたプロキシから`TodoItem`型を`TodoItem`インスタンス。 これを使用、`FromWCFServiceTodoItem`メソッドを次のコード例に示すようにします。

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

このメソッドは、生成されたプロキシからのデータを取得するだけで`TodoItem`を入力し、新しく作成された設定`TodoItem`インスタンス。

### <a name="retrieving-data"></a>データの取得

`TodoServiceClient.BeginGetTodoItems`と`TodoServiceClient.EndGetTodoItems`メソッドが呼び出すために使用される、 `GetTodoItems` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndGetTodoItems`メソッドを 1 回、`TodoServiceClient.BeginGetTodoItems`メソッドが完了するで、`null`パラメーターにデータが渡されるないことを示す、`BeginGetTodoItems`を委任します。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

`TodoServiceClient.EndGetTodoItems`メソッドを返します、`ObservableCollection`の`TodoWCFService.TodoItem`のインスタンスに変換し、`List`の`TodoItem`表示のインスタンス。

### <a name="creating-data"></a>データの作成

`TodoServiceClient.BeginCreateTodoItem`と`TodoServiceClient.EndCreateTodoItem`メソッドが呼び出すために使用される、 `CreateTodoItem` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndCreateTodoItem`メソッドを 1 回、`TodoServiceClient.BeginCreateTodoItem`メソッドが完了すると、使用、`todoItem`パラメーターに渡されるデータ、 `BeginCreateTodoItem` を指定するデリゲート`TodoItem` web サービスによって作成されます。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

Web サービスがスロー、`FaultException`作成に失敗した場合、`TodoItem`が、アプリケーションによって処理されます。

### <a name="updating-data"></a>データの更新

`TodoServiceClient.BeginEditTodoItem`と`TodoServiceClient.EndEditTodoItem`メソッドが呼び出すために使用される、 `EditTodoItem` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndEditTodoItem`メソッドを 1 回、`TodoServiceClient.BeginCreateTodoItem`メソッドが完了すると、使用、`todoItem`パラメーターに渡されるデータ、 `BeginEditTodoItem` を指定するデリゲート`TodoItem` web サービスで更新します。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

Web サービスがスロー、`FaultException`探すか、更新に失敗した場合、`TodoItem`が、アプリケーションで処理します。

### <a name="deleting-data"></a>データの削除

`TodoServiceClient.BeginDeleteTodoItem`と`TodoServiceClient.EndDeleteTodoItem`メソッドが呼び出すために使用される、 `DeleteTodoItem` web サービスによって提供される操作です。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndDeleteTodoItem`メソッドを 1 回、`TodoServiceClient.BeginDeleteTodoItem`メソッドが完了すると、使用、`id`パラメーターに渡されるデータ、 `BeginDeleteTodoItem` を指定するデリゲート`TodoItem` web サービスによって削除されます。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

Web サービスがスロー、`FaultException`探すか、削除に失敗した場合、`TodoItem`が、アプリケーションによって処理されます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションから WCF SOAP サービスを使用する方法を示しました。 一般に、Xamarin プラットフォームでは、同じクライアント側 Silverlight ランタイムに付属する WCF のサブセットをサポートしています。 これには、WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます: HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`クラスです。 さらに、WCF のサポートには、プロキシを生成するための Windows 環境でのみ使用できるツールの使用が必要です。


## <a name="related-links"></a>関連リンク

- [TodoWCF (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
