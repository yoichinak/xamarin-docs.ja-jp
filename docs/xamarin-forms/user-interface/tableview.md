---
title: Xamarin.FormsTableView
description: この記事では、TableView クラスを使用して、 Xamarin.Forms アプリケーションでのスクロールメニュー、設定、入力フォームを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bf09856efddbd1887ee93b34014ef0f573058f4d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84565292"
---
# <a name="xamarinforms-tableview"></a>Xamarin.FormsTableView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView)は、同じテンプレートを共有しない行がある場合に、スクロール可能なデータまたは選択肢の一覧を表示するためのビューです。 [ListView](~/xamarin-forms/user-interface/listview/index.md)とは異なり、には `TableView` の概念がない `ItemsSource` ため、項目を子として手動で追加する必要があります。

![TableView の例](tableview-images/tableview-all-sml.png)

## <a name="use-cases"></a>ユース ケース

[`TableView`](xref:Xamarin.Forms.TableView)次の場合に便利です。

- 設定の一覧を表示する
- フォームでのデータの収集
- 行と行 (数値、パーセンテージ、画像など) とは異なる方法で表示されるデータを表示します。

[`TableView`](xref:Xamarin.Forms.TableView)上のシナリオで一般的に必要とされる、魅力的なセクションでの行のスクロールとレイアウトを処理します。 コントロールは、 `TableView` 使用可能な場合は各プラットフォームの基になる同等のビューを使用し、プラットフォームごとにネイティブな外観を作成します。

## <a name="structure"></a>構造体

の要素 [`TableView`](xref:Xamarin.Forms.TableView) は、セクションに整理されています。 のルートのは、1つ以上のインスタンスの親であるです `TableView` [`TableRoot`](xref:Xamarin.Forms.TableRoot) [`TableSection`](xref:Xamarin.Forms.TableSection) 。 各は [`TableSection`](xref:Xamarin.Forms.TableSection) 、見出しと1つ以上のインスタンスで構成され [`ViewCell`](xref:Xamarin.Forms.ViewCell) ます。

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

## <a name="appearance"></a>外観

[`TableView`](xref:Xamarin.Forms.TableView)[`Intent`](xref:Xamarin.Forms.TableView.Intent)プロパティを公開します。このプロパティは、列挙体の任意のメンバーに設定でき [`TableIntent`](xref:Xamarin.Forms.TableIntent) ます。

- `Data`–データエントリを表示するときに使用します。 [ListView](~/xamarin-forms/user-interface/listview/index.md)は、データのリストをスクロールする場合に、より適切なオプションであることに注意してください。
- `Form`– TableView がフォームとして機能する場合に使用します。
- `Menu`–選択項目のメニューを表示するときに使用します。
- `Settings`–構成設定の一覧を表示するときに使用します。

[`TableIntent`](xref:Xamarin.Forms.TableIntent)選択した値は、 [`TableView`](xref:Xamarin.Forms.TableView) 各プラットフォームでがどのように表示されるかに影響を与える可能性があります。 明確な違いがない場合でも、 `TableIntent` テーブルの使用方法に最も近いものを選択することをお勧めします。

さらに、プロパティをに設定することによって、それぞれに表示されるテキストの色を [`TableSection`](xref:Xamarin.Forms.TableSection) 変更でき `TextColor` [`Color`](xref:Xamarin.Forms.Color) ます。

## <a name="built-in-cells"></a>組み込みセル

Xamarin.Formsには、情報を収集して表示するための組み込みセルが付属しています。 [`ListView`](xref:Xamarin.Forms.ListView)とは [`TableView`](xref:Xamarin.Forms.TableView) 同じセルのすべてを使用できますが、 [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) [`EntryCell`](xref:Xamarin.Forms.EntryCell) はシナリオに最も適してい `TableView` ます。

[Textcell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell)と[ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)の詳細については、 [ListView のセルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)に関する説明を参照してください。

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)は、オン/オフまたは状態の表示とキャプチャに使用するコントロールです `true` / `false` 。 次のプロパティが定義されます。

- `Text`–スイッチの横に表示するテキスト。
- `On`–スイッチをオンまたはオフとして表示するかどうかを指定します。
- `OnColor`– [`Color`](xref:Xamarin.Forms.Color) on の位置にあるときのスイッチの。

これらのプロパティはすべてバインド可能です。

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)また、はイベントを公開し `OnChanged` 、セルの状態の変化に応答できるようにします。

![SwitchCell の例](tableview-images/switch-cell.png)

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell)は、ユーザーが編集できるテキストデータを表示する必要がある場合に便利です。 次のプロパティが定義されます。

