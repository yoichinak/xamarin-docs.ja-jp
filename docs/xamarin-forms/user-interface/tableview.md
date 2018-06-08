---
title: TableView
description: スクロール メニューの設定、および入力フォームを表示します。
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 312472fdfae65bc62b76f4295a13760236dededc
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847660"
---
# <a name="tableview"></a>TableView

[テーブル](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)データや選択肢のスクロール可能なリストを表示するビューは、同じテンプレートを共有しない行がある場所です。 異なり[ListView](~/xamarin-forms/user-interface/listview/index.md)、テーブルには、概念はありません、`ItemsSource`のため、項目を子として手動で追加する必要があります。

このガイドは、次のセクションで構成されます。

- **[ユース ケース](#Use_Cases)** &ndash; ListView またはカスタム ビューではなくテーブルを使用する場合。
- **[テーブル構造](#TableView_Structure)** &ndash;テーブル内で必要なのビューの階層です。
- **[テーブルの外観](#TableView_Appearance)** &ndash;テーブルのカスタマイズのオプションです。
- **[組み込みのセル](#Built-In_Cells)** &ndash;など、組み込みのセル オプション[EntryCell](#EntryCell)と[SwitchCell](#SwitchCell)です。
- **[カスタムのセル](#Custom_Cells)** &ndash;独自のカスタムのセルを作成する方法です。

![](tableview-images/tableview-all-sml.png "テーブルの例")

<a name="Use_Cases" />

## <a name="use-cases"></a>ユース ケース

テーブルは、次のような場合に役立ちます。

- 設定の一覧を表示
- フォームでは、データを収集または
- 行から行 (番号、パーセンテージやイメージなど) に異なる方法で提供されるデータを表示しています。

テーブルは、スクロールし、魅力的なのセクションでは、上記のシナリオの一般的な必要性内の行のレイアウトを処理します。 `TableView`各プラットフォームの使用可能な各プラットフォームのネイティブの外観を作成する場合、同等のビューの基になるコントロールを使用します。

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>テーブルの構造体

内の要素、`TableView`セクションに分かれています。 ルートにある、`TableView`は、 `TableRoot`、1 つまたは複数の親である`TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

各`TableSection`見出しと 1 つまたは複数の ViewCells で構成されます。 ここで、`TableSection`の`Title`プロパティに設定 *「リング」* コンス トラクターで。

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

XAML 上記と同じで、同じレイアウトを実行します。

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
      <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>テーブルの外観

テーブルを公開、`Intent`プロパティで、次のオプションの列挙体です。

- **データ**&ndash;データ エントリを表示するときに使用します。 なお[ListView](~/xamarin-forms/user-interface/listview/index.md)データの一覧をスクロールするためのより良いオプションがあります。
- **フォーム**&ndash;形式として、テーブルが動作しているときに使用します。
- **メニュー** &ndash;の選択項目のメニューを表示するために使用します。
- **設定**&ndash;構成設定の一覧を表示するときに使用します。

`TableIntent`影響を与える可能性を選択する方法、`TableView`プラットフォームごとに表示されます。 選択することをお勧めはオフに違いがある場合でも、`TableIntent`を最も近いテーブルを使用する方法です。

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>組み込みのセル

Xamarin.Forms を収集および情報を表示するための組み込みのセルが付属します。 ListView コントロールとテーブルは、すべての同じセルで使用できますが、次に、テーブルのシナリオに最も関連性。

- **SwitchCell** &ndash;表示とテキスト ラベルと共に、true または false の状態をキャプチャします。
- **EntryCell** &ndash;表示とテキストのキャプチャします。

参照してください[ListView セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)の詳細な説明の[TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell)と[ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)です。

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) 表示およびキャプチャに使用するコントロールは、オン/オフまたは`true` / `false`状態です。

SwitchCells には、テキストを編集してオン/オフ プロパティの 1 行があります。 これらのプロパティの両方が、バインド可能です。

- `Text` &ndash; スイッチの横に表示するテキスト。
- `On` &ndash; かどうか、スイッチには、on または off が表示されます。

なお、`SwitchCell`公開、`OnChanged`イベント、セルの状態の変化に対応することができます。

![](tableview-images/switch-cell.png "SwitchCell 例")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) ユーザーが編集できるテキスト データを表示する必要がある場合に役立ちます。 `EntryCell`s は、カスタマイズ可能な次のプロパティを提供します。

- `Keyboard` &ndash; 編集中に表示するキーボード。 数値、電子メール、電話番号などのオプションがあります。[API のドキュメントを参照してください](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)です。
- `Label` &ndash; テキスト入力フィールドの右側に表示するラベルのテキスト。
- `LabelColor` &ndash; ラベルのテキストの色。
- `Placeholder` &ndash; Null または空である場合に、入力フィールドに表示するテキストです。 このテキストは、テキスト入力の開始時に表示されなくなります。
- `Text` &ndash; 入力フィールド内のテキスト。
- `HorizontalTextAlignment` &ndash; テキストの水平方向の配置。 センター、左、または右整列できます。 [API のドキュメントを参照してください](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/)です。

なお`EntryCell`公開、`Completed`到達した場合に、ユーザー 'done' キーボードのテキストの編集中に発生するイベントです。

![](tableview-images/entry-cell.png "EntryCell 例")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>カスタムのセル
組み込みセルすることで十分でない場合は、存在し、アプリに適した方法でデータをキャプチャするカスタムのセルを使用できます。 たとえばをスライダーを使用するイメージの不透明度を選択するユーザーを表示することがあります。

カスタムのすべてのセルから派生しなければなりません[ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)、組み込みのセルのすべての型を使用している同じ基本クラスです。

カスタムのセルの例を次に示します。

![](tableview-images/custom-cell.png "カスタムのセルの例")

### <a name="xaml"></a>XAML
上記のレイアウトを作成する XAML 次に示します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
        <TableView Intent="Settings">
            <TableRoot>
                <TableSection Title="Getting Started">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Image Source="bulb.png" />
                            <Label Text="left"
                              TextColor="#f35e20" />
                            <Label Text="right"
                              HorizontalOptions="EndAndExpand"
                              TextColor="#503026" />
                        </StackLayout>
                    </ViewCell>
                </TableSection>
            </TableRoot>
        </TableView>
    </ContentPage.Content>
</ContentPage>

```

上記の XAML はずっと行っています。 分割してみましょう。

- ルート要素の下、`TableView`は、`TableRoot`です。
- `TableSection`すぐ下、`TableRoot`です。
- `ViewCell`セクションですで直接定義されています。 異なり`ListView`、 `TableView` (またはそのいずれか) をそのカスタムが不要でセルが定義されている、`ItemTemplate`です。
- StackLayout を使用して、カスタムのセルのレイアウトを管理できます。 任意のレイアウトは、ここでは使用できません。

### <a name="cnum"></a>C&num;

`TableView`動作項目テンプレートの概念がない静的データ、または手動で変更されているデータでは、します。 代わりに、カスタムのセル手動で作成して、テーブルに格納します。 カスタム作成する方法はセルをメモが継承`ViewCell`に追加すること、`TableView`するように、組み込みのセルもサポートされています。
上記のレイアウトを実行するには、c# コードを次に示します。

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

C# の上を行ってずっとです。 分割してみましょう。

- ルート要素の下、`TableView`は、`TableRoot`です。
- `TableSection`すぐ下、`TableRoot`です。
- `ViewCell`セクションですで直接定義されています。 異なり`ListView`、 `TableView` (またはそのいずれか) をそのカスタムが不要でセルが定義されている、`ItemTemplate`です。
- StackLayout を使用して、カスタムのセルのレイアウトを管理できます。 任意のレイアウトは、ここでは使用できません。

カスタムのセルのクラスが定義されていないことに注意してください。 代わりに、`ViewCell`のビューのプロパティの特定のインスタンスに設定されて`ViewCell`です。



## <a name="related-links"></a>関連リンク

- [テーブル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
