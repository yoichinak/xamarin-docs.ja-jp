---
title: スタイル
description: Xamarin.Forms でテキストのスタイル設定
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f38e4bc9ecd66c1dc33e53fa5c9046ff363802e6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="styles"></a>スタイル

_Xamarin.Forms でテキストのスタイル設定_


スタイルは、ラベル、エントリ、およびエディターの外観を調整に使用できます。 スタイルを 1 回定義されているし、多くのビューで使用されることができますが、スタイルは、1 つの型のビューでのみ使用できます。
スタイルを指定することができます、`Key`選択的に特定のコントロールを使用して適用し、`Style`プロパティです。

ここでは、次のトピックについて説明します。

- **[組み込みスタイル](#Built-In_Styles)** &ndash;をスタイル アプリ全体のビューのテキスト ベースの組み込みスタイルを使用します。
- **[カスタム スタイル](#Custom_Styles)** &ndash;組み込みオプションで満足できない場合に、カスタム スタイルを定義します。
- **[スタイルを適用する](#Applying_Styles)** &ndash;ビューにカスタムと組み込みの両方のスタイルを適用します。
- **[ユーザー補助](#Accessibility)** &ndash;テキストがユーザー補助の設定を尊重ことを確認してください。

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>組み込みのスタイル

Xamarin.Forms では、いくつか含まれています[組み込み](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)一般的なシナリオのスタイル。

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

いずれかの組み込みスタイルを適用するには、使用、`DynamicResource`のスタイルを指定するマークアップ拡張機能。

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# の場合は、組み込みのスタイルが選択されている`Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "デバイスのスタイルの例")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>独自のスタイル

スタイルは、set アクセス操作子で構成されているし、プロパティ set アクセス操作子で構成されるプロパティの値に設定されます。

C# の場合は、サイズ 30 の赤いテキスト ラベルのスタイルをカスタムはように定義します。

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

XAML:

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

リソース (すべてのスタイルを含む) は内で定義されていることに注意してください`ContentPage.Resources`、これは、他の一般的なの兄弟`ContentPage.Content`要素。

![](styles-images/customstyle.png "カスタム スタイルの例")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>スタイルを適用します。

一致する任意のビューに適用できるスタイルを作成すると、その`TargetType`です。

XAML をカスタム スタイルが適用されるをビューに指定することによって、`Style`を持つプロパティ、`StaticResource`目的のスタイルを参照するマークアップ拡張機能。

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C# の場合は、スタイルか、ビューに直接適用またはに追加してからページの取得`ResourceDictionary`です。 直接追加します。

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

ページを追加および取得する`ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

ユーザー補助の設定に応答する必要があるために、組み込みスタイルが異なる方法で適用されます。 XAML では、組み込みのスタイルを適用する、`DynamicResource`マークアップ拡張機能を使用します。

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# の場合は、組み込みのスタイルが選択されている`Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>ユーザー補助

ユーザー補助の設定を尊重しやすく組み込みのスタイルが存在します。 組み込みのスタイルを使用する場合、フォント サイズは、ユーザーがそれに応じて、ユーザー補助機能の優先順位を設定する場合を自動的に増加します。

ユーザー補助の設定を有効になっており、無効になっているとスタイルの組み込みスタイルを使用してビューの同じページの次の例について考えます。

無効になっています。

![](styles-images/pre-access.png "ユーザー補助機能が無効なデバイスのスタイル")

有効:

![](styles-images/post-access.png "有効になっているアクセシビリティを持つデバイス スタイル")

ユーザー補助機能には、組み込みスタイルが使用されること、基準として、アプリ内での任意のテキストに関連するスタイルをスタイルが一貫して使用していることを確認します。 参照してください[スタイル](~/xamarin-forms/user-interface/styles/index.md)を拡張して、一般にスタイルを使用して作業の詳細についてはします。


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms、章 12 を使用したモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
