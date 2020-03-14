---
title: カスケード スタイル シート (CSS) を使用した Xamarin.Forms アプリのスタイル設定
description: Xamarin.Forms では、カスケード スタイル シート (CSS) を使用した視覚要素のスタイリングをサポートします。
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 726ebd55b38460ee966113e4ee487327cd42b03d
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305601"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>カスケード スタイル シート (CSS) を使用した Xamarin.Forms アプリのスタイル設定

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin は、カスケードスタイルシート (CSS) を使用したビジュアル要素のスタイル設定をサポートしています。_

Xamarin.Forms アプリケーションは、CSS を使用してスタイルに設定できます。 スタイル シートは、規則の一覧で構成され、各規則は 1 つまたは複数のセレクターと宣言ブロックで構成されます。 宣言ブロックは、中かっこ内の宣言の一覧で構成され、各宣言はプロパティ・コロン・値から構成されます。 ブロック内に複数の宣言が存在する場合、区切り記号としてセミコロンが挿入されます。 次のコード例は、Xamarin.Forms に対応した CSS の一部を示しています。

```css
navigationpage {
    -xf-bar-background-color: lightgray;
}

^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

Xamarin.Forms では、CSS スタイル シートはコンパイル時ではなく、実行時に解析・評価され、使用時に再解析されます。

> [!NOTE]
> 現時点では、XAML のスタイルを使ってできるすべてのスタイリングが、CSS で表現できるわけではありません。 しかし、XAML のスタイルは、Xamarin.Forms で現在サポートされていないプロパティの CSS を補足するために使用できます。 XAML スタイルの詳細については、「[XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)」をご覧ください。

[Monkeyappcss](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)サンプルでは、単純なアプリのスタイルを CSS で使用する方法を示しています。次のスクリーンショットを参照してください。

[![CSS スタイルが設定される MonkeyApp メインページ](css-images/MonkeyAppMainPage.png "CSS スタイルが設定される MonkeyApp メインページ")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS スタイルが設定される MonkeyApp メインページ")

[![CSS スタイルが設定される MonkeyApp の詳細ページ](css-images/MonkeyAppDetailPage.png "CSS スタイルが設定される MonkeyApp の詳細ページ")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS スタイルが設定される MonkeyApp の詳細ページ")

## <a name="consuming-a-style-sheet"></a>スタイル シートの使用

スタイル シートをソリューションに追加する手順は次のとおりです。

1. .NET Standard ライブラリ プロジェクトに空の CSS ファイルを追加します。
1. CSS ファイルのビルドアクションを**EmbeddedResource**に設定します。

### <a name="loading-a-style-sheet"></a>スタイル シートの読み込み

スタイル シートの読み込みに使用できるさまざまな方法があります。

### <a name="xaml"></a>XAML

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加する前に、 [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet)クラスを使用してスタイルシートを読み込んで解析することができます。

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

[`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source)プロパティは、スタイルシートを、外側の XAML ファイルの場所に対する相対 uri として指定します。または、uri が `/`で始まる場合は、プロジェクトのルートからの相対 uri として指定します。

> [!WARNING]
> ビルドアクションが**EmbeddedResource**に設定されていない場合、CSS ファイルの読み込みに失敗します。

また、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加する前に、 [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet)クラスを使用してスタイルシートを読み込んで解析することもできます。そのためには、`CDATA` セクションにインライン展開します。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

