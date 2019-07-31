---
title: 連絡先の ContentProvider の使用
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: e83b9a594bad5ee3d29800988eb94812600da8a6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643705"
---
# <a name="using-the-contacts-contentprovider"></a>連絡先の ContentProvider の使用

によって`ContentProvider`公開されるアクセスデータを使用するコードは、 `ContentProvider`クラスへの参照をまったく必要としません。 代わりに、Uri を使用して、 `ContentProvider`によって公開されるデータの上にカーソルを作成します。 Android では、Uri を使用して、その識別子を使用`ContentProvider`してを公開するアプリケーションのシステムを検索します。 Uri は文字列で`com.android.contacts/data`あり、通常はのように逆引き DNS 形式になります。

開発者がこの文字列を記憶するのではなく、Android の*連絡先*プロバイダー `android.provider.ContactsContract`はクラスでそのメタデータを公開します。 このクラスは、 `ContentProvider`の Uri と、クエリ可能なテーブルと列の名前を決定するために使用されます。

データ型によっては、にアクセスするための特別なアクセス許可も必要になります。 組み込み連絡先リストには、 `android.permission.READ_CONTACTS` **androidmanifest .xml**ファイルのアクセス許可が必要です。

Uri からカーソルを作成するには、次の3つの方法があります。

1. **Managedquery ()** Android 2.3 (API レベル 10) 以前では、は`ManagedQuery`カーソルを返し、データの更新とカーソルの終了も自動的に管理します。 &ndash; このメソッドは、Android 3.0 (API レベル 11) では非推奨とされます。

1. **Contentresolver。 Query ()** &ndash;アンマネージカーソルを返します。これは、コード内で明示的に更新して閉じる必要があることを意味します。

1. **カーソルローダー ()。** &ndash; Android 3.0 (API レベル`CursorLoader` 11) で導入された loadinbackground () は、を`ContentProvider`使用するために推奨される方法です。 `CursorLoader`バックグラウンドスレッド`ContentResolver`でを照会して、UI がブロックされないようにします。
   このクラスには、旧バージョンの Android の v4 互換性ライブラリを使用してアクセスできます。


これらの各メソッドには、同じ基本的な入力セットがあります。

-  **Uri**の完全修飾名。 `ContentProvider` &ndash;
-  **プロジェクション**&ndash;カーソルに対して選択する列を指定します。
-  **選択**&ndash; SQL`WHERE`句に似ています。
-  **Selectionargs**&ndash;選択範囲内で置き換えられるパラメーター。
-  **順序の順序**&ndash;並べ替えの基準となる列。



## <a name="creating-inputs-for-a-query"></a>クエリの入力の作成

この`ContactsProvider`サンプルコードでは、Android の組み込み連絡先プロバイダーに対して非常に単純なクエリを実行します。 実際の Uri または列名を知る必要はありません。連絡先`ContentProvider`のクエリに必要なすべての情報は、 `ContactsContract`クラスによって公開されている定数として利用できます。

カーソルの取得に使用されるメソッドに関係なく、これらの同じオブジェクトは、 *ContactsProvider/ContactsAdapter*ファイルに示されているパラメーターとして使用されます。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

この例`selection` `sortOrder`では、、、 `null`およびは、をに設定することによって無視されます。`selectionArgs`



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>コンテンツプロバイダーの Uri からのカーソルの作成

パラメーターオブジェクトを作成したら、次の3つの方法のいずれかで使用できます。



### <a name="using-a-managed-query"></a>マネージクエリの使用

Android 2.3 (API レベル 10) 以前を対象とするアプリケーションでは、次の方法を使用する必要があります。

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

このカーソルは Android によって管理されるため、閉じる必要はありません。



### <a name="using-contentresolver"></a>ContentResolver の使用

に`ContentResolver`対してカーソルを取得するため`ContentProvider`に直接にアクセスするには、次のようにします。

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

このカーソルはアンマネージドであるため、不要になったときに閉じる必要があります。
コードが開いているカーソルを閉じるようにします。そうしないと、エラーが発生します。

```csharp
cursor.Close();
```

または、を呼び出し`StartManagingCursor()`て`StopManagingCursor()` 、カーソルを "管理" できます。 マネージカーソルは、アクティビティが停止して再起動されたときに、自動的に非アクティブになり、再度クエリが行われます。



### <a name="using-cursorloader"></a>カーソルローダーの使用

Android 3.0 (API レベル 11) 以降用にビルドされたアプリケーションでは、次の方法を使用する必要があります。

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

は`CursorLoader` 、すべてのカーソル操作がバックグラウンドスレッドで実行されることを保証します。また、アクティビティが再起動されたとき (構成の変更などによって)、アクティビティインスタンス間で既存のカーソルをインテリジェントに再利用し、データを再度読み込むことができます。

以前のバージョンの`CursorLoader` Android では、 [v4 サポートライブラリ](https://developer.android.com/tools/support-library/index.html)を使用してクラスを使用することもできます。



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>カスタムアダプターを使用したカーソルデータの表示

連絡先の画像を表示するには、カスタムアダプターを使用します。これにより`PhotoId` 、イメージファイルパスへの参照を手動で解決できるようになります。

カスタムアダプターを使用してデータを表示する例で`CursorLoader`は、を使用して、 **ContactsProvider/ContactsAdapter**から**fillcontacts**メソッドのローカルコレクションにすべての連絡先データを取得します。

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

次に、 `contactList`コレクションを使用して baseadapter のメソッドを実装します。 アダプターは、他のすべてのコレクション&ndash;と同様に実装され`ContentProvider`ます。データは次のようなものであるため、ここでは特別な処理はありません。

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

イメージが存在する場合は、デバイス上のイメージファイルの Uri を使用して表示されます。 アプリケーションは次のようになります。

[![ListView で連絡先を表示するアプリのスクリーンショット1つのエントリの左側に画像が表示されます。](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

同様のコードパターンを使用すると、アプリケーションは、ユーザーの写真、ビデオ、音楽などのさまざまなシステムデータにアクセスできます。
一部のデータ型では、プロジェクトの**Androidmanifest .xml**で特別なアクセス許可を要求する必要があります。



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter を使用してカーソルデータを表示する

カーソルは、 `SimpleCursorAdapter`と共に表示することもできます (ただし、名前のみが表示され、写真は表示されません)。 このコードは、 `ContentProvider`をと共`SimpleCursorAdapter`に使用する方法を示しています (このコードはサンプルには記載されていません)。

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

の実装`SimpleCursorAdapter`の詳細については、 [listviews とアダプター](~/android/user-interface/layouts/list-view/index.md)に関する説明を参照してください。


## <a name="related-links"></a>関連リンク

- [ContactsAdapter Demo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
