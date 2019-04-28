---
title: ASP.NET Web サービス (ASMX) の使用します。
description: この記事では、Xamarin.Forms アプリケーションから ASMX SOAP サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 71fb2b4742a66a560541febc9dbe87aed503da4c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61328643"
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

このサンプルには、物理またはエミュレートされたデバイスを実行しているモバイル アプリケーションと取得、追加、編集、およびデータを削除するメソッドを提供する ASMX サービスが含まれています。 モバイル アプリケーションを実行するとときに、接続する ASMX のローカルにホストされているサービスの次のスクリーン ショット。

![](asmx-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用します。

ASMX サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータ モデルの詳細については、[、データのモデリング](~/xamarin-forms/data-cloud/walkthrough.md)を参照してください。

## <a name="create-the-todoservice-proxy"></a>TodoService プロキシを作成します。

プロキシ クラスと呼ばれる`TodoService`、拡張`SoapHttpClientProtocol`HTTP 経由で、ASMX サービスと通信するためのメソッドを提供します。 プロキシは、Visual Studio 2019 または Visual Studio 2017 でプラットフォーム固有の各プロジェクトに web 参照の追加によって生成されます。 Web 参照には、サービスの Web サービス記述言語 (WSDL) ドキュメントで定義されている各アクションのメソッドおよびイベントが生成されます。

たとえば、`GetTodoItems`サービス操作の結果、`GetTodoItemsAsync`メソッドと`GetTodoItemsCompleted`プロキシ内のイベント。 生成されたメソッドが void の戻り値の型と呼び出す、`GetTodoItems`親アクション`SoapHttpClientProtocol`クラス。 呼び出されたメソッドでは、サービスから応答を受信するときに発生させる、`GetTodoItemsCompleted`イベント イベントの中で応答データを提供します`Result`プロパティ。

## <a name="create-the-isoapservice-implementation"></a>ISoapService 実装を作成します。

サンプルを定義、サービスを使用する共有のクロスプラット フォーム対応のプロジェクトを有効にする、`ISoapService`する次のように、インターフェイス、[タスク非同期プログラミング モデルでC#](/dotnet/csharp/programming-guide/concepts/async/)します。 各プラットフォームの実装、`ISoapService`プラットフォーム固有のプロキシを公開します。 サンプルでは`TaskCompletionSource`タスクの非同期インターフェイスとしてプロキシを公開するオブジェクト。 使用の詳細について`TaskCompletionSource`以下のセクションで、各アクションの種類の実装が見つかりました。

サンプル`SoapService`:

1. インスタンスを作成、`TodoService`クラス レベルのインスタンスとして
1. 呼ばれるコレクションを作成します`Items`を格納する`TodoItem`オブジェクト。
1. 省略可能なカスタム エンドポイントを指定します`Url`プロパティを `TodoService`

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

### <a name="create-data-transfer-objects"></a>データ転送オブジェクトを作成します。

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

このメソッドは、新しい作成`ASMService.TodoItem`インスタンス、および各プロパティから同じプロパティを設定して、`TodoItem`インスタンス。

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

このメソッドは、生成されたプロキシからデータを取得`TodoItem`を入力し、新しく作成設定`TodoItem`インスタンス。

### <a name="retrieve-data"></a>データを取得します。

`ISoapService`インターフェイスが必要ですが、`RefreshDataAsync`を返すメソッドを`Task`項目コレクションを使用します。 ただし、`TodoService.GetTodoItemsAsync`メソッドが void を返します。 インターフェイス パターンを満たすために呼び出す必要がある`GetTodoItemsAsync`、待機、`GetTodoItemsCompleted`イベントを起動するには、コレクションを設定します。 これにより、UI に有効なコレクションを戻すことができます。

次の例は、新しい作成`TaskCompletionSource`、内の非同期呼び出しを開始、`RefreshDataAsync`メソッド、待機、`Task`によって提供される、 `TaskCompletionSource`。 ときに、`TodoService_GetTodoItemsCompleted`イベント ハンドラーが呼び出されますが設定されます、`Items`コレクションと更新プログラム、 `TaskCompletionSource`:

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

詳細については、次を参照してください。[非同期プログラミング モデル](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm)と[TPL と従来の .NET Framework 非同期プログラミング](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming)します。

### <a name="create-or-edit-data"></a>作成またはデータの編集

実装する必要がありますを作成または、データを編集するとき、`ISoapService.SaveTodoItemAsync`メソッド。 このメソッドを検出するかどうか、`TodoItem`新しいまたは更新された項目は、適切なメソッドを呼び出します、`todoService`オブジェクト。 `CreateTodoItemCompleted`と`EditTodoItemCompleted`タイミングを判断するため、イベント ハンドラーを実装する必要がありますも、 `todoService` (これらに統合できる、単一のハンドラーと同じ操作を実行するため) ASMX サービスから応答を受信しました。 次の例では、インターフェイスとイベント ハンドラーの実装だけでなく`TaskCompletionSource`オブジェクトを非同期的に動作するために使用します。

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

### <a name="delete-data"></a>データを削除します。

データを削除するには、同様の実装が必要です。 定義、 `TaskCompletionSource`、イベント ハンドラーを実装し、`ISoapService.DeleteTodoItemAsync`メソッド。

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

## <a name="test-the-web-service"></a>Web サービスをテストします。

物理デバイスまたはエミュレートされたデバイスでローカルにホストされているサービスをテストするには、有効にするには、カスタムの IIS 構成、エンドポイント アドレス、およびファイアウォール規則が必要です。 テスト環境をセットアップする方法の詳細については、次を参照してください。、 [IIS Express へのリモート アクセスを構成する](wcf.md#configure-remote-access-to-iis-express)します。 WCF と ASMX のテストの唯一の違いは、TodoService のポート番号です。

## <a name="related-links"></a>関連リンク

- [TodoASMX (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
