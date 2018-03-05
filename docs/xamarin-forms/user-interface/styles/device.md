---
title: "デバイスのスタイル"
description: "Xamarin.Forms には、Device.Styles クラスでのデバイスのスタイルと呼ばれる、6 つの動的なスタイルが含まれています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: dbe88f39999ee163ff2e7cdf4b9301aa0f0681bc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="device-styles"></a>デバイスのスタイル

_Xamarin.Forms には、Device.Styles クラスでのデバイスのスタイルと呼ばれる、6 つの動的なスタイルが含まれています。_

*デバイス*スタイルは。

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

6 つのすべてのスタイルのみに適用することができます[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンス。 たとえば、`Label`段落の本文が表示されている設定がその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティを[ `BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)です。

次のコード例では、使用方法を示します、*デバイス*XAML ページのスタイル。

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
              Style="{DynamicResource SubtitleTextStyle}" />
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

使用するデバイスのスタイルがバインドされている、`DynamicResource`マークアップ拡張機能です。 スタイルの動的な性質見なすことができ、iOS で変更することによって、**アクセシビリティ**テキストのサイズの設定。 外観、*デバイス*スタイルは、次のスクリーン ショットに示すように、各プラットフォームで異なります。

![](device-images/device-styles.png "各プラットフォームでデバイスのスタイル")

*デバイス*を設定して、スタイルからの派生させることも、 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/)をスタイルとして、デバイスのキー名のプロパティです。 上記のコード例で`myBodyStyle`から継承[ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)し、アクセント記号付きテキストの色を設定します。 動的なスタイルの継承の詳細については、次を参照してください。[動的スタイルの継承](~/xamarin-forms/user-interface/styles/dynamic.md#dynamic-style-inheritance)です。

次のコード例では、C# の場合と同じページを示しています。

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

[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)の各プロパティ[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスからプロパティを適切に設定されて、 [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)クラスです。

## <a name="accessibility"></a>ユーザー補助

*デバイス*フォント サイズは各プラットフォームのユーザー補助の設定が変更されるように変更されますので、スタイルがユーザー補助の設定を尊重します。 そのため、アクセス可能なテキストをサポートすることを確認、*デバイス*スタイルは、アプリケーション内の任意のテキスト スタイルの基礎として使用されます。

次のスクリーン ショットは、最小のアクセス可能なフォント サイズを各プラットフォームでデバイスのスタイルを示しています。

[![](device-images/minimum-size.png "各プラットフォームのモバイル デバイスのアクセス可能な小さいスタイル")](device-images/minimum-size-large.png "各プラットフォームのモバイル デバイスのアクセス可能な小さいスタイル")

次のスクリーン ショットは、最大のアクセス可能なフォント サイズで、各プラットフォームでデバイスのスタイルを示しています。

![](device-images/maximum-size.png "各プラットフォームでアクセス可能な大型デバイス スタイル")

## <a name="summary"></a>まとめ

Xamarin.Forms では、6 つ含まれています*動的*スタイルと呼ばれる*デバイス*スタイルで、 [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)クラスです。 6 つのすべてのスタイルのみに適用することができます[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンス。


## <a name="related-links"></a>関連リンク

- [テキストのスタイル](~/xamarin-forms/user-interface/text/styles.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
