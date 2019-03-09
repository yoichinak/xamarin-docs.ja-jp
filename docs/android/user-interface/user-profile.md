---
title: ユーザー プロファイル
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/22/2018
ms.openlocfilehash: 0613411e5436a0ea8ed08bf4af52dae84a9a701c
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670891"
---
# <a name="user-profile"></a>ユーザー プロファイル

Android には、列挙する連絡先がサポートされていますが、 [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) API Level 5 以降のプロバイダー。 連絡先の一覧を使用して同じくらい簡単ですなど、 [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/)クラスに次のコード例に示すようにします。

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Android 4 (API レベル 14) 以降、 [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/)クラスを利用、`ContactsContract`プロバイダー。 `ContactsContact.Profile`デバイス所有者の名前と電話番号などの連絡先データを含む、デバイスの所有者に個人プロファイルへのアクセスを提供します。


## <a name="required-permissions"></a>必要なアクセス許可

連絡先データを読み書きするアプリケーションを要求する必要があります、`READ_CONTACTS`と`WRITE_CONTACTS`アクセス許可は、それぞれします。
さらに、読み取りをユーザー プロファイルの編集は、アプリケーション要求する必要があります、`READ_PROFILE`と`WRITE_PROFILE`アクセス許可。


## <a name="updating-profile-data"></a>プロファイル データの更新

これらのアクセス許可を設定すると、アプリケーションは、ユーザー プロファイルのデータと対話する Android の通常の手法を使用できます。 たとえば、プロファイルの表示名を更新するには、呼び出す[ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update)で、`Uri`を使用して取得、 [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/)ように、プロパティ以下に：

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>プロファイル データの読み取り

発行するためのクエリ、 [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/)読み取りは、プロファイル データをバックアップします。 たとえば、次のコードでは、ユーザー プロファイルの表示名が表示されます。

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>ユーザー プロファイルに移動します。

最後に、ユーザー プロファイルに移動するのインテントを作成、`ActionView`アクションと`ContactsContract.Profile.ContentUri`に渡す、`StartActivity`このようなメソッド。

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

上記のコードを実行するときに、次のスクリーン ショットに示すように、ユーザー プロファイルが表示されます。

[![John Doe のユーザー プロファイルを表示するプロファイルのスクリーン ショット](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

ユーザー プロファイルの操作は Android では、その他のデータ操作に似ており、デバイスの個人用設定の追加レベル。



## <a name="related-links"></a>関連リンク

- [ContactsProviderDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
