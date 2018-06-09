---
title: Azure のモバイル アプリを使用してオフラインのデータの同期
description: この記事では、Azure Mobile Apps バックエンドを使用する Xamarin.Forms アプリケーションをオフラインの同期機能を追加する方法について説明します。
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: e8b0eeeb4f0033fccd7a61b4acb286bb8457e6c2
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243598"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Azure のモバイル アプリを使用してオフラインのデータの同期

_オフラインの同期により、ユーザーは、データの変更や、表示、追加すると、モバイル アプリケーションを操作したり、ネットワーク接続がない場合でもです。変更は、ローカル データベースに格納し、Azure Mobile Apps インスタンスと、変更を同期することができます、デバイスがオンラインとします。この記事では、オフラインの同期機能を Xamarin.Forms アプリケーションに追加する方法について説明します。_

## <a name="overview"></a>概要

[Azure Mobile クライアント SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)提供、`IMobileServiceTable`インターフェイスで、Azure Mobile Apps インスタンスに格納されているテーブルで実行できる操作を表します。 これらの操作は、Azure Mobile Apps インスタンスに直接接続し、モバイル デバイスは、ネットワーク接続を持っていない場合は失敗します。

オフラインの同期をサポートするために、Azure モバイル クライアント SDK は、サポート*テーブルを同期*、によって提供される`IMobileServiceSyncTable`インターフェイスです。 このインターフェイスには、同じ Create、Read、Update、として削除 (CRUD) 操作が用意されています、`IMobileServiceTable`からの読み取りまたはローカル ストアに書き込むが、操作します。 呼び出しがあるまで、ローカル ストアが Azure Mobile Apps インスタンスから新しいデータで設定されていない*プル*データ。 呼び出しがあるまでに、Azure Mobile Apps インスタンスに送信されていないデータの同様に、*プッシュ*ローカルの変更。

オフラインの同期には、競合の検出で、両方のローカル ストアと同じレコードが変更されたときに、Azure Mobile Apps インスタンスとカスタム競合解決のサポートも含まれています。 ローカル ストア、または Azure Mobile Apps インスタンスで、競合を処理するか、できます。

オフラインの同期に関する詳細については、次を参照してください。 [Azure Mobile Apps でのオフラインのデータ同期](/azure/app-service-mobile/app-service-mobile-offline-data-sync/)と[Xamarin.Forms モバイル アプリへのオフライン同期を有効にする](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/)です。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションと Azure Mobile Apps インスタンス間でオフラインの同期を統合するためのプロセスは次のとおりです。

1. Azure Mobile Apps インスタンスを作成します。 詳細については、次を参照してください。 [Azure Mobile App を消費して](~/xamarin-forms/data-cloud/consuming/azure.md)です。
1. 追加、 [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) Xamarin.Forms ソリューション内のすべてのプロジェクトに NuGet パッケージです。
1. (省略可能)Azure Mobile Apps インスタンスおよび Xamarin.Forms アプリケーションでの認証を有効にします。 詳細については、次を参照してください。 [Azure Mobile Apps にユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure.md)です。

次のセクションでは、Microsoft.Azure.Mobile.Client.SQLiteStore NuGet パッケージを使用してユニバーサル Windows プラットフォーム (UWP) プロジェクトを構成するための追加のセットアップ手順を提供します。 追加の設定を iOS および Android で Microsoft.Azure.Mobile.Client.SQLiteStore NuGet パッケージを使用する必要はありません。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

SQLite でユニバーサル Windows プラットフォーム (UWP) を使用するのには、次の手順を実行します。

