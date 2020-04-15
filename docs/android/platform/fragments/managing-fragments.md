---
title: フラグメントの管理
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3733ed37abe9604e77529db4864f601d2b473280
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027316"
---
# <a name="managing-fragments"></a>フラグメントの管理

フラグメントの管理を支援するため、Android では `FragmentManager` クラスが提供されています。 各アクティビティにある `Android.App.FragmentManager` のインスタンスでは、フラグメントが検索されたり動的に変更されたりします。 これらの変更の各セットは "*トランザクション*" と呼ばれ、`FragmentManager` によって管理される `Android.App.FragmentTransation` クラスに含まれる API のいずれかを使用して実行されます。 アクティビティでは、次のようにしてトランザクションを開始できます。

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

フラグメントに対するこれらの変更は、`Add()`、`Remove(),`、`Replace().` などのメソッドを使用して、`FragmentTransaction` のインスタンスで実行されます。その後、`Commit()` を使用して変更が適用されます。 トランザクションでの変更はすぐには実行されません。
代わりに、アクティビティの UI スレッドでできるだけ早く実行されるようにスケジュールを設定されます。

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

`Activity.OnSaveInstanceState()` が呼び出された後でトランザクションがコミットされると、例外がスローされます。 これは、アクティビティによって状態が保存されるときに、Android でもホストされているフラグメントの状態が保存されるために発生します。 この時点より後にフラグメント トランザクションがコミットされた場合、アクティビティが復元されるときに、これらのトランザクションの状態は失われます。

`FragmentTransaction.AddToBackStack()` を呼び出すことによって、アクティビティの[バック スタック](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html)にフラグメント トランザクションを保存できます。 これにより、ユーザーは **[戻る]** ボタンを押して、フラグメントの変更を後方に移動できるようになります。 このメソッドを呼び出さないと、削除されたフラグメントは破棄され、ユーザーがアクティビティを後方に移動しても使用できなくなります。

次の例では、`FragmentTransaction` の `AddToBackStack` メソッドを使用して 1 つのフラグメントを置き換えながら、バック スタックの最初のフラグメントの状態を維持する方法を示します。

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

## <a name="communicating-with-fragments"></a>フラグメントと通信する

*FragmentManager* では、アクティビティにアタッチされているすべてのフラグメントが把握されており、これらのフラグメントを検索するための 2 つのメソッドが用意されています。

- **FindFragmentById** &ndash; このメソッドでは、レイアウト ファイルで指定されている ID、またはフラグメントがトランザクションの一部として追加されたときのコンテナー ID を使用して、フラグメントが検索されます。

- **FindFragmentByTag** &ndash; このメソッドは、レイアウト ファイルで指定されているタグ、またはトランザクションで追加されたタグを持っているフラグメントを検索するために使用されます。

フラグメントとアクティビティはどちらも `FragmentManager` を参照しているため、同じ手法を使用してそれらの間で通信が行われます。 アプリケーションでは、これら 2 つのメソッドのいずれかを使用して参照フラグメントを検出し、その参照を適切な型にキャストしてから、フラグメントでメソッドを直接呼び出すことができます。 次にスニペットの例を示します。

また、アクティビティで `FragmentManager` を使用してフラグメントを見つけることもできます。

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>アクティビティと通信する

フラグメントでは、`Fragment.Activity` プロパティを使用してそのホストを参照することができます。 アクティビティをより具体的な型にキャストすることにより、アクティビティでは、次の例に示すように、そのホストでメソッドとプロパティを呼び出すことができます。

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
