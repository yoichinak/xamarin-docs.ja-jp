---
title: " Xamarin.Forms label" description: "この記事では、label クラスを使用して Xamarin.Forms アプリケーションで1行と複数行のテキストを表示する方法について説明します。"
ms. 製品: xamarin ms. assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 04/09/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-label"></a>Xamarin.Formsタイトル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin 形式でテキストを表示する_

[`Label`](xref:Xamarin.Forms.Label)ビューは、1つのテキストと複数行のテキストを表示するために使用されます。 ラベルには、文字装飾、色分けされたテキスト、およびカスタムフォント (ファミリ、サイズ、およびオプション) を使用できます。

## <a name="text-decorations"></a>文字の装飾

[`Label`](xref:Xamarin.Forms.Label)プロパティを1つ以上の列挙メンバーに設定することにより、下線および取り消し線の文字装飾をインスタンスに適用でき `Label.TextDecorations` `TextDecorations` ます。

- `None`
- `Underline`
- `Strikethrough`

次の XAML の例は、プロパティを設定する方法を示してい `Label.TextDecorations` ます。

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

これに相当する C# コードを次に示します。

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

次のスクリーンショットは、 `TextDecorations` インスタンスに適用される列挙型メンバーを示してい [`Label`](xref:Xamarin.Forms.Label) ます。

![文字装飾付きのラベル](label-images/label-textdecorations.png)

