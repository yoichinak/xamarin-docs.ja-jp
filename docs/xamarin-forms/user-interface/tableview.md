---
title: Xamarin.Forms テーブル
description: この記事では、Xamarin.Forms テーブル クラスを使用して、アプリケーションでスクロール メニューの設定、および入力フォームを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: c18eba873dc1a1dae36c401507d55652ed233b00
ms.sourcegitcommit: 236a346838c421c7d8951f50abbf4f5365559372
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2019
ms.locfileid: "58641440"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms テーブル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)

[`TableView`](xref:Xamarin.Forms.TableView) データや選択肢のスクロール可能な一覧を表示するビューを同じテンプレートを共有しない行がある場合。 異なり[ListView](~/xamarin-forms/user-interface/listview/index.md)、`TableView`の概念はありません、`ItemsSource`ので、項目を子として手動で追加する必要があります。

![](tableview-images/tableview-all-sml.png "テーブルの例")

<a name="Use_Cases" />

## <a name="use-cases"></a>ユース ケース

[`TableView`](xref:Xamarin.Forms.TableView) ときに便利です。

- 設定の一覧を表示します。
- フォームでは、データの収集または
- 行の行 (数、パーセンテージとイメージなど) に異なる方法で表示されているデータを表示しています。

[`TableView`](xref:Xamarin.Forms.TableView) スクロールし、魅力的なセクションでは、上記のシナリオの一般的な必要性内の行のレイアウトを処理します。 `TableView`コントロールが使用する各プラットフォームの使用可能な各プラットフォームのネイティブの外観を作成する場合、同等のビューを基になります。

<a name="TableView_Structure" />

## <a name="structure"></a>構造体

内の要素を[ `TableView` ](xref:Xamarin.Forms.TableView)セクションに分類されます。 ルートにある、`TableView`は、 [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)、1 つまたは複数の親である[ `TableSection` ](xref:Xamarin.Forms.TableSection)インスタンス。 各[ `TableSection` ](xref:Xamarin.Forms.TableSection)見出しと 1 つ以上から成る[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)インスタンス。

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

同等の C# コードに示します。

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

[`TableView`](xref:Xamarin.Forms.TableView) 公開、 [ `Intent` ](xref:Xamarin.Forms.TableView.Intent)プロパティのいずれかに設定することができます、 [ `TableIntent` ](xref:Xamarin.Forms.TableIntent)列挙型メンバー。

- `Data` – データ エントリを表示するときに使用します。 なお[ListView](~/xamarin-forms/user-interface/listview/index.md)データのリストをスクロールするための優れた選択肢があります。
- `Form` – フォームとして、テーブルが動作しているときに使用します。
- `Menu` – 選択項目のメニューを表示するときに使用します。
- `Settings` – 構成設定の一覧を表示するときに使用します。

[ `TableIntent` ](xref:Xamarin.Forms.TableIntent)値の影響を与える可能性を選択する方法、 [ `TableView` ](xref:Xamarin.Forms.TableView)プラットフォームごとに表示されます。 明確な違いがない場合でも、テーブルの使用方法に最も近い`TableIntent`を選択することをお勧めします。

テキストの色をさらに、各表示[ `TableSection` ](xref:Xamarin.Forms.TableSection)を設定して変更することができます、`TextColor`プロパティを[ `Color`](xref:Xamarin.Forms.Color)します。

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>組み込みのセル

Xamarin.Forms を収集して情報を表示するための組み込みのセルが付属します。 [ `ListView` ](xref:Xamarin.Forms.ListView)と[ `TableView` ](xref:Xamarin.Forms.TableView)同じのセルのすべてを使用できます[ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell)と[ `EntryCell` ](xref:Xamarin.Forms.EntryCell)に最も関連性は、`TableView`シナリオ。

[TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)と[ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell)の詳細な説明は、[ListView セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)を参照してください。

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) は、`on` / `off`または`true` / `false`の状態を表示し、キャプチャするために使用するコントロールです。 これには、次のプロパティを定義します。

- `Text` – スイッチの横に表示するテキスト。
- `On` – として on または off かどうか、スイッチが表示されます。
- `OnColor` –、 [ `Color` ](xref:Xamarin.Forms.Color)の位置にあるときに、スイッチの。

これらすべてのプロパティは、バインド可能です。

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) また、公開、`OnChanged`イベント、セルの状態の変更に応答することができます。

