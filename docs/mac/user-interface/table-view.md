---
title: テーブルのビュー
description: この記事では、表形式ビュー Xamarin.Mac アプリケーションでの操作について説明します。 これには、Xcode、インターフェイスのビルダー、およびコード内とやり取りする内のテーブル ビューの作成について説明します。
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c274405613f079cb61ad9c96497a9effdc7173f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="table-views"></a>テーブルのビュー

_この記事では、表形式ビュー Xamarin.Mac アプリケーションでの操作について説明します。これには、Xcode、インターフェイスのビルダー、およびコード内とやり取りする内のテーブル ビューの作成について説明します。_

Xamarin.Mac アプリケーションでは、c# と .NET で作業するときに同じアクセスがあるテーブルをビューで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、テーブルのビューの管理 (または必要に応じて c# コードで直接作成すること)。

テーブル ビューでは、複数の行の情報の 1 つまたは複数の列を含む表形式でデータを表示します。 作成されるテーブル ビューの種類に基づいて、ユーザーことができますの列で並べ替える、列の再編成、列の追加、列を削除またはテーブル内に含まれるデータを編集します。

[![](table-view-images/intro01.png "表な例")](table-view-images/intro01.png#lightbox)

この記事で Xamarin.Mac アプリケーションでテーブルのビューの操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>テーブルのビューの概要

テーブル ビューでは、複数の行の情報の 1 つまたは複数の列を含む表形式でデータを表示します。 スクロール ビュー内でテーブルのビューが表示されます (`NSScrollView`) macOS 10.7 から始めて、使用できますと`NSView`セルではなく (`NSCell`) を行と列の両方を表示します。 ただし、使用することもできます`NSCell`ただし、次の手順通常サブクラス`NSTableCellView`し、カスタムの行と列を作成します。

データ ソースには代わりに、テーブル ビューでは、独自のデータを格納しません (`NSTableViewDataSource`) を行と、必要に応じてごとに、必要な列の両方を提供します。

テーブル ビューのデリゲートのサブクラスを提供することによって、テーブル ビューの動作をカスタマイズすることができます (`NSTableViewDelegate`) テーブルの列の管理をサポートする機能、行の選択、編集、カスタムの追跡、および個々 の列のカスタム ビューを選択する種類と行です。

テーブルのビューを作成するときに Apple 次示しています。

* 列ヘッダーをクリックしてテーブルの並べ替えにユーザーを許可します。
* 名詞またはその列に表示されているデータを説明する短い名詞句は、列ヘッダーを作成します。

詳細についてを参照してください、[コンテンツ ビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>作成し、Xcode でのテーブルのビューの管理

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイル。

[![](table-view-images/edit01.png "メインのストーリー ボードを選択します。")](table-view-images/edit01.png#lightbox)

Xcode のインターフェイスのビルダーで、ウィンドウのデザインが開きます。

[![](table-view-images/edit02.png "Xcode で UI を編集")](table-view-images/edit02.png#lightbox)

型`table`に、**ライブラリ インスペクター**テーブル ビュー コントロールを検索するが容易に検索ボックス。

[![](table-view-images/edit03.png "ライブラリからテーブル ビューの選択")](table-view-images/edit03.png#lightbox)

テーブル ビューでビュー コント ローラーにドラッグ、**インターフェイス エディター**、ビュー コント ローラーのコンテンツ領域を入力し、縮小して、ウィンドウにも拡張に設定して、**制約エディター**:

[![](table-view-images/edit04.png "制約の編集")](table-view-images/edit04.png#lightbox)

テーブル ビューを選択して、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](table-view-images/edit05.png "属性の検査")](table-view-images/edit05.png#lightbox)

- **コンテンツのモード**-いずれかのビューを使用することができます (`NSView`) またはセル (`NSCell`) を行と列にデータを表示します。 MacOS 10.7 から始めて、ビューを使用する必要があります。
- **行をグループ化を寄せて配置**場合 - `true`、表形式ビューは、フローティング、グループ化セルが描画されます。
- **列**に表示される列の数を定義します。
- **ヘッダー**場合 - `true`、列ヘッダーになります。
- **並べ替え**場合 -`true`ユーザーがドラッグできる、テーブル内の列の順序を変更します。
- **サイズ変更**場合 -`true`ユーザーが列のサイズを変更する列ヘッダーをドラッグできます。
- **列のサイズ変更**-どのテーブルが自動的にサイズの列を制御します。
- **強調表示**-コントロールのセルが選択されているときに使用するテーブルを強調表示の種類。
- **代替行**場合 - `true`、これまで他の行が別の背景色になります。
- **水平グリッド**-横方向に描画セル間の境界線の種類を選択します。
- **垂直グリッド**-セル間垂直方向に描画される境界線の種類を選択します。
- **グリッド色**のセルの境界線の色を設定します。
- **バック グラウンド**のセルの背景色を設定します。
- **選択**-とテーブルに、ユーザーがセルを選択する方法を制御できます。
    - **複数**場合 -`true`ユーザーは、複数の行と列を選択できます。
    - **列**場合 -`true`ユーザーが列を選択できます。
    - **入力選択**場合 -`true`ユーザーが行を選択する文字を入力します。
    - **空**場合 -`true`ユーザーは、行または列を選択する必要はありません、テーブルでは、選択されていないすべてのです。
- **自動保存**の下にあるテーブルの形式が自動的に名前を保存します。
- **列情報**場合 -`true`列の幅と順序が自動的に保存されます。
- **改行**- セルは改行文字を処理する方法を選択します。
- **表示されている最後の行を切り捨てます**場合 -`true`セルの境界内に収まらないデータで切り捨てられます。

> [!IMPORTANT]
> 従来の Xamarin.Mac アプリケーションを保持している場合を除き、`NSView`経由でベース テーブルのビューを使用する必要があります`NSCell`ベース テーブルのビューです。 `NSCell` レガシと見なされ、今後はサポートされません。

テーブルの列を選択して、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](table-view-images/edit06.png "属性の検査")](table-view-images/edit06.png#lightbox)

- **タイトル**-列のタイトルを設定します。
- **配置**-セル内でテキストの配置を設定します。
- **タイトルのフォント**のセルのヘッダー テキストのフォントを選択します。
- **並べ替えキー** -列のデータの並べ替えに使用するキーします。 空白のままに場合は、ユーザーは、この列を並べ替えることはできません。
- **セレクター** -は、**アクション**並べ替えを実行するために使用します。 空白のままに場合は、ユーザーは、この列を並べ替えることはできません。
- **順序**-は、列のデータの並べ替え順序。
- **サイズ変更**-列のサイズ変更の種類を選択します。
- **編集可能な**場合 -`true`ユーザーがセルのベース テーブル内のセルを編集できます。
- **非表示に**場合 -`true`列を非表示にします。

列を変更するには、そのハンドル (列の右側に垂直方向に中央揃え) の左または右にドラッグすることもできます。

みましょう、テーブル ビューで、各列を選択し、最初の列を与える、**タイトル**の`Product`し、もう 1 つ`Details`です。

テーブル セルのビューを選択 (`NSTableViewCell`) で、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](table-view-images/edit07.png "属性の検査")](table-view-images/edit07.png#lightbox)

これらは、すべての標準的なビューのプロパティです。 また、この列は、ここに行サイズの変更オプションがあります。

テーブル ビューのセルを選択 (既定では、これは、 `NSTextField`) で、**インターフェイス階層**あり、次のプロパティでは使用、**属性インスペクター**:

[![](table-view-images/edit08.png "属性の検査")](table-view-images/edit08.png#lightbox)

ここで設定する標準的なテキスト フィールドのすべてのプロパティがあります。 既定では、標準的なテキスト フィールドは列のセルのデータを表示するために使用します。

テーブル セルのビューを選択 (`NSTableFieldCell`) で、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](table-view-images/edit09.png "属性の検査")](table-view-images/edit09.png#lightbox)

ここでの最も重要な設定は次のとおりです。

- **レイアウト**- この列のセルのレイアウト方法を選択します。
- **単一行モードを使用して**場合 -`true`セルが 1 行に制限されます。
- **最初のランタイム レイアウト**場合 -`true`セルの幅を設定 (手動または自動的に) とを選びます初めてアプリケーションを実行して表示されます。
- **アクション**のタイミングを制御、編集**アクション**セルに送信します。
- **動作**のセルが選択可能または編集可能な場合を定義します。
- **リッチ テキスト**場合 -`true`セルは、書式設定およびスタイル設定されたテキストを表示できます。
- **元に戻す**場合 - `true`、に対しては、セルが責任を引き継ぎます動作を元に戻します。

テーブル セルのビューを選択 (`NSTableFieldCell`) のテーブルの列の下部にある、**インターフェイス階層**:

[![](table-view-images/edit10.png "テーブル セルのビューを選択します。")](table-view-images/edit10.png#lightbox)

これにより、ベースとして使用されるテーブル セルのビューを編集して_パターン_指定された列用に作成されたすべてのセル。

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>アクションとコンセントを追加します。

だけ Cocoa UI コントロールと同様に、テーブル ビューを公開する必要があり、その列は、c# コードを使用するセル**アクション**と**コンセント**(必要な機能に基づく)。

プロセスは、テーブル ビュー要素を公開する必要の場合と同じです。

1. 切り替えて、**アシスタント エディター**ことを確認して、`ViewController.h`ファイルが選択されています。 

    [![](table-view-images/edit11.png "アシスタント エディター")](table-view-images/edit11.png#lightbox)
2. テーブル ビューを選択して、**インターフェイス階層**コントロールをクリックし、ドラッグする、`ViewController.h`ファイル。
3. 作成、**コンセント**テーブル ビューと呼ばれる`ProductTable`: 

    [![](table-view-images/edit13.png "コンセントを構成します。")](table-view-images/edit13.png#lightbox)
4. 作成**コンセント**テーブルの列にも呼び出さ`ProductColumn`と`DetailsColumn`: 

    [![](table-view-images/edit14.png "コンセントを構成します。")](table-view-images/edit14.png#lightbox)
5. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

次に、作成してコードの表示、テーブルの一部のデータ、アプリケーションの実行時にします。

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>テーブル ビューを設定します。

テーブルの私たちのインターフェイスのビルダーでデザインし、を介して公開される、**コンセント**、次に読み込むための c# コードを作成する必要があります。

最初に、新しいを作成してみましょう`Product`個々 の行の情報を保持するクラス。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`Product`の**名前**] をクリック、**新規**ボタン。

[![](table-view-images/populate01.png "空のクラスを作成します。")](table-view-images/populate01.png#lightbox)

ように、`Product.cs`次のようなファイルの内容。

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}

```

次に、私たちのサブクラスを作成する必要があります`NSTableDataSource`が要求されるように、このテーブルのデータを提供します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`ProductTableDataSource`の**名前**] をクリック、**新規**ボタンをクリックします。

編集、`ProductTableDataSource.cs`ファイルし、次のようになります。

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

このクラスは、テーブル ビューの項目の記憶域を持つし、オーバーライド、`GetRowCount`をテーブルの行の数を返します。

最後に、私たちのサブクラスを作成する必要があります`NSTableDelegate`をこのテーブルの動作を提供します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`ProductTableDelegate`の**名前**] をクリック、**新規**ボタンをクリックします。

編集、`ProductTableDelegate.cs`ファイルし、次のようになります。

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

インスタンスを作成するとき、 `ProductTableDelegate`、私たちのインスタンスに渡すことも、`ProductTableDataSource`テーブルのデータを提供します。 `GetViewForItem`メソッドは、指定した列と行のセルを表示するビュー (データ) を返すことを担当します。 可能であれば、既存のビューは、セルを表示する再利用する以外の場合、新しいビューを作成する必要があります。

テーブル作成するには、編集、`ViewController.cs`ファイルし、`AwakeFromNib`次のようなメソッドの検索。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

アプリケーションを実行している場合、次のように表示されます。

[![](table-view-images/populate02.png "実行のサンプル アプリ")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>列による並べ替え

ユーザーを列ヘッダーをクリックして、テーブル内のデータを並べ替えるを許可してみましょう。 最初をダブルクリックして、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 選択、`Product`列、入力`Title`の**並べ替えキー**、`compare:`の**セレクター**を選択し、`Ascending`の**順序**:

[![](table-view-images/sort01.png "並べ替えキーの設定")](table-view-images/sort01.png#lightbox)

選択、`Details`列、入力`Description`の**並べ替えキー**、`compare:`の**セレクター**を選択し、`Ascending`の**順序**:

[![](table-view-images/sort02.png "並べ替えキーの設定")](table-view-images/sort02.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ編集しましょう、`ProductTableDataSource.cs`ファイルし、次のメソッドを追加します。

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

`Sort`メソッドにより、データに基づくデータ ソースの並べ替え、指定された`Product`昇順または降順のどちらかのクラス フィールドです。 オーバーライドされた`SortDescriptorsChanged`を使用する列見出しをクリックするたびにメソッドが呼び出されます。 渡されます、**キー**インターフェイス ビルダーとその列の並べ替え順序で設定した値がします。

アプリケーションを実行すると、列ヘッダーをクリックして、その列で行が並べ替えられます。

[![](table-view-images/sort03.png "実行のサンプル アプリ")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>行の選択

1 つの行を選択し、ダブルクリックして、ユーザーを許可するかどうか、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 テーブル ビューを選択して、**インターフェイス階層**をオフにし、**複数**のチェック ボックス、**属性インスペクター**:

[![](table-view-images/select01.png "属性の検査")](table-view-images/select01.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。


次に、編集、`ProductTableDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

これによって、ユーザーのテーブル ビューの 1 行を選択します。 返す`false`の`ShouldSelectRow`のいずれかの行を選択できるユーザーをたくないまたは`false`すべての行が任意の行を選択できるユーザーをたくない場合。

テーブル ビュー (`NSTableView`) 行の選択を操作するための次のメソッドが含まれています。

- `DeselectRow(nint)` -テーブルの特定の行を選択解除します。
- `SelectRow(nint,bool)` -特定の行を選択します。 渡す`false`2 番目のパラメーターを一度に 1 行のみを選択します。
- `SelectedRow` -テーブルで選択されている現在の行を返します。
- `IsRowSelected(nint)` -返します`true`特定の行が選択されている場合。

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>複数行の選択

複数の行を選択し、ダブルクリックして、ユーザーを許可するかどうか、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 テーブル ビューを選択して、**インターフェイス階層**を確認し、**複数**のチェック ボックス、**属性インスペクター**:

[![](table-view-images/select02.png "属性の検査")](table-view-images/select02.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。


次に、編集、`ProductTableDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

これによって、ユーザーのテーブル ビューの 1 行を選択します。 返す`false`の`ShouldSelectRow`のいずれかの行を選択できるユーザーをたくないまたは`false`すべての行が任意の行を選択できるユーザーをたくない場合。

テーブル ビュー (`NSTableView`) 行の選択を操作するための次のメソッドが含まれています。

- `DeselectAll(NSObject)` -テーブル内のすべての行を選択解除します。 使用して`this`最初のパラメーターを選択を行っているオブジェクトに送信します。 
- `DeselectRow(nint)` -テーブルの特定の行を選択解除します。
- `SelectAll(NSobject)` -テーブルのすべての行を選択します。 使用して`this`最初のパラメーターを選択を行っているオブジェクトに送信します。
- `SelectRow(nint,bool)` -特定の行を選択します。 渡す`false`2 番目のパラメーターの選択をクリアし、単一行のみを選択、渡す`true`を選択範囲を拡張し、この行を含めます。
- `SelectRows(NSIndexSet,bool)` -指定した行のセットを選択します。 渡す`false`2 番目のパラメーターの選択を解除し、選択、のみこれらの行を渡す`true`を選択範囲を拡張し、これらの行を含めます。
- `SelectedRow` -テーブルで選択されている現在の行を返します。
- `SelectedRows` 返します、`NSIndexSet`選択された行のインデックスを格納します。
- `SelectedRowCount` -選択した行の数を返します。
- `IsRowSelected(nint)` -返します`true`特定の行が選択されている場合。

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>行を選択する種類

その文字を持つテーブル ビューが選択されている文字を入力するユーザーを許可して、最初の行を選択する場合、ダブルクリック、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 テーブル ビューを選択して、**インターフェイス階層**を確認し、**の種類を選択**のチェック ボックス、**属性インスペクター**:

[![](table-view-images/type01.png "選択範囲の種類を設定")](table-view-images/type01.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ編集しましょう、`ProductTableDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

`GetNextTypeSelectMatch`メソッドは、指定された`searchString`し、最初の行を返す`Product`にその文字列を含む`Title`です。

アプリケーションを実行すると、文字を入力、行を選択します。

[![](table-view-images/type02.png "実行のサンプル アプリ")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>列の並べ替え

ユーザーがドラッグできるようにする場合は、テーブル ビューで列を並べ替えるをダブルクリック、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 テーブル ビューを選択して、**インターフェイス階層**を確認し、**並べ替え**のチェック ボックス、**属性インスペクター**:

[![](table-view-images/reorder01.png "属性の検査")](table-view-images/reorder01.png#lightbox)

値を提供する場合、**自動保存**プロパティとチェック、**列情報**フィールドでは、テーブルのレイアウトを変更しましたがご利用の米国自動的に保存され、次回アプリケーションを復元実行されます。

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ編集しましょう、`ProductTableDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder`メソッドが返す`true`ドラッグに並べ替えることを許可する任意の列の`newColumnIndex`、それ以外の場合の戻り値`false`;

アプリケーションを実行する場合に、列の順序を変更する周囲の列ヘッダーをドラッグしたことができます。

[![](table-view-images/reorder02.png "並べ替えられた列の例")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>セルの編集

ユーザーが指定されたセルの値を編集、編集を許可する場合、`ProductTableDelegate.cs`ファイルし、変更、`GetViewForItem`メソッドを次のようにします。

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

今すぐアプリケーションを実行して、ユーザーはテーブル ビュー内のセルを編集できます。

[![](table-view-images/editing01.png "セルの編集の例")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>テーブルのビューでのイメージの使用

内のセルの一部として画像を含める、 `NSTableView`、データをテーブル ビューのによって返される方法を変更する必要があります`NSTableViewDelegate's``GetViewForItem`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

詳細についてを参照してください、[テーブルのビューを使用してイメージ](~/mac/app-fundamentals/image.md)のセクションで、[画像](~/mac/app-fundamentals/image.md)ドキュメント。

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>行を削除 ボタンを追加します。

アプリの要件に基づき、ありますテーブルの各行の動作設定ボタンを指定する必要。 これの例は、上記で作成した対象テーブル ビューの例の拡大みましょう、**削除**行ごとにボタンをクリックします。

最初に、編集、 `Main.storyboard` Xcode のインターフェイス ビルダーでは、テーブル ビューを選択し、列の数を増やす 3 (3)。 次に、変更、**タイトル**する新しい列の`Action`:

[![](table-view-images/delete01.png "列名の編集")](table-view-images/delete01.png#lightbox)

ストーリー ボードに変更を保存し、Mac の変更を同期用の Visual Studio に戻ります。

次に、編集、`ViewController.cs`ファイルし、次のパブリック メソッドを追加します。

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

同じファイルで変更、デリゲートの作成、新しいテーブルのビュー内の`ViewDidLoad`メソッドを次のようにします。

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

ここで、編集、`ProductTableDelegate.cs`ファイル ビュー コント ローラーにプライベート接続を含めると、デリゲートの新しいインスタンスを作成するときに、パラメーターとして、コント ローラーを実行します。

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

次に、次の新しいプライベート メソッドをクラスに追加します。

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

これにかかる時間が以前に実行されているテキスト ビュー構成をすべて、`GetViewForItem`メソッド (テーブルの最後の列は、テキスト ビューがボタンには含まれません) してから、呼び出し可能な単一の場所に配置します。

最後に、編集、`GetViewForItem`メソッドされ次のようになります。

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btw = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

さらに詳しくは、このコードのセクションではいくつか見てみましょう。 まず、新しい`NSTableViewCell`がアクションを作成するに対してが行われるに基づいて列の名前。 最初の 2 つの列 (**製品**と**詳細**)、新しい`ConfigureTextField`メソッドが呼び出されます。

**アクション**列も、新しい`NSButton`が作成され、Sub ビューとしてのセルに追加します。

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

ボタンの`Tag`プロパティは、現在処理されている行の数を保持するために使用します。 ユーザーがボタンの削除する行を要求したときに、この番号を後で使用が`Activated`イベント。

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

イベント ハンドラーの初めにのボタンと、指定されたテーブル行にある製品を取得します。 アラートは、行の削除を確認するユーザーに表示されます。 ユーザーは、行を削除するが、データ ソースから特定の行が削除されたが、テーブルが再読み込み。

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

最後に、テーブル ビューのセルは、新規に作成されるのではなく再利用されるは、次のコードが構成処理中の列に基づくこと。

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

**アクション**列で、すべてのサブ ビュー スキャンまで、`NSButton`が見つかるが`Tag`プロパティが更新され、現在の行。

これらの場所の変更、アプリの実行時に各の行がある、**削除**ボタンをクリックします。

[![](table-view-images/delete02.png "削除ボタンを持つテーブル ビュー")](table-view-images/delete02.png#lightbox)

ユーザーがクリックしたとき、**削除**ボタン、特定の行を削除するように求める警告が表示されます。

[![](table-view-images/delete03.png "削除行のアラート")](table-view-images/delete03.png#lightbox)

場合は、ユーザーが、削除、行が削除され、テーブルが再描画されます。

[![](table-view-images/delete04.png "行が削除された後の表に、")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>データ バインディングのテーブルのビュー

Xamarin.Mac アプリケーションのコーディングのキー値と、データ バインドの手法を使用するの記述し、保守を作成し、UI 要素を使用する必要のあるコードの量を大幅に削減できます。 また、バックアップ データをさらに分離することの利点がある場合 (_データ モデル_)、正面からのユーザー インターフェイスを終了 (_モデル-ビュー-コント ローラー_) より柔軟なアプリケーションの保守が容易に先行します。デザインします。

キーと値のコーディング (KVC) は、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用して、オブジェクトのプロパティを直接アクセスするための機構 (`get/set`)。 Xamarin.Mac アプリの準拠のアクセサーをキーと値のコーディングを実装すると、キーと値を観察し (KVO)、データ バインディング、基本データ、Cocoa バインディング、および scriptability などその他の macOS 機能へのアクセスを取得します。

詳細についてを参照してください、[テーブル ビューのデータ バインド](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding)のセクションで、[データ バインディングと、キーと値のコーディング](~/mac/app-fundamentals/databinding.md)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでテーブルのビューの操作についての詳細を取得しました。 おにより、さまざまな種類を説明し、テーブルのビューを作成し、Xcode のインターフェイスのビルダーでテーブルのビューを管理する方法および c# コードで、テーブルのビューを使用する方法の使用します。

## <a name="related-links"></a>関連リンク

- [MacTables (サンプル)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [ソース リスト](~/mac/user-interface/source-list.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
