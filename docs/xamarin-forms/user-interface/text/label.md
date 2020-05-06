---
title: Xamarin. フォームラベル
description: この記事では、Xamarin の Label クラスを使用して、アプリケーションに単一のテキストと複数行のテキストを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: 6ce4e39986afb7444e7d126e9789760d9311ca45
ms.sourcegitcommit: 737c7fbbe8aed1f33110a5217d7e6d6ef3e5b785
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793223"
---
# <a name="xamarinforms-label"></a>Xamarin. フォームラベル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin 形式でテキストを表示する_

[`Label`](xref:Xamarin.Forms.Label)ビューは、1つのテキストと複数行のテキストを表示するために使用されます。 ラベルには、文字装飾、色分けされたテキスト、およびカスタムフォント (ファミリ、サイズ、およびオプション) を使用できます。

## <a name="text-decorations"></a>文字の装飾

プロパティを1つ以上の[`Label`](xref:Xamarin.Forms.Label) `TextDecorations`列挙メンバーに設定することにより、下線および取り消し線の文字装飾をインスタンスに適用できます。 `Label.TextDecorations`

- `None`
- `Underline`
- `Strikethrough`

次の XAML の例は、 `Label.TextDecorations`プロパティを設定する方法を示しています。

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

同等の C# コードを次に示します。

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

次のスクリーンショットは`TextDecorations` 、インスタンスに[`Label`](xref:Xamarin.Forms.Label)適用される列挙型メンバーを示しています。

![文字装飾付きのラベル](label-images/label-textdecorations.png)

