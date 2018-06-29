---
title: カスケード スタイル シート (CSS) を使用して Xamarin.Forms アプリのスタイルを設定
description: Xamarin.Forms では、カスケード スタイル シート (CSS) を使用してスタイル ビジュアル要素をサポートします。
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 76ca67f7ac8a8e27e5f502455d48874c775fc172
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794086"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>カスケード スタイル シート (CSS) を使用して Xamarin.Forms アプリのスタイルを設定

_Xamarin.Forms では、カスケード スタイル シート (CSS) を使用してスタイル ビジュアル要素をサポートします。_

Xamarin.Forms 3.0 には、CSS を使用してアプリのスタイルを設定する機能が導入されています。 スタイル シートは、1 つまたは複数のセレクターと宣言ブロックで構成される各規則と、規則の一覧で構成されます。 宣言ブロックは、プロパティ、コロン、および値から成る各宣言に、中かっこ内の宣言の一覧で構成されます。 ブロック内の複数の宣言が存在する場合、区切り記号としてセミコロンが挿入されます。 次のコード例は、Xamarin.Forms 一部準拠している CSS を示しています。

```css
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

Xamarin.Forms で CSS スタイル シートが解析され、コンパイル時ではなく、実行時に評価し、スタイル シートが使用するときに再解析します。

> [!NOTE]
> 現時点では、すべての XAML のスタイルを持つことが、スタイリング CSS を実行できません。 ただし、XAML のスタイルは、Xamarin.Forms で現在サポートされていないプロパティの CSS を補足するものを使用できます。 XAML のスタイルの詳細については、次を参照してください。[スタイル Xamarin.Forms を使ったアプリの XAML スタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)です。

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)サンプル CSS を使用して、簡単なアプリのスタイルを設定して、次のスクリーン ショットに表示されます。

[![CSS スタイルを持つ MonkeyApp メインページ](css-images/MonkeyAppMainPage.png "CSS スタイルを持つ MonkeyApp メインページ")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS スタイルを持つ MonkeyApp メイン ページ")

[![CSS スタイルを持つ MonkeyApp 詳細ページ](css-images/MonkeyAppDetailPage.png "CSS スタイルを持つ MonkeyApp 詳細ページ")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS スタイルを持つ MonkeyApp 詳細ページ")

> [!NOTE]
> 現在の背景色のスタイルを設定することはできません、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)スタイル シートを使用します。 したがって、サンプル アプリケーションで、 [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)コードでプロパティを設定します。

## <a name="consuming-a-style-sheet"></a>スタイル シートの使用

ソリューションに、スタイル シートを追加する手順は次のとおりです。

1. .NET 標準ライブラリ プロジェクトに空の CSS ファイルを追加します。
1. CSS ファイルのビルド アクション設定**EmbeddedResource**です。

### <a name="loading-a-style-sheet"></a>スタイル シートの読み込み

さまざまなスタイル シートの読み込みに使用できる方法があります。

### <a name="xaml"></a>XAML

スタイル シートがロードされ、使用して解析することができます、 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet)クラスに追加される前に、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ページ。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source)プロパティは、URI である場合、外側の XAML ファイルの場所への相対またはプロジェクトのルートからの相対 URI として、スタイル シートを指定、`/`です。

> [!WARNING]
> CSS ファイルがある場合の読み込みに失敗するビルド アクションに設定されていない**EmbeddedResource**です。

スタイル シートを読み込むし、を使用して解析または、 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet)クラスによってインライン展開で、`CDATA`セクション。

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

### <a name="c"></a>C#

C# の場合は、スタイル シートをロード埋め込みリソースとしてに追加された、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ページ。

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

1 番目の引数、`StyleSheet.FromAssemblyResource`メソッドはスタイル シートを含むアセンブリでは、2 番目の引数は、`string`リソース識別子を表すです。 リソース識別子を取得できます、**プロパティ**ウィンドウ、CSS ファイルを選択するとします。

代わりに、スタイル シートから読み込める、`StringReader`に追加し、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ページの。

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

引数、`StyleSheet.FromReader`メソッドは、`TextReader`スタイル シートを読み取ることができます。

## <a name="selecting-elements-and-applying-properties"></a>要素を選択し、プロパティの適用

CSS セレクターを使用してを対象とする要素を確認します。 セレクターが一致するスタイルは、定義の順序で、連続して適用されます。 特定のアイテムで定義されているスタイルは常に最後に適用します。 サポートされているセレクターの詳細については、次を参照してください。[セレクター参照](#selector-reference)です。

CSS は、選択した要素のスタイルを設定するのにプロパティを使用します。 各プロパティには、一連の可能な値と、一部のプロパティに影響する可能性の要素の任意の型の要素のグループに適用されるものです。 サポートされているプロパティの詳細については、次を参照してください。[プロパティ リファレンス](#property-reference)です。

### <a name="selecting-elements-by-type"></a>型で要素の選択

ビジュアル ツリー内の要素は、大文字と小文字を持つ型が選択できる`element`セレクター。

```css
stacklayout {
    margin: 20;
}
```

このセレクターは、いずれかを識別[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)スタイル シートを使用して、20 の均一の幅を余白を設定したページの要素。

> [!NOTE]
> `element`セレクターは、指定した型のサブ クラスを識別できません。

### <a name="selecting-elements-by-base-class"></a>基本クラスによって要素の選択

ビジュアル ツリー内の要素は、大文字と小文字を持つ基本クラスが選択できる`^base`セレクター。

```css
^contentpage {
    background-color: lightgray;
}
```

このセレクターは、いずれかを識別[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)スタイル シートを使用して、背景を設定する要素の色を`lightgray`です。

> [!NOTE]
> `^base`セレクターは、Xamarin.Forms の固有であり、CSS 仕様の一部ではありません。

### <a name="selecting-an-element-by-name"></a>名前で要素の選択

大文字と小文字を区別する `#id` セレクターを使うと、ビジュアルツリー内の個々の要素を選択することができます。

