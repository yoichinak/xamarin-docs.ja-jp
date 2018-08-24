---
title: Xamarin.Mac のテーブル ビュー
description: この記事では、Xamarin.Mac アプリケーションでのテーブル ビューの使用について説明します。 これには、Xcode と Interface Builder とコードで操作するテーブル ビューの作成について説明します。
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 68b52fb4b7a3a65b45fcbecdc865bc64d9865fd9
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251111"
---
# <a name="table-views-in-xamarinmac"></a>Xamarin.Mac のテーブル ビュー

_この記事では、Xamarin.Mac アプリケーションでのテーブル ビューの使用について説明します。これには、Xcode と Interface Builder とコードで操作するテーブル ビューの作成について説明します。_

同じへのアクセス、Xamarin.Mac アプリケーションで c# と .NET を使用する場合があるテーブルをビューで作業する開発者*Objective C*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成およびテーブル ビューを維持 (または必要に応じて c# コードで直接作成する)。

テーブル ビューでは、複数の行の情報の 1 つまたは複数の列を含む表形式でデータを表示します。 作成されるテーブル ビューの種類に基づいて、ユーザーの列で並べ替える、列の再編成、追加列、列を削除または編集できますテーブル内に含まれるデータ。

[![](table-view-images/intro01.png "テーブルな例")](table-view-images/intro01.png#lightbox)

この記事で、Xamarin.Mac アプリケーションでテーブルのビューの基本について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>テーブル ビューの概要

テーブル ビューでは、複数の行の情報の 1 つまたは複数の列を含む表形式でデータを表示します。 スクロール ビューの内部テーブルのビューが表示されます (`NSScrollView`) および macOS 10.7 以降では、使用できます`NSView`セルではなく (`NSCell`) を行と列の両方を表示します。 ただし、使用することもできます`NSCell`ただしが通常サブクラス`NSTableCellView`し、カスタムの行と列を作成します。

テーブル ビューでは、独自のデータを保存しませんに代わりに、データ ソースには、(`NSTableViewDataSource`) を行と、必要に応じてごとに、必要な列の両方を提供します。

テーブル ビューのデリゲートのサブクラスを提供することで、テーブル ビューの動作をカスタマイズすることができます (`NSTableViewDelegate`) テーブルの列の管理をサポートする機能、行の選択、編集、カスタムの追跡、および個々 の列のカスタム ビューを選択する入力し、行。

テーブルのビューを作成するときに Apple は、次は示しています。

* 列ヘッダーをクリックしてテーブルの並べ替えにユーザーを許可します。
* 名詞または短い語句をその列に表示されているデータを記述する列ヘッダーを作成します。

詳細についてを参照してください、[コンテンツ ビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>作成および保守の Xcode でのテーブル ビュー

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリック、`Main.storyboard`ファイル。

[![](table-view-images/edit01.png "メインのストーリー ボードを選択します。")](table-view-images/edit01.png#lightbox)

これが、Xcode の Interface Builder でウィンドウのデザインを開きます。

[![](table-view-images/edit02.png "Xcode では、UI の編集")](table-view-images/edit02.png#lightbox)

型`table`に、**ライブラリ インスペクターの**テーブル ビュー コントロールを検索するが容易に検索ボックス。

[![](table-view-images/edit03.png "ライブラリからのテーブル ビューの選択")](table-view-images/edit03.png#lightbox)

ビュー コント ローラー上にテーブル ビューをドラッグして、**インターフェイス エディター**、ビュー コント ローラーのコンテンツ領域を入力し、縮小して、ウィンドウにも拡張に設定して、**制約エディター**:

[![](table-view-images/edit04.png "制約の編集")](table-view-images/edit04.png#lightbox)

テーブル ビューを選択して、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](table-view-images/edit05.png "属性インスペクター")](table-view-images/edit05.png#lightbox)

- **コンテンツのモード**-いずれかのビューを使用することができます (`NSView`) またはセル (`NSCell`) 行と列にデータを表示します。 MacOS 10.7 以降では、ビューを使用する必要があります。
- **行のグループ化をフローティング**- 場合`true`は、フローティングかのようにテーブル ビューがグループ化されたセルを描画します。
- **列**に表示される列の数を定義します。
- **ヘッダー**場合 - `true`、列ヘッダーになります。
- **並べ替え**場合 -`true`ユーザーがドラッグできる、テーブル内の列の順序を変更します。
- **サイズ変更**場合 -`true`ユーザーが列のサイズを変更する列ヘッダーをドラッグできます。
- **列のサイズ変更**-テーブルのサイズの列が自動方法を制御します。
- **強調表示**-テーブルを強調表示の型のセルが選択されているときに使用するコントロール。
- **代替行の**場合 -`true`これまで、その他の行が別の背景色になります。
- **水平グリッド**-セル間で水平方向に描画される境界線の種類を選択します。
- **垂直グリッド**-セル間を垂直方向に描画される境界線の種類を選択します。
- **グリッドの色**のセルの境界線の色を設定します。
- **バック グラウンド**のセルの背景色を設定します。
- **選択**-としてテーブルで、ユーザーがセルを選択する方法を制御できます。
    - **複数**場合 -`true`ユーザーは、複数の行と列を選択できます。
    - **列**場合 -`true`ユーザーは、列を選択できます。
    - **入力選択**場合 -`true`ユーザーが行を選択する、文字を入力できます。
    - **空**場合 -`true`ユーザーは、行または列を選択する必要はありません、テーブルでは、選択されていないすべての。
- **自動保存**-テーブル形式が自動的に名前を保存します。
- **列情報**場合 -`true`順序と列の幅の内容は自動的に保存します。
- **改行**- セル改行の処理方法を選択します。
- **表示されている最後の行を切り捨てます**場合 -`true`セルの境界内に収まらないデータで切り捨てられます。

> [!IMPORTANT]
> 従来の Xamarin.Mac アプリケーションを保持している場合を除き、`NSView`経由でベース テーブルのビューを使用する必要があります`NSCell`ベースのテーブルのビュー。 `NSCell` 従来と見なされ、今後はサポートされません。

テーブル列の選択、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](table-view-images/edit06.png "属性インスペクター")](table-view-images/edit06.png#lightbox)

- **タイトル**-列のタイトルを設定します。
- **配置**-セル内のテキストの配置を設定します。
- **タイトルのフォント**のセルのヘッダー テキストのフォントを選択します。
- **並べ替えキー** -列のデータの並べ替えに使用するキーします。 ユーザーがこの列を並べ替える場合は空白のままにします。
- **セレクター** -は、**アクション**の並べ替えを実行するために使用します。 ユーザーがこの列を並べ替える場合は空白のままにします。
- **順序**-は、列のデータの並べ替え順序。
- **サイズ変更**-列のサイズ変更の種類を選択します。
- **編集可能な**場合 -`true`ユーザーがセルのベース テーブル内のセルを編集できます。
- **非表示に**場合 -`true`列が非表示になります。

ハンドル (列の右側に垂直方向に中央揃え) の左または右にドラッグして、列サイズを変更することもできます。

テーブル、ビューの各列を選択して最初の列に付ける、**タイトル**の`Product`、もう 1 つ`Details`します。

テーブル セルのビューを選択します (`NSTableViewCell`) で、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](table-view-images/edit07.png "属性インスペクター")](table-view-images/edit07.png#lightbox)

これらは、すべての標準的なビューのプロパティです。 また、ここで、この列の行のサイズ変更オプションがあります。

Table View Cell を選択します (既定では、これは、 `NSTextField`) で、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](table-view-images/edit08.png "属性インスペクター")](table-view-images/edit08.png#lightbox)

ここで設定する標準のテキスト フィールドのすべてのプロパティがあります。 既定では、標準のテキスト フィールドは、列に、セルのデータを表示する使用します。

テーブル セルのビューを選択します (`NSTableFieldCell`) で、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](table-view-images/edit09.png "属性インスペクター")](table-view-images/edit09.png#lightbox)

ここでの最も重要な設定は次のとおりです。

- **レイアウト**- この列のセルのレイアウト方法を選択します。
- **単一行モードを使用して**場合 -`true`セルが 1 行に制限されています。
- **最初の実行時レイアウト**場合 -`true`セルの幅の設定を (手動または自動で) 場合は優先初めて、アプリケーションの実行が表示されます。
- **アクション**のタイミングを制御、編集**アクション**セルが送信されます。
- **動作**のセルが選択可能なまたは編集可能な場合を定義します。
- **リッチ テキスト**場合 -`true`セルは、書式設定とスタイル設定されたテキストを表示できます。
- **元に戻す**場合 - `true`、に対しては、セルが責任を負います動作を元に戻します。

テーブル セルのビューを選択します (`NSTableFieldCell`) のテーブルの列の下部にある、**インターフェイス階層**:

[![](table-view-images/edit10.png "テーブル セルのビューを選択します。")](table-view-images/edit10.png#lightbox)

ベースとして使用するテーブル セル ビューを編集できます_パターン_のすべてのセルを指定した列を作成します。

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>アクションとアウトレットを追加します。

だけ Cocoa UI コントロールと同様に、テーブル ビューを公開する必要があり、その列は、c# コードを使用するセル**アクション**と**Outlet** (必要な機能に基づく)。

プロセスは、任意のテーブル ビュー要素を公開することで同じです。

1. 切り替えて、**アシスタント エディター**いることを確認し、`ViewController.h`ファイルが選択されています。 

    [![](table-view-images/edit11.png "アシスタント エディター")](table-view-images/edit11.png#lightbox)
2. テーブル ビューの選択、**インターフェイス階層**、コントロールのクリックおよびドラッグ、`ViewController.h`ファイル。
3. 作成、**アウトレット**テーブル ビューと呼ばれる`ProductTable`: 

    [![](table-view-images/edit13.png "アウトレットを構成します。")](table-view-images/edit13.png#lightbox)
4. 作成**Outlet**と呼ばれるテーブルの列も`ProductColumn`と`DetailsColumn`: 

    [![](table-view-images/edit14.png "アウトレットを構成します。")](table-view-images/edit14.png#lightbox)
5. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

次に、データを書き込むコードの表示によってテーブルのアプリケーションの実行時にします。

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>テーブル ビューの作成

テーブル、ビューでは、Interface Builder で設計され、を介して公開される、**アウトレット**、次に読み込むための c# コードを作成する必要があります。

最初に、新しいを作成しましょう`Product`個々 の行の情報を保持するクラス。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.** 選択**一般的な** > **空のクラス**、入力`Product`の**名前** をクリックし、**新規**ボタン。

[![](table-view-images/populate01.png "空のクラスを作成します。")](table-view-images/populate01.png#lightbox)

ように、`Product.cs`ファイルの次のようになります。

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

次に、私たちのサブクラスを作成する必要があります`NSTableDataSource`を要求に応じて、テーブルのデータを提供します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.** 選択**全般** > **空のクラス**、入力`ProductTableDataSource`の**名前** をクリック、**新規**ボタン。

編集、`ProductTableDataSource.cs`ファイルを開き、次のようになります。

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

このクラスが、テーブル ビューの項目用のストレージと上書き、`GetRowCount`をテーブルの行の数を返します。

最後に、私たちのサブクラスを作成する必要があります`NSTableDelegate`テーブルの動作を提供します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.** 選択**全般** > **空のクラス**、入力`ProductTableDelegate`の**名前** をクリック、**新規**ボタン。

編集、`ProductTableDelegate.cs`ファイルを開き、次のようになります。

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

インスタンスを作成するとき、 `ProductTableDelegate`、私たちのインスタンスに渡すことも、`ProductTableDataSource`テーブルのデータを提供します。 `GetViewForItem`メソッドが指定した列と行のセルを表示するビュー (データ) が返されます。 可能であれば、既存のビューは、セルを表示する再利用するでない場合、新しいビューを作成する必要があります。

テーブルの作成、編集、`ViewController.cs`ファイル、`AwakeFromNib`メソッドの次のようになります。

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

アプリケーションを実行した場合、次のように表示されます。

[![](table-view-images/populate02.png "サンプル アプリの実行")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>列で並べ替え

列ヘッダーをクリックして、テーブル内のデータの並べ替えにユーザーを許可します。 最初をダブルクリック、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 選択、`Product`列、入力`Title`の**並べ替えキー**、`compare:`の**セレクター**選択と`Ascending`の**順序**:

[![](table-view-images/sort01.png "並べ替えキーの設定")](table-view-images/sort01.png#lightbox)

選択、`Details`列、入力`Description`の**並べ替えキー**、`compare:`の**セレクター**選択と`Ascending`の**順序**:

[![](table-view-images/sort02.png "並べ替えキーの設定")](table-view-images/sort02.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ編集しましょう、`ProductTableDataSource.cs`ファイルを開き、次のメソッドを追加します。

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

`Sort`メソッドでは、データに基づくデータ ソースの並べ替えにより、指定された`Product`昇順または降順のどちらかのクラス フィールド。 オーバーライドされた`SortDescriptorsChanged`使用列見出しをクリックするたびにメソッドが呼び出されます。 渡される、**キー** Interface Builder とその列の並べ替え順序で設定される値。

アプリケーションを実行し、列ヘッダーをクリックした場合、行がその列で並べ替えられます。

[![](table-view-images/sort03.png "アプリの実行例")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>行の選択

ユーザーが 1 つの行を選択し、ダブルクリックできるようにするかどうか、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 テーブル ビューを選択して、**インターフェイス階層**をオフにし、**複数**でチェック ボックスをオン、**属性インスペクター**:

[![](table-view-images/select01.png "属性インスペクター")](table-view-images/select01.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。


次に、編集、`ProductTableDelegate.cs`ファイルを開き、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

これによって、ユーザーのテーブル ビューの 1 行を選択します。 返す`false`の`ShouldSelectRow`のいずれかの行を選択できるユーザーをしたくないまたは`false`任意の行を選択できるユーザーをしたくない場合、すべての行にします。

テーブル ビュー (`NSTableView`) 行の選択を操作するための次のメソッドが含まれています。

- `DeselectRow(nint)` -テーブルの特定の行を選択解除します。
- `SelectRow(nint,bool)` -特定の行を選択します。 渡す`false`2 番目のパラメーターを一度に 1 行のみを選択します。
- `SelectedRow` -テーブルで選択されている現在の行を返します。
- `IsRowSelected(nint)` -返します`true`特定の行が選択されている場合。

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>複数行の選択

ユーザーが複数の行を選択し、ダブルクリックできるようにするかどうか、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 テーブル ビューを選択して、**インターフェイス階層**を確認し、**複数**でチェック ボックスをオン、**属性インスペクター**:

[![](table-view-images/select02.png "属性インスペクター")](table-view-images/select02.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。


次に、編集、`ProductTableDelegate.cs`ファイルを開き、次のメソッドを追加します。

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

これによって、ユーザーのテーブル ビューの 1 行を選択します。 返す`false`の`ShouldSelectRow`のいずれかの行を選択できるユーザーをしたくないまたは`false`任意の行を選択できるユーザーをしたくない場合、すべての行にします。

テーブル ビュー (`NSTableView`) 行の選択を操作するための次のメソッドが含まれています。

- `DeselectAll(NSObject)` -テーブル内のすべての行を選択解除します。 使用`this`でこれを選択するオブジェクトを送信する最初のパラメーター。 
- `DeselectRow(nint)` -テーブルの特定の行を選択解除します。
- `SelectAll(NSobject)` -テーブルのすべての行を選択します。 使用`this`でこれを選択するオブジェクトを送信する最初のパラメーター。
- `SelectRow(nint,bool)` -特定の行を選択します。 渡す`false`2 番目のパラメーターの選択を解除し、1 行だけを選択、渡す`true`選択範囲を拡張し、この行が含まれます。
- `SelectRows(NSIndexSet,bool)` -指定した行のセットを選択します。 渡す`false`2 番目のパラメーターをオフにし、のみこれらを選択します。 行を渡す`true`、選択範囲を拡張し、これらの行が含まれます。
- `SelectedRow` -テーブルで選択されている現在の行を返します。
- `SelectedRows` -を返します、`NSIndexSet`選択された行のインデックスを格納します。
- `SelectedRowCount` -選択した行の数を返します。
- `IsRowSelected(nint)` -返します`true`特定の行が選択されている場合。

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>行を選択する種類

その文字を持つユーザーが選択されているテーブル ビューで文字を入力できるようにして、最初の行を選択する場合、ダブルクリックして、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 テーブル ビューを選択して、**インターフェイス階層**を確認し、**の種類を選択**でチェック ボックスをオン、**属性インスペクター**:

[![](table-view-images/type01.png "選択の種類の設定")](table-view-images/type01.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ編集しましょう、`ProductTableDelegate.cs`ファイルを開き、次のメソッドを追加します。

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

`GetNextTypeSelectMatch`メソッドは、指定された`searchString`し、最初の行を返す`Product`での文字列を持つ`Title`します。

アプリケーションを実行し、文字を入力する場合、行が選択されます。

[![](table-view-images/type02.png "サンプル アプリの実行")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>列の並べ替え

ユーザーがドラッグできるようにする場合は、テーブル ビューで列を並べ替えるをダブルクリックします、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 テーブル ビューを選択して、**インターフェイス階層**を確認し、**並べ替え**でチェック ボックスをオン、**属性インスペクター**:

[![](table-view-images/reorder01.png "属性インスペクター")](table-view-images/reorder01.png#lightbox)

値をプレゼントした場合、 **Autosave**プロパティとチェック、**列情報**フィールドでは、テーブルのレイアウトに行った変更を加えた私たちにとっては自動的に保存して、次回アプリケーションを復元実行されます。

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ編集しましょう、`ProductTableDelegate.cs`ファイルを開き、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder`メソッドが返す必要があります`true`にそれを許可する任意の列のドラッグの順序変更、 `newColumnIndex`, それ以外の場合、return `false`;

アプリケーションを実行した場合、列の順序を変更する列の見出しをドラッグしたことができます。

[![](table-view-images/reorder02.png "並べ替えられた列の例")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>セルの編集

ユーザーが特定のセルの値を編集、編集を許可する場合、`ProductTableDelegate.cs`ファイルし、変更、`GetViewForItem`メソッドとして、次のとおりです。

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

今すぐアプリケーションを実行、ユーザーはテーブル ビュー内のセルを編集できます。

[![](table-view-images/editing01.png "セルの編集の例")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>テーブル ビューでイメージを使用します。

内のセルの一部としてイメージを含める、 `NSTableView`、によって、テーブル ビューのデータを返す方法を変更する必要があります`NSTableViewDelegate's``GetViewForItem`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

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

詳細についてを参照してください、[テーブル ビューの使用を使用してイメージ](~/mac/app-fundamentals/image.md)のセクション、[画像](~/mac/app-fundamentals/image.md)ドキュメント。

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>行を削除 ボタンを追加します。

アプリの要件に基づき、ありますテーブルの各行のアクション ボタンを指定する必要。 これの例は、上記で作成した対象テーブル ビューの例の拡大しましょう、**削除**行ごとにボタンをクリックします。

最初に、編集、 `Main.storyboard` Xcode の Interface Builder で テーブル ビューを選択して列の数を増やす 3 つ (3)。 次に、変更、**タイトル**する新しい列の`Action`:

[![](table-view-images/delete01.png "列名の編集")](table-view-images/delete01.png#lightbox)

ストーリー ボードに変更を保存し、Visual Studio for Mac の変更を同期するに戻ります。

次に、編集、`ViewController.cs`ファイルを開き、次のパブリック メソッドを追加します。

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

同じファイル内の新しいテーブル表示デリゲートの作成を変更`ViewDidLoad`メソッドとして、次のとおりです。

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

これで、編集、`ProductTableDelegate.cs`ビュー コント ローラーにプライベート接続を含めると、デリゲートの新しいインスタンスを作成するときに、パラメーターとして、コント ローラーを実行するファイル。

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

これにより、すべてのテキスト ビューの構成で行っていたされている、`GetViewForItem`メソッド、(テーブルの最後の列は、テキスト ビューがボタンには含まれません) ため、呼び出し可能な単一の場所に配置します。

最後に、編集、`GetViewForItem`メソッドと、次のようになります。

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

詳しくは、このコードのセクションではいくつか見てみましょう。 新しい場合は、まず`NSTableViewCell`がアクションを作成中に実行されますに基づいて列の名前。 最初の 2 つの列 (**製品**と**詳細**)、新しい`ConfigureTextField`メソッドが呼び出されます。

**アクション**列、新しい`NSButton`が作成され、サブ ビューとしてのセルに追加します。

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

ボタンの`Tag`プロパティは、現在処理されている行の数を保持するために使用します。 ユーザーがボタンの削除する行を要求したときに、この数を後で使用が`Activated`イベント。

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

イベント ハンドラーの先頭には、ボタンと、特定のテーブル行の上にある製品を取得します。 行の削除を確認するユーザーにアラートが表示されます。 ユーザーは、行を削除するが、特定の行がデータ ソースから削除されますが、テーブルが再読み込み。

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

最後に、テーブル ビューのセルが新規に作成されているのではなく再利用されている場合、次のコードを構成します処理されている列に基づきます。

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

**アクション**列で、すべてのサブ ビューがまでスキャン、`NSButton`が見つかった`Tag`プロパティは次のように、現在の行で更新されます。

これらの場所の変更、アプリの実行時に各の行がある、**削除**ボタン。

[![](table-view-images/delete02.png "削除ボタンを持つテーブル ビュー")](table-view-images/delete02.png#lightbox)

ユーザーがクリックすると、**削除**ボタン、特定の行を削除するかを確認する警告が表示されます。

[![](table-view-images/delete03.png "削除行のアラート")](table-view-images/delete03.png#lightbox)

ユーザーが削除を選択した場合、行が削除され、テーブルが再描画されます。

[![](table-view-images/delete04.png "行が削除された後の表に、")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>データ バインド テーブル ビュー

キー値コーディングとデータ バインディングの手法を使用すると、Xamarin.Mac アプリケーションで、コードの記述し、保守を設定し、UI 要素を使用する必要があるの量を大幅に短縮できます。 さらに、バックアップ データを分離の利点がある場合も (_データ モデル_)、正面からのユーザー インターフェイスを終了します (_モデル-ビュー-コント ローラー_)、柔軟性の高いアプリケーションを管理しやすくします。デザインします。

キー値コーディング (KVC) は、オブジェクトのプロパティを直接アクセスする、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用するためのメカニズム (`get/set`)。 キー値コーディング準拠アクセサーを実装すると、Xamarin.Mac アプリケーションで、キーと値を観察し (KVO)、データ バインディング、Core Data、Cocoa バインディング、およびあることを示すなどの他の macOS 機能へのアクセスを行えます。

詳細についてを参照してください、[テーブル ビューのデータ バインド](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding)のセクション、[データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでテーブルのビューの使用方法について詳しく説明をしました。 さまざまな種類を説明し、テーブルのビューを作成し、Xcode の Interface Builder でのテーブル ビューを維持する方法および c# コードでテーブルのビューを操作する方法を使用します。

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
