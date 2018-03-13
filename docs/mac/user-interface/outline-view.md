---
title: "アウトライン ビュー"
description: "この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューでの作業について説明します。 これは、作成および Xcode とインターフェイスのビルダーのアウトライン表示を維持し、それらのプログラムの操作について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: dbbd10af046c0a8421e06e675364f92405b2317f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="outline-views"></a>アウトライン ビュー

_この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューでの作業について説明します。これは、作成および Xcode とインターフェイスのビルダーのアウトライン表示を維持し、それらのプログラムの操作について説明します。_

同じアクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときのアウトライン ビュー作業を行う開発者*Objective C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、アウトライン ビューは、ユーザーの管理 (または必要に応じて c# コードで直接作成すること)。

アウトライン表示が、型、ユーザーができるテーブルの順に展開階層データの行を折りたたみます。 テーブル ビューのようには、アウトライン表示は、個々 のアイテムとそれらの項目の属性を表す列を表す行を含む関連項目のセット用のデータを表示します。 テーブル ビューとは異なり、アウトライン表示の項目は単純なリストではありません、階層では、ハード ドライブのファイルとフォルダーのように構成されます。

[![](outline-view-images/populate03.png "実行のサンプル アプリ")](outline-view-images/populate03.png#lightbox)

この記事でしれません Xamarin.Mac アプリケーションでのアウトライン ビューの操作の基礎をについて説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>ビューを説明する概要

アウトライン表示が、型、ユーザーができるテーブルの順に展開階層データの行を折りたたみます。 テーブル ビューのようには、アウトライン表示は、個々 のアイテムとそれらの項目の属性を表す列を表す行を含む関連項目のセット用のデータを表示します。 テーブル ビューとは異なり、アウトライン表示の項目は単純なリストではありません、階層では、ハード ドライブのファイルとフォルダーのように構成されます。

アウトライン表示の項目に他の項目が含まれている場合、展開したりできるユーザーでは折りたたまれてです。 展開可能な項目は、アイテムが折りたたまれているし、項目が展開されている場合に、ダウン ポイント右側を指す三角を表示します。 漏えいの三角形をクリックすると場合、展開したり折りたたんだりする項目。

アウトライン表示 (`NSOutlineView`) テーブルのビューのサブクラス (`NSTableView`) し、そのため、多くの動作をその親クラスから継承します。 その結果、テーブルなど、ビュー、行または列を選択する列ヘッダーなどをドラッグして列を再配置でサポートされているさまざまな操作は、アウトライン表示でもサポートされます。 Xamarin.Mac アプリケーションは、これらの機能のコントロールを持ち、特定の操作を許可または拒否する (コードまたはインターフェイスのビルダーにいずれか)、アウトライン表示のパラメーターを構成することができます。

アウトライン表示は、独自のデータを格納していない、代わりに、データ ソースに依存しています (`NSOutlineViewDataSource`) を行と、必要に応じてごとに、必要な列の両方を提供します。

アウトライン表示デリゲートのサブクラスを提供することにより、アウトライン表示の動作をカスタマイズすることができます (`NSOutlineViewDelegate`) アウトラインの列の管理をサポートする機能、行の選択と編集、カスタムの追跡、および個々 のカスタム ビューを選択する種類列と行です。

アウトライン表示では、テーブル ビューでの動作と機能の多くを共有するためは、通過することができます、[テーブル ビュー](~/mac/user-interface/table-view.md)この記事の内容を続行する前にドキュメント。

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>作成し、Xcode でのアウトライン ビューの管理

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイル。

[![](outline-view-images/edit01.png "メインのストーリー ボードを選択します。")](outline-view-images/edit01.png#lightbox)

Xcode のインターフェイスのビルダーで、ウィンドウのデザインが開きます。

[![](outline-view-images/edit02.png "Xcode で UI を編集")](outline-view-images/edit02.png#lightbox)

型`outline`に、**ライブラリ インスペクター**をアウトライン表示コントロールを検索するが容易に検索ボックス。

[![](outline-view-images/edit03.png "ライブラリから、アウトライン モードを選択します。")](outline-view-images/edit03.png#lightbox)

ビュー コント ローラー上にアウトライン表示をドラッグして、**インターフェイス エディター**、ビュー コント ローラーのコンテンツ領域を入力し、縮小して、ウィンドウにも拡張に設定して、**制約エディター**:

[![](outline-view-images/edit04.png "制約の編集")](outline-view-images/edit04.png#lightbox)

アウトライン ビューを選択して、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](outline-view-images/edit05.png "属性の検査")](outline-view-images/edit05.png#lightbox)

- **列をアウトライン**-、テーブルの列の階層データが表示されます。
- **自動アウトライン列**場合 -`true`アウトライン列が自動的に保存し、アプリケーションの実行時の間で復元します。
- **インデント**-展開されているアイテムの下の列をインデントする量。
- **インデントに依存してセル**場合 -`true`インデントは、マークは、セルとインデントされます。
- **展開されたアイテムを自動保存**場合 -`true`項目の展開/折りたたまれている状態は自動的に保存されアプリケーションの実行時の間で復元します。
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
> 従来の Xamarin.Mac アプリケーションを保持している場合を除き、`NSView`ベースのアウトライン ビューを経由で使用する必要があります`NSCell`ベース テーブルのビューです。 `NSCell` レガシと見なされ、今後はサポートされません。

テーブルの列を選択して、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](outline-view-images/edit06.png "属性の検査")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "属性の検査")](outline-view-images/edit07.png#lightbox)

これらは、すべての標準的なビューのプロパティです。 また、この列は、ここに行サイズの変更オプションがあります。

テーブル ビューのセルを選択 (既定では、これは、 `NSTextField`) で、**インターフェイス階層**あり、次のプロパティでは使用、**属性インスペクター**:

[![](outline-view-images/edit08.png "属性の検査")](outline-view-images/edit08.png#lightbox)

ここで設定する標準的なテキスト フィールドのすべてのプロパティがあります。 既定では、標準的なテキスト フィールドは列のセルのデータを表示するために使用します。

テーブル セルのビューを選択 (`NSTableFieldCell`) で、**インターフェイス階層**、次のプロパティがで利用できる、**属性インスペクター**:

[![](outline-view-images/edit09.png "属性の検査")](outline-view-images/edit09.png#lightbox)

ここでの最も重要な設定は次のとおりです。

- **レイアウト**- この列のセルのレイアウト方法を選択します。
- **単一行モードを使用して**場合 -`true`セルが 1 行に制限されます。
- **最初のランタイム レイアウト**場合 -`true`セルの幅を設定 (手動または自動的に) とを選びます初めてアプリケーションを実行して表示されます。
- **アクション**のタイミングを制御、編集**アクション**セルに送信します。
- **動作**のセルが選択可能または編集可能な場合を定義します。
- **リッチ テキスト**場合 -`true`セルは、書式設定およびスタイル設定されたテキストを表示できます。
- **元に戻す**場合 - `true`、に対しては、セルが責任を引き継ぎます動作を元に戻します。

テーブル セルのビューを選択 (`NSTableFieldCell`) のテーブルの列の下部にある、**インターフェイス階層**:

[![](outline-view-images/edit11.png "テーブル セルのビューを選択します。")](outline-view-images/edit10.png#lightbox)

これにより、ベースとして使用されるテーブル セルのビューを編集して_パターン_指定された列用に作成されたすべてのセル。

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>アクションとコンセントを追加します。

だけ Cocoa UI コントロールと同様に、アウトライン表示を公開する必要があり、その列は、c# コードを使用するセル**アクション**と**コンセント**(必要な機能に基づく)。

プロセスは、アウトライン表示要素を公開する必要の場合と同じです。

1. 切り替えて、**アシスタント エディター**ことを確認して、`ViewController.h`ファイルが選択されています。 

    [![](outline-view-images/edit11.png "正しい .h ファイルを選択します。")](outline-view-images/edit11.png#lightbox)
2. アウトライン表示を選択して、**インターフェイス階層**コントロールをクリックし、ドラッグする、`ViewController.h`ファイル。
3. 作成、**コンセント**、アウトライン表示と呼ばれる`ProductOutline`: 

    [![](outline-view-images/edit13.png "コンセントを構成します。")](outline-view-images/edit13.png#lightbox)
4. 作成**コンセント**テーブルの列にも呼び出さ`ProductColumn`と`DetailsColumn`: 

    [![](outline-view-images/edit14.png "コンセントを構成します。")](outline-view-images/edit14.png#lightbox)
5. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

次に、作成してコードの表示、アウトラインの一部のデータ、アプリケーションを実行するとします。

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>アウトライン表示を設定します。

アウトラインの私たちのインターフェイスのビルダーでデザインおよびを介して公開される、**コンセント**、次に読み込むための c# コードを作成する必要があります。

最初に、新しいを作成してみましょう`Product`個々 の行とサブ製品のグループに関する情報を保持するクラス。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`Product`の**名前**] をクリック、**新規**ボタン。

[![](outline-view-images/populate01.png "空のクラスを作成します。")](outline-view-images/populate01.png#lightbox)

ように、`Product.cs`次のようなファイルの内容。

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

次に、私たちのサブクラスを作成する必要があります`NSOutlineDataSource`が要求されるように、アウトラインのデータを提供します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`ProductOutlineDataSource`の**名前**] をクリック、**新規**ボタンをクリックします。

編集、`ProductTableDataSource.cs`ファイルし、次のようになります。

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

このクラスは、アウトライン表示の項目の記憶域を持つし、上書き、`GetChildrenCount`をテーブルの行の数を返します。 `GetChild` (ように、アウトライン表示が要求する) は、特定の親または子項目を返しますと`ItemExpandable`親または子のいずれかとして指定した項目を定義します。

最後に、私たちのサブクラスを作成する必要があります`NSOutlineDelegate`アウトラインの動作を提供します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし [**追加** > **新しいファイル.**選択**全般** > **空のクラス**、入力`ProductOutlineDelegate`の**名前**] をクリック、**新規**ボタンをクリックします。

編集、`ProductOutlineDelegate.cs`ファイルし、次のようになります。

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

インスタンスを作成するとき、 `ProductOutlineDelegate`、私たちのインスタンスに渡すことも、`ProductOutlineDataSource`アウトラインのデータを提供します。 `GetView`メソッドは、指定した列と行のセルを表示するビュー (データ) を返すことを担当します。 可能であれば、既存のビューは、セルを表示する再利用する以外の場合、新しいビューを作成する必要があります。

アウトラインを設定するを編集しましょう、`MainWindow.cs`ファイルし、`AwakeFromNib`次のようなメソッドの検索。

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

アプリケーションを実行している場合、次のように表示されます。

[![](outline-view-images/populate02.png "折りたたまれたビュー")](outline-view-images/populate02.png#lightbox)

アウトライン表示でノードを展開している場合、次のようになります。

[![](outline-view-images/populate03.png "展開ビュー")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>列による並べ替え

ユーザーを列ヘッダーをクリックして、アウトライン内のデータを並べ替えるを許可してみましょう。 最初をダブルクリックして、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 選択、`Product`列、入力`Title`の**並べ替えキー**、`compare:`の**セレクター**を選択し、`Ascending`の**順序**:

[![](outline-view-images/sort01.png "並べ替えキーの順序の設定")](outline-view-images/sort01.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ編集しましょう、`ProductOutlineDataSource.cs`ファイルし、次のメソッドを追加します。

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

`Sort`メソッドにより、データに基づくデータ ソースの並べ替え、指定された`Product`昇順または降順のどちらかのクラス フィールドです。 オーバーライドされた`SortDescriptorsChanged`を使用する列見出しをクリックするたびにメソッドが呼び出されます。 渡されます、**キー**インターフェイス ビルダーとその列の並べ替え順序で設定した値がします。

アプリケーションを実行すると、列ヘッダーをクリックして、その列で行が並べ替えられます。

[![](outline-view-images/sort02.png "並べ替えの出力の例")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>行の選択

1 つの行を選択し、ダブルクリックして、ユーザーを許可するかどうか、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 アウトライン ビューを選択して、**インターフェイス階層**をオフにし、**複数**のチェック ボックス、**属性インスペクター**:

[![](outline-view-images/select01.png "属性の検査")](outline-view-images/select01.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。


次に、編集、`ProductOutlineDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

これによって、ユーザーをアウトライン表示で、1 つの行を選択します。 返す`false`の`ShouldSelectItem`のいずれかの項目を選択できるユーザーをたくないまたは`false`任意の項目を選択できるユーザーをたくない場合は、各項目にします。

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>複数行の選択

複数の行を選択し、ダブルクリックして、ユーザーを許可するかどうか、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 アウトライン ビューを選択して、**インターフェイス階層**を確認し、**複数**のチェック ボックス、**属性インスペクター**:

[![](outline-view-images/select02.png "属性の検査")](outline-view-images/select02.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。


次に、編集、`ProductOutlineDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

これによって、ユーザーをアウトライン表示で、1 つの行を選択します。 返す`false`の`ShouldSelectRow`のいずれかの項目を選択できるユーザーをたくないまたは`false`任意の項目を選択できるユーザーをたくない場合は、各項目にします。

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>行を選択する種類

その文字を含む、アウトライン モードを選択した文字を入力するユーザーを許可して、最初の行を選択する場合、ダブルクリック、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 アウトライン ビューを選択して、**インターフェイス階層**を確認し、**の種類を選択**のチェック ボックス、**属性インスペクター**:

[![](outline-view-images/type01.png "行型の編集")](outline-view-images/type01.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ編集しましょう、`ProductOutlineDelegate.cs`ファイルし、次のメソッドを追加します。

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

`GetNextTypeSelectMatch`メソッドは、指定された`searchString`最初の項目を返しますと`Product`にその文字列を含む`Title`です。

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>列の並べ替え

ユーザーがドラッグできるようにする場合、アウトライン表示の列の順序を変更をダブルクリックして、`Main.storyboard`編集のためインターフェイス ビルダーで開くファイル。 アウトライン ビューを選択して、**インターフェイス階層**を確認し、**並べ替え**のチェック ボックス、**属性インスペクター**:

[![](outline-view-images/reorder01.png "属性の検査")](outline-view-images/reorder01.png#lightbox)

値を提供する場合、**自動保存**プロパティとチェック、**列情報**フィールドでは、テーブルのレイアウトを変更しましたがご利用の米国自動的に保存され、次回アプリケーションを復元実行されます。

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ編集しましょう、`ProductOutlineDelegate.cs`ファイルし、次のメソッドを追加します。

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder`メソッドが返す`true`ドラッグに並べ替えることを許可する任意の列の`newColumnIndex`、それ以外の場合の戻り値`false`;

アプリケーションを実行する場合に、列の順序を変更する周囲の列ヘッダーをドラッグしたことができます。

[![](outline-view-images/reorder02.png "並べ替え列の例")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>セルの編集

ユーザーが指定されたセルの値を編集、編集を許可する場合、`ProductOutlineDelegate.cs`ファイルし、変更、`GetViewForItem`メソッドを次のようにします。

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

今すぐアプリケーションを実行して、ユーザーはテーブル ビュー内のセルを編集できます。

[![](outline-view-images/editing01.png "セルの編集の例")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>アウトライン ビューでのイメージの使用

内のセルの一部として画像を含める、 `NSOutlineView`、アウトライン表示のによってデータが返される方法を変更する必要があります`NSTableViewDelegate's``GetView`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例:

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

詳細についてを参照してください、[アウトライン ビューでのイメージを使用して](~/mac/app-fundamentals/image.md)のセクションで、[画像](~/mac/app-fundamentals/image.md)ドキュメント。

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>データ バインディングのアウトライン ビュー

Xamarin.Mac アプリケーションのコーディングのキー値と、データ バインドの手法を使用するの記述し、保守を作成し、UI 要素を使用する必要のあるコードの量を大幅に削減できます。 また、バックアップ データをさらに分離することの利点がある場合 (_データ モデル_)、正面からのユーザー インターフェイスを終了 (_モデル-ビュー-コント ローラー_) より柔軟なアプリケーションの保守が容易に先行します。デザインします。

キーと値のコーディング (KVC) は、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用して、オブジェクトのプロパティを直接アクセスするための機構 (`get/set`)。 Xamarin.Mac アプリの準拠のアクセサーをキーと値のコーディングを実装すると、キーと値を観察し (KVO)、データ バインディング、基本データ、Cocoa バインディング、および scriptability などその他の macOS 機能へのアクセスを取得します。

詳細についてを参照してください、[ビューのデータ バインディングのアウトライン](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding)のセクションで、[データ バインディングと、キーと値のコーディング](~/mac/app-fundamentals/databinding.md)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの操作についての詳細を取得しました。 さまざまな種類を説明しましたし、アウトライン ビュー、作成し、Xcode のインターフェイスのビルダーでのアウトライン ビューを管理する方法および c# コードのアウトライン ビューで作業する方法を使用します。

## <a name="related-links"></a>関連リンク

- [MacOutlines (サンプル)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [ソース リスト](~/mac/user-interface/source-list.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [ビューを説明する概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
