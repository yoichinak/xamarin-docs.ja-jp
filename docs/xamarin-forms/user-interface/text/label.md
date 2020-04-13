---
title: Xamarin.Forms ラベル
description: この資料では、Xamarin.Forms ラベル クラスを使用して、アプリケーションで単一行と複数行のテキストを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: bed6b8a774ecb352427f16b78d10e50821f92701
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987596"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms ラベル

[![サンプルの](~/media/shared/download.png)ダウンロード サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.Forms でテキストを表示する_

この[`Label`](xref:Xamarin.Forms.Label)ビューは、単一行と複数行の両方のテキストを表示するために使用されます。 ラベルには、文字装飾、色付きのテキスト、カスタム フォント (ファミリ、サイズ、オプション) を使用できます。

## <a name="text-decorations"></a>文字装飾

下線と取り消し線の文字装飾は[`Label`](xref:Xamarin.Forms.Label)、プロパティを 1`Label.TextDecorations`つ以上`TextDecorations`の列挙メンバーに設定することで、インスタンスに適用できます。

- `None`
- `Underline`
- `Strikethrough`

次の XAML の例では`Label.TextDecorations`、プロパティを設定します。

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

該当の C# コードを次に示します。

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

次のスクリーンショットは、インスタンス`TextDecorations`に適用される列挙[`Label`](xref:Xamarin.Forms.Label)型メンバーを示しています。

![文字装飾を含むラベル](label-images/label-textdecorations.png)

> [!NOTE]
> テキスト装飾は、インスタンスに[`Span`](xref:Xamarin.Forms.Span)適用することもできます。 クラスの詳細については、「`Span`[書式付きテキスト](#Formatted_Text)」を参照してください。

## <a name="character-spacing"></a>文字間隔

文字間隔は、プロパティを[`Label`](xref:Xamarin.Forms.Label)値に設定することによってインスタンスに`double`適用できます。 `Label.CharacterSpacing`

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

該当の C# コードを次に示します。

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

その結果、 によって表示されるテキスト内の文字[`Label`](xref:Xamarin.Forms.Label)は、間隔`CharacterSpacing`が設定された装置に依存しない単位で区切られます。

## <a name="new-lines"></a>改行

XAML から、新しい行にテキストを[`Label`](xref:Xamarin.Forms.Label)強制的に追加するには、主に 2 つの方法があります。

1. ユニコードラインフィード文字を使用します("&#10;" )。
1. *プロパティ要素*構文を使用してテキストを指定します。

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

C# では、テキストを "\n" 文字で新しい行に強制的に挿入できます。

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>色

ラベルは、バインド可能[`TextColor`](xref:Xamarin.Forms.Label.TextColor)なプロパティを使用してカスタム テキストの色を使用するように設定できます。

各プラットフォームで色が使用できるように、特別な注意が必要です。 各プラットフォームは、テキストと背景色の異なる既定値を持っているので、それぞれに動作する既定値を選択するように注意する必要があります。

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

該当の C# コードを次に示します。

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

次のスクリーンショットは、プロパティを設定した結果`TextColor`を示しています。

![ラベルテキストの色の例](label-images/textcolor.png)

色の詳細については、「[色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="fonts"></a>フォント

でのフォント`Label`の指定の詳細については、「 [Fonts](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てとラッピング

ラベルは、プロパティによって公開される、いくつかの方法のいずれかで 1 行に収まらないテキストを処理するように`LineBreakMode`設定できます。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)は、次の値を持つ列挙体です。

- **ヘッドトランケーションは**&ndash;、テキストの先頭を切り捨て、末尾を示します。
- **文字ラップ**&ndash;は、文字境界の新しい行にテキストを折り返します。
- **MiddleTruncation は**&ndash;テキストの先頭と末尾を表示し、中央は省略記号で置き換えます。
- **NoWrap**&ndash;はテキストを折り返さないため、1 行に収まるだけのテキストしか表示されません。
- **TailTruncation は**&ndash;、テキストの先頭を示し、末尾を切り捨てします。
- **ワードラップ**&ndash;は、ワード境界でテキストを折り返します。

## <a name="display-a-specific-number-of-lines"></a>特定の行数を表示する

によって表示される行数は[`Label`](xref:Xamarin.Forms.Label)、プロパティに`Label.MaxLines``int`値を設定することによって指定できます。

- 既定値`MaxLines`である -1 の場合、[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値`Label`は、1 行だけ、切り捨て、またはすべてのテキストを含むすべての行を表示するように指定します。
- 0`MaxLines`の場合は`Label`、 は表示されません。
- 1`MaxLines`の場合、結果はプロパティ[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)を[`NoWrap`](xref:Xamarin.Forms.LineBreakMode)、 、、[`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode)または[`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)[`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)に設定することと同じになります。 ただし、`Label`省略記号の[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)配置に関しては、プロパティの値を尊重します(該当する場合)。
- 1`MaxLines`より大きい場合、`Label`省略記号の配置に関する[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティの値を考慮しながら、指定された行数まで表示されます (該当する場合)。 ただし、プロパティを`MaxLines`1 より大きい値に設定しても、[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティが[`NoWrap`](xref:Xamarin.Forms.LineBreakMode)に設定されている場合は何の効果もありません。

次の XAML の例では`MaxLines`、 に[`Label`](xref:Xamarin.Forms.Label)プロパティを設定する方法を示します。

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

該当の C# コードを次に示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

次のスクリーンショットは、テキストが 2`MaxLines`行を超える長さである場合に、プロパティを 2 に設定した結果を示しています。

![ラベルの最大線の例](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML の表示

クラスには、インスタンス`TextType`がプレーン テキストまたは`Label`HTML テキストのどちらを表示するかを決定するプロパティがあります。 [`Label`](xref:Xamarin.Forms.Label) このプロパティは、列挙体のメンバーのいずれかに設定する`TextType`必要があります。

- `Text`は、プレーン`Label`テキストを表示することを示`Label.TextType`し、プロパティの既定値です。
- `Html`は、HTML`Label`テキストを表示することを示します。

したがって、[`Label`](xref:Xamarin.Forms.Label)インスタンスは プロパティを`Label.TextType``Html`に設定し、プロパティを`Label.Text`HTML 文字列に設定することで HTML を表示できます。

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

上記の例では、HTML の二重引用符をシンボルを使用してエスケープする`\`必要があります。

XAML では、HTML 文字列は`<`、 および`>`シンボルをさらにエスケープするため、読み取れなくなる可能性があります。

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

また、HTML を`CDATA`セクション内にインライン化することもできます。

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

この例では、`Label.Text`プロパティは、セクション内でインライン表示されている HTML 文字列に`CDATA`設定されます。 これは、プロパティが`Text`クラス`ContentProperty`用であるために機能`Label`します。

次のスクリーン ショットは[`Label`](xref:Xamarin.Forms.Label)、表示される HTML を示しています。

![iOS と Android で HTML を表示するラベルのスクリーンショット](label-images/label-html.png)

> [!IMPORTANT]
> での HTML[`Label`](xref:Xamarin.Forms.Label)の表示は、基盤となるプラットフォームでサポートされている HTML タグに限定されます。

<a name="Formatted_Text" />

## <a name="formatted-text"></a>書式付きテキスト

ラベルは、[`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)同じビューで複数のフォントと色を持つテキストを表示できるようにするプロパティを公開します。

プロパティ`FormattedText`は、プロパティを[`FormattedString`](xref:Xamarin.Forms.FormattedString)介して設定された 1[`Span`](xref:Xamarin.Forms.Span)つ以上のインスタンスで構成[`Spans`](xref:Xamarin.Forms.FormattedString.Spans)される type です。 次`Span`のプロパティを使用して、外観を設定できます。

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)– スパンの背景の色。
- `CharacterSpacing`: `double` 型、`Span` テキストの文字間の間隔。
- [`Font`](xref:Xamarin.Forms.Span.Font)– スパン内のテキストのフォント。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)– スパン内のテキストのフォント属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)– スパン内のテキストのフォントが属するフォントファミリー。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)– スパン内のテキストのフォントのサイズ。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)– スパン内のテキストの色。 このプロパティは廃止され、`TextColor`プロパティに置き換えられました。
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)- スパンの既定の行の高さに適用する乗数。 詳細については、「[ラインの高さ](#line-height)」を参照してください。
- [`Style`](xref:Xamarin.Forms.Span.Style)– スパンに適用するスタイル。
- [`Text`](xref:Xamarin.Forms.Span.Text)– スパンのテキスト。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)– スパン内のテキストの色。
- `TextDecorations`- スパン内のテキストに適用する装飾。 詳細については、「[文字装飾](#text-decorations)」を参照してください。

[`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)、 [`Text`](xref:Xamarin.Forms.Span.Text)、および[`Text`](xref:Xamarin.Forms.Span.Text)バインド可能なプロパティには、 の既定[`OneWay`](xref:Xamarin.Forms.BindingMode)のバインド モードがあります。 このバインディング モードの詳細については、『[バインディング モード ガイド』の「デフォルトのバインド](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)[モード](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)」を参照してください。

さらに、この[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)プロパティを使用して、[`Span`](xref:Xamarin.Forms.Span)でジェスチャに応答するジェスチャ 認識機能のコレクションを定義できます。

> [!NOTE]
> HTML を[`Span`](xref:Xamarin.Forms.Span)に表示することはできません。

次の XAML の例`FormattedText`は、3 つの[`Span`](xref:Xamarin.Forms.Span)インスタンスで構成されるプロパティを示しています。

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

該当の C# コードを次に示します。

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
> の[`Text`](xref:Xamarin.Forms.Span.Text)プロパティは`Span`、データ バインディングを通じて設定できます。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。

また[`Span`](xref:Xamarin.Forms.Span)、span の[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)コレクションに追加されたジェスチャにも応答できます。 たとえば、[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)上記のコード例の 2`Span`番目に a が追加されています。 したがって、`Span`これがタップされると`TapGestureRecognizer`、プロパティで定義されたを`ICommand`実行して応答します。 [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) ジェスチャ認識エンジンの詳細については、「 [Xamarin.Forms ジェスチャ](~/xamarin-forms/app-fundamentals/gestures/index.md)」を参照してください。

次のスクリーンショットは、プロパティを 3`FormattedString`つの`Span`インスタンスに設定した結果を示しています。

![ラベル書式付きテキストの例](label-images/formattedtext.png)

## <a name="line-height"></a>[行間]

[`Label`](xref:Xamarin.Forms.Label)と a[`Span`](xref:Xamarin.Forms.Span)の垂直方向の高さは、プロパティ[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)または[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)`double`値を設定することによってカスタマイズできます。 iOS および Android では、これらの値は元の行の高さの乗数であり、ユニバーサル Windows プラットフォーム`Label.LineHeight`(UWP) では、プロパティ値はラベルのフォント サイズの乗数です。

> [!NOTE]
>
> - iOS では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)および プロパティは、1 行に収まるテキストと、複数行に折り返されるテキストの行の高さを変更します。
> - Android では、 [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)および プロパティは、複数行に折り返すテキストの行の高さを変更するだけです。
> - UWP では、[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)プロパティは、複数行に折り返すテキストの行の高さを変更[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)し、プロパティは効果がありません。

次の XAML の例では[`LineHeight`](xref:Xamarin.Forms.Label.LineHeight)、 に[`Label`](xref:Xamarin.Forms.Label)プロパティを設定する方法を示します。

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

該当の C# コードを次に示します。

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

次のスクリーンショットは、プロパティを 1.8 に設定した[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)結果を示しています。

![ラベル行の高さの例](label-images/label-lineheight.png)

次の XAML の例では[`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)、 に[`Span`](xref:Xamarin.Forms.Span)プロパティを設定する方法を示します。

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

該当の C# コードを次に示します。

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

次のスクリーンショットは、プロパティを 1.8 に設定した[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)結果を示しています。

![スパン ラインの高さの例](label-images/span-lineheight.png)

## <a name="padding"></a>パディング

Padding は、要素とその子要素の間のスペースを表し、要素をその要素の内容から分離するために使用されます。 埋め込みは、[`Label`](xref:Xamarin.Forms.Label)プロパティに値を`Label.Padding`設定することでインスタンスに[`Thickness`](xref:Xamarin.Forms.Thickness)適用できます。

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

該当の C# コードを次に示します。

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
> iOS では、[`Label`](xref:Xamarin.Forms.Label)`Padding`プロパティを設定する a が作成されると、パディングが適用され、後でパディング値を更新できます。 ただし、`Padding`プロパティを`Label`設定しない a が作成された場合、後で設定しても効果はありません。
>
> Android およびユニバーサル Windows プラットフォームでは`Padding`、 プロパティ値は、`Label`が作成されるとき、または後で指定できます。

パディングの詳細については、「[余白とパディング](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」を参照してください。

## <a name="hyperlinks"></a>ハイパーリンク

インスタンスによって[`Label`](xref:Xamarin.Forms.Label)[`Span`](xref:Xamarin.Forms.Span)表示されるテキストは、次の方法でハイパーリンクに変換できます。

1. または[`Span`](xref:Xamarin.Forms.Span)`TextColor`の`TextDecoration`プロパティと[`Label`](xref:Xamarin.Forms.Label)プロパティを設定します。
1. プロパティ[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)が にバインド[`Label`](xref:Xamarin.Forms.Label)[`Span`](xref:Xamarin.Forms.Span)[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)`ICommand`され、プロパティに開く URL が含まれている、 または のコレクションに を追加します。 [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)
1. によって実行`ICommand`される を定義します[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)。
1. によって実行されるコードを記述します`ICommand`。

次のコード例は[、Hyperlink Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)サンプルから取得し[`Label`](xref:Xamarin.Forms.Label)、複数[`Span`](xref:Xamarin.Forms.Span)のインスタンスから設定されたコンテンツを示しています。

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

この例では、最初と 3[`Span`](xref:Xamarin.Forms.Span)番目のインスタンスがテキストで構成`Span`され、2 番目のインスタンスはタップ可能なハイパーリンクを表します。 テキストカラーが青に設定され、下線の文字装飾が付いています。 次のスクリーンショットに示すように、ハイパーリンクの外観が作成されます。

[![ハイパーリンク](label-images/hyperlinks-small.png "ハイパーリンク")](label-images/hyperlinks-large.png#lightbox)

ハイパーリンクがタップされると、 プロパティ[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)で`ICommand`[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)定義された を実行して応答します。 また、[`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)プロパティで指定された URL がパラメーター`ICommand`として渡されます。

XAML ページの分離コードには、実装が`TapCommand`含まれています。

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

メソッド`TapCommand`を実行`Launcher.OpenAsync`し、プロパティ値[`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)をパラメーターとして渡します。 この`Launcher.OpenAsync`メソッドは Xamarin.Essentials によって提供され、Url が Web ブラウザーで開きます。 したがって、全体的な効果は、ハイパーリンクがページ上でタップされると、Web ブラウザーが表示され、ハイパーリンクに関連付けられている URL に移動することです。

### <a name="creating-a-reusable-hyperlink-class"></a>再利用可能なハイパーリンク クラスの作成

以前の方法でハイパーリンクを作成するには、アプリケーションでハイパーリンクが必要になるたびに繰り返しコードを記述する必要があります。 ただし、 と[`Label`](xref:Xamarin.Forms.Label)[`Span`](xref:Xamarin.Forms.Span)クラスの両方をサブクラス化して`HyperlinkLabel`作成`HyperlinkSpan`し、クラスを作成し、そこにテキスト形式のコードを追加して、クラスを作成できます。

次のコード例は、ハイパーリンク[デモサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)から取得し、クラス`HyperlinkSpan`を示しています。

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

クラス`HyperlinkSpan`は`Url`プロパティと関連付けられた[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)を定義し、コンストラクターはハイパーリンクの外観を[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)設定し、ハイパーリンクがタップされたときに応答します。 タップ`HyperlinkSpan`されると、`TapGestureRecognizer`は、プロパティで指定された URL`Launcher.OpenAsync`を Web ブラウザーで開く`Url`メソッドを実行して応答します。

クラス`HyperlinkSpan`は、クラスのインスタンスを XAML に追加することで使用できます。

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

前のセクションでは、[`Label`](xref:Xamarin.Forms.Label)インスタンス[`Span`](xref:Xamarin.Forms.Span)ごとに設定とプロパティについて説明しました。 ただし、プロパティのセットは、1 つまたは複数のビューに一貫して適用される 1 つのスタイルにグループ化できます。 これにより、コードの読みやすさが向上し、デザインの変更を実装しやすくなります。 詳細については、「[スタイル](~/xamarin-forms/user-interface/text/styles.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [テキスト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [ハイパーリンク (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Xamarin.Forms を使用したモバイル アプリケーションの作成 第 3 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [ラベルの API](xref:Xamarin.Forms.Label)
- [スパン API](xref:Xamarin.Forms.Span)
