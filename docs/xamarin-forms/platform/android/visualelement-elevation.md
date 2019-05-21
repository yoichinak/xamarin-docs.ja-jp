---
title: Android で VisualElement が昇格されます。
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォームに固有の VisualElements API 21 以降を対象とするアプリケーションでの昇格を制御するを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: c7cc6b560ea91dca89c468271b89d1dcedfcda53
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65927022"
---
# <a name="visualelement-elevation-on-android"></a>Android で VisualElement が昇格されます。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この Android プラットフォームに固有とが、昇格、またはアプリケーションのビジュアル要素の Z オーダーを対象とする API 21 を制御するために使用されるかです。 視覚要素の昇格は、高い Z の値を持つ視覚要素が低い Z の値をもつ視覚要素を塞ぐように、自身の描画順を決定します。 これは XAML で `VisualElement.Elevation` 添付プロパティに`boolean`値を設定して使用します。

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

代わりに、fluent API を使用して C# から使用できます。

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

`Button.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 `VisualElement.SetElevation`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間に存在し、視覚要素の昇格に null 許容型の `float` 値を設定するために使われます。 さらに、`VisualElement.GetElevation`メソッドは、視覚要素の昇格値を取得するために使用できます。

その結果、視覚要素の昇格は、高いZ値の視覚要素が低いZ値の視覚要素を塞ぐように制御されます。 それによって、この例では2番目の[`Button`](xref:Xamarin.Forms.Button)は、より高い昇格値を持つため、[`BoxView`](xref:Xamarin.Forms.BoxView)の上に表示されます。

![](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
