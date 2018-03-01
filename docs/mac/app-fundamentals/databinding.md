---
title: "データのバインドとキー値のコーディング"
description: "この記事では、キーと値のコーディングおよびキーと値の Xcode のインターフェイスのビルダーでの UI 要素にデータ バインドを許可する確認の使用方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 2c01a36eabb15fbe9b975c91328dfa7cfd651896
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="data-binding-and-key-value-coding"></a>データのバインドとキー値のコーディング

_この記事では、キーと値のコーディングおよびキーと値の Xcode のインターフェイスのビルダーでの UI 要素にデータ バインドを許可する確認の使用方法について説明します。_

## <a name="overview"></a>概要

同じキーと値のコーディングおよびデータ バインディング技術へのアクセスがある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときをで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_データは、コードを記述する代わりに UI 要素にバインドします。

キーと値のコーディングおよびでのデータ バインディング技術 Xamarin.Mac アプリケーションを使用するの記述し、保守を作成し、UI 要素を使用する必要のあるコードの量を大幅に削減できます。 また、バックアップ データをさらに分離することの利点がある場合 (_データ モデル_)、正面からのユーザー インターフェイスを終了 (_モデル-ビュー-コント ローラー_) より柔軟なアプリケーションの保守が容易に先行します。デザインします。

[![実行中のアプリの使用例](databinding-images/intro01.png "実行中のアプリの例")](databinding-images/intro01-large.png)

この記事でキーと値のコーディングおよび Xamarin.Mac アプリケーションでのデータ バインディングの操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での c# クラスを OBJECTIVE-C オブジェクトと UI への要素に使用されます。

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>コーディングのキー値とは

アクセサー メソッドまたはインスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) を使用して、オブジェクトのプロパティを直接アクセスするためのメカニズムは、キーと値 (KVC) のコーディング (`get/set`)。 Xamarin.Mac アプリケーションでは、キーと値のコーディング準拠アクセサーを実装、により、キーと値観測 (KVO)、データ バインディング、基本データ、Cocoa バインディング、および scriptability などの他の macOS (旧称 OS X) 機能にアクセスできます。

キーと値のコーディングおよびでのデータ バインディング技術 Xamarin.Mac アプリケーションを使用するの記述し、保守を作成し、UI 要素を使用する必要のあるコードの量を大幅に削減できます。 また、バックアップ データをさらに分離することの利点がある場合 (_データ モデル_)、正面からのユーザー インターフェイスを終了 (_モデル-ビュー-コント ローラー_) より柔軟なアプリケーションの保守が容易に先行します。デザインします。 

たとえば、KVC 準拠しているオブジェクトの次のクラス定義を見てみましょう。

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

最初に、`[Register("PersonModel")]`属性クラスを登録し、目標 C. に公開します。 次に、クラスから継承する必要があります。 `NSObject` (またはそのサブクラスから継承する`NSObject`)、いくつかの基本 KVC 準拠にするクラスを許可するメソッドが追加されます。 次に、`[Export("Name")]`属性の公開、`Name`プロパティをプロパティにアクセスする、KVC と KVO の手法を後で使用されるキー値を定義します。 

最後に、プロパティの値に変更のキー値測定するには、アクセサー ラップする必要がありますにはその値への変更`WillChangeValue`と`DidChangeValue`メソッドの呼び出し (と同じキーを指定する、`Export`属性)。  例:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

この手順は_非常に_Xcode でのデータ バインディングの重要なのインターフェイスのビルダー (ようにこの記事で後述)。

詳細については、Apple を参照してください[キーと値のコーディング プログラミング ガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)です。

### <a name="keys-and-key-paths"></a>キーとキーのパス

A_キー_オブジェクトの特定のプロパティを識別する文字列です。 通常、キーは、準拠しているオブジェクトのコーディングのキーと値のアクセサー メソッドの名前に対応します。 キーが ASCII エンコーディングを使用する必要があります通常は小文字アルファベットで始まり、空白文字を含めることはできません。 上記の例を指定された`Name`のキー値になります`Name`のプロパティ、`PersonModel`クラスです。 ただしはほとんどの場合、キーとそれが公開するプロパティの名前が、同じにするがありません。

