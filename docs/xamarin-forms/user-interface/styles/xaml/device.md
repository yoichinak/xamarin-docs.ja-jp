---
title: Xamarin.Forms でのデバイスのスタイル
description: Xamarin.Forms には、Device.Styles クラスで、デバイスのスタイルと呼ばれる、6 つの動的なスタイルが含まれています。 この記事では、Xamarin.Forms アプリケーションでデバイスのスタイルを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 252f3271c7247f7070df66712035938be651e7f4
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924790"
---
# <a name="device-styles-in-xamarinforms"></a>Xamarin.Forms でのデバイスのスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)

_Xamarin.Forms には、Device.Styles クラスで、デバイスのスタイルと呼ばれる、6 つの動的なスタイルが含まれています。_

*デバイス*スタイルします。

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

6 つのすべてのスタイルにのみ適用[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 など、`Label`段落の本文が表示されている設定がその[ `Style` ](xref:Xamarin.Forms.NavigableElement.Style)プロパティを[ `BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)します。

次のコード例に示しますを使用して、*デバイス*XAML ページのスタイル。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
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

使用するデバイスのスタイルがバインドされている、`DynamicResource`マークアップ拡張機能。 スタイルの動的な性質は、iOS で変更することで確認できます、**アクセシビリティ**テキストのサイズを設定します。 外観、*デバイス*スタイルは、次のスクリーン ショットに示すように、各プラットフォームで異なります。

![](device-images/device-styles.png "各プラットフォームでデバイスのスタイル")

*デバイス*スタイルも設定から派生した、 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティをデバイスのスタイルのキーの名前。 上記のコード例で`myBodyStyle`継承[ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle)およびアクセント記号付きテキストの色を設定します。 動的なスタイルの継承の詳細については、[動的スタイル継承](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance)を参照してください。

次のコード例では、C# で最初のページを示しています。

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
        Icon = "csharp.png";
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

[ `Style` ](xref:Xamarin.Forms.NavigableElement.Style)の各プロパティ[ `Label` ](xref:Xamarin.Forms.Label)インスタンスから適切なプロパティに設定されて、 [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles)クラス。

## <a name="accessibility"></a>ユーザー補助

*デバイス*スタイルがアクセシビリティ設定が各プラットフォームで変更されるように、フォント サイズが変更されますので、ユーザー補助の設定を尊重します。 そのため、アクセス可能なテキストをサポートすることを確認、*デバイス*スタイルは、アプリケーション内の任意のテキスト スタイルの基礎として使用されます。

次のスクリーン ショットは、最小のアクセス可能なフォント サイズで、各プラットフォームでデバイスのスタイルを示しています。

[![](device-images/minimum-size.png "各プラットフォームでアクセス可能な小さいデバイス スタイル")](device-images/minimum-size-large.png#lightbox "各プラットフォームでアクセスできる小型のデバイスのスタイル")

次のスクリーン ショットは、アクセス可能なフォントの最大サイズで、各プラットフォームでデバイスのスタイルを示しています。

![](device-images/maximum-size.png "各プラットフォームでの大規模なデバイスをアクセス可能なスタイル")

## <a name="related-links"></a>関連リンク

- [テキストのスタイル](~/xamarin-forms/user-interface/text/styles.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
