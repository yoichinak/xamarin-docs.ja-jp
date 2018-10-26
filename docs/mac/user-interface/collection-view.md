---
title: Xamarin.Mac でコレクション ビュー
description: この記事では、Xamarin.Mac アプリでのコレクション ビューの使用について説明します。 作成および Xcode と Interface Builder でのコレクション ビューを維持し、それらをプログラムで操作を説明します。
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/24/2017
ms.openlocfilehash: 904db0b97a8b21fd51722b70a63386a53e3f5347
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104038"
---
# <a name="collection-views-in-xamarinmac"></a>Xamarin.Mac でコレクション ビュー

_この記事では、Xamarin.Mac アプリでのコレクション ビューの使用について説明します。作成および Xcode と Interface Builder でのコレクション ビューを維持し、それらをプログラムで操作を説明します。_

C# と .NET Xamarin.Mac アプリケーションでは、開発者の操作がある同じへのアクセス時に AppKit コレクション ビュー コントロールで作業する開発者*Objective C*と*Xcode*は。 開発者が Xcode を使用するため、Xamarin.Mac は直接 Xcode と統合、 _Interface Builder_を作成し、コレクション ビューを維持します。

A`NSCollectionView`サブビューを使用して整理のグリッドを表示、`NSCollectionViewLayout`します。 グリッドで各サブビューがによって表される、`NSCollectionViewItem`からビューのコンテンツの読み込みを管理、`.xib`ファイル。