A_キー パス_はドットの文字列を走査するオブジェクトのプロパティの階層の指定に使用するキーで区切って指定します。 シーケンス内の最初のキーのプロパティは、受信側に対する相対あり、それぞれの後続のキーが前のプロパティの値を基準として評価されます。 同じ方法では、c# クラス内でそのプロパティとオブジェクトを走査するのにドット表記を使用します。

たとえば、展開した場合、`PersonModel`クラスし、追加`Child`プロパティ。

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

お子様の名前のキー パスになります`self.Child.Name`または単に`Child.Name`(キー値が使用されている方法に基づく)。

### <a name="getting-values-using-key-value-coding"></a>コーディングのキー値を使用して値を取得します。

`ValueForKey`メソッドは、指定されたキーの値を返します (として、 `NSString`)、要求を受信 KVC クラスのインスタンスに相対パスです。 たとえば場合、`Person`のインスタンス、`PersonModel`上記で定義されたクラス。

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

値が返されます、`Name`のインスタンスをプロパティ`PersonModel`です。 

### <a name="setting-values-using-key-value-coding"></a>コーディングのキー値を使用して値の設定

同様に、`SetValueForKey`指定したキーの値を設定 (として、 `NSString`)、要求を受信 KVC クラスのインスタンスに相対パスです。 ここでものインスタンスを使用して、`PersonModel`クラス、次のようにします。

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

値を変更、`Name`プロパティを`Jane Doe`です。

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>値の変更を確認します。

