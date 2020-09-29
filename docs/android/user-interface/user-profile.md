---
title: ユーザー プロファイル
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/22/2018
ms.openlocfilehash: 1b7202a199315bf0be1664bf34725c26d12a66c8
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91458078"
---
# <a name="user-profile"></a>ユーザー プロファイル

Android では、API レベル5以降、 [ContactsContract](xref:Android.Provider.ContactsContract) プロバイダーで連絡先を列挙することがサポートされています。 たとえば、連絡先の一覧表示は、次のコード例に示すように、 [Contactcontracts. contacts](xref:Android.Provider.ContactsContract.Contacts) クラスを使用するのと同じように簡単です。

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

Android 4 (API レベル 14) 以降では、 [ContactsContact](xref:Android.Provider.ContactsContract.Profile) クラスはプロバイダーを通じて利用でき `ContactsContract` ます。 は、デバイス `ContactsContact.Profile` の所有者の個人プロファイルへのアクセスを提供します。このプロファイルには、デバイス所有者の名前や電話番号などの連絡先データが含まれます。

## <a name="required-permissions"></a>必要なアクセス許可

連絡先データの読み取りと書き込みを行うには、アプリケーションで `READ_CONTACTS` `WRITE_CONTACTS` アクセス許可とアクセス許可をそれぞれ要求する必要があります。
さらに、ユーザープロファイルを読み取り、編集するには、アプリケーションがおよびのアクセス許可を要求する必要があり `READ_PROFILE` `WRITE_PROFILE` ます。

## <a name="updating-profile-data"></a>プロファイルデータを更新しています

これらのアクセス許可が設定されると、アプリケーションは通常の Android 手法を使用してユーザープロファイルのデータを操作できます。 たとえば、プロファイルの表示名を更新するには、 [ContentResolver.Update](xref:Android.Content.ContentResolver.Update*) `Uri` 次に示すように、 [ContentRawContactsUri](xref:Android.Provider.ContactsContract.Profile.ContentRawContactsUri)プロパティを使用して取得したを使用して、contentresolver. を呼び出します。

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>プロファイルデータの読み取り

[ContactsContact](xref:Android.Provider.ContactsContract.Profile.ContentUri)にクエリを発行すると、プロファイルデータが読み取られます。 たとえば、次のコードは、ユーザープロファイルの表示名を読み取ります。

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

## <a name="navigating-to-the-user-profile"></a>ユーザープロファイルへの移動

最後に、ユーザープロファイルに移動するには、アクションとを使用してインテントを作成し、次 `ActionView` `ContactsContract.Profile.ContentUri` のようにメソッドに渡し `StartActivity` ます。

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);
StartActivity (intent);
```

上記のコードを実行すると、次のスクリーンショットに示すように、ユーザープロファイルが表示されます。

[![John Doe user プロファイルを表示しているプロファイルのスクリーンショット](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

ユーザープロファイルの操作は、Android の他のデータとの対話に似ており、追加のレベルのデバイスの個人用設定を提供します。

## <a name="related-links"></a>関連リンク

- [ContactsProviderDemo (サンプル)](/samples/xamarin/monodroid-samples/contactsproviderdemo)