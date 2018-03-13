---
title: "フラグメントを管理します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: aa7c6eb2435be473049f0799a70a6eb37e678c78
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="managing-fragments"></a>フラグメントを管理します。

Android フラグメントの管理を支援するための提供、`FragmentManager`クラスです。 各アクティビティのインスタンスには`Android.App.FragmentManager`を検索またはそのフラグメントを動的に変更されます。 これらの変更の各セットと呼ばれる、*トランザクション*、クラスに含まれる Api の 1 つを使用して実行し、`Android.App.FragmentTransation`で管理される、`FragmentManager`です。 アクティビティは、次のようにトランザクションを開始可能性があります。

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

フラグメントにこれらの変更で実行されます、`FragmentTransaction`などのメソッドを使用して、インスタンス`Add()`、`Remove(),`と`Replace().`を使用して、変更が適用される、`Commit()`です。 トランザクションの変更はすぐに実行されません。
代わりに、できるだけ早く、アクティビティの UI スレッドで実行する予定です。

次の例では、既存のコンテナーにフラグメントを追加する方法を示します。

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

トランザクションが後にコミットされた場合`Activity.OnSaveInstanceState()`が呼び出されると、例外がスローされます。 これは、アクティビティの状態を保存、ときに Android もホストされるフラグメントの状態が保存されるために発生します。 この時点より後、フラグメントのトランザクションがコミットされた場合は、アクティビティを復元するときにこれらのトランザクションの状態が失われます。

アクティビティのフラグメントのトランザクションを保存することは[バック スタック](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html)を呼び出すことによって`FragmentTransaction.AddToBackStack()`です。 これにより、ユーザーを通して後方に移動するフラグメントの場合に変更、**戻る**ボタンが押されました。 このメソッドの呼び出し、せず削除されるフラグメントが破棄され、するがアクティビティを通じて、ユーザーが移動した場合に使用できます。

次の例を使用する方法を示しています、`AddToBackStack`のメソッド、`FragmentTransaction`バック スタックで最初のフラグメントの状態を維持しながら、1 つのフラグメントを置換します。

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```


## <a name="communicating-with-fragments"></a>フラグメントとの通信

*FragmentManager*知っているすべてのアクティビティに関連付けられているフラグメントとこれらのフラグメントを見つけやすくの 2 つのメソッドを提供します。

-   **FindFragmentById** &ndash;このメソッドは、フラグメントは、トランザクションの一部として追加されたときに、レイアウト ファイルで指定された ID またはコンテナーの ID を使用してフラグメントを検索します。

-   **FindFragmentByTag** &ndash;レイアウト ファイルで指定された、またはトランザクションに追加されたタグのあるフラグメントを検索にこのメソッドを使用します。

フラグメントとアクティビティの両方の参照、`FragmentManager`のため、同じ手法はそれらの間で前後に通信するために使用します。 アプリケーションでは、これら 2 つのメソッドのいずれかを使用してフラグメントの参照を検索、適切な型には、その参照にキャスト、およびフラグメントのメソッドを直接呼び出す可能性があります。 次のスニペットは、例を示します。

アクティビティの使用することも、`FragmentManager`フラグメントを検索します。

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>アクティビティとの通信

使用するフラグメントのことが、`Fragment.Activity`そのホストを参照するプロパティです。 具体的な種類のアクティビティをキャストすることがメソッドの呼び出しとそのホストでは、プロパティをアクティビティの次の例で示すようにできます。

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
