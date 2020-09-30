---
title: Android での VisualElement の昇格
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、API 21 以上を対象とするアプリケーションでの VisualElements の昇格を制御する、Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7eb21f9a6a679c294a848d7fa79721c6db03bb0d
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563225"
---
# <a name="visualelement-elevation-on-android"></a>Android での VisualElement の昇格

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、API 21 以上を対象とするアプリケーションのビジュアル要素の昇格 (Z オーダー) を制御するために使用されます。 ビジュアル要素の昇格によって、描画順序が決定されます。これにより、Z 値が大きい occluding のビジュアル要素は、Z 値が小さくなります。 添付プロパティを値に設定することにより、XAML で使用 `VisualElement.Elevation` され `boolean` ます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

メソッドは、 `Button.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 `VisualElement.SetElevation`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ビジュアル要素の昇格を null 許容に設定するために使用され `float` ます。 また、メソッドを使用して、 `VisualElement.GetElevation` ビジュアル要素の昇格の値を取得することもできます。

結果として、より大きな Z 値を持つビジュアル要素は、小さい Z 値を持つビジュアル要素を occlude するように、ビジュアル要素の昇格を制御できるようになります。 したがって、この例では、 [`Button`](xref:Xamarin.Forms.Button) [`BoxView`](xref:Xamarin.Forms.BoxView) より高い昇格値があるため、の上に2番目のがレンダリングされます。

![VisualElement の昇格のスクリーンショット](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)