---
title: Xamarin.Forms のラベル
description: この記事では、Xamarin.Forms ラベル クラスを使用して、アプリケーションで単一と複数行のテキストを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/28/2019
ms.openlocfilehash: 64fa15a15468a84ada3a377a9ac85bbf6310099c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305589"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms のラベル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin 形式でテキストを表示する_

[`Label`](xref:Xamarin.Forms.Label)ビューは、1つのテキストと複数行のテキストを表示するために使用されます。 ラベルは、テキストを装飾したり、テキストに色を付けたり、カスタム フォント (ファミリ、サイズ、およびオプション) を使用することができます。

## <a name="text-decorations"></a>文字装飾

`Label.TextDecorations` プロパティを1つ以上の `TextDecorations` 列挙メンバーに設定することにより、下線および取り消し線の文字装飾を[`Label`](xref:Xamarin.Forms.Label)インスタンスに適用できます。

- `None`
- `Underline`
- `Strikethrough`

次の XAML の例は、`Label.TextDecorations` プロパティを設定する方法を示しています。

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

次のスクリーンショットは、 [`Label`](xref:Xamarin.Forms.Label)インスタンスに適用される `TextDecorations` 列挙型メンバーを示しています。

![文字装飾付きのラベル](label-images/label-textdecorations.png)

