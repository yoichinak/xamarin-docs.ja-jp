---
title: Xamarin.Forms のラベル
description: この記事では、Xamarin.Forms ラベル クラスを使用して、アプリケーションで単一と複数行のテキストを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 5c12b5a5c0e558403362bd12baddfb8201f40759
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642925"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms のラベル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.Forms にテキストを表示_

[ `Label` ](xref:Xamarin.Forms.Label)は、単一または複数行のテキストを表示するビューとして使用します。 ラベルは、テキストを装飾したり、テキストに色を付けたり、カスタム フォント (ファミリ、サイズ、およびオプション) を使用することができます。

## <a name="text-decorations"></a>文字装飾

`Label.TextDecorations`プロパティや`TextDecorations`列挙型のメンバーを使用することで、[ `Label` ](xref:Xamarin.Forms.Label)のインスタンスに、下線や取り消し線の装飾を適用することができます。

- `None`
- `Underline`
- `Strikethrough`

次の XAML は、`Label.TextDecorations`プロパティを設定する例です。

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

同等のコードをC#で示します。

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

次のスクリーンショットは、`TextDecorations`列挙型のメンバーを[ `Label` ](xref:Xamarin.Forms.Label)のインスタンスに適用した例です。

![](label-images/label-textdecorations.png "ラベルにテキスト装飾を適用")

> [!NOTE]
> テキスト装飾は[ `Span` ](xref:Xamarin.Forms.Span)インスタンスにも適用することができます。 詳細については、`Span`クラスを参照してください（[書式付きテキスト](#Formatted_Text)）。

## <a name="colors"></a>色

ラベルはバインド可能な[ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)プロパティを使用して、カスタムカラーを使用するように設定することができます。

各プラットフォームで色を使用するには特別な注意が必要です。 各プラットフォームごとに、テキストと背景の色の既定値があるため、それぞれに適した既定値を選択するよう注意する必要があります。

次の XAML は`Label`のテキストの色の設定例です :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
    </StackLayout>
</ContentPage>
```

同等のコードをC#で示します。

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

次のスクリーンショットは`TextColor`プロパティを設定した結果の例です。

![](label-images/textcolor.png "ラベル のTextColor 使用例")

色の詳細については、次を参照してください。[Xamarin.Formsでの色](~/xamarin-forms/user-interface/colors.md)。

## <a name="fonts"></a>フォント

`Label`でフォントを指定する方法についての詳細は、[フォント](~/xamarin-forms/user-interface/text/fonts.md)を参照してください。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

`LineBreakMode` プロパティによって公開されているいくつかの方法で、 1つの行に収まらないテキストを処理する設定をラベルに適用することができます。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 次に列挙型の値を示します。

- **HeadTruncation** &ndash;テキストの先頭を切り捨てて末尾を表示します。
- **CharacterWrap** &ndash;テキストを文字の境界で改行します。
- **MiddleTruncation** &ndash;テキストの先頭と末尾を表示し、中間を省略記号で置き換えます。
- **NoWrap** &ndash;テキストを折り返しません。１行で収まるテキストを表示します。
- **TailTruncation** &ndash;テキストの先頭を表示して、末尾を切り捨てます。
- **WordWrap** &ndash;テキストを単語の境目で折り返します。

## <a name="displaying-a-specific-number-of-lines"></a>特定の行数を表示します。

[ `Label` ](xref:Xamarin.Forms.Label)に表示される行数は、 `Label.MaxLines`プロパティに`int`値を指定して設定することができます。

- `MaxLines`が 0 の場合は、`Label` [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値を考慮して、１行だけ（場合によっては切り捨て）、または全てのテキストを含む全ての行を表示します。
- `MaxLines`が 1 の場合は、 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode)プロパティを[ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode)、 [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode)、 [`MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode)、または[ `TailTruncation`](xref:Xamarin.Forms.LineBreakMode)に設定するのと同じです。 ただし、`Label` 省略記号の表示については、 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode)の値を尊重します。
- `MaxLines`が 1 より大きい場合は、指定された行数を`Label`に表示しますが、省略記号の表示は必要に応じて [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値を考慮します。 ただし、`MaxLines`プロパティを 1 より大きい値にしている場合でも、 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode)プロパティに[ `NoWrap`](xref:Xamarin.Forms.LineBreakMode)が設定されている場合は効果がありません。

次の XAML は、[ `Label` ](xref:Xamarin.Forms.Label)の`MaxLines`プロパティを設定する例です:

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

同等のコードをC#で示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

次のスクリーンショットは、テキストが2行以上のスペースを有する場合に`MaxLines`プロパティに 2 を設定した結果を示しています。

![](label-images/label-maxlines.png "ラベルの MaxLines プロパティ使用例")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>書式付きテキスト

ラベルは、 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)同じビューの複数のフォントと色を持つテキストを表示できるようにするプロパティを公開します。

