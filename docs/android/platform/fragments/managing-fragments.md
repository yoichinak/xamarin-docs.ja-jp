---
title: フラグメントの管理
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: f5baf8b46571c9b528fcc666a3f1f4530f18ee07
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761502"
---
# <a name="managing-fragments"></a>フラグメントの管理

フラグメントの管理を支援するために、 `FragmentManager` Android にはクラスが用意されています。 各アクティビティには、その`Android.App.FragmentManager`フラグメントを検索または動的に変更するのインスタンスがあります。 これらの変更の各セットは*トランザクション*と呼ばれ、によって管理`Android.App.FragmentTransation` `FragmentManager`されるクラスに含まれる api の1つを使用して実行されます。 アクティビティは、次のようなトランザクションを開始できます。

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

これらのフラグメントへの`FragmentTransaction`変更は`Add()`、 `Remove(),` `Replace().`などのメソッドを使用してインスタンスで実行され、変更はを`Commit()`使用して適用されます。 トランザクションの変更はすぐには実行されません。
代わりに、アクティビティの UI スレッドでできるだけ早く実行するようにスケジュールされています。

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

が呼び出された後`Activity.OnSaveInstanceState()`にトランザクションがコミットされると、例外がスローされます。 これは、アクティビティによって状態が保存されるときに、Android によって、ホストされているフラグメントの状態も保存されるためです。 この時点より後にフラグメントトランザクションがコミットされた場合、アクティビティが復元されると、これらのトランザクションの状態は失われます。

を`FragmentTransaction.AddToBackStack()`呼び出すことによって、アクティビティの[バックスタック](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html)にフラグメントトランザクションを保存することができます。 これにより、 **[戻る]** ボタンが押されたときに、ユーザーがフラグメントの変更を前後に移動できるようになります。 このメソッドを呼び出さないと、削除されたフラグメントは破棄され、ユーザーがアクティビティをたどって戻ると使用できなくなります。

次の例では、 `AddToBackStack` `FragmentTransaction`のメソッドを使用して1つのフラグメントを置換し、バックスタックの最初のフラグメントの状態を維持する方法を示します。

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

*Fragmentmanager*は、アクティビティにアタッチされているすべてのフラグメントを認識し、これらのフラグメントを見つけるために2つのメソッドを提供します。

- **FindFragmentById**&ndash;このメソッドは、フラグメントがトランザクションの一部として追加されたときに、レイアウトファイルで指定された id またはコンテナー id を使用して検出されます。

- **Findfragmentbytag**&ndash;このメソッドは、レイアウトファイルに指定された、またはトランザクションで追加されたタグを持つフラグメントを検索するために使用されます。

フラグメントとアクティビティはどちらも`FragmentManager`を参照するため、同じ手法を使用してそれらの間で通信を行います。 アプリケーションでは、次の2つのメソッドのいずれかを使用して参照フラグメントを検出し、その参照を適切な型にキャストして、フラグメントのメソッドを直接呼び出すことができます。 次のスニペットに例を示します。

アクティビティでを使用`FragmentManager`してフラグメントを検索することもできます。

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>アクティビティとの通信

フラグメントは、 `Fragment.Activity`プロパティを使用してそのホストを参照することができます。 アクティビティをより具体的な型にキャストすることにより、アクティビティは、次の例に示すように、そのホスト上のメソッドとプロパティを呼び出すことができます。

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
