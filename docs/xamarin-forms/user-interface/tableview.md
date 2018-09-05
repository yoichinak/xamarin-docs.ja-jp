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

[テーブル](xref:Xamarin.Forms.TableView)データやスクロール可能な選択肢の一覧を表示するビューに同じテンプレートを共有しない行がある場合 [ListView](~/xamarin-forms/user-interface/listview/index.md)と異なり、テーブルには`ItemsSource`の概念が無いため、項目を子として手動で追加する必要があります。

このガイドでは、次のセクションで構成されます。

- **[ユース ケース](#Use_Cases)** &ndash; ListView やカスタム ビューではなくテーブルを使用する場合。
- **[テーブル構造](#TableView_Structure)** &ndash;テーブル内で必要なビューの階層。
- **[テーブルの外観](#TableView_Appearance)** &ndash;テーブルのカスタマイズ オプション。
- **[組み込みのセル](#Built-In_Cells)** &ndash;[EntryCell](#EntryCell)と[SwitchCell](#SwitchCell)を含む組み込みのセルオプション。
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

`TableView`の要素はセクションで構成されています。`TableView`のルートは`TableRoot`であり、1つ以上の`TableSections`の親になります。

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

各`TableSection`は見出しと1つ以上の`ViewCells`で構成されます。 ここでは、`TableSection`の`Title`プロパティにコンストラクターで *「Ring」* が設定されています。

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

上記と同じレイアウトを XAML で行います。

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

`TableView`は`Intent`プロパティを公開しており、次のオプションの列挙値です。

- **データ**&ndash;データ エントリを表示するときに使用します。 なお[ListView](~/xamarin-forms/user-interface/listview/index.md)データのリストをスクロールするための優れた選択肢があります。
- **フォーム**&ndash;形式として、テーブルが動作しているときに使用します。
- **メニュー** &ndash;選択項目のメニューを表示するときに使用します。
- **設定**&ndash;構成設定の一覧を表示するときに使用します。

選択した`TableIntent`は、`TableView`の各プラットフォーム毎の表示に影響を与える可能性があります。 明確な違いがない場合でも、テーブルの使用方法に最も近い`TableIntent`を選択することをお勧めします。

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>組み込みのセル

Xamarin.Forms を収集して情報を表示するための組み込みのセルが付属します。 ListView とテーブルは、すべての同じセルで使用できますが、次に、テーブルのシナリオに最も関連性。

- **SwitchCell** &ndash;を提示すると、テキスト ラベルと共に、true または false の状態をキャプチャします。
- **EntryCell** &ndash;表示とテキストをキャプチャします。

[TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell)と[ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)の詳細な説明は、[ListView セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)を参照してください。

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)は、`on` / `off`または`true` / `false`の状態を表示し、キャプチャするために使用するコントロールです。

SwitchCells には、1行のテキストと on / off のプロパティがあります。 これらのプロパティの両方がバインド可能です。

- `Text` &ndash; スイッチの横に表示するテキスト。
- `On` &ndash; on または off として表示されるスイッチ。

なお、`SwitchCell`は`OnChanged`イベントを公開しているため、セルの状態変更に応答することができます。

![](tableview-images/switch-cell.png "SwitchCell 例")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) ユーザーが編集できるテキスト データを表示する必要がある場合に役立ちます。 `EntryCell`にはカスタマイズ可能な次のプロパティがあります。

- `Keyboard` &ndash; 編集中に表示するキーボード。 数値、電子メール、電話番号などのオプションがあります。[API ドキュメントを参照してください。](xref:Xamarin.Forms.Keyboard)	
- `Label` &ndash; テキスト入力フィールドの右側に表示するラベル テキスト。
- `LabelColor` &ndash; ラベルのテキストの色。
- `Placeholder` &ndash; Null または空のときに、入力フィールドに表示するテキスト。 このテキストは、テキスト エントリの開始時に表示されなくなります。
- `Text` &ndash; エントリ フィールド内のテキスト。
- `HorizontalTextAlignment` &ndash; テキストの水平方向の配置。 センター、左、または右に配置できます。 [API ドキュメントを参照してください。](xref:Xamarin.Forms.TextAlignment)

なお`EntryCell`は`Completed`イベントを公開しており、ユーザーがテキストの編集中にキーボードの 'done' を押すと発生します。

![](tableview-images/entry-cell.png "EntryCell 例")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>カスタムのセル
組み込みのセルが十分でない場合、カスタムのセルを使用することで、アプリに合った方法でデータを表示しキャプチャすることができます。 たとえば、画像の不透明度を選択するようにする場合、スライダーを提示したい場合があります。

全てのカスタムセルは、すべての組み込みのセルが使用する基本クラスである[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)から派生する必要があります。

これは、カスタムのセルの例を示します。

![](tableview-images/custom-cell.png "カスタムのセルの例")

### <a name="xaml"></a>XAML
上記のレイアウトを作成する XAML を次に示します。

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

上記の XAML は多くのことを行っています。 分割してみましょう。

- `TableView`のルート要素は、`TableRoot`です。
- `TableRoot`のすぐ下に`TableSection`があります。
- `ViewCell`は`TableSection`の直下で定義されています。 `ListView`とは異なり、 `TableView`ではカスタムセル (またはそのいずれか) が`ItemTemplate`で定義されている必要はありません。
- StackLayout を使用して、カスタムのセルのレイアウトを管理できます。 任意のレイアウトは、ここで使用できます。

### <a name="cnum"></a>C&num;

`TableView`は静的データ、または手動で変更されたデータのため、アイテムテンプレートの概念はありません。 代わりに、カスタムセルを手動で作成でき、テーブルに挿入します。 `ViewCell`から継承するカスタムセルを作成し、組み込みセルと同様に`TableView`に追加する方法もサポートされています。
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

上記 C# は多くのことを行っています。 分割してみましょう。

- `TableView`のルート要素は、`TableRoot`です。
- `TableRoot`のすぐ下に`TableSection`があります。
- `ViewCell`は`TableSection`の直下で定義されています。 `ListView`とは異なり、 `TableView`ではカスタムセル (またはそのいずれか) が`ItemTemplate`で定義されている必要はありません。
- StackLayout を使用して、カスタムのセルのレイアウトを管理できます。 任意のレイアウトは、ここで使用できます。

カスタムのセルのクラスが定義されていないことに注意してください。 代わりに、`ViewCell`のビューのプロパティは、`ViewCell`の特定のインスタンスに対して設定されています。



## <a name="related-links"></a>関連リンク

- [テーブル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
