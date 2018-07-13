---
title: Xamarin.Forms のラベル
description: この記事では、Xamarin.Forms ラベル クラスを使用して、アプリケーションで単一と複数行のテキストを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995967"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms のラベル

_Xamarin.Forms にテキストを表示_

`Label`単一と複数行のテキストを表示するビューを使用します。 ラベルには、(ファミリ、サイズ、およびオプション) カスタムのフォントおよびカラーのテキストを持つことができます。 ここでは、次のトピックについて説明します。

- **[切り捨てと折り返し](#Truncation_and_Wrapping)** &ndash;切り捨てと折り返しテキストを 1 行に収めることはできません、状況を処理するためのオプション。
- **[フォント](#Font)** &ndash;フォント オプション。
- **[色](#Color)** &ndash;ラベル テキストの色のオプション。
- **[書式設定されたテキスト](#Formatted_Text)** &ndash;形式/スタイルの複数のインライン テキストを表示するためのオプション。

## <a name="styling-label"></a>スタイル ラベル

次のセクションでは、対象のプロパティの設定`Label`インスタンスごとに手動でします。 プロパティのセットは、一貫した方法を 1 つまたは複数のビューに適用される 1 つのスタイルに分類できます。 コードの読みやすさを向上でき設計の変更を簡単に実装できます。 参照してください[スタイル](~/xamarin-forms/user-interface/text/styles.md)詳細についてはします。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

によって公開されているいくつかの方法のいずれかで 1 つの行に収まらないテキストを処理するためにラベルを設定することができます、`LineBreakMode`プロパティ。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 次のオプションの列挙体を示します。

- **HeadTruncation** &ndash;末尾を示す、テキストの先頭を切り捨てます。
- **CharacterWrap** &ndash;文字境界でテキストを新しい行をラップします。
- **MiddleTruncation** &ndash;先頭と末尾の省略記号によって中間置換後の文字列で、テキストが表示されます。
- **NoWrap** &ndash;のみを表示するテキストを折り返しませんしたりできるだけ多くのテキストに合わせて 1 つの行にします。
- **TailTruncation** &ndash;末尾を切り捨てる、テキストの先頭を示します。
- **WordWrap** &ndash;ワード境界でテキストをラップします。

## <a name="font"></a>フォント

参照してください[フォントを操作する](~/xamarin-forms/user-interface/text/fonts.md)詳細についてはします。

## <a name="color"></a>色

`Label`s は、バインド可能なを使用してカスタムのテキストの色を使用して、設定することができます`TextColor`プロパティ。

特別な注意は、色は各プラットフォームで使用できることを確認する必要があります。 各プラットフォームには、テキストと背景の色の既定値があるためには、それぞれで動作する、既定値を選択するように注意する必要があります。

ラベルのテキストの色を設定するのにには、次のコードを使用します。

コードは次のとおりです。

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

で XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "ラベル TextColor 例")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>書式設定されたテキスト

ラベルの公開を`FormattedText`プロパティの複数のフォントとテキストを表示することができますし、同じビューでの色します。

`FormattedText`プロパティの型は[ `FormattedString`](xref:Xamarin.Forms.FormattedString)します。 1 つ以上の書式設定された文字列は、 `Span`s、それぞれ次のプロパティ。

- **BackgroundColor** &ndash;蛍光ペンの効果を実現する例については、背景の色を設定するために使用できます。
- **FontAttributes** &ndash;に設定すると、太字、斜体、またはそのどちらも指定できます。
- **FontFamily** &ndash;使用するフォントを設定します。
- **FontSize** &ndash;テキストのサイズを設定します。
- **ForegroundColor** &ndash;テキストの色を設定します。
- **テキスト**&ndash;表示するテキスト。

次の c# コードでは、場所の最初の単語が太字であり、最後の単語が赤色のラベルを示します。

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

で XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "ラベルの FormattedText の例")


## <a name="related-links"></a>関連リンク

- [第 3 章、Xamarin.Forms でモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [ラベルの API](xref:Xamarin.Forms.Label)