`FormattedText`プロパティの型は[ `FormattedString` ](xref:Xamarin.Forms.FormattedString)、1 つまたは複数で構成される[ `Span` ](xref:Xamarin.Forms.Span)設定を使用して、インスタンス、 [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans)プロパティ. `Span` 外観を設定することができます。

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – スパンの背景色。
- [`Font`](xref:Xamarin.Forms.Span.Font) -スパン内のテキストのフォント。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) -スパン内のテキストのフォント属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) -スパン内のテキストフォントが属するフォントファミリ。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) -スパン内のテキストのフォントのサイズ。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) -スパン内のテキストの色。 このプロパティは廃止されましたが、`TextColor`プロパティに置き換えられました。
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -スパンの既定の行の高さに適用する乗数。 詳細については、次を参照してください。[行の高さ](#line-height)
- [`Style`](xref:Xamarin.Forms.Span.Style)–スパンに適用するスタイル。
- [`Text`](xref:Xamarin.Forms.Span.Text) – スパンのテキスト。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) -スパン内のテキストの色。
- `TextDecorations` -スパン内のテキストに適用する装飾。 詳細については、次を参照してください。[文字装飾](#text-decorations)。

> [!NOTE]
> 、 [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) [、`Text`](xref:Xamarin.Forms.Span.Text) [`OneWay`](xref:Xamarin.Forms.BindingMode)および[のバインド可能なプロパティには、の既定のバインディングモードがあります。`Text`](xref:Xamarin.Forms.Span.Text) このバインディングモードの詳細については、「[バインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)ガイド」の[既定のバインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)に関する説明を参照してください。

さらに、 [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers)プロパティを使用して、 [ `Span`](xref:Xamarin.Forms.Span)のジェスチャに応答するジェスチャ レコグナイザーのコレクションを使用することができます。

次のXAMLの例は、3つの[ `Span` ](xref:Xamarin.Forms.Span)インスタンスで構成される`FormattedText`プロパティを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

同等のコードをC#で示します。

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> `Span`の[ `Text` ](xref:Xamarin.Forms.Span.Text)プロパティはデータバインディングを通じて設定できます。 詳細については、次を参照してください。[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)

なお、 [ `Span` ](xref:Xamarin.Forms.Span)は 、span　の[ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers)コレクション に追加されるすべてのジェスチャに応答できます。 たとえば、 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)は、上記のコード例の 2 番目の`Span`に追加されています。 そのため、この`Span`がタップされると、`TapGestureRecognizer`は[ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command)プロパティで定義された`ICommand`を実行して応答します。 ジェスチャレコグナイザーの詳細については、次を参照してください。 [Xamarin.Forms のジェスチャ](~/xamarin-forms/app-fundamentals/gestures/index.md)

次のスクリーンショットは、`FormattedString`プロパティを3つの`Span`インスタンスに設定した結果を示しています。

![](label-images/formattedtext.png "ラベルの FormattedText の設定例")

## <a name="line-height"></a>[行間]

[ `Label` ](xref:Xamarin.Forms.Label)と[ `Span` ](xref:Xamarin.Forms.Span)の垂直方向高さは、 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight)プロパティまたは[ `Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)に`double`値を設定することでカスタマイズすることができます。 IOS と Android では、これらの値は元の行の高さの倍数であり、ユニバーサル Windows プラットフォーム (UWP) の場合は、`Label.LineHeight`プロパティの値がラベルのフォントサイズの倍数になります。

> [!NOTE]
> - iOS では、 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight)と[ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight)プロパティは、 1 つの行に収まるテキストまたは複数行に折り返すテキストの行の高さを変更します。
> - Android では、 [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight)と[ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight)プロパティは、複数行に折り返されるテキスト行の高さのみを変更します。
> - UWP では[ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight)プロパティが複数行に折り返されるテキストの行の高さを変更し、 [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight)プロパティは影響がありません。

次の XAML は、 [ `LineHeight` ](xref:Xamarin.Forms.Label.LineHeight)の[ `Label` ](xref:Xamarin.Forms.Label)プロパティ設定する例を示しています:

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

同等のコードをC#で示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

次のスクリーンショットは、[ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight)プロパティに 1.8 の値を設定した結果を示します。

![](label-images/label-lineheight.png "ラベルの LineHeight プロパティの使用例")

次の XAML の例は、[ `Span` ](xref:Xamarin.Forms.Span)の [ `LineHeight` ](xref:Xamarin.Forms.Span.LineHeight)プロパティを設定する方法を示しています:

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

同等のコードをC#で示します。

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

次のスクリーンショットは、[ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) プロパティに 1.8 設定した結果を示しています。

![](label-images/span-lineheight.png "Sapn LineHeight の使用例")

## <a name="hyperlinks"></a>ハイパーリンク

[`Label`](xref:Xamarin.Forms.Label) [および`Span`](xref:Xamarin.Forms.Span)インスタンスによって表示されるテキストは、次の方法でハイパーリンクに変換できます。

1. または`TextDecoration` `TextColor` [の`Span`](xref:Xamarin.Forms.Span)プロパティとプロパティを設定します。 [`Label`](xref:Xamarin.Forms.Label)
1. [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) `ICommand`を[また`Label`](xref:Xamarin.Forms.Label)は[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) [のコレクションに追加します。このプロパティは、をにバインドし、そのプロパティには開くURLを格納します。`Span`](xref:Xamarin.Forms.Span) [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)
1. によって実行されるを定義し[ます。`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) `ICommand`
1. によって`ICommand`実行されるコードを記述します。

