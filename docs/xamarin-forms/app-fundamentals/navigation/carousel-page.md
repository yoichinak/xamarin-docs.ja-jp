---
title: Xamarin.Forms CarouselPage
description: Xamarin.Forms CarouselPage には、ギャラリーなどのコンテンツのページ間を移動するユーザーが左右にスワイプできるページです。 この記事では、ページのコレクションを移動する、CarouselPage を使用する方法を示します。
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: bce3a60f3647a537906cfa11fc1dcfcc6f5cf365
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998610"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms CarouselPage

_Xamarin.Forms CarouselPage には、ギャラリーなどのコンテンツのページ間を移動するユーザーが左右にスワイプできるページです。この記事では、ページのコレクションを移動する、CarouselPage を使用する方法を示します。_

## <a name="overview"></a>概要

次のスクリーン ショットに示す、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)各プラットフォームで。

![](carousel-page-images/thirdpage.png "CarouselPage 毎時項目")

レイアウトを[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)は各プラットフォームで同じです。 ページは、左から右のコレクションを後方に移動する方向のスワイプ操作と、コレクション内のページへの移動を右左から方向のスワイプ操作によって、移動ことができます。 次のスクリーン ショットでは、最初のページを表示する、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)インスタンス。

![](carousel-page-images/firstpage.png "CarouselPage 最初の項目")

次のスクリーン ショットに示すように 2 番目のページを右から左の移動にスワイプします。

![](carousel-page-images/secondpage.png "CarouselPage 2 番目の項目")

前のページに返す左から右方向のスワイプ操作中に、3 番目のページに移動もう一度右から左にスワイプします。

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>CarouselPage を作成します。

2 つの方法は、作成に使用できる、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage):

- [設定](#Populating_a_CarouselPage_with_a_Page_Collection)、`CarouselPage`子のコレクションを持つ[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス。
- [割り当てる](#Populating_a_CarouselPage_with_a_Template)コレクション、 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)プロパティと割り当てを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 返されるプロパティ[`ContentPage` ](xref:Xamarin.Forms.ContentPage)コレクション内のオブジェクトのインスタンス。

両方の方法で、`CarouselPage`でスワイプ操作が表示される次のページに移動し表示の各ページをさらは。

> [!NOTE]
> A [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)でのみ設定できます[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス、または`ContentPage`派生クラス。

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>ページのコレクションを使用して、CarouselPage の設定

次の XAML コード例は、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 3 つを表示する[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス。

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

次のコード例では、c# で同等の UI を示します。

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

各[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)だけが表示されます、 [ `Label` ](xref:Xamarin.Forms.Label)の特定の色と[ `BoxView` ](xref:Xamarin.Forms.BoxView)その色の。

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) UI の仮想化をサポートしていません。 そのため場合、パフォーマンスに影響する可能性があります、`CarouselPage`が多すぎるの子要素が含まれています。

場合、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)に埋め込まれ、 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)のページ、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)、 [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)プロパティに設定する必要があります`false`ジェスチャが競合を防ぐために、 `CarouselPage` 、`MasterDetailPage`します。

詳細については、 [ `CarouselPage`](xref:Xamarin.Forms.CarouselPage)を参照してください[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)Charles Petzold の Xamarin.Forms book の。

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>テンプレートを使用して、CarouselPage の設定

次の XAML コード例は、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)を割り当てることによって構築された、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティのページを返すコレクション内のオブジェクト:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)設定によってデータが読み込まれて、 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)分離コード ファイルのコンス トラクター内のプロパティ。

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

次のコード例は、相当するものを示しています。 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) c# で作成します。

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

各[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)だけが表示されます、 [ `Label` ](xref:Xamarin.Forms.Label)の特定の色と[ `BoxView` ](xref:Xamarin.Forms.BoxView)その色の。

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) UI の仮想化をサポートしていません。 そのため場合、パフォーマンスに影響する可能性があります、`CarouselPage`が多すぎるの子要素が含まれています。

場合、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)に埋め込まれ、 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)のページ、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)、 [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)プロパティに設定する必要があります`false`ジェスチャが競合を防ぐために、 `CarouselPage` 、`MasterDetailPage`します。

詳細については、 [ `CarouselPage`](xref:Xamarin.Forms.CarouselPage)を参照してください[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)Charles Petzold の Xamarin.Forms book の。

## <a name="summary"></a>まとめ

この記事では、使用する方法を示しました、 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)内のページのコレクションを移動します。 `CarouselPage`はギャラリーと同様に、コンテンツのページ間を移動するユーザーが左右にスワイプできるページです。


## <a name="related-links"></a>関連リンク

- [ページの変数](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
