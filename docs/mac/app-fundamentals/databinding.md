---
title: Xamarin. Mac でのデータバインディングとキー値のコーディング
description: この記事では、キーと値のコードを使用して、Xcode の Interface Builder の UI 要素にデータをバインドできるようにするためのキーと値の監視について説明します。
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: ab5355459f12c8b3cb7fa5f262f015bfd64d3e08
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290158"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Xamarin. Mac でのデータバインディングとキー値のコーディング

_この記事では、キーと値のコードを使用して、Xcode の Interface Builder の UI 要素にデータをバインドできるようにするためのキーと値の監視について説明します。_

## <a name="overview"></a>概要

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合、 *Xcode と*で作業する開発者が行う*のと同じ*キー値のコーディングとデータバインディングの手法にアクセスできます。 Xcode は直接統合されているため、コードを記述する代わりに、Xcode の_Interface Builder_を使用して、UI 要素とのデータバインドを行うことができます。

UI 要素を設定して操作するために、Xamarin. Mac アプリケーションでキー値のコードとデータバインディングの手法を使用することにより、記述して維持する必要があるコードの量を大幅に減らすことができます。 また、フロントエンドのユーザーインターフェイス (_モデルビューコントローラー_) からバッキングデータ (_データモデル_) をさらに分離することもできます。これにより、管理が容易になり、アプリケーションの設計をより柔軟に行うことができます。

[![実行中のアプリの例](databinding-images/intro01.png "実行中のアプリの例")](databinding-images/intro01-large.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのキー値のコーディングとデータバインディングの操作の基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>キー値のコーディングとは

キー値のコーディング (KVC) は、オブジェクトのプロパティに間接的にアクセスするためのメカニズムです。キー (特殊な書式設定文字列) を使用して、インスタンス変数また`get/set`はアクセサーメソッド () を使用してアクセスするのではなく、プロパティを識別します。 Xamarin. Mac アプリケーションでキー値のコーディングに準拠したアクセサーを実装することによって、キー値の観察 (KVO)、データバインディング、コアデータ、Cocoa バインド、および scriptability 可能な他の macOS (旧 OS X) 機能にアクセスできるようになります。

UI 要素を設定して操作するために、Xamarin. Mac アプリケーションでキー値のコードとデータバインディングの手法を使用することにより、記述して維持する必要があるコードの量を大幅に減らすことができます。 また、フロントエンドのユーザーインターフェイス (_モデルビューコントローラー_) からバッキングデータ (_データモデル_) をさらに分離することもできます。これにより、管理が容易になり、アプリケーションの設計をより柔軟に行うことができます。

たとえば、KVC 準拠オブジェクトの次のクラス定義を見てみましょう。

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        public PersonModel ()
        {
        }
    }
}
```

まず、属性`[Register("PersonModel")]`はクラスを登録し、それを目的の C に公開します。 次に、クラスは、(また`NSObject`はから`NSObject`継承するサブクラス) から継承する必要があります。これにより、クラスを kvc 準拠にすることができるいくつかの基本メソッドが追加されます。 次に、 `[Export("Name")]`属性は`Name`プロパティを公開し、後で kvc および kvc 手法を使用してプロパティにアクセスするために使用されるキー値を定義します。

最後に、プロパティの値に対するキー値の観測された変更を可能にするために、アクセサーは、および`WillChangeValue` `DidChangeValue`メソッドの呼び出しの値に対する変更をラップ`Export`する必要があります (属性と同じキーを指定します)。  例えば:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

この手順は、この記事で後ほど説明するように、Xcode の Interface Builder でのデータバインディングでは_非常に_重要です。

詳細については、「Apple の[キー値コードプログラミングガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)」を参照してください。

### <a name="keys-and-key-paths"></a>キーとキーのパス

_キー_は、オブジェクトの特定のプロパティを識別する文字列です。 通常、キーは、キー値のコーディングに準拠したオブジェクトのアクセサーメソッドの名前に対応します。 キーは ASCII エンコードを使用する必要があり、通常は小文字で始まり、空白を含めることはできません。 上記の例では、 `Name`は`PersonModel`クラスのプロパティの`Name`キー値になります。 公開するプロパティのキーと名前は同じである必要はありませんが、ほとんどの場合は同じです。

_キーパス_は、走査するオブジェクトプロパティの階層を指定するために使用される、ドットで区切られたキーの文字列です。 シーケンス内の最初のキーのプロパティは受信側に対して相対的であり、後続の各キーは、前のプロパティの値に対して相対的に評価されます。 同様に、ドット表記を使用して、 C#クラス内のオブジェクトとそのプロパティを走査します。

たとえば、 `PersonModel`クラスを展開し、プロパティを追加`Child`した場合は、次のようになります。

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }

        public PersonModel ()
        {
        }
    }
}
```

