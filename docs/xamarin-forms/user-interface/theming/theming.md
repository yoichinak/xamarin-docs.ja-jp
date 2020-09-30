---
title: アプリケーションのテーマを Xamarin.Forms 適用する
description: テーマをアプリケーションに実装するには Xamarin.Forms 、各テーマの ResourceDictionary を作成し、DynamicResource マークアップ拡張機能を使用してリソースを読み込む必要があります。
ms.prod: xamarin
ms.assetId: B7B17F66-4E37-4B50-9A57-351B62BE4FED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7eef9cdf353c527be4e1de4721a5658a7763dabe
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557544"
---
# <a name="theme-a-no-locxamarinforms-application"></a>アプリケーションのテーマを Xamarin.Forms 適用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Xamarin.Forms アプリケーションは、マークアップ拡張機能を使用して、実行時に動的にスタイル変更に応答でき `DynamicResource` ます。 このマークアップ拡張機能は、 `StaticResource` マークアップ拡張機能に似ています。では、ディクショナリキーを使用してから値をフェッチし [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 ただし、 `StaticResource` マークアップ拡張機能は、単一の辞書参照を実行しますが、 `DynamicResource` マークアップ拡張機能はディクショナリキーへのリンクを保持します。 このため、キーに関連付けられている値が置換された場合、変更はに適用され [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。 これにより、アプリケーションでランタイムテーマを実装できるようになり Xamarin.Forms ます。

アプリケーションにランタイムテーマを実装するプロセス Xamarin.Forms は次のとおりです。

1. 内の各テーマのリソースを定義 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) します。
1. マークアップ拡張機能を使用して、アプリケーションでテーマリソースを使用し `DynamicResource` ます。
1. アプリケーションの **app.xaml** ファイルに既定のテーマを設定します。
1. 実行時にテーマを読み込むコードを追加します。

> [!IMPORTANT]
> `StaticResource`実行時にアプリのテーマを変更する必要がない場合は、マークアップ拡張機能を使用します。

次のスクリーンショットは、テーマが適用されたページを示しています。 iOS アプリケーションでは、明るいテーマと Android アプリケーションを使用して、ダークテーマを使用しています。

[![IOS および Android でのテーマ付きアプリのメインページのスクリーンショット](theming-images/main-page-both-themes.png "テーマ付きアプリのメインページ")](theming-images/main-page-both-themes-large.png#lightbox "テーマ付きアプリのメインページ") 
テーマが適用さ[![れたアプリの詳細ページのスクリーンショット (IOS と Android)](theming-images/detail-page-both-themes.png "テーマ付きアプリの詳細ページ")](theming-images/detail-page-both-themes-large.png#lightbox "テーマ付きアプリの詳細ページ")

> [!NOTE]
> 実行時にテーマを変更するには、XAML スタイルを使用する必要があります。現在、CSS を使用することはできません。

## <a name="define-themes"></a>テーマを定義する

テーマは、に格納されているリソースオブジェクトのコレクションとして定義され [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。

次の例は、サンプルアプリケーションのを示してい `LightTheme` ます。

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

次の例は、サンプルアプリケーションのを示してい `DarkTheme` ます。

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

各 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) には、それぞれの [`Color`](xref:Xamarin.Forms.Color) テーマを定義するリソースが含まれており、それぞれが `ResourceDictionary` 同一のキー値を使用します。 リソースディクショナリの詳細については、「 [リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

> [!IMPORTANT]
> メソッドを呼び出すそれぞれに、分離コードファイルが必要です `ResourceDictionary` `InitializeComponent` 。 これは、選択したテーマを表す CLR オブジェクトを実行時に作成できるようにするために必要です。

## <a name="set-a-default-theme"></a>既定のテーマを設定する

アプリケーションには既定のテーマが必要であるため、コントロールには、使用するリソースの値を設定できます。 既定のテーマを設定するには、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `ResourceDictionary` **app.xaml**に定義されているアプリケーションレベルにテーマをマージします。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>
        <ResourceDictionary Source="Themes/LightTheme.xaml" />
    </Application.Resources>
</Application>
```

リソースディクショナリのマージの詳細については、「マージされた [リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md#merged-resource-dictionaries)」を参照してください。

## <a name="consume-theme-resources"></a>テーマリソースの使用

テーマを表すに格納されているリソースをアプリケーションが使用する場合は、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) マークアップ拡張機能を使用して実行する必要があり `DynamicResource` ます。 これにより、実行時に別のテーマが選択された場合に、新しいテーマの値が適用されるようになります。

次の例は、オブジェクトに適用できるサンプルアプリケーションの3つのスタイルを示してい [`Label`](xref:Xamarin.Forms.Label) ます。

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

これらのスタイルは、アプリケーションレベルのリソースディクショナリで定義されているので、複数のページで使用できます。 各スタイルは、マークアップ拡張機能を使用してテーマリソースを使用 `DynamicResource` します。

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

テーマリソースを直接使用する場合は、マークアップ拡張機能で使用する必要があり `DynamicResource` ます。 ただし、 `DynamicResource` マークアップ拡張機能を使用するスタイルが使用されている場合は、 `StaticResource` マークアップ拡張機能で使用する必要があります。

スタイル設定の詳細については、「 [ Xamarin.Forms XAML スタイルを使用したアプリのスタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)設定」を参照してください。 マークアップ拡張機能の詳細については `DynamicResource` 、「 [」 Xamarin.Forms の「動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)」を参照してください。

## <a name="load-a-theme-at-runtime"></a>実行時にテーマを読み込む

実行時にテーマを選択すると、アプリケーションは次のことを行う必要があります。

1. 現在のテーマをアプリケーションから削除します。 これは [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 、アプリケーションレベルのプロパティをクリアすることで実現され [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。
2. 選択したテーマを読み込みます。 これを実現するには、選択したテーマのインスタンスを `MergedDictionaries` アプリケーションレベルのプロパティに追加し `ResourceDictionary` ます。

[`VisualElement`](xref:Xamarin.Forms.VisualElement)マークアップ拡張機能を使用してプロパティを設定するすべてのオブジェクトで、 `DynamicResource` 新しいテーマの値が適用されます。 これは、 `DynamicResource` マークアップ拡張機能がディクショナリキーへのリンクを保持しているために発生します。 そのため、キーに関連付けられている値が置換されると、その変更がオブジェクトに適用され `VisualElement` ます。

サンプルアプリケーションでは、を含むモーダルページを使用してテーマを選択し [`Picker`](xref:Xamarin.Forms.Picker) ます。 次のコードは、 `OnPickerSelectionChanged` 選択したテーマが変更されたときに実行されるメソッドを示しています。

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

- [テーマ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [システム テーマの変更に対応する](system-theme-changes.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的スタイル Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用して Xamarin.Forms アプリのスタイルを設定する](~/xamarin-forms/user-interface/styles/xaml/index.md)