---
title: ListView の外観のカスタマイズ
description: この記事では、ヘッダー、フッター、グループ、および変数の高さのセルを使用して、Xamarin.Forms アプリケーションでの Listview をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: febf712848b81c09a4e25c824acc097e8b65e409
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245141"
---
# <a name="customizing-listview-appearance"></a>ListView の外観のカスタマイズ

`ListView` 基になるだけでなく、一覧全体の表示を制御するためのオプションがあります`ViewCell`s。 次のオプションがあります。

- [**グループ化**](#Grouping) &ndash;より簡単に移動し、強化された組織の ListView で項目をグループ化します。
- [**ヘッダーとフッター** ](#Headers_and_Footers) &ndash;先頭と末尾、他のアイテムと共にスクロールするビューの情報を表示します。
- [**行の区切り記号**](#Row_Separators) &ndash;表示を切り替える項目間の区分線を非表示にします。
- [**変数の高さ行**](#Row_Heights) &ndash;既定ですべての行を同じ高さはこれを表示するさまざまな高さを持つ行を許可するのには変更できます。

<a name="Grouping" />

## <a name="grouping"></a>グループ化
多くの場合、大量のデータが継続的にスクロール リストに表示されると長すぎて扱いになることができます。 有効にするグループ化は、内容をよく整理して、間を移動するデータを簡単にするプラットフォーム固有のコントロールをアクティブ化でこのような場合、ユーザー エクスペリエンスを向上できます。

グループ化が有効な場合、 `ListView`、各グループ ヘッダー行が追加されます。

グループ化を有効にします。

- (グループの一覧は、要素の一覧をされている各グループ) のリストの一覧を作成します。
- 設定、`ListView`の`ItemsSource`するためのリスト。
- 設定`IsGroupingEnabled`true に設定します。
- 設定[ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/)グループのタイトルとして使用されているグループのプロパティにバインドします。
- [オプション]設定[ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/)グループの短い名前として使用されているグループのプロパティにバインドします。 短い名前は、ジャンプ リスト (iOS の右側の列) に使用されます。

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

上記のコードで`All`バインディング ソースとして、リスト ビューに表示される一覧を示します。 `Title` および`ShortName`グループの見出しに使用されるプロパティです。

この段階で、`All`空のリストです。 プログラムの開始時、リストに表示されます、静的コンス トラクターを追加します。

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

上記のコードでは呼び出すことができますも`Add`の要素に対して`groups`、型のインスタンスは`PageTypeGroup`します。 これは、考えられるため`PageTypeGroup`から継承`List<PageModel>`です。 これは、上記の一覧パターンの一覧の例です。

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

![](customizing-list-appearance-images/grouping-depth.png "ListView のグループ化の例")

持つことに注意してください。

- 設定`GroupShortNameBinding`を`ShortName`グループ クラスで定義されたプロパティ
- 設定`GroupDisplayBinding`を`Title`グループ クラスで定義されたプロパティ
- 設定`IsGroupingEnabled`true
- 変更、`ListView`の`ItemsSource`をグループ化された一覧に

### <a name="customizing-grouping"></a>グループ化をカスタマイズします。

一覧にグループ化が有効になっている場合、グループ ヘッダーもカスタマイズできます。

ような方法、`ListView`が、 `ItemTemplate` 、行の表示方法を定義するため`ListView`が、`GroupHeaderTemplate`です。

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
リストの要素をスクロールして、ヘッダーとフッターに表示する ListView のことができます。 ヘッダーとフッターは、テキスト文字列またはより複雑なレイアウトを指定できます。 これは、異なる[セクション グループ](#Grouping)です。

設定することができます、`Header`や`Footer`を単純な文字列値、またはすることができますには設定をさらに複雑なレイアウトにします。
`HeaderTemplate`と`FooterTemplate`プロパティを使用して作成ヘッダーとフッターのレイアウトをさらに複雑なデータ バインディングをサポートします。

単純なヘッダーとフッターを作成するには、表示するテキストをヘッダーまたはページ フッターのプロパティを設定だけです。 コードは次のとおりです。

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ヘッダーとフッターを含む ListView")

カスタマイズされたヘッダーとフッターを作成するには、ヘッダーとページ フッターのビューを定義します。

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

![](customizing-list-appearance-images/header-custom.png "カスタマイズされたヘッダーとフッターを含む ListView")

<a name="Row_Separators" />

## <a name="row-separators"></a>行区切り記号
間に区切り線が表示される`ListView`iOS および Android では既定の要素。 IOS および Android 上の区分線を非表示にする場合は、設定、 `SeparatorVisibility` listview プロパティです。 オプションを`SeparatorVisibility`は。

* **既定の**-iOS および Android での区切り線を示しています。
* **None** -すべてのプラットフォーム上の区分線を非表示にします。

既定の表示:

C#: 

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "既定の行区切りのリスト ビュー")

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

経由での区切り線の色を設定することも、`SeparatorColor`プロパティ。

C#: 

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "緑の行区切りのリスト ビュー")

> [!NOTE]
> Android で読み込み後にこれらのプロパティのいずれかを設定、`ListView`パフォーマンスを大幅に低下します。

<a name="Row_Heights" />

## <a name="row-heights"></a>行の高さ
ListView のすべての行では、既定では同じ高さがあります。 ListView では、その動作を変更するために使用する 2 つのプロパティがあります。

- `HasUnevenRows` &ndash; `true`/`false` 値、行がある高さが異なる場合に設定`true`です。 既定値は `false` です。
- `RowHeight` &ndash; セットの高さ行`HasUnevenRows`は`false`します。

すべての行の高さを設定するには、設定を`RowHeight`プロパティを`ListView`です。

### <a name="custom-fixed-row-height"></a>カスタムの固定の行の高さ

C#: 

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "固定の行の高さを含む ListView")


### <a name="uneven-rows"></a>不均一な行

個々 の行があるさまざまな高さを希望する場合は、設定、`HasUnevenRows`プロパティを`true`です。
1 回手動で設定する行の高さがないことに注意してください`HasUnevenRows`に設定されている`true`高さは Xamarin.Forms によって自動的に計算されます。


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

各`ListView`行プログラムでサイズを変更できることを指定して、実行時に、`HasUnevenRows`プロパティに設定されている`true`です。 [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/)メソッドは、次のコード例で示したように、現在表示されていない場合でも、セルのサイズを更新します。

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

`OnImageTapped`への応答でイベント ハンドラーが実行される、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)セルでタップとのサイズを大きく、`Image`簡単に表示されるように、セルに表示されます。

![](customizing-list-appearance-images/dynamic-row-resizing.png "ランタイムの行がサイズ変更を含む ListView")

あるパフォーマンスの低下の可能性が高い場合はこの機能が過剰に注意してください。



## <a name="related-links"></a>関連リンク

- [グループ化 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Custom Renderer View (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [行の動的なサイズを変更する (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [1.4 のリリース ノート](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 リリース ノート](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
