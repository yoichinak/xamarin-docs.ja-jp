---
title: ソース リスト
description: この記事では、ソース リスト Xamarin.Mac アプリケーションでの作業について説明します。 これは、作成および Xcode およびインターフェイスのビルダーでのソース リストを維持し、c# コードでこれらと対話するについて説明します。
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a8d3a67768b9e47833d1819c3bf44774a52d2438
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="source-lists"></a>ソース リスト

_この記事では、ソース リスト Xamarin.Mac アプリケーションでの作業について説明します。これは、作成および Xcode およびインターフェイスのビルダーでのソース リストを維持し、c# コードでこれらと対話するについて説明します。_

同じアクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときソース リストで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、ソースの一覧を維持 (または必要に応じて c# コードで直接作成すること)。

ソースの一覧は、特殊な種類のアウトライン表示 Finder または iTunes でサイド バーと同様に、操作のソースを表示するために使用します。

[![](source-list-images/source05.png "ソース一覧の例")](source-list-images/source05.png#lightbox)

この記事で Xamarin.Mac アプリケーションのソースを一覧表示の操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>ソース リストの概要

前述のように、ソース一覧は特殊な種類のアウトライン表示の Finder または iTunes でサイド バーと同様に、操作のソースを表示するために使用されます。 ソース リストは、ユーザーがテーブルの種類の展開または階層データの行を折りたたむです。 テーブル ビューとは異なり、ソース リスト内の項目は単純なリストではありません、階層では、ハード ドライブのファイルとフォルダーのように構成されます。 ソース リスト内の項目に他の項目が含まれている場合に、展開または折りたたむユーザーによってことができます。

元のリストが特別にスタイルのアウトライン表示 (`NSOutlineView`)、自体は、テーブル ビューのサブクラス (`NSTableView`) し、そのため、多くの動作をその親クラスから継承します。 その結果、アウトライン ビューでサポートされているさまざまな操作は、ソース リストとしてもサポートされます。 Xamarin.Mac アプリケーションは、これらの機能のコントロールを持ち、特定の操作を許可または拒否する (コードまたはインターフェイスのビルダーにいずれか)、ソース リストのパラメーターを構成できます。

ソースの一覧は、独自のデータを格納していない、代わりに、データ ソースに依存しています (`NSOutlineViewDataSource`) を行と、必要に応じてごとに、必要な列の両方を提供します。

アウトライン表示デリゲートのサブクラスを提供することにより、ソースの一覧の動作をカスタマイズすることができます (`NSOutlineViewDelegate`) 機能を選択するアウトラインの種類をサポートするために選択し、編集、カスタムの追跡、および個々 のアイテムのカスタム ビューの項目。

ソースの一覧は、テーブルのビューおよびアウトライン ビューとの動作と機能の多くを共有、以降は、通過することができます、[テーブル ビュー](~/mac/user-interface/table-view.md)と[アウトライン ビュー](~/mac/user-interface/outline-view.md)操作を続行する前にドキュメントこの記事ではします。

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>ソース リストを使用します。

ソースの一覧は、特殊な種類のアウトライン表示 Finder または iTunes でサイド バーと同様に、操作のソースを表示するために使用します。 アウトライン表示とは異なりインターフェイス ビルダーで、[ソース] ボックスの一覧を定義して前に作成してみましょうバッキング クラス Xamarin.Mac で。

最初に、新しいを作成してみましょう`SourceListItem`ソース リストからデータを保持するクラス。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`SourceListItem`の**名前**] をクリック、**新規**ボタン。

[![](source-list-images/source01.png "空のクラスを追加します。")](source-list-images/source01.png#lightbox)

ように、`SourceListItem.cs`次のようなファイルの内容。 

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

**ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`SourceListDataSource`の**名前**] をクリック、**新規**ボタンをクリックします。 ように、`SourceListDataSource.cs`次のようなファイルの内容。

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

これは、ソース リストからデータを提供します。

**ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`SourceListDelegate`の**名前**] をクリック、**新規**ボタンをクリックします。 ように、`SourceListDelegate.cs`次のようなファイルの内容。

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

これにより、ソース一覧の動作が提供されます。

最後に、**ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`SourceListView`の**名前**] をクリック、**新規**ボタンをクリックします。 ように、`SourceListView.cs`次のようなファイルの内容。

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

これにより、作成の再利用可能なカスタム サブクラス`NSOutlineView`(`SourceListView`) を行う場合を Xamarin.Mac アプリケーションで元のリストをドライブに使用できます。

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>作成して、Xcode でのソースの一覧を維持します。

ここで、インターフェイスのビルダーで、ソース リストを設計してみましょう。 ダブルクリックして、`Main.storyboard`ファイルをインターフェイスのビルダーで編集してファイルを開いてから分割ビューをドラッグして、**ライブラリ インスペクター**ビュー コント ローラーを追加、およびビューでのサイズを変更するように設定、**制約エディター**:

[![](source-list-images/source00.png "制約の編集")](source-list-images/source00.png#lightbox)

次に、ソースの一覧からドラッグして、**ライブラリ インスペクター**、分割ビューの左側に追加しとで、ビューのサイズを変更するように設定、**制約エディター**:

[![](source-list-images/source02.png "制約の編集")](source-list-images/source02.png#lightbox)

次に切り替え、 **Identity ビュー**ソース リストを選択し、変更の**クラス**に`SourceListView`:

[![](source-list-images/source03.png "クラス名を設定します。")](source-list-images/source03.png#lightbox)

最後に、作成、**コンセント**、ソース リストと呼ばれる`SourceList`で、`ViewController.h`ファイル。

[![](source-list-images/source04.png "コンセントを構成します。")](source-list-images/source04.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>ソースの一覧を設定します。

編集しましょう、 `RotationWindow.cs` Mac 用の Visual Studio でファイルをそれの`AwakeFromNib`次のようなメソッドの検索。

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

`Initialize ()`ソース一覧のに対して呼び出されるメソッドが必要な**コンセント**_する前に_にすべての項目が追加されます。 アイテムの各グループの親項目を作成し、そのグループの項目にサブ項目を追加します。 各グループは、ソース リストのコレクションに追加し、`SourceList.AddItem (...)`です。 最後の 2 行は、ソースの一覧については、データを読み込むし、すべてのグループを展開します。

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

最後に、編集、`AppDelegate.cs`ファイルし、`DidFinishLaunching`次のようなメソッドの検索。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

アプリケーションを実行する場合、次が表示されます。

[![](source-list-images/source05.png "実行のサンプル アプリ")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでのソースの一覧の使用についての詳細を取得しました。 作成して、Xcode のインターフェイスのビルダー内のソースの一覧を管理する方法と c# コードでソースの一覧を使用する方法を説明しました。

## <a name="related-links"></a>関連リンク

- [MacOutlines (サンプル)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [ビューを説明する概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
