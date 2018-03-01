---
title: "ユーザー プロファイル"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>ユーザー プロファイル

Android は列挙の連絡先をサポートされる、 `ContactsContract` API レベル 5 以降のプロバイダー。 たとえば、リストに連絡先が同じくらい簡単です、`ContactContracts.Contacts`クラスに、次のコードに示すように。

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Android 4 (API レベル 14、)、新しい`ContactsContact.Profile`クラスは、ContactsContract プロバイダーを介して使用できます。 `ContactsContact.Profile`デバイスの所有者の名前と電話番号などの連絡先データを含む、デバイスの所有者を個人プロファイルへのアクセスを提供します。

<a name="Required_Permissions" />

## <a name="required-permissions"></a>必要なアクセス許可

連絡先データを読み書きするアプリケーションを要求する必要があります、`Read_Contacts`と`Write_Contacts`アクセス許可、それぞれします。 さらに、閲覧、ユーザー プロファイルを編集、アプリケーション要求する必要があります、`Read_Profile`と`Write_Profile`アクセス許可。

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>プロファイル データの更新

これらのアクセス許可を設定すると、アプリケーションは、ユーザー プロファイルのデータと対話する標準の Android 手法を使用できます。 例についてを呼び出して、プロファイルの表示名を更新する`ContentResolver.Update`で、`Uri`を介して取得、`ContactsContract.Profile.ContentRawContactsUri`プロパティ、次のように。

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>プロファイル データの読み取り

クエリを発行、`ContactsContact.Profile.ContentUri`読み取りは、プロファイル データをバックアップします。 たとえば、次のコードのユーザー プロファイルの表示名が表示されます。

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>ユーザーのアプリに移動します。

最後に Android 4 に付属している新しいユーザー アプリでのユーザー プロファイルに移動すると、作成するだけで、目的とした、`ActionView`アクションと`ContactsContract.Profile.ContentUri`に渡すと、`StartActivity`次のようなメソッド。

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

上記のコードを実行する場合、ユーザー アプリは、次のスクリーン ショットに示すように、ユーザー プロファイルに読み込まれます。

[![スクリーン ショットのユーザーのアプリ、John doe さんのユーザー プロファイルを表示します。](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

ユーザー プロファイルの操作は、android での他のデータとの対話に似ていますし、デバイスの個人用設定の追加レベルを提供します。



## <a name="related-links"></a>関連リンク

- [ContactsProviderDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
