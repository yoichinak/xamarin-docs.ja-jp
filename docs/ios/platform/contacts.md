---
title: 取引先担当者および ContactsUI
description: この記事では、新しいアドレス帳と連絡先 UI Xamarin.iOS アプリのフレームワークの操作について説明します。 これらのフレームワークでは、既存のアドレス帳と iOS の以前のバージョンで使用されているアドレス帳の UI を置き換えます。
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4d963bbefce2b4564c3f352be5768df77b45b34d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="contacts-and-contactsui"></a>取引先担当者および ContactsUI

_この記事では、新しいアドレス帳と連絡先 UI Xamarin.iOS アプリのフレームワークの操作について説明します。これらのフレームワークでは、既存のアドレス帳と iOS の以前のバージョンで使用されているアドレス帳の UI を置き換えます。_

Apple iOS 9 の導入に伴い、2 つの新しいフレームワークをリリースしました`Contacts`と`ContactsUI`置換時に、既存のアドレス帳、および iOS 8 およびそれ以前でアドレス帳の UI フレームワークが使用されています。

2 つの新しいフレームワークには、次の機能が含まれています。

- [**連絡先**](#contacts) -ユーザーの連絡先リストのデータへのアクセスを提供します。
    ほとんどのアプリには、読み取り専用アクセスのみが必要なために、このフレームワークは、スレッド セーフである、読み取り専用アクセスに最適化されています。

- [**ContactsUI** ](#contactsui) -表示するには、Xamarin.iOS UI 要素の編集を選択、および iOS デバイスで連絡先を作成します。

[![](contacts-images/add01.png "IOS デバイスでの使用例コンタクト シート")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> 既存の`AddressBook`と`AddressBookUI`フレームワーク iOS 8 で使用する (前) iOS 9 で廃止されたおよび新しいに置き換える必要が`Contacts`と`ContactsUI`できるだけ早くどの既存 Xamarin.iOS アプリのフレームワークです。 新しいフレームワークに対しては、新しいアプリを記述する必要があります。




次のセクションで、これらの新しいフレームワークと Xamarin.iOS アプリでそれらを実装する方法を見てをみましょう。

<a name="contacts" />

## <a name="the-contacts-framework"></a>連絡先のフレームワーク

連絡先のフレームワークでは、ユーザーの連絡先情報を Xamarin.iOS アクセスを提供します。 ほとんどのアプリには、読み取り専用アクセスのみが必要なために、このフレームワークは、スレッド セーフである、読み取り専用アクセスに最適化されています。

<a name="Contact_Objects" />

### <a name="contact-objects"></a>連絡先オブジェクト

`CNContact`クラス名、アドレスまたは電話番号など、連絡先のプロパティにスレッド セーフである、読み取り専用アクセスを提供します。 `CNContact` などの関数、`NSDictionary`が含まれています (アドレス、電話番号など) のプロパティの複数の読み取り専用コレクション。

[![](contacts-images/contactobjects.png "連絡先オブジェクトの概要")](contacts-images/contactobjects.png#lightbox)

(電子メール アドレスまたは電話番号など) の複数の値を持つことができる任意のプロパティの配列として表されます`NSLabeledValue`オブジェクト。 `NSLabeledValue` 読み取り専用の一連のラベルで構成される、スレッド セーフである組は、値のラベルがユーザー (自宅または職場の電子メールなど) に値を定義します。 連絡先 framework には、定義済みのラベルの選択が用意されています (を使用して、`CNLabelKey`と`CNLabelPhoneNumberKey`静的クラス) をアプリで使用できるかをお客様のニーズのカスタム ラベルを定義するオプションがします。

既存の連絡先の値を調整 (または新規作成) する必要があるすべての Xamarin.iOS アプリを使用して、`NSMutableContact`クラスとそのサブ クラスのバージョン (など`CNMutablePostalAddress`)。

たとえば、次のコードは新しい連絡先を作成し、連絡先のユーザーのコレクションに追加します。

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

IOS 9 デバイスでは、このコードを実行した場合、新しい連絡先は、ユーザーのコレクションに追加されます。 例えば:

[![](contacts-images/add01.png "ユーザーのコレクションに追加された新しい連絡先")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>連絡先の書式設定とローカライズ

連絡先フレームワークには、複数のオブジェクトが含まれています。 とするのに役立つ方法は、書式設定し、ユーザーに表示するためのコンテンツをローカライズします。 たとえば、次のコードが正しい形式で指定連絡先の名前と住所を表示するため。

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

アプリの UI に表示されるプロパティのラベル、連絡先フレームワークにもそれらの文字列をローカライズするためのメソッドを持っています。 ここでも、これは、アプリを実行している iOS デバイスの現在のロケールに基づいています。 例えば:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOption.Nickname));
Console.WriteLine(CNLabeledValue.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>既存の連絡先をフェッチ

インスタンスを使用して、`CNContactStore`クラス、ユーザーの連絡先データベースから連絡先に関する情報をフェッチすることができます。 `CNContactStore`すべてのフェッチまたは連絡先、およびデータベースからグループを更新するために必要なメソッドが含まれています。 これらのメソッドは同期であるために、それらを UI のブロックを防止するには、バック グラウンド スレッドで実行することをお勧めします。

述語を使用して (から構築された、`CNContact`クラス)、データベースから連絡先をフェッチするときに返される結果をフィルター処理することができます。 文字列を含むメンバーのみをフェッチする`Appleseed`、次のコードを使用します。

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> 連絡先、フレームワークによっては、汎用的な複合述語はサポートされていません。

例については、のみにフェッチを制限する、 **GivenName**と**FamilyName**連絡先のプロパティは、次のコードを使用します。

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

このコードは、このサンプルで作成した後に実行された場合、**連絡先オブジェクト**前のセクション、先ほど作成した"John Appleseed"連絡先を返すことができます。

### <a name="contact-access-privacy"></a>アクセスのプライバシー

エンドユーザーを許可または、アプリケーションごとにその連絡先情報へのアクセスを拒否するため、最初に確認するへの呼び出し、`CNContactStore`アプリのアクセスを許可するよう求めるダイアログが表示されます。

アクセス許可の要求は、1 回のみ表示されます、初めてのアプリが実行、およびそれ以降を実行するために呼び出す、`CNContactStore`その時点で、ユーザーが選択されているアクセス許可を使用します。

連絡先データベースへのアクセスを拒否するユーザーを適切に処理するようアプリを設計する必要があります。

#### <a name="fetching-partial-contacts"></a>部分的な連絡先をフェッチ

A_部分にお問い合わせください_は連絡先ストアから利用可能なプロパティの一部のみがフェッチされることにお問い合わせください。 以前はフェッチされませんが、プロパティにアクセスしようとすると、例外になります。

担当者がいずれかを使用して目的のプロパティを持つかどうかを確認する簡単に確認することができます、`IsKeyAvailable`または`AreKeysAvailable`のメソッド、`CNContact`インスタンス。 例えば:

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
> `GetUnifiedContact`と`GetUnifiedContacts`のメソッド、`CNContactStore`クラス_のみ_によって提供されるフェッチ キーから要求されたプロパティを上限とする部分的な連絡先を返します。

### <a name="unified-contacts"></a>統合メンバー

ユーザー (iCloud、Facebook や Google メール) などの連絡先データベースに 1 人のユーザーの連絡先情報のさまざまなソースがあります。 IOS および OS X アプリでこの連絡先の情報に自動的に相互リンクされているし、する、1 つとしてユーザーに表示される_にお問い合わせください Unified_:

[![](contacts-images/unified01.png "統一された連絡先の概要")](contacts-images/unified01.png#lightbox)

この統合にお問い合わせは、(必要な場合は、連絡先を更新するために使用する必要があります) を独自の一意の識別子を指定するリンクの連絡先情報の一時的なメモリ内ビューです。 既定では、連絡先、フレームワークは可能な場合は、Unified 連絡先を返します。

### <a name="creating-and-updating-contacts"></a>作成して、連絡先を更新

説明したとおり、 [Contact オブジェクト](#Contact_Objects)前のセクションを使用する、`CNContactStore`クラスのインスタンスおよび、`CNMutableContact`ユーザーに書き込まれる、新しい連絡先を作成するにを使用してデータベースにお問い合わせください、 `CNSaveRequest`:

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

A`CNSaveRequest`を 1 つの操作に複数の連絡先とグループの変更をキャッシュし、に、これらの変更のバッチにも使用することができます、`CNContactStore`です。

フェッチ操作から取得した、変更できない連絡先を更新するには、まず次に変更し、連絡先のストアに保存する変更可能なコピーを要求する必要があります。 例えば:

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

連絡先が変更されるたびに連絡先ストアの投稿、`CNContactStoreDidChangeNotification`既定通知センターにします。 キャッシュに格納されては現在、連絡先を表示するか、連絡先ストアからこれらのオブジェクトを更新する必要があります (`CNContactStore`)。

### <a name="containers-and-groups"></a>コンテナーとグループ

ユーザーの連絡先は、または ローカル ユーザーのデバイスとして (Facebook、Google など) の 1 つまたは複数のサーバー アカウントから、デバイスに同期された連絡先が存在できます。 アドレス帳の各プールが独自_コンテナー_し、指定された連絡先が 1 つのコンテナーにのみ存在できます。

[![](contacts-images/containers01.png "コンテナーとグループの概要")](contacts-images/containers01.png#lightbox)

連絡先の 1 つ以上に配置するいくつかのコンテナーを使用する_グループ_または_サブグループ_です。 この動作は、特定のコンテナーのバッキング ストアに左右されます。 たとえば、iCloud のみ 1 つのコンテナーが多数のグループ (ただし、下位のグループではありません) を持つことができます。 その一方で、Microsoft Exchange は、グループをサポートしていませんが、複数のコンテナー (Exchange フォルダーごとに 1 つ) を持つことができます。

[![](contacts-images/containers02.png "コンテナーとグループ内で重複します。")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>ContactsUI フレームワーク

ここで、アプリケーションは、カスタム UI を表示する必要はありませんの場合、表示、編集を選択し、Xamarin.iOS アプリで連絡先を作成する UI 要素を提示する ContactsUI フレームワークを使用できます。

Xamarin.iOS アプリで連絡先をサポートするために作成する必要があるコードの量の削減だけでなく Apple の組み込みコントロールを使用してがアプリケーションのユーザーに一貫性のあるインターフェイスを提供します。

### <a name="the-contact-picker-view-controller"></a>連絡先ピッカー ビュー コント ローラー

連絡先ピッカー ビュー コント ローラー (`CNContactPickerViewController`) ユーザーがユーザーの連絡先データベースから連絡先または連絡先のプロパティを選択できる標準の連絡先ピッカー ビューを管理します。 ユーザーが 1 つまたは複数の連絡先を (その使用法に基づく) を選択して、ピッカーのビューの連絡コント ローラーがピッカーを表示する前にアクセス許可を要求していません。

呼び出す前に、`CNContactPickerViewController`クラス、プロパティを定義するどのユーザーが選択し、表示と連絡先のプロパティの選択を制御する述語を定義できます。

継承するクラスのインスタンスを使用して`CNContactPickerDelegate`ピッカーを使用して、ユーザーの対話に応答します。 例えば:

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

データベースに連絡先から電子メール アドレスを選択するユーザーを許可するのには、次のコードを使用できます。

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

### <a name="the-contact-view-controller"></a>連絡先のビュー コント ローラー

連絡先ビュー コント ローラー (`CNContactViewController`) クラスは、エンドユーザーに標準ビューの連絡先を表示するコント ローラーを提供します。 連絡先の表示が新しい新規、不明または既存の連絡先を表示し、適切な静的コンス トラクターを呼び出すことによって、ビューが表示される前に、種類を指定する必要があります (`FromNewContact`、 `FromUnknownContact`、 `FromContact`)。 たとえば、次のように入力します。

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリケーションでの連絡先と連絡先の UI フレームワークでの作業についての詳細を取得しました。 最初に、さまざまな種類の連絡先 framework が提供するオブジェクトとを新規作成または既存の連絡先へのアクセスの使用方法を説明します。 既存の連絡先を選択し、連絡先情報を表示するにお問い合わせの UI フレームワークも検討します。


## <a name="related-links"></a>関連リンク

- [QuickContacts サンプル](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [新機能 iOS 9](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-DontLinkElementID_14))
- [連絡先 Framework リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)
- [ContactsUI Framework リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)
