---
title: Azure Mobile Apps の使用
description: Azure Mobile Apps では、モバイル認証、オフライン同期、およびプッシュ通知のサポートにより、Azure App Service でホストされているスケーラブルなバックエンドでアプリを開発できます。 これは Node.js バックエンドを使用する Azure Mobile Apps に適用できるのみ、この記事では、クエリ、挿入、更新、および Azure Mobile Apps インスタンス内のテーブルに格納されたデータを削除する方法について説明します。
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: e2e9e2c05d3f6e467fd47b31af4f53049aab2709
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-an-azure-mobile-app"></a>Azure Mobile Appsの使用

_Azure Mobile Appsでは、モバイル認証、オフライン同期、およびプッシュ通知のサポートにより、Azure App Serviceでホストされているスケーラブルなバックエンドでアプリを開発できます。この記事は、Azure Mobile AppsでNode.jsバックエンドを使用する場合にのみ適用され、Azure Mobile Appsインスタンスのテーブルに格納されたデータのクエリ、挿入、更新、および削除方法について説明しています。_

Xamarin.Forms で利用できる Azure Mobile Apps インスタンスを作成する方法については、 [Xamarin.Forms アプリを作成する](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/) を参照してください。これらの手順に従うと、後に設定して、Azure Mobile Apps インスタンスを使用するダウンロード可能なサンプル アプリケーションを構成できます。 `Constants.ApplicationURL` Azure Mobile Apps インスタンスの URL にします。 次に、サンプル アプリケーションを実行すると、次のスクリーン ショットに示すように、Azure Mobile Apps インスタンスに接続します。

![](azure-images/portal.png "サンプル アプリケーション")

Azure Mobile Apps へのアクセスは、 [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) を介して行われ、Xamarin.FormsサンプルアプリケーションからAzureへのすべての接続はHTTPS経由で行われます。

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="consuming-an-azure-mobile-app-instance"></a>Azure のモバイル アプリ インスタンスの使用

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)が提供する`MobileServiceClient`クラスは、次のコード例に示すように Azure Mobile Apps インスタンスへのアクセスするために Xamarin.Forms アプリケーションによって使用されます。

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

ときに、`MobileServiceClient`インスタンスが作成されると、Azure Mobile Apps インスタンスを識別する、アプリケーションの URL を指定する必要があります。 モバイル アプリのダッシュ ボードからこの値を取得できます、 [Microsoft Azure ポータル](https://portal.azure.com/)です。

参照、`TodoItem`そのテーブルで操作を実行する前に、Azure Mobile Apps インスタンスに格納されているテーブルを取得する必要があります。 これは、呼び出すことによって実現、`GetTable`メソッドを`MobileServiceClient`インスタンスを返す、`IMobileServiceTable<TodoItem>`参照します。

### <a name="querying-data"></a>データのクエリ

テーブルの内容を呼び出すことによって取得できます、`IMobileServiceTable.ToEnumerableAsync`メソッドを非同期的にクエリを評価し、結果を返します。 データを含めることによってサーバー側をフィルター処理、`Where`クエリ内の句。 `Where`句は、次のコード例に示すように行フィルター、テーブルに対するクエリに述語を適用します。

```csharp
public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync (bool syncItems = false)
{
  ...
  IEnumerable<TodoItem> items = await todoTable
              .Where (todoItem => !todoItem.Done)
              .ToEnumerableAsync ();

  return new ObservableCollection<TodoItem> (items);
}
```

このクエリからのすべての項目を返します、`TodoItem`テーブルです`Done`プロパティと等しい`false`です。 クエリの結果は、後に配置されます、`ObservableCollection`表示用です。

### <a name="inserting-data"></a>データの挿入

Azure Mobile Apps インスタンスでデータを挿入するときに新しい列が自動的に生成する必要に応じて、テーブルでの提供、Azure Mobile Apps インスタンスでその動的スキーマが有効にします。 `IMobileServiceTable.InsertAsync`次のコード例に示すように、指定したテーブルに新しいデータの行を挿入するメソッドを使用します。

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

挿入リクエストを行うときに、ID を Azure Mobile Apps インスタンスに渡されるデータで指定されている必要があります。 挿入リクエストには、ID が含まれている場合、`MobileServiceInvalidOperationException`がスローされます。

後に、`InsertAsync`メソッドが完了すると、Azure Mobile Apps インスタンス内のデータの ID に設定されます、 `TodoItem` Xamarin.Forms アプリケーション内のインスタンス。

### <a name="updating-data"></a>データの更新

Azure Mobile Apps インスタンス内のデータを更新するときに新しい列が自動的に生成されます必要に応じて、テーブルでの提供、Azure Mobile Apps インスタンスでその動的スキーマが有効にします。 `IMobileServiceTable.UpdateAsync`次のコード例に示すように、新しい情報で既存のデータを更新するメソッドを使用します。

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

更新リクエストを行うときは、Azure Mobile Apps インスタンスは、更新するデータを識別できるように ID を指定してください。 この ID の値が格納されている、`TodoItem.ID`プロパティです。 更新のリクエストが含まれていない ID には更新するには、Azure Mobile Apps インスタンス データを決定する方法はありません、そのため、`MobileServiceInvalidOperationException`がスローされます。

### <a name="deleting-data"></a>データの削除

`IMobileServiceTable.DeleteAsync`次のコード例に示すように、Azure Mobile Apps テーブルからデータを削除するメソッドを使用します。

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

削除リクエストを行うときは、Azure Mobile Apps インスタンス が削除されるデータを識別できるように、ID を指定してください。 この ID の値が格納されているのは`TodoItem.ID`プロパティです。 削除リクエストに ID が含まれていない場合は、Azure Mobile Apps インスタンスが削除するデータを決定する方法はありませんので、`MobileServiceInvalidOperationException`がスローされます。

## <a name="summary"></a>まとめ

この記事では、Azure Mobile Apps インスタンス内のテーブルに格納されたデータをクエリ、挿入、更新、および 削除する際の[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)の使用方法を説明しました。 このSDKはAzure Mobile Apps インスタンスにアクセスするためにXamarin.Forms アプリケーションによって使用される `MobileServiceClient` クラスを提供します。


## <a name="related-links"></a>関連リンク

- [TodoAzure (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Xamarin.Forms アプリを作成します。](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
