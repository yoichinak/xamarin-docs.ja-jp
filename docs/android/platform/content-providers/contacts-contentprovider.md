---
title: 連絡先の ContentProvider の使用
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 48bb334e7e400d57e7eddc23b0b4ff183a7eba9b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669415"
---
# <a name="using-the-contacts-contentprovider"></a>連絡先の ContentProvider の使用

コードの使用がによって公開されるデータをアクセス、`ContentProvider`への参照を必要としない、`ContentProvider`すべてのクラスします。 によって公開されるデータ上でカーソルを作成する Uri を使用する代わりに、`ContentProvider`します。 Android は、Uri を使用して、システムを公開するアプリケーションで検索して、`ContentProvider`その識別子を使用します。 Uri は、逆引き DNS 形式で通常は、文字列など`com.android.contacts/data`します。

開発者がこの文字列は、Android を記憶するのではなく*連絡先*プロバイダーでは、そのメタデータを公開する、`android.provider.ContactsContract`クラス。 このクラスを使用して特定の Uri を`ContentProvider`テーブルとクエリを実行できる列の名前とします。

一部のデータ型にアクセスする特別なアクセス許可も必要です。 組み込みの連絡先リストが必要です、`android.permission.READ_CONTACTS`でアクセス許可、 **AndroidManifest.xml**ファイル。

Uri からカーソルを作成する 3 つの方法はあります。

1. **ManagedQuery()** &ndash; Android 2.3 (API レベル 10) 以降では、推奨されるアプローチを`ManagedQuery`カーソルを返し、データを更新して、カーソルを閉じるも自動的に管理します。 このメソッドは、Android 3.0 (API レベル 11) で非推奨とされます。

1. **ContentResolver.Query()** &ndash;は、更新、コードで明示的に終了する必要があります、非管理対象のカーソルを返します。

1. **CursorLoader() します。LoadInBackground()** &ndash; Android 3.0 (API レベル 11) で導入された`CursorLoader`が使用することをお勧め、`ContentProvider`します。 `CursorLoader` クエリを`ContentResolver`UI がブロックされないように、バック グラウンド スレッドでします。
   このクラスは、古いバージョンの Android v4 互換性ライブラリを使用してアクセスできます。


これらの各メソッドは、入力の同じ基本的なセットがあります。

-  **Uri** &ndash;の完全修飾名、`ContentProvider`します。
-  **プロジェクション**&ndash;カーソルを選択する列を指定します。
-  **選択範囲**&ndash;と同様に、SQL`WHERE`句。
-  **SelectionArgs** &ndash;選択範囲の代わりに使用するパラメーター。
-  **SortOrder** &ndash;によって並べ替える列。



## <a name="creating-inputs-for-a-query"></a>クエリの入力の作成

`ContactsProvider`サンプル コードは、Android の組み込みの連絡先プロバイダーに対して非常に単純なクエリを実行します。 実際 Uri または列の名前の連絡先を照会するために必要なすべての情報を把握する必要はありません`ContentProvider`によって公開されている定数として使用できますが、`ContactsContract`クラス。

ようには、これらの同じオブジェクトをパラメーターとして使用するよう、カーソルを取得する際に使用する方法に関係なく、 *ContactsProvider/ContactsAdapter.cs*ファイル。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

この例で、 `selection`、`selectionArgs`と`sortOrder`設定することにより無視されます`null`します。



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>コンテンツ プロバイダーの Uri からカーソルを作成します。

パラメーター オブジェクトが作成されるとは、次の 3 つの方法のいずれかで使用できます。



### <a name="using-a-managed-query"></a>管理対象のクエリを使用します。

Android 2.3 (API レベル 10) を対象とするアプリケーションや、以前は、このメソッドを使用する必要があります。

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

このカーソルを閉じるには必要はありませんので、Android によって管理されます。



### <a name="using-contentresolver"></a>ContentResolver を使用します。

アクセスする`ContentResolver`に対してカーソルを取得するには、直接、`ContentProvider`は、次のような完了します。

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

このカーソルは、管理対象はないため、不要になったときに終了しなければなりません。
コードが開いているカーソルを閉じると、それ以外の場合、エラーが発生ことを確認してください。

```csharp
cursor.Close();
```

または、呼び出せる`StartManagingCursor()`と`StopManagingCursor()`カーソルを管理します。 管理対象のカーソルは自動的に非アクティブになり、再度照会活動は停止して再起動します。



### <a name="using-cursorloader"></a>CursorLoader を使用します。

Android 3.0 (API レベル 11) 向けのアプリケーションまたは新しいはこのメソッドを使用する必要があります。

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader`によりすべてのカーソル操作は、バック グラウンド スレッドで行われ、適切に再利用できる既存のカーソル アクティビティのインスタンス間でアクティビティが再開されると (例: 構成の変更) が原因ではなく、データをもう一度再読み込みします。

以前の Android バージョンを使用することも、`CursorLoader`クラスを使用して、 [v4 サポート ライブラリ](https://developer.android.com/tools/support-library/index.html)します。



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>カスタム アダプターを使用したカーソルのデータを表示します。

連絡先の画像の表示に使用しますカスタム アダプターでは、手動で解決できるように、`PhotoId`イメージ ファイルのパスへの参照。

この例では、カスタム アダプターを使用したデータを表示する、`CursorLoader`でローカル コレクションにすべての連絡先データを取得する、 **FillContacts**メソッドから**ContactsProvider/ContactsAdapter.cs**:

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

使用して BaseAdapter のメソッドを実装し、`contactList`コレクション。 他のコレクションだと同様、アダプターの実装は&ndash;からデータのソースであるために、特別な処理をここではありません、 `ContentProvider`:

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

イメージが表示されます (存在する) 場合、Uri、デバイス上の画像ファイルを使用しています。 アプリケーションのようになります。

[![は、ListView で連絡先を表示するアプリのスクリーン ショットイメージが 1 つのエントリの左側に表示されます。](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

ようなコード パターンを使用して、アプリケーションは、さまざまなユーザーの写真、ビデオ、音楽を含むシステム データにアクセスできます。
一部のデータ型で、プロジェクトの要求に特別なアクセス許可を必要と**AndroidManifest.xml**します。



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter でカーソルのデータの表示

カーソルに表示されることも、 `SimpleCursorAdapter` (名のみが表示されますが、写真ではありません)。 このコードを使用する方法の例を`ContentProvider`で`SimpleCursorAdapter`(このコードは、サンプルでは表示されません)。

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

参照してください、 [Listview と Adapter](~/android/user-interface/layouts/list-view/index.md)実装について詳しくは`SimpleCursorAdapter`します。


## <a name="related-links"></a>関連リンク

- [ContactsAdapter デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