> [!NOTE]
> 文字装飾は[`Span`](xref:Xamarin.Forms.Span)インスタンスにも適用できます。 `Span` クラスの詳細については、「[書式設定](#Formatted_Text)されたテキスト」を参照してください。

## <a name="character-spacing"></a>文字間隔

文字間隔は、`Label.CharacterSpacing` プロパティを `double` 値に設定することによって[`Label`](xref:Xamarin.Forms.Label)インスタンスに適用できます。

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

同等の C# コードを次に示します。

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

結果として、 [`Label`](xref:Xamarin.Forms.Label)によって表示されるテキスト内の文字は、デバイスに依存しない単位 `CharacterSpacing` 間隔が区別されます。

## <a name="colors"></a>色

ラベルは、バインド可能な[`TextColor`](xref:Xamarin.Forms.Label.TextColor)プロパティを使用して、カスタムテキストの色を使用するように設定できます。

各プラットフォームで色を使用するには特別な注意が必要です。 各プラットフォームごとに、テキストと背景の色の既定値があるため、それぞれに適した既定値を選択するよう注意する必要があります。

次の XAML の例では、`Label`のテキストの色を設定します。

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

次のスクリーンショットは、`TextColor` プロパティを設定した結果を示しています。

![ラベルの TextColor の例](label-images/textcolor.png)

色の詳細については、「[色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="fonts"></a>フォント

`Label`でフォントを指定する方法の詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

ラベルは、`LineBreakMode` プロパティによって公開される、複数の方法のいずれかで1行に収めることができないテキストを処理するように設定できます。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)は、次の値を持つ列挙体です。

- ヘッド**切り捨て**&ndash; テキストの先頭を切り捨て、末尾を表示します。
- 文字の**折り返し**&ndash; 文字の境界でテキストを新しい行にラップします。
- **MiddleTruncation** &ndash; には、テキストの先頭と末尾が表示され、中央のは省略記号で置き換えられます。
- **NoWrap** &ndash; では、テキストを折り返しません。1行に収まる程度のテキストだけが表示されます。
- **TailTruncation** &ndash; テキストの先頭を示し、末尾を切り捨てます。
- **ワードラップ &ndash; ワード**境界でテキストをラップします。

## <a name="display-a-specific-number-of-lines"></a>特定の行数を表示する

[`Label`](xref:Xamarin.Forms.Label)によって表示される行数は、`Label.MaxLines` プロパティを `int` 値に設定することによって指定できます。

- `MaxLines` が-1 (既定値) の場合、`Label` は、 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値に従い、1行だけを表示するか、切り捨てられるか、またはすべてのテキストを含むすべての行を表示します。
- `MaxLines` が0の場合、`Label` は表示されません。
- `MaxLines` が1の場合、結果は[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティを[`NoWrap`](xref:Xamarin.Forms.LineBreakMode)、 [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode)、 [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)、または[`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)に設定した場合と同じになります。 ただし、`Label` は、省略記号の配置 (該当する場合) に関して、 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値を尊重します。
- `MaxLines` が1より大きい場合、`Label` は指定された行数まで表示され、必要に応じて、省略記号の配置に関して[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値が考慮されます。 ただし、 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティが[`NoWrap`](xref:Xamarin.Forms.LineBreakMode)に設定されている場合、`MaxLines` プロパティを1より大きい値に設定しても効果はありません。

次の XAML の例は、 [`Label`](xref:Xamarin.Forms.Label)の `MaxLines` プロパティを設定する方法を示しています。

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

次のスクリーンショットは、`MaxLines` プロパティを2に設定した結果を示しています。これは、テキストが2行を超えて占有するのに十分な長さです。

![MaxLines のラベルの例](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML の表示

[`Label`](xref:Xamarin.Forms.Label)クラスには `TextType` プロパティがあり、`Label` インスタンスでプレーンテキストを表示するか、HTML テキストを表示するかを決定します。 このプロパティは、`TextType` 列挙体のいずれかのメンバーに設定する必要があります。

- `Text` は、`Label` がプレーンテキストを表示し、`Label.TextType` プロパティの既定値であることを示します。
- `Html` は、`Label` に HTML テキストが表示されることを示します。

したがって、 [`Label`](xref:Xamarin.Forms.Label)インスタンスは、`Label.TextType` プロパティを `Html`に、`Label.Text` プロパティを html 文字列に設定することにより、html を表示できます。

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

上記の例では、HTML の二重引用符文字を `\` 記号を使用してエスケープする必要があります。

XAML では、`<` と `>` シンボルをさらにエスケープすることにより、HTML 文字列を読み取ることができなくなる可能性があります。

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

または、読みやすくするために、`CDATA` セクションで HTML をインライン化することもできます。

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

この例では、`Label.Text` プロパティは、`CDATA` セクションにインライン展開されている HTML 文字列に設定されています。 これは、`Text` プロパティが `Label` クラスの `ContentProperty` であるために機能します。

次のスクリーンショットは、HTML を表示する[`Label`](xref:Xamarin.Forms.Label)を示しています。

![IOS と Android での HTML を表示するラベルのスクリーンショット](label-images/label-html.png)

> [!IMPORTANT]
> [`Label`](xref:Xamarin.Forms.Label)に html を表示することは、基になるプラットフォームでサポートされている html タグに限定されます。

<a name="Formatted_Text" />

## <a name="formatted-text"></a>書式付きテキスト

ラベルは[`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)プロパティを公開します。これにより、同じビューの複数のフォントと色を持つテキストを表示できます。

`FormattedText` プロパティの型は[`FormattedString`](xref:Xamarin.Forms.FormattedString)で、 [`Spans`](xref:Xamarin.Forms.FormattedString.Spans)プロパティを使用して設定された1つ以上の[`Span`](xref:Xamarin.Forms.Span)インスタンスで構成されています。 次の `Span` プロパティを使用して、外観を設定できます。

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) –スパンの背景の色です。
- `CharacterSpacing`: `double` 型、`Span` テキストの文字間の間隔。
- [`Font`](xref:Xamarin.Forms.Span.Font) –スパン内のテキストのフォント。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) –スパン内のテキストのフォント属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) –スパン内のテキストのフォントが属するフォントファミリ。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) –スパン内のテキストのフォントのサイズ。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) –スパン内のテキストの色です。 このプロパティは互換性のために残されており、`TextColor` プロパティによって置き換えられています。
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -スパンの既定の行の高さに適用する乗数。 詳細については、「[行の高さ](#line-height)」を参照してください。
- [`Style`](xref:Xamarin.Forms.Span.Style) –スパンに適用するスタイル。
- [`Text`](xref:Xamarin.Forms.Span.Text) –スパンのテキスト。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) –スパン内のテキストの色です。
- `TextDecorations`-スパン内のテキストに適用する装飾。 詳細については、「[文字装飾](#text-decorations)」を参照してください。

[`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)、 [`Text`](xref:Xamarin.Forms.Span.Text)、および[`Text`](xref:Xamarin.Forms.Span.Text)のバインド可能なプロパティには、 [`OneWay`](xref:Xamarin.Forms.BindingMode)の既定のバインディングモードがあります。 このバインディングモードの詳細については、「[バインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)ガイド」の[既定のバインドモード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)に関する説明を参照してください。

また、 [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)プロパティを使用して、 [`Span`](xref:Xamarin.Forms.Span)のジェスチャに応答するジェスチャレコグナイザーのコレクションを定義することもできます。

> [!NOTE]
> [`Span`](xref:Xamarin.Forms.Span)に HTML を表示することはできません。

次の XAML の例は、3つの[`Span`](xref:Xamarin.Forms.Span)インスタンスで構成される `FormattedText` プロパティを示しています。

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
> `Span` の[`Text`](xref:Xamarin.Forms.Span.Text)プロパティは、データバインディングを使用して設定できます。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。

[`Span`](xref:Xamarin.Forms.Span)は、スパンの[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)コレクションに追加されたジェスチャにも応答することに注意してください。 たとえば、上記のコード例では、2番目の `Span` に[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)が追加されています。 したがって、この `Span` がタップされると、`TapGestureRecognizer` は[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)プロパティによって定義された `ICommand` を実行することによって応答します。 ジェスチャレコグナイザーの詳細については、「 [Xamarin のジェスチャ](~/xamarin-forms/app-fundamentals/gestures/index.md)」を参照してください。

次のスクリーンショットは、`FormattedString` プロパティを3つの `Span` インスタンスに設定した結果を示しています。

![ラベル FormattedText の例](label-images/formattedtext.png)

## <a name="line-height"></a>[行間]

[`Label`](xref:Xamarin.Forms.Label)と[`Span`](xref:Xamarin.Forms.Span)の垂直方向の高さは、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティまたは[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)を `double` 値に設定することによってカスタマイズできます。 IOS と Android では、これらの値は元の行の高さの乗数であり、ユニバーサル Windows プラットフォーム (UWP) では、`Label.LineHeight` プロパティ値はラベルのフォントサイズの乗数です。

> [!NOTE]
>
> - IOS では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティと[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)プロパティによって、1行に収まるテキストの行の高さと、複数の行に折り返されるテキストが変更されます。
> - Android では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティと[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)プロパティによって、複数の行に折り返されるテキストの行の高さのみが変更されます。
> - UWP では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティによって、複数の行に折り返されるテキストの行の高さが変更され、 [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)プロパティは無効になります。

次の XAML の例は、 [`Label`](xref:Xamarin.Forms.Label)の[`LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティを設定する方法を示しています。

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

次の XAML の例は、 [`Span`](xref:Xamarin.Forms.Span)の[`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)プロパティを設定する方法を示しています。

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

余白は、要素とその子要素の間のスペースを表し、要素を独自のコンテンツから分離するために使用されます。 パディングは、`Label.Padding` プロパティを[`Thickness`](xref:Xamarin.Forms.Thickness)値に設定することによって[`Label`](xref:Xamarin.Forms.Label)インスタンスに適用できます。

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
> IOS では、`Padding` プロパティを設定する[`Label`](xref:Xamarin.Forms.Label)が作成されると、パディングが適用され、埋め込み値を後で更新できるようになります。 ただし、`Padding` プロパティを設定しない `Label` が作成された場合、後で設定しようとしても効果はありません。
>
> Android およびユニバーサル Windows プラットフォームでは、`Padding` プロパティの値は `Label` の作成時に指定するか、後で指定することができます。

埋め込みの詳細については、「[余白と埋め込み](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」を参照してください。

## <a name="hyperlinks"></a>ハイパーリンク

[`Label`](xref:Xamarin.Forms.Label)と[`Span`](xref:Xamarin.Forms.Span)インスタンスによって表示されるテキストは、次の方法でハイパーリンクに変換できます。

1. [`Label`](xref:Xamarin.Forms.Label)または[`Span`](xref:Xamarin.Forms.Span)の `TextColor` と `TextDecoration` のプロパティを設定します。
1. [`Command`プロパティが](xref:Xamarin.Forms.TapGestureRecognizer.Command)`ICommand`にバインドされている[`Label`](xref:Xamarin.Forms.Label)または[`Span`](xref:Xamarin.Forms.Span)の[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)コレクションに[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)を追加します。この[`CommandParameter`プロパティに](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)は、開く URL が含まれています。
1. [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)によって実行される `ICommand` を定義します。
1. `ICommand`によって実行されるコードを記述します。

次のコード例は、ハイパーリンクの[デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)のサンプルから抜粋した、複数の[`Span`](xref:Xamarin.Forms.Span)インスタンスからコンテンツが設定されている[`Label`](xref:Xamarin.Forms.Label)を示しています。

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

この例では、1番目と3番目の[`Span`](xref:Xamarin.Forms.Span)インスタンスがテキストを構成し、2番目の `Span` は tappable ハイパーリンクを表します。 テキストの色が青色に設定され、下線が付きます。 これにより、次のスクリーンショットに示すように、ハイパーリンクの外観が作成されます。

[![ハイパーリンク](label-images/hyperlinks-small.png "ハイパーリンク")](label-images/hyperlinks-large.png#lightbox)

ハイパーリンクがタップされると、 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)は[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)プロパティによって定義された `ICommand` を実行して応答します。 また、 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)プロパティによって指定された URL が `ICommand` にパラメーターとして渡されます。

XAML ページの分離コードには、`TapCommand` の実装が含まれています。

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

`TapCommand` は、 [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)プロパティ値をパラメーターとして渡して、`Launcher.OpenAsync` メソッドを実行します。 `Launcher.OpenAsync` メソッドは、Xamarin. Essentials によって提供され、web ブラウザーで URL を開きます。 そのため、全体的な影響として、ページでハイパーリンクをタップすると、web ブラウザーが表示され、ハイパーリンクに関連付けられている URL に移動します。

### <a name="creating-a-reusable-hyperlink-class"></a>再利用可能なハイパーリンククラスを作成する

前の方法でハイパーリンクを作成するには、アプリケーションでハイパーリンクが必要になるたびに繰り返しコードを記述する必要があります。 ただし、 [`Label`](xref:Xamarin.Forms.Label)クラスと[`Span`](xref:Xamarin.Forms.Span)クラスの両方をサブクラス化して、`HyperlinkLabel` および `HyperlinkSpan` クラスを作成できます。これには、ジェスチャ認識エンジンとテキスト書式コードが追加されています。

次のコード例は、ハイパーリンクの[デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)のサンプルから抜粋した `HyperlinkSpan` クラスを示しています。

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

`HyperlinkSpan` クラスは `Url` プロパティと関連付けられた[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)を定義し、コンストラクターはハイパーリンクの外観と、ハイパーリンクがタップされたときに応答する[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)を設定します。 `HyperlinkSpan` がタップされると、`TapGestureRecognizer` は `Launcher.OpenAsync` メソッドを実行して、`Url` プロパティによって指定された URL を web ブラウザーで開くことで応答します。

`HyperlinkSpan` クラスは、クラスのインスタンスを XAML に追加することによって使用できます。

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

前のセクションでは、インスタンスごとにプロパティを[`Label`](xref:Xamarin.Forms.Label)および[`Span`](xref:Xamarin.Forms.Span)する設定について説明しています。 ただし、プロパティのセットは、1 つまたは複数のビューに一貫して適用されるように、1 つのスタイルにグループ化できます。 これによりコードの読みやすさを向上でき設計の変更を簡単に実装しやすくなります。 詳細については、「[スタイル](~/xamarin-forms/user-interface/text/styles.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [ハイパーリンク (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Xamarin. Forms を使用した Mobile Apps の作成、第3章](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [ラベルの API](xref:Xamarin.Forms.Label)
- [スパン API](xref:Xamarin.Forms.Span)
