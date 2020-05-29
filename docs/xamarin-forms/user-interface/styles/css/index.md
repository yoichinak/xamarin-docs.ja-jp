---
title: Xamarin.Formsカスケードスタイルシートを使用したアプリのスタイル設定 (CSS)
description: Xamarin.Formsカスケードスタイルシート (CSS) を使用したビジュアル要素のスタイル設定をサポートします。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3e333fcdca12a6bd2f71a1ecbeeb1fd3d5f5be13
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140141"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Xamarin.Formsカスケードスタイルシートを使用したアプリのスタイル設定 (CSS)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin は、カスケードスタイルシート (CSS) を使用したビジュアル要素のスタイル設定をサポートしています。_

Xamarin.Formsアプリケーションは、CSS を使用してスタイルを設定できます。 スタイルシートは、ルールの一覧で構成され、各ルールは1つ以上のセレクターと宣言ブロックで構成されます。 宣言ブロックは、中かっこで囲まれた宣言のリストで構成され、各宣言はプロパティ、コロン、および値で構成されます。 1つのブロックに複数の宣言がある場合、セミコロンは区切り記号として挿入されます。 次のコード例は、準拠している CSS を示してい Xamarin.Forms ます。

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

では、 Xamarin.Forms CSS スタイルシートはコンパイル時ではなく実行時に解析および評価され、使用時にスタイルシートが再解析されます。

> [!NOTE]
> 現時点では、XAML スタイルで可能なすべてのスタイル設定を CSS で実行することはできません。 ただし、XAML スタイルを使用して、で現在サポートされていないプロパティの CSS を補うことができ Xamarin.Forms ます。 XAML スタイルの詳細については、「 [ Xamarin.Forms xaml スタイルを使用したアプリのスタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)設定」を参照してください。

[Monkeyappcss](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)サンプルでは、単純なアプリのスタイルを CSS で使用する方法を示しています。次のスクリーンショットを参照してください。

