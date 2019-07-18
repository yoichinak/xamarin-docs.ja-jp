---
title: データ バインディングとキー値は、Xamarin.Mac でコーディング
description: この記事では、キーと値のコーディングと観察の Xcode の Interface Builder での UI 要素へのデータ バインディングを許可するキーと値を使用してについて説明します。
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 4a391160f2102fd1f069a45eb7c16aec91dfd7e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61428098"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>データ バインディングとキー値は、Xamarin.Mac でコーディング

_この記事では、キーと値のコーディングと観察の Xcode の Interface Builder での UI 要素へのデータ バインディングを許可するキーと値を使用してについて説明します。_

## <a name="overview"></a>概要

同じキー値コーディングとデータ バインド手法へのアクセスがある、Xamarin.Mac アプリケーションで c# と .NET を使用する場合をで作業する開発者*Objective C*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_データは、コードを記述する代わりに UI 要素にバインドします。

キーと値のコーディングおよびデータ バインド、Xamarin.Mac アプリケーションで技術を使用して、コードの記述し、保守を設定し、UI 要素を使用する必要があるの量を大幅に短縮できます。 さらに、バックアップ データを分離の利点がある場合も (_データ モデル_)、正面からのユーザー インターフェイスを終了します (_モデル-ビュー-コント ローラー_)、柔軟性の高いアプリケーションを管理しやすくします。デザインします。

