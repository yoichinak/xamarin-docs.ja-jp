---
title: ListView の外観のカスタマイズ
description: この記事では、ヘッダー、フッター、グループ、および高さが可変のセルを使用して、Xamarin.Forms アプリケーションの Listview をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
ms.openlocfilehash: fc0664ff32e63af5d0c80f69ff69f4992ad0c708
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770308"
---
# <a name="customizing-listview-appearance"></a>ListView の外観のカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

[`ListView`](xref:Xamarin.Forms.ListView)には、リストの各行の[`ViewCell`](xref:Xamarin.Forms.ViewCell)インスタンスに加えて、一覧の表示を制御する機能があります。

<a name="Grouping" />

## <a name="grouping"></a>グループ化
多くの場合、大規模なデータ セットは、継続的にスクロール リストに表示された場合は、扱いになります。 グループ化を有効にするには、コンテンツをより適切に整理して移動するデータを簡単にするプラットフォーム固有のコントロールをアクティブ化するこのような場合、ユーザー エクスペリエンスが向上します。

グループ化が有効な場合、 `ListView`、各グループ ヘッダー行が追加されます。

グループ化を有効にします。

- (グループの一覧、各グループの要素の一覧) のリストの一覧を作成します。
- 設定、`ListView`の`ItemsSource`リストにします。
- 設定`IsGroupingEnabled`を true にします。
- 設定[ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding)をグループのタイトルとして使用されているグループのプロパティにバインドします。
- [省略可能]設定[ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding)をグループの短い名前として使用されているグループのプロパティにバインドします。 ジャンプ リスト (iOS の右側にある列) の短い名前が使用されます。

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

上記のコードで`All`バインディング ソースとして、ListView に与えられる一覧を示します。 `Title` `ShortName`はグループの見出しに使用されるプロパティです。

この段階で、`All`は空のリストです。 プログラムの開始時、リストに表示されます、静的コンストラクターを追加します。

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
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
        }
        All = Groups; //set the publicly accessible list
}
```

上のコードでは、型`Add` `PageTypeGroup`のインスタンスであるの`groups`要素に対してを呼び出すこともできます。 これは、考えられるため、`PageTypeGroup`から継承`List<PageModel>`します。 これは、上記の一覧パターンの一覧の例です。

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

次のような結果が得られます。

![](customizing-list-appearance-images/grouping-depth.png "ListView グループ化の例")

持つことに注意してください。

- 設定`GroupShortNameBinding`を`ShortName`グループ クラスで定義されたプロパティ
- 設定`GroupDisplayBinding`を`Title`グループ クラスで定義されたプロパティ
- 設定`IsGroupingEnabled`true
- 変更、`ListView`の`ItemsSource`にグループ化された一覧

### <a name="customizing-grouping"></a>グループ化をカスタマイズします。

一覧にグループ化が有効になっている場合、グループ ヘッダーはカスタマイズもできます。

方法に似ています`ListView`が、 `ItemTemplate` 、行の表示方法を定義するため`ListView`が、`GroupHeaderTemplate`します。

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

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>ヘッダーとフッター
リストの要素にスクロールするヘッダーとフッターを提示する ListView のことができます。 ヘッダーとフッターは、テキスト文字列またはより複雑なレイアウトを指定できます。 これは異なることに注意してください。[セクション グループ](#Grouping)します。

設定することができます、`Header`や`Footer`単純な文字列値、またはに設定により複雑なレイアウトです。
`HeaderTemplate`と`FooterTemplate`プロパティを使用して作成ヘッダーとフッターのレイアウトをさらに複雑なデータ バインディングをサポートします。

単純なヘッダー/フッターを作成するには、表示するテキストをヘッダーまたはページ フッターのプロパティを設定だけです。 コードは次のとおりです。

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

で XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView ヘッダーとフッター")

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

![](customizing-list-appearance-images/header-custom.png "カスタマイズされたヘッダーとフッターを ListView")

## <a name="scrollbar-visibility"></a>スクロールバーの表示

[`ListView`](xref:Xamarin.Forms.ListView)プロパティ`HorizontalScrollBarVisibility`と`VerticalScrollBarVisibility`プロパティがあります。これらの[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)プロパティは、水平方向または垂直方向のスクロールバーが表示されるタイミングを表す値を取得または設定します。 どちらのプロパティも、次の値に設定できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)プラットフォームの既定のスクロールバーの動作を示します。は、プロパティ`HorizontalScrollBarVisibility`と`VerticalScrollBarVisibility`プロパティの既定値です。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)コンテンツがビューに収まらない場合でも、スクロールバーが表示されないことを示します。

<a name="Row_Separators" />

## <a name="row-separators"></a>行区切り記号
間に区分線が表示される`ListView`既定では iOS と Android での要素。 IOS や Android 上の区分線を非表示にする場合は、設定、 `SeparatorVisibility` ListView のプロパティ。 オプション`SeparatorVisibility`は。

- **既定の**-iOS および Android での区切り線を示しています。
- **None** -すべてのプラットフォーム上の区分線を非表示にします。

既定の可視性:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "既定の行の区切り記号で ListView")

None:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "行の区切り文字のない ListView")

使用して区切り線の色を設定することも、`SeparatorColor`プロパティ。

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "緑の行の区切り記号で ListView")

> [!NOTE]
> Android で読み込み後にこれらのプロパティのいずれかを設定、`ListView`パフォーマンスが大幅に低下します。

<a name="Row_Heights" />

## <a name="row-heights"></a>行の高さ
既定では、同じ高さがある、ListView のすべての行。 ListView では、その動作を変更するために使用できる 2 つのプロパティがあります。

- `HasUnevenRows` &ndash; `true`/`false` 値、行にある高さが異なる場合に設定`true`します。 既定値は `false` です。
- `RowHeight` &ndash; セットの高さときに行`HasUnevenRows`は`false`します。

すべての行の高さを設定するには、設定を`RowHeight`プロパティを`ListView`します。

### <a name="custom-fixed-row-height"></a>カスタムの固定の行の高さ

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "固定の行の高さを持つ ListView")

### <a name="uneven-rows"></a>不均一な行

設定することができますが異なる高さの個々 の行が希望される場合、`HasUnevenRows`プロパティを`true`します。
1 回手動で設定する行の高さがないことに注意してください`HasUnevenRows`に設定されている`true`高さは、Xamarin.Forms が自動的に計算されます。

C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "不均一な行を含む ListView")

### <a name="runtime-resizing-of-rows"></a>ランタイムは、行のサイズ変更

個別`ListView`行プログラムでサイズを変更できること、実行時に、`HasUnevenRows`プロパティに設定されて`true`します。 [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッドは、次のコード例に示すように現在表示されている、違う場合でも、セルのサイズを更新します。

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

`OnImageTapped`への応答でイベント ハンドラーが実行を[ `Image` ](xref:Xamarin.Forms.Image)セルでタップとのサイズを大きく、`Image`が簡単に表示されるように、セルに表示します。

![](customizing-list-appearance-images/dynamic-row-resizing.png "ランタイムの行のサイズを変更することで ListView")

パフォーマンスの低下の可能性が高い場合、この機能が過剰に注意してください。

## <a name="related-links"></a>関連リンク

- [グループ化 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Custom Renderer View (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [行の動的なサイズ変更 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [1.4 リリース ノート](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 リリース ノート](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
