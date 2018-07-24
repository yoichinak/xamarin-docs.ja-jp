---
title: Xamarin.Forms のラベル
description: この記事では、Xamarin.Forms ラベル クラスを使用して、アプリケーションで単一と複数行のテキストを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203037"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms のラベル

_Xamarin.Forms にテキストを表示_

[ `Label` ](xref:Xamarin.Forms.Label)単一および複数行のテキストを表示するビューを使用します。 ラベルには、(ファミリ、サイズ、およびオプション) カスタムのフォントおよびカラーのテキストを持つことができます。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

によって公開されているいくつかの方法のいずれかで 1 つの行に収まらないテキストを処理するためにラベルを設定することができます、`LineBreakMode`プロパティ。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 次の値を持つ列挙型を示します。

- **HeadTruncation** &ndash;末尾を示す、テキストの先頭を切り捨てます。
- **CharacterWrap** &ndash;文字境界でテキストを新しい行をラップします。
- **MiddleTruncation** &ndash;先頭と末尾の省略記号によって中間置換後の文字列で、テキストが表示されます。
- **NoWrap** &ndash;のみを表示するテキストを折り返しませんしたりできるだけ多くのテキストに合わせて 1 つの行にします。
- **TailTruncation** &ndash;末尾を切り捨てる、テキストの先頭を示します。
- **WordWrap** &ndash;ワード境界でテキストをラップします。

## <a name="fonts"></a>フォント

フォントを指定することの詳細については、`Label`を参照してください[フォント](~/xamarin-forms/user-interface/text/fonts.md)します。

## <a name="colors"></a>色

`Label`s は、バインド可能なを使用してカスタムのテキストの色を使用して、設定することができます[ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)プロパティ。

特別な注意は、色は各プラットフォームで使用できることを確認する必要があります。 各プラットフォームには、テキストと背景の色の既定値があるためには、それぞれで動作する、既定値を選択するように注意する必要があります。

ラベルのテキストの色を設定するのにには、次の XAML を使用します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

同等の c# コードに示します。

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

次のスクリーン ショットの設定の結果を表示する、`TextColor`プロパティ。

![](label-images/textcolor.png "ラベル TextColor 例")

色の詳細については、次を参照してください。[色](~/xamarin-forms/user-interface/colors.md)します。

<a name="Formatted_Text" />

## <a name="formatted-text"></a>書式設定されたテキスト

ラベルの公開、 [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText)プロパティにより、複数のフォントとテキストの表示と同じビューでの色します。

`FormattedText`プロパティの型は[ `FormattedString` ](xref:Xamarin.Forms.FormattedString)、1 つまたは複数で構成される[ `Span` ](xref:Xamarin.Forms.Span)設定を使用して、インスタンス、 [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans)プロパティ. 各`Span`次のプロパティを所有します。

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – span の背景の色。
- [`Font`](xref:Xamarin.Forms.Span.Font) -範囲内のテキストのフォント。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) -範囲内のテキストのフォント属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) -範囲内のテキストのフォントが所属するフォント ファミリ。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) -範囲内のテキストのフォントのサイズ。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) -範囲内のテキストの色。 このプロパティは廃止され、置き換えられましたが、`TextColor`プロパティ。
- [`Style`](xref:Xamarin.Forms.Span.Style) – スパンに適用するスタイル。
- [`Text`](xref:Xamarin.Forms.Span.Text) – スパンのテキスト。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) -範囲内のテキストの色。

XAML の例を次に示します、`FormattedText`プロパティの 3 つで構成される`Span`インスタンス。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

同等の c# コードに示します。

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> [ `Text` ](xref:Xamarin.Forms.Span.Text)のプロパティを`Span`データ バインドを通じて設定できます。 詳細については、次を参照してください。[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md)します。

次のスクリーン ショットの設定の結果を表示する、`FormattedString`プロパティを 3 つ`Span`インスタンス。

![](label-images/formattedtext.png "ラベルの FormattedText の例")

## <a name="styling-a-label"></a>ラベルのスタイル設定

前のセクションでは、対象設定[ `Label` ](xref:Xamarin.Forms.Label) -インスタンスごとのプロパティ。 ただし、プロパティのセットは、1 つまたは複数のビューに一貫して適用されるスタイルを 1 つにグループ化できます。 コードの読みやすさを向上でき設計の変更を簡単に実装できます。 詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/text/styles.md)します。

## <a name="related-links"></a>関連リンク

- [第 3 章、Xamarin.Forms でモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [ラベルの API](xref:Xamarin.Forms.Label)
