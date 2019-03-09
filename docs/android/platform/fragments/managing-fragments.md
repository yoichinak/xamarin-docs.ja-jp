---
title: フラグメントの管理
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 3e0430b8ed9c42030441021e71c3b08b1ddccc57
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670546"
---
# <a name="managing-fragments"></a>フラグメントの管理

Android フラグメントの管理を支援するための提供、`FragmentManager`クラス。 各アクティビティのインスタンスが実行されて`Android.App.FragmentManager`を検索またはそのフラグメントを動的に変更されます。 これらの変更の各セットと呼ばれる、*トランザクション*、いずれかのクラスに含まれている Api を使用して実行し、`Android.App.FragmentTransation`で管理される、 `FragmentManager`。 アクティビティは、このようなトランザクションを開始可能性があります。

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

フラグメントにこれらの変更が実行される、`FragmentTransaction`などのメソッドを使用して、インスタンス`Add()`、`Remove(),`と`Replace().`を使用して、変更が適用される、 `Commit()`。 すぐに、トランザクションでの変更は行われません。
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

場合は、トランザクションのコミット後`Activity.OnSaveInstanceState()`が呼び出されると、例外がスローされます。 これは、アクティビティの状態を保存、ときに Android もホストされているフラグメントの状態が保存されるために発生します。 この時点より後のフラグメントのトランザクションがコミットされた場合は、アクティビティが復元されるときにこれらのトランザクションの状態が失われます。

アクティビティのフラグメントのトランザクションを保存することは[戻るスタック](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html)を呼び出すことによって`FragmentTransaction.AddToBackStack()`します。 これにより、ユーザーによる下位に移動するフラグメントの場合に変更、**戻る**ボタンが押されました。 このメソッドを呼び出し、削除されたフラグメントが破棄されは、ユーザーがアクティビティでもう一度移動する場合は使用できません。

次の例は、使用する方法を示します、`AddToBackStack`のメソッド、`FragmentTransaction`戻るスタックの最初のフラグメントの状態を維持しながら、1 つのフラグメントを置換します。

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

*FragmentManager*に関するすべてのアクティビティに関連付けられているフラグメントを知っているし、これらのフラグメントを検索する 2 つのメソッドを提供します。

-   **FindFragmentById** &ndash;このメソッドは、フラグメントは、トランザクションの一部として追加されたときに、レイアウト ファイルで指定された ID またはコンテナーの ID を使用してフラグメントを検索します。

-   **FindFragmentByTag** &ndash;レイアウト ファイルで指定された、またはトランザクションに追加されたタグを持つフラグメントを検索するこのメソッドが使用されます。

フラグメントとアクティビティの両方の参照、`FragmentManager`同じ手法がそれらの間を行き来通信に使用されるため、します。 アプリケーションでは、フラグメントの参照を検索するには、これら 2 つのメソッドのいずれかを使用して、適切な型には、その参照をキャスト、およびがフラグメント上のメソッドを直接呼び出す可能性があります。 次のスニペットでは、例を示します。

アクティビティを使用することも、`FragmentManager`フラグメントを検索します。

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>アクティビティとの通信

フラグメントを使用することは、`Fragment.Activity`そのホストを参照するプロパティ。 アクティビティをより具体的な型をキャストすることによってことがメソッドの呼び出しとそのホストでは、プロパティをアクティビティの次の例に示すようにできます。

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