[![CSS スタイルが設定される MonkeyApp メインページ](css-images/MonkeyAppMainPage.png "CSS スタイルが設定される MonkeyApp メインページ")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS スタイルが設定される MonkeyApp メインページ")

[![CSS スタイルが設定される MonkeyApp の詳細ページ](css-images/MonkeyAppDetailPage.png "CSS スタイルが設定される MonkeyApp の詳細ページ")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS スタイルが設定される MonkeyApp の詳細ページ")

## <a name="consuming-a-style-sheet"></a>スタイルシートの使用

ソリューションにスタイルシートを追加するには、次の手順を実行します。

1. 空の CSS ファイルを .NET Standard ライブラリプロジェクトに追加します。
1. CSS ファイルのビルドアクションを**EmbeddedResource**に設定します。

### <a name="loading-a-style-sheet"></a>スタイルシートの読み込み

スタイルシートの読み込みに使用できる方法はいくつかあります。

> [!NOTE]
> 現在、実行時にスタイルシートを変更し、新しいスタイルシートを適用することはできません。

### <a name="xaml"></a>XAML

スタイルシートは [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) 、に追加する前に、クラスで読み込んで解析することができ [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

プロパティは、 [`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) 外側の XAML ファイルの位置からの相対 uri としてスタイルシートを指定します。または、uri がで始まる場合は、プロジェクトのルートからの相対 uri として指定し `/` ます。

> [!WARNING]
> ビルドアクションが**EmbeddedResource**に設定されていない場合、CSS ファイルの読み込みに失敗します。

または、スタイルシートをに追加する前に、クラスを使用してスタイルシートを読み込んで解析することもできます。これを行うには、 [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) セクションにインライン展開し `CDATA` ます。

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

C# では、スタイルシートをから読み込んで、に追加することができ `StringReader` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。

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

メソッドの引数は `StyleSheet.FromReader` 、 `TextReader` スタイルシートを読み取ったです。

## <a name="selecting-elements-and-applying-properties"></a>要素の選択とプロパティの適用

CSS はセレクターを使用して、対象とする要素を決定します。 セレクターが一致するスタイルは、定義の順序で連続して適用されます。 特定の項目に定義されているスタイルは、常に最後に適用されます。 サポートされているセレクターの詳細については、「 [Selector Reference](#selector-reference)」を参照してください。

CSS では、プロパティを使用して、選択した要素のスタイルを適用します。 各プロパティには使用可能な値のセットがあり、一部のプロパティは要素の型に影響を与えますが、他のプロパティは要素のグループに適用されます。 サポートされるプロパティの詳細については、「[プロパティリファレンス](#property-reference)」を参照してください。

> [!IMPORTANT]
> CSS 変数はサポートされていません。

### <a name="selecting-elements-by-type"></a>種類別の要素の選択

ビジュアルツリー内の要素は、大文字と小文字を区別しないセレクターを使用して、型で選択でき `element` ます。

```css
stacklayout {
    margin: 20;
}
```

このセレクターは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) スタイルシートを使用するページ上のすべての要素を識別し、それらの余白を均一の太さ20に設定します。

> [!NOTE]
> セレクターは、 `element` 指定された型のサブクラスを識別しません。

### <a name="selecting-elements-by-base-class"></a>基本クラスによる要素の選択

ビジュアルツリー内の要素は、大文字と小文字を区別しないセレクターを使用して、基本クラスで選択でき `^base` ます。

```css
^contentpage {
    background-color: lightgray;
}
```

このセレクターは、 [`ContentPage`](xref:Xamarin.Forms.ContentPage) スタイルシートを使用するすべての要素を識別し、その背景色をに設定し `lightgray` ます。

> [!NOTE]
> `^base`セレクターはに固有であり、 Xamarin.Forms CSS 仕様には含まれていません。

### <a name="selecting-an-element-by-name"></a>名前による要素の選択

大文字と小文字を区別するセレクターを使用して、ビジュアルツリー内の個々の要素を選択でき `#id` ます。

```css
#listView {
    background-color: lightgray;
}
```

このセレクター [`StyleId`](xref:Xamarin.Forms.Element.StyleId) は、プロパティがに設定されている要素を識別 `listView` します。 ただし、 `StyleId` プロパティが設定されていない場合、セレクターは要素のを使用するようにフォールバックし `x:Name` ます。 したがって、次の XAML の例では、 `#listView` セレクターが属性がに設定されているを識別 [`ListView`](xref:Xamarin.Forms.ListView) し、 `x:Name` `listView` 背景色をに設定し `lightgray` ます。

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

### <a name="selecting-elements-with-a-specific-class-attribute"></a>特定のクラス属性を持つ要素の選択

特定のクラス属性を持つ要素は、大文字と小文字を区別するセレクターで選択でき `.class` ます。

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

Css クラスは、 [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) 要素のプロパティを css クラス名に設定することによって、XAML 要素に割り当てることができます。 したがって、次の XAML の例では、クラスで定義されているスタイルが最初のに割り当てられ、クラスで定義されている `.detailPageTitle` [`Label`](xref:Xamarin.Forms.Label) スタイルが `.detailPageSubtitle` 2 番目のに割り当てられ `Label` ます。

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

### <a name="selecting-child-elements"></a>子要素の選択

大文字と小文字を区別しないセレクターを使用して、ビジュアルツリー内の子要素を選択でき `element element` ます。

```css
listview image {
    height: 60;
    width: 60;
}
```

このセレクター [`Image`](xref:Xamarin.Forms.Image) は、要素の子である要素を識別 [`ListView`](xref:Xamarin.Forms.ListView) し、それらの高さと幅を60に設定します。 したがって、次の XAML の例では、セレクターがの `listview image` 子であるを識別し、その [`Image`](xref:Xamarin.Forms.Image) [`ListView`](xref:Xamarin.Forms.ListView) 高さと幅を60に設定します。

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
> このセレクターでは、 `element element` 子要素が親の_直接_の子である必要はありません。子要素は、親が異なる場合があります。 先祖が指定された最初の要素である場合、選択が行われます。

### <a name="selecting-direct-child-elements"></a>直接の子要素の選択

ビジュアルツリー内の直接の子要素は、大文字と小文字を区別しないセレクターで選択でき `element>element` ます。

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

このセレクター [`Image`](xref:Xamarin.Forms.Image) は、要素の直接の子である要素を識別 [`StackLayout`](xref:Xamarin.Forms.StackLayout) し、それらの高さと幅を200に設定します。 したがって、次の XAML の例では、セレクターがの `stacklayout>image` 直接の子であるを識別し、その [`Image`](xref:Xamarin.Forms.Image) [`StackLayout`](xref:Xamarin.Forms.StackLayout) 高さと幅を200に設定します。

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
> `element>element`セレクターでは、子要素が親の_直接_の子である必要があります。

## <a name="selector-reference"></a>セレクターリファレンス

では、次の CSS セレクターがサポートされてい Xamarin.Forms ます。

|セレクター|例|説明|
|---|---|---|
|`.class`|`.header`|' Header ' を含むプロパティを持つすべての要素を選択 `StyleClass` します。 このセレクターでは大文字と小文字が区別されることに注意してください。|
|`#id`|`#email`|がに設定されているすべての要素を選択 `StyleId` `email` します。 `StyleId`が設定されていない場合は、にフォールバック `x:Name` します。 XAML を使用する場合 `x:Name` は、よりも優先され `StyleId` ます。 このセレクターでは大文字と小文字が区別されることに注意してください。|
|`*`|`*`|すべての要素を選択します。|
|`element`|`label`|型のすべての要素 `Label` を選択しますが、サブクラスは選択しません。 このセレクターは大文字と小文字が区別されないことに注意してください。|
|`^base`|`^contentpage`|を含むすべての要素 `ContentPage` を基底クラスとして選択し `ContentPage` ます。 このセレクターでは大文字と小文字が区別されず、CSS 仕様の一部ではないことに注意してください。|
|`element,element`|`label,button`|すべて `Button` の要素とすべて `Label` の要素を選択します。 このセレクターは大文字と小文字が区別されないことに注意してください。|
|`element element`|`stacklayout label`|`Label`内のすべての要素を選択 `StackLayout` します。 このセレクターは大文字と小文字が区別されないことに注意してください。|
|`element>element`|`stacklayout>label`|`Label`を直接の親として使用しているすべての要素を選択 `StackLayout` します。 このセレクターは大文字と小文字が区別されないことに注意してください。|
|`element+element`|`label+entry`|`Entry`の直後にあるすべての要素を選択 `Label` します。 このセレクターは大文字と小文字が区別されないことに注意してください。|
|`element~element`|`label~entry`|`Entry`の前にあるすべての要素を選択 `Label` します。 このセレクターは大文字と小文字が区別されないことに注意してください。|

セレクターが一致するスタイルは、定義の順序で連続して適用されます。 特定の項目に定義されているスタイルは、常に最後に適用されます。

> [!TIP]
> セレクターは、などの制限なしで組み合わせることができ `StackLayout>ContentView>label.email` ます。

現在、次のセレクターはサポートされていません。

- `[attribute]`
- `@media` および `@supports`
- `:` および `::`

> [!NOTE]
> 特異性、および特異性のオーバーライドはサポートされていません。

## <a name="property-reference"></a>プロパティ リファレンス

では、次の CSS プロパティがサポートされてい Xamarin.Forms ます ([**値**] 列では、型は_斜体_、文字列リテラルは `gray` )。

|プロパティ|適用対象|値|例|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_色_ \|`initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_string_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`, `ImageButton`|_色_ \|`initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_double_ \|`initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_double_ \|`initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_色_ \|`initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_double_ \|`initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \|`auto` \| `initial`. さらに、0 ~ 100% の範囲のパーセンテージを符号と共に指定でき `%` ます。|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_float_ \|`initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_float_ \|`initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_string_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_double_ \|_namedsize_ \|  `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double_ \|`initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`letter-spacing`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `SearchHandler`, `Span`, `TimePicker`|_double_ \|`initial`|`letter-spacing: 2.5;`|
|`line-height`|`Label`, `Span`|_double_ \|`initial` |`line-height: 1.8;`|
|`margin`|`View`|_太さ_ \|`initial` |`margin: 6 12;`|
|`margin-left`|`View`|_太さ_ \|`initial` |`margin-left: 3;`|
|`margin-top`|`View`|_太さ_ \|`initial` |`margin-top: 2;`|
|`margin-right`|`View`|_太さ_ \|`initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_太さ_ \|`initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_INT_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_double_ \|`initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double_ \|`initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double_ \|`initial` |`opacity: .3;`|
|`order`|`VisualElement`|_INT_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_太さ_ \|`initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_double_ \|`initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \|`initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \|`initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \|`initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _double_ \|`initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left``right`右から左に記述された環境では回避する必要があります。| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _double_、 _double_ \|`initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial`|`visibility: hidden;`|
|`width`|`VisualElement`|_double_ \|`initial`|`min-width: 320;`|

> [!NOTE]
> `initial`は、すべてのプロパティの有効な値です。 別のスタイルから設定された値 (既定値にリセット) をクリアします。

次のプロパティは現在サポートされていません。

- `all: initial`.
- レイアウトのプロパティ (ボックスまたはグリッド)。
- 、などの短縮形のプロパティ `font` `border` 。

また、値はない `inherit` ため、継承はサポートされません。 したがって、たとえば、レイアウトのプロパティを設定して、 `font-size` レイアウト内のすべてのインスタンスが値を継承することを想定することはできません [`Label`](xref:Xamarin.Forms.Label) 。 1つの例外は `direction` プロパティで、既定値は `inherit` です。

ターゲット `Span` 要素には既知の問題があり、範囲が CSS スタイルのターゲットになるのを防ぐために、要素と名前の両方 (記号を使用 `#` ) になります。 `Span`要素はから派生 `GestureElement` します。これにはプロパティがないため、 `StyleClass` SPAN では CSS クラスのターゲット設定がサポートされません。 詳細については、「 [Span コントロールに CSS スタイルを適用できない](https://github.com/xamarin/Xamarin.Forms/issues/5979)」を参照してください。

### <a name="xamarinforms-specific-properties"></a>Xamarin.Forms特定のプロパティ

次の Xamarin.Forms 特定の CSS プロパティもサポートされています ([**値**] 列では、型は_斜体_ですが、文字列リテラルは `gray` )。

|プロパティ|適用対象|値|例|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_色_ \|`initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_色_ \|`initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`, `SearchBar`|_INT_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_色_ \|`initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_色_ \|`initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both`は、でのみサポートされてい `ScrollView` ます。 |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|_引用符で囲まれたテキスト_ \|`initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_色_ \|`initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_double_ \|`initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_色_ \|`initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_string_ \| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>Xamarin.Formsシェル固有のプロパティ

次の Xamarin.Forms シェル固有の CSS プロパティもサポートされています ([**値**] 列では、型は_斜体_ですが、文字列リテラルは `gray` )。

|プロパティ|適用対象|値|例|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_色_ \|`initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_色_ \|`initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_色_ \|`initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_色_ \|`initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_色_ \|`initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_色_ \|`initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_色_ \|`initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_色_ \|`initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_色_ \|`initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_色_ \|`initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_色_ \|`initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>Color

次の `color` 値がサポートされています。

- `X11`CSS の色、UWP の定義済みの色、および色に一致する[色](https://en.wikipedia.org/wiki/X11_color_names) Xamarin.Forms 。 これらの色の値は大文字と小文字を区別しないことに注意してください。
- 16進数の色: `#rgb` 、、 `#argb` `#rrggbb` 、`#aarrggbb`
- rgb 色: `rgb(255,0,0)` 、 `rgb(100%,0%,0%)` 。 値の範囲は 0-255 ~ 0%-100% です。
- rgba 色: `rgba(255, 0, 0, 0.8)` 、 `rgba(100%, 0%, 0%, 0.8)` 。 不透明度の値は、0.0 ~ 1.0 の範囲で指定します。
- hsl の色: `hsl(120, 100%, 50%)` 。 H 値は0-360 の範囲にあり、s と l の範囲は 0%-100% です。
- hsla の色: `hsla(120, 100%, 50%, .8)` 。 不透明度の値は、0.0 ~ 1.0 の範囲で指定します。

### <a name="thickness"></a>太さ

1つ、2つ、3つ、または4つの `thickness` 値がサポートされており、それぞれが空白で区切られています。

- 1つの値が均一の太さを示します。
- 2つの値は、垂直方向の幅を示します。
- 3つの値は、top、横方向 (左と右)、下の太さを示します。
- 4つの値は、top、right、bottom、left の各太さを示します。

> [!NOTE]
> CSS `thickness` 値は XAML 値とは異なり [`Thickness`](xref:Xamarin.Forms.Thickness) ます。 たとえば、XAML では、2つの値は `Thickness` 水平方向の幅を示し、4つの値は `Thickness` left、top、right、bottom の各太さを示します。 また、XAML `Thickness` 値はコンマで区切られます。

### <a name="namedsize"></a>NamedSize

次の大文字と小文字を区別しない `namedsize` 値がサポートされています。

- `default`
- `micro`
- `small`
- `medium`
- `large`

各値の正確な意味 `namedsize` は、プラットフォームに依存し、ビューに依存します。

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin 大学での CSS Xamarin.Forms

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms3.0 CSS ビデオ**

## <a name="related-links"></a>関連リンク

- [MonkeyAppCSS (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin.FormsXAML スタイルを使用したアプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