> [!NOTE]
> 文字装飾はインスタンスに[`Span`](xref:Xamarin.Forms.Span)も適用できます。 クラスの詳細につい`Span`ては、「[書式設定](#Formatted_Text)されたテキスト」を参照してください。

## <a name="character-spacing"></a>文字間隔

プロパティを`double`値に設定する[`Label`](xref:Xamarin.Forms.Label)ことによって`Label.CharacterSpacing` 、文字間隔をインスタンスに適用できます。

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

同等の C# コードを次に示します。

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

結果として、によって表示される[`Label`](xref:Xamarin.Forms.Label)テキスト内`CharacterSpacing`の文字は、デバイスに依存しない単位になります。

## <a name="new-lines"></a>改行

XAML から新しい行にテキスト[`Label`](xref:Xamarin.Forms.Label)を強制するには、主に次の2つの方法があります。

1. Unicode ラインフィード文字を使用します。これ&amp;は "#10;" です。
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

ラベルは、バインド[`TextColor`](xref:Xamarin.Forms.Label.TextColor)可能なプロパティを使用してカスタムテキストの色を使用するように設定できます。

各プラットフォームで色が使用できるようにするには、特別な注意が必要です。 各プラットフォームにはテキストと背景色について異なる既定値があるため、各プラットフォームで動作する既定値を選択する必要があります。

次の XAML の例では、 `Label`のテキストの色を設定します。

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

同等の C# コードを次に示します。

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

次のスクリーンショットは、プロパティを`TextColor`設定した結果を示しています。

![ラベルの TextColor の例](label-images/textcolor.png)

色の詳細については、「[色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="fonts"></a>フォント

でフォントを指定する方法の詳細`Label`については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

ラベルは、 `LineBreakMode`プロパティによって公開されるいくつかの方法のいずれかで1行に収めることができないテキストを処理するように設定できます。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)は、次の値を持つ列挙体です。

- ヘッド**切り捨て** &ndash;は、終了を示すテキストの先頭を切り捨てます。
- 文字の**折り返し** &ndash;は、文字の境界でテキストを新しい行にラップします。
- **MiddleTruncation** &ndash;には、テキストの先頭と末尾が表示されます。中央の部分は省略記号で置き換えられます。
- [ **NoWrap** &ndash; ] では、テキストを折り返しません。1行に収まる程度のテキストだけが表示されます。
- **TailTruncation** &ndash;は、テキストの先頭を切り捨て、末尾を切り捨てます。
- ワードラップは、ワード境界でテキスト**を** &ndash;ラップします。

## <a name="display-a-specific-number-of-lines"></a>特定の行数を表示する

によって表示される行[`Label`](xref:Xamarin.Forms.Label)の数は、 `Label.MaxLines`プロパティを`int`値に設定することによって指定できます。

- が`MaxLines` -1 (既定値) の場合、は`Label` 、 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値に対して、1行だけを表示するか、切り捨てられるか、すべてのテキストを含むすべての行を表示するかを指定します。
- が`MaxLines` 0 の場合、 `Label`は表示されません。
- が`MaxLines` 1 の場合、結果は[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 、プロパティを、 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode) [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode)、 [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)、または[`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)に設定するのと同じになります。 ただし、で`Label`は、省略記号の配置[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)に関して、プロパティの値が尊重されます (該当する場合)。
- が`MaxLines` 1 より大きい場合、は`Label`指定された行数まで表示され、必要に応じて、省略[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)記号の配置に関してプロパティの値が考慮されます。 ただし、プロパティが`MaxLines`に[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)設定されている場合、プロパティを1より大きい値に設定しても効果はありません。

次の XAML の例は、 `MaxLines`でプロパティを[`Label`](xref:Xamarin.Forms.Label)設定する方法を示しています。

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

同等の C# コードを次に示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

次のスクリーンショットは、 `MaxLines`プロパティを2に設定した結果を示しています。これは、テキストが2行を超えて占有するのに十分な長さである場合に発生します。

![MaxLines のラベルの例](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML の表示

[`Label`](xref:Xamarin.Forms.Label)クラスには、 `TextType` `Label`インスタンスでプレーンテキストを表示するか、HTML テキストを表示するかを決定するプロパティがあります。 このプロパティは、 `TextType`列挙体のいずれかのメンバーに設定する必要があります。

- `Text`はプレーンテキスト`Label`を表示し、が`Label.TextType`プロパティの既定値であることを示します。
- `Html`が HTML テキスト`Label`を表示することを示します。

このため[`Label`](xref:Xamarin.Forms.Label) 、インスタンスでは、 `Label.TextType`プロパティをに`Html`、 `Label.Text`プロパティを html 文字列に設定することにより、html を表示できます。

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

上記の例では、HTML の二重引用符文字を`\`シンボルを使用してエスケープする必要があります。

XAML では、シンボル`<`と`>`記号をさらにエスケープすることにより、HTML 文字列を読み取ることができなくなる可能性があります。

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

また、読みやすくするために、 `CDATA`セクションに HTML をインライン展開することもできます。

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

この例では、 `Label.Text`プロパティは、 `CDATA`セクションにインライン展開されている HTML 文字列に設定されています。 これは、 `Text`プロパティが`ContentProperty` `Label`クラスのであるために機能します。

次のスクリーンショットは[`Label`](xref:Xamarin.Forms.Label) 、HTML の表示を示しています。

![IOS と Android での HTML を表示するラベルのスクリーンショット](label-images/label-html.png)

> [!IMPORTANT]
> での HTML の[`Label`](xref:Xamarin.Forms.Label)表示は、基になるプラットフォームでサポートされている html タグに限定されます。

<a name="Formatted_Text" />

## <a name="formatted-text"></a>書式付きテキスト

ラベルは、 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)同じビューの複数のフォントと色を持つテキストを表示できるようにするプロパティを公開します。

プロパティ`FormattedText`は、 [`Spans`](xref:Xamarin.Forms.FormattedString.Spans)プロパティを[`FormattedString`](xref:Xamarin.Forms.FormattedString)使用して設定される[`Span`](xref:Xamarin.Forms.Span) 1 つ以上のインスタンスで構成される型です。 次`Span`のプロパティを使用して、外観を設定できます。

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor): スパンの背景の色。
- `CharacterSpacing`: `double` 型、`Span` テキストの文字間の間隔。
- [`Font`](xref:Xamarin.Forms.Span.Font)–スパン内のテキストのフォント。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)–スパン内のテキストのフォント属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)–スパン内のテキストのフォントが属するフォントファミリ。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)–スパン内のテキストのフォントのサイズ。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)–スパン内のテキストの色。 このプロパティは互換性のために残されて`TextColor`いますが、プロパティによって置き換えられています。
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)-スパンの既定の行の高さに適用する乗数。 詳細については、「[行の高さ](#line-height)」を参照してください。
- [`Style`](xref:Xamarin.Forms.Span.Style)–スパンに適用するスタイル。
- [`Text`](xref:Xamarin.Forms.Span.Text)–スパンのテキスト。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)–スパン内のテキストの色。
- `TextDecorations`-スパン内のテキストに適用する装飾。 詳細については、「[文字装飾](#text-decorations)」を参照してください。

、 [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) [`Text`](xref:Xamarin.Forms.Span.Text)、および[`Text`](xref:Xamarin.Forms.Span.Text)のバインド可能なプロパティには、の[`OneWay`](xref:Xamarin.Forms.BindingMode)既定のバインディングモードがあります。 このバインディングモードの詳細については、「[バインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)ガイド」の[既定のバインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)に関する説明を参照してください。

また、 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)プロパティを使用して、の[`Span`](xref:Xamarin.Forms.Span)ジェスチャに応答するジェスチャレコグナイザーのコレクションを定義することもできます。

> [!NOTE]
> に HTML を表示することはできませ[`Span`](xref:Xamarin.Forms.Span)ん。

次の XAML の例は`FormattedText` 、3つ[`Span`](xref:Xamarin.Forms.Span)のインスタンスで構成されるプロパティを示しています。

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

同等の C# コードを次に示します。

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
> の[`Text`](xref:Xamarin.Forms.Span.Text)プロパティは、 `Span`データバインディングを使用して設定できます。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。

は、 [`Span`](xref:Xamarin.Forms.Span)スパンの[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)コレクションに追加されたジェスチャにも応答できることに注意してください。 たとえば、上記の[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)コード例では、が`Span` 2 番目のに追加されています。 この`Span`ため、がタップされる`TapGestureRecognizer`と、は[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)プロパティに`ICommand`よって定義されたを実行して応答します。 ジェスチャレコグナイザーの詳細については、「 [Xamarin のジェスチャ](~/xamarin-forms/app-fundamentals/gestures/index.md)」を参照してください。

次のスクリーンショットは、 `FormattedString`プロパティを3つ`Span`のインスタンスに設定した結果を示しています。

![ラベル FormattedText の例](label-images/formattedtext.png)

## <a name="line-height"></a>[行間]

[`Label`](xref:Xamarin.Forms.Label)と[`Span`](xref:Xamarin.Forms.Span)の垂直方向の高さをカスタマイズするには、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティまたは[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) `double`値を設定します。 IOS と Android では、これらの値は元の行の高さの乗数であり、ユニバーサル Windows プラットフォーム ( `Label.LineHeight` UWP) では、プロパティ値はラベルのフォントサイズの乗数です。

> [!NOTE]
>
> - IOS では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティ[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)とプロパティによって、1行に収まるテキストの行の高さと、複数の行に折り返されるテキストが変更されます。
> - Android では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティ[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)とプロパティは、複数の行に折り返されるテキストの行の高さのみを変更します。
> - UWP では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティは、折り返されているテキストの行の高さを複数の[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)行に変更します。このプロパティは効果がありません。

次の XAML の例は、 [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight)でプロパティを[`Label`](xref:Xamarin.Forms.Label)設定する方法を示しています。

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

同等の C# コードを次に示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

次のスクリーンショットは、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティを1.8 に設定した結果を示しています。

![ラベル LineHeight の例](label-images/label-lineheight.png)

次の XAML の例は、 [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)でプロパティを[`Span`](xref:Xamarin.Forms.Span)設定する方法を示しています。

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

同等の C# コードを次に示します。

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

次のスクリーンショットは、 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)プロパティを1.8 に設定した結果を示しています。

![Span LineHeight の例](label-images/span-lineheight.png)

## <a name="padding"></a>パディング

余白は、要素とその子要素の間のスペースを表し、要素を独自のコンテンツから分離するために使用されます。 プロパティを[`Thickness`](xref:Xamarin.Forms.Thickness)値に設定[`Label`](xref:Xamarin.Forms.Label)することによっ`Label.Padding`て、インスタンスに埋め込みを適用できます。

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

同等の C# コードを次に示します。

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
> IOS で[`Label`](xref:Xamarin.Forms.Label)は、 `Padding`プロパティを設定するが作成されると、埋め込みが適用され、埋め込み値を後で更新できるようになります。 ただし、 `Padding`プロパティが`Label`設定されていないを作成した場合、後で設定しようとしても効果はありません。
>
> Android およびユニバーサル Windows プラットフォームでは、の`Padding` `Label`作成時または後にプロパティ値を指定できます。

埋め込みの詳細については、「[余白と埋め込み](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」を参照してください。

## <a name="hyperlinks"></a>ハイパーリンク

および[`Span`](xref:Xamarin.Forms.Span)インスタンスによっ[`Label`](xref:Xamarin.Forms.Label)て表示されるテキストは、次の方法でハイパーリンクに変換できます。

1. または`TextColor` [`Span`](xref:Xamarin.Forms.Span)の`TextDecoration`プロパティとプロパティを設定します。 [`Label`](xref:Xamarin.Forms.Label)
1. を[`Label`](xref:Xamarin.Forms.Label)また[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)は[`Span`](xref:Xamarin.Forms.Span)の[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)コレクションに追加します。 [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)このプロパティは、 `ICommand`をにバインド[`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)し、そのプロパティには開く URL を格納します。
1. `ICommand`によって実行されるを定義[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)します。
1. によって実行されるコードを記述`ICommand`します。

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

ハイパーリンクがタップ[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)されると、はその`ICommand` [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)プロパティによって定義されたを実行して応答します。 さらに、 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)プロパティによって指定された URL は、 `ICommand`パラメーターとしてに渡されます。