キーと値 (KVO) を観察しを使用して、オブザーバーを KVC 準拠クラスの特定のキーに関連付けることができます、いつでも (KVC 手法を使用して、または c# コードで指定されたプロパティに直接アクセスする)、そのキーの値が変更された通知。 例:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

ここで、いつでも、`Name`のプロパティ、`Person`のインスタンス、`PersonModel`クラスを変更すると、新しい値コンソールに出力します。 

詳細については、Apple を参照してください[キーと値を観察しプログラミング ガイドの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)です。

## <a name="data-binding"></a>データ バインディング

次のセクションでは、コーディングのキー値とキー値が準拠しているクラスを観察しを使用して、読み取りと書き込みの c# コードを使用して値の代わりに、Xcode のインターフェイスのビルダーでの UI 要素にデータをバインドする方法に表示されます。 この方法で区別する、_データ モデル_それらを表示するために使用するビューからアプリケーションを作成する、Xamarin.Mac より柔軟かつ容易に管理します。 書き込まれる必要のあるコードの量も大幅に低下します。

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>データ モデルを定義します。

Xamarin.Mac として動作するアプリケーションで定義されている KVC/KVO 準拠しているクラスがある必要がある前にデータ バインド インターフェイスのビルダーでの UI 要素を_データ モデル_バインドにします。 データ モデルは、すべてのユーザー インターフェイスに表示されますで、ユーザーがアプリケーションの実行中に、ui がデータに変更を加えるを送受信するデータを提供します。

たとえば、従業員のグループが管理されるアプリケーションを作成していた場合は、データ モデルを定義する次のクラスを使用します。

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

このクラスの機能のほとんどで説明した、[キーと値のコーディングは](#What_is_Key-Value_Coding)前のセクションです。 ただし、いくつかの特定の要素とのデータ モデルとして機能するには、このクラスの許可に加えられたいくつかの追加機能を見てみましょう**配列コント ローラー**と**ツリー コント ローラー** (これは、データを後で使用します。バインド**ツリー ビュー**、**アウトライン ビュー**と**コレクション ビュー**)。

最初であるため、従業員がマネージャーを使用して、 `NSArray` (具体的には、`NSMutableArray`値を変更できるように) を添付することを管理する社員を許可します。

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

2 つの処理には、ここに注意してください:

1. 使用して、 `NSMutableArray` 、c# の配列またはコレクション AppKit コントロールにデータをバインドする必要はなどのための標準ではなく**テーブル ビュー**、**アウトライン ビュー**と**コレクション**.
2. キャストにして、従業員の配列を公開お、`NSArray`書式指定済みの名前のデータ バインディングのため、変更、c# `People`、データ バインディングが必要ですが、その`personModelArray`形式で**{class_name} 配列**(注意してください最初の文字が行われたこと小文字のみ)。

次に、いくつか特別な名前公開をサポートするメソッドを追加する必要があります**配列コント ローラー**と**ツリー コント ローラー**:

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

これにより、コント ローラーを要求し、それらを表示するデータを変更できます。 などの公開されている`NSArray`非常に特定の名前付け規則 (つまり、一般的な c# の名前付け規則とは異なります) があるこれら上記。

- `addObject:` -配列にオブジェクトを追加します。
- `insertObject:in{class_name}ArrayAtIndex:` -場所`{class_name}`クラスの名前を指定します。 このメソッドは、オブジェクトを配列の指定したインデックス位置に挿入します。
- `removeObjectFrom{class_name}ArrayAtIndex:` -場所`{class_name}`クラスの名前を指定します。 このメソッドは、配列の指定したインデックスにあるオブジェクトを削除します。
- `set{class_name}Array:` -場所`{class_name}`クラスの名前を指定します。 このメソッドを使用すると、既存の実行を新しいものに置き換えます。

これらのメソッドの内部では、配列に変更をラップした`WillChangeValue`と`DidChangeValue`KVO コンプライアンスのためのメッセージ。

最後に、以降、`Icon`プロパティの値に依存、 `isManager` 、プロパティに対する変更を`isManager`プロパティに反映されない可能性があります、 `Icon` (KVO) 中にデータ バインドされている UI 要素の。

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

それを修正するには、次のコードを使用します。

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

注意して、独自のキーだけでなく、`isManager`アクセサーは送信も、`WillChangeValue`と`DidChangeValue`のメッセージの`Icon`も変更が表示されるようにキーします。

使用する、`PersonModel`この記事の残りの部分のデータ モデル。

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>単純データ バインディング

モデルでは、データ定義、Xcode のインターフェイスのビルダーでのデータ バインディングの簡単な例を見てみましょう。 たとえば、Xamarin.Mac アプリケーションを編集するために使用するのにフォームを追加してみましょう、`PersonModel`上に定義しました。 モデルのプロパティを表示および編集には、いくつかのテキスト フィールドとチェック ボックスを追加します。

最初に、新しく追加してみましょう**ビュー コント ローラー**を弊社**Main.storyboard**インターフェイス ビルダー内のファイルとそのクラスの名前を`SimpleViewController`:。 

[![新しいビュー コント ローラーを追加する](databinding-images/simple01.png "新しいビュー コント ローラーを追加します。")](databinding-images/simple01-large.png)

次に、Mac を Visual Studio に戻り、編集、 **SimpleViewController.cs** (つまり、プロジェクトに自動的に追加された) ファイルのインスタンスを公開し、`PersonModel`おになるデータをフォームをバインドします。 次のコードを追加します。

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

次に、ビューが読み込まれるときにみましょうのインスタンスを作成、`PersonModel`し、次のコードを設定します。

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

フォームを作成する必要があります、これをダブルクリックして、 **Main.storyboard**編集のためインターフェイス ビルダーで開くファイル。 次のように表示するフォームのレイアウト:

[![Xcode でストーリー ボードの編集](databinding-images/simple02.png "Xcode でストーリー ボードの編集")](databinding-images/simple02-large.png)

データ バインド フォームを`PersonModel`を介して公開されることを`Person`キーを次の操作します。

1. 選択、 **Employee Name**テキスト フィールドに切り替えると、**バインド インスペクター**です。
2. チェック、**バインド**ボックスし、**ビューの単純なコント ローラー**ドロップダウン リストからです。 次に入力`self.Person.Name`の**キー パス**: 

    [![キーのパスを入力する](databinding-images/simple03.png "キーのパスを入力します。")](databinding-images/simple03-large.png)
3. 選択、**職業**テキスト フィールドおよびチェック、**にバインド**ボックスし、選択**ビューの単純なコント ローラー**ドロップダウン リストからです。 次に入力`self.Person.Occupation`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple04.png "キーのパスを入力します。")](databinding-images/simple04-large.png)
4. 選択、**従業員がマネージャー**  チェック ボックスを確認し、**にバインド**ボックスし、**ビューの単純なコント ローラー**ドロップダウン リストからです。 次に入力`self.Person.isManager`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple05.png "キーのパスを入力します。")](databinding-images/simple05-large.png)
5. 選択、**管理された従業員の数**テキスト フィールドおよびチェック、**にバインド**ボックスし、選択**ビューの単純なコント ローラー**ドロップダウン リストからです。 次に入力`self.Person.NumberOfEmployees`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple06.png "キーのパスを入力します。")](databinding-images/simple06-large.png)
6. 数の従業員管理のラベルとテキスト フィールドを非表示にたい場合は、従業員がマネージャーではありません。
7. 選択、**管理された従業員の数**ラベル、展開、 **Hidden** turndown とチェック、**にバインド**ボックスし、選択**単純なビュー コント ローラー**ドロップダウン リストからです。 次に入力`self.Person.isManager`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple07.png "キーのパスを入力します。")](databinding-images/simple07-large.png)
8. 選択`NSNegateBoolean`から、**値トランスフォーマー**ドロップダウンします。  

    ![NSNegateBoolean キーの変換を選択すると](databinding-images/simple08.png "NSNegateBoolean キーの変換を選択します。")
