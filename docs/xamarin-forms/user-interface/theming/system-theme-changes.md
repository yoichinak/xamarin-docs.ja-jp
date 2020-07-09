---
title: アプリケーションのシステムテーマの変更に応答する Xamarin.Forms
description: Xamarin.Formsアプリケーションは、OnAppTheme の種類と DynamicResource マークアップ拡張機能を使用して、オペレーティングシステムのテーマの変更に応答できます。
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b451fe004ca21c8770658f31c9c38253e073c259
ms.sourcegitcommit: 82eabb0eaa4a674897aa6d5e64efb91fd580c330
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100184"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>アプリケーションのシステムテーマの変更に応答する Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

通常、デバイスには明るいテーマとダークテーマが含まれており、それぞれがオペレーティングシステムレベルで設定できるさまざまな外観設定を参照しています。 アプリケーションはこれらのシステムテーマを尊重し、システムテーマが変更されたときに直ちに応答する必要があります。

システムテーマは、デバイスの構成によっては、さまざまな理由で変更される可能性があります。 これには、ユーザーによって明示的に変更されたシステムテーマが含まれ、時刻によって変化します。また、低光などの環境要因によって変化します。

Xamarin.Formsアプリケーションは、マークアップ拡張機能を使用してリソースを消費することによって、システムテーマの変更に応答でき `AppThemeBinding` `SetAppThemeColor` ます。また、およびの拡張メソッドも対象と `SetOnAppTheme<T>` なります。

> [!IMPORTANT]
> システムテーマの変更への対応は、現在試験段階であり、フラグを設定することによってのみ使用でき `AppTheme_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

Xamarin.Formsシステムテーマの変更に応答するには、次の要件を満たす必要があります。

- Xamarin.Forms4.6.0.967 以上。
- iOS 13 以上。
- Android 10 (API 29) 以上。
- UWP ビルド14393以上。

次のスクリーンショットは、iOS および Android の明るいシステムテーマとダークシステムテーマのテーマが適用されたページを示しています。

[![IOS および Android でのテーマ付きアプリのメインページのスクリーンショット](system-theme-changes-images/main-page-both-themes.png "テーマ付きアプリのメインページ")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "テーマ付きアプリのメインページ") 
テーマが適用さ[![れたアプリの詳細ページのスクリーンショット (IOS と Android)](system-theme-changes-images/detail-page-both-themes.png "テーマ付きアプリの詳細ページ")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "テーマ付きアプリの詳細ページ")

## <a name="define-and-consume-theme-resources"></a>テーマリソースの定義と使用

ライトおよびダークテーマのリソースは、 `AppThemeBinding` マークアップ拡張機能と、および拡張メソッドを使用して使用でき `SetAppThemeColor` `SetOnAppTheme<T>` ます。 これらの方法では、現在のシステムテーマの値に基づいてリソースが自動的に適用されます。 また、これらのリソースを使用するオブジェクトは、アプリの実行中にシステムテーマが変更されると、自動的に更新されます。

### <a name="appthemebinding-markup-extension"></a>AppThemeBinding マークアップ拡張

`AppThemeBinding`マークアップ拡張機能を使用すると、現在のシステムテーマに基づいて、イメージや色などのリソースを使用できます。

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{AppThemeBinding Light=Green, Dark=Red}" />
        <Image Source="{AppThemeBinding Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

この例では、デバイスが明るいテーマを使用している場合、最初のテキストの色 [`Label`](xref:Xamarin.Forms.Label) が緑色に設定されています。また、デバイスがダークテーマを使用している場合は赤に設定されます。 同様に、は、 [`Image`](xref:Xamarin.Forms.Image) 現在のシステムテーマに基づいて異なるイメージファイルを表示します。

また、で定義されているリソースは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `StaticResource` マークアップ拡張機能で使用できます。

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Light colors -->
        <Color x:Key="LightPrimaryColor">WhiteSmoke</Color>
        <Color x:Key="LightSecondaryColor">Black</Color>

        <!-- Dark colors -->
        <Color x:Key="DarkPrimaryColor">Teal</Color>
        <Color x:Key="DarkSecondaryColor">White</Color>

        <Style x:Key="ButtonStyle"
               TargetType="Button">
            <Setter Property="BackgroundColor"
                    Value="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}" />
            <Setter Property="TextColor"
                    Value="{AppThemeBinding Light={StaticResource LightSecondaryColor}, Dark={StaticResource DarkSecondaryColor}}" />
        </Style>

    </ContentPage.Resources>

    <Grid BackgroundColor="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}">
      <Button Text="MORE INFO"
              Style="{StaticResource ButtonStyle}" />
    </Grid>    
</ContentPage>    
```

