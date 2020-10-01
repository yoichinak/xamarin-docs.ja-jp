---
title: 連絡先の ContentProvider の使用
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: 8c3608837132e221bec12b2c00fb2dd0b1730e76
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454079"
---
# <a name="using-the-contacts-contentprovider"></a>連絡先の ContentProvider の使用

`ContentProvider` によって公開されるアクセス データを使用するコードでは、`ContentProvider` クラスへの参照はまったく必要ありません。 代わりに、URI を使用して、`ContentProvider` によって公開されるデータに対するカーソルを作成します。 Android では、URI を使用して、その識別子で `ContentProvider` を公開しているアプリケーションがシステムから検索されます。 URI は文字列であり、通常は `com.android.contacts/data` などの逆引き DNS 形式です。

開発者がこの文字列を記憶するのではなく、Android の "*連絡先*" プロバイダーによって `android.provider.ContactsContract` クラスでそのメタデータが公開されます。 このクラスを使用して、`ContentProvider` の URI と、クエリを実行できるテーブルと列の名前が決定されます。

一部のデータ型については、アクセスするために特別なアクセス許可も必要になります。 組み込みの連絡先リストを使用するには、**AndroidManifest.xml** ファイルに `android.permission.READ_CONTACTS` アクセス許可が必要です。

URI からカーソルを作成する方法は、3 つあります。

1. **ManagedQuery()** &ndash; Android 2.3 (API レベル 10) 以前で推奨される方法であり、`ManagedQuery` によってカーソルが返され、データの更新やカーソルのクローズも自動的に管理されます。 このメソッドは、Android 3.0 (API レベル 11) で非推奨になっています。

1. **ContentResolver.Query()** &ndash; アンマネージド カーソルが返されます。これは、カーソルの更新やクローズをコード内で明示的に行う必要があることを意味します。

1. **CursorLoader().LoadInBackground()** &ndash; Android 3.0 (API レベル 11) で導入された方法であり、現在では `CursorLoader` が `ContentProvider` を使用するための推奨される方法です。 `CursorLoader` による `ContentResolver` のクエリは、UI がブロックされないように、バックグラウンド スレッドで実行されます。
   このクラスには、v4 互換性ライブラリを使用する旧バージョンの Android でアクセスできます。

これらの各メソッドには、同じ基本的な入力セットがあります。

- **Uri** &ndash; `ContentProvider` の完全修飾名。
- **Projection** &ndash; カーソルに対して選択する列の指定。
- **Selection** &ndash; SQL の `WHERE` 句に相当。
- **SelectionArgs** &ndash; Selection で置き換えるパラメーター。
- **SortOrder** &ndash; 並べ替えに使用する列。

## <a name="creating-inputs-for-a-query"></a>クエリの入力の作成

`ContactsProvider` サンプルのコードでは、Android の組み込み連絡先プロバイダーに対して、非常に簡単なクエリが実行されます。 実際の URI または列名を知る必要はありません。連絡先 `ContentProvider` のクエリに必要なすべての情報は、`ContactsContract` クラスによって公開されている定数として利用できます。

カーソルを取得するために使用するメソッドに関係なく、*ContactsProvider/ContactsAdapter.cs* ファイルで示されているように、同じオブジェクトがパラメーターとして使用されます。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

この例では、`selection`、`selectionArgs`、`sortOrder` は、`null` に設定することによって無視されます。

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>コンテンツ プロバイダーの URI からのカーソルの作成

パラメーター オブジェクトが作成されたら、次の 3 つの方法のいずれかで使用できます。

### <a name="using-a-managed-query"></a>マネージド クエリの使用

Android 2.3 (API レベル 10) 以前を対象とするアプリケーションでは、次のメソッドを使用する必要があります。

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

このカーソルは Android によって管理されるため、閉じる必要はありません。

### <a name="using-contentresolver"></a>ContentResolver の使用

`ContentResolver` に直接アクセスして `ContentProvider` に対するカーソルを取得するには、次のようにします。

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

このカーソルはアンマネージドであるため、不要になったら閉じる必要があります。
開かれているカーソルを必ずコードで閉じます。そうしないと、エラーが発生します。

```csharp
cursor.Close();
```

または、`StartManagingCursor()` と `StopManagingCursor()` を呼び出して、カーソルを "管理" することもできます。 マネージド カーソルは、アクティビティが停止されると自動的に非アクティブになり、アクティビティが再度開始されると再度クエリが実行されます。

### <a name="using-cursorloader"></a>CursorLoader の使用

Android 3.0 (API レベル 11) 以降用に構築されたアプリケーションでは、次のメソッドを使用する必要があります。

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` では、すべてのカーソル操作がバックグラウンド スレッドで行われることが保証され、アクティビティが再度開始されたときは (構成の変更などにより)、データを再度読み込むのではなく、アクティビティ インスタンス間で既存のカーソルをインテリジェントに再利用できます。

以前のバージョンの Android でも、[v4 サポート ライブラリ](https://developer.android.com/tools/support-library/index.html)を使用することで、`CursorLoader` クラスを使用できます。

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>カスタム アダプターでのカーソル データの表示

連絡先の画像を表示するには、カスタム アダプターを使用します。これにより、イメージ ファイル パスへの `PhotoId` 参照を手動で解決できるようになります。

カスタム アダプターを使用してデータを表示するため、例では、`CursorLoader` を使用して、**ContactsProvider/ContactsAdapter.cs** の **FillContacts** メソッドでローカル コレクションにすべての連絡先データを取得します。

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

次に、`contactList` コレクションを使用して、BaseAdapter のメソッドを実装します。 アダプターは、他のコレクションと同じように実装されます。データは `ContentProvider` から提供されるため、ここでは特別な処理は行われません。

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

画像が存在する場合は、画像ファイルへの URI を使用してデバイスに画像が表示されます。 アプリケーションは次のようになります。

[![ListView に連絡先を表示するアプリのスクリーンショット、1 つのエントリの左側に画像が表示されている](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

同様のコード パターンをアプリケーションで使用して、ユーザーの写真、ビデオ、音楽などのさまざまなシステム データにアクセスできます。
一部のデータ型については、プロジェクトの **AndroidManifest.xml** で特別なアクセス許可を要求する必要があります。

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter でのカーソル データの表示

カーソルは、`SimpleCursorAdapter` で表示することもできます (ただし、名前のみが表示され、写真は表示されません)。 次のコードでは、`SimpleCursorAdapter` で `ContentProvider` を使用する方法を示します (このコードはサンプルにはありません)。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

`SimpleCursorAdapter` の実装について詳しくは、[ListView とアダプター](~/android/user-interface/layouts/list-view/index.md)に関する記事をご覧ください。

## <a name="related-links"></a>関連リンク

- [ContactsAdapter Demo (サンプル)](/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)