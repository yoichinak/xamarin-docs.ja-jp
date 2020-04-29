---
title: Xamarin. Forms アプリケーションでのシステムテーマの変更に応答する
description: Xamarin アプリケーションは、OnAppTheme の種類と DynamicResource マークアップ拡張機能を使用して、オペレーティングシステムのテーマの変更に応答できます。
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: c524ac0809044e576a8d56561642f6c3bf2df4a4
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82533025"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>Xamarin. Forms アプリケーションでのシステムテーマの変更に応答する

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

通常、デバイスには明るいテーマとダークテーマが含まれており、それぞれがオペレーティングシステムレベルで設定できるさまざまな外観設定を参照しています。 アプリケーションはこれらのシステムテーマを尊重し、システムテーマが変更されたときに直ちに応答する必要があります。

システムテーマは、デバイスの構成によっては、さまざまな理由で変更される可能性があります。 これには、ユーザーによって明示的に変更されたシステムテーマが含まれ、時刻によって変化します。また、低光などの環境要因によって変化します。

Xamarin アプリケーションは、 `AppThemeColor`クラス、 `OnAppTheme<T>`クラス、および`OnAppTheme`マークアップ拡張機能を使用してリソースを定義することで、システムテーマの変更に応答できます。 これらのリソースは、 `DynamicResource`マークアップ拡張機能で使用する必要があります。

> [!IMPORTANT]
> システムテーマの変更への対応は、現在試験段階であり、 `AppTheme_Experimental`フラグを設定することによってのみ使用できます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

システムテーマの変更に応答するには、次の要件が満たされている必要があります。

- Xamarin. Forms 4.6 以上。
- iOS 13 以上。
- Android 10 (API 29) 以上。
- UWP ビルド14393以上。

次のスクリーンショットは、iOS および Android の明るいシステムテーマとダークシステムテーマのテーマが適用されたページを示しています。

[![Screenshot of the main page of a themed app, on iOS and Android](system-theme-changes-images/main-page-both-themes.png "テーマ付きアプリのメインページ")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "テーマ付きアプリのメインページ")
テーマが適用されたアプリのメインページのスクリーンショット、[![テーマ付きアプリの詳細ページ (ios および android)](system-theme-changes-images/detail-page-both-themes.png "テーマ付きアプリの詳細ページ")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "テーマ付きアプリの詳細ページ")

## <a name="define-and-consume-theme-resources"></a>テーマリソースの定義と使用

明るいテーマとダークテーマのリソースは、 `AppThemeColor`クラス、 `OnAppTheme<T>`クラス、および`OnAppTheme`マークアップ拡張機能を使用して定義できます。 各方法では、現在のシステムテーマの値に基づいて、これらのリソースが自動的に適用されます。 また、これらのリソースを使用するオブジェクトは、アプリの実行中にシステムテーマが変更されると、自動的に更新されます。

### <a name="appthemecolor"></a>AppThemeColor

`AppThemeColor`クラスは、明るいシステムテーマ[`Color`](xref:Xamarin.Forms.Color)とダークシステムテーマのリソースを定義するために使用されます。 `AppThemeColor`リソースは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)次のように定義する必要があります。

```xaml
<Application ...>
    <Application.Resources>
        <AppThemeColor x:Key="PageBackgroundColor"
                       Light="White"
                       Dark="Black" />
        <AppThemeColor x:Key="NavigationBarColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="PrimaryColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="SecondaryColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="PrimaryTextColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="SecondaryTextColor"
                       Light="White"
                       Dark="White" />
        <AppThemeColor x:Key="TertiaryTextColor"
                       Light="Gray"
                       Dark="WhiteSmoke" />
        <AppThemeColor x:Key="TransparentColor"
                       Light="Transparent"
                       Dark="Transparent" />
    </Application.Resources>
</Application>
```

各`AppThemeColor`リソースには`x:Key`属性が必要です。これにより、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に説明的なキーが与えられます。 プロパティ`Light`と`Dark`プロパティの値はオブジェクトで[`Color`](xref:Xamarin.Forms.Color)ある必要があります。 また、プロパティを`Default`に設定して、 `Color`使用するオブジェクトが既定で使用するように設定することもできます。

`AppThemeColor`リソースはインラインで使用できます。

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

