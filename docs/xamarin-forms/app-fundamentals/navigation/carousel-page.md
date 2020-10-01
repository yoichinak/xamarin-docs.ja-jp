---
title: Xamarin.Forms のカルーセル ページ
description: Xamarin.Forms の CarouselPage は、ギャラリーのように、ユーザーが端から端までスワイプしてコンテンツの各ページをナビゲートできるページです。 この記事では、CarouselPage を使用してページのコレクション内を移動する方法を示します。
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0110fa7c348c4facaf60cd261c8a584904aa0b8e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563069"
---
# <a name="no-locxamarinforms-carousel-page"></a>Xamarin.Forms のカルーセル ページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)

_Xamarin.Forms の CarouselPage は、ギャラリーのように、ユーザーが端から端までスワイプしてコンテンツの各ページをナビゲートできるページです。この記事では、CarouselPage を使用してページのコレクション内を移動する方法を示します。_

> [!IMPORTANT]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) は [`CarouselView`](xref:Xamarin.Forms.CarouselView) に置き換えられています。これにより、ユーザーがスワイプして項目のコレクション内を移動できるスクロール可能なレイアウトが提供されます。 `CarouselView` の詳細については、「[Xamarin.Forms CarouselView](~/xamarin-forms/user-interface/carouselview/index.md)」を参照してください。

次のスクリーンショットは、各プラットフォームの [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) を示します。

![CarouselPage の 3 番目の項目](carousel-page-images/thirdpage.png)

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) のレイアウトは、どのプラットフォームでも同じです。 右から左にスワイプするとコレクション内のページを前方に移動でき、左から右にスワイプするとコレクション内のページを後方に移動できます。 次のスクリーンショットは、[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) インスタンスの最初のページです。

![CarouselPage の 1 番目の項目](carousel-page-images/firstpage.png)

次のスクリーンショットのように、右から左にスワイプすると 2 番目のページに移動します。

![CarouselPage の 2 番目の項目](carousel-page-images/secondpage.png)

もう一度右から左にスワイプすると 3 番目のページに移動し、左から右にスワイプすると前のページに戻ります。

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) では、UI の仮想化はサポートされません。 したがって、`CarouselPage` に含まれる子要素が多すぎると、パフォーマンスに影響する可能性があります。

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) を [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) の [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) ページに埋め込む場合は、`CarouselPage` と `MasterDetailPage` の間でジェスチャが競合するのを防ぐため、[`MasterDetailPage.IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) プロパティを `false` に設定する必要があります。

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) の詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)を参照してください。

## <a name="create-a-carouselpage"></a>CarouselPage を作成する

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) を作成するには、次の 2 つの方法を使用することができます。

- 子 [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスのコレクションを `CarouselPage` に[設定](#populate-a-carouselpage-with-a-page-collection)します。
- コレクションを [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) プロパティに[割り当て](#populate-a-carouselpage-with-a-template)、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティに割り当てて、コレクション内のオブジェクトに対する [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスを取得します。

どちらの方法でも、`CarouselPage` では各ページが順番に表示され、スワイプ操作によって次のページの表示に移動します。

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) に設定できるのは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスまたは `ContentPage` 派生クラスだけです。

### <a name="populate-a-carouselpage-with-a-page-collection"></a>CarouselPage にページ コレクションを設定する

次の XAML コード例では、3 つの [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスを表示する [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) を示します。

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

次に示すのは、C# での同等の UI のコード例です。

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

各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) では、特定の色に対する [`Label`](xref:Xamarin.Forms.Label) と、その色の [`BoxView`](xref:Xamarin.Forms.BoxView) が単に表示されます。

### <a name="populate-a-carouselpage-with-a-template"></a>CarouselPage にテンプレートを設定する

次の XAML コード例は、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティに割り当てて、コレクション内のオブジェクト用のページを返すことによって、[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) を作成します。

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

分離コード ファイルのコンストラクターで [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) プロパティを設定することにより、[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) にデータを設定します。

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

次に示すのは、C# で作成された同等の [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) のコード例です。

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

各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) では、特定の色に対する [`Label`](xref:Xamarin.Forms.Label) と、その色の [`BoxView`](xref:Xamarin.Forms.BoxView) が単に表示されます。

## <a name="related-links"></a>関連リンク

- [ページの変数](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (サンプル)](/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)
- [CarouselPageTemplate (サンプル)](/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)