---
title: 'Xamarin.Essentials: 連絡先'
description: Xamarin.Essentials の Contacts クラスを使用すると、ユーザーは連絡先を選択して、それに関する情報を取得できます。
ms.assetid: 02280c42-720a-44c3-979e-4818a20c9821
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8e47fd77bedb701e1953d903091c77af31cb6012
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972293"
---
# <a name="no-locxamarinessentials-contacts"></a>Xamarin.Essentials: 連絡先

**Contacts** クラスを使用すると、ユーザーは連絡先を選択して、それに関する情報を取得できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Contacts** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

`ReadContacts` アクセス許可が必要です。Android プロジェクト内で構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.ReadContacts)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.READ_CONTACTS" /> />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可]** 領域を探し、このアクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="ios"></a>[iOS](#tab/ios)

`Info.plist` で、次のキーを追加します。

```xml
<key>NSContactsUsageDescription</key>
<string>This app needs access to contacts to pick a contact and get info.</string>
```

アプリ固有の説明の `<string>` がユーザーに表示されたら、それを更新してください。

# <a name="uwp"></a>[UWP](#tab/uwp)

**[機能]** の `Package.appxmanifest` で、`Contact` の機能がオンになっていることを確認します。

-----

## <a name="pick-a-contact"></a>連絡先を選択する

`Contacts.PickContactAsync()` を呼び出すと、[連絡先] ダイアログが表示され、ユーザーはユーザーに関する情報を受信できるようになります。


```csharp
try
{
    var contact = await Contacts.PickContactAsync();

    if(contact == null)
        return;

    var id = contact.Id;
    var namePrefix = contact.NamePrefix;
    var givenName = contact.GivenName;
    var middleName = contact.MiddleName;
    var familyName = contact.FamilyName;
    var nameSuffix = contact.NameSuffix;
    var displayName = contact.DisplayName;
    var phones = contact.Phones; // List of phone numbers
    var emails = contact.Emails; // List of email addresses
}
catch (Exception ex)
{
    // Handle exception here.
}
```

## <a name="get-all-contacts"></a>すべての連絡先を取得する

```csharp
ObservableCollection<Contact> contactsCollect = new ObservableCollection<Contact>();

try
{
    // cancellationToken parameter is optional
    var cancellationToken = default(CancellationToken);
    var contacts = await Contacts.GetAllAsync(cancellationToken);

    if (contacts == null)
        return;

    foreach (var contact in contacts)
        contactsCollect.Add(contact);
}
catch (Exception ex)
{
    // Handle exception here.
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

- `GetAllAsync` メソッドの `cancellationToken` パラメーターは、UWP でのみ使用されます。

# <a name="ios"></a>[Android](#tab/ios)

- `GetAllAsync` メソッドの `cancellationToken` パラメーターは、UWP でのみ使用されます。
- iOS プラットフォームでは、`DisplayName` プロパティがネイティブにサポートされていないため、`DisplayName` 値は "GivenName FamilyName" として構成されます。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----


## <a name="api"></a>API

- [Contacts ソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Contacts)
- [Contacts API のドキュメント](xref:Xamarin.Essentials.Contacts)