子の名前`self.Child.Name`へのキーパスは、または ( `Child.Name`キー値がどのように使用されていたかに基づいて) 単純です。

### <a name="getting-values-using-key-value-coding"></a>キー値のコードを使用した値の取得

メソッド`ValueForKey`は、要求を受け取る kvc クラスのインスタンスに`NSString`対して、指定されたキーの値を (として) 返します。 たとえば、が上`Person`で定義された`PersonModel`クラスのインスタンスである場合は、次のようになります。

```csharp
// Read value
var name = Person.ValueForKey (new NSString("Name"));
```

これにより、の`Name` `PersonModel`インスタンスのプロパティの値が返されます。

### <a name="setting-values-using-key-value-coding"></a>キー値のコードを使用した値の設定

同様に、 `SetValueForKey`は、要求を受け取る kvc クラスのインスタンス`NSString`を基準として、指定されたキーの値をとして設定します。 次に示すように、 `PersonModel`クラスのインスタンスを使用します。

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

は、 `Name`プロパティの値をに`Jane Doe`変更します。

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>値の変更の観察

キー値の観察 (KVO) を使用すると、KVO 準拠クラスの特定のキーにオブザーバーをアタッチし、そのキーの値が変更されるたびに通知を受け取ることができます (KVO の手法をC#使用するか、コード内の特定のプロパティに直接アクセスします)。 例えば:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

これで、 `Name` `PersonModel`クラスの`Person`インスタンスのプロパティが変更されるたびに、新しい値がコンソールに書き込まれます。

詳細については、「[キー値の監視のプログラミングガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)」の Apple の概要を参照してください。

## <a name="data-binding"></a>データ バインディング

次のセクションでは、コードを使用して値の読み取りと書き込みを行うのではなく、キー値のコードとキー値の監視に準拠したクラスを使用C#して、Xcode の INTERFACE BUILDER の UI 要素にデータをバインドする方法について説明します。 この方法では、_データモデル_を表示するために使用されているビューとは別にして、Xamarin. Mac アプリケーションの柔軟性と保守性を高めることができます。 また、記述する必要があるコードの量を大幅に減らすことができます。

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>データモデルの定義

Interface Builder で UI 要素をデータバインドするには、その前に、バインドの_データモデル_として機能するように、Xamarin. Mac アプリケーションで kvc/kvc に準拠したクラスが定義されている必要があります。 データモデルは、ユーザーインターフェイスに表示されるすべてのデータを提供し、アプリケーションの実行中にユーザーが UI で行ったデータへの変更を受け取ります。

たとえば、従業員のグループを管理するアプリケーションを作成する場合は、次のクラスを使用してデータモデルを定義できます。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

このクラスのほとんどの機能については、上の「[キー値のコーディングとは](#What_is_Key-Value_Coding)」セクションで説明しました。 ただし、このクラスが**配列コントローラー**と**ツリーコントローラー**のデータモデルとして機能できるようにするために作成されたいくつかの特定の要素といくつかの追加について説明します (後でデータバインド**ツリービュー**、**アウトラインビューに使用します)。** および**コレクションビュー**)。

1つ目の理由は、従業員が上司である可能性が`NSArray`あるため、 `NSMutableArray` (具体的には、値を変更できるように) を使用して、自分が管理している従業員を関連付けることができるようにしました。

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

次の2つの点に注意してください。

1. これは、 `NSMutableArray` **テーブルビュー**、**アウトラインビュー** 、**コレクション**などの appkit コントロールにデータをバインドするための要件であるため、標準C#の配列またはコレクションではなくを使用していました。
2. 従業員の配列を公開する`NSArray`には、データバインディングのためにをにキャストし、 C#書式設定`People`された名前を、データバインディング`personModelArray`で想定さ**れる形式に**変更します (最初の文字が小文字になっています)。

次に、**配列コントローラー**と**ツリーコントローラー**をサポートするために、特別な名前のパブリックメソッドをいくつか追加する必要があります。

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

これにより、コントローラーは、表示されるデータを要求および変更できます。 上で公開`NSArray`されたのように、これらは非常に具体的な名前付けC#規則を持ちます (一般的な名前付け規則とは異なります)。

- `addObject:`-オブジェクトを配列に追加します。
- `insertObject:in{class_name}ArrayAtIndex:``{class_name}` -はクラスの名前です。 このメソッドは、指定されたインデックスの位置にあるオブジェクトを配列に挿入します。
- `removeObjectFrom{class_name}ArrayAtIndex:``{class_name}` -はクラスの名前です。 このメソッドは、配列内の指定したインデックス位置にあるオブジェクトを削除します。
- `set{class_name}Array:``{class_name}` -はクラスの名前です。 このメソッドを使用すると、既存のキャリーを新しいものに置き換えることができます。

これらのメソッドの内部では、の`WillChangeValue`配列に対する変更と`DidChangeValue` kvo 準拠のメッセージをラップしています。

最後に、プロパティ`Icon`は`isManager` `isManager`プロパティの値に依存しているため、プロパティへの変更は、データバインド`Icon`された UI 要素 (kvo 中) には反映されない可能性があります。

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
```

これを修正するには、次のコードを使用します。

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

独自のキー `isManager`に加えて、アクセサーも`Icon`キーのメッセージ`WillChangeValue`と`DidChangeValue`メッセージを送信するので、変更も表示されることに注意してください。

この記事の残りの`PersonModel`部分では、このデータモデルを使用します。

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>単純データ バインディング

データモデルを定義したので、Xcode の Interface Builder でのデータバインディングの簡単な例を見てみましょう。 たとえば、前に定義したを編集`PersonModel`するために使用できるフォームを Xamarin. Mac アプリケーションに追加してみましょう。 いくつかのテキストフィールドと、モデルのプロパティを表示および編集するためのチェックボックスを追加します。

まず、Interface Builder の**メインのストーリーボード**ファイルに新しい`SimpleViewController`**ビューコントローラー**を追加し、そのクラスにという名前を指定します。

[![新しいビューコントローラーの追加](databinding-images/simple01.png "新しいビューコントローラーの追加")](databinding-images/simple01-large.png#lightbox)

次に、Visual Studio for Mac に戻り、(プロジェクトに自動的に追加された) **SimpleViewController.cs**ファイルを編集して、 `PersonModel`フォームのデータバインド先となるのインスタンスを公開します。 次のコードを追加します。

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

次に、ビューが読み込まれたら、の`PersonModel`インスタンスを作成し、次のコードを入力します。

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

次に、フォームを作成し、**メインのストーリーボード**ファイルをダブルクリックして、Interface Builder で編集できるようにします。 次のようにフォームをレイアウトします。

[![Xcode でストーリーボードを編集する](databinding-images/simple02.png "Xcode でストーリーボードを編集する")](databinding-images/simple02-large.png#lightbox)

この`Person`キーを使用して公開`PersonModel`したにフォームをデータバインドするには、次の手順を実行します。

1. **[Employee Name]** テキストフィールドを選択し、[**バインド] インスペクター**に切り替えます。
2. **[バインド先]** ボックスをオンにし、ドロップダウンから **[簡易ビューコントローラー]** を選択します。 次に`self.Person.Name` 、**キーのパス**を入力します。

    [![キーのパスを入力する](databinding-images/simple03.png "キーのパスを入力する")](databinding-images/simple03-large.png#lightbox)
3. **[職業]** テキストフィールドを選択し、 **[バインド先]** ボックスをオンにして、ドロップダウンから **[簡易ビューコントローラー]** を選択します。 次に`self.Person.Occupation` 、**キーのパス**を入力します。

    [![キーのパスを入力する](databinding-images/simple04.png "キーのパスを入力する")](databinding-images/simple04-large.png#lightbox)
4. **[従業員はマネージャーで]** ある チェックボックスをオンにし、 **[バインド先]** チェックボックスをオンにして、ドロップダウンから **[簡易ビューコントローラー]** を選択します。 次に`self.Person.isManager` 、**キーのパス**を入力します。

    [![キーのパスを入力する](databinding-images/simple05.png "キーのパスを入力する")](databinding-images/simple05-large.png#lightbox)
5. **[Number Of Employees Managed]** Text フィールドを選択し、 **[バインド先]** ボックスをオンにして、ドロップダウンから **[簡易ビューコントローラー]** を選択します。 次に`self.Person.NumberOfEmployees` 、**キーのパス**を入力します。

    [![キーのパスを入力する](databinding-images/simple06.png "キーのパスを入力する")](databinding-images/simple06-large.png#lightbox)
6. 従業員がマネージャーでない場合は、Employees 管理対象ラベルとテキストフィールドの数を非表示にします。
7. **[Number Of Employees]** \ (従業員管理 \) ラベルを選択し、**非表示**の turndown を展開して **[バインド先]** ボックスをオンにし、ドロップダウンから **[簡易ビューコントローラー]** を選択します。 次に`self.Person.isManager` 、**キーのパス**を入力します。

    [![キーのパスを入力する](databinding-images/simple07.png "キーのパスを入力する")](databinding-images/simple07-large.png#lightbox)
8. [ `NSNegateBoolean` **値トランスフォーマー** ] ドロップダウンから次のように選択します。

    ![NSNegateBoolean キー変換の選択](databinding-images/simple08.png "NSNegateBoolean キー変換の選択")
9. これにより、 `isManager`プロパティの値が`false`の場合にラベルが非表示になることがデータバインディングに伝えられます。
10. [**従業員の管理対象の**テキスト] フィールドに対して、手順 7. および 8. を繰り返します。
11. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

アプリケーションを実行すると、 `Person`プロパティの値が自動的に設定されます。

[![自動入力されたフォームを表示する](databinding-images/simple09.png "自動入力されたフォームを表示する")](databinding-images/simple09-large.png#lightbox)

ユーザーがフォームに対して行った変更は、ビューコントローラーの`Person`プロパティに書き戻されます。 たとえば、Employee をオフにする`PersonModel`と、**マネージャーに**よっての`Person`インスタンスが更新され、[管理されているラベル] と [テキスト] フィールド**の数**は、データバインディングによって自動的に非表示になります。

[![管理者以外の従業員の数を非表示に]する(databinding-images/simple10.png "管理者以外の従業員の数を非表示に")する](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>テーブルビューのデータバインド

データバインディングの基本を説明したので、次は、_配列コントローラー_とテーブルビューへのデータバインディングを使用して、より複雑なデータバインディングタスクを見てみましょう。 テーブルビューの操作の詳細については、[テーブルビュー](~/mac/user-interface/table-view.md)のドキュメントを参照してください。

まず、Interface Builder の**メインのストーリーボード**ファイルに新しい`TableViewController`**ビューコントローラー**を追加し、そのクラスにという名前を指定します。

[![新しいビューコントローラーの追加](databinding-images/table01.png "新しいビューコントローラーの追加")](databinding-images/table01-large.png#lightbox)

次に、 **TableViewController.cs**ファイルを編集して (プロジェクトに自動的に追加されていた)、`NSArray`フォームを`PersonModel`データバインドするクラスの配列 () を公開します。 次のコードを追加します。

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

前の「[データモデルを定義](#Defining_your_Data_Model)する」セクションで説明した`PersonModels` `PersonModel`クラスと同様に、配列コントローラーとのコレクションからデータの読み取りと書き込みを行うために、4つの特別な名前のパブリックメソッドを公開しました。

次に、ビューが読み込まれたら、次のコードを使用して配列を設定する必要があります。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

ここで、テーブルビューを作成し、**メインのストーリーボード**ファイルをダブルクリックして、Interface Builder で編集するために開きます。 テーブルを次のようにレイアウトします。

[![新しいテーブルビューをレイアウトする](databinding-images/table02.png "新しいテーブルビューをレイアウトする")](databinding-images/table02-large.png#lightbox)

テーブルにバインドされたデータを提供する**配列コントローラー**を追加する必要があります。次の手順を実行します。

1. **ライブラリインスペクター**から**インターフェイスエディター**に**配列コントローラー**をドラッグします。

    ![ライブラリからの配列コントローラーの選択](databinding-images/table03.png "ライブラリからの配列コントローラーの選択")
2. **インターフェイス階層**で **[配列コントローラー]** を選択し、**属性インスペクター**に切り替えます。

    [![属性インスペクターの選択](databinding-images/table04.png "属性インスペクターの選択")](databinding-images/table04-large.png#lightbox)
3. `PersonModel` **クラス名**として「」と入力し、**プラス**ボタンをクリックして、3つのキーを追加します。 名前を、 `Occupation`次`isManager`のように指定します。 `Name`

    ![必要なキーパスを追加する](databinding-images/table05.png "必要なキーパスを追加する")
4. これにより、配列がどのように管理しているか、およびキーによって公開する必要があるプロパティが配列コントローラーに伝えられます。
5. **バインドインスペクター**に切り替え、 **[コンテンツ配列]** で **[バインド先]** と **[テーブルビューコントローラー]** を選択します。 **モデルキー**の`self.personModelArray`パスを入力してください:

    ![キーのパスを入力する](databinding-images/table06.png "キーのパスを入力する")
6. これにより、ビューコントローラーで公開さ`PersonModels`れたの配列に配列コントローラーが結び付けられます。

ここで、テーブルビューを配列コントローラーにバインドする必要があります。次の手順を実行します。

1. テーブルビューと**バインドインスペクター**を選択します。

    [![バインドインスペクターの選択](databinding-images/table07.png "バインドインスペクターの選択")](databinding-images/table07-large.png#lightbox)
2. テーブルの**内容**turndown の下で、[ **Bind To** and **Array Controller**] を選択します。 [ `arrangedObjects` **コントローラーキー** ] フィールドに「」と入力します。

    ![コントローラーキーの定義](databinding-images/table08.png "コントローラーキーの定義")
3. **[Employee]** 列の下にある**テーブルビューセル**を選択します。 Turndown の**値**の下にある [**バインド] インスペクター**で、[**テーブルセルビュー** **にバインド**] を選択します。 モデル`objectValue.Name`キーの**パス**として「」と入力します。

    [![モデルキーのパスを設定する](databinding-images/table09.png "モデルキーのパスを設定する")](databinding-images/table09-large.png#lightbox)
4. `objectValue`は、配列`PersonModel`コントローラーによって管理されている配列内の現在のです。
5. **[職業]** 列の下にある [**テーブルビュー] セル**を選択します。 Turndown の**値**の下にある [**バインド] インスペクター**で、[**テーブルセルビュー** **にバインド**] を選択します。 モデル`objectValue.Occupation`キーの**パス**として「」と入力します。

    [![モデルキーのパスを設定する](databinding-images/table10.png "モデルキーのパスを設定する")](databinding-images/table10-large.png#lightbox)
6. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

アプリケーションを実行すると、テーブルに次の`PersonModels`配列が格納されます。

[![アプリケーションの実行](databinding-images/table11.png "アプリケーションの実行")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>データバインディングのアウトラインビュー

アウトラインビューに対するデータバインディングは、テーブルビューに対するバインドとよく似ています。 主な違いは、**配列コントローラー**ではなく**ツリーコントローラー**を使用して、バインドされたデータをアウトラインビューに提供することです。 アウトラインビューの操作の詳細については、[アウトラインビュー](~/mac/user-interface/outline-view.md)のドキュメントを参照してください。

まず、Interface Builder の**メインのストーリーボード**ファイルに新しい`OutlineViewController`**ビューコントローラー**を追加し、そのクラスにという名前を指定します。

[![新しいビューコントローラーの追加](databinding-images/outline01.png "新しいビューコントローラーの追加")](databinding-images/outline01-large.png#lightbox)

次に、 **OutlineViewController.cs**ファイルを編集して (プロジェクトに自動的に追加されていた)、`NSArray`フォームを`PersonModel`データバインドするクラスの配列 () を公開します。 次のコードを追加します。

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

前の「[データモデルの定義](#Defining_your_Data_Model)」セクションで説明した`PersonModels` `PersonModel`クラスと同様に、4つの特別な名前のパブリックメソッドを公開しました。これにより、ツリーコントローラーとのコレクションからデータの読み取りと書き込みを行うことができます。

次に、ビューが読み込まれたら、次のコードを使用して配列を設定する必要があります。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

ここで、アウトラインビューを作成し、**メインのストーリーボード**ファイルをダブルクリックして、Interface Builder で編集するために開きます。 テーブルを次のようにレイアウトします。

[![アウトラインビューの作成](databinding-images/outline02.png "アウトラインビューの作成")](databinding-images/outline02-large.png#lightbox)

このアウトラインにバインドされたデータを提供するには、**ツリーコントローラー**を追加する必要があります。次の手順を実行します。

1. **ライブラリインスペクター**から**インターフェイスエディター**に**ツリーコントローラー**をドラッグします。

    ![ライブラリからツリーコントローラーを選択する](databinding-images/outline03.png "ライブラリからツリーコントローラーを選択する")
2. **インターフェイス階層**で **[ツリーコントローラー]** を選択し、**属性インスペクター**に切り替えます。

    [![属性インスペクターの選択](databinding-images/outline04.png "属性インスペクターの選択")](databinding-images/outline04-large.png#lightbox)
3. `PersonModel` **クラス名**として「」と入力し、**プラス**ボタンをクリックして、3つのキーを追加します。 名前を、 `Occupation`次`isManager`のように指定します。 `Name`

    ![必要なキーパスを追加する](databinding-images/outline05.png "必要なキーパスを追加する")
4. これにより、ツリーコントローラーがどのように配列を管理しているか、および (キーを使用して) 公開する必要があるプロパティが示されます。
5. [**ツリーコントローラー** `personModelArray` ] セクションで、 **[子]** `NumberOfEmployees`に「」と入力し、 **Count**の下に「」と入力して、 `isEmployee` **リーフ**の下に「」と入力します

    ![ツリーコントローラーのキーパスの設定](databinding-images/outline05.png "ツリーコントローラーのキーパスの設定")
6. これにより、子ノードを検索する場所、子ノードの数、現在のノードに子ノードがあるかどうかがツリーコントローラーに示されます。
7. **バインドインスペクター**に切り替え、 **[コンテンツ配列]** で、 **[バインド先]** と **[ファイルの所有者]** を選択します。 **モデルキー**の`self.personModelArray`パスを入力してください:

    ![キーのパスを編集する](databinding-images/outline06.png "キーのパスを編集する")
8. これにより、ビューコントローラーで公開し`PersonModels`たの配列に、ツリーコントローラーが関連付けられます。

ここで、アウトラインビューをツリーコントローラーにバインドし、次の手順を実行します。

1. [アウトライン] ビューを選択し、**バインドインスペクター**で次のように選択します。

    [![バインドインスペクターの選択](databinding-images/outline07.png "バインドインスペクターの選択")](databinding-images/outline07-large.png#lightbox)
2. **アウトラインビュー**の コンテンツ turndown で、**バインド先** と **ツリーコントローラー** を選択します。 [ `arrangedObjects` **コントローラーキー** ] フィールドに「」と入力します。

    ![コントローラーキーの設定](databinding-images/outline08.png "コントローラーキーの設定")
3. **[Employee]** 列の下にある**テーブルビューセル**を選択します。 Turndown の**値**の下にある [**バインド] インスペクター**で、[**テーブルセルビュー** **にバインド**] を選択します。 モデル`objectValue.Name`キーの**パス**として「」と入力します。

    [![モデルキーパスの入力](databinding-images/outline09.png "モデルキーパスの入力")](databinding-images/outline09-large.png#lightbox)
4. `objectValue`は、ツリー `PersonModel`コントローラーによって管理されている配列内の現在のです。
5. **[職業]** 列の下にある [**テーブルビュー] セル**を選択します。 Turndown の**値**の下にある [**バインド] インスペクター**で、[**テーブルセルビュー** **にバインド**] を選択します。 モデル`objectValue.Occupation`キーの**パス**として「」と入力します。

    [![モデルキーパスの入力](databinding-images/outline10.png "モデルキーパスの入力")](databinding-images/outline10-large.png#lightbox)
6. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

アプリケーションを実行すると、の`PersonModels`配列がアウトラインに設定されます。

[![アプリケーションの実行](databinding-images/outline11.png "アプリケーションの実行")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>コレクションビューのデータバインディング

コレクションビューを使用したデータバインディングは、コレクションにデータを提供するために配列コントローラーが使用されるため、テーブルビューとのバインドとよく似ています。 コレクションビューには事前設定された表示形式がないため、ユーザーの操作に関するフィードバックを提供し、ユーザーの選択を追跡するために、より多くの作業が必要になります。

> [!IMPORTANT]
> Xcode 7 および macOS 10.11 (以降) の問題により、コレクションビューはストーリーボード (storyboard) ファイルの内部では使用できません。 そのため、xib ファイルを引き続き使用して、Xamarin. Mac アプリのコレクションビューを定義する必要があります。 詳細については、[コレクションビュー](~/mac/user-interface/collection-view.md)のドキュメントを参照してください。

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`:

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1. **Collection View Item** -  That manages a single instance of an item in the collection.
2. **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>ネイティブクラッシュのデバッグ

データバインディングに誤りを加えると、アンマネージコードで_ネイティブクラッシュ_が発生し、Xamarin. Mac アプリケーションが`SIGABRT`エラーで完全に失敗する可能性があります。

[![ネイティブクラッシュダイアログボックスの例](databinding-images/debug01.png "ネイティブクラッシュダイアログボックスの例")](databinding-images/debug01-large.png#lightbox)

通常、データバインディング中のネイティブクラッシュには、主に次の4つの原因があります。

1. データモデルは、またはの`NSObject` `NSObject`サブクラスから継承されません。
2. `[Export("key-name")]`属性を使用して、プロパティを目的の C に公開していません。
3. および`WillChangeValue` `Export`メソッドの呼び出しでアクセサーの値の変更をラップしていません (属性と同じキーを指定しています)。 `DidChangeValue`
4. Interface Builder の**バインドインスペクター**に間違ったキーまたは入力ミスのキーがあります。

### <a name="decoding-a-crash"></a>クラッシュのデコード

データバインディングでネイティブクラッシュが発生しているので、それを見つけて修正する方法を示すことができます。 Interface Builder では、コレクションビューの最初のラベルのバインドをから`Name`に`Title`変更してみましょう。

[![バインドキーの編集](databinding-images/debug02.png "バインドキーの編集")](databinding-images/debug02-large.png#lightbox)

変更を保存し、Visual Studio for Mac に切り替えて、Xcode と同期し、アプリケーションを実行してみましょう。 コレクションビューが表示されると`SIGABRT` 、がキー `Title`を使用してプロパティを公開し`PersonModel`ないため、アプリケーションは、(Visual Studio for Mac の**アプリケーション出力**に示されているように) エラーを一時的にクラッシュします。

[![バインドエラーの例](databinding-images/debug03.png "バインドエラーの例")](databinding-images/debug03-large.png#lightbox)

**アプリケーション出力**のエラーの一番上までスクロールすると、問題を解決するためのキーが表示されます。

[![エラーログで問題を見つける](databinding-images/debug04.png "エラーログで問題を見つける")](databinding-images/debug04-large.png#lightbox)

この行は、バインド先のオブジェクト`Title`にキーが存在しないことを示しています。 バインドを Interface Builder に変更し、 `Name`保存、同期、リビルド、および実行すると、アプリケーションは問題なく動作するようになります。

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでのデータバインディングとキー値のコーディングの使用について詳しく説明しました。 まず、キー値のコード ( C# kvc) とキー値の観測 (kvc) を使用して、目的の C にクラスを公開する方法を見てきました。 次に、KVO に準拠したクラスを使用し、データを Xcode の Interface Builder の UI 要素にバインドする方法について説明しました。 最後に、**配列コントローラー**と**ツリーコントローラー**を使用した複雑なデータバインディングについて説明しました。


## <a name="related-links"></a>関連リンク

- [MacDatabinding ストーリーボード (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-storyboard)
- [MacDatabinding Xib (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-xibs)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [標準コントロール](~/mac/user-interface/standard-controls.md)
- [テーブルビュー](~/mac/user-interface/table-view.md)
- [アウトラインビュー](~/mac/user-interface/outline-view.md)
- [コレクションビュー](~/mac/user-interface/collection-view.md)
- [キー値のコーディングのプログラミングガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [キー値観察のプログラミングガイドの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Cocoa バインディングの概要プログラミングに関するトピック](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa バインドリファレンスの概要](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
