---
title: Azure Mobile Apps のオフライン データの同期
description: この記事では、Azure Mobile Apps バックエンドを使用する Xamarin.Forms アプリケーションにオフライン同期機能を追加する方法について説明します。
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: 6080b4dc152558d6f532399cee7424670c588c28
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058179"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Azure Mobile Apps のオフライン データの同期

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)

_オフライン同期により、データの変更や、表示、追加、モバイル アプリケーションと対話するユーザーのネットワーク接続がないときでもです。変更は、ローカルのデータベースに格納され、デバイスがオンラインになると変更は、Azure Mobile Apps のインスタンスと同期ことができます。この記事では、Xamarin.Forms アプリケーションにオフライン同期機能を追加する方法について説明します。_

## <a name="overview"></a>概要

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)提供、`IMobileServiceTable`インターフェイスで、Azure Mobile Apps インスタンスに格納されたテーブルで実行できる操作を表します。 これらの操作では、Azure Mobile Apps インスタンスに直接接続し、モバイル デバイスは、ネットワーク接続があるない場合は失敗します。

オフライン同期をサポートするために、Azure Mobile クライアント SDK のサポート*テーブルを同期*、によって提供される、`IMobileServiceSyncTable`インターフェイス。 このインターフェイスには、同じの作成、読み取り、更新、削除 (CRUD) 操作が用意されています、`IMobileServiceTable`が、操作からの読み取りまたはローカル ストアに書き込みます。 呼び出しになるまでローカル ストアが Azure Mobile Apps インスタンスの新しいデータで指定されていない*プル*データ。 呼び出しがあるまでに、Azure Mobile Apps インスタンスに送信されていないデータの同様に、*プッシュ*ローカルの変更。

オフライン同期には、Azure Mobile Apps のインスタンスとカスタム競合解決で、両方のローカル ストアと同じレコードが変更されたときの競合を検出のサポートも含まれています。 競合は、ローカル ストア、または Azure Mobile Apps のインスタンスか、処理できます。

オフライン同期の詳細については、[Azure Mobile Apps でのオフライン データ同期](/azure/app-service-mobile/app-service-mobile-offline-data-sync/)と[Xamarin.Forms モバイル アプリのオフライン同期を有効にする](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/)を参照してください。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションと Azure Mobile Apps インスタンス間でのオフライン同期を統合するためのプロセスは次のとおりです。

1. Azure Mobile Apps インスタンスを作成します。 詳細については、[Azure Mobile App の使用](~/xamarin-forms/data-cloud/consuming/azure.md) を参照してください。
1. 追加、 [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) Xamarin.Forms ソリューションのすべてのプロジェクトに NuGet パッケージ。
1. (省略可能)Azure Mobile Apps のインスタンスと、Xamarin.Forms アプリケーションでの認証を有効にします。 詳細については、[Azure Mobile Apps のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md) を参照してください。

次のセクションでは、Microsoft.Azure.Mobile.Client.SQLiteStore NuGet パッケージを使用するユニバーサル Windows プラットフォーム (UWP) プロジェクトを構成するための追加のセットアップ手順を提供します。 追加の設定を iOS と Android で Microsoft.Azure.Mobile.Client.SQLiteStore NuGet パッケージを使用する必要はありません。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) で、SQLite を使用するには、次の手順に従います。

