---
title: Xamarin. Mac のソースリスト
description: この記事では、Xamarin. Mac アプリケーションでのソースリストの使用について説明します。 Xcode と Interface Builder でのソースリストの作成と保守、および C# コードでのソースリストとの対話について説明します。
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 9280917f451d58ae864398088bec7faaf15ad96a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937346"
---
# <a name="source-lists-in-xamarinmac"></a>Xamarin. Mac のソースリスト

_この記事では、Xamarin. Mac アプリケーションでのソースリストの使用について説明します。Xcode と Interface Builder でのソースリストの作成と保守、および C# コードでのソースリストとの対話について説明します。_

Xamarin. Mac アプリケーションで C# と .NET を使用する場合、 *Xcode と*で作業*している*開発者が同じソースリストにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してソースリストを作成および管理できます (または、必要に応じて C# コードで直接作成することもできます)。

ソースリストは、Finder または iTunes のサイドバーのような、アクションのソースを表示するために使用される特殊な種類のアウトラインビューです。

[![ソースリストの例](source-list-images/source05.png)](source-list-images/source05.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのソースリストの使用に関する基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に[Hello, Mac](~/mac/get-started/hello-mac.md)の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)」と「[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

<a name="Introduction_to_Outline_Views"></a>

## <a name="introduction-to-source-lists"></a>ソースリストの概要

前述のように、ソースリストは、Finder または iTunes のサイドバーのようなアクションのソースを表示するために使用される、特殊な種類のアウトラインビューです。 ソースリストは、ユーザーが階層データの行を展開したり折りたたんだりできるようにするテーブルの種類です。 テーブルビューとは異なり、ソースリスト内の項目はフラットな一覧には含まれず、ハードドライブ上のファイルやフォルダーなどの階層に編成されます。 ソースリストのアイテムに他のアイテムが含まれている場合は、そのアイテムをユーザーが展開または折りたたむことができます。

ソースリストは、特 `NSOutlineView` にテーブルビュー () のサブクラスであり、その `NSTableView` 親クラスからの動作の多くを継承する、特別にスタイルが適用されたアウトラインビュー () です。 その結果、アウトラインビューでサポートされている多くの操作は、ソースリストでもサポートされます。 Xamarin アプリケーションはこれらの機能を制御し、特定の操作を許可または禁止するためにソースリストのパラメーター (コードまたは Interface Builder) を構成できます。

ソースリストは、独自のデータを格納するのではなく、必要に応じて、データソース () を使用して `NSOutlineViewDataSource` 必要な行と列の両方を提供します。

ソースリストの動作をカスタマイズするには、アウトラインビューのデリゲート () のサブクラスを使用して、 `NSOutlineViewDelegate` アウトラインの種類をサポートし、各項目の機能、項目の選択と編集、カスタム追跡、およびカスタムビューを選択します。

ソースリストでは、テーブルビューとアウトラインビューでの動作と機能の多くが共有されているため、この記事を続行する前に、[テーブルビュー](~/mac/user-interface/table-view.md)と[アウトラインビュー](~/mac/user-interface/outline-view.md)のドキュメントを参照することをお勧めします。

<a name="Working_with_Source_Lists"></a>

## <a name="working-with-source-lists"></a>ソースリストの操作

ソースリストは、Finder または iTunes のサイドバーのような、アクションのソースを表示するために使用される特殊な種類のアウトラインビューです。 アウトライン表示とは異なり、Interface Builder でソースリストを定義する前に、Xamarin. Mac でバッキングクラスを作成してみましょう。

まず、 `SourceListItem` ソースリストのデータを保持する新しいクラスを作成してみましょう。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `SourceListItem` 、[**新規**] ボタンをクリックします。 **Name**

[![空のクラスの追加](source-list-images/source01.png)](source-list-images/source01.png#lightbox)

ファイルの `SourceListItem.cs` 外観を次のようにします。 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

**ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `SourceListDataSource` 、[**新規**] ボタンをクリックします。 **Name** ファイルの `SourceListDataSource.cs` 外観を次のようにします。

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

これにより、ソースリストのデータが提供されます。

**ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `SourceListDelegate` 、[**新規**] ボタンをクリックします。 **Name** ファイルの `SourceListDelegate.cs` 外観を次のようにします。

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

これにより、ソースリストの動作が提供されます。

最後に、**ソリューションエクスプローラー**でプロジェクトを右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `SourceListView` 、[**新規**] ボタンをクリックします。 **Name** ファイルの `SourceListView.cs` 外観を次のようにします。

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

これにより、() の再利用可能なカスタムサブクラスが作成されます。このサブクラスを使用して、 `NSOutlineView` `SourceListView` 作成したすべての Xamarin. Mac アプリケーションでソースリストを駆動できます。

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode"></a>

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Xcode でのソースリストの作成と管理

次に、Interface Builder でソースリストを設計しましょう。 ファイルをダブルクリックし `Main.storyboard` て編集するには Interface Builder で開き、[**ライブラリインスペクター**] から分割ビューをドラッグして、ビューコントローラーに追加し、[**制約エディター**] のビューでサイズを変更するように設定します。

[![制約の編集](source-list-images/source00.png)](source-list-images/source00.png#lightbox)

次に、**ライブラリインスペクター**からソースリストをドラッグし、分割ビューの左側に追加して、**制約エディター**のビューでサイズを変更するように設定します。

[![制約の編集](source-list-images/source02.png)](source-list-images/source02.png#lightbox)

次に、 **Id ビュー**に切り替え、ソースリストを選択し、**クラス**をに変更し `SourceListView` ます。

[![クラス名の設定](source-list-images/source03.png)](source-list-images/source03.png#lightbox)

最後に、ファイルにというソースリストの**アウトレット**を作成し `SourceList` `ViewController.h` ます。

[![アウトレットの構成](source-list-images/source04.png)](source-list-images/source04.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

<a name="Populating the Source List"></a>

## <a name="populating-the-source-list"></a>ソースリストの設定

`RotationWindow.cs`Visual Studio for Mac でファイルを編集し、次のようにメソッドを作成してみましょう `AwakeFromNib` 。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

メソッドは、 `Initialize ()` 項目が追加さ_れる前に_、ソースリストの**アウトレット**に対して呼び出す必要があります。 アイテムのグループごとに、親アイテムを作成し、そのグループアイテムにサブアイテムを追加します。 その後、各グループがソースリストのコレクションに追加され `SourceList.AddItem (...)` ます。 最後の2行は、ソースリストのデータを読み込み、すべてのグループを展開します。

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

最後に、ファイルを編集し、メソッドを次のようにし `AppDelegate.cs` `DidFinishLaunching` ます。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

アプリケーションを実行すると、次のように表示されます。

[![アプリの実行例](source-list-images/source05.png)](source-list-images/source05.png#lightbox)

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでソースリストを使用する方法について詳しく説明しました。 Xcode の Interface Builder でソースリストを作成して管理する方法と、C# コードでソースリストを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacOutlines (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [アウトラインビューの概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
