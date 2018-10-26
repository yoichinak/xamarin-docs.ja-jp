---
title: watchOS Xamarin でのテーブル コントロール
description: このドキュメントでは、Xamarin で watchOS テーブル コントロールを使用する方法について説明します。 テーブルの追加、行コント ローラーの追加、作成および設定の詳細は、タップへの応答行がについて説明します。
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: cd5e7299874bbfb1b652315a549b9d067d58e9a0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109394"
---
# <a name="watchos-table-controls-in-xamarin"></a>watchOS Xamarin でのテーブル コントロール

WatchOS`WKInterfaceTable`コントロールは、iOS 対応よりもはるかに簡単ですが、同様の役割を実行します。 カスタムのレイアウトがあることができ、タッチ イベントに応答する行のスクロール リストが作成されます。

![](table-images/table-list-sml.png "ウォッチ リスト テーブル") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>テーブルの追加

ドラッグ、**テーブル**シーンにコントロール。 既定では、この (表示される 1 つ指定されていない行レイアウト) のように表示されます。

[![](table-images/add-table-sml.png "テーブルの追加")](table-images/add-table.png#lightbox)

テーブル、名前を付けて、**プロパティ**パッドの**名前**ボックスで、コード内に参照できるようにします。

## <a name="add-a-row-controller"></a>行コント ローラーを追加します。

テーブルを含む行コント ローラーによって表される、単一の行を自動的に含まれます、**グループ**既定で制御します。

設定する、**クラス**行コント ローラー内の行を選択します。、**ドキュメント アウトライン**でクラス名を入力し、**プロパティ**パッド。

[![](table-images/add-row-controller-sml.png "Properties pad でクラス名を入力します。")](table-images/add-row-controller.png#lightbox)

対応する IDE を作成、行のコント ローラーのクラスを設定すると、C#プロジェクト内のファイル。 (ラベルなど) のコントロールの行にドラッグし、コード内に参照できるように、名前を付けておく。




## <a name="create-and-populate-rows"></a>作成し、行を設定

`SetNumberOfRows` 各の行コント ローラー クラスを作成を使用して、行、`Identifier`をしいものを選択します。 カスタム行コント ローラーを指定した場合`Identifier`、変更**既定**で以下のコード スニペットを使用した id にします。 `RowController` *行ごとに*場合は、作成`SetNumberOfRows`が呼び出されますとテーブルが表示されます。

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> テーブルの行は、iOS のようになった仮想化されません。 (Apple は、20 未満を推奨) 行の数を制限してください。

行が作成されたら、各セルを設定する必要があります (など`GetCell`iOS での操作)。 次のコード スニペットから、 [WatchTables 例](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)行ごとにラベルを更新

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> 使用して`SetNumberOfRows`を使用してループと`GetRowController`によりテーブル全体のウォッチ ポイントに送信されます。 テーブルの後続のビューを追加または削除する必要がある場合に特定の行を使用して、`InsertRowsAt`と`RemoveRowsAt`パフォーマンスが向上します。


## <a name="respond-to-taps"></a>タップに応答します。

2 つの方法で、行の選択に応答することができます。

- 実装、`DidSelectRow`インターフェイス コント ローラーで、メソッド、または
- ストーリー ボード セグエを作成して実装`GetContextForSegue`する場合は、行の選択をもう 1 つのシーンを開きます。

### <a name="didselectrow"></a>DidSelectRow

行の選択をプログラムで処理するには、実装、`DidSelectRow`メソッド。 新しいシーンを開くには、次のように使用します。`PushController`シーンの識別子とを使用するデータ コンテキストを渡します。

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

もう 1 つのシーンにセグエをストーリー ボード上のテーブル行を列からドラッグ (を押しながら、**コントロール**ドラッグするときにキー)。
必ず、セグエを選択しで識別子を**プロパティ**パッド (など`secondLevel`次の例で)。

インターフェイス コント ローラーの実装、`GetContextForSegue`メソッドと、セグエによって提示されるシーンに戻るデータ コンテキストを提供する必要があります。

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

このデータは、対象ストーリー ボードのシーンに渡されるその`Awake`メソッド。

## <a name="multiple-row-types"></a>複数の行の種類

既定では、テーブル コントロールは、デザインできる 1 つの行型を持ちます。 使用してをその他の行 'テンプレート' を追加する、**行**ボックスに、**プロパティ**パッドの複数の行コント ローラーを作成します。

![](table-images/prototype-rows1.png "プロトタイプの行の数を設定します。")

設定、**行**プロパティを**3**にコントロールをドラッグして追加の行のプレース ホルダーが作成されます。 行ごとに、設定、**クラス**内の名前、**プロパティ**パッドの行のコント ローラー クラスが作成されることを確認します。

![](table-images/prototype-rows2.png "デザイナー内のプロトタイプの行")

テーブルに別の行の種類の使用を設定する、`SetRowTypes`テーブル内の行ごとに使用する行コント ローラーの種類を指定します。 行の識別子を使用すると、各行の行コント ローラーを使用する必要がありますを指定します。

この配列の要素の数は、テーブル内に存在する行の数と一致する必要があります。

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

複数の行コント ローラーを持つテーブルを設定するときに、UI の値を設定するように期待するどの型を追跡する必要があります。

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

watchOS 3 には、テーブルの新しい機能が導入されました。 詳細ページをスクロールする機能がテーブルに戻り、別の行を選択することなく、行ごとに関連します。 詳細画面を上下にスワイプするか、デジタル クラウンを使用してスクロールできます。

![](table-images/table-scroll-sml.png "垂直方向のページングの詳細の例") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> この機能は現在のみ使用可能な Xcode の Interface Builder のストーリー ボードを編集します。

この機能を有効にするのには、選択、`WKInterfaceTable`ティック、デザイン画面で、**垂直詳細ページング**オプション。

![](table-images/vertical-detail-paging-sml.png "垂直方向の詳細のページング オプションを選択します。")

として[Apple によって説明](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023)テーブル ナビゲーションを使用する必要がありますセグエ ページング機能が動作します。 使用する既存のコードを再書き込み`PushController`代わりにセグエを使用します。

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>付録: 行コント ローラーのコード例

IDE では、行のコント ローラーは、デザイナーで作成されたときに、2 つのコード ファイルが自動的に作成します。 これらの生成されたファイル内のコードは、参照を以下に示します。

最初の例、クラスの名前は**RowController.cs**、次のようにします。

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

他の **. designer.cs**ファイルを含む、outlet と action いずれかで次の例など、デザイナー画面上に作成される部分クラス定義は、`WKInterfaceLabel`コントロール。

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

Outlet と action がここで宣言し、コード - で参照する、ただし **. designer.cs**ファイルを直接編集しない必要があります。



## <a name="related-links"></a>関連リンク

- [WatchTables (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のテーブルのドキュメント](https://developer.apple.com/reference/watchkit/wkinterfacetable)
