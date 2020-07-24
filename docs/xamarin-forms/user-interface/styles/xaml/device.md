---
title: のデバイスのスタイルXamarin.Forms
description: Xamarin.Formsには、デバイススタイルと呼ばれる6つの動的スタイルが、デバイススタイルクラスに含まれています。 この記事では、アプリケーションでデバイスのスタイルを使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 04669479bb321da4fee6c45fd0f2c00deb5bbf1a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929728"
---
# <a name="device-styles-in-xamarinforms"></a>のデバイスのスタイルXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin.Formsには、デバイススタイルと呼ばれる6つの動的スタイルが、デバイススタイルクラスに含まれています。_

*デバイス*のスタイルは次のとおりです。

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

6つのスタイルはすべてインスタンスにのみ適用でき [`Label`](xref:Xamarin.Forms.Label) ます。 たとえば、 `Label` 段落の本文を表示しているでは、プロパティがに設定されている可能性があり [`Style`](xref:Xamarin.Forms.NavigableElement.Style) [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) ます。

次のコード例は、XAML ページで*デバイス*のスタイルを使用する方法を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

デバイススタイルは、マークアップ拡張機能を使用してにバインドされ `DynamicResource` ます。 スタイルの動的な性質は、テキストサイズの**ユーザー補助**の設定を変更することで、iOS で見ることができます。 *デバイス*スタイルの外観は、次のスクリーンショットに示すように、プラットフォームによって異なります。

![各プラットフォームでのデバイスのスタイル](device-images/device-styles.png)

*デバイススタイル* [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) は、プロパティをデバイススタイルのキー名に設定することによって、から派生させることもできます。 上記のコード例では、はを `myBodyStyle` 継承 [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) し、アクセント付きのテキストの色を設定します。 動的スタイルの継承の詳細については、「[動的スタイルの継承](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance)」を参照してください。

次のコード例は、C# の対応するページを示しています。

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[`Style`](xref:Xamarin.Forms.NavigableElement.Style)各インスタンスのプロパティ [`Label`](xref:Xamarin.Forms.Label) は、クラスからの適切なプロパティに設定され [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) ます。

## <a name="accessibility"></a>ユーザー補助

*デバイス*のスタイルはユーザー補助の設定を優先します。そのため、各プラットフォームでアクセシビリティの設定が変更されると、フォントサイズが変更されます。 そのため、ユーザー補助テキストをサポートするには、アプリケーション内でテキストスタイルのベースとして*デバイス*のスタイルが使用されていることを確認してください。

次のスクリーンショットは、各プラットフォームのデバイススタイルを示しています。アクセス可能なフォントサイズは最も小さくなっています。

[![各プラットフォームでアクセス可能な小さいデバイススタイル](device-images/minimum-size.png)](device-images/minimum-size-large.png#lightbox "各プラットフォームでアクセス可能な小さいデバイススタイル")

次のスクリーンショットは、各プラットフォームのデバイススタイルを示しています。アクセス可能なフォントサイズは最大です。

![各プラットフォームのアクセス可能な大規模なデバイススタイル](device-images/maximum-size.png)

## <a name="related-links"></a>関連リンク

- [テキストスタイル](~/xamarin-forms/user-interface/text/styles.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [デバイス. スタイル](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
