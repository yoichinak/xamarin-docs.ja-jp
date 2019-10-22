---
title: TableView
description: この記事では、Xamarin TableView クラスを使用して、アプリケーションでのスクロールメニュー、設定、入力フォームを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: 67625aa413880023cce6d3e5e21e4d3bd0ec8e4c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695987"
---
# <a name="xamarinforms-tableview"></a>TableView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView)は、同じテンプレートを共有しない行がある場合に、スクロール可能なデータまたは選択肢の一覧を表示するためのビューです。 [ListView](~/xamarin-forms/user-interface/listview/index.md)とは異なり、`TableView` には `ItemsSource` の概念がないため、項目を子として手動で追加する必要があります。

![TableView の例](tableview-images/tableview-all-sml.png)

<a name="Use_Cases" />

## <a name="use-cases"></a>ユース ケース

[`TableView`](xref:Xamarin.Forms.TableView)は、次の場合に役立ちます。

- 設定の一覧を表示する
- フォームでのデータの収集
- 行と行 (数値、パーセンテージ、画像など) とは異なる方法で表示されるデータを表示します。

[`TableView`](xref:Xamarin.Forms.TableView)は、上のシナリオの一般的なニーズである、魅力的なセクションで行のスクロールとレイアウトを処理します。 @No__t_0 コントロールは、各プラットフォームの基になる同等のビューを使用して、各プラットフォームのネイティブな外観を作成します。

<a name="TableView_Structure" />

## <a name="structure"></a>構造体

[@No__t_1](xref:Xamarin.Forms.TableView)内の要素は、セクションに整理されています。 @No__t_0 のルートには、1つ以上の[`TableSection`](xref:Xamarin.Forms.TableSection)インスタンスの親である[`TableRoot`](xref:Xamarin.Forms.TableRoot)です。 各[`TableSection`](xref:Xamarin.Forms.TableSection)は、1つの見出しと1つ以上の[`ViewCell`](xref:Xamarin.Forms.ViewCell)インスタンスで構成されます。

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

これに相当する C# コードを次に示します。

```csharp
Content = new TableView
{
    Root = new TableRoot
    {
        new TableSection("Ring")
        {
          // TableSection constructor takes title as an optional parameter
          new SwitchCell { Text = "New Voice Mail" },
          new SwitchCell { Text = "New Mail", On = true }
        }
    },
    Intent = TableIntent.Settings
};
```

<a name="TableView_Appearance" />

## <a name="appearance"></a>外観

[`TableView`](xref:Xamarin.Forms.TableView)は[`Intent`](xref:Xamarin.Forms.TableView.Intent)プロパティを公開します。これは、 [`TableIntent`](xref:Xamarin.Forms.TableIntent)列挙型のメンバーに設定できます。

- `Data` –データエントリを表示するときに使用します。 [ListView](~/xamarin-forms/user-interface/listview/index.md)は、データのリストをスクロールする場合に、より適切なオプションであることに注意してください。
- `Form` – TableView がフォームとして機能する場合に使用します。
- `Menu` –選択項目のメニューを表示するときに使用します。
- `Settings` –構成設定の一覧を表示するときに使用します。

選択した[`TableIntent`](xref:Xamarin.Forms.TableIntent)値は、各プラットフォームで[`TableView`](xref:Xamarin.Forms.TableView)がどのように表示されるかに影響を与える可能性があります。 明確な違いがない場合でも、テーブルの使用方法に最も近い `TableIntent` を選択することをお勧めします。

また、各[`TableSection`](xref:Xamarin.Forms.TableSection)に表示されるテキストの色は、`TextColor` プロパティを[`Color`](xref:Xamarin.Forms.Color)に設定することによって変更できます。

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>組み込みセル

Xamarin. Forms には、情報を収集して表示するための組み込みセルが付属しています。 [@No__t_1](xref:Xamarin.Forms.ListView)と[`TableView`](xref:Xamarin.Forms.TableView)は同じセルのすべてを使用できますが、 [`SwitchCell`](xref:Xamarin.Forms.SwitchCell)と[`EntryCell`](xref:Xamarin.Forms.EntryCell)は `TableView` のシナリオに最も適しています。

[Textcell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell)と[ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)の詳細については、 [ListView のセルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)に関する説明を参照してください。

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)は、オン/オフまたは `true` / `false` 状態の表示とキャプチャに使用するコントロールです。 次のプロパティを定義します。

- `Text` –スイッチの横に表示するテキストです。
- `On` –スイッチをオンまたはオフとして表示するかどうかを指定します。
- `OnColor` – on の位置にあるときのスイッチの[`Color`](xref:Xamarin.Forms.Color) 。

これらのプロパティはすべてバインド可能です。

また[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)は、`OnChanged` イベントを公開して、セルの状態の変化に応答できるようにします。

![SwitchCell の例](tableview-images/switch-cell.png)

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell)は、ユーザーが編集できるテキストデータを表示する必要がある場合に便利です。 次のプロパティを定義します。

