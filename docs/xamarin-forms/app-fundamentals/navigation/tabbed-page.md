---
title: Xamarin.Forms のタブ付きページ
description: Xamarin.Form の TabbedPage は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。 この記事では、TabbedPage を使用してページのコレクションを移動する方法について説明します。
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 6979b4d3e0d750ee962346a94dd832c86c92d995
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652948"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms のタブ付きページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)

_Xamarin.Form の TabbedPage は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。この記事では、TabbedPage を使用してページのコレクションを移動する方法について説明します。_

## <a name="overview"></a>概要

次のスクリーンショットは、各プラットフォームの [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を示します。

![](tabbed-page-images/tab1.png "TabbedPage の例")

次のスクリーンショットは、各プラットフォームのタブ形式を拡大したものです。

![](tabbed-page-images/tabbedpage-components.png "TabbedPage のタブ コンポーネント")

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) のレイアウトとタブは、プラットフォームによって異なります。

- iOS では、タブのリストが画面の下部に表示され、その上に詳細エリアが表示されます。 また、各タブにはアイコン イメージがあり、これらのサイズは、通常の解像度の場合は 30 × 30 (PNG) の透明度、高解像度の場合は 60 × 60、iPhone 6 Plus の場合は 90 × 90 である必要があります。 6 個以上のタブがある場合、 *[その他]* タブが表示され、これを使用して追加のタブにアクセスできます。 Xamarin.Forms アプリケーションでイメージを読み込む方法の詳細については、[イメージの操作](~/xamarin-forms/user-interface/images.md)に関するページを参照してください。 アイコンの要件の詳細については、[タブ付きアプリケーションの作成](~/ios/user-interface/controls/creating-tabbed-applications.md)に関するページを参照してください。

  > [!NOTE]
  > iOS 用の `TabbedRenderer` には、指定されたソースからタブ アイコンを読み込むために使用できるオーバーライド可能な `GetIcon` メソッドがあることに注意してください。 このオーバーライドにより、SVG イメージを `TabbedPage` のアイコンとして使用することができます。 さらに、アイコンの選択バージョンと未選択バージョンも提供することができます。

- Android では、既定により、タブのリストが画面の上部に表示され、その下に詳細エリアが表示されますが、 タブのリストは、プラットフォーム固有で画面の下部に移動できます。 詳細については、「[Setting TabbedPage Toolbar Placement and Color](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md)」(TabbedPage ツールバーの配置と色の設定) を参照してください。

  > [!NOTE]
  > Android で AppCompat を使用する場合、各タブにはアイコンも表示されることに注意してください。 また、Android AppCompat 用の `TabbedPageRenderer` には、カスタム `Drawable` からタブ アイコンを読み込むために使用できるオーバーライド可能な `GetIconDrawable` メソッドがあることにも注意してください。 このオーバーライドは、SVG イメージを `TabbedPage` のアイコンとして使用できるようにし、上部のタブ バーと下部のタブ バーの両方で機能します。 また、オーバーライド可能な `SetTabIcon` メソッドは、上部のタブ バー用のカスタム `Drawable` からタブ アイコンを読み込むために使用することもできます。

- Windows タブレット フォーム ファクターでは、タブは常に表示されるとは限らないため、ユーザーは下方向にスワイプ (マウスが接続されている場合は右クリック) して `TabbedPage` でタブを表示する必要があります (次のスクリーンショットを参照)。

    ![](tabbed-page-images/windows-tabs.png "Windows の TabbedPage のタブ")

## <a name="creating-a-tabbedpage"></a>TabbedPage の作成

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は次の特性を定義します。

- タイプが [`Color`](xref:Xamarin.Forms.Color) の[`BarBackgroundColor`](xref:Xamarin.Forms.TabbedPage.BarBackgroundColor) (タブ バーの背景色)。
- タイプが [`Color`](xref:Xamarin.Forms.Color) の[`BarTextColor`](xref:Xamarin.Forms.TabbedPage.BarTextColor) (タブ バーのテキストの色)。
- タイプが [`Color`](xref:Xamarin.Forms.Color) の [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) (タブが選択されているときの色)。
- タイプが [`Color`](xref:Xamarin.Forms.Color) の [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) (タブが選択されていないときの色)。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、スタイルを指定でき、プロパティがデータ バインディングの対象になる場合があります。

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成するには、次の 2 つの方法を使用することができます。

