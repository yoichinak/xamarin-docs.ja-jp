---
title: Xamarin. Mac のテーブルビュー
description: この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用について説明します。 Xcode と Interface Builder でのテーブルビューの作成と、コードでのテーブルビューの操作について説明します。
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 7b341ee5ee72c3a89ab14161862896585ed498fc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772687"
---
# <a name="table-views-in-xamarinmac"></a>Xamarin. Mac のテーブルビュー

_この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用について説明します。Xcode と Interface Builder でのテーブルビューの作成と、コードでのテーブルビューの操作について説明します。_

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合、 *Xcode および*で作業している開発者が行うの*と同じ*テーブルビューにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してテーブルビューを作成および管理できます (また、必要C#に応じて、コード内で直接作成することもできます)。

テーブルビューでは、複数行の情報の1つ以上の列を含む表形式のデータが表示されます。 ユーザーは、作成されるテーブルビューの種類に基づいて、列の並べ替え、列の再編成、列の追加、列の削除、テーブル内のデータの編集を行うことができます。

[![](table-view-images/intro01.png "テーブルの例")](table-view-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用に関する基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

[Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの「[クラス/ C#メソッドを目的に公開](~/mac/internals/how-it-works.md)する」セクションを参照して、 C#クラスをに接続するために使用`Register`さ`Export`れるコマンドとコマンドについて説明します。目的 C オブジェクトと UI 要素。

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>テーブルビューの概要

テーブルビューでは、複数行の情報の1つ以上の列を含む表形式のデータが表示されます。 テーブルビューは、スクロールビュー内に表示`NSScrollView`されます ()。 macOS 10.7 以降では`NSView` 、セル (`NSCell`) の代わりに任意のを使用して、行と列の両方を表示できます。 ただし、それでも使用`NSCell`できますが、通常は、カスタムの行と列をサブクラス`NSTableCellView`化して作成します。

テーブルビューには独自のデータは格納されません。代わりに、必要に応じ`NSTableViewDataSource`て、データソース () を使用して必要な行と列の両方を指定します。

テーブルビューの動作をカスタマイズするには、テーブルビューのデリゲート (`NSTableViewDelegate`) のサブクラスを使用して、テーブルの列の管理、型の選択、機能の選択、行の選択と編集、カスタムの追跡、および個々の列に対するカスタムビューをサポートします。レコード.

Apple では、テーブルビューを作成するときに次のことが提案されます。

- 列ヘッダーをクリックして、ユーザーがテーブルを並べ替えることができるようにします。
- 列に表示されるデータを記述する名詞または短い名詞の語句である列ヘッダーを作成します。

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[コンテンツビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) 」セクションを参照してください。

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Xcode でのテーブルビューの作成と管理

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは、プロジェクトに`.storyboard`自動的に含まれるファイルに定義されています。 Windows のデザインを編集するには、**ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックします。

[![](table-view-images/edit01.png "メインストーリーボードの選択")](table-view-images/edit01.png#lightbox)

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![](table-view-images/edit02.png "Xcode で UI を編集する")](table-view-images/edit02.png#lightbox)

`table` **ライブラリインスペクターの**検索ボックスに「」と入力して、テーブルビューコントロールを簡単に見つけられるようにします。

[![](table-view-images/edit03.png "ライブラリからテーブルビューを選択する")](table-view-images/edit03.png#lightbox)

**インターフェイスエディター**で、ビューコントローラーにテーブルビューをドラッグし、ビューコントローラーのコンテンツ領域を塗りつぶすように設定します。次に、**制約エディター**でウィンドウのサイズを縮小し、拡大する場所に設定します。

[![](table-view-images/edit04.png "制約の編集")](table-view-images/edit04.png#lightbox)

**インターフェイス階層**でテーブルビューを選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit05.png "属性インスペクター")](table-view-images/edit05.png#lightbox)

- **コンテンツモード**-ビュー (`NSView`) またはセル (`NSCell`) のいずれかを使用して、行と列のデータを表示できます。 MacOS 10.7 以降では、ビューを使用する必要があります。
- [**フローティンググループ行]** -if `true`テーブルビューでは、グループ化されたセルが浮動小数点型として描画されます。
- **Columns** -表示される列の数を定義します。
- **Headers** -If `true`列にはヘッダーが含まれます。
- **順序**を変更`true`する-の場合、ユーザーはテーブル内の列の順序を変更することができます。
- **[サイズ変更]** -の場合`true`、ユーザーは列ヘッダーをドラッグして列のサイズを変更できます。
- **列**のサイズ変更-テーブルの列のサイズを自動調整する方法を制御します。
- **強調表示**-セルを選択したときにテーブルで使用する強調表示の種類を制御します。
- **代替行**-If `true`、その他の行の背景色は異なります。
- **水平グリッド**-横方向のセルの間に描画する境界線の種類を選択します。
- **垂直グリッド**-セル間に描画する境界線の種類を垂直方向に選択します。
- **グリッドの色**-セルの境界線の色を設定します。
- **Background** -セルの背景色を設定します。
- **選択**-ユーザーがテーブル内のセルを選択する方法を、次のように制御できます。
  - **複数**の場合`true`、ユーザーは複数の行と列を選択できます。
  - **列**-の`true`場合、ユーザーは列を選択できます。
  - **「Select** -If `true`」と入力すると、ユーザーは文字を入力して行を選択できます。
  - **Empty** -ユーザー `true`が行または列を選択する必要がない場合、テーブルでは何も選択できません。
- [**自動**保存]-テーブル形式が自動的に保存される名前。
- **列情報**-の`true`場合、列の順序と幅が自動的に保存されます。
- [**改行]-セル**が改行を処理する方法を選択します。
- **最後に表示**される行`true`を切り捨てます。の場合、セルはデータ内で切り捨てられ、その範囲内に収まりません。

> [!IMPORTANT]
> 従来の Xamarin. Mac アプリケーションを維持してい`NSView`ない場合は、ベースのテーブル`NSCell`ビューでベースのテーブルビューを使用する必要があります。 `NSCell`は従来と見なされ、今後サポートされない可能性があります。

**インターフェイス階層**内のテーブル列を選択します。**属性インスペクター**では、次のプロパティを使用できます。

[![](table-view-images/edit06.png "属性インスペクター")](table-view-images/edit06.png#lightbox)

- **タイトル**-列のタイトルを設定します。
- **Alignment** -セル内のテキストの配置を設定します。
- **[タイトルのフォント]** -セルのヘッダーテキストのフォントを選択します。
- **Sort key** -列のデータを並べ替えるために使用されるキーです。 ユーザーがこの列を並べ替えられない場合は、空白のままにします。
- **Selector** -並べ替えを実行するために使用される**アクション**です。 ユーザーがこの列を並べ替えられない場合は、空白のままにします。
- **Order** -列データの並べ替え順序です。
- **サイズ変更**中-列のサイズ変更の種類を選択します。
- **編集可能**- `true`の場合、ユーザーはセルベーステーブル内のセルを編集できます。
- **Hidden** -の`true`場合、列は非表示になります。

また、列のハンドル (垂直方向に列の右側に中央揃え) をドラッグして列のサイズを変更することもできます。

テーブルビューの各列を選択し、最初の列に**タイトル** `Product` `Details`と2番目の列を指定してみましょう。

**インターフェイス階層**でテーブルセルビュー`NSTableViewCell`() を選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit07.png "属性インスペクター")](table-view-images/edit07.png#lightbox)

これらは、標準ビューのすべてのプロパティです。 また、この列の行のサイズを変更することもできます。

`NSTextField`**インターフェイス階層**でテーブルビューセル (既定では) を選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit08.png "属性インスペクター")](table-view-images/edit08.png#lightbox)

ここでは、標準のテキストフィールドのすべてのプロパティを設定します。 既定では、標準のテキストフィールドを使用して、列のセルのデータが表示されます。

**インターフェイス階層**でテーブルセルビュー`NSTableFieldCell`() を選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![](table-view-images/edit09.png "属性インスペクター")](table-view-images/edit09.png#lightbox)

ここで最も重要な設定は次のとおりです。

- **[レイアウト]** -この列のセルのレイアウトを選択します。
- **単一行モードを使用**し`true`ます。の場合、セルは1行に制限されます。
- **最初の実行時レイアウト**の`true`幅-If の場合、アプリケーションを初めて実行したときに表示されるときに、セルの幅 (手動または自動) が優先されます。
- **Action** -セルに対して編集**アクション**が送信されるタイミングを制御します。
- **Behavior** -セルを選択可能にするか、編集可能にするかを定義します。
- **リッチテキスト**-If `true`セルには、書式設定されたテキストを表示できます。
- `true`[元に**戻す**]: このセルは、元に戻す動作の役割を担います。

**インターフェイス階層**のテーブル列の`NSTableFieldCell`下部にあるテーブルセルビュー () を選択します。

[![](table-view-images/edit10.png "テーブルセルビューの選択")](table-view-images/edit10.png#lightbox)

これにより、特定の列に対して作成されたすべてのセルのベース_パターン_として使用されるテーブルセルビューを編集できます。

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>アクションとアウトレットの追加

他の Cocoa UI コントロールと同様に、テーブルビューとその列とセルは、**アクション**と**アウトレット**( C#必要な機能に基づく) を使用してコードに公開する必要があります。

このプロセスは、公開するすべてのテーブルビュー要素で同じです。

1. **アシスタントエディター**に切り替えて、 `ViewController.h`ファイルが選択されていることを確認します。 

    [![](table-view-images/edit11.png "アシスタントエディター")](table-view-images/edit11.png#lightbox)
2. **インターフェイス階層**からテーブルビューを選択し、コントロールをクリックして、 `ViewController.h`ファイルにドラッグします。
3. テーブルビュー `ProductTable`のアウトレットを作成します。 

    [![](table-view-images/edit13.png "アウトレットの構成")](table-view-images/edit13.png#lightbox)
4. テーブルの列に対しても、および`ProductColumn`と`DetailsColumn`呼ばれるコンセントを作成します。 

    [![](table-view-images/edit14.png "アウトレットの構成")](table-view-images/edit14.png#lightbox)
5. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、アプリケーションの実行時にテーブルのデータを表示するコードを記述します。

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>テーブルビューの設定

Interface Builder で設計され、**アウトレット**を介して公開されるテーブルビューではC# 、次にコードを作成して設定する必要があります。

まず、個々の行の情報`Product`を保持する新しいクラスを作成してみましょう。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **...** ] を選択します。[**汎用** > **空のクラス**] `Product`を選択し、**名前**として「」と入力して、 **[新規]** ボタンをクリックします。

[![](table-view-images/populate01.png "空のクラスの作成")](table-view-images/populate01.png#lightbox)

ファイルの`Product.cs`外観を次のようにします。

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

次に、要求に応じてテーブルの`NSTableDataSource`データを提供するために、のサブクラスを作成する必要があります。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **...** ] を選択します。[**汎用** > **空のクラス**] `ProductTableDataSource`を選択し、**名前**として「」と入力して、 **[新規]** ボタンをクリックします。

`ProductTableDataSource.cs`ファイルを編集し、次のように表示します。

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

このクラスは、 `GetRowCount`テーブルビューの項目のストレージを保持し、をオーバーライドしてテーブル内の行の数を返します。

最後に、テーブルの動作を提供する`NSTableDelegate`ために、のサブクラスを作成する必要があります。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **...** ] を選択します。[**汎用** > **空のクラス**] `ProductTableDelegate`を選択し、**名前**として「」と入力して、 **[新規]** ボタンをクリックします。

`ProductTableDelegate.cs`ファイルを編集し、次のように表示します。

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

の`ProductTableDelegate`インスタンスを作成するときに、テーブルのデータを提供`ProductTableDataSource`するのインスタンスも渡します。 `GetViewForItem`メソッドは、ビュー (データ) を取得して、列と列を指定するセルを表示します。 可能であれば、新しいビューを作成する必要がない場合は、既存のビューを再利用してセルを表示します。

テーブルを作成するには、 `ViewController.cs`ファイルを編集して、メソッドを`AwakeFromNib`次のようにします。

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

[![](table-view-images/populate02.png "サンプルアプリの実行")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>列で並べ替え

ユーザーが列ヘッダーをクリックしてテーブル内のデータを並べ替えることができるようにします。 まず、 `Main.storyboard`ファイルをダブルクリックして、Interface Builder で編集するために開きます。 `compare:` `Title` `Ascending`列を選択し、並べ替えキーとして「」と入力します。 `Product`

[![](table-view-images/sort01.png "並べ替えキーの設定")](table-view-images/sort01.png#lightbox)

`compare:` `Description` `Ascending`列を選択し、並べ替えキーとして「」と入力します。 `Details`

[![](table-view-images/sort02.png "並べ替えキーの設定")](table-view-images/sort02.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、 `ProductTableDataSource.cs`ファイルを編集し、次のメソッドを追加します。

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

メソッドを使用すると、指定`Product`したクラスフィールドに基づいてデータソース内のデータを昇順または降順で並べ替えることができます。 `Sort` オーバーライド`SortDescriptorsChanged`されたメソッドは、が列見出しをクリックするたびに呼び出されます。 Interface Builder に設定した**キー**値とその列の並べ替え順序が渡されます。

アプリケーションを実行して列ヘッダーをクリックすると、行はその列で並べ替えられます。

[![](table-view-images/sort03.png "アプリの実行例")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>行の選択

ユーザーが単一行を選択できるようにするには、 `Main.storyboard`ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[複数]** チェックボックスをオフにします。

[![](table-view-images/select01.png "属性インスペクター")](table-view-images/select01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、 `ProductTableDelegate.cs`ファイルを編集し、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

これにより、ユーザーはテーブルビューの任意の1行を選択できるようになります。 ユーザー `false`が行`ShouldSelectRow`を選択できないようにする場合は、ユーザーが選択`false`できない行については、すべての行に対してを返します。

テーブルビュー (`NSTableView`) には、行の選択を操作するための次のメソッドが含まれています。

- `DeselectRow(nint)`-テーブル内の指定された行を選択解除します。
- `SelectRow(nint,bool)`-指定された行を選択します。 2 `false`番目のパラメーターにを渡して、一度に1つの行だけを選択します。
- `SelectedRow`-テーブルで選択されている現在の行を返します。
- `IsRowSelected(nint)`-指定`true`された行が選択されている場合はを返します。

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>複数行の選択

ユーザーが複数の行を選択できるようにするには、 `Main.storyboard`ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[複数]** チェックボックスをオンにします。

[![](table-view-images/select02.png "属性インスペクター")](table-view-images/select02.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、 `ProductTableDelegate.cs`ファイルを編集し、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

これにより、ユーザーはテーブルビューの任意の1行を選択できるようになります。 ユーザー `false`が行`ShouldSelectRow`を選択できないようにする場合は、ユーザーが選択`false`できない行については、すべての行に対してを返します。

テーブルビュー (`NSTableView`) には、行の選択を操作するための次のメソッドが含まれています。

- `DeselectAll(NSObject)`-テーブル内のすべての行を選択解除します。 最初`this`のパラメーターとしてを使用して、選択したオブジェクト内でを送信します。 
- `DeselectRow(nint)`-テーブル内の指定された行を選択解除します。
- `SelectAll(NSobject)`-テーブル内のすべての行を選択します。 最初`this`のパラメーターとしてを使用して、選択したオブジェクト内でを送信します。
- `SelectRow(nint,bool)`-指定された行を選択します。 2 `false`番目のパラメーターにを渡して選択範囲をクリアし、1 `true`行だけを選択します。を渡して選択範囲を拡張し、この行を含めます。
- `SelectRows(NSIndexSet,bool)`-指定された行のセットを選択します。 2 `false`番目のパラメーターにを渡して選択範囲をクリアし、これら`true`の行のみを選択し、を渡して選択範囲を拡張し、これらの行を含めます。
- `SelectedRow`-テーブルで選択されている現在の行を返します。
- `SelectedRows`-選択さ`NSIndexSet`れた行のインデックスを含むを返します。
- `SelectedRowCount`-選択された行の数を返します。
- `IsRowSelected(nint)`-指定`true`された行が選択されている場合はを返します。

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>入力して行を選択します

ユーザーがテーブルビューを選択した状態で文字を入力し、その文字を含む最初の行を選択できるようにするには`Main.storyboard` 、ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[型の選択**] チェックボックスをオンにします。

[![](table-view-images/type01.png "選択の種類の設定")](table-view-images/type01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、 `ProductTableDelegate.cs`ファイルを編集し、次のメソッドを追加します。

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

メソッド`GetNextTypeSelectMatch`は、指定さ`searchString`れたを受け取り、その文字列`Product` `Title`を含む最初のの行を返します。

アプリケーションを実行して文字を入力すると、行が選択されます。

[![](table-view-images/type02.png "サンプルアプリの実行")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>列の並べ替え

テーブルビューでユーザーが列の順序を変更できるようにするには、 `Main.storyboard`ファイルをダブルクリックして、Interface Builder で編集するために開きます。 **インターフェイス階層**でテーブルビューを選択し、**属性インスペクター**の **[並べ替え]** チェックボックスをオンにします。

[![](table-view-images/reorder01.png "属性インスペクター")](table-view-images/reorder01.png#lightbox)

"**自動保存**" プロパティに値を指定して **[列情報]** フィールドを確認すると、テーブルのレイアウトに対して行った変更は自動的に保存され、次にアプリケーションが実行されたときに復元されます。

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、 `ProductTableDelegate.cs`ファイルを編集し、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
  return true;
}
```

メソッド`ShouldReorder`は、 `false` `true` ドラッグを許可する任意の列に対してを返し、それ以外の場合は`newColumnIndex`を返します。

アプリケーションを実行すると、列のヘッダーをドラッグして列の順序を変更できます。

[![](table-view-images/reorder02.png "並べ替えられた列の例")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>セルの編集

ユーザーが特定のセルの値を編集できるようにするには、 `ProductTableDelegate.cs`ファイルを編集し、次の`GetViewForItem`ようにメソッドを変更します。

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

[![](table-view-images/editing01.png "セルを編集する例")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>テーブルビューでの画像の使用

画像を`NSTableView`内のセルの一部として含めるには、テーブルビューの`GetViewForItem` `NSTableViewDelegate's`メソッドによるデータの取得方法を変更して、通常`NSTextField`の`NSTableCellView`の代わりにを使用する必要があります。 例えば:

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

まず、Xcode の`Main.storyboard` Interface Builder のを編集し、テーブルビューを選択して、列の数を3に増やします。 次に、新しい列の**タイトル**を次の`Action`ように変更します。

[![](table-view-images/delete01.png "列名の編集")](table-view-images/delete01.png#lightbox)

変更をストーリーボードに保存し Visual Studio for Mac に戻り、変更を同期します。

次に、 `ViewController.cs`ファイルを編集し、次のパブリックメソッドを追加します。

```csharp
public void ReloadTable ()
{
  ProductTable.ReloadData ();
}
```

同じファイルで、メソッド内の`ViewDidLoad`新しいテーブルビューデリゲートの作成を次のように変更します。

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

次に、 `ProductTableDelegate.cs`ファイルを編集してビューコントローラーへのプライベート接続を含め、デリゲートの新しいインスタンスを作成するときに、コントローラーをパラメーターとして取得します。

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

これは、 `GetViewForItem`メソッドで以前に実行されていたすべてのテキストビューの構成を受け取り、単一の呼び出し可能な場所に配置します (テーブルの最後の列にはテキストビューが含まれていますが、ボタンは含まれていないため)。

最後に、 `GetViewForItem`メソッドを編集し、次のように表示します。

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

このコードのいくつかのセクションをさらに詳しく見てみましょう。 まず、列の名前`NSTableViewCell`に基づいて新しいアクションが作成されます。 最初の2つの列 (**Product**および**Details**) では`ConfigureTextField` 、新しいメソッドが呼び出されます。

**[アクション]** 列には、 `NSButton`新しいが作成され、そのセルにサブビューとして追加されます。

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

ボタンの`Tag`プロパティは、現在処理中の行の番号を保持するために使用されます。 この数値は、後でユーザーがボタンの`Activated`イベントで削除される行を要求したときに使用されます。

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

**[アクション]** 列では、が見つかる`NSButton` `Tag`まですべてのサブビューがスキャンされ、そのプロパティが現在の行をポイントするように更新されます。

これらの変更が適用されると、アプリの実行時に、各行に **[削除]** ボタンが表示されます。

[![](table-view-images/delete02.png "[削除] ボタンが表示されたテーブルビュー")](table-view-images/delete02.png#lightbox)

ユーザーが **[削除]** ボタンをクリックすると、特定の行を削除するように求める警告が表示されます。

[![](table-view-images/delete03.png "行の削除の警告")](table-view-images/delete03.png#lightbox)

ユーザーが [削除] を選択すると、行が削除され、テーブルが再描画されます。

[![](table-view-images/delete04.png "行が削除された後のテーブル")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>データバインディングテーブルビュー

UI 要素を設定して操作するために、Xamarin. Mac アプリケーションでキー値のコードとデータバインディングの手法を使用することにより、記述して維持する必要があるコードの量を大幅に減らすことができます。 また、フロントエンドのユーザーインターフェイス (_モデルビューコントローラー_) からバッキングデータ (_データモデル_) をさらに分離することもできます。これにより、管理が容易になり、アプリケーションの設計をより柔軟に行うことができます。

キー値のコーディング (KVC) は、オブジェクトのプロパティに間接的にアクセスするためのメカニズムです。キー (特殊な書式設定文字列) を使用して、インスタンス変数また`get/set`はアクセサーメソッド () を使用してアクセスするのではなく、プロパティを識別します。 Xamarin. Mac アプリケーションでキー値のコーディングに準拠したアクセサーを実装することによって、キー値の観察 (KVO)、データバインディング、コアデータ、Cocoa バインド、および scriptability などの他の macOS 機能にアクセスできます。

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
