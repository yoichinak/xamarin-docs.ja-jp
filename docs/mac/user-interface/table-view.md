---
title: Xamarin. Mac のテーブルビュー
description: この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用について説明します。 Xcode と Interface Builder でのテーブルビューの作成と、コードでのテーブルビューの操作について説明します。
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 36bed05c1e60004125406c3ed2df66fcfe2be10b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008328"
---
# <a name="table-views-in-xamarinmac"></a>Xamarin. Mac のテーブルビュー

_この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用について説明します。Xcode と Interface Builder でのテーブルビューの作成と、コードでのテーブルビューの操作について説明します。_

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合、 *Xcode および*で作業している開発者が行うの*と同じ*テーブルビューにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してテーブルビューを作成および管理できます (また、必要C#に応じて、コード内で直接作成することもできます)。

テーブルビューでは、複数行の情報の1つ以上の列を含む表形式のデータが表示されます。 ユーザーは、作成されるテーブルビューの種類に基づいて、列の並べ替え、列の再編成、列の追加、列の削除、テーブル内のデータの編集を行うことができます。

[![](table-view-images/intro01.png "An example table")](table-view-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用に関する基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

[Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの「 C# [クラス/ C#メソッドを目的](~/mac/internals/how-it-works.md)として公開する」セクションを参照することもできます。ここでは、クラスを目的のために接続するために使用する`Register`と`Export`のコマンドについて説明します。オブジェクトと UI 要素。

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>テーブルビューの概要

テーブルビューでは、複数行の情報の1つ以上の列を含む表形式のデータが表示されます。 テーブルビューは、スクロールビュー (`NSScrollView`) 内に表示され、macOS 10.7 以降では、セルではなく任意の `NSView` (`NSCell`) を使用して、行と列の両方を表示できます。 ただし、`NSCell` を使用することもできますが、通常は `NSTableCellView` をサブクラス化して、カスタムの行と列を作成します。

テーブルビューには独自のデータは格納されません。代わりに、必要に応じて、データソース (`NSTableViewDataSource`) を使用して必要な行と列の両方を提供します。

テーブルビューの動作をカスタマイズするには、テーブルビューデリゲート (`NSTableViewDelegate`) のサブクラスを使用して、テーブルの列の管理、型の選択、機能の選択、行の選択と編集、カスタムの追跡、および個々の列と行のカスタムビューをサポートします。

Apple では、テーブルビューを作成するときに次のことが提案されます。

- 列ヘッダーをクリックして、ユーザーがテーブルを並べ替えることができるようにします。
- 列に表示されるデータを記述する名詞または短い名詞の語句である列ヘッダーを作成します。

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[コンテンツビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) 」セクションを参照してください。

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Xcode でのテーブルビューの作成と管理

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは、プロジェクトに自動的に含まれる `.storyboard` ファイルで定義されています。 Windows のデザインを編集するには、**ソリューションエクスプローラー**で、`Main.storyboard` ファイルをダブルクリックします。

[![](table-view-images/edit01.png "Selecting the main storyboard")](table-view-images/edit01.png#lightbox)

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![](table-view-images/edit02.png "Editing the UI in Xcode")](table-view-images/edit02.png#lightbox)

**ライブラリインスペクターの**検索ボックスに「`table`」と入力して、テーブルビューコントロールを簡単に見つけられるようにします。

[![](table-view-images/edit03.png "Selecting a Table View from the Library")](table-view-images/edit03.png#lightbox)

**インターフェイスエディター**で、ビューコントローラーにテーブルビューをドラッグし、ビューコントローラーのコンテンツ領域を塗りつぶすように設定します。次に、**制約エディター**でウィンドウのサイズを縮小し、拡大する場所に設定します。

[![](table-view-images/edit04.png "Editing constraints")](table-view-images/edit04.png#lightbox)

**インターフェイス階層**でテーブルビューを選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit05.png "The Attribute Inspector")](table-view-images/edit05.png#lightbox)

- **コンテンツモード**-ビュー (`NSView`) またはセル (`NSCell`) のいずれかを使用して、行と列のデータを表示できます。 MacOS 10.7 以降では、ビューを使用する必要があります。
- [**フローティンググループ行]** -`true`の場合、テーブルビューでは、グループ化されたセルが浮動小数点型として描画されます。
- **Columns** -表示される列の数を定義します。
- **ヘッダー** -`true`場合、列にはヘッダーがあります。
- **[並べ替え]** -`true`した場合、ユーザーはテーブル内の列の順序を変更することができます。
- **[サイズ変更]** -`true`すると、ユーザーは列ヘッダーをドラッグして列のサイズを変更できるようになります。
- **列**のサイズ変更-テーブルの列のサイズを自動調整する方法を制御します。
- **強調表示**-セルを選択したときにテーブルで使用する強調表示の種類を制御します。
- **代替行**-`true`の場合、他の行の背景色は異なります。
- **水平グリッド**-横方向のセルの間に描画する境界線の種類を選択します。
- **垂直グリッド**-セル間に描画する境界線の種類を垂直方向に選択します。
- **グリッドの色**-セルの境界線の色を設定します。
- **Background** -セルの背景色を設定します。
- **選択**-ユーザーがテーブル内のセルを選択する方法を、次のように制御できます。
  - **Multiple** -`true`場合、ユーザーは複数の行と列を選択できます。
  - **列**-`true`の場合、ユーザーは列を選択できます。
  - **型の選択**-`true`した場合、ユーザーは文字を入力して行を選択できます。
  - **Empty** -`true`の場合、ユーザーは行または列を選択する必要がないため、テーブルでは何も選択できません。
- [**自動**保存]-テーブル形式が自動的に保存される名前。
- **列情報**-`true`場合、列の順序と幅が自動的に保存されます。
- [**改行]-セル**が改行を処理する方法を選択します。
- **最後に表示**される行を切り捨てます。 `true`した場合、データがその範囲内に収まりきらない場合に、セルが切り捨てられます。

> [!IMPORTANT]
> 従来の Xamarin. Mac アプリケーションを維持していない場合は、`NSCell` ベースのテーブルビューで `NSView` ベースのテーブルビューを使用する必要があります。 `NSCell` は従来と見なされ、今後サポートされない可能性があります。

**インターフェイス階層**内のテーブル列を選択します。**属性インスペクター**では、次のプロパティを使用できます。

[![](table-view-images/edit06.png "The Attribute Inspector")](table-view-images/edit06.png#lightbox)

- **タイトル**-列のタイトルを設定します。
- **Alignment** -セル内のテキストの配置を設定します。
- **[タイトルのフォント]** -セルのヘッダーテキストのフォントを選択します。
- **Sort key** -列のデータを並べ替えるために使用されるキーです。 ユーザーがこの列を並べ替えられない場合は、空白のままにします。
- **Selector** -並べ替えを実行するために使用される**アクション**です。 ユーザーがこの列を並べ替えられない場合は、空白のままにします。
- **Order** -列データの並べ替え順序です。
- **サイズ変更**中-列のサイズ変更の種類を選択します。
- **編集可能**-`true`した場合、ユーザーはセルベーステーブル内のセルを編集できます。
- **Hidden** -`true`の場合、列は非表示になります。

また、列のハンドル (垂直方向に列の右側に中央揃え) をドラッグして列のサイズを変更することもできます。

テーブルビューの各列を選択し、最初の列に `Product` の**タイトル**を指定し、2つ目の列に `Details`を付けます。

**インターフェイス階層**でテーブルセルビュー (`NSTableViewCell`) を選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit07.png "The Attribute Inspector")](table-view-images/edit07.png#lightbox)

これらは、標準ビューのすべてのプロパティです。 また、この列の行のサイズを変更することもできます。

**インターフェイス階層**でテーブルビューセル (既定では `NSTextField`) を選択します。**属性インスペクター**では、次のプロパティを使用できます。

[![](table-view-images/edit08.png "The Attribute Inspector")](table-view-images/edit08.png#lightbox)

ここでは、標準のテキストフィールドのすべてのプロパティを設定します。 既定では、標準のテキストフィールドを使用して、列のセルのデータが表示されます。

**インターフェイス階層**でテーブルセルビュー (`NSTableFieldCell`) を選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit09.png "The Attribute Inspector")](table-view-images/edit09.png#lightbox)

ここで最も重要な設定は次のとおりです。

- **[レイアウト]** -この列のセルのレイアウトを選択します。
- **単一行モードを使用**します。 `true`の場合、セルは1行に制限されます。
- **最初の実行時レイアウトの幅**-`true`した場合、このセルには、アプリケーションを初めて実行したときに表示される幅 (手動または自動) が優先されます。
- **Action** -セルに対して編集**アクション**が送信されるタイミングを制御します。
- **Behavior** -セルを選択可能にするか、編集可能にするかを定義します。
- **リッチテキスト**-`true`した場合、セルには書式設定されたテキストを表示できます。
- **[元に戻す]** -`true`の場合、セルは元に戻す動作の役割を担います。

**インターフェイス階層**のテーブル列の下部にあるテーブルセルビュー (`NSTableFieldCell`) を選択します。

[![](table-view-images/edit10.png "Selecting the Table Cell View")](table-view-images/edit10.png#lightbox)

これにより、特定の列に対して作成されたすべてのセルのベース_パターン_として使用されるテーブルセルビューを編集できます。

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>アクションとアウトレットの追加

他の Cocoa UI コントロールと同様に、テーブルビューとその列とセルは、**アクション**と**アウトレット**( C#必要な機能に基づく) を使用してコードに公開する必要があります。

このプロセスは、公開するすべてのテーブルビュー要素で同じです。

1. **アシスタントエディター**に切り替えて、`ViewController.h` ファイルが選択されていることを確認します。 

    [![](table-view-images/edit11.png "The Assistant Editor")](table-view-images/edit11.png#lightbox)
2. **インターフェイス階層**からテーブルビューを選択し、コントロールをクリックして、`ViewController.h` ファイルにドラッグします。
3. `ProductTable`というテーブルビューの**アウトレット**を作成します。 

    [![](table-view-images/edit13.png "Configuring an Outlet")](table-view-images/edit13.png#lightbox)
4. `ProductColumn` と `DetailsColumn`と呼ばれるテーブル列の**アウトレット**を作成します。 

    [![](table-view-images/edit14.png "Configuring an Outlet")](table-view-images/edit14.png#lightbox)
5. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、アプリケーションの実行時にテーブルのデータを表示するコードを記述します。

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>テーブルビューの設定

Interface Builder で設計され、**アウトレット**を介して公開されるテーブルビューではC# 、次にコードを作成して設定する必要があります。

まず、個々の行の情報を保持する新しい `Product` クラスを作成してみましょう。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、 > 新しいファイルの**追加** **...**  を選択します。**全般** > **空のクラス** を選択し、**名前**として「`Product`」と入力して、**新規** ボタンをクリックします。

[![](table-view-images/populate01.png "Creating an empty class")](table-view-images/populate01.png#lightbox)

`Product.cs` ファイルは次のようになります。

```csharp
using System;

namespace MacTables
{
  public class Product
  {
    #region Computed Properties
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

次に、要求に応じてテーブルのデータを提供するために、`NSTableDataSource` のサブクラスを作成する必要があります。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、 > 新しいファイルの**追加** **...**  を選択します。**全般** > **空のクラス** を選択し、**名前**として「`ProductTableDataSource`」と入力して、**新規** ボタンをクリックします。

`ProductTableDataSource.cs` ファイルを編集し、次のように表示します。

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

このクラスには、テーブルビューの項目のストレージが含まれており、テーブルの行数を返すために `GetRowCount` をオーバーライドします。

最後に、`NSTableDelegate` のサブクラスを作成して、テーブルの動作を指定する必要があります。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、 > 新しいファイルの**追加** **...**  を選択します。**全般** > **空のクラス** を選択し、**名前**として「`ProductTableDelegate`」と入力して、**新規** ボタンをクリックします。

`ProductTableDelegate.cs` ファイルを編集し、次のように表示します。

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

`ProductTableDelegate`のインスタンスを作成するときに、テーブルのデータを提供する `ProductTableDataSource` のインスタンスも渡します。 `GetViewForItem` メソッドは、ビュー (データ) を取得して、列と行の列のセルを表示する役割を担います。 可能であれば、新しいビューを作成する必要がない場合は、既存のビューを再利用してセルを表示します。

テーブルを作成するには、`ViewController.cs` ファイルを編集し、`AwakeFromNib` メソッドを次のようにします。

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

アプリケーションを実行すると、次のように表示されます。

[![](table-view-images/populate02.png "A sample app run")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>列で並べ替え

ユーザーが列ヘッダーをクリックしてテーブル内のデータを並べ替えることができるようにします。 まず、`Main.storyboard` ファイルをダブルクリックして、Interface Builder で編集するために開きます。 [`Product`] 列を選択し、**並べ替えキー**として「`Title`」と入力し、**セレクター**に `compare:` を入力して、**注文**の `Ascending` を選択します。

[![](table-view-images/sort01.png "Setting the sort key")](table-view-images/sort01.png#lightbox)

[`Details`] 列を選択し、**並べ替えキー**として「`Description`」と入力し、**セレクター**に `compare:` を入力して、**注文**の `Ascending` を選択します。

[![](table-view-images/sort02.png "Setting the sort key")](table-view-images/sort02.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、`ProductTableDataSource.cs` ファイルを編集し、次のメソッドを追加します。

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

`Sort` メソッドを使用すると、指定された `Product` クラスフィールドに基づいて、昇順または降順にデータソースのデータを並べ替えることができます。 オーバーライドされた `SortDescriptorsChanged` メソッドは、が列見出しをクリックするたびに呼び出されます。 Interface Builder に設定した**キー**値とその列の並べ替え順序が渡されます。

アプリケーションを実行して列ヘッダーをクリックすると、行はその列で並べ替えられます。

[![](table-view-images/sort03.png "An example app run")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>行の選択

ユーザーが単一行を選択できるようにするには、`Main.storyboard` ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[複数]** チェックボックスをオフにします。

[![](table-view-images/select01.png "The Attribute Inspector")](table-view-images/select01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、`ProductTableDelegate.cs` ファイルを編集し、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

これにより、ユーザーはテーブルビューの任意の1行を選択できるようになります。 ユーザーが行を選択できないようにする場合は、すべての行についてユーザーが選択または `false` できないようにするには、`ShouldSelectRow` の `false` を返します。

テーブルビュー (`NSTableView`) には、行の選択を操作するための次のメソッドが含まれています。

- `DeselectRow(nint)`-テーブル内の指定された行を選択解除します。
- `SelectRow(nint,bool)`-指定された行を選択します。 2番目のパラメーターを一度に1行だけ選択するには、`false` を渡します。
- `SelectedRow`-テーブルで選択されている現在の行を返します。
- `IsRowSelected(nint)`-指定した行が選択されている場合に `true` を返します。

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>複数行の選択

ユーザーが複数の行を選択できるようにするには、`Main.storyboard` ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[複数]** チェックボックスをオンにします。

[![](table-view-images/select02.png "The Attribute Inspector")](table-view-images/select02.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、`ProductTableDelegate.cs` ファイルを編集し、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

これにより、ユーザーはテーブルビューの任意の1行を選択できるようになります。 ユーザーが行を選択できないようにする場合は、すべての行についてユーザーが選択または `false` できないようにするには、`ShouldSelectRow` の `false` を返します。

テーブルビュー (`NSTableView`) には、行の選択を操作するための次のメソッドが含まれています。

- `DeselectAll(NSObject)`-テーブル内のすべての行を選択解除します。 最初のパラメーターに `this` を使用して、選択したオブジェクトに送信します。 
- `DeselectRow(nint)`-テーブル内の指定された行を選択解除します。
- `SelectAll(NSobject)`-テーブル内のすべての行を選択します。 最初のパラメーターに `this` を使用して、選択したオブジェクトに送信します。
- `SelectRow(nint,bool)`-指定された行を選択します。 2番目のパラメーターに `false` を渡して選択範囲をクリアし、1行だけを選択します。次に、`true` を渡して選択範囲を拡張し、この行を含めます。
- `SelectRows(NSIndexSet,bool)`-指定された行のセットを選択します。 2番目のパラメーターに `false` を渡して選択範囲をクリアし、これらの行のみを選択します。次に、`true` を渡して選択範囲を拡張し、これらの行を含めます。
- `SelectedRow`-テーブルで選択されている現在の行を返します。
- `SelectedRows`-選択された行のインデックスを含む `NSIndexSet` を返します。
- `SelectedRowCount`-選択された行の数を返します。
- `IsRowSelected(nint)`-指定した行が選択されている場合に `true` を返します。

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>入力して行を選択します

ユーザーがテーブルビューを選択した状態で文字を入力し、その文字を含む最初の行を選択できるようにするには、`Main.storyboard` ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[型の選択**] チェックボックスをオンにします。

[![](table-view-images/type01.png "Setting the selection type")](table-view-images/type01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、`ProductTableDelegate.cs` ファイルを編集し、次のメソッドを追加します。

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

`GetNextTypeSelectMatch` メソッドは、指定された `searchString` を受け取り、その文字列が `Title`内にある最初の `Product` の行を返します。

アプリケーションを実行して文字を入力すると、行が選択されます。

[![](table-view-images/type02.png "A sample app run")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>列の並べ替え

ユーザーがテーブルビューの列の順序を変更できるようにするには、`Main.storyboard` ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[並べ替え]** チェックボックスをオンにします。

[![](table-view-images/reorder01.png "The Attribute Inspector")](table-view-images/reorder01.png#lightbox)

"**自動保存**" プロパティに値を指定して **[列情報]** フィールドを確認すると、テーブルのレイアウトに対して行った変更は自動的に保存され、次にアプリケーションが実行されたときに復元されます。

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、`ProductTableDelegate.cs` ファイルを編集し、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
  return true;
}
```

`ShouldReorder` メソッドは、ドラッグして `newColumnIndex`に移動できる任意の列に対して `true` を返します。それ以外の場合は `false`を返します。

アプリケーションを実行すると、列のヘッダーをドラッグして列の順序を変更できます。

[![](table-view-images/reorder02.png "An example of the reordered columns")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>セルの編集

ユーザーが特定のセルの値を編集できるようにするには、`ProductTableDelegate.cs` ファイルを編集し、次のように `GetViewForItem` メソッドを変更します。

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

アプリケーションを実行すると、ユーザーはテーブルビューのセルを編集できるようになります。

[![](table-view-images/editing01.png "An example of editing a cell")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>テーブルビューでの画像の使用

画像を `NSTableView` のセルの一部として含めるには、通常の `NSTextField` の代わりに `NSTableCellView` を使用するように、テーブルビューの `NSTableViewDelegate's` `GetViewForItem` メソッドによって返されるデータの方法を変更する必要があります。 (例:

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

詳細については、[イメージの操作](~/mac/app-fundamentals/image.md)に関するドキュメントの「[テーブルビューでのイメージの使用](~/mac/app-fundamentals/image.md)」セクションを参照してください。

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>行に Delete ボタンを追加する

アプリの要件に基づいて、テーブルの行ごとにアクションボタンを指定することが必要になる場合があります。 この例として、上記で作成したテーブルビューの例を拡張して、各行に **[削除]** ボタンを追加してみましょう。

まず、Xcode の Interface Builder で `Main.storyboard` を編集し、テーブルビューを選択して、列の数を3に増やします。 次に、新しい列の**タイトル**を `Action`に変更します。

[![](table-view-images/delete01.png "Editing the column name")](table-view-images/delete01.png#lightbox)

変更をストーリーボードに保存し Visual Studio for Mac に戻り、変更を同期します。

次に、`ViewController.cs` ファイルを編集し、次のパブリックメソッドを追加します。

```csharp
public void ReloadTable ()
{
  ProductTable.ReloadData ();
}
```

同じファイルで、`ViewDidLoad` メソッド内の新しいテーブルビューデリゲートの作成を次のように変更します。

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

次に、`ProductTableDelegate.cs` ファイルを編集して、ビューコントローラーへのプライベート接続を含め、デリゲートの新しいインスタンスを作成するときに、コントローラーをパラメーターとして取得します。

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

次に、次の新しいプライベートメソッドをクラスに追加します。

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

これにより、`GetViewForItem` メソッドで以前に実行されていたすべてのテキストビューの構成が取得され、1つの呼び出し可能な場所に配置されます (テーブルの最後の列にはテキストビューが含まれていますが、ボタンは含まれていないため)。

最後に、`GetViewForItem` メソッドを編集し、次のように表示します。

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
        var btn = sender as NSButton;
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
      var btn = subview as NSButton;
      if (btn != null) {
        btn.Tag = row;
      }
    }
    break;
  }

  return view;
}
```

このコードのいくつかのセクションをさらに詳しく見てみましょう。 最初に、新しい `NSTableViewCell` が作成されている場合は、列の名前に基づいてアクションが実行されます。 最初の2つの列 (**Product**および**Details**) では、新しい `ConfigureTextField` メソッドが呼び出されます。

**[アクション]** 列には、新しい `NSButton` が作成され、そのセルにサブビューとして追加されます。

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

ボタンの `Tag` プロパティは、現在処理中の行の番号を保持するために使用されます。 この数値は、後でユーザーがボタンの `Activated` イベントで削除される行を要求したときに使用されます。

```csharp
// Wireup events
button.Activated += (sender, e) => {
  // Get button and product
  var btn = sender as NSButton;
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

イベントハンドラーの開始時に、特定のテーブル行にあるボタンと製品が取得されます。 次に、行の削除を確認するアラートがユーザーに表示されます。 ユーザーが行の削除を選択すると、指定された行がデータソースから削除され、テーブルが再読み込みされます。

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

最後に、テーブルビューのセルが新しく作成されるのではなく再利用されている場合は、次のコードによって、処理される列に基づいて構成されます。

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
    var btn = subview as NSButton;
    if (btn != null) {
      btn.Tag = row;
    }
  }
  break;
}

```

**[アクション]** 列では、`NSButton` が見つかるまですべてのサブビューがスキャンされ、`Tag` プロパティが更新されて現在の行をポイントします。

これらの変更が適用されると、アプリの実行時に、各行に **[削除]** ボタンが表示されます。

[![](table-view-images/delete02.png "The table view with deletion buttons")](table-view-images/delete02.png#lightbox)

ユーザーが **[削除]** ボタンをクリックすると、特定の行を削除するように求める警告が表示されます。

[![](table-view-images/delete03.png "A delete row alert")](table-view-images/delete03.png#lightbox)

ユーザーが [削除] を選択すると、行が削除され、テーブルが再描画されます。

[![](table-view-images/delete04.png "The table after the row is deleted")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>データバインディングテーブルビュー

UI 要素を設定して操作するために、Xamarin. Mac アプリケーションでキー値のコードとデータバインディングの手法を使用することにより、記述して維持する必要があるコードの量を大幅に減らすことができます。 また、フロントエンドのユーザーインターフェイス (_モデルビューコントローラー_) からバッキングデータ (_データモデル_) をさらに分離することもできます。これにより、管理が容易になり、アプリケーションの設計をより柔軟に行うことができます。

キー値のコーディング (KVC) は、キー (特別に書式設定された文字列) を使用して、インスタンス変数またはアクセサーメソッド (`get/set`) を介してアクセスするのではなく、プロパティを識別するために、オブジェクトのプロパティに間接的にアクセスするメカニズムです。 Xamarin. Mac アプリケーションでキー値のコーディングに準拠したアクセサーを実装することによって、キー値の観察 (KVO)、データバインディング、コアデータ、Cocoa バインド、および scriptability などの他の macOS 機能にアクセスできます。

詳細については、[データバインディングとキー値のコーディング](~/mac/app-fundamentals/databinding.md)に関するドキュメントの[テーブルビューのデータバインディング](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding)に関するセクションを参照してください。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用方法について詳しく説明しました。 テーブルビューのさまざまな種類と用途、Xcode の Interface Builder でテーブルビューを作成および管理する方法、コードでC#テーブルビューを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacTables (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactables)
- [MacImages (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [ソース リスト](~/mac/user-interface/source-list.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
