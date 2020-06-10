---
title: Xamarin. Mac のコレクションビュー
description: この記事では、Xamarin. Mac アプリでのコレクションビューの操作について説明します。 Xcode と Interface Builder でのコレクションビューの作成と管理、およびプログラムによる操作について説明します。
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/24/2017
ms.openlocfilehash: f6e9a9338c0bce628cfd62d1106601ddc7a11490
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568634"
---
# <a name="collection-views-in-xamarinmac"></a>Xamarin. Mac のコレクションビュー

_この記事では、Xamarin. Mac アプリでのコレクションビューの操作について説明します。Xcode と Interface Builder でのコレクションビューの作成と管理、およびプログラムによる操作について説明します。_

Xamarin. Mac アプリで C# と .NET を使用する場合、開発者は同じ AppKit コレクションビューコントロールにアクセスできます。このコントロールは、 *Xcode および*で作業する開発者が*実行します*。 Xcode は直接統合されているため、開発者は Xcode の_Interface Builder_を使用してコレクションビューを作成および管理します。

は `NSCollectionView` 、を使用して整理されたサブビューのグリッドを表示 `NSCollectionViewLayout` します。 グリッド内の各サブビューは、 `NSCollectionViewItem` ファイルからのビューのコンテンツの読み込みを管理するによって表され `.xib` ます。