1. インストール、[ユニバーサル Windows プラットフォーム用 SQLite](http://sqlite.org/2016/sqlite-uwp-3120200.vsix)開発環境での Visual Studio 拡張機能。
1. Visual Studio で UWP プロジェクトを右クリックして**参照 > 参照の追加**に移動します**拡張機能**を追加し、**ユニバーサル Windows プラットフォーム用 SQLite**と**ユニバーサル Windows プラットフォーム アプリ用の visual C 2015 ランタイム**UWP プロジェクトに拡張します。

## <a name="initializing-the-local-store"></a>ローカル ストアを初期化しています

同期テーブル操作を実行する前に、ローカル ストアを初期化する必要があります。 これは、Xamarin.Forms ソリューションのポータブル クラス ライブラリ (PCL) プロジェクトで実現されます。

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

によって新しいローカルの SQLite データベースが作成された、`MobileServiceSQLiteStore`クラス、まだ存在していないこと。 次に、`DefineTable<T>`メソッドは、ローカル ストア内のフィールドに一致するテーブルを作成、`TodoItem`存在しないことを入力します。

A*同期コンテキスト*に関連付けられている、`MobileServiceClient`インスタンス、および同期テーブルに対して行われた変更が追跡されます。 同期コンテキストにより、作成、更新、および削除 (CUD) 操作の順序付きリストを保持するキューを後で Azure Mobile Apps インスタンスに送信されます。 `IMobileServiceSyncContext.InitializeAsync()`メソッドを使用して、ローカル ストアを同期コンテキストに関連付けます。

`todoTable`フィールドは、 `IMobileServiceSyncTable`、ため、すべての CRUD 操作は、ローカル ストアを使用します。

## <a name="performing-synchronization"></a>同期を実行します。

ローカル ストアは、Azure Mobile Apps と同期されるインスタンス、`SyncAsync`メソッドが呼び出されます。

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

`IMobileServiceSyncTable.PushAsync`メソッドは、特定のテーブルではなく、同期コンテキストで動作し、最後のプッシュ以降の CUD のすべての変更を送信します。

によってプルが実行される、`IMobileServiceSyncTable.PullAsync`メソッドを 1 つのテーブル。 最初のパラメーター、`PullAsync`メソッドは、モバイル デバイスでのみ使用されるクエリの名前。 Null 以外のクエリで、Azure Mobile Client SDK を実行する名前の結果を提供する、*増分同期*を毎回プル操作の結果を返す最新、`updatedAt`結果からのタイムスタンプが、ローカルに格納されています。システム テーブル。 それ以降のプル操作は、そのタイムスタンプより後、レコードをのみ取得されます。 または、*完全同期*渡すことで実現できる`null`クエリ名としてそのプル操作ごとに取得されるすべてのレコードの結果します。 次のすべての同期操作は、受信したデータはローカル ストアに挿入されます。

> [!NOTE]
> 保留中のローカル更新のあるテーブルに対してプルを実行する場合、プルは同期コンテキストに対してプッシュを実行最初。 これには、既にキューに登録の変更と Azure Mobile Apps のインスタンスから新しいデータ間の競合が最小限に抑えます。

`SyncAsync`メソッドには、Azure Mobile Apps のインスタンスと、両方のローカル ストアで同じレコードが変更されたときの競合を処理するための基本的な実装も含まれています。 競合している、ローカル ストアと、Azure Mobile Apps インスタンスの両方にデータが更新されているときに、`SyncAsync`メソッドは、Azure Mobile Apps インスタンスに格納されたデータから、ローカル ストア内のデータを更新します。 その他のすべての競合が発生したときに、`SyncAsync`メソッドは、ローカルの変更を破棄します。 これはシナリオで処理は、Azure Mobile Apps のインスタンスから削除されたデータのローカルの変更が存在します。

運用環境のアプリケーションでは、開発者がカスタムを書き込む必要があります`IMobileServiceSyncHandler`競合処理の実装はお客様のユース ケースに適しています。 詳細については、次を参照してください。[オプティミスティック同時実行競合の解決を使用して](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency)、Azure portal と[詳細は、管理されたクライアント SDK でサポートされるオフライン](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/)MSDN ブログ。

## <a name="purging-data"></a>データの削除

使用してデータのローカル ストア内のテーブルをクリアすることができます、`IMobileServiceSyncTable.PurgeAsync`メソッド。 このメソッドは、アプリケーションが必要とする古いデータを削除するなどのシナリオをサポートします。 たとえば、サンプル アプリケーションのみが表示されます`TodoItem`に完了しないインスタンス。 そのため、完了した項目では、ローカルに保存する必要がなくなります。 ローカル ストアから完了した項目の削除は、次のように実行できます。

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

呼び出し`PurgeAsync`もプッシュ操作をトリガーします。 そのため、ローカル完了としてマークされているすべての項目は、ローカル ストアから削除される前に Azure Mobile Apps インスタンスに送信されます。 ただし、Azure Mobile Apps のインスタンスとの同期を保留中の操作がある場合、消去がスローされます、`InvalidOperationException`しない限り、`force`にパラメーターが設定されている`true`します。 他の方法を調べる、`IMobileServiceSyncContext.PendingOperations`プロパティで、保留中の Azure Mobile Apps のインスタンスにプッシュされていないし、プロパティが 0 の場合のみ、消去を実行する操作の数を返します。

> [!NOTE]
> 呼び出す`PurgeAsync`で、`force`パラメーターに設定`true`保留中の変更が失われます。

## <a name="initiating-synchronization"></a>同期の開始

サンプル アプリケーションで、`SyncAsync`を通じてメソッドが呼び出される、`TodoList.OnAppearing`メソッド。

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

これは、アプリケーションは開始時に Azure Mobile Apps のインスタンスと同期しようとしていることを意味します。

使用して、データの一覧と、Windows プラットフォームを更新するプルを使用して同期を iOS と Android で開始するさらに、**同期**ユーザー インターフェイスでボタンをクリックします。 詳細については、[プルして更新](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh)を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションにオフライン同期機能を追加する方法について説明します。 オフライン同期により、データの変更や、表示、追加、モバイル アプリケーションと対話するユーザーのネットワーク接続がないときでもです。 変更は、ローカルのデータベースに格納され、デバイスがオンラインになると変更は、Azure Mobile Apps のインスタンスと同期ことができます。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuthOfflineSync (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Azure Mobile Apps の使](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps を使ったユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
