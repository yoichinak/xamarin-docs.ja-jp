---
title: group1
description: Xamarin.Forms でテキストを表示します。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e0caa12136feb84d22ec4b90b84f3f92a601e0c0
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847512"
---
# <a name="label"></a>group1

_Xamarin.Forms でテキストを表示します。_

`Label`テキスト、単一およびマルチ ラインを表示するビューを使用します。 ラベルには、(ファミリ、サイズ、およびオプション) カスタム フォントおよびカラーのテキストを持つことができます。 ここでは、次のトピックについて説明します。

- **[切り捨てと折り返し](#Truncation_and_Wrapping)** &ndash;切り捨てとテキスト 1 行に収まらないで状況を処理するためのオプションをラップします。
- **[フォント](#Font)** &ndash;フォント オプション。
- **[色](#Color)** &ndash;ラベル テキストの色のオプションです。
- **[書式付きテキスト](#Formatted_Text)** &ndash;テキストの書式とスタイルの複数のインラインを表示するためのオプションです。

## <a name="styling-label"></a>ラベルのスタイル処理

次のセクションでは、対象のプロパティの設定`Label`-インスタンスごとに手動でします。 プロパティのセットは、1 つまたは複数のビューに一貫して適用される 1 つのスタイルに分類できます。 コードの読みやすさを向上し、設計の変更を簡単に実装する、このできます。 参照してください[スタイル](~/xamarin-forms/user-interface/text/styles.md)詳細についてはします。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>切り捨てと折り返し

によって公開されているいくつかの方法のいずれかで 1 つの行に収まらないテキスト処理にラベルを設定することができます、`LineBreakMode`プロパティです。 [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) 次のオプションの列挙型を示します。

- **HeadTruncation** &ndash;末尾を示す、テキストの先頭を切り捨てます。
- **CharacterWrap** &ndash;文字境界で新しい行にテキストをラップします。
- **MiddleTruncation** &ndash;の先頭と末尾の省略記号によって中間置換後の文字列で、テキストが表示されます。
- **[Nowrap]** &ndash;のみを表示するテキストを折り返しませんと多くのテキストに合わせて 1 つの行にします。
- **TailTruncation** &ndash;最後の切り捨て、テキストの先頭を示します。
- **右端で折り返す**&ndash;ワード境界でテキストが折り返されます。

## <a name="font"></a>フォント

参照してください[フォントを操作する](~/xamarin-forms/user-interface/text/fonts.md)詳細についてはします。

## <a name="color"></a>色

`Label`s は、バインド可能なを使用してカスタム テキスト色を使用する設定できます`TextColor`プロパティです。

特別な注意は、色を各プラットフォームで使用可能になることを確認する必要があります。 各プラットフォームには、テキスト色と背景色ごとに異なる既定があるために、各で動作する、既定値を取得するように注意する必要があります。

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

XAML:

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

## <a name="formatted-text"></a>書式設定テキスト

ラベルの公開、`FormattedText`プロパティと同じビューで色の複数のフォントのテキストを表示することができます。

`FormattedText`プロパティの型は[ `FormattedString`](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/)です。 1 つ以上の書式設定された文字列が構成される`Span`s、次のプロパティを持つ 1 つずつ。

- **BackgroundColor** &ndash;蛍光ペンの効果を実現する例については、背景色を設定するために使用できます。
- **FontAttributes** &ndash;太字に設定する、斜体、またはそのどちらも指定できます。
- **FontFamily** &ndash;使用するフォントを設定します。
- **FontSize** &ndash;テキストのサイズを設定します。
- **ForegroundColor** &ndash;テキストの色を設定します。
- **テキスト**&ndash;を提示するテキスト。

次の c# コードでは、ここで、最初の単語は太字、最後の単語が赤のラベルを示します。

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

XAML:

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

![](label-images/formattedtext.png "FormattedText 例ラベルを付ける")


## <a name="related-links"></a>関連リンク

- [第 3 章、Xamarin.Forms を使用したモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [ラベルの API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