また、 `AppThemeColor`暗黙的または明示的[`Style`](xref:Xamarin.Forms.Style)なオブジェクトによってリソースを使用することもできます。

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor`リソースは、 `DynamicResource`マークアップ拡張機能で使用する必要があります。 これにより、システムテーマが変更されたときに、使用中のオブジェクトの外観が更新されます。

### <a name="onappthemelttgt"></a>OnAppTheme&lt;T&gt;

`OnAppTheme<T>`クラスは、ライトおよびダークシステムテーマに対して任意の型のリソースを定義するために使用されます。 `OnAppTheme<T>`リソースは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `x:TypeArguments`属性の値として`T`引数を指定して、で定義する必要があります。

```xaml
<Application ...>
    <Application.Resources>
        <OnAppTheme x:Key="ImageLogo"
                    x:TypeArguments="FileImageSource"
                    Light="lightlogo.png"
                    Dark="darklogo.png" />
    </Application.Resources>
</Application>
```

各`OnAppTheme<T>`リソースには`x:Key`属性が必要です。これにより、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に説明的なキーが与えられます。 プロパティ`Light`と`Dark`プロパティの値は、 `x:TypeArguments`属性として定義されている型のオブジェクトである必要があります。 また、プロパティは`Default` 、既定で使用するオブジェクトによって`T`使用される型のオブジェクトに設定できます。

`OnAppTheme<T>`リソースはインラインで使用できます。

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

また、 `OnAppTheme<T>`暗黙的または明示的[`Style`](xref:Xamarin.Forms.Style)なオブジェクトによってリソースを使用することもできます。

```xaml
<Style x:Key="imageLogoStyle"
       TargetType="Image">
    <Setter Property="Source"
            Value="{DynamicResource ImageLogo}" />
    <Setter Property="Aspect"
            Value="AspectFit" />
</Style>
```

> [!IMPORTANT]
> `OnAppTheme<T>`リソースは、 `DynamicResource`マークアップ拡張機能で使用する必要があります。 これにより、システムテーマが変更されたときに、使用中のオブジェクトの外観が更新されます。

### <a name="onapptheme-markup-extension"></a>OnAppTheme マークアップ拡張機能

`OnAppTheme`マークアップ拡張機能を使用すると、現在のシステムテーマに基づいて、イメージや色など、消費するリソースを指定できます。 `OnAppTheme<T>`クラスと同じ機能を提供しますが、より簡潔に表現できます。

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

この例では、デバイスが明るいテーマを[`Label`](xref:Xamarin.Forms.Label)使用している場合、最初のテキストの色が緑色に設定されています。また、デバイスがダークテーマを使用している場合は赤に設定されます。 同様に、 [`Image`](xref:Xamarin.Forms.Image)は、現在のシステムテーマに基づいて異なるイメージファイルを表示します。

`OnAppTheme`マークアップ拡張機能の詳細については、「 [onapptheme マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)」を参照してください。

## <a name="detect-the-current-system-theme"></a>現在のシステムテーマの検出

現在のシステムテーマを検出するには、 `Application.RequestedTheme`プロパティの値を取得します。

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

プロパティ`RequestedTheme`は、 `OSAppTheme`列挙体のメンバーを返します。 `OSAppTheme` 列挙体を使って、次のメンバーを定義できます。

- `Unspecified`。デバイスが指定されていないテーマを使用していることを示します。
- `Light`。デバイスがライトテーマを使用していることを示します。
- `Dark`。デバイスがダークテーマを使用していることを示します。

## <a name="react-to-theme-changes"></a>テーマの変更に反応する

デバイスのシステムテーマは、デバイスの構成方法に応じて、さまざまな理由で変更される可能性があります。 `Application.RequestedThemeChanged`イベントを処理することによってシステムテーマが変更されたときに、Xamarin のフォームアプリに通知することができます。

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

イベント`AppThemeChangedEventArgs`に`RequestedThemeChanged`付随するオブジェクトには、型`RequestedTheme` `OSAppTheme`のという名前のプロパティが1つあります。 このプロパティは、要求されたシステムテーマを検出するために調べることができます。

## <a name="related-links"></a>関連リンク

- [SystemThemes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [OnAppTheme マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin 形式の動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
