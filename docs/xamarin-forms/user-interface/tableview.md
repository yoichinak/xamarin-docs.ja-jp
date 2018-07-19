---
title: Xamarin.Forms テーブル
description: この記事では、Xamarin.Forms テーブル クラスを使用して、アプリケーションでスクロール メニューの設定、および入力フォームを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 47cd79611cfeaf48c0422772d8f3e75eb57ba771
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996054"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms テーブル

[テーブル](xref:Xamarin.Forms.TableView)データや選択肢のスクロール可能な一覧を表示するビューを同じテンプレートを共有しない行がある場合。 異なり[ListView](~/xamarin-forms/user-interface/listview/index.md)、テーブルには、概念はありません、`ItemsSource`ので、項目を子として手動で追加する必要があります。

このガイドでは、次のセクションで構成されます。

- **[ユース ケース](#Use_Cases)** &ndash; ListView やカスタム ビューではなくテーブルを使用する場合。
- **[テーブル構造](#TableView_Structure)** &ndash;テーブル内で必要なビューの階層。
- **[テーブルの外観](#TableView_Appearance)** &ndash;テーブルのカスタマイズ オプション。
- **[組み込みのセル](#Built-In_Cells)** &ndash;など、組み込みのセル オプション[EntryCell](#EntryCell)と[SwitchCell](#SwitchCell)します。
- **[カスタム セル](#Custom_Cells)** &ndash;独自のカスタムのセルを作成する方法。

![](tableview-images/tableview-all-sml.png "テーブルの例")

<a name="Use_Cases" />

## <a name="use-cases"></a>ユース ケース

テーブルは、次のような場合に役立ちます。

- 設定の一覧を表示します。
- フォームでは、データの収集または
- 行の行 (数、パーセンテージとイメージなど) に異なる方法で表示されているデータを表示しています。

テーブルは、スクロールし、魅力的なセクションでは、上記のシナリオの一般的な必要性内の行のレイアウトを処理します。 `TableView`コントロールが使用する各プラットフォームの使用可能な各プラットフォームのネイティブの外観を作成する場合、同等のビューを基になります。

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>テーブルの構造体

内の要素を`TableView`セクションに分類されます。 ルートにある、`TableView`は、 `TableRoot`、1 つまたは複数の親である`TableSections`:

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

上記と同じ XAML で同じレイアウトを行います。

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

テーブルを公開、`Intent`プロパティで、次のオプションの列挙値です。

- **データ**&ndash;データ エントリを表示するときに使用します。 なお[ListView](~/xamarin-forms/user-interface/listview/index.md)データのリストをスクロールするための優れた選択肢があります。
- **フォーム**&ndash;形式として、テーブルが動作しているときに使用します。
- **メニュー** &ndash;選択項目のメニューを表示するときに使用します。
- **設定**&ndash;構成設定の一覧を表示するときに使用します。

`TableIntent`影響を与える可能性を選択する方法、`TableView`プラットフォームごとに表示されます。 選択するベスト プラクティスはオフの相違点がありますが、場合でも、`TableIntent`に最も近いテーブルを使用する方法。

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>組み込みのセル

Xamarin.Forms を収集して情報を表示するための組み込みのセルが付属します。 ListView とテーブルは、すべての同じセルで使用できますが、次に、テーブルのシナリオに最も関連性。

- **SwitchCell** &ndash;を提示すると、テキスト ラベルと共に、true または false の状態をキャプチャします。
- **EntryCell** &ndash;表示とテキストをキャプチャします。

参照してください[ListView セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)の詳細については[TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell)と[ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)します。

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) コントロールを表示およびキャプチャを使用するには、オン/オフまたは`true` / `false`状態。

SwitchCells には、テキストを編集して、オン/オフ プロパティの 1 つの行があります。 これらのプロパティの両方がバインド可能です。

- `Text` &ndash; スイッチの横に表示するテキスト。
- `On` &ndash; かどうかと on または off スイッチが表示されます。

なお、`SwitchCell`公開、`OnChanged`イベント、セルの状態の変更に応答することができます。

![](tableview-images/switch-cell.png "SwitchCell 例")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) ユーザーが編集できるテキスト データを表示する必要がある場合に役立ちます。 `EntryCell`s、カスタマイズ可能な次のプロパティがあります。

- `Keyboard` &ndash; 編集中に表示するキーボード。 数値、電子メール、電話番号などのようなもののオプションがあります。[API ドキュメントを参照してください。](xref:Xamarin.Forms.Keyboard)します。
- `Label` &ndash; テキスト入力フィールドの右側に表示するラベル テキスト。
- `LabelColor` &ndash; ラベルのテキストの色。
- `Placeholder` &ndash; Null または空のときに、入力フィールドに表示するテキスト。 このテキストは、テキスト エントリの開始時に表示されなくなります。
- `Text` &ndash; エントリ フィールド内のテキスト。
- `HorizontalTextAlignment` &ndash; テキストの水平方向の配置。 センター、左、または適切な配置できます。 [API ドキュメントを参照してください。](xref:Xamarin.Forms.TextAlignment)します。

なお`EntryCell`公開、`Completed`ユーザーが達したとき 'done' キーボードのテキストの編集中に発生するイベントです。

![](tableview-images/entry-cell.png "EntryCell 例")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>カスタムのセル
組み込みのセルは十分でなく、ときに、存在し、アプリにとって合理的な方法でデータをキャプチャするカスタムのセルを使用できます。 たとえば、画像の不透明度を選択するようにする場合、スライダーを提示したい場合があります。

カスタムのすべてのセルがから派生する必要があります[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)、組み込みのセルのすべての型を使用する同じ基本クラス。

これは、カスタムのセルの例を示します。

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

上記の XAML はよく行っています。 分割してみましょう。

- ルート要素の下、`TableView`は、`TableRoot`します。
- `TableSection`すぐ下、`TableRoot`します。
- `ViewCell`テーブル セクションですで直接定義されています。 異なり`ListView`、 `TableView` (またはそのいずれか) をそのカスタムは不要でセルが定義されている、`ItemTemplate`します。
- StackLayout を使用して、カスタムのセルのレイアウトを管理できます。 任意のレイアウトは、ここで使用できます。

### <a name="cnum"></a>C&num;

`TableView`は静的データ、または手動で変更されたデータでがない項目テンプレートの概念です。 代わりに、カスタム セル手動で作成でき、テーブルに挿入します。 カスタムを作成する方法はセルを注が継承`ViewCell`に追加し、`TableView`するように、組み込みのセルもサポートされています。
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

上記 C# ではな処理を実行します。 分割してみましょう。

- ルート要素の下、`TableView`は、`TableRoot`します。
- `TableSection`すぐ下、`TableRoot`します。
- `ViewCell`テーブル セクションですで直接定義されています。 異なり`ListView`、 `TableView` (またはそのいずれか) をそのカスタムは不要でセルが定義されている、`ItemTemplate`します。
- StackLayout を使用して、カスタムのセルのレイアウトを管理できます。 任意のレイアウトは、ここで使用できます。

カスタムのセルのクラスが定義されていないことに注意してください。 代わりに、`ViewCell`のビューのプロパティの特定のインスタンスに対して設定されて`ViewCell`します。



## <a name="related-links"></a>関連リンク

- [テーブル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