次のコード例では、[ハイパーリンクのデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)のサンプルで、 [`Label`](xref:Xamarin.Forms.Label)複数[`Span`](xref:Xamarin.Forms.Span)のインスタンスからコンテンツが設定されているを示しています。

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Alternatively, click " />
            <Span Text="here"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://docs.microsoft.com/xamarin/" />
                </Span.GestureRecognizers>
            </Span>
            <Span Text=" to view Xamarin documentation." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

この例では、1番目[`Span`](xref:Xamarin.Forms.Span)と3番目のインスタンスが`Span`テキストを構成し、2番目のインスタンスは tappable ハイパーリンクを表します。 テキストの色が青色に設定され、下線が付きます。 これにより、次のスクリーンショットに示すように、ハイパーリンクの外観が作成されます。

[![ハイパーリンク](label-images/hyperlinks-small.png "ハイパーリンク")](label-images/hyperlinks-large.png#lightbox)

ハイパーリンクがタップ[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)されると、はその[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)プロパティによっ`ICommand`て定義されたを実行して応答します。 さらに、 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)プロパティによって指定された URL は、 `ICommand`パラメーターとしてに渡されます。

XAML ページの分離コードには、次の`TapCommand`実装が含まれています。

```csharp
public partial class MainPage : ContentPage
{
    public ICommand TapCommand => new Command<string>(OpenBrowser);

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }

    void OpenBrowser(string url)
    {
        Device.OpenUri(new Uri(url));
    }
}
```

は`TapCommand` 、 `OpenBrowser`メソッドを実行して[`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 、プロパティ値をパラメーターとして渡します。 次に、このメソッドはメソッド[`Device.OpenUri`](xref:Xamarin.Forms.Device.OpenUri*)を呼び出して、web ブラウザーで URL を開きます。 そのため、全体的な影響として、ページでハイパーリンクをタップすると、web ブラウザーが表示され、ハイパーリンクに関連付けられている URL に移動します。

### <a name="creating-a-reusable-hyperlink-class"></a>再利用可能なハイパーリンククラスを作成する

前の方法でハイパーリンクを作成するには、アプリケーションでハイパーリンクが必要になるたびに繰り返しコードを記述する必要があります。 ただし、クラスと[`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span)クラスの両方をサブクラス化`HyperlinkLabel`し`HyperlinkSpan`て、クラスとクラスを作成することができます。これには、ジェスチャ認識エンジンとテキスト書式コードを追加します。

次のコード例は、ハイパーリンクの[デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)のサンプルから抜粋し`HyperlinkSpan`たクラスを示しています。

```csharp
public class HyperlinkSpan : Span
{
    public static readonly BindableProperty UrlProperty =
        BindableProperty.Create(nameof(Url), typeof(string), typeof(HyperlinkSpan), null);

    public string Url
    {
        get { return (string)GetValue(UrlProperty); }
        set { SetValue(UrlProperty, value); }
    }

    public HyperlinkSpan()
    {
        TextDecorations = TextDecorations.Underline;
        TextColor = Color.Blue;
        GestureRecognizers.Add(new TapGestureRecognizer
        {
            Command = new Command(() => Device.OpenUri(new Uri(Url)))
        });
    }
}
```

クラス`HyperlinkSpan` `Url`はプロパティを定義し、関連[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)付けられており、コンストラクターはハイパーリンクの[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)外観と、ハイパーリンクがタップされたときに応答するを設定します。 がタップされると、 `TapGestureRecognizer`は、 [`Device.OpenUri`](xref:Xamarin.Forms.Device.OpenUri*)メソッドを実行して、 `Url`プロパティによって指定された URL を web ブラウザーで開くことで応答します。 `HyperlinkSpan`

`HyperlinkSpan`クラスは、クラスのインスタンスを XAML に追加することによって使用できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:HyperlinkDemo"
             x:Class="HyperlinkDemo.MainPage">
    <StackLayout>
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Alternatively, click " />
                    <local:HyperlinkSpan Text="here"
                                         Url="https://docs.microsoft.com/appcenter/" />
                    <Span Text=" to view AppCenter documentation." />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

## <a name="styling-labels"></a>スタイル ラベル

前のセクションでは、インスタンスごとに[`Label`](xref:Xamarin.Forms.Label)と[`Span`](xref:Xamarin.Forms.Span) のプロパティを設定する方法について説明しました。 ただし、プロパティのセットは、1 つまたは複数のビューに一貫して適用されるように、1 つのスタイルにグループ化できます。 これによりコードの読みやすさを向上でき設計の変更を簡単に実装しやすくなります。 詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/text/styles.md)。

## <a name="related-links"></a>関連リンク

- [テキスト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [ハイパーリンク (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [第 3 章、Xamarin.Forms でモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [ラベルの API](xref:Xamarin.Forms.Label)
- [API のスパン](xref:Xamarin.Forms.Span)
