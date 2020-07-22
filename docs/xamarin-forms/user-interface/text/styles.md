---
title: Xamarin.Formsテキストスタイル
description: この記事では、アプリケーションでテキストをスタイル設定する方法について説明し Xamarin.Forms ます。 スタイルは一度定義することも、多くのビューで使用することもできますが、1つの型のビューでのみ使用できます。
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7d82348231c4b4905f2f70b80f73c45f2f0bf66b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84572001"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.Formsテキストスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin 形式でのテキストのスタイル設定_

スタイルを使用して、ラベル、エントリ、およびエディターの外観を調整できます。 スタイルは一度定義することも、多くのビューで使用することもできますが、1つの型のビューでのみ使用できます。
スタイルを指定し、 `Key` 特定のコントロールのプロパティを使用して選択的に適用でき `Style` ます。

## <a name="built-in-styles"></a>組み込みスタイル

Xamarin.Formsには、一般的なシナリオのための組み込みスタイルがいくつか[組み込ま](xref:Xamarin.Forms.Device.Styles)れています。

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

組み込みのスタイルのいずれかを適用するには、 `DynamicResource` マークアップ拡張機能を使用してスタイルを指定します。

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# では、組み込みのスタイルは次のものから選択され `Device.Styles` ます。

```csharp
label.Style = Device.Styles.TitleStyle;
```

![デバイススタイルの例](styles-images/builtinstyles.png)

## <a name="custom-styles"></a>カスタムスタイル

スタイルは setter と setter で構成され、プロパティとプロパティが設定される値で構成されます。

C# では、サイズが30の赤いテキストを持つラベルのカスタムスタイルは、次のように定義されます。

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

XAML の場合:

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

リソース (すべてのスタイルを含む) が内で定義されていることに注意して `ContentPage.Resources` ください。これは、より一般的な要素の兄弟です `ContentPage.Content` 。

![カスタムスタイルの例](styles-images/customstyle.png)

## <a name="applying-styles"></a>スタイルの適用

作成されたスタイルは、そのスタイルに一致する任意のビューに適用でき `TargetType` ます。

XAML では、 `Style` `StaticResource` 目的のスタイルを参照するマークアップ拡張機能を使用してプロパティを指定することで、カスタムスタイルがビューに適用されます。

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C# では、スタイルをビューに直接適用するか、またはページのに追加して取得することができ `ResourceDictionary` ます。 直接追加するには:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

ページのを追加および取得するには、次のようにし `ResourceDictionary` ます。

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

組み込みスタイルは、ユーザー補助の設定に応答する必要があるため、異なる方法で適用されます。 XAML で組み込みスタイルを適用するには、 `DynamicResource` マークアップ拡張機能を使用します。

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# では、組み込みのスタイルは次のものから選択され `Device.Styles` ます。

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>アクセシビリティ

組み込みスタイルは、ユーザー補助の設定を容易にするために用意されています。 組み込みのスタイルのいずれかを使用する場合、ユーザーがそれに応じてユーザー補助の設定を設定すると、フォントサイズが自動的に増加します。

ユーザー補助の設定が有効になっていて無効になっている組み込みスタイルを使用してスタイル設定されたビューの同じページの例を次に示します。

無効:

![ユーザー補助機能が無効になっているデバイスのスタイル](styles-images/pre-access.png)

有効:

![ユーザー補助機能が有効になっているデバイスのスタイル](styles-images/post-access.png)

アクセシビリティを確保するには、組み込みスタイルがアプリ内でテキスト関連のスタイルの基礎として使用されていること、およびスタイルを一貫して使用していることを確認してください。 一般的なスタイルの拡張と操作の詳細については、「[スタイル](~/xamarin-forms/user-interface/styles/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms第12章を使用した Mobile Apps の作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Style](xref:Xamarin.Forms.Style)