[![アプリの実行例](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリでコレクションビューを操作する方法の基本について説明します。 この記事で使用されている主要な概念と手法について説明しているように、最初に[Hello, Mac](~/mac/get-started/hello-mac.md)の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)」および「[アクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

<a name="About_Collection_Views"></a>

## <a name="about-collection-views"></a>コレクションビューについて

コレクションビュー () の主な目的は、 `NSCollectionView` コレクションビューレイアウト () を使用して整理された方法でオブジェクトのグループを視覚的に配置することです `NSCollectionViewLayout` 。個々のオブジェクト () は、 `NSCollectionViewItem` より大きなコレクションで独自のビューを取得します。 コレクションビューはデータバインディングとキー値のコーディング手法によって動作するため、この記事を続行する前に、[データバインディングとキー値のコーディング](~/mac/app-fundamentals/databinding.md)に関するドキュメントを読む必要があります。

コレクションビューには、標準の組み込みコレクションビューアイテム (アウトラインやテーブルビューなど) がないため、開発者は、イメージフィールド、テキストフィールド、ラベルなどの他の AppKit コントロールを使用して、_プロトタイプビュー_をデザインおよび実装する責任があります。このプロトタイプビューは、コレクションビューによって管理され、ファイルに格納されている各項目を表示および操作するために使用され `.xib` ます。

開発者はコレクションビューアイテムのルックアンドフィールを担当しているため、コレクションビューには、グリッド内で選択された項目を強調表示するためのサポートが組み込まれていません。 この機能の実装については、この記事で説明します。

<a name="Defining_your_Data_Model"></a>

## <a name="defining-the-data-model"></a>データモデルの定義

Interface Builder でコレクションビューをデータバインドする前に、バインドの_データモデル_として機能するように、キー値のコーディング (kvc)/Keyvalue 観測 (kvc) 準拠のクラスを Xamarin. Mac アプリで定義する必要があります。 データモデルは、コレクションに表示されるすべてのデータを提供し、アプリケーションの実行中にユーザーが UI で行ったデータへの変更を受け取ります。

従業員のグループを管理するアプリの例を見て、次のクラスを使用してデータモデルを定義できます。

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
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
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

`PersonModel`データモデルは、この記事の残りの部分で使用されます。

<a name="Working_with_a_Collection_View"></a>

## <a name="working-with-a-collection-view"></a>コレクションビューの操作

コレクションビューを使用したデータバインディングは、 `NSCollectionViewDataSource` コレクションにデータを提供するために使用されるテーブルビューとのバインドとよく似ています。 コレクションビューには事前設定された表示形式がないため、ユーザーの操作に関するフィードバックを提供し、ユーザーの選択を追跡するために、より多くの作業が必要になります。

<a name="Creating-the-Cell-Prototype"></a>

### <a name="creating-the-cell-prototype"></a>セルプロトタイプの作成

コレクションビューには既定のセルプロトタイプが含まれていないため、開発者は1つ以上 `.xib` のファイルを Xamarin. Mac アプリに追加して、個々のセルのレイアウトとコンテンツを定義する必要があります。

次の操作を行います。

1. **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。
2. [ **Mac**  >  **ビューコントローラー**] を選択し、名前 ( `EmployeeItem` この例ではなど) を指定して、[**新規**作成] ボタンをクリックします。 

    ![新しいビューコントローラーの追加](collection-view-images/proto01.png)

    これにより `EmployeeItem.cs` 、、 `EmployeeItemController.cs` および `EmployeeItemController.xib` ファイルがプロジェクトのソリューションに追加されます。
3. ファイルをダブルクリックし `EmployeeItemController.xib` て、Xcode の Interface Builder で編集するために開きます。
4. `NSBox` `NSImageView` ビューにとという2つのコントロールを追加し、次のように `NSLabel` レイアウトします。

    ![セルプロトタイプのレイアウトのデザイン](collection-view-images/proto02.png)
5. **アシスタントエディター**を開き、の**アウトレット**を作成して、 `NSBox` セルの選択状態を示すために使用できるようにします。

    ![アウトレットでの NSBox の公開](collection-view-images/proto03.png)
6. **標準エディター**に戻り、[イメージ] ビューを選択します。
7. **バインドインスペクター**で、[ファイルの所有者**にバインドする**] を選択  >  **File's Owner**し、**モデルキーのパス**を次のように入力し `self.Person.Icon` ます。

    ![アイコンのバインド](collection-view-images/proto04.png)
8. 最初のラベルを選択し、**バインドインスペクター**で [ファイルの所有者**にバインドする**] を選択し、  >  **File's Owner**次のような**モデルキーパス**を入力し `self.Person.Name` ます。

    ![名前のバインド](collection-view-images/proto05.png)
9. 2番目のラベルを選択し、**バインドインスペクター**で [ファイルの所有者**にバインドする**] を選択し、  >  **File's Owner**次のような**モデルキーパス**を入力し `self.Person.Occupation` ます。

    ![職業のバインド](collection-view-images/proto06.png)
10. 変更内容をファイルに保存 `.xib` し、Visual Studio に戻って変更を同期します。

ファイルを編集 `EmployeeItemController.cs` し、次のように表示します。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

このコードを詳しく見ると、クラスはから継承さ `NSCollectionViewItem` れるため、コレクションビューセルのプロトタイプとして機能できます。 プロパティは、 `Person` イメージビューにデータをバインドするために使用されたクラスと、Xcode のラベルを公開します。 これは、上で作成したのインスタンスです `PersonModel` 。

プロパティは、 `BackgroundColor` `NSBox` `FillColor` セルの選択状態を表示するために使用されるコントロールのへのショートカットです。 次のコードでは、のプロパティをオーバーライドすることにより、 `Selected` `NSCollectionViewItem` この選択状態を設定またはクリアします。

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"></a>

### <a name="creating-the-collection-view-data-source"></a>コレクションビューのデータソースの作成

コレクションビューのデータソース () は、コレクション `NSCollectionViewDataSource` ビューのすべてのデータを提供し、コレクション `.xib` 内の各項目に必要なコレクションビューのセル (プロトタイプを使用) を作成して設定します。

プロジェクトに新しいクラスを追加し、それを呼び出して `CollectionViewDataSource` 、次のように表示します。

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

このコードを詳しく見ると、クラスはから継承 `NSCollectionViewDataSource` し、プロパティを通じてインスタンスのリストを公開し `PersonModel` `Data` ます。

このコレクションにはセクションが1つしかないため、コードはメソッドをオーバーライド `GetNumberOfSections` し、常にを返し `1` ます。 さらに、 `GetNumberofItems` メソッドは、プロパティリスト内の項目の数を返し `Data` ます。

メソッドは、 `GetItem` 新しいセルが必要になるたびに呼び出され、次のようになります。

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem`コレクションビューのメソッドは、の再利用可能なインスタンスを作成または返すために呼び出され、 `EmployeeItemController` その `Person` プロパティは、要求されたセルに表示される項目に設定されます。 

を `EmployeeItemController` コレクションビューコントローラーに事前に登録するには、次のコードを使用する必要があります。

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

呼び出しで使用される**識別子**() は、 `EmployeeCell` `MakeItem` コレクションビューに登録されているビューコントローラーの名前と一致_する必要があり_ます。 この手順については、以下で詳しく説明します。

<a name="Handling-Item-Selection"></a>

### <a name="handling-item-selection"></a>項目の選択の処理

コレクション内の項目の選択と deselection を処理するには、が `NSCollectionViewDelegate` 必要です。 この例では、組み込みのレイアウト型を使用するため `NSCollectionViewFlowLayout` 、 `NSCollectionViewDelegateFlowLayout` このデリゲートの特定のバージョンが必要になります。

新しいクラスをプロジェクトに追加し、それを呼び出して `CollectionViewDelegate` 、次のように表示します。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

`ItemsSelected`メソッドと `ItemsDeselected` メソッドはオーバーライドされ、 `PersonSelected` ユーザーが項目を選択または選択解除したときに、コレクションビューを処理するビューコントローラーのプロパティを設定またはクリアするために使用されます。 これについては、以下で詳しく説明します。

<a name="Creating-the-Collection-View-in-Interface-Builder"></a>

### <a name="creating-the-collection-view-in-interface-builder"></a>Interface Builder でのコレクションビューの作成

必要なすべてのサポートパーツが配置されているので、メインストーリーボードを編集し、コレクションビューを追加することができます。

次の操作を行います。

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Main.Storyboard` て、Xcode の Interface Builder で編集するために開きます。 **Solution Explorer**
2. コレクションビューをメインビューにドラッグして、ビューに合わせてサイズを変更します。

    ![レイアウトへのコレクションビューの追加](collection-view-images/collection01.png)
3. コレクションビューを選択した状態で、制約エディターを使用して、サイズを変更したときにビューにピン留めします。

    ![制約の追加](collection-view-images/collection02.png)
4. **デザインサーフェイス**でコレクションビューが選択されていることを確認し (これを含む、**境界スクロールビュー**または**クリップビュー**ではありません)、**アシスタントエディター**に切り替え、コレクションビューの**アウトレット**を作成します。

    ![制約の追加](collection-view-images/collection03.png)
5. 変更を保存し、Visual Studio に戻り、同期します。

<a name="Bringing-it-all-Together"></a>

## <a name="bringing-it-all-together"></a>まとめ

データモデルとして機能するクラス () を使用して、データを提供するためにが追加され、項目の選択を処理するためにが作成され、が `PersonModel` `NSCollectionViewDataSource` メインの `NSCollectionViewDelegateFlowLayout` ストーリーボードに追加され、が `NSCollectionView` コンセントとして公開されるようになりました ( `EmployeeCollection` )。

最後の手順では、コレクションビューを含むビューコントローラーを編集し、すべての要素をまとめて、コレクションにデータを設定し、項目の選択を処理します。

ファイルを編集 `ViewController.cs` し、次のように表示します。

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

このコードを詳しく見ていくと、 `Datasource` プロパティはコレクションビューのデータを提供するのインスタンスを保持するように定義され `CollectionViewDataSource` ます。 `PersonSelected`プロパティは、 `PersonModel` コレクションビューで現在選択されている項目を表すを保持するように定義されます。 このプロパティ `SelectionChanged` は、選択内容が変更された場合にもイベントを発生させます。

クラスは、 `ConfigureCollectionView` 次の行を使用して、コレクションビューでセルプロトタイプとして機能するビューコントローラーを登録するために使用されます。

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

**Identifier** `EmployeeCell` プロトタイプの登録に使用された識別子 () が、上記で定義されているのメソッドで呼び出された識別子と一致していることに注意して `GetItem` `CollectionViewDataSource` ください。

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

また、ビューコントローラーの種類は、プロトタイプを正確に定義するファイルの名前と一致**する必要があり** `.xib` ます。 **exactly** この例の場合は、 `EmployeeItemController` と `EmployeeItemController.xib` です。

コレクションビュー内の項目の実際のレイアウトは、コレクションビューレイアウトクラスによって制御され、新しいインスタンスをプロパティに割り当てることによって、実行時に動的に変更できます `CollectionViewLayout` 。 このプロパティを変更すると、変更をアニメーション化せずにコレクションビューの外観が更新されます。

Apple では、最も一般的な使用方法であるとを処理するコレクションビューを使用して、2つの組み込みのレイアウトの種類を提供 `NSCollectionViewFlowLayout` して `NSCollectionViewGridLayout` います。 開発者が円に項目を配置するなどのカスタム書式を必要とした場合は、のカスタムインスタンスを作成 `NSCollectionViewLayout` し、必要なメソッドをオーバーライドして目的の効果を実現できます。

この例では、既定のフローレイアウトを使用してクラスのインスタンスを作成 `NSCollectionViewFlowLayout` し、次のように構成します。

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

プロパティは、 `ItemSize` コレクション内の個々のセルのサイズを定義します。 プロパティは、 `SectionInset` セルがレイアウトされるコレクションの端からのインセットを定義します。 `MinimumInteritemSpacing`項目間の最小間隔を定義し、 `MinimumLineSpacing` コレクション内の行の間の最小間隔を定義します。

レイアウトはコレクションビューに割り当てられ、のインスタンス `CollectionViewDelegate` は項目の選択を処理するためにアタッチされます。

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

メソッドは、の新しいインスタンスを作成し、それにデータを設定して、 `PopulateWithData` `CollectionViewDataSource` それをコレクションビューにアタッチし、メソッドを呼び出して `ReloadData` 項目を表示します。

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

`ViewDidLoad`メソッドはオーバーライドされ、メソッドとメソッドを呼び出して、 `ConfigureCollectionView` `PopulateWithData` 最後のコレクションビューをユーザーに表示します。

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでコレクションビューを使用する方法について詳しく説明しました。 まず、キー値のコード (KVC) とキー値の観測 (KVC) を使用して、C# クラスを目的の C に公開する方法を見てきました。 次に、KVO に準拠したクラスを使用し、データを Xcode の Interface Builder のコレクションビューにバインドする方法について説明しました。 最後に、C# コードでコレクションビューを操作する方法を示しました。

## <a name="related-links"></a>関連リンク

- [MacCollectionNew (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccollectionnew)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