```css
#listView {
    background-color: lightgray;
}
```

このセレクターは、要素を識別が[ `StyleId` ](xref:Xamarin.Forms.Element.StyleId)プロパティに設定されている`listView`です。 ただし場合、`StyleId`プロパティが設定されていない、セレクターがフォールバックして、使用して、`x:Name`要素のです。 したがって、次の XAML の例で、`#listView`セレクターが示されます、 [ `ListView` ](xref:Xamarin.Forms.ListView)が`x:Name`属性に設定されている`listView`に背景色を設定および`lightgray`です。

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

### <a name="selecting-elements-with-a-specific-class-attribute"></a>特定のクラスの属性を持つ要素を選択します。

大文字と小文字で特定のクラス属性を持つ要素を選択できる`.class`セレクター。

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

設定して、XAML 要素に CSS クラスを割り当てることが、 [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) CSS クラス名に要素のプロパティです。 そのため、次の XAML の例で、スタイルによって定義、`.detailPageTitle`クラスは、最初に割り当てられた[ `Label` ](xref:Xamarin.Forms.Label)、によって定義されたスタイルの中に、`.detailPageSubtitle`クラスは、2 番目に割り当てられた`Label`です。

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

ビジュアル ツリー内の子要素は、大文字と小文字で選択できる`element element`セレクター。

```css
listview image {
    height: 60;
    width: 60;
}
```

このセレクターは、いずれかを識別[ `Image` ](xref:Xamarin.Forms.Image)要素の子である[ `ListView` ](xref:Xamarin.Forms.ListView)要素、およびの高さと幅を 60 に設定します。 したがって、次の XAML の例で、`listview image`セレクターが示されます、 [ `Image` ](xref:Xamarin.Forms.Image)の子である、 [ `ListView` ](xref:Xamarin.Forms.ListView)、およびその高さと幅を 60 に設定します。

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
> `element element`セレクターに子要素は必要ありません、_直接_親-子要素の子は、別の親を持つ可能性があります。 選択は、その先祖とは、指定された最初の要素を発生します。

### <a name="selecting-direct-child-elements"></a>直接の子要素の選択

大文字と小文字のビジュアル ツリー内の子要素を選択することができますを直接`element>element`セレクター。

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

このセレクターは、いずれかを識別[ `Image` ](xref:Xamarin.Forms.Image)である要素の直接の子[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)要素、およびの高さと幅を 200 に設定します。 したがって、次の XAML の例で、`stacklayout>image`セレクターが示されます、 [ `Image` ](xref:Xamarin.Forms.Image)の直接の子である、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、およびその高さと幅を 200 に設定します。

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
> `element>element`セレクターは、子要素がである必要があります、_直接_親の子です。

## <a name="selector-reference"></a>セレクターの参照

Xamarin.Forms では、次の CSS セレクターがサポートされています。