> [!NOTE]
> 文字装飾はインスタンスにも適用でき [`Span`](xref:Xamarin.Forms.Span) ます。 クラスの詳細については `Span` 、「[書式設定](#formatted-text)されたテキスト」を参照してください。

## <a name="character-spacing"></a>文字間隔

[`Label`](xref:Xamarin.Forms.Label)プロパティを値に設定することによって、文字間隔をインスタンスに適用でき `Label.CharacterSpacing` `double` ます。

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

これに相当する C# コードを次に示します。

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

結果として、によって表示されるテキスト内の文字 [`Label`](xref:Xamarin.Forms.Label) は、 `CharacterSpacing` デバイスに依存しない単位になります。

## <a name="new-lines"></a>改行

XAML から新しい行にテキストを強制するには、主に次の2つの方法があり [`Label`](xref:Xamarin.Forms.Label) ます。

1. Unicode ラインフィード文字を使用します。これは " &amp; #10;" です。
1. *Property 要素*の構文を使用してテキストを指定します。

次のコードは、両方の手法の例を示しています。

```xaml
<!-- Unicode line feed character -->
<Label Text="First line &#10; Second line" />

<!-- Property element syntax -->
<Label>
    <Label.Text>
        First line
        Second line
    </Label.Text>
</Label>
```

C# では、"\n" という文字を含む新しい行にテキストを強制的に挿入できます。

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>色

ラベルは、バインド可能なプロパティを使用してカスタムテキストの色を使用するように設定でき [`TextColor`](xref:Xamarin.Forms.Label.TextColor) ます。

各プラットフォームで色が使用できるようにするには、特別な注意が必要です。 各プラットフォームにはテキストと背景色について異なる既定値があるため、各プラットフォームで動作する既定値を選択する必要があります。

次の XAML の例では、のテキストの色を設定し `Label` ます。

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

これに相当する C# コードを次に示します。

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

次のスクリーンショットは、プロパティを設定した結果を示してい `TextColor` ます。

![ラベルの TextColor の例](label-images/textcolor.png)

色の詳細については、「[色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="fonts"></a>フォント

でフォントを指定する方法の詳細につい `Label` ては、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

ラベルは、プロパティによって公開されるいくつかの方法のいずれかで1行に収めることができないテキストを処理するように設定でき `LineBreakMode` ます。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)は、次の値を持つ列挙体です。

- **ヘッドの切り捨て** &ndash;テキストの先頭を切り捨て、末尾を表示します。
- 文字の**折り返し** &ndash;文字の境界でテキストを新しい行に折り返します。
- **MiddleTruncation** &ndash;テキストの先頭と末尾を表示します。中央の部分は省略記号で置き換えられます。
- **NoWrap** &ndash;はテキストを折り返しません。1行に収まる程度のテキストだけを表示します。
- **TailTruncation** &ndash;テキストの先頭を切り捨て、末尾を切り捨てます。
- **折り返し** &ndash;ワード境界でテキストをラップします。

## <a name="display-a-specific-number-of-lines"></a>特定の行数を表示する

によって表示される行の数は、 [`Label`](xref:Xamarin.Forms.Label) プロパティを値に設定することによって指定でき `Label.MaxLines` `int` ます。

- `MaxLines`が-1 (既定値) の場合、は、 `Label` プロパティの値に対して、 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 1 行だけを表示するか、切り捨てられるか、すべてのテキストを含むすべての行を表示するかを指定します。
- `MaxLines`が0の場合、は `Label` 表示されません。
- `MaxLines`が1の場合、結果は [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 、プロパティを [`NoWrap`](xref:Xamarin.Forms.LineBreakMode) 、、 [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode) [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode) 、またはに設定するのと同じになり [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode) ます。 ただし、では、 `Label` [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 省略記号の配置に関して、プロパティの値が尊重されます (該当する場合)。
- `MaxLines`が1より大きい場合、は `Label` 指定された行数まで表示され、必要に [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 応じて、省略記号の配置に関してプロパティの値が考慮されます。 ただし、プロパティ `MaxLines` [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) がに設定されている場合、プロパティを1より大きい値に設定しても効果はありません [`NoWrap`](xref:Xamarin.Forms.LineBreakMode) 。

次の XAML の例は、でプロパティを設定する方法を示してい `MaxLines` [`Label`](xref:Xamarin.Forms.Label) ます。

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

これに相当する C# コードを次に示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

次のスクリーンショットは、プロパティを2に設定した結果を示して `MaxLines` います。これは、テキストが2行を超えて占有するのに十分な長さである場合に発生します。

![MaxLines のラベルの例](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML の表示

クラスには、 [`Label`](xref:Xamarin.Forms.Label) `TextType` インスタンスで `Label` プレーンテキストを表示するか、HTML テキストを表示するかを決定するプロパティがあります。 このプロパティは、列挙体のいずれかのメンバーに設定する必要があり `TextType` ます。

- `Text`は `Label` プレーンテキストを表示し、がプロパティの既定値であることを示し `Label.TextType` ます。
- `Html`が HTML テキストを表示することを示し `Label` ます。

このため、インスタンスでは、 [`Label`](xref:Xamarin.Forms.Label) プロパティを `Label.TextType` に、 `Html` プロパティを html 文字列に設定することにより、html を表示でき `Label.Text` ます。

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

上記の例では、HTML の二重引用符文字をシンボルを使用してエスケープする必要があり `\` ます。

XAML では、シンボルと記号をさらにエスケープすることにより、HTML 文字列を読み取ることができなくなる可能性が `<` `>` あります。

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

また、読みやすくするために、セクションに HTML をインライン展開することもでき `CDATA` ます。

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

この例では、 `Label.Text` プロパティは、セクションにインライン展開されている HTML 文字列に設定されてい `CDATA` ます。 これは、 `Text` プロパティがクラスのであるために機能し `ContentProperty` `Label` ます。

次のスクリーンショットは、HTML の表示を示してい [`Label`](xref:Xamarin.Forms.Label) ます。

![IOS と Android での HTML を表示するラベルのスクリーンショット](label-images/label-html.png)

> [!IMPORTANT]
> での HTML の表示 [`Label`](xref:Xamarin.Forms.Label) は、基になるプラットフォームでサポートされている html タグに限定されます。

## <a name="formatted-text"></a>書式付きテキスト

ラベルは、 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 同じビューの複数のフォントと色を持つテキストを表示できるようにするプロパティを公開します。

`FormattedText`プロパティは [`FormattedString`](xref:Xamarin.Forms.FormattedString) 、プロパティを使用して設定される1つ以上のインスタンスで構成される型です [`Span`](xref:Xamarin.Forms.Span) [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) 。 次のプロパティを使用して、 `Span` 外観を設定できます。

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor): スパンの背景の色。
- `CharacterSpacing`: `double` 型、`Span` テキストの文字間の間隔。
- [`Font`](xref:Xamarin.Forms.Span.Font)–スパン内のテキストのフォント。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)–スパン内のテキストのフォント属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)–スパン内のテキストのフォントが属するフォントファミリ。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)–スパン内のテキストのフォントのサイズ。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)–スパン内のテキストの色。 このプロパティは互換性のために残されていますが、プロパティによって置き換えられてい `TextColor` ます。
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)-スパンの既定の行の高さに適用する乗数。 詳細については、「[行の高さ](#line-height)」を参照してください。
- [`Style`](xref:Xamarin.Forms.Span.Style)–スパンに適用するスタイル。
- [`Text`](xref:Xamarin.Forms.Span.Text)–スパンのテキスト。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)–スパン内のテキストの色。
- `TextDecorations`-スパン内のテキストに適用する装飾。 詳細については、「[文字装飾](#text-decorations)」を参照してください。

、、およびのバインド可能なプロパティには、 [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) [`Text`](xref:Xamarin.Forms.Span.Text) [`Text`](xref:Xamarin.Forms.Span.Text) の既定のバインディングモードがあり [`OneWay`](xref:Xamarin.Forms.BindingMode) ます。 このバインディングモードの詳細については、「[バインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)ガイド」の[既定のバインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)に関する説明を参照してください。

また、プロパティを [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) 使用して、のジェスチャに応答するジェスチャレコグナイザーのコレクションを定義することもでき [`Span`](xref:Xamarin.Forms.Span) ます。

> [!NOTE]
> に HTML を表示することはできません [`Span`](xref:Xamarin.Forms.Span) 。

次の XAML の例は、 `FormattedText` 3 つのインスタンスで構成されるプロパティを示してい [`Span`](xref:Xamarin.Forms.Span) ます。

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

これに相当する C# コードを次に示します。

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
> [`Text`](xref:Xamarin.Forms.Span.Text)のプロパティは、 `Span` データバインディングを使用して設定できます。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。

は、 [`Span`](xref:Xamarin.Forms.Span) スパンのコレクションに追加されたジェスチャにも応答できることに注意して [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) ください。 たとえば、上記の [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) コード例では、が2番目のに追加されてい `Span` ます。 このため、がタップされると `Span` 、は `TapGestureRecognizer` `ICommand` プロパティによって定義されたを実行して応答し [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) ます。 ジェスチャレコグナイザーの詳細については、「 [ Xamarin.Forms ジェスチャ](~/xamarin-forms/app-fundamentals/gestures/index.md)」を参照してください。

次のスクリーンショットは、プロパティを3つのインスタンスに設定した結果を示してい `FormattedString` `Span` ます。

![ラベル FormattedText の例](label-images/formattedtext.png)

## <a name="line-height"></a>[行間]

との垂直方向の高さを [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) カスタマイズするには、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) プロパティまたは [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 値を設定し `double` ます。 IOS と Android では、これらの値は元の行の高さの乗数であり、ユニバーサル Windows プラットフォーム (UWP) では、 `Label.LineHeight` プロパティ値はラベルのフォントサイズの乗数です。

> [!NOTE]
>
> - IOS では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) プロパティとプロパティによって、 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 1 行に収まるテキストの行の高さと、複数の行に折り返されるテキストが変更されます。
> - Android では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) プロパティとプロパティは、 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) 複数の行に折り返されるテキストの行の高さのみを変更します。
> - UWP では、プロパティは、折り返されて [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) いるテキストの行の高さを複数の行に変更し [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) ます。このプロパティは効果がありません。

次の XAML の例は、でプロパティを設定する方法を示してい [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Label`](xref:Xamarin.Forms.Label) ます。

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

これに相当する C# コードを次に示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

次のスクリーンショットは、プロパティを1.8 に設定した結果を示してい [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) ます。

![ラベル LineHeight の例](label-images/label-lineheight.png)

次の XAML の例は、でプロパティを設定する方法を示してい [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) [`Span`](xref:Xamarin.Forms.Span) ます。

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

これに相当する C# コードを次に示します。

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

次のスクリーンショットは、プロパティを1.8 に設定した結果を示してい [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) ます。

![Span LineHeight の例](label-images/span-lineheight.png)

## <a name="padding"></a>余白

余白は、要素とその子要素の間のスペースを表し、要素を独自のコンテンツから分離するために使用されます。 [`Label`](xref:Xamarin.Forms.Label)プロパティを値に設定することによって、インスタンスに埋め込みを適用でき `Label.Padding` [`Thickness`](xref:Xamarin.Forms.Thickness) ます。

```xaml
<Label Padding="10">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum" />
            <Span Text="dolor sit amet." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

これに相当する C# コードを次に示します。

```csharp
FormattedString formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum"
});
formattedString.Spans.Add(new Span
{
  Text = "dolor sit amet."
});
Label label = new Label
{
    FormattedText = formattedString,
    Padding = new Thickness(20)
};
```

> [!IMPORTANT]
> IOS で [`Label`](xref:Xamarin.Forms.Label) は、プロパティを設定するが作成されると、 `Padding` 埋め込みが適用され、埋め込み値を後で更新できるようになります。 ただし、プロパティが設定されていないを作成した場合 `Label` `Padding` 、後で設定しようとしても効果はありません。
>
> Android およびユニバーサル Windows プラットフォームでは、 `Padding` の作成時または後にプロパティ値を指定でき `Label` ます。

埋め込みの詳細については、「[余白と埋め込み](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」を参照してください。

## <a name="hyperlinks"></a>ハイパーリンク

およびインスタンスによって表示されるテキストは、 [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) 次の方法でハイパーリンクに変換できます。

1. `TextColor` `TextDecoration` またはのプロパティとプロパティを設定し [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) ます。
1. を [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) またはのコレクションに追加 [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) します。このプロパティは、を [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) にバインドし、そのプロパティには `ICommand` [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 開く URL を格納します。
1. に `ICommand` よって実行されるを定義し [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) ます。
1. によって実行されるコードを記述し `ICommand` ます。

次のコード例では、[ハイパーリンクのデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)のサンプルで、 [`Label`](xref:Xamarin.Forms.Label) 複数のインスタンスからコンテンツが設定されているを示してい [`Span`](xref:Xamarin.Forms.Span) ます。

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

この例では、1番目と3番目の [`Span`](xref:Xamarin.Forms.Span) インスタンスがテキストを構成し、2番目のインスタンスは `Span` tappable ハイパーリンクを表します。 テキストの色が青色に設定され、下線が付きます。 これにより、次のスクリーンショットに示すように、ハイパーリンクの外観が作成されます。

[![ハイパーリンク](label-images/hyperlinks-small.png "ハイパーリンク")](label-images/hyperlinks-large.png#lightbox)

ハイパーリンクがタップされると、はその [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) プロパティによって定義されたを実行して応答し `ICommand` [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) ます。 さらに、プロパティによって指定された URL は [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 、 `ICommand` パラメーターとしてに渡されます。

XAML ページの分離コードには、次の実装が含まれてい `TapCommand` ます。

```csharp
public partial class MainPage : ContentPage
{
    // Launcher.OpenAsync is provided by Xamarin.Essentials.
    public ICommand TapCommand => new Command<string>(async (url) => await Launcher.OpenAsync(url));

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }
}
```

は、 `TapCommand` メソッドを実行し `Launcher.OpenAsync` [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) て、プロパティ値をパラメーターとして渡します。 `Launcher.OpenAsync`メソッドは、によって提供され、 Xamarin.Essentials web ブラウザーで URL を開きます。 そのため、全体的な影響として、ページでハイパーリンクをタップすると、web ブラウザーが表示され、ハイパーリンクに関連付けられている URL に移動します。

### <a name="creating-a-reusable-hyperlink-class"></a>再利用可能なハイパーリンククラスを作成する

前の方法でハイパーリンクを作成するには、アプリケーションでハイパーリンクが必要になるたびに繰り返しコードを記述する必要があります。 ただし、クラスとクラスの両方をサブクラス化して、 [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) クラスとクラスを作成することができます。これには、 `HyperlinkLabel` `HyperlinkSpan` ジェスチャ認識エンジンとテキスト書式コードを追加します。

次のコード例は、ハイパーリンクの[デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)のサンプルから抜粋したクラスを示してい `HyperlinkSpan` ます。

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
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

`HyperlinkSpan`クラスはプロパティを定義し、 `Url` 関連付けられており、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) コンストラクターはハイパーリンクの外観と、 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) ハイパーリンクがタップされたときに応答するを設定します。 がタップされると、は、 `HyperlinkSpan` `TapGestureRecognizer` メソッドを実行して、 `Launcher.OpenAsync` プロパティによって指定された URL を web ブラウザーで開くことで応答し `Url` ます。

クラスは、 `HyperlinkSpan` クラスのインスタンスを XAML に追加することによって使用できます。

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

## <a name="styling-labels"></a>ラベルのスタイル設定

前のセクションでは、インスタンスごとの設定とプロパティについて説明して [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) います。 ただし、一連のプロパティを1つのスタイルにグループ化して、1つまたは複数のビューに一貫して適用することができます。 これにより、コードの読みやすさが向上し、デザインの変更を実装しやすくなります。 詳細については、「[スタイル](~/xamarin-forms/user-interface/text/styles.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [ハイパーリンク (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [第3章で Mobile Apps を作成する Xamarin.Forms](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [ラベルの API](xref:Xamarin.Forms.Label)
- [スパン API](xref:Xamarin.Forms.Span)