9. データ バインディングを示すこの場合、ラベルを非表示にすることの値、`isManager`プロパティは`false`します。
10. 手順 7 と 8 を繰り返します、**数の従業員マネージ**テキスト フィールド。
11. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

値をアプリケーションを実行する場合、`Person`プロパティは、フォームを自動的に設定します。

[![自動設定されたフォームを示す](databinding-images/simple09.png "自動設定されたフォームを表示")](databinding-images/simple09-large.png)

ユーザーがフォームに変更を加えた書き戻されますを`Person`ビュー コント ローラー内のプロパティです。 たとえば、選択を解除する**従業員がマネージャー**更新プログラム、`Person`のインスタンス、`PersonModel`と**管理された従業員の数**ラベルとテキスト フィールドが自動的に (経由で非表示データ バインディングの場合):

[![管理者以外のメンバーの従業員の数を非表示にする](databinding-images/simple10.png "管理者以外のメンバーの従業員の数を非表示にします。")](databinding-images/simple10-large.png)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>テーブル ビューのデータ バインディング

これでがデータ バインドを邪魔の基礎、見てみましょうより複雑なデータ バインドのタスクを使用して、_配列コント ローラー_およびテーブル ビューにデータをバインドします。 テーブルのビューの操作の詳細についてを参照してください、[テーブル ビュー](~/mac/user-interface/table-view.md)ドキュメント。

最初に、新しく追加してみましょう**ビュー コント ローラー**を弊社**Main.storyboard**インターフェイス ビルダー内のファイルとそのクラスの名前を`TableViewController`:。

[![新しいビュー コント ローラーを追加する](databinding-images/table01.png "新しいビュー コント ローラーを追加します。")](databinding-images/table01-large.png)

次に、編集しましょう、 **TableViewController.cs**ファイル (つまり、プロジェクトに自動的に追加された) と配列を公開して (`NSArray`) の`PersonModel`おなるはデータ バインディングをフォームのクラスです。 次のコードを追加します。

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

