---
title: Xamarin. Mac のアウトラインビュー
description: この記事では、Xamarin. Mac アプリケーションでアウトラインビューを使用する方法について説明します。 Xcode と Interface Builder でのアウトライン表示の作成と管理、およびプログラムによる作業について説明します。
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 7f1ae2ecfa7d6dbed56b8009593fc172615fd051
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928797"
---
# <a name="outline-views-in-xamarinmac"></a>Xamarin. Mac のアウトラインビュー

_この記事では、Xamarin. Mac アプリケーションでアウトラインビューを使用する方法について説明します。Xcode と Interface Builder でのアウトライン表示の作成と管理、およびプログラムによる作業について説明します。_

Xamarin. Mac アプリケーションで C# と .NET を使用する場合、 *Xcode と*で作業している開発*者と同じ*アウトラインビューにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してアウトラインビューを作成および管理できます (または、必要に応じて C# コードで直接作成することもできます)。

アウトラインビューは、ユーザーが階層データの行を展開したり折りたたんだりできるようにするテーブルの一種です。 テーブルビューと同様に、アウトラインビューには、関連するアイテムのセットのデータが表示されます。これらのアイテムの属性を表す行は、個々のアイテムと列を表します。 テーブルビューとは異なり、アウトラインビューの項目は、単純なリストに含まれていません。これらは、ハードドライブ上のファイルやフォルダーなどの階層に編成されています。