- `Keyboard` –編集中に表示するキーボード。 数値、電子メール、電話番号などのオプションがあります。 [API ドキュメントを参照して](xref:Xamarin.Forms.Keyboard)ください。
- `Label` –テキスト入力フィールドの左側に表示されるラベルテキスト。
- `LabelColor` –ラベルのテキストの色です。
- `Placeholder` –入力フィールドが null または空の場合に表示されるテキストです。 テキスト入力が開始されると、このテキストは表示されなくなります。
- `Text` –入力フィールドのテキスト。
- `HorizontalTextAlignment` –テキストの水平方向の配置です。 値は、[中央]、[左]、または [右揃え] です。 [API のドキュメントを参照してください](xref:Xamarin.Forms.TextAlignment)。
- `VerticalTextAlignment` –テキストの垂直方向の配置です。 値は `Start`、`Center`、または `End` です。

また[`EntryCell`](xref:Xamarin.Forms.EntryCell)は、ユーザーがテキストの編集中にキーボードの [完了] ボタンをクリックしたときに発生する `Completed` イベントも公開します。

![EntryCell の例](tableview-images/entry-cell.png)

<a name="Custom_Cells" />

## <a name="custom-cells"></a>カスタムセル

組み込みセルが十分でない場合は、カスタムセルを使用して、アプリにとって意味のある方法でデータを表示し、キャプチャすることができます。 たとえば、ユーザーがイメージの不透明度を選択できるようにスライダーを表示することができます。

すべてのカスタムセルは[`ViewCell`](xref:Xamarin.Forms.ViewCell)から派生する必要があります。これは、組み込みのすべてのセル型が使用するのと同じ基本クラスです。

カスタムセルの例を次に示します。

![カスタムセルの例](tableview-images/custom-cell.png)

次の例は、上記のスクリーンショットで[`TableView`](xref:Xamarin.Forms.TableView)を作成するために使用される XAML を示しています。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoTableView.TablePage"
             Title="TableView">
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
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() { Source = "bulb.png"});
layout.Children.Add (new Label()
{
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label ()
{
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot ()
{
    new TableSection("Getting Started")
    {
        new ViewCell() {View = layout}
    }
};
Content = table;
```

[@No__t_1](xref:Xamarin.Forms.TableView)の下のルート要素は[`TableRoot`](xref:Xamarin.Forms.TableRoot)であり、`TableRoot` のすぐ下に[`TableSection`](xref:Xamarin.Forms.TableSection)があります。 [@No__t_1](xref:Xamarin.Forms.ViewCell)は `TableSection` の下で直接定義され、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)はカスタムセルのレイアウトを管理するために使用されます。ただし、ここでは任意のレイアウトを使用できます。

> [!NOTE]
> [@No__t_1](xref:Xamarin.Forms.ListView)とは異なり、 [`TableView`](xref:Xamarin.Forms.TableView)では、カスタム (または任意の) セルが `ItemTemplate` で定義されている必要はありません。

## <a name="row-height"></a>行の高さ

[@No__t_1](xref:Xamarin.Forms.TableView)クラスには、セルの行の高さを変更するために使用できる2つのプロパティがあります。

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) –各行の高さを `int` に設定します。
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) – `true` に設定すると、行の高さが変化します。 このプロパティを `true` に設定すると、行の高さが自動的に計算され、Xamarin. Forms によって適用されることに注意してください。

[@No__t_1](xref:Xamarin.Forms.TableView)内のセルのコンテンツの高さが変更されると、その行の高さは Android とユニバーサル WINDOWS プラットフォーム (UWP) で暗黙的に更新されます。 ただし、iOS では、 [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)プロパティを `true` に設定し、 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッドを呼び出すことによって、強制的に更新する必要があります。

次の XAML の例は、 [`ViewCell`](xref:Xamarin.Forms.ViewCell)を含む[`TableView`](xref:Xamarin.Forms.TableView)を示しています。

```xaml
<ContentPage ...>
    <TableView ...
               HasUnevenRows="true">
        <TableRoot>
            ...
            <TableSection ...>
                ...
                <ViewCell x:Name="_viewCell"
                          Tapped="OnViewCellTapped">
                    <Grid Margin="15,0">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Label Text="Tap this cell." />
                        <Label x:Name="_target"
                               Grid.Row="1"
                               Text="The cell has changed size."
                               IsVisible="false" />
                    </Grid>
                </ViewCell>
            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

[@No__t_1](xref:Xamarin.Forms.ViewCell)がタップされると、`OnViewCellTapped` イベントハンドラーが実行されます。

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

@No__t_0 イベントハンドラーは[`ViewCell`](xref:Xamarin.Forms.ViewCell)の2番目の[`Label`](xref:Xamarin.Forms.Label)を表示または非表示にし、 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッドを呼び出すことによってセルのサイズを明示的に更新します。

次のスクリーンショットは、このセルをタップする前のセルを示しています。

![サイズを変更する前に ViewCell](tableview-images/cell-beforeresize.png)

次のスクリーンショットでは、セルがタップされた後に表示されます。

![サイズ変更後の ViewCell](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> この機能が過剰になると、パフォーマンスが低下する可能性が高くなります。

## <a name="related-links"></a>関連リンク

- [TableView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
