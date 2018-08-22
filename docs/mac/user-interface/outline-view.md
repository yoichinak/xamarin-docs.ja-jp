---
title: Xamarin.Mac のアウトライン ビュー
description: この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの使用について説明します。 作成および Xcode と Interface Builder でのアウトライン ビューを維持し、それらをプログラムで操作がについて説明します。
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7228a22eeae3dfdb354ba3693c277251fd2cd821
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251230"
---
# <a name="outline-views-in-xamarinmac"></a>Xamarin.Mac のアウトライン ビュー

_この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの使用について説明します。作成および Xcode と Interface Builder でのアウトライン ビューを維持し、それらをプログラムで操作がについて説明します。_

同じアクセス権がある、Xamarin.Mac アプリケーションで c# と .NET を使用する場合のアウトライン ビューで作業する開発者*Objective C*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成し、アウトライン ビューの管理 (または必要に応じて c# コードで直接作成) します。

アウトライン表示は、ユーザーができるテーブルの種類の展開または階層データの行を折りたたむには。 テーブル ビューでは、ように、アウトライン ビューには、個々 のアイテムとそれらの項目の属性を表す列を表す行を含む、一連の関連するアイテムのデータが表示されます。 テーブル ビューとは異なり、アウトライン表示で項目がフラット リストに含まれていない、ハード ドライブのファイルとフォルダーのように、階層で構成されます。

