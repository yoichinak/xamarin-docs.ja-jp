---
title: ユーザー プロファイル
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/22/2018
ms.openlocfilehash: 252a104118b0419f33abdf7f522ad8fc358e3f76
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028711"
---
# <a name="user-profile"></a>ユーザー プロファイル

Android では、API レベル5以降、 [ContactsContract](xref:Android.Provider.ContactsContract)プロバイダーで連絡先を列挙することがサポートされています。 たとえば、連絡先の一覧表示は、次のコード例に示すように、 [Contactcontracts. contacts](xref:Android.Provider.ContactsContract.Contacts)クラスを使用するのと同じように簡単です。

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

Android 4 (API レベル 14) 以降では、`ContactsContract` プロバイダーを通じて[ContactsContact](xref:Android.Provider.ContactsContract.Profile)クラスを使用できます。 `ContactsContact.Profile` を使用すると、デバイス所有者の名前や電話番号などの連絡先データを含む、デバイスの所有者の個人プロファイルにアクセスできます。

## <a name="required-permissions"></a>必要なアクセス許可

連絡先データの読み取りと書き込みを行うには、アプリケーションが `READ_CONTACTS` と `WRITE_CONTACTS` のアクセス許可をそれぞれ要求する必要があります。
さらに、ユーザープロファイルを読み取り、編集するには、アプリケーションで `READ_PROFILE` と `WRITE_PROFILE` のアクセス許可を要求する必要があります。

## <a name="updating-profile-data"></a>プロファイルデータを更新しています

これらのアクセス許可が設定されると、アプリケーションは通常の Android 手法を使用してユーザープロファイルのデータを操作できます。 たとえば、プロファイルの表示名を更新するには、次に示すように、 [ContactsContract](xref:Android.Provider.ContactsContract.Profile.ContentRawContactsUri)プロパティを使用して取得した `Uri` で、 [contentresolver.](xref:Android.Content.ContentResolver.Update*)を呼び出します。

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

最後に、ユーザープロファイルに移動するには、`ActionView` アクションと `ContactsContract.Profile.ContentUri` を使用してインテントを作成し、次のように `StartActivity` メソッドに渡します。

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);
StartActivity (intent);
```

上記のコードを実行すると、次のスクリーンショットに示すように、ユーザープロファイルが表示されます。

[John Doe user プロファイルを表示しているプロファイルのスクリーンショット![](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

ユーザープロファイルの操作は、Android の他のデータとの対話に似ており、追加のレベルのデバイスの個人用設定を提供します。

## <a name="related-links"></a>関連リンク

- [ContactsProviderDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/contactsproviderdemo)
- [アイスクリームサンドイッチの導入](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
