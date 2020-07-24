---
title: ASP.NET Web サービス (ASMX) を使用する
description: この記事では、アプリケーションから ASMX SOAP サービスを使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: aa600974cdf25f8f85d9152edc4a377334cc8c78
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936553"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>ASP.NET Web サービス (ASMX) を使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX は、Simple Object Access Protocol (SOAP) を使用してメッセージを送信する web サービスを構築する機能を提供します。SOAP は、web サービスを構築してアクセスするための、プラットフォームに依存しない、言語に依存しないプロトコルです。ASMX サービスのコンシューマーは、サービスの実装に使用されるプラットフォーム、オブジェクトモデル、プログラミング言語について何も知る必要はありません。SOAP メッセージを送受信する方法を理解する必要があるだけです。この記事では、アプリケーションから ASMX SOAP サービスを使用する方法について説明し Xamarin.Forms ます。_

SOAP メッセージは、次の要素を含む XML ドキュメントです。

- SOAP メッセージとして XML ドキュメントを識別する、 *Envelope*という名前のルート要素。
- 認証データなどのアプリケーション固有の情報を格納する*ヘッダー*要素 (省略可能)。 *Header*要素が存在する場合は、 *Envelope*要素の最初の子要素である必要があります。
- 受信者を対象とした SOAP メッセージを含む必須の*Body*要素。
- エラーメッセージを示すために使用されるオプションの*Fault*要素。 *Fault*要素が存在する場合は、 *Body*要素の子要素である必要があります。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポートプロトコルに対して動作します。 ただし、ASMX サービスは HTTP 経由でのみ動作します。 Xamarin プラットフォームは、HTTP を介した標準的な SOAP 1.1 実装をサポートしています。これには、標準的な ASMX サービス構成の多くがサポートされています。

このサンプルには、物理デバイスまたはエミュレートされたデバイスで実行されるモバイルアプリケーションと、データを取得、追加、編集、および削除するためのメソッドを提供する ASMX サービスが含まれています。 モバイルアプリケーションを実行すると、次のスクリーンショットに示すように、ローカルでホストされている ASMX サービスに接続します。

![サンプル アプリケーション](asmx-images/portal.png)

> [!NOTE]
> IOS 9 以降では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。 IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。
> `HTTPS`インターネットリソースに対してプロトコルとセキュリティで保護された通信を使用できない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用する

ASMX サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成する|XML シリアル化 TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化 TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化 TodoItem|

アプリケーションで使用されるデータモデルの詳細については、「[データのモデリング](~/xamarin-forms/data-cloud/web-services/introduction.md)」を参照してください。

## <a name="create-the-todoservice-proxy"></a>TodoService プロキシを作成する

と呼ばれるプロキシクラスは、を `TodoService` 拡張 `SoapHttpClientProtocol` し、HTTP 経由で ASMX サービスと通信するためのメソッドを提供します。 プロキシは、Visual Studio 2019 または Visual Studio 2017 のプラットフォーム固有の各プロジェクトへの web 参照を追加することによって生成されます。 Web 参照は、サービスの Web サービス記述言語 (WSDL) ドキュメントで定義されている各アクションのメソッドとイベントを生成します。

たとえば、 `GetTodoItems` サービスアクションによっ `GetTodoItemsAsync` て、プロキシにメソッドとイベントが生成され `GetTodoItemsCompleted` ます。 生成されたメソッドは void 戻り値の型を持ち、 `GetTodoItems` 親クラスでアクションを呼び出し `SoapHttpClientProtocol` ます。 呼び出されたメソッドがサービスから応答を受け取ると、そのイベントが発生 `GetTodoItemsCompleted` し、イベントのプロパティ内に応答データが提供され `Result` ます。

## <a name="create-the-isoapservice-implementation"></a>ISoapService 実装を作成する