|[セレクター]|例|説明|
|---|---|---|
|`.class`|`.header`|すべての要素を選択、 `StyleClass` 'ヘッダー' を含むプロパティです。 このセレクターは、大文字小文字を区別ことに注意してください。|
|`#id`|`#email`|すべての要素を選択`StyleId`'éý'`email`です。 場合`StyleId`が設定されていない、フォールバック ツー`x:Name`です。 XAML を使用するときに`x:Name`が優先`StyleId`です。 このセレクターは、大文字小文字を区別ことに注意してください。|
|`*`|`*`|すべての要素を選択します。|
|`element`|`label`|型の要素をすべて選択`Label`、いないサブ クラスが、します。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`^base`|`^contentpage`|すべての要素を選択`ContentPage`、基本クラスとして含む`ContentPage`自体です。 このセレクターは大文字と小文字および CSS の仕様の一部でないことに注意してください。|
|`element,element`|`label,button`|すべて選択`Button`要素とすべて`Label`要素。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element element`|`stacklayout label`|すべて選択`Label`内の要素、`StackLayout`です。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element>element`|`stacklayout>label`|すべて選択`Label`を持つ要素`StackLayout`直接の親として。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element+element`|`label+entry`|すべて選択`Entry`要素の後に直接、`Label`です。 このセレクターは、大文字小文字を区別しないことに注意してください。|
|`element~element`|`label~entry`|すべて選択`Entry`要素に続く、`Label`です。 このセレクターは、大文字小文字を区別しないことに注意してください。|

セレクターが一致するスタイルは、定義の順序で、連続して適用されます。 特定のアイテムで定義されているスタイルは常に最後に適用します。

> [!TIP]
> セレクターを組み合わせて、制限なしなど`StackLayout>ContentView>label.email`です。

次のセレクターは、現在サポートされていません。

- `[attribute]`
- `@media` および `@supports`
- `:` および `::`

> [!NOTE]
> 、の特異性と特異性上書きはサポートされていません。

## <a name="property-reference"></a>プロパティの参照

Xamarin.Forms では次の CSS プロパティがサポートされている (で、**値** 列の型は_斜体_文字列リテラルは、 `gray`)。

|プロパティ|対象|値|例|
|---|---|---|---|
|`background-color`|`VisualElement`|_色_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_文字列_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_色_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_double 型_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_色_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_文字列_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_二重_\| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double 型_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_太さ_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_太さ_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_太さ_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_太さ_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_太さ_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_double 型_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double 型_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double 型_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_太さ_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_double 型_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _double 型_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _double 型_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _double 型_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` および`right`右から左へ記述する環境では避ける必要があります。| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_double 型_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` すべてのプロパティの有効な値です。 別のスタイル設定された値 (既定値にリセット) をクリアします。

次のプロパティは現在サポートされていません。

- `all: initial`。
- レイアウト プロパティ (ボックス、またはグリッド)。
- 短縮形のプロパティなど`font`、および`border`です。

さらに、あるありません`inherit`値などの継承はサポートされていません。 したがって、たとえば、設定できません、`font-size`レイアウトのプロパティすべてを予定があり、 [ `Label` ](xref:Xamarin.Forms.Label)値を継承するレイアウト内のインスタンス。 1 つの例外は、`direction`を既定値を持つプロパティの`inherit`します。

### <a name="color"></a>色

次`color`値がサポートされます。

- `X11` [色](https://en.wikipedia.org/wiki/X11_color_names/)、CSS カラー、UWP 定義済みの色、および Xamarin.Forms 色に一致します。 これらの色値は大文字小文字を区別しないことに注意してください。
- 16 進数の色: `#rgb`、 `#argb`、 `#rrggbb`、 `#aarrggbb`
- rgb 色: `rgb(255,0,0)`、`rgb(100%,0%,0%)`です。 値は範囲 0 ~ 255 0 ~ 100% です。
- rgba 色: `rgba(255, 0, 0, 0.8)`、`rgba(100%, 0%, 0%, 0.8)`です。 不透明度の値は 0.0 ~ 1.0 の範囲内でです。
- hsl の色:`hsl(120, 100%, 50%)`です。 H の値は 0 ~ 360 の範囲内では s と l 中で、範囲 0 ~ 100% です。
- hsla のカラー:`hsla(120, 100%, 50%, .8)`です。 不透明度の値は 0.0 ~ 1.0 の範囲内でです。

### <a name="thickness"></a>太さ

1 つ、2、3、または 4`thickness`値がサポートされている、空白で区切られた各。

- 1 つの値では、均一の太さを示します。
- 2 つの値は、縦、横方向の幅を示します。
- 次の 3 つの値は、top、横方向 (左、右下) は、下端の太さを示します。
- 次の 4 つの値は、top、右上、、下部にある 左側の太さを示します。

> [!NOTE]
> CSS`thickness`値と異なる XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/)値。 たとえば、XAML 2 つの値で`Thickness`4 値中に、水平、垂直方向の幅を示す`Thickness`太さ下、左、上、右側を示します。 さらに、XAML`Thickness`値は、コンマで区切られました。

### <a name="namedsize"></a>NamedSize

大文字と小文字の次`namedsize`値がサポートされます。

- `default`
- `micro`
- `small`
- `medium`
- `large`

それぞれの意味は、`namedsize`値がプラットフォームに依存し、ビューに依存します。

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin.University と Xamarin.Forms の CSS

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS により、 [Xamarin 大学](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [MonkeyAppCSS (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