[![アプリの実行例](outline-view-images/populate03.png)](outline-view-images/populate03.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでアウトラインビューを操作する方法の基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に[Hello, Mac](~/mac/get-started/hello-mac.md)の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)」と「[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

<a name="Introduction_to_Outline_Views"></a>

## <a name="introduction-to-outline-views"></a>アウトラインビューの概要

アウトラインビューは、ユーザーが階層データの行を展開したり折りたたんだりできるようにするテーブルの一種です。 テーブルビューと同様に、アウトラインビューには、関連するアイテムのセットのデータが表示されます。これらのアイテムの属性を表す行は、個々のアイテムと列を表します。 テーブルビューとは異なり、アウトラインビューの項目は、単純なリストに含まれていません。これらは、ハードドライブ上のファイルやフォルダーなどの階層に編成されています。

アウトラインビューのアイテムに他のアイテムが含まれている場合は、ユーザーが展開または折りたたむことができます。 展開可能な項目には、項目が折りたたまれたときに右側を指し、項目が展開されたときに下向きの三角形が表示されます。 [公開] 三角形をクリックすると、項目が展開または折りたたまれます。

アウトラインビュー ( `NSOutlineView` ) は、テーブルビュー () のサブクラスであり、そのため `NSTableView` 、その親クラスから動作の多くを継承します。 その結果、テーブルビューでサポートされている多くの操作 (行や列の選択、列ヘッダーのドラッグによる列の再配置など) も、アウトラインビューでサポートされます。 Xamarin アプリケーションはこれらの機能を制御し、特定の操作を許可または禁止するためにアウトラインビューのパラメーター (コードまたは Interface Builder) を構成できます。

アウトラインビューは、独自のデータを格納するのではなく、必要に応じて、データソース () を使用して `NSOutlineViewDataSource` 必要な行と列の両方を提供します。

アウトラインビューの動作をカスタマイズするには、アウトラインビューのデリゲート () のサブクラスを使用して、 `NSOutlineViewDelegate` アウトライン列の管理、型の選択、機能の選択、行の選択と編集、カスタム追跡、および個々の列と行のカスタムビューをサポートします。

アウトラインビューでは、その動作と機能の多くがテーブルビューで共有されるため、この記事を続行する前に、[テーブルビュー](~/mac/user-interface/table-view.md)のドキュメントを参照することをお勧めします。

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode"></a>

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Xcode でのアウトライン表示の作成と管理

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは `.storyboard` 、プロジェクトに自動的に含まれるファイルに定義されています。 Windows のデザインを編集するには、**ソリューションエクスプローラー**で、ファイルをダブルクリックし `Main.storyboard` ます。

[![メインストーリーボードの選択](outline-view-images/edit01.png)](outline-view-images/edit01.png#lightbox)

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![Xcode で UI を編集する](outline-view-images/edit02.png)](outline-view-images/edit02.png#lightbox)

`outline`**ライブラリインスペクターの**検索ボックスに「」と入力すると、アウトラインビューコントロールを簡単に見つけることができます。

[![ライブラリからアウトラインビューを選択する](outline-view-images/edit03.png)](outline-view-images/edit03.png#lightbox)

**インターフェイスエディター**で、ビューコントローラーにアウトラインビューをドラッグし、ビューコントローラーのコンテンツ領域を塗りつぶすように設定します。次に、**制約エディター**でウィンドウを使用して縮小および拡大する場所を設定します。

[![制約の編集](outline-view-images/edit04.png)](outline-view-images/edit04.png#lightbox)

**インターフェイス階層**でアウトラインビューを選択すると、**属性インスペクター**で次のプロパティを使用できます。

[![属性インスペクター](outline-view-images/edit05.png)](outline-view-images/edit05.png#lightbox)

- [**アウトライン列**-階層データが表示されるテーブル列です。
- [**自動保存アウトライン列**]-の場合 `true` 、アプリケーションの実行間でアウトライン列が自動的に保存され、復元されます。
- **インデント**-展開された項目の下に列をインデントする量。
- **インデントはセル**に従い `true` ます。の場合、インデント記号はセルと共にインデントされます。
- [展開された**アイテム**を自動的に保存する]-の場合、展開されたアイテムの折りたたまれた状態が、 `true` アプリケーションの実行間で自動的に保存され、復元されます。
- **コンテンツモード**-ビュー () またはセル () のいずれかを使用して、 `NSView` `NSCell` 行と列のデータを表示できます。 MacOS 10.7 以降では、ビューを使用する必要があります。
- [**フローティンググループ行]** -If `true` テーブルビューでは、グループ化されたセルが浮動小数点型として描画されます。
- **Columns** -表示される列の数を定義します。
- **Headers** -If `true` 列にはヘッダーが含まれます。
- **順序**を変更する-の場合 `true` 、ユーザーはテーブル内の列の順序を変更することができます。
- [**サイズ変更**]-の場合 `true` 、ユーザーは列ヘッダーをドラッグして列のサイズを変更できます。
- **列**のサイズ変更-テーブルの列のサイズを自動調整する方法を制御します。
- **強調表示**-セルを選択したときにテーブルで使用する強調表示の種類を制御します。
- **代替行**-If `true` 、その他の行の背景色は異なります。
- **水平グリッド**-横方向のセルの間に描画する境界線の種類を選択します。
- **垂直グリッド**-セル間に描画する境界線の種類を垂直方向に選択します。
- [**グリッドの色**-セルの境界線の色を設定します。
- **Background** -セルの背景色を設定します。
- **選択**-ユーザーがテーブル内のセルを選択する方法を、次のように制御できます。
  - **複数**の場合 `true` 、ユーザーは複数の行と列を選択できます。
  - **列**-の場合 `true` 、ユーザーは列を選択できます。
  - **「Select** -If」と入力すると、 `true` ユーザーは文字を入力して行を選択できます。
  - **Empty** - `true` ユーザーが行または列を選択する必要がない場合、テーブルでは何も選択できません。
- [**自動**保存]-テーブル形式が自動的に保存される名前。
- **列情報**-の場合、 `true` 列の順序と幅が自動的に保存されます。
- [**改行]-セル**が改行を処理する方法を選択します。
- **最後に表示**される行を切り捨て `true` ます。の場合、セルはデータ内で切り捨てられ、その範囲内に収まりません。

> [!IMPORTANT]
> 従来の Xamarin. Mac アプリケーションを維持していない場合は、ベースの `NSView` テーブルビューに基づくアウトラインビューを使用する必要があり `NSCell` ます。 `NSCell`は従来と見なされ、今後サポートされない可能性があります。

**インターフェイス階層**内のテーブル列を選択します。**属性インスペクター**では、次のプロパティを使用できます。

[![属性インスペクター](outline-view-images/edit06.png)](outline-view-images/edit06.png#lightbox)

- **タイトル**-列のタイトルを設定します。
- **Alignment** -セル内のテキストの配置を設定します。
- [**タイトルのフォント**]-セルのヘッダーテキストのフォントを選択します。
- **Sort key** -列のデータを並べ替えるために使用されるキーです。 ユーザーがこの列を並べ替えられない場合は、空白のままにします。
- **Selector** -並べ替えを実行するために使用される**アクション**です。 ユーザーがこの列を並べ替えられない場合は、空白のままにします。
- **Order** -列データの並べ替え順序です。
- [**サイズ変更**中-列のサイズ変更の種類を選択します。
- **編集可能**-の場合 `true` 、ユーザーはセルベーステーブル内のセルを編集できます。
- **Hidden** -の場合 `true` 、列は非表示になります。

また、列のハンドル (垂直方向に列の右側に中央揃え) をドラッグして列のサイズを変更することもできます。

テーブルビューの各列を選択し、最初の列に**タイトル** `Product` と2番目の列を指定してみましょう `Details` 。

インターフェイス階層でテーブルセルビュー () を選択する `NSTableViewCell` と、**属性インスペクター**で次のプロパティを使用できます。 **Interface Hierarchy**

[![属性インスペクター](outline-view-images/edit07.png)](outline-view-images/edit07.png#lightbox)

これらは、標準ビューのすべてのプロパティです。 また、この列の行のサイズを変更することもできます。

インターフェイス階層でテーブルビューセル (既定では) を選択する `NSTextField` と**Interface Hierarchy** 、**属性インスペクター**で次のプロパティを使用できます。

[![属性インスペクター](outline-view-images/edit08.png)](outline-view-images/edit08.png#lightbox)

ここでは、標準のテキストフィールドのすべてのプロパティを設定します。 既定では、標準のテキストフィールドを使用して、列のセルのデータが表示されます。

インターフェイス階層でテーブルセルビュー () を選択する `NSTableFieldCell` と、**属性インスペクター**で次のプロパティを使用できます。 **Interface Hierarchy**

[![属性インスペクター](outline-view-images/edit09.png)](outline-view-images/edit09.png#lightbox)

ここで最も重要な設定は次のとおりです。

- [**レイアウト**]-この列のセルのレイアウトを選択します。
- **単一行モードを使用**し `true` ます。の場合、セルは1行に制限されます。
- **最初の実行時レイアウトの幅**-If の場合、アプリケーションを初めて実行したときに表示されるときに、 `true` セルの幅 (手動または自動) が優先されます。
- **Action** -セルに対して編集**アクション**が送信されるタイミングを制御します。
- **Behavior** -セルを選択可能にするか、編集可能にするかを定義します。
- **リッチテキスト**-If `true` セルには、書式設定されたテキストを表示できます。
- [**元に戻す**]: `true` このセルは、元に戻す動作の役割を担います。

`NSTableFieldCell`**インターフェイス階層**のテーブル列の下部にあるテーブルセルビュー () を選択します。

[![テーブルセルビューの選択](outline-view-images/edit11.png)](outline-view-images/edit10.png#lightbox)

これにより、特定の列に対して作成されたすべてのセルのベース_パターン_として使用されるテーブルセルビューを編集できます。

<a name="Adding_Actions_and_Outlets"></a>

### <a name="adding-actions-and-outlets"></a>アクションとアウトレットの追加

他の Cocoa UI コントロールと同様に、アウトラインビューを公開し、**アクション**と**アウトレット**(必要な機能に基づく) を使用して C# コードに列とセルを表示する必要があります。

このプロセスは、公開するすべてのアウトラインビュー要素で同じです。

1. **アシスタントエディター**に切り替えて、ファイルが選択されていることを確認し `ViewController.h` ます。

    [![正しい .h ファイルを選択する](outline-view-images/edit11.png)](outline-view-images/edit11.png#lightbox)
2. **インターフェイス階層**からアウトラインビューを選択し、コントロールをクリックして、ファイルにドラッグし `ViewController.h` ます。
3. という名前の [アウトライン] ビューの**アウトレット**を作成し `ProductOutline` ます。

    [![アウトレットの構成](outline-view-images/edit13.png)](outline-view-images/edit13.png#lightbox)
4. テーブルの列に対しても、およびと呼ばれる**コンセント**を作成し `ProductColumn` `DetailsColumn` ます。

    [![アウトレットの構成](outline-view-images/edit14.png)](outline-view-images/edit14.png#lightbox)
5. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、アプリケーションの実行時にアウトラインのいくつかのデータを表示するコードを記述します。

<a name="Populating_the_Table_View"></a>

## <a name="populating-the-outline-view"></a>アウトライン表示の設定

Interface Builder で設計され、**アウトレット**を介して公開されるアウトラインビューでは、次に C# コードを作成してそれを設定する必要があります。

まず、 `Product` サブ製品の個々の行とグループの情報を保持する新しいクラスを作成してみましょう。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `Product` 、[**新規**] ボタンをクリックします。 **Name**

[![空のクラスの作成](outline-view-images/populate01.png)](outline-view-images/populate01.png#lightbox)

ファイルの `Product.cs` 外観を次のようにします。

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
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

次に、のサブクラスを作成し `NSOutlineDataSource` て、要求に応じてアウトラインのデータを提供する必要があります。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `ProductOutlineDataSource` 、[**新規**] ボタンをクリックします。 **Name**

ファイルを編集 `ProductTableDataSource.cs` し、次のように表示します。

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }

        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }

        }
        #endregion
    }
}
```

このクラスは、アウトラインビューの項目のストレージを保持し、をオーバーライドして `GetChildrenCount` テーブル内の行の数を返します。 は、 `GetChild` (アウトラインビューによって要求された) 特定の親項目または子項目を返し `ItemExpandable` ます。は、指定された項目を親または子として定義します。

最後に、のサブクラスを作成し `NSOutlineDelegate` て、アウトラインの動作を提供する必要があります。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加**  >  **...** ] を選択します。[**汎用**空のクラス] を選択し、名前として「」  >  **Empty Class**と入力して `ProductOutlineDelegate` 、[**新規**] ボタンをクリックします。 **Name**

ファイルを編集 `ProductOutlineDelegate.cs` し、次のように表示します。

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

のインスタンスを作成するときに、 `ProductOutlineDelegate` `ProductOutlineDataSource` アウトラインのデータを提供するのインスタンスも渡します。 `GetView`メソッドは、ビュー (データ) を取得して、列と列を指定するセルを表示します。 可能であれば、新しいビューを作成する必要がない場合は、既存のビューを再利用してセルを表示します。

アウトラインを設定するには、ファイルを編集 `MainWindow.cs` して、 `AwakeFromNib` メソッドを次のようにします。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

アプリケーションを実行すると、次のように表示されます。

[![折りたたまれたビュー](outline-view-images/populate02.png)](outline-view-images/populate02.png#lightbox)

[アウトライン] ビューでノードを展開すると、次のように表示されます。

[![展開されたビュー](outline-view-images/populate03.png)](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column"></a>

## <a name="sorting-by-column"></a>列で並べ替え

列ヘッダーをクリックして、ユーザーがアウトライン内のデータを並べ替えることができるようにします。 まず、ファイルをダブルクリックし `Main.storyboard` て、Interface Builder で編集するために開きます。 列を選択し、並べ替えキーとして「」 `Product` と入力し `Title` **Sort Key** `compare:` **Selector** `Ascending` ます。 **Order**

[![並べ替えキーの順序を設定する](outline-view-images/sort01.png)](outline-view-images/sort01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、ファイルを編集 `ProductOutlineDataSource.cs` し、次のメソッドを追加します。

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
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

メソッドを使用すると、指定した `Sort` クラスフィールドに基づいてデータソース内のデータを `Product` 昇順または降順で並べ替えることができます。 オーバーライド `SortDescriptorsChanged` されたメソッドは、が列見出しをクリックするたびに呼び出されます。 Interface Builder に設定した**キー**値とその列の並べ替え順序が渡されます。

アプリケーションを実行して列ヘッダーをクリックすると、行はその列で並べ替えられます。

[![並べ替えられた出力の例](outline-view-images/sort02.png)](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection"></a>

## <a name="row-selection"></a>行の選択

ユーザーが単一行を選択できるようにするには、ファイルをダブルクリックし `Main.storyboard` て、Interface Builder で編集するために開きます。 **インターフェイス階層**でアウトラインビューを選択し、**属性インスペクター**の [**複数**] チェックボックスをオフにします。

[![属性インスペクター](outline-view-images/select01.png)](outline-view-images/select01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、ファイルを編集 `ProductOutlineDelegate.cs` し、次のメソッドを追加します。

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

これにより、ユーザーはアウトラインビューで任意の1行を選択できるようになります。 ユーザーが項目を選択できないようにする `false` `ShouldSelectItem` 場合は、ユーザーが選択できない項目についてはを返し、すべての項目についてはを返し `false` ます。

<a name="Multiple_Row_Selection"></a>

## <a name="multiple-row-selection"></a>複数行の選択

ユーザーが複数の行を選択できるようにするには、ファイルをダブルクリックし `Main.storyboard` て、Interface Builder で編集するために開きます。 **インターフェイス階層**でアウトラインビューを選択し、**属性インスペクター**の [**複数**] チェックボックスをオンにします。

[![属性インスペクター](outline-view-images/select02.png)](outline-view-images/select02.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、ファイルを編集 `ProductOutlineDelegate.cs` し、次のメソッドを追加します。

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

これにより、ユーザーはアウトラインビューで任意の1行を選択できるようになります。 ユーザーが項目を選択できないようにする `false` `ShouldSelectRow` 場合は、ユーザーが選択できない項目についてはを返し、すべての項目についてはを返し `false` ます。

<a name="Type_to_Select_Row"></a>

## <a name="type-to-select-row"></a>入力して行を選択します

ユーザーがアウトライン表示を選択した文字を入力し、その文字を含む最初の行を選択できるようにするには、ファイルをダブルクリックし `Main.storyboard` て、Interface Builder で編集するために開きます。 **インターフェイス階層**でアウトラインビューを選択し、**属性インスペクター**の **[型の選択**] チェックボックスをオンにします。

[![行の種類の編集](outline-view-images/type01.png)](outline-view-images/type01.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、ファイルを編集 `ProductOutlineDelegate.cs` し、次のメソッドを追加します。

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

メソッドは、指定されたを受け取り、その `GetNextTypeSelectMatch` 文字列を `searchString` 含む最初のの項目を返し `Product` `Title` ます。

<a name="Reordering_Columns"></a>

## <a name="reordering-columns"></a>列の並べ替え

アウトラインビューでユーザーが列の順序を変更できるようにするには、ファイルをダブルクリックし `Main.storyboard` て、Interface Builder で編集するために開きます。 **インターフェイス階層**でアウトラインビューを選択し、**属性インスペクター**の [**並べ替え**] チェックボックスをオンにします。

[![属性インスペクター](outline-view-images/reorder01.png)](outline-view-images/reorder01.png#lightbox)

"**自動保存**" プロパティに値を指定して [**列情報**] フィールドを確認すると、テーブルのレイアウトに対して行った変更は自動的に保存され、次にアプリケーションが実行されたときに復元されます。

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ここで、ファイルを編集 `ProductOutlineDelegate.cs` し、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder`メソッドは `true` 、ドラッグを許可する任意の列に対してを返し、それ以外の場合はを `newColumnIndex` 返し `false` ます。

アプリケーションを実行すると、列のヘッダーをドラッグして列の順序を変更できます。

[![列の並べ替えの例](outline-view-images/reorder02.png)](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells"></a>

## <a name="editing-cells"></a>セルの編集

ユーザーが特定のセルの値を編集できるようにするには、ファイルを編集 `ProductOutlineDelegate.cs` し、次のようにメソッドを変更し `GetViewForItem` ます。

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

アプリケーションを実行すると、ユーザーはテーブルビューのセルを編集できるようになります。

[![セルを編集する例](outline-view-images/editing01.png)](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views"></a>

## <a name="using-images-in-outline-views"></a>アウトラインビューでの画像の使用

画像を内のセルの一部として含めるには、 `NSOutlineView` アウトラインビューのメソッドによるデータの取得方法を変更して、 `NSTableViewDelegate's` `GetView` `NSTableCellView` 通常のの代わりにを使用する必要があり `NSTextField` ます。 次に例を示します。

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

詳細については、[イメージの操作](~/mac/app-fundamentals/image.md)に関するドキュメントの「[アウトラインビューでのイメージの使用](~/mac/app-fundamentals/image.md)」セクションを参照してください。

<a name="Data_Binding_Outline_Views"></a>

## <a name="data-binding-outline-views"></a>データバインディングのアウトラインビュー

UI 要素を設定して操作するために、Xamarin. Mac アプリケーションでキー値のコードとデータバインディングの手法を使用することにより、記述して維持する必要があるコードの量を大幅に減らすことができます。 また、フロントエンドのユーザーインターフェイス (_モデルビューコントローラー_) からバッキングデータ (_データモデル_) をさらに分離することもできます。これにより、管理が容易になり、アプリケーションの設計をより柔軟に行うことができます。

キー値のコーディング (KVC) は、オブジェクトのプロパティに間接的にアクセスするためのメカニズムです。キー (特殊な書式設定文字列) を使用して、インスタンス変数またはアクセサーメソッド () を使用してアクセスするのではなく、プロパティを識別し `get/set` ます。 Xamarin. Mac アプリケーションでキー値のコーディングに準拠したアクセサーを実装することによって、キー値の観察 (KVO)、データバインディング、コアデータ、Cocoa バインド、および scriptability などの他の macOS 機能にアクセスできます。

詳細については、[データバインディングとキー値のコーディング](~/mac/app-fundamentals/databinding.md)に関するドキュメントの「[データバインディングの概要](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding)」セクションを参照してください。

<a name="Summary"></a>

## <a name="summary"></a>要約

この記事では、Xamarin. Mac アプリケーションでアウトラインビューを使用する方法について詳しく説明しました。 アウトラインビューのさまざまな型と用途、Xcode の Interface Builder でアウトラインビューを作成および管理する方法、C# コードでアウトラインビューを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacOutlines (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [MacImages (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [ソース リスト](~/mac/user-interface/source-list.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [アウトラインビューの概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
