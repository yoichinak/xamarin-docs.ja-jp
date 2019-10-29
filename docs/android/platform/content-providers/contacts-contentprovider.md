---
title: 連絡先の ContentProvider の使用
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: fca57b7af34ae2b28dda9bf20a95183138cbc641
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020545"
---
# <a name="using-the-contacts-contentprovider"></a>連絡先の ContentProvider の使用

`ContentProvider` によって公開されるアクセスデータを使用するコードは、`ContentProvider` クラスへの参照をまったく必要としません。 代わりに、Uri を使用して、`ContentProvider`によって公開されるデータの上にカーソルを作成します。 Android では、Uri を使用して、その識別子を持つ `ContentProvider` を公開するアプリケーションのシステムを検索します。 Uri は文字列であり、通常は `com.android.contacts/data`などの逆引き DNS 形式です。

Android の*連絡先*プロバイダーは、この文字列を覚えているのではなく、`android.provider.ContactsContract` クラスでそのメタデータを公開します。 このクラスを使用して、`ContentProvider` の Uri のほか、クエリを実行できるテーブルと列の名前を決定します。

データ型によっては、にアクセスするための特別なアクセス許可も必要になります。 組み込み連絡先リストには、 **Androidmanifest .xml**ファイルの `android.permission.READ_CONTACTS` アクセス許可が必要です。

Uri からカーソルを作成するには、次の3つの方法があります。

1. **Managedquery ()** &ndash;、Android 2.3 (API レベル 10) 以前では、`ManagedQuery` はカーソルを返します。また、データの更新やカーソルの終了を自動的に管理します。 このメソッドは、Android 3.0 (API レベル 11) では非推奨とされます。

1. **Contentresolver. Query ()** &ndash; はアンマネージカーソルを返します。つまり、コード内で明示的に更新して閉じる必要があります。

1. **カーソルローダー ()。LoadInBackground ()** &ndash; Android 3.0 (API レベル 11) で導入されました。 `CursorLoader` は、`ContentProvider` を使用するための推奨される方法です。 `CursorLoader` は、UI がブロックされないように、バックグラウンドスレッドで `ContentResolver` を照会します。
   このクラスには、旧バージョンの Android の v4 互換性ライブラリを使用してアクセスできます。

これらの各メソッドには、同じ基本的な入力セットがあります。

- **Uri** &ndash; `ContentProvider` の完全修飾名です。
- **予測**&ndash;、カーソルに対して選択する列を指定します。
- **選択**&ndash; SQL `WHERE` 句に似ています。
- **Selectionargs** &ndash; 選択範囲内で置換されるパラメーターです。
- 並べ替えの基準となる列 &ndash;**順序**を並べ替えます。

## <a name="creating-inputs-for-a-query"></a>クエリの入力の作成

`ContactsProvider` サンプルコードは、Android の組み込み連絡先プロバイダーに対して非常に単純なクエリを実行します。 実際の Uri または列名を知る必要はありません。連絡先 `ContentProvider` のクエリに必要なすべての情報は、`ContactsContract` クラスによって公開されている定数として利用できます。

カーソルの取得に使用されるメソッドに関係なく、これらの同じオブジェクトは、 *ContactsProvider/ContactsAdapter*ファイルに示されているパラメーターとして使用されます。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

この例では、`selection`、`selectionArgs` および `sortOrder` は `null`に設定することによって無視されます。

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>コンテンツプロバイダーの Uri からのカーソルの作成

パラメーターオブジェクトを作成したら、次の3つの方法のいずれかで使用できます。

### <a name="using-a-managed-query"></a>マネージクエリの使用

Android 2.3 (API レベル 10) 以前を対象とするアプリケーションでは、次の方法を使用する必要があります。

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

このカーソルは Android によって管理されるため、閉じる必要はありません。

### <a name="using-contentresolver"></a>ContentResolver の使用

`ContentProvider` に対してカーソルを取得するために `ContentResolver` に直接アクセスするには、次のようにします。

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

このカーソルはアンマネージドであるため、不要になったときに閉じる必要があります。
コードが開いているカーソルを閉じるようにします。そうしないと、エラーが発生します。

```csharp
cursor.Close();
```

または、`StartManagingCursor()` を呼び出して、カーソルを ' 管理 ' `StopManagingCursor()` できます。 マネージカーソルは、アクティビティが停止して再起動されたときに、自動的に非アクティブになり、再度クエリが行われます。

### <a name="using-cursorloader"></a>カーソルローダーの使用

Android 3.0 (API レベル 11) 以降用にビルドされたアプリケーションでは、次の方法を使用する必要があります。

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` は、すべてのカーソル操作がバックグラウンドスレッドで実行されることを保証します。また、アクティビティが再起動されたとき (構成の変更などにより)、アクティビティインスタンス間で既存のカーソルをインテリジェントに再利用し、データを再度読み込むことができます。

以前のバージョンの Android では、 [v4 サポートライブラリ](https://developer.android.com/tools/support-library/index.html)を使用して `CursorLoader` クラスを使用することもできます。

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>カスタムアダプターを使用したカーソルデータの表示

連絡先の画像を表示するには、カスタムアダプターを使用します。これにより、イメージファイルパスへの `PhotoId` 参照を手動で解決できるようになります。

カスタムアダプターを使用してデータを表示する例では、`CursorLoader` を使用して、 **ContactsProvider/ContactsAdapter**から**fillcontacts**メソッドのローカルコレクションにすべての連絡先データを取得します。

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

次に、`contactList` コレクションを使用して、BaseAdapter のメソッドを実装します。 アダプターは、他のコレクション &ndash; と同様に実装されます。データは `ContentProvider`を基にしているため、ここでは特別な処理は行われません。

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

[ListView で連絡先を表示するアプリのスクリーンショットを![します。1つのエントリの左側に画像が表示されます。](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

同様のコードパターンを使用すると、アプリケーションは、ユーザーの写真、ビデオ、音楽などのさまざまなシステムデータにアクセスできます。
一部のデータ型では、プロジェクトの**Androidmanifest .xml**で特別なアクセス許可を要求する必要があります。

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter を使用してカーソルデータを表示する

カーソルは、`SimpleCursorAdapter` と共に表示することもできます (ただし、写真ではなく、名前のみが表示されます)。 このコードは、`SimpleCursorAdapter` で `ContentProvider` を使用する方法を示しています (このコードはサンプルには記載されていません)。

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

`SimpleCursorAdapter`の実装の詳細については、 [Listviews とアダプター](~/android/user-interface/layouts/list-view/index.md)に関する説明を参照してください。

## <a name="related-links"></a>関連リンク

- [ContactsAdapter Demo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
