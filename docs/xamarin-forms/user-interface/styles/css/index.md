---
title: カスケード スタイル シート (CSS) を使用した Xamarin.Forms アプリのスタイル設定
description: Xamarin.Forms では、カスケード スタイル シート (CSS) を使用した視覚要素のスタイリングをサポートします。
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>カスケード スタイル シート (CSS) を使用した Xamarin.Forms アプリのスタイル設定

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)

_Xamarin.Forms では、カスケード スタイル シート (CSS) を使用した視覚要素のスタイリングをサポートします。_

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
> 現時点では、XAML のスタイルを使ってできるすべてのスタイリングが、CSS で表現できるわけではありません。 しかし、XAML のスタイルは、Xamarin.Forms で現在サポートされていないプロパティの CSS を補足するために使用できます。 XAML のスタイルの詳細については、次を参照してください。[XAML スタイルを使った Xamarin.Forms アプリのスタイリング](~/xamarin-forms/user-interface/styles/xaml/index.md)。

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) サンプルは、CSS を使用して簡単なアプリのスタイルを設定するデモで、次のスクリーン ショットのように表示されます。

[![CSS スタイルのメイン ページを MonkeyApp](css-images/MonkeyAppMainPage.png "MonkeyApp のメイン ページで CSS スタイル")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS スタイルを持つ MonkeyApp メイン ページ")

[![CSS のスタイル設定の詳細ページを MonkeyApp](css-images/MonkeyAppDetailPage.png "CSS スタイルの詳細ページを MonkeyApp")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS スタイルを持つ MonkeyApp 詳細ページ")

## <a name="consuming-a-style-sheet"></a>スタイル シートの使用

スタイル シートをソリューションに追加する手順は次のとおりです。

1. .NET Standard ライブラリ プロジェクトに空の CSS ファイルを追加します。
1. CSS ファイルのビルド アクションを **EmbeddedResource** に設定します。

### <a name="loading-a-style-sheet"></a>スタイル シートの読み込み

スタイル シートの読み込みに使用できるさまざまな方法があります。

### <a name="xaml"></a>XAML

スタイル シートの読み込みおよびで解析できる、 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet)クラスに追加される前に、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) プロパティは、XAML ファイルを含む位置からの相対 URI、また `/` から始まる URI の場合はプロジェクトのルートからの相対 URI としてスタイルシートを指定します。

> [!WARNING]
> CSS ファイルには、ビルド アクションに設定されていない場合の読み込みが失敗**EmbeddedResource**します。

スタイル シートを読み込むし、で解析または、 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet)に追加される前に、クラス、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)により、インライン展開で、`CDATA`セクション。

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

リソース ディクショナリの詳細については、次を参照してください。[リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)します。

### <a name="c"></a>C#

C#、スタイル シートを埋め込みリソースとして読み込まれに追加できる、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

1 番目の引数 `StyleSheet.FromAssemblyResource` メソッドは、スタイル シートを含んでいるアセンブリで、2 番目の引数は、リソース識別子を表す `string` です。 リソース識別子は、CSS ファイルを選択したときに、**プロパティ** ウィンドウから取得することができます。

スタイル シートを読み込むことができます、また、`StringReader`に追加し、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

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

`StyleSheet.FromReader` メソッドの引数は、スタイルシートを読み込ませた `TextReader` です。

## <a name="selecting-elements-and-applying-properties"></a>要素の選択とプロパティの適用