- `Keyboard`–編集中に表示するキーボード。 数値、電子メール、電話番号などのオプションがあります。 [API ドキュメントを参照して](xref:Xamarin.Forms.Keyboard)ください。
- `Label`–テキスト入力フィールドの左側に表示されるラベルテキスト。
- `LabelColor`–ラベルのテキストの色。
- `Placeholder`–入力フィールドが null または空の場合に表示されるテキストです。 テキスト入力が開始されると、このテキストは表示されなくなります。
- `Text`–入力フィールドのテキスト。
- `HorizontalTextAlignment`–テキストの水平方向の配置。 値は、[中央]、[左]、または [右揃え] です。 [API のドキュメントを参照してください](xref:Xamarin.Forms.TextAlignment)。
- `VerticalTextAlignment`–テキストの垂直方向の配置。 値は `Start` 、、 `Center` 、または `End` です。

[`EntryCell`](xref:Xamarin.Forms.EntryCell)また、は、 `Completed` ユーザーがテキストの編集中にキーボードの [完了] ボタンをクリックしたときに発生するイベントも公開します。

![EntryCell の例](tableview-images/entry-cell.png)

## <a name="custom-cells"></a>カスタム セル

組み込みセルが十分でない場合は、カスタムセルを使用して、アプリにとって意味のある方法でデータを表示し、キャプチャすることができます。 たとえば、ユーザーがイメージの不透明度を選択できるようにスライダーを表示することができます。

すべてのカスタムセルは [`ViewCell`](xref:Xamarin.Forms.ViewCell) 、組み込みのすべてのセル型が使用するのと同じ基本クラスから派生する必要があります。

カスタムセルの例を次に示します。

![カスタムセルの例](tableview-images/custom-cell.png)

次の例は、上記のスクリーンショットでを作成するために使用される XAML を示してい [`TableView`](xref:Xamarin.Forms.TableView) ます。

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

の下のルート要素 [`TableView`](xref:Xamarin.Forms.TableView) は [`TableRoot`](xref:Xamarin.Forms.TableRoot) であり、の [`TableSection`](xref:Xamarin.Forms.TableSection) すぐ下にあり `TableRoot` ます。 は、の [`ViewCell`](xref:Xamarin.Forms.ViewCell) 下で直接定義され、 `TableSection` を使用して [`StackLayout`](xref:Xamarin.Forms.StackLayout) カスタムセルのレイアウトを管理します。ただし、ここでは任意のレイアウトを使用できます。

> [!NOTE]
> とは異なり [`ListView`](xref:Xamarin.Forms.ListView) 、で [`TableView`](xref:Xamarin.Forms.TableView) はカスタム (または任意の) セルがで定義されている必要はありません `ItemTemplate` 。

## <a name="row-height"></a>行の高さ

[`TableView`](xref:Xamarin.Forms.TableView)クラスには、セルの行の高さを変更するために使用できる2つのプロパティがあります。

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)–各行の高さをに設定 `int` します。
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)–に設定すると、行の高さが変化 `true` します。 このプロパティをに設定すると `true` 、行の高さが自動的に計算され、によって適用されることに注意して Xamarin.Forms ください。

内のセルのコンテンツの高さが変更された場合 [`TableView`](xref:Xamarin.Forms.TableView) 、行の高さは Android とユニバーサル Windows プラットフォーム (UWP) で暗黙的に更新されます。 ただし、iOS では、プロパティをに設定し、メソッドを呼び出すことによって、強制的に更新する必要があり [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) `true` [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) ます。

次の XAML の例は、を含むを示して [`TableView`](xref:Xamarin.Forms.TableView) い [`ViewCell`](xref:Xamarin.Forms.ViewCell) ます。

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

[`ViewCell`](xref:Xamarin.Forms.ViewCell)がタップされると、 `OnViewCellTapped` イベントハンドラーが実行されます。

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

`OnViewCellTapped`イベントハンドラーは、の2番目のを表示または非表示に [`Label`](xref:Xamarin.Forms.Label) し、メソッドを [`ViewCell`](xref:Xamarin.Forms.ViewCell) 呼び出すことによってセルのサイズを明示的に更新し [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) ます。

次のスクリーンショットは、このセルをタップする前のセルを示しています。

![サイズを変更する前に ViewCell](tableview-images/cell-beforeresize.png)

次のスクリーンショットでは、セルがタップされた後に表示されます。

![サイズ変更後の ViewCell](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> この機能が過剰になると、パフォーマンスが低下する可能性が高くなります。

## <a name="related-links"></a>関連リンク

- [TableView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