XAML ページの分離コードには、次の`TapCommand`実装が含まれています。

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

は`TapCommand` 、 `Launcher.OpenAsync`メソッドを実行して[`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 、プロパティ値をパラメーターとして渡します。 `Launcher.OpenAsync`メソッドは、Xamarin. Essentials によって提供され、web ブラウザーで URL を開きます。 そのため、全体的な影響として、ページでハイパーリンクをタップすると、web ブラウザーが表示され、ハイパーリンクに関連付けられている URL に移動します。

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
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

クラス`HyperlinkSpan`は`Url`プロパティを定義し、関連[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)付けられており、コンストラクターはハイパーリンクの[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)外観と、ハイパーリンクがタップされたときに応答するを設定します。 `HyperlinkSpan`がタップされると、 `TapGestureRecognizer`は、 `Launcher.OpenAsync`メソッドを実行して、 `Url`プロパティによって指定された URL を web ブラウザーで開くことで応答します。

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

## <a name="styling-labels"></a>ラベルのスタイル設定

前のセクションでは[`Label`](xref:Xamarin.Forms.Label) 、 [`Span`](xref:Xamarin.Forms.Span)インスタンスごとの設定とプロパティについて説明しています。 ただし、一連のプロパティを1つのスタイルにグループ化して、1つまたは複数のビューに一貫して適用することができます。 これにより、コードの読みやすさが向上し、デザインの変更を実装しやすくなります。 詳細については、「[スタイル](~/xamarin-forms/user-interface/text/styles.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [ハイパーリンク (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Xamarin. Forms を使用した Mobile Apps の作成、第3章](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [ラベルの API](xref:Xamarin.Forms.Label)
- [スパン API](xref:Xamarin.Forms.Span)