[![アプリの実行例](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

この記事では、Xamarin.Mac アプリでのコレクション ビューを操作の基本について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事全体で使用される手法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>コレクション ビューについて

コレクション ビューの主な目的 (`NSCollectionView`) コレクション ビューのレイアウトを使用して整理された方法でオブジェクトのグループを視覚的に配置するには (`NSCollectionViewLayout`)、個々 のオブジェクトと (`NSCollectionViewItem`) 大規模なコレクションで独自のビューを取得します。 コレクション ビューは、データ バインディングとキー値コーディングの手法を使用して動作し、そのため、お読みください、[データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)この記事に進む前にドキュメント。

コレクション ビューには標準的な組み込みコレクション ビューなどの項目 (アウトラインまたはテーブル ビューで行います)、れていないため、開発者は、設計および実装する責任を負います、_プロトタイプ ビュー_イメージ フィールドなどの他の AppKit コントロールを使用します。、テキスト フィールド、ラベルなど。このプロトタイプのビューを表示し、コレクション ビューによって管理されている各項目を操作に使用してに格納されている場合は、`.xib`ファイル。

開発者がコレクション ビューの項目のルック アンド フィールを行うため、コレクション ビューには、グリッドで選択した項目を強調表示の組み込みサポートがありません。 この機能の実装は、この記事で説明されます。

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>データ モデルを定義します。

データ、キー値コーディング (KVC) インターフェイス ビルダーでは、コレクション ビューをバインドする前に、Xamarin.Mac アプリとして動作するキーと値を観察し (KVO) 準拠のクラスを定義する必要があります/、_データ モデル_バインディング。 データ モデルは、すべてのコレクションに表示され、ユーザーがアプリケーションの実行中に UI で、データへの変更を受信するデータを提供します。

従業員のグループを管理するアプリの例を見て、次のクラスを使用してデータ モデルを定義できます。

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

`PersonModel`データ モデルをこの記事の残りの部分で使用されます。

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>コレクション ビューの操作

コレクション ビューを使用してデータ バインディングはテーブル ビューでは、非常に多くの like バインド`NSCollectionViewDataSource`コレクションのデータを提供するために使用します。 コレクション ビューを事前設定された表示形式を持たない、ため多くの作業がユーザーの相互作用のフィードバックを提供して、ユーザーの選択を追跡するために必要です。

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>セルのプロトタイプの作成

コレクション ビューに既定のセルのプロトタイプが含まれていないので、開発者が 1 つまたは複数追加する必要があります`.xib`ファイルを Xamarin.Mac アプリに個々 のセルの内容とレイアウトを定義します。

次の手順で行います。

1. **ソリューション エクスプ ローラー**プロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**
2. 選択**Mac** > **ビュー コント ローラー**、名前を付けます (など`EmployeeItem`この例では) をクリックし、**新規**ボタンを作成します。 

    ![新しいビュー コント ローラーの追加](collection-view-images/proto01.png)

    これが追加されます、 `EmployeeItem.cs`、`EmployeeItemController.cs`と`EmployeeItemController.xib`プロジェクトのソリューション ファイル。
3. ダブルクリックして、`EmployeeItemController.xib`ファイルを開き、Xcode の Interface Builder で編集します。
4. 追加、 `NSBox`、`NSImageView`と 2 つ`NSLabel`ビューにコントロールし、それらを次のようにレイアウトします。

    ![セルのプロトタイプのレイアウトのデザイン](collection-view-images/proto02.png)
5. 開く、**アシスタント エディター**を作成し、**アウトレット**の`NSBox`セルの選択状態を示すために使用できるように。

    ![アウトレットの NSBox を公開します。](collection-view-images/proto03.png)
6. 戻り、**標準エディター**イメージ ビューを選択します。
7. **バインド インスペクター**を選択します**バインドに** > **ファイルの所有者**を入力し、**モデルのキー パス**の`self.Person.Icon`:

    ![アイコンのバインド](collection-view-images/proto04.png)
8. 最初のラベルを選択し、**バインド インスペクター**を選択します**バインドを** > **ファイルの所有者**を入力し、**モデル キー パス**の`self.Person.Name`:

    ![名前のバインド](collection-view-images/proto05.png)
9. 2 番目のラベルを選択し、**バインド インスペクター**を選択します**バインドに** > **ファイルの所有者**を入力し、**モデル キー パス**の`self.Person.Occupation`:

    ![職業のバインド](collection-view-images/proto06.png)
10. 変更を保存、`.xib`ファイルを開き、変更の同期を Visual Studio に戻ります。

編集、`EmployeeItemController.cs`ファイルを開き、次のようになります。

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

このコードの詳細を見ると、クラスは継承`NSCollectionViewItem`コレクション ビュー セルのプロトタイプを果たすことができるようにします。 `Person`プロパティは、イメージの表示と Xcode でラベルにデータをバインドに使用されたクラスを公開します。 これは、インスタンス、`PersonModel`上記で作成します。

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

コレクション ビューのデータ ソース (`NSCollectionViewDataSource`) コレクション ビューのすべてのデータを提供し、作成および Collection View Cell を設定します (を使用して、`.xib`プロトタイプ)、コレクション内の各項目の必要に応じて。

その呼び出しのプロジェクトの新しいクラスを追加`CollectionViewDataSource`し、次のようになります。

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

このコードの詳細を見ると、クラスは継承`NSCollectionViewDataSource`の一覧を公開および`PersonModel`インスタンスを通じてその`Data`プロパティ。

このコレクションの 1 つのセクションのみのため、コードがオーバーライド、`GetNumberOfSections`メソッドを常に返します`1`します。 さらに、`GetNumberofItems`メソッドのオーバーライドでの項目数を返します、`Data`プロパティの一覧。

`GetItem`新しいセルが必要ですし、次のように、メソッドが呼び出されます。

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem`の再利用可能なインスタンスを作成またはコレクション ビューのメソッドが呼び出され、`EmployeeItemController`とその`Person`プロパティは、要求されたセルに表示されている項目に設定します。 

`EmployeeItemController`事前に次のコードを使用して、コレクションのビュー コント ローラーに登録する必要があります。

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**識別子**(`EmployeeCell`) で使用される、`MakeItem`呼び出す_する必要があります_コレクション ビューに登録済みのビュー コント ローラーの名前と一致します。 この手順は、以下で詳しく取り上げます。

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>処理項目の選択

選択されをコレクション内の項目の選択解除を処理するために、`NSCollectionViewDelegate`が必要になります。 この例で使用する組み込みため`NSCollectionViewFlowLayout`レイアウトの種類、`NSCollectionViewDelegateFlowLayout`このデリゲートの特定のバージョンが必要になります。

呼び出すことが、プロジェクトに新しいクラスを追加`CollectionViewDelegate`し、次のようになります。

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

`ItemsSelected`と`ItemsDeselected`メソッドがオーバーライドされ、オンまたはオフにするために使用、`PersonSelected`ユーザーが選択または項目の選択を解除するときに、コレクション ビューを処理しているビュー コント ローラーのプロパティ。 これは、以下で詳しく表示されます。

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>インターフェイス ビルダーでのコレクション ビューの作成

すべての場所に必要なサポート情報、メインのストーリー ボードを編集できる、コレクション ビューを追加します。

次の手順で行います。

1. ダブルクリックして、`Main.Storyboard`ファイル、**ソリューション エクスプ ローラー**の Interface Builder を Xcode で開きます。
2. メイン ビューにコレクション ビューをドラッグし、サイズを変更して、ビューを入力します。

    ![コレクション ビューのレイアウトに追加](collection-view-images/collection01.png)
3. コレクション ビューを選択、制約エディターを使用してください、サイズが変更されたときに、ビューにピン留めしています。

    ![制約の追加](collection-view-images/collection02.png)
4. コレクション ビューが選択されていることを確認、**デザイン サーフェイス**(および not、**スクロール ビューの枠線**または**のクリップ表示**それを含む) に切り替え、 **アシスタント エディター**を作成し、**アウトレット**コレクション ビューの。

    ![制約の追加](collection-view-images/collection03.png)
5. 変更を保存し、同期する Visual Studio に戻ります。

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>すべての統合を取り込む

今すぐデータ モデルとして機能するクラスに格納されたすべての関連部分 (`PersonModel`)、`NSCollectionViewDataSource`にデータを提供するが追加されました、`NSCollectionViewDelegateFlowLayout`項目の選択を処理するために作成された、`NSCollectionView`メイン ストーリー ボードに追加されましたアウトレットとして公開されると (`EmployeeCollection`)。

最後の手順では、コレクション ビューを含むビュー コント ローラーを編集し、すべてのコレクションを設定し、項目の選択を処理する部分を表示します。

編集、`ViewController.cs`ファイルを開き、次のようになります。

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

詳しくは、次のコードを見て、`Datasource`のインスタンスを保持するプロパティが定義されている、`CollectionViewDataSource`コレクション ビューに、データを提供します。 A`PersonSelected`を保持するプロパティが定義されている、`PersonModel`コレクション ビューで現在選択されている項目を表します。 このプロパティも発生します。、`SelectionChanged`選択が変更されたときにイベント。

`ConfigureCollectionView`クラスは、次の行を使用してコレクション ビュー セル プロトタイプとして機能するビュー コント ローラーの登録に使用します。

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

注意して、**識別子**(`EmployeeCell`) でという 1 つ、プロトタイプの一致を登録するために使用、`GetItem`のメソッド、`CollectionViewDataSource`上で定義します。

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

ビュー コント ローラーの種類ではさらに、**する必要があります**の名前と一致、`.xib`プロトタイプを定義するファイル**まったく**します。 この例では、`EmployeeItemController`と`EmployeeItemController.xib`します。

コレクション ビュー内の項目の実際のレイアウトがコレクション ビューのレイアウト クラスによって制御され、新しいインスタンスを割り当てることによって実行時に動的に変更できます、`CollectionViewLayout`プロパティ。 このプロパティを変更せず、変更をアニメーション化、コレクション ビューの外観を更新します。

Apple に 2 つの組み込みのレイアウト型は、最も一般的な使用を処理するコレクション ビューが用意されています:`NSCollectionViewFlowLayout`と`NSCollectionViewGridLayout`します。 カスタム インスタンスを作成できますが、開発者には、円に含まれる項目のレイアウトなどのカスタム書式指定が必要な場合`NSCollectionViewLayout`し、目的の効果を実現するために必要なメソッドをオーバーライドします。

この例のインスタンスを作成するために、既定のフロー レイアウトを使用して、`NSCollectionViewFlowLayout`クラスし、次のように構成します。

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize`プロパティ コレクション内の各セルのサイズを定義します。 `SectionInset`プロパティでレイアウトするセルのコレクションのエッジからくぼみを定義します。 `MinimumInteritemSpacing` アイテム間の最小間隔を定義し、`MinimumLineSpacing`コレクション内の行の最小間隔を定義します。

レイアウトは、コレクション ビューのインスタンスに割り当てられている、`CollectionViewDelegate`が項目の選択を処理するためにアタッチされています。

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData`メソッドの新しいインスタンスを作成、 `CollectionViewDataSource`、データを設定します、コレクション ビューと呼び出しにアタッチします。、`ReloadData`項目を表示するメソッド。

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

`ViewDidLoad`メソッドがオーバーライドされていて、呼び出し、`ConfigureCollectionView`と`PopulateWithData`最終的なコレクション ビューをユーザーに表示する方法。

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

この記事では、Xamarin.Mac アプリケーションでコレクション ビューの使用方法について詳しく説明をしました。 最初に、キー値コーディング (KVC) とキー値を観察し (KVO) を使用して OBJECTIVE-C を c# クラスを公開することについて説明しました。 次に、KVO 準拠のクラスを使用する方法を説明しましたし、データが Xcode の Interface Builder でのコレクション ビューにバインドします。 最後に、c# コードで、コレクション ビューと対話する方法を示しました。

## <a name="related-links"></a>関連リンク

- [MacCollectionNew (サンプル)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
