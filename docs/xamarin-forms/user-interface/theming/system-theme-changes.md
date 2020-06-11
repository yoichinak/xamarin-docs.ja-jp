---
title: "アプリケーションのシステムテーマ変更に応答する Xamarin.Forms " 説明: " Xamarin.Forms アプリケーションは、onapptheme の種類と dynamicresource マークアップ拡張機能を使用して、オペレーティングシステムのテーマの変更に応答できます。"
ms. assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D: xamarin ms テクノロジ: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 04/22/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>アプリケーションのシステムテーマの変更に応答する Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

通常、デバイスには明るいテーマとダークテーマが含まれており、それぞれがオペレーティングシステムレベルで設定できるさまざまな外観設定を参照しています。 アプリケーションはこれらのシステムテーマを尊重し、システムテーマが変更されたときに直ちに応答する必要があります。

システムテーマは、デバイスの構成によっては、さまざまな理由で変更される可能性があります。 これには、ユーザーによって明示的に変更されたシステムテーマが含まれ、時刻によって変化します。また、低光などの環境要因によって変化します。

Xamarin.Formsアプリケーションは、 `AppThemeColor` クラス、 `OnAppTheme<T>` クラス、およびマークアップ拡張機能を使用してリソースを定義することで、システムテーマの変更に応答でき `OnAppTheme` ます。 これらのリソースは、マークアップ拡張機能で使用する必要があり `DynamicResource` ます。

> [!IMPORTANT]
> システムテーマの変更への対応は、現在試験段階であり、フラグを設定することによってのみ使用でき `AppTheme_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

Xamarin.Formsシステムテーマの変更に応答するには、次の要件を満たす必要があります。

- Xamarin.Forms4.6 以上。
- iOS 13 以上。
- Android 10 (API 29) 以上。
- UWP ビルド14393以上。

次のスクリーンショットは、iOS および Android の明るいシステムテーマとダークシステムテーマのテーマが適用されたページを示しています。

[![IOS および Android でのテーマ付きアプリのメインページのスクリーンショット](system-theme-changes-images/main-page-both-themes.png "テーマ付きアプリのメインページ")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "テーマ付きアプリのメインページ") 
テーマが適用さ[![れたアプリの詳細ページのスクリーンショット (IOS と Android)](system-theme-changes-images/detail-page-both-themes.png "テーマ付きアプリの詳細ページ")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "テーマ付きアプリの詳細ページ")

## <a name="define-and-consume-theme-resources"></a>テーマリソースの定義と使用

明るいテーマとダークテーマのリソースは、クラス、 `AppThemeColor` `OnAppTheme<T>` クラス、およびマークアップ拡張機能を使用して定義でき `OnAppTheme` ます。 各方法では、現在のシステムテーマの値に基づいて、これらのリソースが自動的に適用されます。 また、これらのリソースを使用するオブジェクトは、アプリの実行中にシステムテーマが変更されると、自動的に更新されます。

### <a name="appthemecolor"></a>AppThemeColor

クラスは、 `AppThemeColor` [`Color`](xref:Xamarin.Forms.Color) 明るいシステムテーマとダークシステムテーマのリソースを定義するために使用されます。 `AppThemeColor`リソースは、次のように定義する必要があり [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。

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

各 `AppThemeColor` リソースには属性が必要 `x:Key` です。これにより、に説明的なキーが与えら [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) れます。 `Light`プロパティとプロパティの値は `Dark` オブジェクトである必要があり [`Color`](xref:Xamarin.Forms.Color) ます。 また、プロパティをに設定して `Default` 、 `Color` 使用するオブジェクトが既定で使用するように設定することもできます。

`AppThemeColor`リソースはインラインで使用できます。

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

また、 `AppThemeColor` 暗黙的または明示的なオブジェクトによってリソースを使用することもでき [`Style`](xref:Xamarin.Forms.Style) ます。

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor`リソースは、マークアップ拡張機能で使用する必要があり `DynamicResource` ます。 これにより、システムテーマが変更されたときに、使用中のオブジェクトの外観が更新されます。

### <a name="onappthemelttgt"></a>OnAppTheme &lt; T&gt;

クラスは、 `OnAppTheme<T>` ライトおよびダークシステムテーマに対して任意の型のリソースを定義するために使用されます。 `OnAppTheme<T>`リソースは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `T` 属性の値として引数を指定して、で定義する必要があり `x:TypeArguments` ます。

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

各 `OnAppTheme<T>` リソースには属性が必要 `x:Key` です。これにより、に説明的なキーが与えら [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) れます。 プロパティとプロパティの値は、 `Light` `Dark` 属性として定義されている型のオブジェクトである必要があり `x:TypeArguments` ます。 また、プロパティは、 `Default` `T` 既定で使用するオブジェクトによって使用される型のオブジェクトに設定できます。

`OnAppTheme<T>`リソースはインラインで使用できます。

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

また、 `OnAppTheme<T>` 暗黙的または明示的なオブジェクトによってリソースを使用することもでき [`Style`](xref:Xamarin.Forms.Style) ます。

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
> `OnAppTheme<T>`リソースは、マークアップ拡張機能で使用する必要があり `DynamicResource` ます。 これにより、システムテーマが変更されたときに、使用中のオブジェクトの外観が更新されます。

### <a name="onapptheme-markup-extension"></a>OnAppTheme のマークアップ拡張機能

`OnAppTheme`マークアップ拡張機能を使用すると、現在のシステムテーマに基づいて、イメージや色など、消費するリソースを指定できます。 クラスと同じ機能を提供し `OnAppTheme<T>` ますが、より簡潔に表現できます。

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

この例では、デバイスが明るいテーマを使用している場合、最初のテキストの色 [`Label`](xref:Xamarin.Forms.Label) が緑色に設定されています。また、デバイスがダークテーマを使用している場合は赤に設定されます。 同様に、は、 [`Image`](xref:Xamarin.Forms.Image) 現在のシステムテーマに基づいて異なるイメージファイルを表示します。

マークアップ拡張機能の詳細については `OnAppTheme` 、「 [Onapptheme マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)」を参照してください。

## <a name="detect-the-current-system-theme"></a>現在のシステムテーマの検出

現在のシステムテーマを検出するには、プロパティの値を取得し `Application.RequestedTheme` ます。

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

プロパティは、 `RequestedTheme` `OSAppTheme` 列挙体のメンバーを返します。 `OSAppTheme` 列挙体を使って、次のメンバーを定義できます。

- `Unspecified`。デバイスが指定されていないテーマを使用していることを示します。
- `Light`。デバイスがライトテーマを使用していることを示します。
- `Dark`。デバイスがダークテーマを使用していることを示します。

## <a name="react-to-theme-changes"></a>テーマの変更に反応する

デバイスのシステムテーマは、デバイスの構成方法に応じて、さまざまな理由で変更される可能性があります。 Xamarin.Formsアプリは、イベントを処理することによってシステムテーマが変更されたときに通知を受けることができ `Application.RequestedThemeChanged` ます。

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

`AppThemeChangedEventArgs`イベントに付随するオブジェクトに `RequestedThemeChanged` は、型のという名前のプロパティが1つあり `RequestedTheme` `OSAppTheme` ます。 このプロパティは、要求されたシステムテーマを検出するために調べることができます。

## <a name="related-links"></a>関連リンク

- [SystemThemes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [OnAppTheme のマークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的スタイルXamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用して Xamarin.Forms アプリのスタイルを設定する](~/xamarin-forms/user-interface/styles/xaml/index.md)
