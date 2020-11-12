---
title: Xamarin.Forms の TabbedPage
description: Xamarin.Forms の TabbedPage は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。 この記事では、TabbedPage を使用してページのコレクションを移動する方法について説明します。
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d5c380a5ce6e76b0f9275b09d2943be479ef09e4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370898"
---
# <a name="no-locxamarinforms-tabbedpage"></a>Xamarin.Forms の TabbedPage

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)

Xamarin.Forms の [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。 次のスクリーンショットは、iOS と Android での `TabbedPage` を示しています。

[![iOS と Android 上の 3 つのタブを含む TabbedPage のスクリーンショット](tabbed-page-images/tabbedpage-today.png "3 つのタブがある TabbedPage")](tabbed-page-images/tabbedpage-today-large.png#lightbox "3 つのタブがある TabbedPage")

iOS では、タブのリストが画面の下部に表示され、その上に詳細エリアが表示されます。 各タブは、タイトルとアイコンで構成されます。これは、アルファ チャネルを含む PNG ファイルです。 縦長の向きでは、タブ バーのアイコンがタブ タイトルの上に表示されます。 横長の向きでは、アイコンとタイトルが横に並んで表示されます。 また、デバイスと向きに応じて、ノーマルまたはコンパクトなタブ バーが表示される場合があります。 6 個以上のタブがある場合、 **[その他]** タブが表示され、これを使用して追加のタブにアクセスできます。 アイコン要件の詳細については、developer.apple.com で[タブ バーのアイコン サイズ](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons#tab-bar-icon-size)に関するページを参照してください。

> [!TIP]
> iOS 用の `TabbedRenderer` には、指定されたソースからタブ アイコンを読み込むために使用できるオーバーライド可能な `GetIcon` メソッドがあります。 このオーバーライドにより、SVG イメージを `TabbedPage` のアイコンとして使用することができます。 さらに、アイコンの選択バージョンと未選択バージョンも提供することができます。

Android では、タブのリストが画面の上部に表示され、その下に詳細エリアが表示されます。 各タブは、タイトルとアイコンで構成されます。これは、アルファ チャネルを含む PNG ファイルです。 タブは、プラットフォーム固有で画面の下部に移動できます。 6 個以上のタブがあり、画面の下部にタブ リストがある場合、 *[その他]* タブが表示され、これを使用して追加のタブにアクセスできます。 アイコン要件の詳細については、material.io の[タブ](https://material.io/components/tabs/#)に関するページと、developer.android.com の「[各種のセル密度をサポートする](https://developer.android.com/training/multiscreen/screendensities)」を参照してください。 タブを画面の下部に移動する方法については、[TabbedPage ツール バーの配置と色の設定](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md)に関する記事を参照してください。

> [!TIP]
> Android AppCompat 用の `TabbedPageRenderer` には、カスタム `Drawable` からタブ アイコンを読み込むために使用できるオーバーライド可能な `SetTabIconImageSource` メソッドがあります。 このオーバーライドは、SVG イメージを `TabbedPage` のアイコンとして使用できるようにし、上部のタブ バーと下部のタブ バーの両方で機能します。

Universal Windows Platform (UWP) では、タブのリストが画面の上部に表示され、その下に詳細エリアが表示されます。 各タブは、タイトルで構成されます。 ただし、各タブにはプラットフォーム固有のアイコンを追加できます。 詳細については、「[Windows 上の TabbedPage アイコン](~/xamarin-forms/platform/windows/tabbedpage-icons.md)」を参照してください。

## <a name="create-a-tabbedpage"></a>TabbedPage の作成

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成するには、次の 2 つの方法を使用することができます。

- 子 [`Page`](xref:Xamarin.Forms.Page) オブジェクトのコレクション ([`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトのコレクションなど) を使って [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成する。 詳細については、「[ページ コレクションを使って TabbedPage を作成する](#populate-a-tabbedpage-with-a-page-collection)」を参照してください。
- コレクションを [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) プロパティに割り当て、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティに割り当てて、コレクション内のオブジェクト用のページを返すようにする。 詳細については、「[テンプレートを使って TabbedPage を作成する](#populate-a-tabbedpage-with-a-template)」を参照してください。

どちらの方法を使用する場合も、ユーザーが各タブを選択すると、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) に各ページが表示されます。

> [!IMPORTANT]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスおよび [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスのみで作成することをお勧めします。 こうすることにより、すべてのプラットフォームで一貫したユーザー エクスペリエンスを提供することができます。

また、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) では次のプロパティが定義されます。

- タイプが [`Color`](xref:Xamarin.Forms.Color) の [`BarBackgroundColor`](xref:Xamarin.Forms.TabbedPage.BarBackgroundColor) (タブ バーの背景色)。
- タイプが [`Color`](xref:Xamarin.Forms.Color) の [`BarTextColor`](xref:Xamarin.Forms.TabbedPage.BarTextColor) (タブ バーのテキストの色)。
- タイプが [`Color`](xref:Xamarin.Forms.Color) の [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) (タブが選択されているときの色)。
- タイプが [`Color`](xref:Xamarin.Forms.Color) の [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) (タブが選択されていないときの色)。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、スタイルを指定でき、プロパティがデータ バインディングの対象になる場合があります。

> [!WARNING]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) では、`TabbedPage` の構築時に各 [`Page`](xref:Xamarin.Forms.Page) オブジェクトが作成されます。 これにより、特に `TabbedPage` がアプリケーションのルート ページである場合に、ユーザー エクスペリエンスが低下する可能性があります。 ただし、Xamarin.Forms シェルを使用すると、ナビゲーションに応じて、タブ バーを介してアクセスされるページをオン デマンドで作成できます。 詳細は、「[Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

## <a name="populate-a-tabbedpage-with-a-page-collection"></a>ページ コレクションを使って TabbedPage を作成する

子 [`Page`](xref:Xamarin.Forms.Page) オブジェクトのコレクション ([`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトのコレクションなど) を使って [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成できます。 これは、`Page` オブジェクトを [`TabbedPage.Children`](xref:Xamarin.Forms.MultiPage`1.Children*) コレクションに追加することで実現されます。 XAML では次のようにしてこれが実現されます。

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" IconImageSource="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) の派生元である、[`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1) クラスの [`Children`](xref:Xamarin.Forms.MultiPage`1.Children*) プロパティは、`MultiPage<T>` の `ContentProperty` です。 そのため、XAML では、[`Page`](xref:Xamarin.Forms.Page) オブジェクトを `Children` プロパティに明示的に割り当てる必要はありません。

これに相当する C# コードを次に示します。

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    NavigationPage navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.IconImageSource = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

この例では、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) に 2 つの [`Page`](xref:Xamarin.Forms.ContentPage) オブジェクトが設定されています。 最初の子は [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトで、2 番目の子は、`ContentPage` オブジェクトを含む [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) です。

次のスクリーンショットは、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 内の [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトを示しています。

[![iOS と Android 上の 3 つのタブを含む TabbedPage のスクリーンショット](tabbed-page-images/tabbedpage-today.png "3 つのタブがある TabbedPage")](tabbed-page-images/tabbedpage-today-large.png#lightbox "3 つのタブがある TabbedPage")

別のタブを選択すると、タブを表す [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが表示されます。

[![iOS と Android 上のタブを含む TabbedPage のスクリーンショット](tabbed-page-images/tabbedpage-week.png "タブがある TabbedPage")](tabbed-page-images/tabbedpage-week-large.png#lightbox "タブがある TabbedPage")

**[スケジュール]** タブで、 [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) オブジェクト内にラップされます。

> [!WARNING]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) を [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) に配置することはできますが、`TabbedPage` を `NavigationPage` に配置することはお勧めしません。 これは、iOS では、`UITabBarController` が常に `UINavigationController` のラッパーとして機能するためです。 詳細については、iOS 開発者ライブラリの「[Combined View Controller Interfaces](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html)」 (View Controller インターフェイスの結合) を参照してください。

## <a name="navigate-within-a-tab"></a>タブ内での移動

ナビゲーションは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) オブジェクト内にラップされている場合に、タブ内で実行できます。 これを行うには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティに対して [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) メソッドを呼び出します。

```csharp
await Navigation.PushAsync (new UpcomingAppointmentsPage ());
```

移動先のページは、[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) メソッドへの引数として指定されます。 この例では、`UpcomingAppointmentsPage` ページがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。

[![iOS および Android 上のタブ内のナビゲーションのスクリーンショット](tabbed-page-images/tabbedpage-upcoming.png "タブ内の TabbedPage ナビゲーション")](tabbed-page-images/tabbedpage-upcoming-large.png#lightbox "タブ内の TabbedPage ナビゲーション")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスを使用してナビゲーションを実行する方法の詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

## <a name="populate-a-tabbedpage-with-a-template"></a>テンプレートを使って TabbedPage を作成する

データのコレクションを [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) プロパティに割り当て、データを [`Page`](xref:Xamarin.Forms.Page) オブジェクトとしてテンプレート化する [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティに [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) オブジェクトを割り当てることにより、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成できます。 XAML では次のようにしてこれが実現されます。

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage"
            ItemsSource="{x:Static local:MonkeyDataModel.All}">            
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" IconImageSource="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

これに相当する C# コードを次に示します。

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() =>
    {
      var nameLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage
      {
        IconImageSource = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children =
          {
            nameLabel,
            image,
            new StackLayout
            {
              Padding = new Thickness (50, 10),
              Children =
              {
                new StackLayout
                {
                  Orientation = StackOrientation.Horizontal,
                  Children =
                  {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                // ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

この例では、各タブは、[`Image`](xref:Xamarin.Forms.Image) および [`Label`](xref:Xamarin.Forms.Label) オブジェクトを使用してタブのデータを表示する [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトで構成されています。

[![iOS と Android 上のテンプレート化された TabbedPage のスクリーンショット](tabbed-page-images/tabbedpage-template.png "テンプレート化された TabbedPage")](tabbed-page-images/tabbedpage-template-large.png#lightbox "テンプレート化された TabbedPage")

別のタブを選択すると、タブを表す [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが表示されます。

## <a name="related-links"></a>関連リンク

- [ナビゲーション ページを含むタブ付きページ (サンプル)](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)
- [タブ付きページ (サンプル)](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)
- [階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [ページの変数](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPage API](xref:Xamarin.Forms.TabbedPage)