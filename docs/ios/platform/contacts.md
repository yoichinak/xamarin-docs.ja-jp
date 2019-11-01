---
title: ContactsUI の Contacts と連絡先
description: この記事では、Xamarin iOS アプリで新しい連絡先と連絡先の UI フレームワークを使用する方法について説明します。 これらのフレームワークは、以前のバージョンの iOS で使用されていた既存のアドレス帳およびアドレス帳の UI に代わるものです。
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 438ed93bafa37496e6a97ea2fe98ca6a515682cf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032567"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>ContactsUI の Contacts と連絡先

_この記事では、Xamarin iOS アプリで新しい連絡先と連絡先の UI フレームワークを使用する方法について説明します。これらのフレームワークは、以前のバージョンの iOS で使用されていた既存のアドレス帳およびアドレス帳の UI に代わるものです。_

IOS 9 の導入により、Apple は、iOS 8 以前で使用されていた既存のアドレス帳とアドレス帳の UI フレームワークを置き換える2つの新しいフレームワーク `Contacts` と `ContactsUI`をリリースしました。

2つの新しいフレームワークには、次の機能が含まれています。

- [**連絡先**](#contacts)-ユーザーの連絡先一覧データへのアクセスを提供します。
  ほとんどのアプリは読み取り専用アクセスのみを必要とするため、このフレームワークは、スレッドセーフで読み取り専用アクセスに対して最適化されています。

- [**ContactsUI**](#contactsui) -ios デバイスで連絡先を表示、編集、選択、および作成するための XAMARIN の UI 要素を提供します。

[![](contacts-images/add01.png "An example Contact Sheet on an iOS device")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> IOS 8 (およびそれ以前) で使用されていた既存の `AddressBook` と `AddressBookUI` フレームワークは、iOS 9 では非推奨とされており、既存の Xamarin iOS アプリでできるだけ早く新しい `Contacts` および `ContactsUI` フレームワークに置き換える必要があります。 新しいアプリは、新しいフレームワークに対して作成する必要があります。

以下のセクションでは、これらの新しいフレームワークと、それらを Xamarin iOS アプリに実装する方法について説明します。

<a name="contacts" />

## <a name="the-contacts-framework"></a>連絡先フレームワーク

連絡先フレームワークでは、ユーザーの連絡先情報に対して Xamarin. iOS アクセスを提供します。 ほとんどのアプリは読み取り専用アクセスのみを必要とするため、このフレームワークは、スレッドセーフで読み取り専用アクセスに対して最適化されています。

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Contact オブジェクト

`CNContact` クラスは、連絡先のプロパティ (名前、住所、電話番号など) に対して、スレッドセーフで読み取り専用のアクセスを提供します。 `NSDictionary` のような機能を `CNContact`、複数の読み取り専用コレクション (住所や電話番号など) が含まれています。

[![](contacts-images/contactobjects.png "Contact Object overview")](contacts-images/contactobjects.png#lightbox)

複数の値 (電子メールアドレスや電話番号など) を持つことができるプロパティについては、`NSLabeledValue` オブジェクトの配列として表現されます。 `NSLabeledValue` は、ラベルと値の読み取り専用セットで構成されるスレッドセーフなタプルであり、ラベルによってユーザーの値 (自宅や勤務先の電子メールなど) が定義されます。 連絡先フレームワークには、アプリで使用できる定義済みのラベル (`CNLabelKey` および `CNLabelPhoneNumberKey` 静的クラスを使用) が用意されています。また、ニーズに合わせてカスタムラベルを定義することもできます。

既存の連絡先の値を調整する必要がある (または新しい連絡先を作成する) Xamarin iOS アプリの場合は、クラスの `NSMutableContact` バージョンとそのサブクラス (`CNMutablePostalAddress`など) を使用します。

たとえば、次のコードでは、新しい連絡先を作成し、ユーザーの連絡先のコレクションに追加します。

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

このコードが iOS 9 デバイスで実行されている場合は、新しい連絡先がユーザーのコレクションに追加されます。 (例:

[![](contacts-images/add01.png "A new contact added to the user's collection")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>連絡先の書式設定とローカライズ

Contacts フレームワークには、ユーザーに表示するコンテンツの書式設定とローカライズに役立つオブジェクトとメソッドがいくつか含まれています。 たとえば、次のコードでは、表示する連絡先の名前と住所を正しく書式設定しています。

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

アプリの UI に表示されるプロパティラベルについては、Contact framework にもこれらの文字列をローカライズするためのメソッドがあります。 ここでも、これは、アプリが実行されている iOS デバイスの現在のロケールに基づいています。 (例:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>既存の連絡先を取得しています

`CNContactStore` クラスのインスタンスを使用すると、ユーザーの連絡先データベースから連絡先情報を取得できます。 `CNContactStore` には、データベースから連絡先とグループをフェッチまたは更新するために必要なすべての方法が含まれています。 これらのメソッドは同期的であるため、UI がブロックされるのを防ぐために、バックグラウンドスレッドで実行することをお勧めします。

(`CNContact` クラスから構築された) 述語を使用すると、データベースから連絡先をフェッチするときに返される結果をフィルター処理できます。 文字列 `Appleseed`を含む連絡先のみをフェッチするには、次のコードを使用します。

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> 汎用述語と複合述語は、Contacts フレームワークではサポートされていません。

たとえば、アドレス帳の**GivenName**および**FamilyName**プロパティのみにフェッチを制限するには、次のコードを使用します。

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

最後に、データベースを検索し、結果を返すには、次のコードを使用します。

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

上記の「 **Contacts オブジェクト**」セクションで作成したサンプルの後でこのコードを実行すると、先ほど作成した "John Appleseed" 連絡先が返されます。

### <a name="contact-access-privacy"></a>連絡先アクセスのプライバシー

エンドユーザーは、アプリケーションごとに連絡先情報へのアクセスを許可または拒否できるため、初めて `CNContactStore`の呼び出しを行うときに、アプリへのアクセスを許可するように求めるダイアログが表示されます。

アクセス許可要求は1回だけ提示されます。アプリが初めて実行されるときに、その後の実行または `CNContactStore` の呼び出しで、その時点でユーザーが選択したアクセス許可が使用されます。

ユーザーが連絡先データベースへのアクセスを拒否されるように、アプリを設計する必要があります。

#### <a name="fetching-partial-contacts"></a>部分的な連絡先のフェッチ

_一部の連絡先_とは、使用可能なプロパティの一部だけがの連絡先ストアからフェッチされた連絡先です。 以前にフェッチされていないプロパティにアクセスしようとすると、例外が発生します。

`CNContact` インスタンスの `IsKeyAvailable` メソッドまたは `AreKeysAvailable` メソッドを使用して、特定の連絡先に必要なプロパティがあるかどうかを簡単に確認できます。 (例:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> `CNContactStore` クラスの `GetUnifiedContact` および `GetUnifiedContacts` メソッドは、指定されたフェッチキーから要求されたプロパティに限定された部分的な連絡先_のみ_を返します。

### <a name="unified-contacts"></a>ユニファイド連絡先

ユーザーは、連絡先データベース (iCloud、Facebook、Google Mail など) の1人に対して、異なる連絡先情報のソースを持っている場合があります。 IOS と OS X のアプリでは、この連絡先情報は自動的にリンクされ、単一の統合された_連絡先_としてユーザーに表示されます。

[![](contacts-images/unified01.png "Unified Contacts overview")](contacts-images/unified01.png#lightbox)

このユニファイド連絡先は、リンクの連絡先情報を一時的にメモリ内で表示したものであり、独自の一意の識別子が付与されます (必要に応じて連絡先を再フェッチために使用する必要があります)。 既定では、可能な限り、連絡先フレームワークは統合された連絡先を返します。

### <a name="creating-and-updating-contacts"></a>連絡先の作成と更新

前述の「 [Contact Objects](#Contact_Objects) 」セクションで説明したように、`CNContactStore` と `CNMutableContact` のインスタンスを使用して新しい連絡先を作成し、`CNSaveRequest`を使用してユーザーの連絡先データベースに書き込みます。

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

`CNSaveRequest` を使用すると、複数の連絡先とグループの変更を1つの操作にキャッシュし、それらの変更を `CNContactStore`に対してバッチ処理することもできます。

フェッチ操作から取得した変更できない連絡先を更新するには、最初に変更可能なコピーを要求してから、連絡先ストアに保存し直す必要があります。 (例:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>連絡先の変更通知

連絡先が変更されるたびに、連絡先ストアは既定の通知センターに `CNContactStoreDidChangeNotification` を投稿します。 キャッシュされているか、現在連絡先を表示している場合は、それらのオブジェクトを連絡先ストア (`CNContactStore`) から更新する必要があります。

### <a name="containers-and-groups"></a>コンテナーとグループ

ユーザーの連絡先は、ユーザーのデバイスにローカルに存在することも、1つまたは複数のサーバーアカウント (Facebook や Google など) からデバイスに同期された連絡先として使用することもできます。 連絡先の各プールには独自の_コンテナー_があり、特定の連絡先は1つのコンテナーにのみ存在できます。

[![](contacts-images/containers01.png "Containers and Groups overview")](contacts-images/containers01.png#lightbox)

一部のコンテナーでは、連絡先を1つ以上の_グループ_または_サブグループ_に配置できます。 この動作は、特定のコンテナーのバッキングストアに依存します。 たとえば、iCloud に含まれるコンテナーは1つだけですが、多数のグループを持つことができます (ただし、サブグループは含まれません)。 一方、Microsoft Exchange では、グループはサポートされていませんが、複数のコンテナー (Exchange フォルダーごとに1つ) を持つことができます。

[![](contacts-images/containers02.png "Overlap within Containers and Groups")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>ContactsUI フレームワーク

アプリケーションでカスタム UI を提供する必要がない場合は、ContactsUI フレームワークを使用して UI 要素を表示し、Xamarin. iOS アプリで連絡先を表示、編集、選択、および作成することができます。

Apple の組み込みコントロールを使用すると、Xamarin. iOS アプリで連絡先をサポートするために作成する必要があるコードの量を減らすだけでなく、一貫したインターフェイスをアプリのユーザーに提供できます。

### <a name="the-contact-picker-view-controller"></a>連絡先ピッカービューコントローラー

連絡先の選択ビューコントローラー (`CNContactPickerViewController`) は、ユーザーが連絡先または連絡先のプロパティをユーザーの連絡先データベースから選択できる標準の連絡先選択ビューを管理します。 ユーザーは、(使用状況に基づいて) 1 つ以上の連絡先を選択できます。また、[コンタクトピッカー] ビューコントローラーは、ピッカーを表示する前にアクセス許可を要求しません。

`CNContactPickerViewController` クラスを呼び出す前に、ユーザーが選択できるプロパティと、連絡先のプロパティの表示と選択を制御するための述語を定義します。

`CNContactPickerDelegate` から継承するクラスのインスタンスを使用して、ユーザーのピッカーとの対話に応答します。 (例:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

ユーザーがデータベースの連絡先から電子メールアドレスを選択できるようにするには、次のコードを使用します。

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>連絡先ビューコントローラー

Contact View Controller (`CNContactViewController`) クラスは、標準の連絡先ビューをエンドユーザーに提示するためのコントローラーを提供します。 連絡先ビューには、新しい、不明または既存の連絡先を表示できます。また、正しい静的コンストラクター (`FromNewContact`、`FromUnknownContact`、`FromContact`) を呼び出すことで、ビューを表示する前に種類を指定する必要があります。 たとえば、次のように入力します。

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>まとめ

この記事では、Xamarin. iOS アプリケーションで連絡先と連絡先の UI フレームワークを操作する方法について詳しく説明しました。 まず、Contact framework が提供するさまざまな種類のオブジェクトと、それらを使用して新規作成や既存の連絡先へのアクセスを行う方法について説明します。 また、連絡先 UI フレームワークを調べて既存の連絡先を選択し、連絡先情報を表示します。

## <a name="related-links"></a>関連リンク

- [連絡先のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/contacts/)
- [IOS 9 の新機能](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Contacts フレームワークリファレンス](https://developer.apple.com/documentation/contacts?language=objc)
- [ContactsUI Framework リファレンス](https://developer.apple.com/documentation/contactsui?language=objc)