リソースディクショナリの詳細については、「[リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

### <a name="c"></a>C\#

でC#は、スタイルシートを `StringReader` から読み込み、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加することができます。

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

`StyleSheet.FromReader` メソッドの引数は、スタイルシートを読み取った `TextReader` です。

## <a name="selecting-elements-and-applying-properties"></a>要素の選択とプロパティの適用

CSS は、セレクターを使用して対象とする要素を決定します。 セレクターが一致するスタイルは、定義した順序で順番に適用されます。 特定のアイテムに定義されているスタイルは、常に最後に適用されます。 サポートされているセレクターの詳細については、「 [Selector Reference](#selector-reference)」を参照してください。

CSS は、プロパティを使用して選択した要素のスタイルを設定します。 各プロパティは有効な値のセットを持ち、一部のプロパティは任意の要素の型に影響を与えることができます。また一方で要素のグループに適用されるものもあります。 サポートされるプロパティの詳細については、「[プロパティリファレンス](#property-reference)」を参照してください。

### <a name="selecting-elements-by-type"></a>型による要素の選択

ビジュアルツリー内の要素は、大文字小文字を区別しない `element` セレクターを使用して、型で選択できます。

```css
stacklayout {
    margin: 20;
}
```

このセレクターは、スタイルシートを使用するページ上の[`StackLayout`](xref:Xamarin.Forms.StackLayout)要素を識別し、それらの余白を均一の太さで20に設定します。

> [!NOTE]
> `element` セレクターは、指定された型のサブクラスを識別しません。

### <a name="selecting-elements-by-base-class"></a>基本クラスによる要素の選択

ビジュアルツリー内の要素は、大文字小文字を区別しない `^base` セレクターを使用して、基本クラスで選択できます。

```css
^contentpage {
    background-color: lightgray;
}
```

このセレクターは、スタイルシートを使用する[`ContentPage`](xref:Xamarin.Forms.ContentPage)要素を識別し、それらの背景色を `lightgray`に設定します。

> [!NOTE]
> `^base` セレクターは、Xamarin. Forms に固有のものであり、CSS 仕様には含まれていません。

### <a name="selecting-an-element-by-name"></a>名前による要素の選択

大文字と小文字を区別する `#id` セレクターを使用して、ビジュアルツリー内の個々の要素を選択できます。

```css
#listView {
    background-color: lightgray;
}
```

このセレクターは、 [`StyleId`](xref:Xamarin.Forms.Element.StyleId)プロパティが `listView`に設定されている要素を識別します。 ただし、`StyleId` プロパティが設定されていない場合、セレクターは要素の `x:Name` を使用するようにフォールバックします。 したがって、次の XAML の例では、`#listView` セレクターは `x:Name` 属性が `listView`に設定されている[`ListView`](xref:Xamarin.Forms.ListView)を識別し、その背景色を `lightgray`に設定します。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>特定のクラス属性を使った要素の選択

特定のクラス属性を持つ要素を選択するには、大文字と小文字を区別する `.class` セレクターを使用します。

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

CSS クラスは、要素の[`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)プロパティを css クラス名に設定することによって、XAML 要素に割り当てることができます。 したがって、次の XAML の例では、`.detailPageTitle` クラスによって定義されたスタイルが最初の[`Label`](xref:Xamarin.Forms.Label)に割り当てられ、`.detailPageSubtitle` クラスで定義されているスタイルが2番目の `Label`に割り当てられます。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>子要素を選択します。

大文字と小文字を区別しない `element element` セレクターを使用して、ビジュアルツリー内の子要素を選択できます。

```css
listview image {
    height: 60;
    width: 60;
}
```

このセレクターは、 [`ListView`](xref:Xamarin.Forms.ListView)要素の子である[`Image`](xref:Xamarin.Forms.Image)要素を識別し、それらの高さと幅を60に設定します。 したがって、次の XAML の例では、`listview image` セレクターは[`ListView`](xref:Xamarin.Forms.ListView)の子である[`Image`](xref:Xamarin.Forms.Image)を識別し、その高さと幅を60に設定します。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> `element element` セレクターでは、子要素が親の_直接_の子である必要はありません。子要素は、親が異なる場合があります。 選択は、その先祖が指定された最初の要素と一致する場合に発生します。

### <a name="selecting-direct-child-elements"></a>直接の子要素を選択します。

ビジュアルツリー内の直接の子要素は、大文字小文字を区別しない `element>element` セレクターを使用して選択できます。

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

このセレクターは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)要素の直接の子である[`Image`](xref:Xamarin.Forms.Image)要素を識別し、それらの高さと幅を200に設定します。 したがって、次の XAML の例では、`stacklayout>image` セレクターは[`StackLayout`](xref:Xamarin.Forms.StackLayout)の直接の子である[`Image`](xref:Xamarin.Forms.Image)を識別し、その高さと幅を200に設定します。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> `element>element` セレクターを選択するには、子要素が親の_直接_の子である必要があります。

## <a name="selector-reference"></a>セレクター リファレンス

Xamarin.Forms では、次の CSS セレクターがサポートされています。

|セレクター|例|説明|
|---|---|---|
|`.class`|`.header`|' Header ' を含む `StyleClass` プロパティを持つすべての要素を選択します。 このセレクターは、大文字小文字を区別することに注意してください。|
|`#id`|`#email`|`StyleId` が `email`に設定されているすべての要素を選択します。 `StyleId` が設定されていない場合は、`x:Name`にフォールバックします。 XAML を使用する場合は、`StyleId`よりも `x:Name` が優先されます。 このセレクターは、大文字小文字を区別することに注意してください。|
|`*`|`*`|すべての要素を選択します。|
|`element`|`label`|`Label`型のすべての要素を選択しますが、サブクラスは選択しません。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`^base`|`^contentpage`|`ContentPage` 自体を含む基本クラスとして `ContentPage` を持つすべての要素を選択します。 このセレクターは大文字と小文字を区別せず、また CSS の仕様の一部ではないことに注意してください。|
|`element,element`|`label,button`|すべての `Button` 要素とすべての `Label` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element element`|`stacklayout label`|`StackLayout`内のすべての `Label` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element>element`|`stacklayout>label`|`StackLayout` を持つすべての `Label` 要素を直接の親として選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element+element`|`label+entry`|`Label`の直後にすべての `Entry` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element~element`|`label~entry`|`Label`の前にあるすべての `Entry` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|

セレクターが一致するスタイルは、定義した順序で順番に適用されます。 特定のアイテムに定義されているスタイルは、常に最後に適用されます。

> [!TIP]
> セレクターは、`StackLayout>ContentView>label.email`などの制限なしで組み合わせることができます。

次のセレクターは、現在サポートされていません。

- `[attribute]`
- `@media` および `@supports`
- `:` および `::`

> [!NOTE]
> 詳細度と詳細度の上書きはサポートされていません。

## <a name="property-reference"></a>プロパティの参照

次の CSS プロパティは、Xamarin. Forms によってサポートされています ( **[値]** 列の型は_斜体_ですが、文字列リテラルは `gray`)。

|プロパティ|状態名|値|例|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_色_\| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_文字列_\| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`、`Frame`、`ImageButton`|_色_\| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`、`Button`、`Frame`, `ImageButton`|_ダブル_\| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_ダブル_\| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`、`BoxView`、`Button`、`CheckBox`、`DatePicker`、`Editor`、`Entry`、`Label`、`Picker`、`ProgressBar`、`SearchBar`、`Switch`、`TimePicker`|_色_\| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_ダブル_\| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`。 さらに、0 ~ 100% の範囲のパーセンテージを `%` 記号と共に指定できます。|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_float_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_float_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_文字列_\| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_double_\| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_ダブル_\| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`letter-spacing`|`Button`、`DatePicker`、`Editor`、`Entry`、`Label`、`Picker`、`SearchBar`、`SearchHandler`、`Span`、`TimePicker`|_ダブル_\| `initial`|`letter-spacing: 2.5;`|
|`line-height`|`Label`, `Span`|_ダブル_\| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_太さ_\| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_太さ_\| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_太さ_\| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_太さ_\| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_太さ_\| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_ダブル_\| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_ダブル_\| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_ダブル_\| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_int_ \| `initial`|`order: -1;`|
|`padding`|`Button`、`ImageButton`、`Layout`, `Page`|_太さ_\| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`、`ImageButton`、`Layout`, `Page`|_ダブル_\| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`、`ImageButton`、`Layout`, `Page`| _ダブル_\| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`、`ImageButton`、`Layout`, `Page`| _ダブル_\| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`、`ImageButton`、`Layout`, `Page`| _ダブル_\| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _ダブル_\| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`、`EntryCell`、`Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`。 `left` と `right` は、右から左に記述する環境では避ける必要があります。| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`、`rotate`、`rotateX`、`rotateY`、`scale`、`scaleX`、`scaleY`、`translate`、`translateX`、`translateY`、`initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _double_、 _double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial`|`visibility: hidden;`|
|`width`|`VisualElement`|_ダブル_\| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` は、すべてのプロパティに対して有効な値です。 別のスタイルが設定された値をクリア(既定値にリセット)します。

次のプロパティは、現在サポートされていません。

- [https://login.microsoftonline.com/consumers/](`all: initial`)
- レイアウト プロパティ (box または grid)。
- `font`、`border`などの短縮形のプロパティ。

また、`inherit` 値はないため、継承はサポートされません。 したがって、たとえば、レイアウトの `font-size` プロパティを設定し、レイアウト内のすべての[`Label`](xref:Xamarin.Forms.Label)インスタンスが値を継承することを想定することはできません。 1つの例外は `direction` プロパティで、既定値は `inherit`です。

`Span` 要素をターゲットにすることは既知の問題であり、要素と名前の両方で CSS スタイルのターゲットになることを防ぐことができます (`#` シンボルを使用)。 `Span` 要素は `GestureElement`から派生します。これには `StyleClass` プロパティがないため、span では CSS クラスのターゲット設定がサポートされません。 詳細については、「 [Span コントロールに CSS スタイルを適用できない](https://github.com/xamarin/Xamarin.Forms/issues/5979)」を参照してください。

### <a name="xamarinforms-specific-properties"></a>Xamarin. フォーム固有のプロパティ

次の Xamarin. Forms 固有の CSS プロパティもサポートされています ( **[値]** 列では、型は_斜体_ですが、文字列リテラルは `gray`)。

|プロパティ|状態名|値|例|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_色_\| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_色_\| `initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`|_int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_色_\| `initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_色_\| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`。 `both` は `ScrollView`でのみサポートされています。 |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`、`Editor`、`SearchBar`|_引用符で囲ま_れたテキスト \| `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`、`Editor`、`SearchBar`|_色_\| `initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_ダブル_\| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_色_\| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_文字列_\| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>Xamarin. フォームシェル固有のプロパティ

次の Xamarin. フォームシェル固有の CSS プロパティもサポートされています ( **[値]** 列では、型は_斜体_ですが、文字列リテラルは `gray`)。

|プロパティ|状態名|値|例|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_色_\| `initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_色_\| `initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_色_\| `initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_色_\| `initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_色_\| `initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_色_\| `initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_色_\| `initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_色_\| `initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_色_\| `initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_色_\| `initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_色_\| `initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>色

次の `color` の値がサポートされています。

- CSS の色、UWP の定義済みの色、および Xamarin 形式の色と一致する[色](https://en.wikipedia.org/wiki/X11_color_names)を `X11` します。 これらの色値は大文字小文字を区別しないことに注意してください。
- 16進数の色: `#rgb`、`#argb`、`#rrggbb`、`#aarrggbb`
- rgb 色: `rgb(255,0,0)`、`rgb(100%,0%,0%)`。 値は 0 ~ 255 0 ~ 100% の範囲です。
- rgba 色: `rgba(255, 0, 0, 0.8)`、`rgba(100%, 0%, 0%, 0.8)`。 不透明度の値は 0.0 ~ 1.0 の範囲です。
- hsl の色: `hsl(120, 100%, 50%)`。 H の値は 0 ~ 360 の範囲で s と l は範囲 0 ~ 100% です。
- hsla の色: `hsla(120, 100%, 50%, .8)`します。 不透明度の値は 0.0 ~ 1.0 の範囲です。

### <a name="thickness"></a>太さ

1、2、3、または4つの `thickness` 値がサポートされており、それぞれが空白で区切られています。

- 1 つの値では、均一な厚みを示します。
- 2 つの値は、上下・左右の太さを示します。
- 3 つの値は、上・左右・下の太さを示します。
- 4 つの値は、上・右・下・左の太さを示します。

> [!NOTE]
> CSS `thickness` の値は、XAML [`Thickness`](xref:Xamarin.Forms.Thickness)の値とは異なります。 たとえば、XAML では、2つの値の `Thickness` は水平方向の幅を示し、4つの値の `Thickness` は左、次に上、次に右、次に下の太さを示します。 また、XAML `Thickness` の値はコンマで区切られます。

### <a name="namedsize"></a>NamedSize

次の大文字と小文字を区別しない `namedsize` 値がサポートされています。

- `default`
- `micro`
- `small`
- `medium`
- `large`

各 `namedsize` 値の正確な意味は、プラットフォームに依存し、ビューに依存します。

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin.Forms CSS を Xamarin.University で

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin. Forms 3.0 CSS ビデオ**

## <a name="related-links"></a>関連リンク

- [MonkeyAppCSS (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