上と同様、`PersonModel`クラス上の[、データ モデルを定義する](#Defining_your_Data_Model) セクションで、特別に指定された 4 つのパブリック メソッドを公開したおように配列コント ローラーおよび読み取りと書き込みのデータのコレクションを`PersonModels`です。

次に、ビューが読み込まれるときにこのコードを持つ配列を設定する必要があります。

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

テーブル、ビューを作成する必要があります、これをダブルクリックして、 **Main.storyboard**編集のためインターフェイス ビルダーで開くファイル。 次のように表示するテーブルのレイアウト:

[![新しい表形式ビューのレイアウト](databinding-images/table02.png "新しい表形式ビューのレイアウト")](databinding-images/table02-large.png)

追加する必要があります、**配列コント ローラー**このテーブルにバインドされたデータを提供する、次の操作します。

1. ドラッグ、**配列コント ローラー**から、**ライブラリ インスペクター**上に、**インターフェイス エディター**:  

    ![ライブラリから配列コント ローラーを選択すると](databinding-images/table03.png "ライブラリから配列コント ローラーを選択します。")
2. 選択**配列コント ローラー**で、**インターフェイス階層**に切り替えると、**属性インスペクター**:  

    [![属性のインスペクターを選択すると](databinding-images/table04.png "属性インスペクターを選択します。")](databinding-images/table04-large.png)
3. 入力`PersonModel`の**クラス名**、 をクリックして、 **Plus**ボタンをクリックし、3 つのキーを追加します。 名前を付けます`Name`、`Occupation`と`isManager`:  

    ![必須のキーのパスを追加する](databinding-images/table05.png "必須のキーのパスを追加します。")
4. 新機能は、管理の配列、配列コント ローラーに指示このありプロパティ (キー) を使用して公開する必要があります。
5. 切り替えて、**バインド インスペクター**し、**コンテンツ配列**選択**にバインド**と**テーブル ビューのコント ローラー**です。 入力、**キーのパスをモデル**の`self.personModelArray`:  

    ![キーのパスを入力する](databinding-images/table06.png "キーのパスを入力します。")
6. これにより、関連付けの配列を配列コント ローラー`PersonModels`ビューのコント ローラーは公開されています。

配列コント ローラーに、テーブル ビューをバインドする必要があります、これは、次の操作を行います。

1. テーブル ビューを選択し、**インスペクターをバインド**:  

    [![バインディング インスペクターを選択すると](databinding-images/table07.png "バインディング インスペクターを選択します。")](databinding-images/table07-large.png)
2. 下にある、**テーブルの内容**turndown、select**バインド**と**配列コント ローラー**です。 入力`arrangedObjects`の**コント ローラー キー**フィールド。  

    ![コント ローラーのキーを定義する](databinding-images/table08.png "コント ローラーのキーを定義します。")
3. 選択、**テーブル ビューのセル**下にある、**従業員**列です。 **バインド インスペクター**下にある、**値**turndown、select**にバインド**と**テーブル セル ビュー**です。 入力`objectValue.Name`の**キーのパスをモデル**:  

    [![モデルのキー パスを設定して](databinding-images/table09.png "モデルのキー パスを設定します。")](databinding-images/table09-large.png)
4. `objectValue` 現在`PersonModel`配列コント ローラーによって管理されている配列。
5. 選択、**テーブル ビューのセル**下にある、**職業**列です。 **バインド インスペクター**下にある、**値**turndown、select**にバインド**と**テーブル セル ビュー**です。 入力`objectValue.Occupation`の**キーのパスをモデル**:  

    [![モデルのキー パスを設定して](databinding-images/table10.png "モデルのキー パスを設定します。")](databinding-images/table10-large.png)
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

配列で、テーブルにデータを設定して、アプリケーションを実行する場合`PersonModels`:

[![アプリケーションを実行している](databinding-images/table11.png "アプリケーションを実行します。")](databinding-images/table11-large.png)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>アウトライン表示のデータ バインディング

アウトライン ビューに対してデータ バインドは、テーブル ビューに対するバインドとよく似ています。 主な違いは、使用すること、**ツリー コント ローラー**の代わりに、**配列コント ローラー**をアウトライン表示にバインドされたデータを提供します。 アウトライン ビューの使用の詳細についてを参照してください、[アウトライン ビュー](~/mac/user-interface/outline-view.md)ドキュメント。

最初に、新しく追加してみましょう**ビュー コント ローラー**を弊社**Main.storyboard**インターフェイス ビルダー内のファイルとそのクラスの名前を`OutlineViewController`:。 

[![新しいビュー コント ローラーを追加する](databinding-images/outline01.png "新しいビュー コント ローラーを追加します。")](databinding-images/outline01-large.png)

次に、編集しましょう、 **OutlineViewController.cs**ファイル (つまり、プロジェクトに自動的に追加された) と配列を公開して (`NSArray`) の`PersonModel`おなるはデータ バインディングをフォームのクラスです。 次のコードを追加します。

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

上と同様、`PersonModel`クラス上の[、データ モデルを定義する](#Defining_your_Data_Model) セクションで、特別に指定された 4 つのパブリック メソッドを公開したおように、ツリーのコント ローラーおよび読み取りと書き込みのデータのコレクションから`PersonModels`です。

次に、ビューが読み込まれるときにこのコードを持つ配列を設定する必要があります。

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

当社のアウトライン ビューを作成する必要があります、これをダブルクリックして、 **Main.storyboard**編集のためインターフェイス ビルダーで開くファイル。 次のように表示するテーブルのレイアウト:

[![アウトライン表示を作成して](databinding-images/outline02.png "アウトライン表示を作成します。")](databinding-images/outline02-large.png)

追加する必要があります、**ツリー コント ローラー**アウトラインにバインドされたデータを提供する、次の操作します。

1. ドラッグ、**コント ローラーのツリー**から、**ライブラリ インスペクター**上に、**インターフェイス エディター**:  

    ![ライブラリからツリー コント ローラーを選択すると](databinding-images/outline03.png "ライブラリからツリー コント ローラーを選択します。")
2. 選択**ツリー コント ローラー**で、**インターフェイス階層**に切り替えると、**属性インスペクター**:  

    [![属性のインスペクターを選択すると](databinding-images/outline04.png "属性インスペクターを選択します。")](databinding-images/outline04-large.png)
3. 入力`PersonModel`の**クラス名**、 をクリックして、 **Plus**ボタンをクリックし、3 つのキーを追加します。 名前を付けます`Name`、`Occupation`と`isManager`:  

    ![必須のキーのパスを追加する](databinding-images/outline05.png "必須のキーのパスを追加します。")
4. こうこと、ツリーのコント ローラーとは、管理の配列およびプロパティ (キー) を介した公開が必要があります。
5. 下にある、**ツリー コント ローラー**セクションで、入力`personModelArray`の**子**、入力`NumberOfEmployees`下にある、**カウント**を入力し、 `isEmployee` **リーフ**:  

    ![ツリーのコント ローラーのキーのパスを設定](databinding-images/outline05.png "ツリー コント ローラーのキー パスの設定")
6. これは、ツリーのコント ローラーに、ノードがある子ノードの数は、および現在のノードに子ノードがあるかどうかは、すべての子を検索する場所を示しています。
7. 切り替えて、**バインド インスペクター**し、**コンテンツ配列**選択**にバインド**と**ファイルの所有者**です。 入力、**キーのパスをモデル**の`self.personModelArray`:  

    ![キーのパスを編集](databinding-images/outline06.png "キー パスの編集")
8. これにより、関連付けの配列に、ツリーのコント ローラー`PersonModels`ビューのコント ローラーは公開されています。

ツリーのコント ローラーに、アウトライン表示をバインドする必要があります、これは、次の操作を行います。

1. アウトライン表示を選択し、、**インスペクターのバインド**を選択します。  

    [![バインディング インスペクターを選択すると](databinding-images/outline07.png "バインディング インスペクターを選択します。")](databinding-images/outline07-large.png)
2. 下にある、**内容のアウトライン表示**turndown、select**にバインド**と**ツリー コント ローラー**です。 入力`arrangedObjects`の**コント ローラー キー**フィールド。  

    ![コント ローラーのキーを設定する](databinding-images/outline08.png "コント ローラーのキーを設定します。")
3. 選択、**テーブル ビューのセル**下にある、**従業員**列です。 **バインド インスペクター**下にある、**値**turndown、select**にバインド**と**テーブル セル ビュー**です。 入力`objectValue.Name`の**キーのパスをモデル**:  

    [![モデルのキーのパスを入力する](databinding-images/outline09.png "モデルのキーのパスを入力します。")](databinding-images/outline09-large.png)
4. `objectValue` 現在`PersonModel`ツリー コント ローラーによって管理されている配列。
5. 選択、**テーブル ビューのセル**下にある、**職業**列です。 **バインド インスペクター**下にある、**値**turndown、select**にバインド**と**テーブル セル ビュー**です。 入力`objectValue.Occupation`の**キーのパスをモデル**:  

    [![モデルのキーのパスを入力する](databinding-images/outline10.png "モデルのキーのパスを入力します。")](databinding-images/outline10-large.png)
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

アプリケーションを実行している場合、アウトラインが設定されます、配列の`PersonModels`:

[![アプリケーションを実行している](databinding-images/outline11.png "アプリケーションを実行します。")](databinding-images/outline11-large.png)

### <a name="collection-view-data-binding"></a>コレクション ビューのデータ バインディング

データ バインディング コレクション ビューにはテーブル ビューでは、バインドとよく似て配列コント ローラーは、データ コレクションを提供するために使用します。 コレクション ビューが既定の表示形式があるないためより多くの作業はユーザー操作のフィードバックを提供し、ユーザーの選択を追跡するために必要です。

> [!IMPORTANT]
> 問題のため、Xcode 7 および macOS 10.11 (以降) で、コレクション ビューでは、ストーリー ボード (.storyboard) ファイル内で使用できません。 その結果、引き続き Xamarin.Mac アプリに、コレクション ビューの定義に .xib ファイルを使用する必要があります。 参照してください、[コレクション ビュー](~/mac/user-interface/collection-view.md)詳細についてはドキュメントです。

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

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

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

## <a name="debugging-native-crashes"></a>ネイティブのクラッシュをデバッグ

データ バインドに間違いを犯す可能性、_ネイティブ クラッシュ_でアンマネージ コードと、Xamarin.Mac アプリケーションで完全に失敗する、`SIGABRT`エラー。

[![ネイティブのクラッシュ ダイアログ ボックスの例](databinding-images/debug01.png "ネイティブ クラッシュ ダイアログ ボックスの例")](databinding-images/debug01-large.png)

データ バインド中には通常するクラッシュをネイティブの 4 つの主要な原因があります。

1. データ モデルがから継承していない`NSObject`またはそのサブクラスの`NSObject`します。
2. Objective C を使用するプロパティが公開されていません、`[Export("key-name")]`属性。
3. アクセサーの値に変更をラップしなかった`WillChangeValue`と`DidChangeValue`メソッドの呼び出し (と同じキーを指定する、`Export`属性)。
4. 正しくないか、入力の間違いキーがある、**バインド インスペクター**インターフェイスのビルダー。

### <a name="decoding-a-crash"></a>クラッシュのデコード

みましょうを探して、その修正方法を説明できるように、データ バインディングでネイティブのクラッシュが発生します。 インターフェイスのビルダーからコレクション ビューの例の最初のラベルのバインディングを変更してみましょう`Name`に`Title`:

[![バインド キーを編集](databinding-images/debug02.png "バインド キーを編集")](databinding-images/debug02-large.png)

みましょう変更を保存、Xcode と同期し、アプリケーションを実行する Mac 用の Visual Studio に戻ります。 アプリケーションがすぐにクラッシュし、コレクション ビューが表示される場合、`SIGABRT`エラー (で示すように、**アプリケーション出力**Mac 用の Visual Studio で) ため、`PersonModel`キーを持つプロパティを公開しません`Title`:

[![バインディング エラーの例](databinding-images/debug03.png "バインディング エラーの例")](databinding-images/debug03-large.png)

内のエラーの最上部までスクロールする場合、**アプリケーション出力**お問題を解決するためのキーを参照してください。

[![エラー ログで問題を発見して](databinding-images/debug04.png "エラー ログで問題を検索します。")](databinding-images/debug04-large.png)

この行は、お客様の認証をキー`Title`にバインドすることをオブジェクト上に存在しません。 おを変更する場合、バインド バックアップ`Name`インターフェイス ビルダーで、保存、同期を再構築し、実行に問題なく正常に、アプリケーションは実行されます。

## <a name="summary"></a>まとめ

この記事では、データ バインディングと Xamarin.Mac アプリケーションのコーディングのキーと値の操作についての詳細を取得しました。 最初に、キーと値 (KVC) のコーディングを使用し、キーと値 (KVO) を観察しによって Objective C に c# クラスを公開する調べるとします。 次に、KVO 準拠のクラスを使用する方法を示したし、データは Xcode のインターフェイスのビルダーでの UI 要素にバインドします。 複合データ バインディングを使用して最後に、示しました**配列コント ローラー**と**ツリー コント ローラー**です。


## <a name="related-links"></a>関連リンク

- [MacDatabinding ストーリー ボード (サンプル)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (サンプル)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [標準コントロール](~/mac/user-interface/standard-controls.md)
- [テーブルのビュー](~/mac/user-interface/table-view.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [コレクション ビュー](~/mac/user-interface/collection-view.md)
- [キーと値のコーディングのプログラミング ガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [キーと値を観察しプログラミング ガイドの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [プログラミングのトピック Cocoa バインディングの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa バインド参照の概要](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