この例では、の背景色 [`Grid`](xref:Xamarin.Forms.Grid) とスタイルは、 [`Button`](xref:Xamarin.Forms.Button) デバイスが明るいテーマとダークテーマのどちらを使用しているかに基づいて変化します。

マークアップ拡張機能の詳細については `AppThemeBinding` 、「 [AppThemeBinding markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension)」を参照してください。

### <a name="extension-methods"></a>拡張メソッド

Xamarin.Forms`SetAppThemeColor`オブジェクトが `SetOnAppTheme<T>` [`VisualElement`](xref:Xamarin.Forms.VisualElement) システムテーマの変更に応答できるようにするおよび拡張メソッドを含みます。

`SetAppThemeColor`メソッドを使用すると、現在の [`Color`](xref:Xamarin.Forms.Color) システムテーマに基づいてターゲットプロパティに設定されるオブジェクトを指定できます。

```csharp
Label label = new Label();
label.SetAppThemeColor(Label.TextColorProperty, Color.Green, Color.Red);
```

この例では、デバイスが明るいテーマを使用している場合、のテキストの色 [`Label`](xref:Xamarin.Forms.Label) は緑色に設定されており、デバイスがダークテーマを使用している場合は赤に設定されます。

`SetOnAppTheme<T>`メソッドを使用すると、 `T` 現在のシステムテーマに基づいてターゲットプロパティに設定される型のオブジェクトを指定できます。

```csharp
Image image = new Image();
image.SetOnAppTheme<FileImageSource>(Image.SourceProperty, "lightlogo.png", "darklogo.png");
```

この例では、 [`Image`](xref:Xamarin.Forms.Image) デバイスがライトテーマを使用している `lightlogo.png` ときと、 `darklogo.png` デバイスがダークテーマを使用しているときにが表示されます。

## <a name="detect-the-current-system-theme"></a>現在のシステムテーマの検出

現在のシステムテーマを検出するには、プロパティの値を取得し `Application.RequestedTheme` ます。

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

プロパティは、 `RequestedTheme` `OSAppTheme` 列挙体のメンバーを返します。 `OSAppTheme` 列挙体を使って、次のメンバーを定義できます。

- `Unspecified`。デバイスが指定されていないテーマを使用していることを示します。
- `Light`。デバイスがライトテーマを使用していることを示します。
- `Dark`。デバイスがダークテーマを使用していることを示します。

## <a name="set-the-current-user-theme"></a>現在のユーザーのテーマを設定する

アプリケーションで使用されるテーマは、 `Application.UserAppTheme` `OSAppTheme` 現在どのシステムテーマが動作しているかに関係なく、型のプロパティを使用して設定できます。

```csharp
Application.Current.UserAppTheme = OSAppTheme.Dark;
```

この例では、アプリケーションは、現在どのシステムテーマが動作しているかに関係なく、システムのダークモードで定義されたテーマを使用するように設定されています。

> [!NOTE]
> プロパティをに設定すると、 `UserAppTheme` `OSAppTheme.Unspecified` オペレーティングシステムのテーマが既定値に設定されます。

## <a name="react-to-theme-changes"></a>テーマの変更に反応する

デバイスのシステムテーマは、デバイスの構成方法に応じて、さまざまな理由で変更される可能性があります。 Xamarin.Formsアプリは、イベントを処理することによってシステムテーマが変更されたときに通知を受けることができ `Application.RequestedThemeChanged` ます。

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

`AppThemeChangedEventArgs`イベントに付随するオブジェクトに `RequestedThemeChanged` は、型のという名前のプロパティが1つあり `RequestedTheme` `OSAppTheme` ます。 このプロパティは、要求されたシステムテーマを検出するために調べることができます。

> [!IMPORTANT]
> Android でのテーマの変更に応答するには、 `ConfigChanges.UiMode` クラスの属性にフラグを含める必要があり `Activity` `MainActivity` ます。

## <a name="related-links"></a>関連リンク

- [SystemThemes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [AppThemeBinding マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML スタイルを使用して Xamarin.Forms アプリのスタイルを設定する](~/xamarin-forms/user-interface/styles/xaml/index.md)
