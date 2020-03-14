---
title: ListView の外観
description: この記事では、ヘッダー、フッター、グループ、および高さが可変のセルを使用して、Xamarin.Forms アプリケーションの Listview をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
ms.openlocfilehash: 90b0e0f3802ce766decb802c9406d72b5966360e
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305649"
---
# <a name="listview-appearance"></a>ListView の外観

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

Xamarin [`ListView`](xref:Xamarin.Forms.ListView)を使用すると、リストの各行の[`ViewCell`](xref:Xamarin.Forms.ViewCell)インスタンスに加えて、一覧の表示をカスタマイズできます。

## <a name="grouping"></a>グループ化

連続したスクロールリストに表示されている場合、データの大規模なセットが扱いにくくなることがあります。 グループ化を有効にするには、コンテンツをより適切に整理して移動するデータを簡単にするプラットフォーム固有のコントロールをアクティブ化するこのような場合、ユーザー エクスペリエンスが向上します。

`ListView`に対してグループ化がアクティブになると、グループごとにヘッダー行が追加されます。

グループ化を有効にします。

- (グループの一覧、各グループの要素の一覧) のリストの一覧を作成します。
- `ListView`の `ItemsSource` をその一覧に設定します。
- `IsGroupingEnabled` を true に設定します。
- グループのタイトルとして使用されているグループのプロパティにバインドするには、 [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding)を設定します。
- Optionalグループの短い名前として使用されているグループのプロパティにバインドするには、 [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding)を設定します。 ジャンプ リスト (iOS の右側にある列) の短い名前が使用されます。

グループのクラスを作成して開始します。

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

上記のコードでは、`All` は、ListView にバインドソースとして指定されるリストです。 `Title` と `ShortName` は、グループの見出しに使用されるプロパティです。

この段階では、`All` は空のリストです。 プログラムの開始時、リストに表示されます、静的コンス トラクターを追加します。

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alpha", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        };
        All = Groups; //set the publicly accessible list
}
```

上記のコードでは、`Groups`の要素で `Add` を呼び出すこともできます。これは `PageTypeGroup`型のインスタンスです。 `PageTypeGroup` は `List<PageModel>`から継承されるため、このメソッドを使用できます。

グループ化された一覧を表示するための XAML を次に示します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
                   GroupDisplayBinding="{Binding Title}"
                   GroupShortNameBinding="{Binding ShortName}"
                   IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

この XAML は、次の操作を実行します。

- `GroupShortNameBinding` を、group クラスで定義されている `ShortName` プロパティに設定します。
- `GroupDisplayBinding` を、group クラスで定義されている `Title` プロパティに設定します。
- `IsGroupingEnabled` を true に設定します
- `ListView`の `ItemsSource` をグループ化されたリストに変更しました

次のスクリーンショットは、結果として得られる UI を示しています。

![](customizing-list-appearance-images/grouping-depth.png "ListView Grouping Example")

### <a name="customizing-grouping"></a>カスタマイズ (グループ化を)

一覧にグループ化が有効になっている場合、グループ ヘッダーはカスタマイズもできます。

`ListView` には、行の表示方法を定義するための `ItemTemplate` があるのと同様に、`ListView` には `GroupHeaderTemplate`があります。

XAML でグループ ヘッダーをカスタマイズする例を次に示します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
                  GroupDisplayBinding="{Binding Title}"
                  GroupShortNameBinding="{Binding ShortName}"
                  IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding ShortName}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

## <a name="headers-and-footers"></a>ヘッダーとフッター

リストの要素にスクロールするヘッダーとフッターを提示する ListView のことができます。 ヘッダーとフッターは、テキスト文字列またはより複雑なレイアウトを指定できます。 この動作は、[セクショングループ](#grouping)とは別のものです。

`Header` または `Footer` を `string` 値に設定することも、より複雑なレイアウトに設定することもできます。 また、`HeaderTemplate` プロパティと `FooterTemplate` プロパティもあります。これにより、データバインディングをサポートするヘッダーとフッターに対してより複雑なレイアウトを作成できます。

基本ヘッダー/フッターを作成するには、表示するテキストにヘッダーまたはフッターのプロパティを設定するだけです。 コード内で以下のように指定します。

```csharp
ListView HeaderList = new ListView()
{
    Header = "Header",
    Footer = "Footer"
};
```

で XAML:

```xaml
<ListView x:Name="HeaderList" 
          Header="Header"
          Footer="Footer">
    ...
</ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView with Header and Footer")

カスタマイズされたヘッダーとフッターを作成するには、ヘッダーとフッターのビューを定義します。

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
               TextColor="Olive"
               BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
               TextColor="Gray"
               BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView with Customized Header and Footer")

## <a name="scrollbar-visibility"></a>スクロールバーの表示

[`ListView`](xref:Xamarin.Forms.ListView)クラスには、水平方向または垂直方向のスクロールバーが表示されるタイミングを表す[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)の値を取得または設定する `HorizontalScrollBarVisibility` プロパティと `VerticalScrollBarVisibility` プロパティがあります。 どちらのプロパティも、次の値に設定できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)は、プラットフォームの既定のスクロールバーの動作を示します。は、`HorizontalScrollBarVisibility` プロパティと `VerticalScrollBarVisibility` プロパティの既定値です。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)は、ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)は、ビューにコンテンツが収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="row-separators"></a>行区切り記号

既定では、iOS と Android では、`ListView` の要素間に区切り線が表示されます。 IOS と Android の区切り線を非表示にする場合は、ListView の [`SeparatorVisibility`] プロパティを設定します。 `SeparatorVisibility` のオプションは次のとおりです。

- **既定**-IOS と Android の区切り線を表示します。
- **None** -すべてのプラットフォームの区切り記号を非表示にします。

既定の可視性:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView with Default Row Separators")

なし:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView without Row Separators")

また、`SeparatorColor` プロパティを使用して、区切り線の色を設定することもできます。

C#:

```csharp
SeparatorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView with Green Row Separators")

> [!NOTE]
> `ListView` の読み込み後に Android でこれらのプロパティのいずれかを設定すると、パフォーマンスが大幅に低下します。

## <a name="row-height"></a>行の高さ

既定では、同じ高さがある、ListView のすべての行。 ListView では、その動作を変更するために使用できる 2 つのプロパティがあります。

- `false` に設定されている場合、&ndash; `true`/`true`値を `HasUnevenRows` すると、行の高さが変化します。 既定値は `false` です。
- `HasUnevenRows` を `false`するときに各行の高さを `RowHeight` &ndash; 設定します。

すべての行の高さを設定するには、`ListView`の [`RowHeight`] プロパティを設定します。

### <a name="custom-fixed-row-height"></a>カスタム固定行の高さ

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView with Fixed Row Height")

### <a name="uneven-rows"></a>不均等の行

個々の行の高さを変えたい場合は、`HasUnevenRows` プロパティを `true`に設定します。 `HasUnevenRows` を `true`に設定した後、行の高さを手動で設定する必要はありません。これは、高さが Xamarin. Forms によって自動的に計算されるためです。

C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView with Uneven Rows")

### <a name="resize-rows-at-runtime"></a>実行時に行のサイズを変更する

`HasUnevenRows` プロパティが `true`に設定されていれば、実行時に個々の `ListView` の行のサイズを変更できます。 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッドは、次のコード例に示すように、現在表示されていない場合でもセルのサイズを更新します。

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` イベントハンドラーは、タップされるセル内の[`Image`](xref:Xamarin.Forms.Image)に応答して実行され、セルに表示される `Image` のサイズを大きくすることで、簡単に表示できるようにします。

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView with Runtime Row Resizing")

> [!WARNING]
> ランタイム行のサイズ変更が過剰になると、パフォーマンスが低下する可能性があります。

## <a name="related-links"></a>関連リンク

- [グループ化 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [カスタムレンダラービュー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [行の動的なサイズ変更 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [1.4 リリースノート](https://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 リリースノート](https://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
