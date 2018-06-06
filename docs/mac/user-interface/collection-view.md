---
title: Xamarin.Mac でコレクション ビュー
description: この記事では、Xamarin.Mac アプリでのコレクション ビューの使用について説明します。 これは、作成および Xcode とインターフェイスのビルダー コレクション ビューを維持し、それらのプログラムの操作について説明します。
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: 8e354b1ce273b10758a7d8c1361055b972839943
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792558"
---
# <a name="collection-views-in-xamarinmac"></a>Xamarin.Mac でコレクション ビュー

_この記事では、Xamarin.Mac アプリでのコレクション ビューの使用について説明します。これは、作成および Xcode とインターフェイスのビルダー コレクション ビューを維持し、それらのプログラムの操作について説明します。_

Xamarin.Mac アプリで、開発者は、c# と .NET の作業がある同じへのアクセス時に AppKit コレクション ビュー コントロールで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 開発者は Xcode の使用 Xamarin.Mac は、Xcode と直接統合、ため_インターフェイス ビルダー_を作成してコレクション ビューを保持します。

A`NSCollectionView`サブビューを使用して構成のグリッドを表示、`NSCollectionViewLayout`です。 グリッドで各サブビューがによって表される、`NSCollectionViewItem`からビューのコンテンツの読み込みを管理する、`.xib`ファイル。