[![実行中のアプリの例](databinding-images/intro01.png "実行中のアプリの例")](databinding-images/intro01-large.png#lightbox)

この記事では、キー値コーディングし、Xamarin.Mac アプリケーションでのデータ バインディングの操作の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>キー値コーディング

オブジェクトのプロパティを直接アクセスする、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用するためのメカニズムは、キー値コーディング (KVC) (`get/set`)。 キー値コーディング準拠アクセサーを実装すると、Xamarin.Mac アプリケーションで、キーと値観測 (KVO)、データ バインディング、Core Data、Cocoa バインド、およびあることを示すなどの他の macOS (旧称 OS X) 機能へのアクセスを行えます。

キーと値のコーディングおよびデータ バインド、Xamarin.Mac アプリケーションで技術を使用して、コードの記述し、保守を設定し、UI 要素を使用する必要があるの量を大幅に短縮できます。 さらに、バックアップ データを分離の利点がある場合も (_データ モデル_)、正面からのユーザー インターフェイスを終了します (_モデル-ビュー-コント ローラー_)、柔軟性の高いアプリケーションを管理しやすくします。デザインします。 

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

まず、`[Register("PersonModel")]`属性クラスを登録し、OBJECTIVE-C に公開します。 継承するクラスが必要し、 `NSObject` (またはそのサブクラスから継承する`NSObject`)、いくつかの基本クラスに KVC 準拠にできるようにするメソッドが追加されます。 次に、`[Export("Name")]`属性の公開、`Name`プロパティ KVC と KVO 手法を通じてプロパティへのアクセスを後で使用されるキー値を定義します。 

最後に、キーと値の確認プロパティの値を変更するには、アクセサーする必要があります変更をラップするのにはその値を`WillChangeValue`と`DidChangeValue`メソッドの呼び出し (と同じキーを指定する、`Export`属性)。  例えば:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

この手順は_非常に_Xcode でのデータ バインディングの重要なの Interface Builder (この記事の後半でここに表示されます)。

詳細については、Apple を参照してください[キー値コーディング プログラミング ガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)します。

### <a name="keys-and-key-paths"></a>キーとキーのパス

A_キー_オブジェクトの特定のプロパティを識別する文字列します。 通常、キーは、準拠しているオブジェクトのコーディングのキーと値のアクセサー メソッドの名前に対応します。 キーは ASCII エンコーディングを使用する必要があります、通常は、小文字で始まるし、空白文字を含めることはできません。 上記の例のように指定`Name`のキー値になります`Name`のプロパティ、`PersonModel`クラス。 キーとそれが公開するプロパティの名前は、ほとんどの場合は、同じにする必要はありません。

A_キー パス_はドットの文字列をスキャンするには、オブジェクトのプロパティの階層の指定に使用されるキーの分離します。 受信側に対する相対パス、シーケンス内の最初のキーのプロパティですし、各後続のキーが前のプロパティの値を基準に評価されます。 同じ方法では、オブジェクトと c# クラスでは、そのプロパティを走査するのにドット表記を使用します。

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

お子様の名前をキー パスになります。`self.Child.Name`または単に`Child.Name`(キー値が使用されている方法に基づく)。

### <a name="getting-values-using-key-value-coding"></a>キー値コーディングを使用して値を取得します。

`ValueForKey`メソッドは、指定したキーの値を返します (として、 `NSString`) 要求を受信 KVC クラスのインスタンスからの相対します。 たとえば場合、`Person`のインスタンスである、`PersonModel`上で定義したクラス。

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

値が返されます、`Name`プロパティのインスタンスを`PersonModel`します。 

### <a name="setting-values-using-key-value-coding"></a>キー値コーディングを使用して値の設定

同様に、`SetValueForKey`指定したキーの値を設定 (として、 `NSString`) 要求を受信 KVC クラスのインスタンスからの相対します。 繰り返しますのインスタンスを使用して、`PersonModel`クラスを次に示すよう。

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

値を変更、`Name`プロパティを`Jane Doe`します。

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>値の変更を監視すること

キーと値の確認 (KVO) を使用して、KVC 準拠のクラスの特定のキーにオブザーバーをアタッチすることができ、そのキーの値を変更すると、(KVC 手法を使用してまたは、c# コードで指定されたプロパティに直接アクセスする) たびに通知します。 例えば:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

ここで、いつでも、`Name`のプロパティ、`Person`のインスタンス、`PersonModel`クラスを変更すると、新しい値コンソールに書き込みます。 

詳細については、Apple を参照してください[キーと値を観察しプログラミング ガイドの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)します。

## <a name="data-binding"></a>データ バインディング

次のセクションでは、キー値コーディングと準拠のクラスを観察するキーと値を使用して、読み取りと書き込みの c# コードを使用して値の代わりに、Xcode の Interface Builder の UI 要素にデータをバインドする方法に表示されます。 この方法で分離する、_データ モデル_からそれらを表示するために使用するビューには、Xamarin.Mac アプリケーションをより柔軟で保守が簡単に行います。 書き込まれる必要のあるコードの量も大幅に低下します。

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>データ モデルを定義します。

として機能する、Xamarin.Mac アプリケーションで定義されている KVC/KVO 準拠しているクラスがある必要がある前にデータ バインド インターフェイス ビルダーでの UI 要素を_データ モデル_バインディング。 データ モデルは、すべてのユーザー インターフェイスに表示されます、ユーザーがアプリケーションの実行中に UI で、データへの変更を送受信するデータを提供します。

たとえば、従業員のグループを管理するアプリケーションを作成していた場合は、データ モデルを定義する次のクラスを使用できます。

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

このクラスの機能の大半が記載された、[はキー値コーディング](#What_is_Key-Value_Coding)前のセクション。 ただし、いくつかの特定の要素とのデータ モデルとして機能するには、このクラスの許可に加えられたいくつかの追加を見てみましょう**配列コント ローラー**と**コント ローラーのツリー** (これは、データを後で使用します。バインド**ツリー ビュー**、**アウトライン ビュー**と**コレクション ビュー**)。

最初に、従業員とマネージャーは、ため、使用した、 `NSArray` (具体的には、`NSMutableArray`値を変更できるように) を添付するを管理する社員を許可します。

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

ここで注意する 2 つは、

1. 使用して、`NSMutableArray`標準の c# の配列またはコレクションのため、これは、AppKit コントロールにデータをバインドする要件などではなく**テーブル ビュー**、**アウトライン ビュー**と**コレクション**.
2. キャストして従業員の配列を公開しました、`NSArray`データのバインドを目的し、変更されたそのC#名、書式設定された`People`、データ バインディングが想定する 1 つに`personModelArray`形式で **{class_name} 配列**(最初の文字に小文字に変換が行われたことに注意してください)。

次に、いくつか特別な名前のパブリック メソッドをサポートするを追加する必要があります**配列コント ローラー**と**コント ローラーのツリー**:

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

これにより、コント ローラーを要求し、それらを表示するデータを変更できます。 などの公開されている`NSArray`非常に特定の名前付け規則 (一般的な c# 名前付け規則とは異なる) ことがあるこれら上記。

- `addObject:` -配列にオブジェクトを追加します。
- `insertObject:in{class_name}ArrayAtIndex:` -場所`{class_name}`クラスの名前を指定します。 このメソッドは、指定したインデックス位置にある配列にオブジェクトを挿入します。
- `removeObjectFrom{class_name}ArrayAtIndex:` -場所`{class_name}`クラスの名前を指定します。 このメソッドは、指定したインデックス位置にある配列内のオブジェクトを削除します。
- `set{class_name}Array:` -場所`{class_name}`クラスの名前を指定します。 このメソッドを使用すると、新しい、既存の実行に置き換えることができます。

これらのメソッドの内部では、内の配列への変更をラップした`WillChangeValue`と`DidChangeValue`KVO コンプライアンス メッセージ。

最後に、以降、`Icon`の値にプロパティが依存して、 `isManager` 、プロパティに対する変更を`isManager`プロパティに反映されない可能性があります、 `Icon` (KVO) 中にデータ バインドされている UI 要素の。

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

独自のキーだけでなく、`isManager`アクセサーが送信することも、`WillChangeValue`と`DidChangeValue`向けのメッセージ、`Icon`キーの変更も表示されるようにします。

使用する、`PersonModel`この記事の残りの部分でのデータ モデル。

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>単純データ バインディング

定義されている、データ モデルには、Xcode の Interface Builder でのデータ バインディングの簡単な例を見てみましょう。 など、Xamarin.Mac アプリケーションを編集するために使用するのには、フォームを追加しましょう、`PersonModel`上で定義したです。 私たちのモデルのプロパティを表示および編集には、いくつかのテキスト フィールドとチェック ボックスを追加します。

最初に、新しい追加**ビュー コント ローラー**を**Main.storyboard**インターフェイス ビルダーでファイルし、そのクラスの名前を`SimpleViewController`: 

[![新しいビュー コント ローラーの追加](databinding-images/simple01.png "新しいビュー コント ローラーの追加")](databinding-images/simple01-large.png#lightbox)

次に、Mac に Visual Studio に戻り、編集、 **SimpleViewController.cs** (つまり、プロジェクトに自動的に追加された) ファイルのインスタンスを公開し、`PersonModel`データ バインディングをフォームをいたします。 次のコードを追加します。

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

次に、ビューが読み込まれるときにみましょうのインスタンスを作成、`PersonModel`し、このコードを設定します。

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

フォームを作成する必要があります、ダブルクリック、 **Main.storyboard**ファイルを開き、インターフェイス ビルダーで編集します。 レイアウト フォームは、次のようになります。

[![Xcode でストーリー ボードを編集](databinding-images/simple02.png "Xcode でストーリー ボードの編集")](databinding-images/simple02-large.png#lightbox)

データにバインドするためのフォーム、`PersonModel`経由で公開しました、`Person`キーを次の操作を行います。

1. 選択、 **Employee Name**テキスト フィールドに切り替えると、**バインド インスペクター**します。
2. チェック、**バインド**ボックスを選択します**単純なビュー コント ローラー**ドロップダウン リストから。 次に入力`self.Person.Name`の**キー パス**: 

    [![キーのパスを入力する](databinding-images/simple03.png "キーのパスを入力します。")](databinding-images/simple03-large.png#lightbox)
3. 選択、**職業**テキスト フィールドとチェック、**バインド**ボックスを選択します**単純なビュー コント ローラー**ドロップダウン リストから。 次に入力`self.Person.Occupation`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple04.png "キーのパスを入力します。")](databinding-images/simple04-large.png#lightbox)
4. 選択、**従業員がマネージャー**チェック ボックスを確認し、**にバインド**ボックスを選択します**単純なビュー コント ローラー**ドロップダウン リストから。 次に入力`self.Person.isManager`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple05.png "キーのパスを入力します。")](databinding-images/simple05-large.png#lightbox)
5. 選択、**管理された従業員の数**テキスト フィールドとチェック、**にバインド**ボックスを選択します**単純なビュー コント ローラー**ドロップダウン リストから。 次に入力`self.Person.NumberOfEmployees`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple06.png "キーのパスを入力します。")](databinding-images/simple06-large.png#lightbox)
6. 数の従業員管理されているラベルとテキスト フィールドを非表示にたい場合は、従業員がマネージャーではありません。
7. 選択、**管理された従業員の数**ラベル、展開、 **Hidden**折り返しとチェック、**にバインド**ボックスを選択します**単純なビュー コント ローラー**ドロップダウン リストから。 次に入力`self.Person.isManager`の**キー パス**:  

    [![キーのパスを入力する](databinding-images/simple07.png "キーのパスを入力します。")](databinding-images/simple07-large.png#lightbox)
8. 選択`NSNegateBoolean`から、**値トランスフォーマー**ドロップダウン。  

    ![NSNegateBoolean キーの変換を選択する](databinding-images/simple08.png "NSNegateBoolean キーの変換を選択します。")
9. データ バインディングを示しますこの場合、ラベルを非表示にすることの値、`isManager`プロパティは`false`します。
10. 手順 7. および 8 を繰り返して、**数の従業員管理**テキスト フィールド。
11. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

アプリケーションからの値を実行する場合、`Person`プロパティは、フォームを自動的に設定します。

[![自動設定フォームを示す](databinding-images/simple09.png "を自動に設定されたフォームを表示")](databinding-images/simple09-large.png#lightbox)

ユーザーがフォームにすべての変更にライトバックは、`Person`ビュー コント ローラーのプロパティ。 たとえば、選択を解除する**従業員がマネージャー**更新プログラム、`Person`のインスタンス、`PersonModel`と**管理された従業員の数**ラベルとテキスト フィールドが自動的に (を介して非表示データ バインディングの場合):

[![管理者以外の従業員の数を非表示](databinding-images/simple10.png "管理者以外の従業員の数を非表示")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>テーブル ビューのデータ バインディング

データ バインディングの基礎をしたらを見てみましょうより複雑なデータ バインドのタスクを使用して、_配列コント ローラー_およびテーブル ビューにデータをバインドします。 テーブル ビューの使用の詳細についてを参照してください、[テーブル ビュー](~/mac/user-interface/table-view.md)ドキュメント。

最初に、新しい追加**ビュー コント ローラー**を**Main.storyboard**インターフェイス ビルダーでファイルし、そのクラスの名前を`TableViewController`:

[![新しいビュー コント ローラーの追加](databinding-images/table01.png "新しいビュー コント ローラーの追加")](databinding-images/table01-large.png#lightbox)

次に、編集、 **TableViewController.cs** (つまり、プロジェクトに自動的に追加された) ファイル サービスおよび公開配列 (`NSArray`) の`PersonModel`クラスのデータ バインディングをフォームをいたします。 次のコードを追加します。

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

上と同様、`PersonModel`クラス上の[、データ モデルを定義する](#Defining_your_Data_Model) セクションで、特別に指定された 4 つのパブリック メソッドを公開していますようにのコレクションから配列コント ローラーおよび読み取りと書き込みデータ`PersonModels`。

次に、ビューが読み込まれるときにこのコードで配列を設定する必要があります。

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

テーブル、ビューを作成する必要があります、ダブルクリック、 **Main.storyboard**ファイルを開き、インターフェイス ビルダーで編集します。 レイアウト テーブルは、次のようになります。

[![新しいテーブルのビュー レイアウト](databinding-images/table02.png "新しい表形式ビューのレイアウト")](databinding-images/table02-large.png#lightbox)

追加する必要があります、**配列コント ローラー**このテーブルにバインドされたデータを提供する、次の操作します。

1. ドラッグ、**配列コント ローラー**から、**ライブラリ インスペクター**上に、**インターフェイス エディター**:  

    ![配列コント ローラーをライブラリから選択](databinding-images/table03.png "ライブラリから、配列コント ローラーを選択します。")
2. 選択**配列コント ローラー**で、**インターフェイス階層**に切り替えると、**属性インスペクター**:  

    [![属性インスペクターを選択する](databinding-images/table04.png "Attributes Inspector の選択")](databinding-images/table04-large.png#lightbox)
3. 入力`PersonModel`の**クラス名**、 をクリックして、 **Plus**ボタンをクリックし、3 つのキーを追加します。 名前を付けます`Name`、`Occupation`と`isManager`:  

    ![必要なキー パスを追加する](databinding-images/table05.png "必要なキー パスを追加します。")
4. 内容は、管理の配列、配列コント ローラーこれはありプロパティ (キー) を使用して公開する必要があります。
5. 切り替えて、**バインド インスペクター** **コンテンツ配列**選択**にバインド**と**テーブル ビュー コント ローラー**。 入力、**キーのパスをモデル化**の`self.personModelArray`:  

    ![キーのパスを入力する](databinding-images/table06.png "キーのパスを入力します。")
6. 配列を配列コント ローラーを結び付けるこの`PersonModels`ビュー コント ローラーで公開します。

テーブル、ビューを配列コント ローラーにバインドする必要があります、これで、次の操作を行います。

1. テーブル ビューを選択し、**バインド インスペクター**:  

    [![バインド インスペクターを選択する](databinding-images/table07.png "バインド インスペクターを選択します。")](databinding-images/table07-large.png#lightbox)
2. 、**テーブルの内容**折り返しで、**バインド**と**配列コント ローラー**します。 入力`arrangedObjects`の**コント ローラー キー**フィールド。  

    ![コント ローラーのキーを定義する](databinding-images/table08.png "コント ローラーのキーを定義します。")
3. 選択、 **Table View Cell**下、**従業員**列。 **バインド インスペクター**下、**値**折り返しで、**にバインド**と**テーブル セル ビュー**。 入力`objectValue.Name`の**キーのパスをモデル化**:  

    [![モデルのキー パスを設定する](databinding-images/table09.png "モデルのキー パスを設定します。")](databinding-images/table09-large.png#lightbox)
4. `objectValue` 現在は、`PersonModel`配列コント ローラーによって管理されている配列。
5. 選択、 **Table View Cell**下、**職業**列。 **バインド インスペクター**下、**値**折り返しで、**にバインド**と**テーブル セル ビュー**。 入力`objectValue.Occupation`の**キーのパスをモデル化**:  

    [![モデルのキー パスを設定する](databinding-images/table10.png "モデルのキー パスを設定します。")](databinding-images/table10-large.png#lightbox)
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

配列で、テーブルにデータを設定して、アプリケーションを実行した場合`PersonModels`:

[![アプリケーションを実行している](databinding-images/table11.png "アプリケーションを実行します。")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>アウトライン ビューのデータ バインディング

アウトライン表示に対してデータ バインディングは、テーブル ビューに対するバインディングによく似ています。 主な違いは、使用すること、**コント ローラーのツリー**の代わりに、**配列コント ローラー**アウトライン ビューにバインドされたデータを提供します。 アウトライン ビューの使用の詳細についてを参照してください、[アウトライン ビュー](~/mac/user-interface/outline-view.md)ドキュメント。

最初に、新しい追加**ビュー コント ローラー**を**Main.storyboard**インターフェイス ビルダーでファイルし、そのクラスの名前を`OutlineViewController`: 

[![新しいビュー コント ローラーの追加](databinding-images/outline01.png "新しいビュー コント ローラーの追加")](databinding-images/outline01-large.png#lightbox)

次に、編集、 **OutlineViewController.cs** (つまり、プロジェクトに自動的に追加された) ファイル サービスおよび公開配列 (`NSArray`) の`PersonModel`クラスのデータ バインディングをフォームをいたします。 次のコードを追加します。

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

上で実行したよう、`PersonModel`クラスの上、 [、データ モデルを定義する](#Defining_your_Data_Model) セクションで、特別に指定された 4 つのパブリック メソッドを公開していますようにコント ローラーのツリーおよび読み取りと書き込みデータのコレクションから`PersonModels`します。

次に、ビューが読み込まれるときにこのコードで配列を設定する必要があります。

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

アウトライン ビューを作成する必要がありますをダブルクリック、 **Main.storyboard**ファイルを開き、インターフェイス ビルダーで編集します。 レイアウト テーブルは、次のようになります。

[![アウトライン ビューを作成する](databinding-images/outline02.png "アウトライン ビューの作成")](databinding-images/outline02-large.png#lightbox)

追加する必要があります、**コント ローラーのツリー**アウトラインにバインドされたデータを提供する、次の操作します。

1. ドラッグ、**コント ローラーのツリー**から、**ライブラリ インスペクター**上に、**インターフェイス エディター**:  

    ![ツリーのコント ローラーをライブラリから選択](databinding-images/outline03.png "ライブラリからツリー コント ローラーを選択します。")
2. 選択**コント ローラーのツリー**で、**インターフェイス階層**に切り替えると、**属性インスペクター**:  

    [![属性インスペクターを選択する](databinding-images/outline04.png "属性インスペクターを選択します。")](databinding-images/outline04-large.png#lightbox)
3. 入力`PersonModel`の**クラス名**、 をクリックして、 **Plus**ボタンをクリックし、3 つのキーを追加します。 名前を付けます`Name`、`Occupation`と`isManager`:  

    ![必要なキー パスを追加する](databinding-images/outline05.png "必要なキー パスを追加します。")
4. 配列を管理にどのようなツリーのコント ローラーこれはありプロパティ (キー) を使用して公開する必要があります。
5. 、**コント ローラーのツリー**セクションに、入力`personModelArray`の**子**、入力`NumberOfEmployees`、**数**入力と`isEmployee`下**リーフ**:  

    ![コント ローラーのツリーのキー パスの設定](databinding-images/outline05.png "コント ローラーのツリーのキー パスの設定")
6. これは、ツリーのコント ローラーに、ノードはある子ノードの数は、現在のノードに子ノードがあるかどうかは、すべての子を検索する場所を指示します。
7. 切り替えて、**バインド インスペクター** **コンテンツ配列**選択**にバインド**と**ファイルの所有者**。 入力、**キーのパスをモデル化**の`self.personModelArray`:  

    ![キー パスの編集](databinding-images/outline06.png "キー パスの編集")
8. これによって、ツリーのコント ローラーの配列を`PersonModels`ビュー コント ローラーで公開します。

ツリーのコント ローラーに、アウトライン ビューをバインドする必要があります、これで、次の操作を行います。

1. アウトライン ビューを選択し、、**バインド インスペクター**を選択します。  

    [![バインド インスペクターを選択する](databinding-images/outline07.png "バインド インスペクターを選択します。")](databinding-images/outline07-large.png#lightbox)
2. 下、**アウトライン ビューの内容**折り返しで、**バインド**と**コント ローラーのツリー**。 入力`arrangedObjects`の**コント ローラー キー**フィールド。  

    ![コント ローラーのキーを設定する](databinding-images/outline08.png "コント ローラーのキーを設定します。")
3. 選択、 **Table View Cell**下、**従業員**列。 **バインド インスペクター**下、**値**折り返しで、**にバインド**と**テーブル セル ビュー**。 入力`objectValue.Name`の**キーのパスをモデル化**:  

    [![モデルのキーのパスを入力する](databinding-images/outline09.png "モデルのキーのパスを入力します。")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` 現在は、`PersonModel`ツリー コント ローラーによって管理されている配列。
5. 選択、 **Table View Cell**下、**職業**列。 **バインド インスペクター**下、**値**折り返しで、**にバインド**と**テーブル セル ビュー**。 入力`objectValue.Occupation`の**キーのパスをモデル化**:  

    [![モデルのキーのパスを入力する](databinding-images/outline10.png "モデルのキーのパスを入力します。")](databinding-images/outline10-large.png#lightbox)
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

アプリケーションを実行した場合、アウトラインが設定されます、配列の`PersonModels`:

[![アプリケーションを実行している](databinding-images/outline11.png "アプリケーションを実行します。")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>コレクション ビューのデータ バインディング

コレクション ビューを使用してデータ バインディングには、配列コント ローラーを使用して、コレクションのデータを提供するにはテーブル ビューでは、バインドとよく似て。 コレクション ビューを事前設定された表示形式を持たない、ため多くの作業がユーザーの相互作用のフィードバックを提供して、ユーザーの選択を追跡するために必要です。

> [!IMPORTANT]
> Xcode 7 および macOS 10.11 (以降) で問題は、コレクション ビューでは、ストーリー ボード (.storyboard) ファイルで使用するのにはできません。 その結果、.xib ファイルを使用して Xamarin.Mac アプリのコレクション ビューを定義する続行する必要があります。 参照してください、[コレクション ビュー](~/mac/user-interface/collection-view.md)詳細についてはドキュメントです。

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

## <a name="debugging-native-crashes"></a>ネイティブ クラッシュのデバッグ

データ バインドで間違いを犯す可能性を_ネイティブ クラッシュ_アンマネージ コードになり、Xamarin.Mac アプリケーションで完全に失敗する、`SIGABRT`エラー。

[![ネイティブ クラッシュのダイアログ ボックスの例](databinding-images/debug01.png "ネイティブ クラッシュのダイアログ ボックスの例")](databinding-images/debug01-large.png#lightbox)

データ バインド時に通常はネイティブ クラッシュの 4 つの主な原因があります。

1. データ モデルを継承しない`NSObject`またはそのサブクラス`NSObject`します。
2. Objective C を使用するプロパティを公開しなかった、`[Export("key-name")]`属性。
3. アクセサーの値に変更をラップしなかった`WillChangeValue`と`DidChangeValue`メソッドの呼び出し (と同じキーを指定する、`Export`属性)。
4. 間違っているか、入力の間違いキーがある、**バインド インスペクター**インターフェイス ビルダーでします。

### <a name="decoding-a-crash"></a>クラッシュのデコード

みましょうを見つけて、その修正方法を紹介できるように、データ バインドでネイティブのクラッシュが発生します。 インターフェイス ビルダーでは、最初のラベルからコレクション ビューの例では、バインドを変更してみましょう`Name`に`Title`:

[![バインド キーを編集](databinding-images/debug02.png "バインド キーを編集")](databinding-images/debug02-large.png#lightbox)

変更を保存、Visual Studio for Mac は Xcode と同期され、アプリケーションの実行に戻り、みましょう。 アプリケーションが一時的にクラッシュし、コレクション ビューが表示されるときに、`SIGABRT`エラー (ように、**アプリケーション出力**Visual Studio for Mac で) ため、`PersonModel`キーを持つプロパティを公開しません`Title`:

[![バインド エラーの例](databinding-images/debug03.png "バインド エラーの例")](databinding-images/debug03-large.png#lightbox)

内のエラーの最上部までスクロールする場合、**アプリケーション出力**問題を解決する鍵ことがわかります。

[![エラー ログで問題を見つける](databinding-images/debug04.png "エラー ログで、問題の検索")](databinding-images/debug04-large.png#lightbox)

この行は、ことを通知する、キー`Title`にバインドしているオブジェクトに存在しません。 変更する場合、バインド元に戻す`Name`インターフェイス ビルダーで、保存、同期、再構築し、実行に問題なく期待どおりに、アプリケーションは実行されます。

## <a name="summary"></a>まとめ

この記事では、データ バインディングとキー値は、Xamarin.Mac アプリケーションでコーディングの使用方法について詳しく説明をしました。 最初に、キー値コーディング (KVC) を使用して、キーと値 (KVO) を観察する OBJECTIVE-C を c# クラスを公開することについて説明しました。 次に、KVO 準拠のクラスを使用する方法を説明しましたし、データは Xcode の Interface Builder での UI 要素にバインドします。 最後に、複雑なデータ バインディングを使用して示しました**配列コント ローラー**と**コント ローラーのツリー**します。


## <a name="related-links"></a>関連リンク

- [MacDatabinding ストーリー ボード (サンプル)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding Xib (サンプル)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [標準コントロール](~/mac/user-interface/standard-controls.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [コレクション ビュー](~/mac/user-interface/collection-view.md)
- [キー値コーディングのプログラミング ガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [キーと値を観察しプログラミング ガイドの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Cocoa バインドのプログラミングのトピックの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa バインド参照の概要](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