このサンプルでは、共有のクロスプラットフォームプロジェクトがサービスと連携できるようにするために、 `ISoapService` [C# のタスクの非同期プログラミングモデル](/dotnet/csharp/programming-guide/concepts/async/)に従ったインターフェイスを定義しています。 各プラットフォームには、プラットフォーム固有のプロキシを公開するためのが実装されて `ISoapService` います。 このサンプルでは、 `TaskCompletionSource` オブジェクトを使用して、プロキシをタスクの非同期インターフェイスとして公開します。 の使用方法の詳細について `TaskCompletionSource` は、以下のセクションの各アクションの種類の実装を参照してください。

サンプルは `SoapService` 次のとおりです。

1. を `TodoService` クラスレベルのインスタンスとしてインスタンス化します。
1. `Items`オブジェクトを格納するために呼び出されるコレクションを作成します。 `TodoItem`
1. のオプションのプロパティのカスタムエンドポイントを指定します。 `Url``TodoService`

```csharp
public class SoapService : ISoapService
{
    ASMXService.TodoService todoService;
    public List<TodoItem> Items { get; private set; } = new List<TodoItem>();

    public SoapService ()
    {
        todoService = new ASMXService.TodoService ();
        todoService.Url = Constants.SoapUrl;
        ...
    }
}
```

### <a name="create-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプルアプリケーションでは、クラスを使用して `TodoItem` データをモデル化します。 Web サービスに項目を格納するには、 `TodoItem` 最初にプロキシ生成型に変換する必要があり `TodoItem` ます。 これは、次の `ToASMXServiceTodoItem` コード例に示すように、メソッドによって実現されます。

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

このメソッドは、新しい `ASMService.TodoItem` インスタンスを作成し、インスタンスの各プロパティを同一のプロパティに設定し `TodoItem` ます。

同様に、web サービスからデータを取得する場合は、プロキシによって生成された型からインスタンスにデータを変換する必要があり `TodoItem` `TodoItem` ます。 これは、次の `FromASMXServiceTodoItem` コード例に示すように、メソッドを使用して行います。

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

このメソッドは、プロキシによって生成された型からデータを取得 `TodoItem` し、新しく作成されたインスタンスに設定し `TodoItem` ます。

### <a name="retrieve-data"></a>データを取得する

インターフェイスは、 `ISoapService` `RefreshDataAsync` メソッドが項目コレクションと共にを返すことを想定して `Task` います。 ただし、この `TodoService.GetTodoItemsAsync` メソッドは void を返します。 インターフェイスパターンを満たすには、を呼び出し、 `GetTodoItemsAsync` `GetTodoItemsCompleted` イベントが起動するまで待機して、コレクションにデータを設定する必要があります。 これにより、有効なコレクションを UI に返すことができます。

次の例では、新しいを作成し `TaskCompletionSource` 、メソッドで非同期呼び出しを開始し、に `RefreshDataAsync` `Task` よって提供されるを待機し `TaskCompletionSource` ます。 `TodoService_GetTodoItemsCompleted`イベントハンドラーが呼び出されると、コレクションが設定され `Items` 、が更新され `TaskCompletionSource` ます。

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> getRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.GetTodoItemsCompleted += TodoService_GetTodoItemsCompleted;
    }

    public async Task<List<TodoItem>> RefreshDataAsync()
    {
        getRequestComplete = new TaskCompletionSource<bool>();
        todoService.GetTodoItemsAsync();
        await getRequestComplete.Task;
        return Items;
    }

    private void TodoService_GetTodoItemsCompleted(object sender, ASMXService.GetTodoItemsCompletedEventArgs e)
    {
        try
        {
            getRequestComplete = getRequestComplete ?? new TaskCompletionSource<bool>();

            Items = new List<TodoItem>();
            foreach (var item in e.Result)
            {
                Items.Add(FromASMXServiceTodoItem(item));
            }
            getRequestComplete?.TrySetResult(true);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(@"\t\tERROR {0}", ex.Message);
        }
    }

    ...
}
```

詳細については、「[非同期プログラミングモデル](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm)と[TPL および従来の .NET Framework 非同期プログラミング](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming)」を参照してください。

### <a name="create-or-edit-data"></a>データの作成または編集

データを作成または編集する場合は、メソッドを実装する必要があり `ISoapService.SaveTodoItemAsync` ます。 このメソッド `TodoItem` は、が新規または更新された項目であるかどうかを検出し、オブジェクトに対して適切なメソッドを呼び出し `todoService` ます。 `CreateTodoItemCompleted`および `EditTodoItemCompleted` イベントハンドラーは、が ASMX サービスからの応答を受信したことを知るためにも実装する必要があり `todoService` ます (これらは同じ操作を実行するため、1つのハンドラーにまとめることができます)。 次の例では、インターフェイスとイベントハンドラーの実装、および `TaskCompletionSource` 非同期操作に使用されるオブジェクトを示します。

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> saveRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.CreateTodoItemCompleted += TodoService_SaveTodoItemCompleted;
        todoService.EditTodoItemCompleted += TodoService_SaveTodoItemCompleted;
    }

    public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
    {
        try
        {
            var todoItem = ToASMXServiceTodoItem(item);
            saveRequestComplete = new TaskCompletionSource<bool>();
            if (isNewItem)
            {
                todoService.CreateTodoItemAsync(todoItem);
            }
            else
            {
                todoService.EditTodoItemAsync(todoItem);
            }
            await saveRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_SaveTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        saveRequestComplete?.TrySetResult(true);
    }

    ...
}
```

### <a name="delete-data"></a>データの削除

データを削除するには、同様の実装が必要です。 を定義し `TaskCompletionSource` 、イベントハンドラーを実装し、メソッドを実装し `ISoapService.DeleteTodoItemAsync` ます。

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> deleteRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.DeleteTodoItemCompleted += TodoService_DeleteTodoItemCompleted;
    }

    public async Task DeleteTodoItemAsync (string id)
    {
        try
        {
            deleteRequestComplete = new TaskCompletionSource<bool>();
            todoService.DeleteTodoItemAsync(id);
            await deleteRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_DeleteTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        deleteRequestComplete?.TrySetResult(true);
    }

    ...
}
```

## <a name="test-the-web-service"></a>Web サービスをテストする

ローカルにホストされるサービスを使用して物理デバイスまたはエミュレートされたデバイスをテストするには、カスタムの IIS 構成、エンドポイントアドレス、およびファイアウォール規則を設定する必要があります。 テスト用に環境をセットアップする方法の詳細については、 [IIS Express へのリモートアクセスの構成](wcf.md#configure-remote-access-to-iis-express)に関するセクションを参照してください。 WCF と ASMX のテストの唯一の違いは、TodoService のポート番号です。

## <a name="related-links"></a>関連リンク

- [TodoASMX (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