![](tableview-images/switch-cell.png "SwitchCell 例")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) ユーザーが編集できるテキスト データを表示する必要がある場合に役立ちます。 これには、次のプロパティを定義します。

- `Keyboard` – 編集中に表示するキーボード。 数値、電子メール、電話番号などのオプションがあります。[API ドキュメントを参照してください。](xref:Xamarin.Forms.Keyboard)
- `Label` – テキスト入力フィールドの左側に表示するラベル テキスト。
- `LabelColor` – ラベルのテキストの色。
- `Placeholder` – が null または空のときに、入力フィールドに表示するテキスト。 このテキストは、テキスト エントリの開始時に表示されなくなります。
- `Text` – エントリ フィールド内のテキスト。
- `HorizontalTextAlignment` – テキストの水平方向の配置。 センター、左、または右に配置できます。 [API ドキュメントを参照してください。](xref:Xamarin.Forms.TextAlignment)

[`EntryCell`](xref:Xamarin.Forms.EntryCell) 公開、`Completed`イベントで、ユーザーがテキストの編集中に、キーボードの '完了' ボタンを押すときに発生します。

![](tableview-images/entry-cell.png "EntryCell 例")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>カスタムのセル

組み込みのセルが十分でない場合、カスタムのセルを使用することで、アプリに合った方法でデータを表示しキャプチャすることができます。 たとえば、画像の不透明度を選択するようにする場合、スライダーを提示したい場合があります。

全てのカスタムセルは、すべての組み込みのセルが使用する基本クラスである[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)から派生する必要があります。

これは、カスタムのセルの例を示します。

![](tableview-images/custom-cell.png "カスタムのセルの例")

次の例を作成するために使用する XAML を示しています、 [ `TableView` ](xref:Xamarin.Forms.TableView)で上記のスクリーン ショット。

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

同等の C# コードに示します。

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

ルート要素の下、 [ `TableView` ](xref:Xamarin.Forms.TableView)は、 [ `TableRoot`](xref:Xamarin.Forms.TableRoot)があると、 [ `TableSection` ](xref:Xamarin.Forms.TableSection)すぐ下、`TableRoot`します。 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)で直接定義されています、 `TableSection`、および[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)のレイアウトをここで使用できますが、カスタムのセルのレイアウトを管理に使用されます。

> [!NOTE]
> 異なり[ `ListView` ](xref:Xamarin.Forms.ListView)、 [ `TableView` ](xref:Xamarin.Forms.TableView) (またはそのいずれか) をそのカスタムは不要でセルが定義されている、`ItemTemplate`します。

## <a name="row-height"></a>行の高さ

[ `TableView` ](xref:Xamarin.Forms.TableView)クラスには 2 つのプロパティのセルの行の高さを変更するために使用できます。

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) – に各行の高さを設定、`int`します。
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) – 行である高さが異なる場合に設定`true`します。 このプロパティを設定する場合は注意`true`行の高さが自動的に計算および Xamarin.Forms で適用します。

ときにセル内のコンテンツの高さを[ `TableView` ](xref:Xamarin.Forms.TableView)が変更されると、行の高さは、Android、ユニバーサル Windows プラットフォーム (UWP) で暗黙的に更新されます。 ただし、iOS でその必要があります設定を更新する、 [ `HasUnevenRows` ](xref:Xamarin.Forms.TableView.HasUnevenRows)プロパティを`true`と呼び出すことによって、 [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッド。

次の XAML の例は、 [ `TableView` ](xref:Xamarin.Forms.TableView)を格納している、 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell):

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

ときに、 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)がタップされた、`OnViewCellTapped`イベント ハンドラーが実行されます。

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

`OnViewCellTapped`イベント ハンドラー表示と、2 つ目の非表示[ `Label` ](xref:Xamarin.Forms.Label)で、 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)、明示的に呼び出すことによって、セルのサイズを更新し、 [ `Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッド。

次のスクリーン ショットは、時に消費する前に、セルを表示します。

![](tableview-images/cell-beforeresize.png "ViewCell のサイズ変更する前に")

次のスクリーン ショットでは、タップすると後にセルを表示します。

![](tableview-images/cell-afterresize.png "ViewCell のサイズ変更した後")

> [!IMPORTANT]
> 強力なパフォーマンスの低下の可能性がある場合、この機能が過剰です。

## <a name="related-links"></a>関連リンク

- [テーブル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
