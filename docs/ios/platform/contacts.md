---
title: 連絡先と ContactsUI Xamarin.iOS で
description: この記事では、新しいアドレス帳と連絡先の UI フレームワーク Xamarin.iOS アプリでの操作について説明します。 これらのフレームワークでは、既存のアドレス帳と iOS の以前のバージョンで使用されるアドレス帳の UI を置き換えます。
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e3f1533605d08df58d8d257714dd8135690c0e5d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105455"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>連絡先と ContactsUI Xamarin.iOS で

_この記事では、新しいアドレス帳と連絡先の UI フレームワーク Xamarin.iOS アプリでの操作について説明します。これらのフレームワークでは、既存のアドレス帳と iOS の以前のバージョンで使用されるアドレス帳の UI を置き換えます。_

Apple iOS 9 の導入に伴い、2 つの新しいフレームワークをリリースしました`Contacts`と`ContactsUI`置換時に、既存のアドレス帳、および iOS 8 およびそれ以前でアドレス帳の UI フレームワークを使用します。

2 つの新しいフレームワークには、次の機能が含まれます。

- [**連絡先**](#contacts) -ユーザーの連絡先リストのデータへのアクセスを提供します。
    ほとんどのアプリには、読み取り専用アクセスのみ必要とするため、このフレームワークは、スレッド セーフの読み取り専用アクセスの最適化されています。

- [**ContactsUI** ](#contactsui) -を表示するには、Xamarin.iOS の UI 要素の編集、選択、および iOS デバイスでの連絡先を作成します。

[![](contacts-images/add01.png "IOS デバイスでの使用例コンタクト シート")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> 既存の`AddressBook`と`AddressBookUI`フレームワーク iOS 8 で使用 (前) iOS 9 では非推奨し、新しいで置き換える必要があります`Contacts`と`ContactsUI`できるだけ早くすべて既存の Xamarin.iOS アプリのフレームワークです。 新しいフレームワークに対して、新しいアプリを記述する必要があります。




次のセクションでは、これらの新しいフレームワークと Xamarin.iOS アプリでそれらを実装する方法を見てをみましょう。

<a name="contacts" />

## <a name="the-contacts-framework"></a>連絡先のフレームワーク

連絡先フレームワークでは、ユーザーの連絡先情報への Xamarin.iOS のアクセスを提供します。 ほとんどのアプリには、読み取り専用アクセスのみ必要とするため、このフレームワークは、スレッド セーフの読み取り専用アクセスの最適化されています。

<a name="Contact_Objects" />

### <a name="contact-objects"></a>連絡先オブジェクト

`CNContact`クラス名、アドレスまたは電話番号などの連絡先のプロパティにスレッド セーフの読み取り専用アクセスを提供します。 `CNContact` などの関数を`NSDictionary`が含まれています (アドレス、電話番号など) のプロパティの複数の読み取り専用コレクション。

[![](contacts-images/contactobjects.png "連絡先オブジェクトの概要")](contacts-images/contactobjects.png#lightbox)

(電子メール アドレスまたは電話番号) などの複数の値を持つことができるプロパティの配列として表されます`NSLabeledValue`オブジェクト。 `NSLabeledValue` ラベルの読み取り専用のセットで構成されるスレッド セーフの組は、値のラベルがユーザー (自宅または職場の電子メールなど) に値を定義します。 連絡先のフレームワークには、定義済みのラベルの選択範囲が用意されています (を使用して、`CNLabelKey`と`CNLabelPhoneNumberKey`静的クラス)、アプリで使用することができますか、ニーズのカスタム ラベルを定義するオプションがあります。

既存の連絡先の値を調整する (または新規作成) する必要があるすべての Xamarin.iOS アプリを使用して、`NSMutableContact`クラスとそのサブ クラスのバージョン (など`CNMutablePostalAddress`)。

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

このコードは、iOS 9 デバイスで実行される場合は、ユーザーのコレクションに新しい連絡先が追加されます。 例えば:

[![](contacts-images/add01.png "ユーザーのコレクションに追加された新しい連絡先")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>連絡先の書式設定とローカライズ

連絡先のフレームワークには、いくつかのオブジェクトが含まれています。 とするのに役立つメソッドが書式設定し、ユーザーに表示するためのコンテンツをローカライズします。 たとえば、次のコードは、連絡先の名前と住所を表示するためは正しく書式設定。

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

アプリケーションの UI を表示するプロパティのラベル、連絡先のフレームワークもこれらの文字列をローカライズするためのメソッドを持っています。 ここでも、アプリで実行されている iOS デバイスの現在のロケールに基づきます。 例えば:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>既存の連絡先をフェッチしています

インスタンスを使用して、`CNContactStore`クラス、ユーザーの連絡先データベースからの連絡先情報をフェッチすることができます。 `CNContactStore`のすべてのフェッチまたは連絡先およびグループ データベースからを更新するために必要なメソッドが含まれています。 これらのメソッドは同期であるために、UI のブロックを防ぐためのバック グラウンド スレッドで実行することをお勧めします。

述語を使用して (から構築された、`CNContact`クラス)、データベースから連絡先をフェッチするときに返される結果をフィルター処理できます。 文字列が含まれている連絡先のみをフェッチする`Appleseed`、次のコードを使用します。

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> ジェネリックと複合の述語は、連絡先フレームワークによってサポートされていません。

制限のみをフェッチするなど、 **GivenName**と**FamilyName**連絡先のプロパティは、次のコードを使用します。

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

最後に、データベースを検索し、結果を返す、次のコードを使用します。

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

このコードは、サンプルで作成した後に実行された場合、 **Contacts オブジェクト**前のセクションで作成した"John Appleseed"連絡先が返されます。

### <a name="contact-access-privacy"></a>連絡先のアクセスのプライバシー

エンドユーザーは、許可またはアプリケーションごとに連絡先情報へのアクセスを拒否、ため、最初に呼び出しを作成するには`CNContactStore`アプリのアクセスを許可するかを確認するダイアログ ボックスが表示されます。

アクセス許可要求は、1 回のみ表示されます、初めてのアプリが実行、およびそれ以降を実行またはへの呼び出し、`CNContactStore`その時点で、ユーザーが選択されているアクセス許可を使用します。

その連絡先データベースへのアクセスを拒否するユーザーを適切に処理するようアプリを設計する必要があります。

#### <a name="fetching-partial-contacts"></a>部分的な連絡先をフェッチしています

A_部分にお問い合わせください_の連絡先ストアから利用可能なプロパティの一部のみがフェッチされる連絡先。 以前フェッチされていないプロパティにアクセスしようとすると、例外になります。

いずれかを使用して、必要なプロパティが指定された連絡先が含まれている簡単に確認することができます、`IsKeyAvailable`または`AreKeysAvailable`のメソッド、`CNContact`インスタンス。 例えば:

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
> `GetUnifiedContact`と`GetUnifiedContacts`のメソッド、`CNContactStore`クラス_のみ_部分の連絡先が提供されるフェッチ キーから要求されたプロパティに制限されます。

### <a name="unified-contacts"></a>統合メンバー

ユーザー (Facebook や Google のメールは、iCloud) などの連絡先のデータベースに 1 人のユーザーの連絡先情報のさまざまなソースがあります。 IOS および OS X アプリの場合は、この連絡先の情報に自動的に相互リンクされているし、する、1 つとしてユーザーに表示される_にお問い合わせください Unified_:

[![](contacts-images/unified01.png "連絡先の統合の概要")](contacts-images/unified01.png#lightbox)

Unified 連絡は、(必要な場合は、連絡先を更新するために使用する必要があります) を独自の一意の識別子を指定するリンクの連絡先情報のメモリ内の一時的なビューです。 連絡先のフレームワークでは既定では、可能な限り統一の連絡先を返します。

### <a name="creating-and-updating-contacts"></a>作成と連絡先の更新

説明したように、 [Contact オブジェクト](#Contact_Objects)前のセクションを使用する、`CNContactStore`のインスタンスを`CNMutableContact`ユーザーに書き込まれる、新しい連絡先を作成するにを使用してデータベースにお問い合わせください、 `CNSaveRequest`:

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

A`CNSaveRequest`を 1 つの操作に複数の連絡先およびグループの変更をキャッシュし、その変更をバッチ処理もでき、`CNContactStore`します。

フェッチ操作から取得した変更できない連絡先を更新するには、まず、変更および連絡先ストアに保存する変更可能なコピーを要求する必要があります。 例えば:

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

連絡先が変更されるたびに、連絡先ストアの投稿、`CNContactStoreDidChangeNotification`既定の通知センターにします。 キャッシュするか、または連絡先を表示する現在の場合は、連絡先ストアからこれらのオブジェクトを更新する必要があります (`CNContactStore`)。

### <a name="containers-and-groups"></a>コンテナーとグループ

ユーザーの連絡先は、またはローカル ユーザーのデバイスに 1 つまたは複数のサーバー アカウント (Facebook、Google など) から、デバイスに同期された連絡先として存在します。 連絡先の各プールには、独自_コンテナー_し、特定の連絡先は、1 つのコンテナーにのみ存在できます。

[![](contacts-images/containers01.png "コンテナーとグループの概要")](contacts-images/containers01.png#lightbox)

1 つまたは複数に配置する連絡先のいくつかのコンテナーを使用する_グループ_または_サブグループ_します。 この動作は、特定のコンテナーのバッキング ストアに依存します。 たとえば、iCloud に 1 つだけのコンテナーがありますが、多くのグループ (ただし、サブグループではありません) を持つことができます。 Microsoft Exchange は、その一方で、グループをサポートしていませんが (Exchange フォルダーごとに 1 つ) の複数のコンテナーを持つことができます。

[![](contacts-images/containers02.png "コンテナーとグループ内で重複します。")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>ContactsUI フレームワーク

アプリケーションはない必要がある場合、カスタム UI を表示する、表示、編集、選択、および xamarin ios アプリで連絡先を作成する UI 要素を提示する ContactsUI フレームワークを使用できます。

Xamarin.iOS アプリで連絡先をサポートするために作成する必要があるコードの量の削減だけでなく Apple の組み込みコントロールを使用して、アプリのユーザーに一貫性のあるインターフェイスを提供します。

### <a name="the-contact-picker-view-controller"></a>連絡先ピッカー ビュー コント ローラー

連絡先ピッカーのビュー コント ローラー (`CNContactPickerViewController`) ユーザーがユーザーの連絡先データベースから連絡先をまたはにお問い合わせくださいプロパティを選択できる標準の連絡先ピッカー ビューを管理します。 お問い合わせくださいピッカー ビュー コント ローラーがピッカーを表示する前にアクセス許可を要求していないと、ユーザーが 1 つまたは複数の連絡先を (その使用法に基づく) を選択できます。

呼び出す前に、`CNContactPickerViewController`クラス、ユーザーが選択され、表示と連絡先のプロパティの選択を制御する述語を定義するプロパティを定義します。

継承するクラスのインスタンスを使用して、`CNContactPickerDelegate`ピッカーを使用して、ユーザーの対話に応答します。 例えば:

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

ユーザーが自身のデータベース内の連絡先から電子メール アドレスを選択できるようにするには、次のコードを使用できます。

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

連絡先のビュー コント ローラー (`CNContactViewController`) クラスには、エンドユーザーに標準の連絡先のビューを提示するコント ローラーが用意されています。 連絡先ビューが新規、不明、または既存の新しい連絡先を表示し、適切な静的コンス トラクターを呼び出すことによって、ビューが表示される前に、型を指定する必要があります (`FromNewContact`、 `FromUnknownContact`、 `FromContact`)。 たとえば、次のように入力します。

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリケーションで連絡先と連絡先の UI フレームワークの使用方法について詳しく説明をしました。 最初に、さまざまな種類の連絡先のフレームワークを提供するオブジェクトとを新規作成または既存の連絡先へのアクセスの使用方法を説明しました。 既存の連絡先を選択して連絡先情報の表示にお問い合わせください UI フレームワークについても確認します。


## <a name="related-links"></a>関連リンク

- [QuickContacts サンプル](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [IOS 9 で新します。](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [連絡先のフレームワーク参照](https://developer.apple.com/documentation/contacts?language=objc)
- [ContactsUI のフレームワーク参照](https://developer.apple.com/documentation/contactsui?language=objc)
