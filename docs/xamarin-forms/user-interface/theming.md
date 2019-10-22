---
title: Xamarin. Forms アプリケーションのテーマ
description: テーマは、各テーマの ResourceDictionary を作成し、DynamicResource マークアップ拡張機能を使用してリソースを読み込むことによって、Xamarin アプリケーションで実装できます。
ms.prod: xamarin
ms.assetId: B7B17F66-4E37-4B50-9A57-351B62BE4FED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2019
ms.openlocfilehash: 3e0f508a9c980c02681f1be581846f9f2f25e2d0
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "69529279"
---
# <a name="theming-a-xamarinforms-application"></a>Xamarin. Forms アプリケーションのテーマ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Xamarin アプリケーションは、`DynamicResource` マークアップ拡張機能を使用して、実行時に動的にスタイル変更に応答できます。 このマークアップ拡張機能は、`StaticResource` マークアップ拡張機能に似ています。では、どちらもディクショナリキーを使用して[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)から値をフェッチします。 ただし、`StaticResource` マークアップ拡張機能は単一の辞書参照を実行しますが、`DynamicResource` マークアップ拡張機能はディクショナリキーへのリンクを保持します。 このため、キーに関連付けられている値が置換された場合、変更は[`VisualElement`](xref:Xamarin.Forms.VisualElement)に適用されます。 これにより、実行時のテーマを Xamarin. Forms アプリケーションで実装できるようになります。

Xamarin. フォームアプリケーションでランタイムテーマを実装するプロセスは次のとおりです。

1. [@No__t_1](xref:Xamarin.Forms.ResourceDictionary)内の各テーマのリソースを定義します。
1. @No__t_0 マークアップ拡張機能を使用して、アプリケーションでテーマリソースを使用します。
1. アプリケーションの**app.xaml**ファイルに既定のテーマを設定します。
1. 実行時にテーマを読み込むコードを追加します。

次のスクリーンショットは、テーマが適用されたページを示しています。 iOS アプリケーションでは、明るいテーマと Android アプリケーションを使用して、ダークテーマを使用しています。

