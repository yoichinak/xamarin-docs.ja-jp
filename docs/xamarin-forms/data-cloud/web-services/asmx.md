---
title: ASP.NET Web サービス (ASMX) を使用する
description: この記事では、Xamarin.Forms アプリケーションから ASMX SOAP サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 124cffaf827ebeb8109f3cd4ac4dfd2701e43f9c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656656"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>ASP.NET Web サービス (ASMX) を使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX では、簡易オブジェクト アクセス プロトコル (SOAP) を使用してメッセージを送信する web サービスをビルドする機能を提供します。SOAP は、構築、および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。のみの SOAP メッセージを送受信する方法を理解する必要があります。この記事では、Xamarin.Forms アプリケーションから ASMX SOAP サービスを使用する方法を示します。_

SOAP メッセージは、次の要素を含む XML ドキュメントを示します。

- という名前のルート要素*エンベロープ*SOAP メッセージとして XML ドキュメントを識別します。
- 省略可能な*ヘッダー*認証データなどのアプリケーションに固有の情報を含む要素。 場合、*ヘッダー*要素が存在するは、最初の子要素があります、*エンベロープ*要素。
- 必要な*本文*SOAP メッセージの受信者のためのものを含む要素。
- 省略可能な*フォールト*をエラー メッセージを示すために使用される要素。 場合、*フォールト*要素が存在するの子要素があります、*本文*要素。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポート プロトコルで操作できます。 ただし、ASMX サービスは HTTP 経由でのみ操作できます。 Xamarin プラットフォームは、HTTP 経由で SOAP 1.1 の標準的な実装をサポートしていて、標準の ASMX サービスの構成の多くのサポートが含まれます。

このサンプルには、物理デバイスまたはエミュレートされたデバイスで実行されるモバイルアプリケーションと、データを取得、追加、編集、および削除するためのメソッドを提供する ASMX サービスが含まれています。 モバイルアプリケーションを実行すると、次のスクリーンショットに示すように、ローカルでホストされている ASMX サービスに接続します。

![](asmx-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用する

ASMX サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータ モデルの詳細については、[、データのモデリング](~/xamarin-forms/data-cloud/web-services/introduction.md)を参照してください。

## <a name="create-the-todoservice-proxy"></a>TodoService プロキシを作成する

と呼ばれる`TodoService`プロキシクラスは、 `SoapHttpClientProtocol`を拡張し、HTTP 経由で ASMX サービスと通信するためのメソッドを提供します。 プロキシは、Visual Studio 2019 または Visual Studio 2017 のプラットフォーム固有の各プロジェクトへの web 参照を追加することによって生成されます。 Web 参照は、サービスの Web サービス記述言語 (WSDL) ドキュメントで定義されている各アクションのメソッドとイベントを生成します。

たとえば、サービスアクション`GetTodoItems`によって、プロキシ`GetTodoItemsAsync`にメソッドと`GetTodoItemsCompleted`イベントが生成されます。 生成されたメソッドは void 戻り値の型`GetTodoItems`を持ち、親`SoapHttpClientProtocol`クラスでアクションを呼び出します。 呼び出されたメソッドがサービスから応答を受け取ると、そのイベント`GetTodoItemsCompleted`が発生し、イベントの`Result`プロパティ内に応答データが提供されます。

## <a name="create-the-isoapservice-implementation"></a>ISoapService 実装を作成する

このサンプルでは、共有のクロスプラットフォームプロジェクトがサービスと連携できるようにするため`ISoapService`に、[のC#非同期プログラミングモデルのタスク](/dotnet/csharp/programming-guide/concepts/async/)に従うインターフェイスを定義しています。 各プラットフォームには`ISoapService` 、プラットフォーム固有のプロキシを公開するためのが実装されています。 このサンプルで`TaskCompletionSource`は、オブジェクトを使用して、プロキシをタスクの非同期インターフェイスとして公開します。 の使用方法`TaskCompletionSource`の詳細については、以下のセクションの各アクションの種類の実装を参照してください。

サンプル`SoapService`は次のとおりです。

1. をクラス`TodoService`レベルのインスタンスとしてインスタンス化します。
1. オブジェクトを格納`TodoItem`する`Items`ために呼び出されるコレクションを作成します。
1. のオプション`Url`のプロパティのカスタムエンドポイントを指定します。`TodoService`

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

このメソッドは、新しい`ASMService.TodoItem`インスタンスを作成し、 `TodoItem`インスタンスの各プロパティを同一のプロパティに設定します。

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

このメソッドは、プロキシによって生成`TodoItem`された型からデータを取得`TodoItem`し、新しく作成されたインスタンスに設定します。

### <a name="retrieve-data"></a>データの取得

インターフェイス`ISoapService`は、メソッド`RefreshDataAsync`が項目コレクションと`Task`共にを返すことを想定しています。 ただし、この`TodoService.GetTodoItemsAsync`メソッドは void を返します。 インターフェイスパターンを満たすには、を呼び出し`GetTodoItemsAsync`、イベントが`GetTodoItemsCompleted`起動するまで待機して、コレクションにデータを設定する必要があります。 これにより、有効なコレクションを UI に返すことができます。

次の例では、 `TaskCompletionSource`新しいを作成し、 `RefreshDataAsync` `Task`メソッドで非同期呼び出しを開始し、によっ`TaskCompletionSource`て提供されるを待機します。 イベントハンドラーが呼び出されると、コレクションが`Items`設定され、 `TaskCompletionSource`が更新されます。 `TodoService_GetTodoItemsCompleted`

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

データを作成または編集する場合は、 `ISoapService.SaveTodoItemAsync`メソッドを実装する必要があります。 このメソッドは、 `TodoItem`が新規または更新された項目であるかどうかを検出し、 `todoService`オブジェクトに対して適切なメソッドを呼び出します。 および`CreateTodoItemCompleted` `todoService`イベントハンドラーは、が ASMX サービスからの応答を受信したことを知るためにも実装する必要があります (これらは同じ操作を実行するため、1つのハンドラーにまとめることができます)。 `EditTodoItemCompleted` 次の例では、インターフェイスとイベントハンドラーの実装、および`TaskCompletionSource`非同期操作に使用されるオブジェクトを示します。

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

データを削除するには、同様の実装が必要です。 を定義し`ISoapService.DeleteTodoItemAsync` 、イベントハンドラーを実装し、メソッドを`TaskCompletionSource`実装します。

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