- 子 [`Page`](xref:Xamarin.Forms.Page) オブジェクトのコレクション ([`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスのコレクションなど) を使って [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を[作成](#Populating_a_TabbedPage_with_a_Page_Collection)する。
- コレクションを [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) プロパティに[割り当て](#Populating_a_TabbedPage_with_a_Template)、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティに割り当てて、コレクション内のオブジェクト用のページを返すようにする。

どちらの方法を使用する場合も、ユーザーが各タブを選択すると、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) に各ページが表示されます。

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスおよび [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスのみで作成することをお勧めします。 こうすることにより、すべてのプラットフォームで一貫したユーザー エクスペリエンスを提供することができます。

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>ページ コレクションを使って TabbedPage を作成する

次の XAML コード例は、子 [`Page`](xref:Xamarin.Forms.Page) オブジェクトのコレクションを使用して、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成します。

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

次のコード例は、同じ [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を C# で作成します。

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.IconImageSource = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、2 つの子 [`Page`](xref:Xamarin.Forms.Page) オブジェクトで作成されます。 最初の子は、[`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスで、2 番目のタブは、`ContentPage` インスタンスを含む [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) です。

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) では、UI の仮想化はサポートされません。 このため、`TabbedPage` に含まれる子要素が多すぎると、パフォーマンスに影響する可能性があります。

次のスクリーンショットは、`TodayPage` [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスを示します。これは、 *[今日]* タブで表示されます。

![](tabbed-page-images/today-page.png "TabbedPage 内の ContentPage")

*[スケジュール]* タブを選択すると、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスにラップされている `SchedulePage` [`ContentPage`](xref:Xamarin.Forms.ContentPage) が表示されます。これを次のスクリーンショットに示します。

![](tabbed-page-images/schedule-page.png "TabbedPage 内の NavigationPage")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のレイアウトの詳細については、「[ナビゲーションを実行する](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) を [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) に配置することは可能ですが、`TabbedPage` を `NavigationPage` に配置することはお勧めしません。 これは、iOS では、`UITabBarController` が常に `UINavigationController` のラッパーとして機能するためです。 詳細については、iOS 開発者ライブラリの「[Combined View Controller Interfaces](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html)」 (View Controller インターフェイスの結合) を参照してください。

#### <a name="navigation-inside-a-tab"></a>タブ内のナビゲーション

2 番目のタブからナビゲーションを実行するには、次のコード例で示すように、[`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティで [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) メソッドを呼び出します。

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

これにより、`UpcomingAppointmentsPage` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。 これを次のスクリーンショットに示します。

![](tabbed-page-images/navigationpage.png "タブ内のナビゲーション")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスを使用してナビゲーションを実行する方法の詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>テンプレートを使って TabbedPage を作成する

次の XAML コード例は、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) プロパティに割り当てて、コレクション内のオブジェクト用のページを返すことによって、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を作成します。

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
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

分離コード ファイルのコンストラクターで [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) プロパティを設定することにより、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) にデータを設定します。

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

次のコード例は、同じ [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) を C# で作成します。

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        IconImageSource = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
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

各タブでは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) が表示されます。これは、一連の [`StackLayout`](xref:Xamarin.Forms.StackLayout) インスタンスと [`Label`](xref:Xamarin.Forms.Label) インスタンスを使用して、各タブのデータを表示します。次のスクリーンショットは、 *[Tamarin]* タブのコンテンツを示します。

![](tabbed-page-images/tab3.png "テンプレートを使って TabbedPage を作成する")

別のタブを選択すると、そのタブのコンテンツが表示されます。

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) では、UI の仮想化はサポートされません。 このため、`TabbedPage` に含まれる子要素が多すぎると、パフォーマンスに影響する可能性があります。

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) の詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)を参照してください。

## <a name="summary"></a>まとめ

この記事では、TabbedPage を使用してページのコレクションを移動する方法について説明しました。 Xamarin.Form の [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。


## <a name="related-links"></a>関連リンク

- [ページの変数](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [ナビゲーション ページを含むタブ付きページ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)
- [タブ付きページ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