テーマが適用さ[![れたアプリのメインページのスクリーンショット (ios および 
 android](theming-images/main-page-both-themes.png "テーマ付きアプリのメインページ")](theming-images/main-page-both-themes-large.png#lightbox "テーマ付きアプリのメインページ")の場合)、テーマが適用さ[![れたアプリの詳細ページのスクリーンショット (ios および android)](theming-images/detail-page-both-themes.png "テーマ付きアプリの詳細ページ")](theming-images/detail-page-both-themes-large.png#lightbox "テーマ付きアプリの詳細ページ")

## <a name="define-themes"></a>テーマを定義する

テーマは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に格納されているリソースオブジェクトのコレクションとして定義されます。

次の例は、サンプルアプリケーションの `LightTheme` を示しています。

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.LightTheme">
    <Color x:Key="PageBackgroundColor">White</Color>
    <Color x:Key="NavigationBarColor">WhiteSmoke</Color>
    <Color x:Key="PrimaryColor">WhiteSmoke</Color>
    <Color x:Key="SecondaryColor">Black</Color>
    <Color x:Key="PrimaryTextColor">Black</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">Gray</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

次の例は、サンプルアプリケーションの `DarkTheme` を示しています。

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.DarkTheme">
    <Color x:Key="PageBackgroundColor">Black</Color>
    <Color x:Key="NavigationBarColor">Teal</Color>
    <Color x:Key="PrimaryColor">Teal</Color>
    <Color x:Key="SecondaryColor">White</Color>
    <Color x:Key="PrimaryTextColor">White</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">WhiteSmoke</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

各[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)には、それぞれのテーマを定義する[`Color`](xref:Xamarin.Forms.Color)リソースが含まれており、各 `ResourceDictionary` は同一のキー値を使用します。 リソースディクショナリの詳細については、「[リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

> [!IMPORTANT]
> @No__t_1 メソッドを呼び出す各 `ResourceDictionary` には、分離コードファイルが必要です。 これは、選択したテーマを表す CLR オブジェクトを実行時に作成できるようにするために必要です。

## <a name="set-a-default-theme"></a>既定のテーマを設定する

アプリケーションには既定のテーマが必要であるため、コントロールには、使用するリソースの値を設定できます。 既定のテーマを設定するには、 **app.xaml**に定義されているアプリケーションレベルの `ResourceDictionary` にテーマの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)をマージします。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>
        <ResourceDictionary Source="Themes/LightTheme.xaml" />
    </Application.Resources>
</Application>
```

リソースディクショナリのマージの詳細については、「マージされた[リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md#merged-resource-dictionaries)」を参照してください。

## <a name="consume-theme-resources"></a>テーマリソースの使用

アプリケーションで、テーマを表す[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に格納されているリソースを使用する場合は、`DynamicResource` マークアップ拡張機能を使用して実行する必要があります。 これにより、実行時に別のテーマが選択された場合に、新しいテーマの値が適用されるようになります。

次の例では、 [`Label`](xref:Xamarin.Forms.Label)オブジェクトに適用できるサンプルアプリケーションの3つのスタイルを示します。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>

        <Style x:Key="LargeLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource SecondaryTextColor}" />
            <Setter Property="FontSize"
                    Value="30" />
        </Style>

        <Style x:Key="MediumLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource PrimaryTextColor}" />
            <Setter Property="FontSize"
                    Value="25" />
        </Style>

        <Style x:Key="SmallLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource TertiaryTextColor}" />
            <Setter Property="FontSize"
                    Value="15" />
        </Style>

    </Application.Resources>
</Application>
```

これらのスタイルは、アプリケーションレベルのリソースディクショナリで定義されているので、複数のページで使用できます。 各スタイルでは、`DynamicResource` マークアップ拡張機能を使用してテーマリソースが使用されます。

これらのスタイルは、次のページによって使用されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ThemingDemo"
             x:Class="ThemingDemo.UserSummaryPage"
             Title="User Summary"
             BackgroundColor="{DynamicResource PageBackgroundColor}">
    ...
    <ScrollView>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="200" />
                <RowDefinition Height="120" />
                <RowDefinition Height="70" />
            </Grid.RowDefinitions>
            <Grid BackgroundColor="{DynamicResource PrimaryColor}">
                <Label Text="Face-Palm Monkey"
                       VerticalOptions="Center"
                       Margin="15"
                       Style="{StaticResource MediumLabelStyle}" />
                ...
            </Grid>
            <StackLayout Grid.Row="1"
                         Margin="10">
                <Label Text="This monkey reacts appropriately to ridiculous assertions and actions."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Cynical but not unfriendly."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Seven varieties of grimaces."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Doesn't laugh at your jokes."
                       Style="{StaticResource SmallLabelStyle}" />
            </StackLayout>
            ...
        </Grid>
    </ScrollView>
</ContentPage>
```

テーマリソースを直接使用する場合は、`DynamicResource` マークアップ拡張機能で使用する必要があります。 ただし、`DynamicResource` マークアップ拡張機能を使用するスタイルが使用されている場合は、`StaticResource` マークアップ拡張機能で使用する必要があります。

スタイル設定の詳細については、「 [XAML スタイルを使用した Xamarin. Forms アプリのスタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)設定」を参照してください。 @No__t_0 マークアップ拡張機能の詳細については、「 [Xamarin. フォームの動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)」を参照してください。

## <a name="load-a-theme-at-runtime"></a>実行時にテーマを読み込む

実行時にテーマを選択すると、アプリケーションは次のことを行う必要があります。

1. 現在のテーマをアプリケーションから削除します。 これは、アプリケーションレベルの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)の[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)プロパティをクリアすることで実現されます。
2. 選択したテーマを読み込みます。 これを実現するには、選択したテーマのインスタンスをアプリケーションレベル `ResourceDictionary` の `MergedDictionaries` プロパティに追加します。

@No__t_2 マークアップ拡張機能を使用してプロパティを設定する[`VisualElement`](xref:Xamarin.Forms.VisualElement)オブジェクトでは、新しいテーマの値が適用されます。 これは、`DynamicResource` マークアップ拡張機能がディクショナリキーへのリンクを保持しているために発生します。 そのため、キーに関連付けられている値が置換されると、その変更が `VisualElement` オブジェクトに適用されます。

サンプルアプリケーションでは、 [`Picker`](xref:Xamarin.Forms.Picker)を含むモーダルページを使用してテーマを選択します。 次のコードは、選択したテーマが変更されたときに実行される `OnPickerSelectionChanged` メソッドを示しています。

```csharp
void OnPickerSelectionChanged(object sender, EventArgs e)
{
    Picker picker = sender as Picker;
    Theme theme = (Theme)picker.SelectedItem;

    ICollection<ResourceDictionary> mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    if (mergedDictionaries != null)
    {
        mergedDictionaries.Clear();

        switch (theme)
        {
            case Theme.Dark:
                mergedDictionaries.Add(new DarkTheme());
                break;
            case Theme.Light:
            default:
                mergedDictionaries.Add(new LightTheme());
                break;
        }
    }
}
```

## <a name="related-links"></a>関連リンク

- [テーマ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin 形式の動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
