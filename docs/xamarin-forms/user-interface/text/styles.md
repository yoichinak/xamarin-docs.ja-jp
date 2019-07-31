---
title: Xamarin.Forms のテキストのスタイル
description: この記事で説明します Xamarin.Forms アプリケーションでテキストのスタイル設定する方法。 スタイルを 1 回定義されているし、多くのビューで使用されることができますが、スタイルは、1 つの型のビューでのみ使用できます。
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: b639a7fdefb8fca67d833b07ef9aa1a85da67ef6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642884"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.Forms のテキストのスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.Forms でテキストのスタイル設定_

ラベル、エントリ、およびエディターの外観を調整するスタイルを使用できます。 スタイルを 1 回定義されているし、多くのビューで使用されることができますが、スタイルは、1 つの型のビューでのみ使用できます。
スタイルを指定することができます、`Key`選択的に特定のコントロールを使用して適用し、`Style`プロパティ。

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>組み込みのスタイル

Xamarin.Forms では、いくつか含まれています[組み込み](xref:Xamarin.Forms.Device.Styles)一般的なシナリオのスタイル。

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

いずれかの組み込みのスタイルを適用するには、使用、`DynamicResource`スタイルを指定するマークアップ拡張機能。

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# での組み込みスタイルが選択されている`Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "デバイスのスタイルの例")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>カスタム スタイル

Set アクセス操作子で構成されているスタイル プロパティ set アクセス操作子で構成され、プロパティの値に設定されます。

C# でサイズ 30 の赤いテキスト ラベルのカスタム スタイルをよう定義は。

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

で XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

リソース (すべてのスタイルを含む) は内で定義されていることに注意してください。 `ContentPage.Resources`、これは、なじみの兄弟`ContentPage.Content`要素。

![](styles-images/customstyle.png "カスタム スタイルの例")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>スタイルの適用

スタイルが作成されたら、任意のビューのマッチングに適用できる、`TargetType`します。

XAML でカスタム スタイルが適用されますをビューに指定することによって、`Style`プロパティを`StaticResource`目的のスタイルを参照するマークアップ拡張機能。

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C# の場合は、スタイル、ビューに直接適用またはに追加し、取得できるからページの`ResourceDictionary`します。 直接追加します。

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

追加してから、ページの取得`ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

ユーザー補助の設定に応答する必要があるために、組み込みスタイルが異なる方法で適用されます。 XAML での組み込みのスタイルを適用する、`DynamicResource`マークアップ拡張機能を使用します。

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# での組み込みスタイルが選択されている`Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>ユーザー補助

ユーザー補助の設定を尊重しやすく組み込みのスタイルが存在します。 組み込みのスタイルを使用する場合、フォント サイズは、ユーザーがそれに応じて、ユーザー補助の設定を設定する場合を自動的に増やします。

ビューのユーザー補助の設定を有効になっており、無効になっている組み込みのスタイルのスタイル設定の同じページの次の例を検討してください。

無効になっています。

![](styles-images/pre-access.png "ユーザー補助機能を無効になっているデバイスのスタイル")

有効:

![](styles-images/post-access.png "アクセシビリティを有効になっているデバイスのスタイル")

ユーザー補助のために、基準として、アプリ内の任意のテキスト関連スタイルに組み込みのスタイルを使用して、スタイルが一貫して使用していることを確認します。 参照してください[スタイル](~/xamarin-forms/user-interface/styles/index.md)詳細については、拡張して、一般にスタイルを使用します。


## <a name="related-links"></a>関連リンク

- [第 12 章、Xamarin.Forms でモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [テキスト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [スタイル](xref:Xamarin.Forms.Style)