[![](outline-view-images/populate03.png "アプリの実行例")](outline-view-images/populate03.png#lightbox)

この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの操作の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>アウトライン ビューの概要

アウトライン表示は、ユーザーができるテーブルの種類の展開または階層データの行を折りたたむには。 テーブル ビューでは、ように、アウトライン ビューには、個々 のアイテムとそれらの項目の属性を表す列を表す行を含む、一連の関連するアイテムのデータが表示されます。 テーブル ビューとは異なり、アウトライン表示で項目がフラット リストに含まれていない、ハード ドライブのファイルとフォルダーのように、階層で構成されます。

アウトライン表示の項目に他の項目が含まれている場合に、展開またはユーザーが折りたたまれていることができます。 展開可能な項目には、アイテムが折りたたまれているし、下を向き、項目が展開されているときに、右を指す漏えい三角形が表示されます。 漏えい三角形をクリックすると、展開したり折りたたんだりする項目。

アウトライン ビュー (`NSOutlineView`)、テーブル ビューのサブクラス (`NSTableView`) し、そのため、多くの動作をその親クラスから継承します。 その結果、テーブルなどの表示、行または列を選択する列ヘッダーなどをドラッグして列を再配置でサポートされている多くの操作は、アウトライン表示でもサポートされます。 Xamarin.Mac アプリケーションは、これらの機能の制御を備え、特定の操作を許可または拒否する (コードまたはインターフェイス ビルダーでいずれか)、アウトライン ビューのパラメーターを構成できます。

アウトライン表示では、独自のデータを保存しませんに代わりに、データ ソースには、(`NSOutlineViewDataSource`) を行と、必要に応じてごとに、必要な列の両方を提供します。

アウトライン ビューのデリゲートのサブクラスを提供することにより、アウトライン ビューの動作をカスタマイズすることができます (`NSOutlineViewDelegate`) アウトラインの列の管理をサポートする機能、行の選択、編集、カスタムの追跡、および個々 のカスタム ビューを選択する入力列と行。

アウトライン表示では、テーブル ビューでの動作と機能の多くを共有するためを経由する可能性があります、[テーブル ビュー](~/mac/user-interface/table-view.md)この記事に進む前にドキュメント。

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>作成して、Xcode でのアウトライン ビューの保守

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリック、`Main.storyboard`ファイル。

[![](outline-view-images/edit01.png "メインのストーリー ボードを選択します。")](outline-view-images/edit01.png#lightbox)

これが、Xcode の Interface Builder でウィンドウのデザインを開きます。

[![](outline-view-images/edit02.png "Xcode では、UI の編集")](outline-view-images/edit02.png#lightbox)

型`outline`に、**ライブラリ インスペクターの**アウトライン ビューのコントロールを検索するが容易に検索ボックス。

[![](outline-view-images/edit03.png "ライブラリから、アウトライン ビューの選択")](outline-view-images/edit03.png#lightbox)

アウトライン ビューのビュー コント ローラーにドラッグ、**インターフェイス エディター**、ビュー コント ローラーのコンテンツ領域を入力し、縮小して、ウィンドウにも拡張に設定して、**制約エディター**:

[![](outline-view-images/edit04.png "制約の編集")](outline-view-images/edit04.png#lightbox)

アウトライン ビューを選択して、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](outline-view-images/edit05.png "属性インスペクター")](outline-view-images/edit05.png#lightbox)

- **列のアウトライン**-階層のデータを表示する、テーブルの列。
- **自動アウトライン列**場合 - `true`、自動的に保存され、アプリケーションの実行の復元は、アウトライン列。
- **インデント**-列を展開したアイテムをインデントする量。
- **インデントに依存してセル**場合 -`true`セルとインデントのマークがインデントされます。
- **展開されたアイテムを自動保存**場合 - `true`、自動的に保存され、アプリケーションの実行の復元は項目の展開/折りたたみの状態。
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
> 従来の Xamarin.Mac アプリケーションを保持している場合を除き、`NSView`ベースのアウトライン ビューで使用する必要があります`NSCell`ベースのテーブルのビュー。 `NSCell` 従来と見なされ、今後はサポートされません。

テーブル列の選択、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](outline-view-images/edit06.png "属性インスペクター")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "属性インスペクター")](outline-view-images/edit07.png#lightbox)

これらは、すべての標準的なビューのプロパティです。 また、ここで、この列の行のサイズ変更オプションがあります。

Table View Cell を選択します (既定では、これは、 `NSTextField`) で、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](outline-view-images/edit08.png "属性インスペクター")](outline-view-images/edit08.png#lightbox)

ここで設定する標準のテキスト フィールドのすべてのプロパティがあります。 既定では、標準のテキスト フィールドは、列に、セルのデータを表示する使用します。

テーブル セルのビューを選択します (`NSTableFieldCell`) で、**インターフェイス階層**し、次のプロパティが使用可能、**属性インスペクター**:

[![](outline-view-images/edit09.png "属性インスペクター")](outline-view-images/edit09.png#lightbox)

ここでの最も重要な設定は次のとおりです。

- **レイアウト**- この列のセルのレイアウト方法を選択します。
- **単一行モードを使用して**場合 -`true`セルが 1 行に制限されています。
- **最初の実行時レイアウト**場合 -`true`セルの幅の設定を (手動または自動で) 場合は優先初めて、アプリケーションの実行が表示されます。
- **アクション**のタイミングを制御、編集**アクション**セルが送信されます。
- **動作**のセルが選択可能なまたは編集可能な場合を定義します。
- **リッチ テキスト**場合 -`true`セルは、書式設定とスタイル設定されたテキストを表示できます。
- **元に戻す**場合 - `true`、に対しては、セルが責任を負います動作を元に戻します。

テーブル セルのビューを選択します (`NSTableFieldCell`) のテーブルの列の下部にある、**インターフェイス階層**:

[![](outline-view-images/edit11.png "テーブル セルのビューを選択します。")](outline-view-images/edit10.png#lightbox)

ベースとして使用するテーブル セル ビューを編集できます_パターン_のすべてのセルを指定した列を作成します。

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>アクションとアウトレットを追加します。

だけ Cocoa UI コントロールと同様に、アウトライン ビューを公開する必要があり、その列は、c# コードを使用するセル**アクション**と**Outlet** (必要な機能に基づく)。

プロセスは、任意のアウトライン ビュー要素を公開することで同じです。

1. 切り替えて、**アシスタント エディター**いることを確認し、`ViewController.h`ファイルが選択されています。 

    [![](outline-view-images/edit11.png "正しい .h ファイルを選択します。")](outline-view-images/edit11.png#lightbox)
2. アウトライン ビューの選択、**インターフェイス階層**、コントロールのクリックおよびドラッグ、`ViewController.h`ファイル。
3. 作成、**アウトレット**アウトライン ビューと呼ばれる`ProductOutline`: 

    [![](outline-view-images/edit13.png "アウトレットを構成します。")](outline-view-images/edit13.png#lightbox)
4. 作成**Outlet**と呼ばれるテーブルの列も`ProductColumn`と`DetailsColumn`: 

    [![](outline-view-images/edit14.png "アウトレットを構成します。")](outline-view-images/edit14.png#lightbox)
5. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

次に、データを書き込むコード表示いくつかの概要については、アプリケーションの実行時にします。

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>アウトライン ビューの作成

アウトライン表示では、Interface Builder で設計され、を介して公開される、**アウトレット**、次に読み込むための c# コードを作成する必要があります。

最初に、新しいを作成しましょう`Product`個々 の行とサブ製品のグループの情報を保持するクラス。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.** 選択**一般的な** > **空のクラス**、入力`Product`の**名前** をクリックし、**新規**ボタン。

[![](outline-view-images/populate01.png "空のクラスを作成します。")](outline-view-images/populate01.png#lightbox)

ように、`Product.cs`ファイルの次のようになります。

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

次に、私たちのサブクラスを作成する必要があります`NSOutlineDataSource`を要求に応じて、当社の概要については、データを提供します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.** 選択**全般** > **空のクラス**、入力`ProductOutlineDataSource`の**名前** をクリック、**新規**ボタン。

編集、`ProductTableDataSource.cs`ファイルを開き、次のようになります。

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

このクラスは、アウトライン ビューの項目用のストレージとよりも優先されます、`GetChildrenCount`をテーブルの行の数を返します。 `GetChild` (アウトライン ビューからの要求) では、特定の親または子項目を返します、`ItemExpandable`親または子のいずれかとして指定した項目を定義します。

最後に、私たちのサブクラスを作成する必要があります`NSOutlineDelegate`当社の概要については、動作を提供します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.** 選択**全般** > **空のクラス**、入力`ProductOutlineDelegate`の**名前** をクリック、**新規**ボタン。

編集、`ProductOutlineDelegate.cs`ファイルを開き、次のようになります。

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

インスタンスを作成するとき、 `ProductOutlineDelegate`、私たちのインスタンスに渡すことも、`ProductOutlineDataSource`の概要については、データを提供します。 `GetView`メソッドが指定した列と行のセルを表示するビュー (データ) が返されます。 可能であれば、既存のビューは、セルを表示する再利用するでない場合、新しいビューを作成する必要があります。

アウトラインを設定するには、編集、`MainWindow.cs`ファイル、`AwakeFromNib`メソッドの次のようになります。

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

アプリケーションを実行した場合、次のように表示されます。

[![](outline-view-images/populate02.png "折りたたまれたビュー")](outline-view-images/populate02.png#lightbox)

アウトライン ビューのノードを展開し場合、次のようになります。

[![](outline-view-images/populate03.png "展開ビュー")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>列で並べ替え

列ヘッダーをクリックして、アウトラインにあるデータの並べ替えにユーザーを許可します。 最初をダブルクリック、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 選択、`Product`列、入力`Title`の**並べ替えキー**、`compare:`の**セレクター**選択と`Ascending`の**順序**:

[![](outline-view-images/sort01.png "並べ替えキーの順序を設定します。")](outline-view-images/sort01.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ編集しましょう、`ProductOutlineDataSource.cs`ファイルを開き、次のメソッドを追加します。

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

`Sort`メソッドでは、データに基づくデータ ソースの並べ替えにより、指定された`Product`昇順または降順のどちらかのクラス フィールド。 オーバーライドされた`SortDescriptorsChanged`使用列見出しをクリックするたびにメソッドが呼び出されます。 渡される、**キー** Interface Builder とその列の並べ替え順序で設定される値。

アプリケーションを実行し、列ヘッダーをクリックした場合、行がその列で並べ替えられます。

[![](outline-view-images/sort02.png "並べ替えの出力の例")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>行の選択

ユーザーが 1 つの行を選択し、ダブルクリックできるようにするかどうか、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 アウトライン ビューを選択して、**インターフェイス階層**をオフにし、**複数**でチェック ボックスをオン、**属性インスペクター**:

[![](outline-view-images/select01.png "属性インスペクター")](outline-view-images/select01.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。


次に、編集、`ProductOutlineDelegate.cs`ファイルを開き、次のメソッドを追加します。

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

これによって、ユーザーをアウトライン ビューの任意の 1 つの行を選択します。 返す`false`の`ShouldSelectItem`のいずれかの項目を選択できるユーザーをしたくないまたは`false`任意の項目を選択することができるユーザーをしたくない場合は、各項目にします。

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>複数行の選択

ユーザーが複数の行を選択し、ダブルクリックできるようにするかどうか、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 アウトライン ビューを選択して、**インターフェイス階層**を確認し、**複数**でチェック ボックスをオン、**属性インスペクター**:

[![](outline-view-images/select02.png "属性インスペクター")](outline-view-images/select02.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。


次に、編集、`ProductOutlineDelegate.cs`ファイルを開き、次のメソッドを追加します。

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

これによって、ユーザーをアウトライン ビューの任意の 1 つの行を選択します。 返す`false`の`ShouldSelectRow`のいずれかの項目を選択できるユーザーをしたくないまたは`false`任意の項目を選択することができるユーザーをしたくない場合は、各項目にします。

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>行を選択する種類

その文字を持つユーザーが選択されているアウトライン ビューで文字を入力できるようにして、最初の行を選択する場合、ダブルクリックして、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 アウトライン ビューを選択して、**インターフェイス階層**を確認し、**の種類を選択**でチェック ボックスをオン、**属性インスペクター**:

[![](outline-view-images/type01.png "行の種類の編集")](outline-view-images/type01.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ編集しましょう、`ProductOutlineDelegate.cs`ファイルを開き、次のメソッドを追加します。

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

`GetNextTypeSelectMatch`メソッドは、指定された`searchString`最初の項目を返しますと`Product`での文字列を持つ`Title`。

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>列の並べ替え

ユーザーがドラッグできるようにする場合は、アウトライン ビューの列の並べ替えをダブルクリックします、`Main.storyboard`ファイルを開き、インターフェイス ビルダーで編集します。 アウトライン ビューを選択して、**インターフェイス階層**を確認し、**並べ替え**でチェック ボックスをオン、**属性インスペクター**:

[![](outline-view-images/reorder01.png "属性インスペクター")](outline-view-images/reorder01.png#lightbox)

値をプレゼントした場合、 **Autosave**プロパティとチェック、**列情報**フィールドでは、テーブルのレイアウトに行った変更を加えた私たちにとっては自動的に保存して、次回アプリケーションを復元実行されます。

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ編集しましょう、`ProductOutlineDelegate.cs`ファイルを開き、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder`メソッドが返す必要があります`true`にそれを許可する任意の列のドラッグの順序変更、 `newColumnIndex`, それ以外の場合、return `false`;

アプリケーションを実行した場合、列の順序を変更する列の見出しをドラッグしたことができます。

[![](outline-view-images/reorder02.png "並べ替え列の例")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>セルの編集

ユーザーが特定のセルの値を編集、編集を許可する場合、`ProductOutlineDelegate.cs`ファイルし、変更、`GetViewForItem`メソッドとして、次のとおりです。

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

今すぐアプリケーションを実行、ユーザーはテーブル ビュー内のセルを編集できます。

[![](outline-view-images/editing01.png "セルの編集の例")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>アウトライン ビューのイメージを使用します。

内のセルの一部としてイメージを含める、 `NSOutlineView`、アウトライン ビューのデータを返す方法を変更する必要があります`NSTableViewDelegate's``GetView`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

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

詳細についてを参照してください、[アウトライン ビューの使用を使用してイメージ](~/mac/app-fundamentals/image.md)のセクション、[画像](~/mac/app-fundamentals/image.md)ドキュメント。

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>データ バインディングのアウトライン ビュー

キー値コーディングとデータ バインディングの手法を使用すると、Xamarin.Mac アプリケーションで、コードの記述し、保守を設定し、UI 要素を使用する必要があるの量を大幅に短縮できます。 さらに、バックアップ データを分離の利点がある場合も (_データ モデル_)、正面からのユーザー インターフェイスを終了します (_モデル-ビュー-コント ローラー_)、柔軟性の高いアプリケーションを管理しやすくします。デザインします。

キー値コーディング (KVC) は、オブジェクトのプロパティを直接アクセスする、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用するためのメカニズム (`get/set`)。 キー値コーディング準拠アクセサーを実装すると、Xamarin.Mac アプリケーションで、キーと値を観察し (KVO)、データ バインディング、Core Data、Cocoa バインディング、およびあることを示すなどの他の macOS 機能へのアクセスを行えます。

詳細についてを参照してください、[アウトライン ビューのデータ バインディング](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding)のセクション、[データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの使用方法について詳しく説明をしました。 さまざまな種類を確認し、アウトライン ビューを作成し、Xcode の Interface Builder でのアウトライン ビューを維持する方法、c# コードのアウトライン ビューで操作する方法を使用します。

## <a name="related-links"></a>関連リンク

- [MacOutlines (サンプル)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [ソース リスト](~/mac/user-interface/source-list.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [アウトライン ビューの概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