1. インストール、[ユニバーサル Windows プラットフォームに対して SQLite](http://sqlite.org/2016/sqlite-uwp-3120200.vsix)開発環境での Visual Studio 拡張機能です。
1. UWP プロジェクトで Visual Studio で、右クリックして**参照 > 参照の追加**に移動**拡張**を追加し、**ユニバーサル Windows プラットフォームに対して SQLite**と**ユニバーサル Windows プラットフォーム アプリ用の visual C 2015 ランタイム**UWP プロジェクトの拡張機能です。

## <a name="initializing-the-local-store"></a>ローカル ストアの初期化

同期のテーブル操作を実行する前に、ローカル ストアを初期化する必要があります。 Xamarin.Forms ソリューションのポータブル クラス ライブラリ (PCL) プロジェクトでこれを実現します。

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

によって新しいローカル SQLite データベースが作成された、`MobileServiceSQLiteStore`クラスが存在しないことです。 次に、`DefineTable<T>`メソッド内のフィールドに一致するローカル ストアにテーブルを作成する、`TodoItem`が存在しないことを入力します。

A*同期コンテキスト*に関連付けられている、`MobileServiceClient`インスタンス、および同期テーブルで行われた変更を追跡します。 同期コンテキストでは、Create、Update、および削除 (CUD) の操作の順序付きリストを保持するキューを後で、Azure Mobile Apps インスタンスに送信されますを保持します。 `IMobileServiceSyncContext.InitializeAsync()`にローカル ストアを同期コンテキストに関連付けるメソッドを使用します。

`todoTable`フィールドは、 `IMobileServiceSyncTable`、ため、すべての CRUD 操作は、ローカル ストアを使用します。

## <a name="performing-synchronization"></a>同期を実行します。

ローカル ストアが Azure Mobile Apps と同期されているインスタンスの場合、`SyncAsync`メソッドが呼び出されます。

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

`IMobileServiceSyncTable.PushAsync`メソッドは、特定のテーブルではなく、同期コンテキストで動作し、最後のプッシュ以降 CUD のすべての変更を送信します。

Äs ' í プル、`IMobileServiceSyncTable.PullAsync`メソッドを 1 つのテーブルです。 最初のパラメーター、`PullAsync`メソッドは、モバイル デバイスでのみ使用されるクエリ名。 Null でないクエリを実行する Azure のモバイル クライアント SDK の名前の結果を提供する、*差分同期*場所ごとに、プル操作の結果を返す最新、`updatedAt`結果からのタイムスタンプが、ローカルに格納されています。システム テーブル。 その後にプル操作は、そのタイムスタンプの後に、レコードをのみ取得されます。 または、*完全な同期*を渡すことによって実現できます`null`クエリ名としてその結果、各プル操作で取得されているすべてのレコードです。 次のすべての同期操作は、受信したデータは、ローカル ストアに挿入されます。

> [!NOTE]
> プルが保留中のローカルの更新があるテーブルに対して実行される場合、プルは最初に実行されている同期コンテキストでプッシュを使用します。 これは、既にキューに登録の変更と、Azure Mobile Apps インスタンスからの新しいデータ間の競合を最小化します。

`SyncAsync`メソッドにも、Azure Mobile Apps インスタンスと、両方のローカル ストアで同じレコードが変更されたときに競合を処理するための基本的な実装が含まれています。 競合しているローカル ストアと、Azure Mobile Apps インスタンスの両方にデータが更新されているときに、`SyncAsync`メソッドは、Azure Mobile Apps インスタンスに格納されたデータからローカル ストア内のデータを更新します。 その他のすべての競合が発生したときに、`SyncAsync`メソッドは、ローカルの変更を破棄します。 Azure Mobile Apps インスタンスから削除されているデータのローカルの変更が存在するシナリオが処理されます。

実稼働アプリケーションでは、開発者がカスタムを記述する必要があります`IMobileServiceSyncHandler`ユース ケースに適しています競合処理の実装です。 詳細については、次を参照してください。[オプティミスティック同時実行の競合の解決を使用して](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency)、Azure ポータルでと[深いがマネージ クライアント SDK でオフライン サポートの詳細](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/)MSDN ブログ。

## <a name="purging-data"></a>データの削除

ローカル ストア内のテーブルをクリアするにを使用してデータの`IMobileServiceSyncTable.PurgeAsync`メソッドです。 このメソッドは、アプリケーションが不要になったが必要な古いデータを削除するなどのシナリオをサポートします。 たとえば、サンプル アプリケーションのみが表示されます`TodoItem`インスタンスが完了していません。 そのため、完了した項目では、ローカルに保存する必要がなくなります。 ローカル ストアから完了したアイテムの削除は、次のように実行できます。

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

呼び出し`PurgeAsync`もプッシュ操作をトリガーします。 そのため、ローカルで完了としてマークされているすべての項目は、ローカル ストアから削除される前に Azure Mobile Apps インスタンスに送信されます。 ただし、Azure Mobile Apps インスタンスとの同期を保留中の操作がある場合、消去がスローされます、`InvalidOperationException`しない限り、`force`にパラメーターが設定されている`true`です。 他の方法は、調べること、`IMobileServiceSyncContext.PendingOperations`プロパティで、保留中の Azure Mobile Apps インスタンスにプッシュされていないし、プロパティが 0 の場合にのみ、消去を実行する操作の数を返します。

> [!NOTE]
> 呼び出す`PurgeAsync`で、`force`パラメーターに設定`true`保留中の変更は失われます。

## <a name="initiating-synchronization"></a>同期を開始しています

サンプル アプリケーションで、`SyncAsync`を通じてメソッドが呼び出された、`TodoList.OnAppearing`メソッド。

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

これは、アプリケーションは開始するときに、Azure Mobile Apps インスタンスと同期しようとしていることを意味します。

さらに、同期を開始する iOS および Android でのデータの一覧と Windows プラットフォームを使用して更新するプルを使用して、**同期**ユーザー インターフェイスでボタンをクリックします。 詳細については、次を参照してください。[更新するプル](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh)です。

## <a name="summary"></a>まとめ

この記事では、オフラインの同期機能を Xamarin.Forms アプリケーションに追加する方法について説明します。 オフラインの同期により、ユーザーは、データの変更や、表示、追加すると、モバイル アプリケーションを操作したり、ネットワーク接続がない場合でもです。 変更は、ローカル データベースに格納し、Azure Mobile Apps インスタンスと、変更を同期することができます、デバイスがオンラインとします。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuthOfflineSync (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Azure のモバイル アプリの使用](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure のモバイル アプリを使用してユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure のモバイル クライアント SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