CSS は、セレクターを使用して対象とする要素を決定します。 セレクターが一致するスタイルは、定義した順序で順番に適用されます。 特定のアイテムに定義されているスタイルは、常に最後に適用されます。 サポートされているセレクターの詳細については、[セレクター参照](#selector-reference) を参照してください。

CSS は、プロパティを使用して選択した要素のスタイルを設定します。 各プロパティは有効な値のセットを持ち、一部のプロパティは任意の要素の型に影響を与えることができます。また一方で要素のグループに適用されるものもあります。 サポートされているプロパティの詳細については、[プロパティ リファレンス](#property-reference) を参照してください。

### <a name="selecting-elements-by-type"></a>型による要素の選択

大文字と小文字を区別しない `element` セレクターを使うと、ビジュアルツリー内の要素を型を指定して選択することができます。

```css
stacklayout {
    margin: 20;
}
```

このセレクタは、スタイルシートを適用したページ上のすべての [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 要素を特定し、余白を均一20に設定します。

> [!NOTE]
> `element` セレクターは、指定した型のサブクラスを特定することはできません。

### <a name="selecting-elements-by-base-class"></a>基本クラスによる要素の選択

大文字と小文字を区別しない `^base` セレクターを使うと、ビジュアルツリー内の要素を基本クラスを指定して選択することができます。

```css
^contentpage {
    background-color: lightgray;
}
```

このセレクターは、スタイルシートを適用したページ上のすべての [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) を特定し、背景色を `lightgray` に設定します。

> [!NOTE]
> `^base`セレクターは、Xamarin.Forms 固有のものであり、CSS 仕様の一部ではありません。

### <a name="selecting-an-element-by-name"></a>名前による要素の選択

大文字と小文字を区別する `#id` セレクターを使うと、ビジュアルツリー内の個々の要素を選択することができます。

```css
#listView {
    background-color: lightgray;
}
```

このセレクターは、[ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) プロパティに `listView` が設定されている要素を特定します。 ただし、`StyleId` プロパティが設定されていない場合、セレクターは要素の `x:Name` を使ってフォールバックします。 したがって、次の XAML の例では、`#listView` セレクターは、`x:Name` 属性に `listView` が設定されている [ `ListView` ](xref:Xamarin.Forms.ListView) を特定し、背景色を `lightgray` に設定します。

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

大文字と小文字を区別する `.class` セレクターを使うと、特定のクラス属性を持つ要素を選択できます。

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

CSS クラスは、要素の [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) プロパティに CSS クラス名を設定して、XAML 要素に 割り当てることができます。 したがって、次の XAML の例では、`.detailPageTitle` クラスによって定義されたスタイルが、最初の [ `Label` ](xref:Xamarin.Forms.Label) に割り当てられ、`.detailPageSubtitle` クラスは 2 番目の `Label` に割り当てられます。

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

大文字と小文字を区別しない `element element` セレクターを使うと、ビジュアル ツリー内の子要素を選択できます。

```css
listview image {
    height: 60;
    width: 60;
}
```

このセレクターは、[ `ListView` ](xref:Xamarin.Forms.ListView) 要素の子であるすべての [ `Image` ](xref:Xamarin.Forms.Image) 要素を特定し、高さと幅を 60 に設定します。 したがって、次の XAML の例では、`listview image` セレクターは、[ `ListView` ](xref:Xamarin.Forms.ListView) の子である [ `Image` ](xref:Xamarin.Forms.Image) を特定し、高さと幅を 60 に設定します。

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
> `element element` セレクターは、子要素が親の_直接_の子である必要はありません。子要素は異なる親を持つ可能性があります。 選択は、その先祖が指定された最初の要素と一致する場合に発生します。

### <a name="selecting-direct-child-elements"></a>直接の子要素を選択します。

大文字と小文字を区別しない `element>element` セレクターを使うと、ビジュアル ツリー内の直接の子要素を選択することができます。

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

このセレクターは、[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 要素の直接の子であるすべての [ `Image` ](xref:Xamarin.Forms.Image) 要素を特定し、高さと幅を 200 に設定します。 したがって、次の XAML の例では、`stacklayout>image` セレクターは、[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) の直接の子である [ `Image` ](xref:Xamarin.Forms.Image) を特定し、高さと幅を 200 に設定します。

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
> `element>element` セレクターは、子要素が親の_直接_の子である必要があります。

## <a name="selector-reference"></a>セレクター リファレンス

Xamarin.Forms では、次の CSS セレクターがサポートされています。

|[セレクター]|例|説明|
|---|---|---|
|`.class`|`.header`|`StyleClass` プロパティに `header` を含むすべての要素を選択します。 このセレクターは、大文字小文字を区別することに注意してください。|
|`#id`|`#email`|`StyleId` に `email` が設定されているすべての要素を選択します。 `StyleId` が設定されていない場合は、`x:Name` でフォールバックします。 XAML を使用する場合は、`StyleId` より `x:Name`が優先されます。 このセレクターは、大文字小文字を区別することに注意してください。|
|`*`|`*`|すべての要素を選択します。|
|`element`|`label`|`Label` 型の要素をすべて選択します。ただしサブクラスは選択しません。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`^base`|`^contentpage`|基本クラスが `ContentPage` であるすべての要素を選択します。これには `ContentPage` 自体も含みます。 このセレクターは大文字と小文字を区別せず、また CSS の仕様の一部ではないことに注意してください。|
|`element,element`|`label,button`|すべての `Button` 要素とすべての `Label` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element element`|`stacklayout label`|`StackLayout` 内のすべての `Label` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element>element`|`stacklayout>label`|`StackLayout` を直接の親として持つすべての `Label` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element+element`|`label+entry`|`Label` の後に隣接するすべての `Entry`  要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element~element`|`label~entry`|`Label` に続くすべての `Entry` 要素を選択します。 このセレクターは、大文字小文字を区別しないことに注意してください。|

セレクターが一致するスタイルは、定義した順序で順番に適用されます。 特定のアイテムに定義されているスタイルは、常に最後に適用されます。

> [!TIP]
> セレクターは、`StackLayout>ContentView>label.email` のように無制限に組み合わせることができます。

次のセレクターは、現在サポートされていません。

- `[attribute]`
- `@media` および `@supports`
- `:` および `::`

> [!NOTE]
> 詳細度と詳細度の上書きはサポートされていません。

## <a name="property-reference"></a>プロパティ リファレンス

Xamarin.Forms では次の CSS プロパティがサポートされています。 ( **値** 列の型は _斜体_ で文字列リテラルは `gray` で表記します)

|プロパティ|対象|値|例|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_color_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_string_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`、 `Frame`、 `ImageButton`|_color_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_double_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_double_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_color_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_double_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`します。 さらに、パーセンテージで、範囲 0 ~ 100% を指定できます、`%`サインオンします。|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_浮動小数点数_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_浮動小数点数_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_string_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_double_  \| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`line-height`|`Label`, `Span`|_double_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_thickness_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_thickness_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_thickness_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_thickness_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_thickness_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_Int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_Int_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_thickness_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _double_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left` と `right` は 文字方向が右から左の環境においては避けるべきです。| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _二重_、 _double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_double_ \| `initial`|`min-width: 320;`|

次の Xamarin.Forms 特定 CSS プロパティがサポートされても (で、**値**列、型は_斜体_文字列リテラルは、 `gray`)。

|プロパティ|対象|値|例|
|---|---|---|---|
|`-xf-placeholder`|`Entry`、 `Editor`、 `SearchBar`|_引用符で囲まれたテキスト_ \| `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`、 `Editor`、 `SearchBar`|_color_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-max-length`|`Entry`, `Editor`|_Int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_color_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_color_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` のみサポートされますが、`ScrollView`します。 |`-xf-orientation: horizontal;`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-min-track-color`|`Slider`|_color_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-max-track-color`|`Slider`|_color_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-thumb-color`|`Slider`|_color_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-spacing`|`StackLayout`|_double_ \| `initial` |`-xf-spacing: 8;`|

> [!NOTE]
> `initial` は、すべてのプロパティで有効な値です。 別のスタイルが設定された値をクリア(既定値にリセット)します。

次のプロパティは、現在サポートされていません。

- `all: initial`。
- レイアウト プロパティ (box または grid)。
- `font` や `border` などの短縮形のプロパティ。

さらに、`inherit` 値は存在しないため、継承はサポートされていません。 したがって、たとえば、`font-size` プロパティをレイアウトに設定し、そのレイアウト内のすべての [ `Label` ](xref:Xamarin.Forms.Label) インスタンスにその値が継承されることは期待できません。 1つの例外として、`direction` プロパティがあり、それは `inherit` の規定値を持ちます。

### <a name="color"></a>色

次のような `color` 値がサポートされます。

- `X11` [colors](https://en.wikipedia.org/wiki/X11_color_names/) は、CSS の色、UWP 定義済みの色、および Xamarin.Forms の色に一致します。 これらの色値は大文字小文字を区別しないことに注意してください。
- 16 進数の色: `#rgb`、 `#argb`、 `#rrggbb`、 `#aarrggbb`
- rgb 色: `rgb(255,0,0)`、`rgb(100%,0%,0%)`。 値は 0 ~ 255 0 ~ 100% の範囲です。
- rgba 色: `rgba(255, 0, 0, 0.8)`、`rgba(100%, 0%, 0%, 0.8)`。 不透明度の値は 0.0 ~ 1.0 の範囲です。
- hsl の色:`hsl(120, 100%, 50%)`です。 H の値は 0 ~ 360 の範囲で s と l は範囲 0 ~ 100% です。
- hsla のカラー:`hsla(120, 100%, 50%, .8)`します。 不透明度の値は 0.0 ~ 1.0 の範囲です。

### <a name="thickness"></a>太さ

1、2、3、または 4つの `thickness` 値がサポートされています。各値は空白で区切ります。

- 1 つの値では、均一な厚みを示します。
- 2 つの値は、上下・左右の太さを示します。
- 3 つの値は、上・左右・下の太さを示します。
- 4 つの値は、上・右・下・左の太さを示します。

> [!NOTE]
> CSS の `thickness` 値と XAML の [ `Thickness` ](xref:Xamarin.Forms.Thickness) 値は異なります。 たとえば、XAML では、2 つの値の `Thickness` は左右・上下の順で、4 つの値の `Thickness` は、左・上・右・下の順で太さを示します。 さらに、XAML の `Thickness` 値は、コンマで区切られます。

### <a name="namedsize"></a>NamedSize

次のような大文字と小文字を区別する `namedsize` 値がサポートされています。

- `default`
- `micro`
- `small`
- `medium`
- `large`

それぞれの `namedsize` 値 の厳密な意味は、プラットフォームやビューに依存します。

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin.Forms CSS を Xamarin.University で

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**[Xamarin University](https://university.xamarin.com/) による Xamarin.Forms 3.0 CSS**

## <a name="related-links"></a>関連リンク

- [MonkeyAppCSS (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