[![実行のサンプル アプリ](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

この記事では、Xamarin.Mac アプリでは、コレクション ビューの操作の基本について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主要な概念とこの記事全体で使用される手法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>コレクション ビューの概要

コレクション ビューの主な目的は、(`NSCollectionView`) は、コレクション ビューのレイアウトを使用して別の方法でオブジェクトのグループを視覚的に配置する (`NSCollectionViewLayout`)、個々 のオブジェクトと (`NSCollectionViewItem`) 大規模なコレクション内の独自のビューを取得します。 コレクション ビューがデータ バインドと、キーと値のコーディング技法を使用して動作しなどをお読みください、[データ バインディングと、キーと値のコーディング](~/mac/app-fundamentals/databinding.md)この記事の内容を続行する前にドキュメント。

コレクション ビューには、標準的な組み込みコレクション ビューの項目 (はのように、アウトラインまたはテーブル ビュー)、れていないため、開発者が設計と実装を担当する、_プロトタイプ ビュー_イメージ フィールドなどの他の AppKit コントロールの使用、テキスト フィールド、ラベルなどです。このプロトタイプ ビューを表示して、コレクション ビューによって管理されている各項目を使用するために使用してに格納されて、`.xib`ファイル。

開発者は、コレクション ビューの項目のルック アンド フィールを担当するため、コレクション ビューには、グリッドで選択した項目を強調表示の組み込みサポートがありません。 この機能を実装することは、この記事で説明されます。

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>データ モデルを定義します。

データ、キーと値のコーディング (KVC) インターフェイス ビルダーでは、コレクション ビューをバインドする前に、Xamarin.Mac アプリとして機能するキーと値を観察し (KVO) 準拠のクラスを定義する必要があります/、_データ モデル_バインディングのです。 データ モデルでは、コレクションに表示され、ユーザーがアプリケーションの実行中に、ui がデータに変更を受信するデータのすべてを提供します。

従業員のグループを管理するアプリの例を実行、次のクラスは、データ モデルを定義するために使用可能性があります。

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

`PersonModel`この記事の残りの部分ではデータ モデルを使用します。

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>コレクション ビューの操作

コレクション ビューとデータ バインディングは非常に近い like バインド ビューでは、テーブル、`NSCollectionViewDataSource`データ コレクションを提供するために使用します。 コレクション ビューが既定の表示形式があるないためより多くの作業はユーザー操作のフィードバックを提供し、ユーザーの選択を追跡するために必要です。

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>セルのプロトタイプの作成

コレクション ビューに既定のセルのプロトタイプが含まれていないため、開発者が 1 つまたは複数を追加する必要があります`.xib`Xamarin.Mac をアプリにレイアウトと個々 のセルの内容を定義するファイル。

次の手順で行います。

1. **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **追加** > **新しいファイル.**
2. 選択**Mac** > **ビュー コント ローラー**、名前を付けます (など`EmployeeItem`この例では) をクリックし、**新規**を作成するにはボタン。 

    ![新しいビュー コント ローラーを追加します。](collection-view-images/proto01.png)

    これが追加されます、 `EmployeeItem.cs`、`EmployeeItemController.cs`と`EmployeeItemController.xib`プロジェクトのソリューション ファイルです。
3. ダブルクリックして、`EmployeeItemController.xib`編集のため Xcode のインターフェイスのビルダーで開くファイル。
4. 追加、 `NSBox`、`NSImageView`と 2 つ`NSLabel`ビューをコントロールし、それらを次のようにレイアウトします。

    ![セルのプロトタイプのレイアウトのデザイン](collection-view-images/proto02.png)
5. 開く、**アシスタント エディター**を作成し、**コンセント**の`NSBox`これは、セルの選択状態を示すために使用できるようにします。

    ![コンセントに NSBox を公開します。](collection-view-images/proto03.png)
6. 戻り、**標準エディター**ビューのイメージを選択します。
7. **インスペクターのバインド****バインドを** > **ファイルの所有者**を入力し、**モデル キー パス**の`self.Person.Icon`:

    ![アイコンのバインド](collection-view-images/proto04.png)
8. 最初のラベルを選択し、、**インスペクターのバインド****バインドを** > **ファイルの所有者**を入力し、**モデル キー パス**の`self.Person.Name`:

    ![バインディングの名前](collection-view-images/proto05.png)
9. 2 番目のラベルを選択し、、**インスペクターのバインド****バインドに** > **ファイルの所有者**を入力し、**モデル キー パス**の`self.Person.Occupation`:

    ![職業のバインド](collection-view-images/proto06.png)
10. 変更を保存、`.xib`ファイルし、変更の同期を Visual Studio に戻ります。

編集、`EmployeeItemController.cs`ファイルし、次のようになります。

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

このコードの詳細を見ると、クラス継承`NSCollectionViewItem`コレクション ビューのセルのプロトタイプとして動作できるようにします。 `Person`プロパティは、イメージのビューと Xcode でのラベルへのデータ バインドに使用されたクラスを公開します。 これは、インスタンス、`PersonModel`上記で作成します。

`BackgroundColor`プロパティへのショートカットは、`NSBox`コントロールの`FillColor`セルの選択状態の表示に使用されます。 オーバーライドすることで、`Selected`のプロパティ、 `NSCollectionViewItem`、次のコードを設定またはこの選択状態をクリアします。

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

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>コレクション ビューのデータ ソースを作成します。

詳細については、コレクションの データ ソースの表示 (`NSCollectionViewDataSource`) コレクション ビューのすべてのデータを提供し、作成し、コレクション ビューのセルに入力 (を使用して、`.xib`プロトタイプ)、コレクション内の各項目の必要に応じて。

新しいクラスを追加、プロジェクト、それを呼び出す`CollectionViewDataSource`され次のようになります。

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

このコードの詳細を見て、クラスは継承`NSCollectionViewDataSource`のリストを公開および`PersonModel`を通じてインスタンスその`Data`プロパティです。

コードがオーバーライドのみ、このコレクションには 1 つのセクションがあるので、`GetNumberOfSections`メソッドを常に返す`1`です。 さらに、`GetNumberofItems`メソッドのオーバーライドでの項目数を返します、`Data`プロパティの一覧です。

`GetItem`メソッドは、別のセルが必要とは、次のようになります。

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem`の再利用可能なインスタンスを作成またはコレクション ビューのメソッドが呼び出されて、`EmployeeItemController`とその`Person`プロパティは、要求されたセルに表示されている項目に設定します。 

`EmployeeItemController`次のコードを使用して、コレクション ビューのコント ローラーに登録されている必要があります。

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**識別子**(`EmployeeCell`) で使用される、`MakeItem`呼び出す_必要があります_コレクション ビューに登録されているビュー コント ローラーの名前と一致します。 この手順は、以下で詳しく取り上げます。

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>処理の項目の選択

処理が選択されをコレクション内の項目の選択を解除するために、`NSCollectionViewDelegate`が必要になります。 この例で使用する組み込みため`NSCollectionViewFlowLayout`レイアウトの種類、`NSCollectionViewDelegateFlowLayout`このデリゲートの特定のバージョンが必要になります。

プロジェクトに新しいクラスを追加でも呼び出せます`CollectionViewDelegate`され次のようになります。

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

`ItemsSelected`と`ItemsDeselected`メソッドがオーバーライドされ、オンまたはオフにするために使用、`PersonSelected`ユーザーを選択またはアイテムの選択を解除するときに、コレクション ビューを処理しているビュー コント ローラーのプロパティです。 これは、以下で詳細に表示されます。

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>インターフェイスのビルダーでのコレクション ビューの作成

すべての場所に必要なサポート部分には、メインのストーリー ボードを編集してコレクション ビューを追加します。

次の手順で行います。

1. ダブルクリックして、`Main.Storyboard`ファイルで、**ソリューション エクスプ ローラー**を開くには、Xcode で編集するためのインターフェイスのビルダー。
2. コレクション ビューをメイン ビューにドラッグし、サイズを変更してビュー全体にします。

    ![コレクション ビューのレイアウトに追加](collection-view-images/collection01.png)
3. コレクション ビューが選択されている、制約エディターを使用して、サイズが変更されるときに、ビューにピン留めします。

    ![制約の追加](collection-view-images/collection02.png)
4. コレクション ビューが選択されていることを確認、**デザイン サーフェイス**(および not、**枠線の Scroll View**または**のクリップ表示**それを含む) に切り替え、 **アシスタント エディター**を作成し、**コンセント**コレクション ビューの。

    ![制約の追加](collection-view-images/collection03.png)
5. 変更を保存し、同期する Visual Studio に戻ります。

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>すべての統合を取り込む

今すぐデータ モデルとして機能するクラスを持つ所定の位置に格納されたすべての関連部分 (`PersonModel`)、`NSCollectionViewDataSource`は、データを提供に追加されて、`NSCollectionViewDelegateFlowLayout`項目の選択項目を処理するために作成された、`NSCollectionView`メイン ストーリー ボードに追加されましたコンセントとして公開されると (`EmployeeCollection`)。

最後の手順では、コレクション ビューを含むビュー コント ローラーを編集し、すべてのコレクションを設定し、項目の選択を処理する部分です。

編集、`ViewController.cs`ファイルし、次のようになります。

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

このコードを見る、詳細で作成すること、`Datasource`のインスタンスを保持するプロパティが定義されて、`CollectionViewDataSource`コレクション ビューに、データを提供します。 A`PersonSelected`を保持するプロパティが定義されている、`PersonModel`コレクション ビューに現在選択されている項目を表すです。 このプロパティにも発生、`SelectionChanged`選択が変更されたときにイベント。

`ConfigureCollectionView`クラスは、次の行を使用して、コレクション ビューとセル プロトタイプとして機能するビュー、コント ローラーを登録に使用します。

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

注意して、**識別子**(`EmployeeCell`) で、いずれかが呼び出されるプロトタイプの一致を登録するために使用、`GetItem`のメソッド、`CollectionViewDataSource`上記で定義されました。

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

ビュー コント ローラーの種類ではさらに、**必要があります**の名前に一致、`.xib`プロトタイプを定義するファイル**まったく**です。 この例では、場合`EmployeeItemController`と`EmployeeItemController.xib`です。

コレクション ビュー内の項目の実際のレイアウトは、コレクション ビューのレイアウトのクラスによって制御され、新しいインスタンスを割り当てることによって実行時に動的に変更できます、`CollectionViewLayout`プロパティです。 このプロパティを変更すると、変更をアニメーション化することがなくコレクション ビューの外観が更新されます。

Apple コレクション ビューが最も一般的な使用を処理する 2 つの組み込みの配置の型は付属しています:`NSCollectionViewFlowLayout`と`NSCollectionViewGridLayout`です。 カスタム インスタンスを作成する開発者には、円で項目をレイアウトなどのカスタム書式指定が必要な場合`NSCollectionViewLayout`所定の効果を実現するために必要なメソッドをオーバーライドします。

この例のインスタンスを作成するために既定のフローのレイアウトを使用して、`NSCollectionViewFlowLayout`クラスし、次のように構成されます。

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize`プロパティでは、コレクションの各セルのサイズを定義します。 `SectionInset`プロパティは、セルのレイアウトは、コレクションの端からくぼみを定義します。 `MinimumInteritemSpacing` アイテムの最小間隔を定義および`MinimumLineSpacing`コレクション内の行の最小間隔を定義します。

レイアウトが、コレクション ビューのインスタンスに割り当てられている、`CollectionViewDelegate`項目の選択を処理する接続します。

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData`メソッドの新しいインスタンスを作成、 `CollectionViewDataSource`、データを設定、コレクション ビューと呼び出しにアタッチ、`ReloadData`項目を表示する方法。

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

`ViewDidLoad`メソッドがオーバーライドされ、呼び出し、`ConfigureCollectionView`と`PopulateWithData`ユーザーに、最後のコレクション ビューを表示する方法。

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでコレクション ビューの操作についての詳細を取得しました。 まず、OBJECTIVE-C する c# クラスを公開するキーと値のコーディング (KVC) およびキーと値を観察し (KVO) を使用する調べるとします。 次に、KVO 準拠のクラスを使用する方法を示したし、データは Xcode のインターフェイスのビルダー内のコレクション ビューにバインドします。 最後に、c# コードでコレクション ビューと対話する方法を示しました。

## <a name="related-links"></a>関連リンク

- [MacCollectionNew (サンプル)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
