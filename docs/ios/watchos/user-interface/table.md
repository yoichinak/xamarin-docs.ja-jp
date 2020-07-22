---
title: Xamarin の watchOS Table コントロール
description: このドキュメントでは、Xamarin で watchOS table コントロールを使用する方法について説明します。 テーブルの追加、行コントローラーの追加、行の作成と設定、タップへの応答などについて説明します。
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 015f0732e4d8cdf771af3e1d0b3cc3e31b6e806c
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572264"
---
# <a name="watchos-table-controls-in-xamarin"></a>Xamarin の watchOS Table コントロール

WatchOS `WKInterfaceTable` コントロールは、対応する iOS よりもはるかに簡単ですが、同様の役割を果たします。 カスタムレイアウトを持つことができ、タッチイベントに応答する行のスクロールリストを作成します。

![](table-images/table-list-sml.png "ウォッチテーブルの一覧") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>テーブルの追加

**テーブル**コントロールをシーンにドラッグします。 既定では、次のようになります (1 つの指定されていない行のレイアウトを示します)。

[![](table-images/add-table-sml.png "Adding a table")](table-images/add-table.png#lightbox)

コードで参照できるように、[**プロパティ**] パッドの [**名前**] ボックスにテーブルの名前を付けます。

## <a name="add-a-row-controller"></a>行コントローラーを追加する

このテーブルには、既定で**グループ**コントロールを含む行コントローラーによって表される1つの行が自動的に含まれます。

行コントローラーの**クラス**を設定するには、**ドキュメントアウトライン**で行を選択し、[**プロパティ**] パッドでクラス名を入力します。

[![](table-images/add-row-controller-sml.png "Entering a class name in the Properties pad")](table-images/add-row-controller.png#lightbox)

行のコントローラーのクラスが設定されると、IDE によって、対応する C# ファイルがプロジェクトに作成されます。 コントロール (ラベルなど) を行にドラッグし、コードで参照できるように名前を付けます。

## <a name="create-and-populate-rows"></a>行の作成と設定

`SetNumberOfRows`を使用して正しい列を選択することにより、各行の行コントローラークラスを作成し `Identifier` ます。 行コントローラーにカスタムを設定した場合は `Identifier` 、次のコードスニペットの**既定値**を、使用した識別子に変更します。 が呼び出され、テーブルが表示されると、 `RowController` *すべての行に対し*てが作成され `SetNumberOfRows` ます。

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
    // loads row controller by identifier
```

> [!IMPORTANT]
> テーブル行は、iOS でのようには仮想化されません。 行の数を制限します (Apple では20未満にすることをお勧めします)。

行が作成されたら、各セル (iOS の場合と同様) を設定する必要があり `GetCell` ます。 [WatchTables の例](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)のこのコードスニペットでは、各行のラベルが更新されます。

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> を使用し、を使用してループすると、 `SetNumberOfRows` `GetRowController` テーブル全体がウォッチに送信されます。 テーブルの後続のビューでは、特定の行を追加または削除する必要がある場合は `InsertRowsAt` 、とを使用して `RemoveRowsAt` パフォーマンスを向上させます。

## <a name="respond-to-taps"></a>タップに応答する

行の選択に応答するには、次の2つの方法があります。

- `DidSelectRow`インターフェイスコントローラーに対してメソッドを実装します。
- ストーリーボードにセグエを作成し、 `GetContextForSegue` 行の選択で別のシーンを開く場合はを実装します。

### <a name="didselectrow"></a>DidSelectRow

行の選択をプログラムで処理するには、メソッドを実装し `DidSelectRow` ます。 新しいシーンを開くには、を使用して、 `PushController` シーンの識別子と、使用するデータコンテキストを渡します。

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

ストーリーボード上のセグエを、テーブルの行から別のシーンにドラッグします (ドラッグ中は**ctrl**キーを押したままにします)。
セグエを選択し、[**プロパティ**] パッドで識別子を指定します (以下の `secondLevel` 例を参照)。

インターフェイスコントローラーで、メソッドを実装 `GetContextForSegue` し、セグエによって示されるシーンに提供されるデータコンテキストを返します。

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

このデータは、メソッド内のターゲットストーリーボードシーンに渡され `Awake` ます。

## <a name="multiple-row-types"></a>複数の行の種類

既定では、テーブルコントロールには、デザインできる単一行の型があります。 行 ' templates ' を追加するには、**プロパティ**パッドの [**行**] ボックスを使用して、さらに行コントローラーを作成します。

![](table-images/prototype-rows1.png "Setting the number of Prototype rows")

**Rows**プロパティを**3**に設定すると、コントロールをにドラッグするための追加の行プレースホルダーが作成されます。 行ごとに、**プロパティ**パッドで**クラス**名を設定して、行コントローラークラスが作成されるようにします。

![](table-images/prototype-rows2.png "The prototype rows in the designer")

異なる行の種類を含むテーブルにデータを読み込むには、メソッドを使用して、テーブルの各行 `SetRowTypes` に使用する行コントローラーの種類を指定します。 行の識別子を使用して、行ごとに使用する行コントローラーを指定します。

この配列内の要素の数は、テーブルに含まれると予想される行数と一致している必要があります。

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

複数の行コントローラーを含むテーブルにデータを読み込む場合は、UI を設定するときに予期していた型を追跡する必要があります。

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

## <a name="vertical-detail-paging"></a>垂直方向の詳細ページング

watchOS 3 ではテーブルの新機能が導入されました。テーブルに戻って別の行を選択しなくても、各行に関連する詳細ページをスクロールできます。 詳細画面をスクロールするには、上下にスワイプするか、Digital Crown を使用します。

![](table-images/table-scroll-sml.png "垂直方向の詳細ページングの例") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> この機能は現在、Xcode Interface Builder でストーリーボードを編集することによってのみ使用できます。

この機能を有効にするには、 `WKInterfaceTable` デザイン画面でを選択し、**垂直方向の詳細ページング**オプションをオンにします。

![](table-images/vertical-detail-paging-sml.png "Selecting the Vertical Detail Paging option")

[Apple によって説明](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023)されているように、テーブルナビゲーションでは、ページング機能が動作するためにセグエを使用する必要があります。 を使用する既存のコードを再作成 `PushController` し、代わりにセグエを使用します。

<a name="add_row_controller"></a>

## <a name="appendix-row-controller-code-example"></a>付録: 行コントローラーのコード例

デザイナーで行コントローラーを作成すると、IDE によって自動的に2つのコードファイルが作成されます。 これらの生成されたファイルのコードを次に示します。

最初のはクラスの名前が付けられます。たとえば、 **RowController.cs**のようになります。

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

**Designer.cs**ファイルは部分クラス定義で、この例では1つのコントロールを使用して、デザイナー画面に作成されるコンセントとアクションを含み `WKInterfaceLabel` ます。

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

ここで宣言されているコンセントとアクションは、コードで参照できます。ただし、 **designer.cs**ファイルは直接編集しないでください。

## <a name="related-links"></a>関連リンク

- [WatchTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のテーブルドキュメント](https://developer.apple.com/reference/watchkit/wkinterfacetable)
