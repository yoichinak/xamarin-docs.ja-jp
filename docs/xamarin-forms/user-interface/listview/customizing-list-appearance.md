---
title: ListView の外観
description: この記事では Xamarin.Forms 、ヘッダー、フッター、グループ、および可変の高さセルを使用して、アプリケーションで ListViews をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: be8dd5d29aebf29395885d650fbd28082013d0d1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929165"
---
# <a name="listview-appearance"></a>ListView の外観

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

を使用すると、リスト Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) の各行のインスタンスに加えて、一覧の表示をカスタマイズでき [`ViewCell`](xref:Xamarin.Forms.ViewCell) ます。

## <a name="grouping"></a>グループ化

連続したスクロールリストに表示されている場合、データの大規模なセットが扱いにくくなることがあります。 グループ化を有効にすると、コンテンツを整理したり、データの移動を容易にするプラットフォーム固有のコントロールをアクティブ化したりすることで、ユーザーエクスペリエンスを向上させることができます。

に対してグループ化がアクティブになると `ListView` 、各グループにヘッダー行が追加されます。

グループ化を有効にするには:

- リストの一覧を作成します (グループのリスト、各グループは要素の一覧です)。
- のを `ListView` `ItemsSource` そのリストに設定します。
- `IsGroupingEnabled` を true に設定します。
- [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding)グループのタイトルとして使用されているグループのプロパティにバインドするように設定します。
- Optional[`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding)グループの短い名前として使用されているグループのプロパティにバインドするように設定します。 短い名前は、ジャンプリスト (iOS の右側の列) に使用されます。

まず、グループのクラスを作成します。

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

上記のコードで `All` は、は、ListView にバインドソースとして指定されるリストです。 `Title`および `ShortName` は、グループの見出しに使用されるプロパティです。

この段階で `All` は、は空のリストです。 プログラムの開始時にリストが設定されるように、静的コンストラクターを追加します。

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

上のコードでは `Add` 、型のインスタンスであるの要素に対してを呼び出すこともでき `Groups` `PageTypeGroup` ます。 はを継承するため、このメソッドを使用でき `PageTypeGroup` `List<PageModel>` ます。

グループ化されたリストを表示するための XAML は次のとおりです。

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

- `GroupShortNameBinding` `ShortName` Group クラスで定義されているプロパティに設定します。
- `GroupDisplayBinding` `Title` Group クラスで定義されているプロパティに設定します。
- `IsGroupingEnabled`True に設定
- をグループ化された `ListView` リストに変更しました。 `ItemsSource`

次のスクリーンショットは、結果として得られる UI を示しています。

![ListView グループ化の例](customizing-list-appearance-images/grouping-depth.png)

### <a name="customizing-grouping"></a>カスタマイズ (グループ化を)

一覧でグループ化が有効になっている場合は、グループヘッダーをカスタマイズすることもできます。

には、 `ListView` `ItemTemplate` 行の表示方法を定義するためのが用意されているのと同様に、 `ListView` があり `GroupHeaderTemplate` ます。

XAML でグループヘッダーをカスタマイズする例を次に示します。

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

ListView は、リストの要素でスクロールするヘッダーとフッターを表示することができます。 ヘッダーとフッターには、テキストの文字列またはより複雑なレイアウトを指定できます。 この動作は、[セクショングループ](#grouping)とは別のものです。

またはを値に設定することも、 `Header` `Footer` `string` より複雑なレイアウトに設定することもできます。 また `HeaderTemplate` 、 `FooterTemplate` データバインディングをサポートするヘッダーとフッターに対してより複雑なレイアウトを作成するためのプロパティもあります。

基本ヘッダー/フッターを作成するには、表示するテキストにヘッダーまたはフッターのプロパティを設定するだけです。 コードは次のとおりです。

```csharp
ListView HeaderList = new ListView()
{
    Header = "Header",
    Footer = "Footer"
};
```

XAML の場合:

```xaml
<ListView x:Name="HeaderList" 
          Header="Header"
          Footer="Footer">
    ...
</ListView>
```

![ヘッダーとフッターを含む ListView](customizing-list-appearance-images/header-default.png)

カスタマイズしたヘッダーとフッターを作成するには、次のようにヘッダーとフッターのビューを定義します。

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

![カスタマイズされたヘッダーとフッターを含む ListView](customizing-list-appearance-images/header-custom.png)

## <a name="scrollbar-visibility"></a>スクロールバーの表示

[`ListView`](xref:Xamarin.Forms.ListView)クラスには `HorizontalScrollBarVisibility` プロパティとプロパティがあり、 `VerticalScrollBarVisibility` [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 水平または垂直のスクロールバーが表示されるタイミングを表す値を取得または設定します。 どちらのプロパティも、次の値に設定できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)プラットフォームの既定のスクロールバーの動作を示し `HorizontalScrollBarVisibility` ます。は、プロパティとプロパティの既定値です `VerticalScrollBarVisibility` 。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)コンテンツがビューに収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="row-separators"></a>行区切り記号

`ListView`既定では、iOS と Android では、要素間に区切り線が表示されます。 IOS と Android の区切り線を非表示にする場合は、 `SeparatorVisibility` ListView でプロパティを設定します。 のオプション `SeparatorVisibility` は次のとおりです。

- **既定**-IOS と Android の区切り線を表示します。
- **None** -すべてのプラットフォームの区切り記号を非表示にします。

既定の表示:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![既定の行区切り記号を含む ListView](customizing-list-appearance-images/separator-default.png)

なし: 

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![行区切り記号を含まない ListView](customizing-list-appearance-images/separator-none.png)

プロパティを使用して、区切り線の色を設定することもでき `SeparatorColor` ます。

C#:

```csharp
SeparatorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![緑色の行区切り記号を含む ListView](customizing-list-appearance-images/separator-custom.png)

> [!NOTE]
> の読み込み後に Android でこれらのプロパティのいずれかを設定すると、 `ListView` パフォーマンスが大幅に低下します。

## <a name="row-height"></a>行の高さ

既定では、ListView 内のすべての行の高さは同じになります。 ListView には、その動作を変更するために使用できる2つのプロパティがあります。

- `HasUnevenRows`&ndash; `true`/`false`値をに設定すると、行の高さが変化 `true` します。 既定値は `false` です。
- `RowHeight`&ndash;がの場合に各行の高さを設定し `HasUnevenRows` `false` ます。

すべての行の高さを設定するには、のプロパティを設定し `RowHeight` `ListView` ます。

### <a name="custom-fixed-row-height"></a>カスタム固定行の高さ

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![行の高さが固定される ListView](customizing-list-appearance-images/height-custom.png)

### <a name="uneven-rows"></a>不均等の行

個々の行の高さを変えたい場合は、 `HasUnevenRows` プロパティをに設定でき `true` ます。 高さはに `HasUnevenRows` `true` よって自動的に計算されるため、をに設定すると、行の高さを手動で設定する必要はありません Xamarin.Forms 。

C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![不均等な行を含む ListView](customizing-list-appearance-images/height-uneven.png)

### <a name="resize-rows-at-runtime"></a>実行時に行のサイズを変更する

`ListView`プロパティがに設定されていれば、実行時に個々の行のサイズをプログラムで変更でき `HasUnevenRows` `true` ます。 メソッドは、 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) 次のコード例に示すように、現在表示されていない場合でもセルのサイズを更新します。

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

`OnImageTapped`イベントハンドラーは、タップされるセル内のに対する応答として実行され、 [`Image`](xref:Xamarin.Forms.Image) セルに表示されるのサイズを大きくして、簡単に `Image` 表示できるようにします。

![ランタイム行のサイズ変更による ListView](customizing-list-appearance-images/dynamic-row-resizing.png)

> [!WARNING]
> ランタイム行のサイズ変更が過剰になると、パフォーマンスが低下する可能性があります。

## <a name="related-links"></a>関連リンク

- [グループ化 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [カスタムレンダラービュー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [行の動的なサイズ変更 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [1.4 リリースノート](https://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 リリースノート](https://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
