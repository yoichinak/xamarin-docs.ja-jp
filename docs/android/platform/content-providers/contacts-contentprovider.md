---
title: 連絡先 ContentProvider を使用します。
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 754b81cec7f1adbe5c7ff1c820260e162e226b15
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="using-the-contacts-contentprovider"></a>連絡先 ContentProvider を使用します。

コードの使用によって公開されるデータにアクセスする、`ContentProvider`への参照を必要としない、`ContentProvider`すべてのクラスです。 によって公開されるデータ上でカーソルを作成する Uri を使用する代わりに、`ContentProvider`です。 Android では、Uri を使用してアプリケーションを公開するシステムの検索、`ContentProvider`その識別子を使用します。 Uri は、通常は、逆引き DNS 形式で、文字列など`com.android.contacts/data`です。

開発者がこの文字列は、Android の保存を行うのではなく*連絡先*プロバイダーでは、そのメタデータを公開する、`android.provider.ContactsContract`クラスです。 このクラスを使用して決定の Uri を`ContentProvider`だけでなく、テーブルやクエリできる列の名前。

一部のデータ型にアクセスする特殊なアクセス許可も必要です。 組み込みの連絡先リストが必要です、`android.permission.READ_CONTACTS`でアクセス許可、 **AndroidManifest.xml**ファイル。

これには、Uri からカーソルを作成する 3 つの方法があります。

1. **ManagedQuery()** &ndash; Android 2.3 (API レベル 10) と、前の推奨されるアプローチ、`ManagedQuery`カーソルを返し、データを更新して、カーソルを閉じるも自動的に管理します。 このメソッドは、Android 3.0 (API レベル 11) では使用されなくなりました。

1. **ContentResolver.Query()** &ndash;アンマネージ カーソルは、更新、コードで明示的に閉じる必要がありますが返されます。

1. **CursorLoader() です。LoadInBackground()** &ndash; Android 3.0 (API レベル 11) で導入された`CursorLoader`を使用することをお勧め、`ContentProvider`です。 `CursorLoader` クエリ、`ContentResolver`バック グラウンドのスレッド UI がブロックされないようにします。
   このクラスは、古いバージョンの Android ライブラリを使用して、v4 互換性でアクセスできます。


これらの各メソッドは、入力の同じ基本的なセットがあります。

-  **Uri** &ndash;の完全修飾名、`ContentProvider`です。
-  **プロジェクション**&ndash;のカーソルを選択する列を指定します。
-  **選択範囲**&ndash;と同様に、SQL`WHERE`句。
-  **SelectionArgs** &ndash;選択範囲内に置き換えられるパラメーター。
-  **SortOrder** &ndash;列で並べ替えを行う。



## <a name="creating-inputs-for-a-query"></a>クエリの入力の作成

`ContactsProvider`サンプル コードは、Android の組み込みの連絡先プロバイダーに対して非常に単純なクエリを実行します。 実際 Uri または列の名前の連絡先をクエリに必要なすべての情報を知る必要はありません`ContentProvider`によって公開されている定数として使用できますが、`ContactsContract`クラスです。

示すようには、これらの同じオブジェクトをパラメーターとして使用するよう、カーソルを取得する方法が使用に関係なく、 *ContactsProvider/ContactsAdapter.cs*ファイル。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

この例で、 `selection`、`selectionArgs`と`sortOrder`設定することにより無視されます`null`です。



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>コンテンツ プロバイダーの Uri からカーソルを作成します。

パラメーター オブジェクトが作成された後は、次の 3 つの方法のいずれかで使用できます。



### <a name="using-a-managed-query"></a>管理対象のクエリを使用します。

Android 2.3 (API レベル 10) を対象とするアプリケーション以前、このメソッドを使用する必要があります。

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

このカーソルを閉じるには必要はありませんので、Android によって管理されます。



### <a name="using-contentresolver"></a>ContentResolver を使用します。

アクセス`ContentResolver`に対してカーソルを取得するには、直接、`ContentProvider`できる次のように行われます。

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

このカーソルは、管理されている不要になったときに終了してください。
コードが開いているカーソルを閉じると、それ以外の場合、エラーが発生ことを確認します。

```csharp
cursor.Close();
```

代わりに、呼び出すことができます`StartManagingCursor()`と`StopManagingCursor()`カーソルを管理します。 管理対象のカーソルは自動的に非アクティブ化され、アクティビティを停止して再起動するときに再度クエリを実行します。



### <a name="using-cursorloader"></a>CursorLoader を使用します。

Android 3.0 (API レベル 11) 用にビルドされたアプリケーションまたは新しいはこのメソッドを使用する必要があります。

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader`ですべてのカーソル操作がバック グラウンド スレッドで実行し、インテリジェントに再利用できます既存カーソル アクティビティのインスタンス間でアクティビティが再開されると (例: 構成の変更) によってではなく、データをもう一度再読み込みすることです。

以前の Android バージョンを使用できますも、`CursorLoader`クラスを使用して、 [v4 サポート ライブラリ](http://developer.android.com/tools/support-library/index.html)です。



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>カスタム アダプターでカーソル データの表示

連絡先の画像を表示する使わカスタム アダプターおを手動で解決できるよう、`PhotoId`イメージ ファイルのパスへの参照。

この例では、カスタム アダプターでデータを表示する、`CursorLoader`でローカルのコレクションにすべての連絡先データを取得する、 **FillContacts**メソッドから**ContactsProvider/ContactsAdapter.cs**:

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

使用して BaseAdapter のメソッドを実装、`contactList`コレクション。 同様に、その他のコレクションを使用して、アダプターの実装は&ndash;のデータに属しているために、特別な処理をここではありません、 `ContentProvider`:

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

(存在する場合、イメージが表示されます、デバイス上の画像ファイルを Uri を使用します。 アプリケーションは、次のようになります。

[![ListView; で連絡先を表示するアプリのスクリーン ショットイメージが 1 つのエントリの左側に表示されます。](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

同様のコード パターンを使用して、アプリケーションは、さまざまなユーザーの写真、ビデオ、音楽を含むシステム データにアクセスできます。
一部のデータ型は、プロジェクトの要求する特殊なアクセス許可を必要と**AndroidManifest.xml**です。



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter カーソル データの表示

カーソルに表示されることも、 `SimpleCursorAdapter` (名前のみが表示されますがフォトされません)。 このコードを使用する方法の例、`ContentProvider`で`SimpleCursorAdapter`(このコードは、サンプルでは表示されません)。

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

参照してください、[の Listview およびアダプター](~/android/user-interface/layouts/list-view/index.md)さらに実装する方法についての`SimpleCursorAdapter`します。


## <a name="related-links"></a>関連リンク

- [ContactsAdapter デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
