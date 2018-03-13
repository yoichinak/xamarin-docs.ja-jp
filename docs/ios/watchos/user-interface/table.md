---
title: "テーブル コントロール"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0b8d8d08db15959a47093f255a891605a089ea00
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="table-control"></a>テーブル コントロール

WatchOS`WKInterfaceTable`コントロールが iOS、対応するものよりもはるかに簡単にはのような役割を実行します。 カスタムのレイアウトを持つおよびタッチ イベントに応答する行のスクロール リストを作成します。

![](table-images/table-list-sml.png "ウォッチ リスト テーブル") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>テーブルを追加します。

ドラッグ、**テーブル**シーンを制御します。 既定ではこの (表示される 1 つ指定されていない行のレイアウト) のようになります。

[![](table-images/add-table-sml.png "テーブルを追加します。")](table-images/add-table.png#lightbox)

テーブルの名前、**プロパティ**パッドの**名前**ボックスで、コード内を参照するようにします。

## <a name="add-a-row-controller"></a>行のコント ローラーを追加します。

テーブルに自動的を含む行のコント ローラーによって表される、1 つの行が含まれています、**グループ**既定では制御します。

設定する、**クラス**行コント ローラー内の行を選択、 **ドキュメント アウトライン**にクラス名を入力し、**プロパティ**パッド。

[![](table-images/add-row-controller-sml.png "プロパティのパッドにクラス名を入力します。")](table-images/add-row-controller.png#lightbox)

行のコント ローラーのクラスを設定すると、IDE は、プロジェクトに対応する c# ファイルを作成します。 (ラベルなど) のコントロールの行にドラッグし、コード内に参照できるように、名前を付けておく。




## <a name="create-and-populate-rows"></a>行を作成し、

`SetNumberOfRows` コント ローラー クラスごとに行が作成行を使用して、`Identifier`しいものを選択します。 カスタム行コント ローラーを指定した場合`Identifier`、変更**既定**を使用する識別子の下のコード スニペット。 `RowController` *行ごとに*場合は、作成`SetNumberOfRows`が呼び出されたと表示されているテーブル。

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> **注**: テーブルの行が iOS のようになった仮想化できません。 (Apple の推奨 20 未満) の行の数を制限しようとしてください。
行が作成された後は、各セルを読み込む必要があります。 (のような`GetCell`iOS での操作と)。 このコード スニペットから、 [WatchTables 例](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)行ごとにラベルを更新

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> **注:**を使用する`SetNumberOfRows`を使用してループと`GetRowController`と、テーブル全体のウォッチ ポイントに送信されます。 テーブルの後続のビューを追加または削除する必要がある場合の特定の行を使用して`InsertRowsAt`と`RemoveRowsAt`パフォーマンスが向上します。


## <a name="respond-to-taps"></a>タップへの応答します。

2 つの異なる方法で行の選択に応答することができます。

- 実装、`DidSelectRow`インターフェイス コント ローラーで、メソッド、または
- ストーリー ボード上、segue を作成し、実装`GetContextForSegue`を開くには別のシーン行の選択をする場合。

### <a name="didselectrow"></a>DidSelectRow

行の選択をプログラムで処理するには、実装、`DidSelectRow`メソッドです。 新しいシーンを開くには、使用`PushController`シーンの識別子および使用するデータ コンテキストを渡します。

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

テーブルの行からストーリー ボード上、segue が別のシーンにドラッグ (を押しながら、**コントロール**ドラッグするときにキー)。
必ず、segue を選択しで識別子を**プロパティ**パッド (など`secondLevel`次の例で)。

インターフェイス コント ローラーの実装、`GetContextForSegue`メソッドと戻り値が提供するデータ コンテキスト、segue によって提示されるシーンにします。

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

このデータは、対象ストーリー ボードのシーンに渡される、`Awake`メソッドです。

## <a name="multiple-row-types"></a>複数行の種類

既定では、テーブル コントロールは、デザインできる単一行の種類を持ちます。 複数行テンプレートを使用して追加する、**行**ボックスに、**プロパティ**パッド以上の行のコント ローラーを作成します。

![](table-images/prototype-rows1.png "プロトタイプの行の数の設定")

設定、**行**プロパティを**3**にコントロールをドラッグして追加の行のプレース ホルダーが作成されます。 行ごとに、設定、**クラス**内の名前、**プロパティ**パッドの行のコント ローラー クラスが作成されることを確認します。

![](table-images/prototype-rows2.png "デザイナーでのプロトタイプ行")

別の行の種類の使用を含むテーブルを作成する、`SetRowTypes`テーブルの行ごとに使用する行のコント ローラーの種類を指定します。 行の識別子を使用すると、各行の行のコント ローラーを使用する必要がありますを指定します。

この配列内の要素の数は、テーブル内に存在する行の数と一致する必要があります。

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

複数の行のコント ローラーを含むテーブルを作成するときに、UI を設定すると予想される種類を追跡する必要があります。

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>垂直方向の詳細のページング

watchOS 3 には、テーブルの新しい機能が導入されました。 テーブルに戻り、別の行を選択することなく、行ごとに関連する詳細ページをスクロールする機能。 詳細画面を上下にスワイプするか、デジタル クラウンを使用してスクロールできます。

![](table-images/table-scroll-sml.png "垂直方向の詳細のページングの使用例") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> **警告:**この機能は現在のみ使用可能なインターフェイスのビルダーを Xcode でストーリー ボードを編集します。

この機能を有効にするには選択、`WKInterfaceTable`ティックとデザイン画面で、**垂直詳細ページング**オプション。

![](table-images/vertical-detail-paging-sml.png "垂直詳細ページング オプションを選択します。")

として[Apple によって説明](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023)テーブルのナビゲーションを使用する必要があります segues ページング機能を利用します。 使用する既存のコードを再書き込み`PushController`代わりに使用する segues です。

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>付録: コント ローラーのコード例

IDE では、行のコント ローラーがデザイナーで作成されたときに、2 つのコード ファイルが自動的に作成します。 これらの生成されたファイル内のコードは、参照を以下に示します。

最初のたとえば、クラスの名前は**RowController.cs**、次のようにします。

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

他の**. designer.cs**ファイル コンセントと 1 つでは、この例など、デザイナー画面上に作成されるアクションを含む部分クラス定義は、`WKInterfaceLabel`コントロール。

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

コンセントとアクションがここで宣言されているしで参照できますコード - ただし、 **. designer.cs**ファイルを直接編集しないでください。



## <a name="related-links"></a>関連リンク

- [WatchTables (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のテーブルのドキュメント](https://developer.apple.com/reference/watchkit/wkinterfacetable)
