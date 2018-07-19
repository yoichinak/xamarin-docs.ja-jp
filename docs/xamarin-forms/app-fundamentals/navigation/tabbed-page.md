---
title: Xamarin.Forms のタブ付きページ
description: Xamarin.Forms TabbedPage は、詳細領域にコンテンツを読み込む各タブでのタブと、詳細領域が大きくの一覧で構成されます。 この記事では、ページのコレクションを移動する、TabbedPage を使用する方法を示します。
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3eb978780222da2050fc91dfa41c68ef4bd3b6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996296"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms のタブ付きページ

_Xamarin.Forms TabbedPage は、詳細領域にコンテンツを読み込む各タブでのタブと、詳細領域が大きくの一覧で構成されます。この記事では、ページのコレクションを移動する、TabbedPage を使用する方法を示します。_

## <a name="overview"></a>概要

次のスクリーン ショットに示す、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)各プラットフォームで。

![](tabbed-page-images/tab1.png "TabbedPage 例")

次のスクリーン ショットは、各プラットフォームで、タブ形式に注目します。

![](tabbed-page-images/tabbedpage-components.png "TabbedPage タブ コンポーネント")

レイアウト、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)と、そのタブは、プラットフォームに依存します。

- Ios では、タブの一覧が、画面の下部に表示し、詳細エリアがその上。 各タブは 30x の透明度が通常の解決の PNG を 30、60 x 60 の高解像度、および 90 x 90 iPhone 6 用として使用するアイコン イメージもがさらに解決します。 複数の 5 つのタブがある場合、*詳細* タブが表示され、使用できる追加のタブにアクセスします。 Xamarin.Forms アプリケーションでイメージの読み込みの詳細については、次を参照してください。[イメージを操作](~/xamarin-forms/user-interface/images.md)します。 アイコンの要件の詳細については、次を参照してください。[タブ付きアプリケーションを作成する](~/ios/user-interface/controls/creating-tabbed-applications.md)します。

    > [!NOTE]
  > なお、 `TabbedRenderer` iOS が、オーバーライド可能な`GetIcon`メソッドをタブのアイコンを指定したソースから読み込むために使用できます。 このオーバーライドでは、上のアイコンとして SVG イメージを使用すること、`TabbedPage`します。 さらに、アイコンの選択と選択されていないバージョンを指定することができます。

- Android では、既定では、画面の上部にあるタブの一覧が表示され、詳細エリアが未満です。 ただし、タブの一覧は、プラットフォーム固有の画面の下部に移動できます。 詳細については、次を参照してください。 [TabbedPage ツールバーの配置の設定と色](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)します。

    > [!NOTE]
  > Android では、AppCompat を使用して、各タブもアイコンが表示されます、注意してください。 さらに、 `TabbedPageRenderer` Android AppCompat が、オーバーライド可能な`SetTabIcon`カスタムからタブのアイコンの読み込みに使用できるメソッド`Drawable`します。 このオーバーライドでは、上のアイコンとして SVG イメージを使用すること、`TabbedPage`します。

- Windows タブレット フォーム ファクター、上のタブが表示されない常に、ユーザーがスワイプ ダウンする必要があります (または右クリックし、マウスが取り付けられている場合)、タブを表示する、 `TabbedPage` (次に示す)。

![](tabbed-page-images/windows-tabs.png "Windows 上の TabbedPage タブ")

## <a name="creating-a-tabbedpage"></a>TabbedPage を作成します。

2 つの方法は、作成に使用できる、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [設定](#Populating_a_TabbedPage_with_a_Page_Collection)、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)子のコレクションを持つ[ `Page` ](xref:Xamarin.Forms.Page)などのコレクション オブジェクト[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス。
- [割り当てる](#Populating_a_TabbedPage_with_a_Template)コレクション、 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)プロパティと割り当てを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティのページを返すコレクション内のオブジェクト。

両方の方法で、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)ユーザーは、各タブを選択すると各ページが表示されます。

> [!NOTE]
> お勧めしますが、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)を代入するか[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)と[ `ContentPage`](xref:Xamarin.Forms.ContentPage)インスタンスのみです。 これは、すべてのプラットフォームで一貫したユーザー エクスペリエンスを確保するのに役立ちます。

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>TabbedPage ページのコレクションの作成

次の XAML コード例は、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)子のコレクションを設定することによって構築された[ `Page` ](xref:Xamarin.Forms.Page)オブジェクト。

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

次のコード例は、相当するものを示しています。 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) c# で作成します。

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)は 2 つの子が格納されます[ `Page` ](xref:Xamarin.Forms.Page)オブジェクト。 最初の子は、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス、および 2 番目のタブは、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)を含む、`ContentPage`インスタンス。

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) UI の仮想化をサポートしていません。 そのため場合、パフォーマンスに影響する可能性があります、`TabbedPage`が多すぎるの子要素が含まれています。

次のスクリーン ショットに示さ、 `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)上に表示されるインスタンス、*今日*タブ。

![](tabbed-page-images/today-page.png "TabbedPage に ContentPage")

選択すると、*スケジュール*タブが表示されます、 `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)にラップされて、インスタンス、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)インスタンスとに、次のスクリーン ショット。

![](tabbed-page-images/schedule-page.png "NavigationPage を TabbedPage に")

レイアウトについては、 [ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)を参照してください[を実行するナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)します。

> [!NOTE]
> 配置することができます、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)に、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)、配置することは推奨されません、`TabbedPage`に、 `NavigationPage`。 これは、ios を`UITabBarController`のラッパーとして機能して常に、`UINavigationController`します。 詳細については、次を参照してください。[ビュー コント ローラーのインターフェイスを組み合わせて](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html)ios 開発者ライブラリ。

#### <a name="navigation-inside-a-tab"></a>タブ内のナビゲーション

ナビゲーションを呼び出すことによって 2 番目のタブから実行することができます、 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*)メソッドを[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)のプロパティ、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス、次のコード例で示した。

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

これにより、`UpcomingAppointmentsPage` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。 これは、次のスクリーン ショットに示されます。

![](tabbed-page-images/navigationpage.png "タブ内のナビゲーション")

ナビゲーションを使用して実行の詳細については、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)クラスを参照してください[階層型ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)します。

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>テンプレートを使用して、TabbedPage の設定

次の XAML コード例は、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)を割り当てることによって構築された、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を[ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)プロパティのページを返すコレクション内のオブジェクト:

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
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
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

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)設定によってデータが読み込まれて、 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource)分離コード ファイルのコンス トラクター内のプロパティ。

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

次のコード例は、相当するものを示しています。 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) c# で作成します。

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
        Icon = "monkeyicon.png",
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

各タブが表示されます、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)一連を使用する[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)と[ `Label` ](xref:Xamarin.Forms.Label)タブのデータを表示するインスタンス。次のスクリーン ショットの内容を表示する、 *Tamarin*タブ。

![](tabbed-page-images/tab3.png "テンプレートを使用して、TabbedPage の設定")

別のタブを選択し、そのタブのコンテンツが表示されます。

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) UI の仮想化をサポートしていません。 そのため場合、パフォーマンスに影響する可能性があります、`TabbedPage`が多すぎるの子要素が含まれています。

詳細については、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)を参照してください[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)Charles Petzold の Xamarin.Forms book の。

## <a name="summary"></a>まとめ

この記事では、TabbedPage を使用して、ページのコレクションを移動する方法を説明します。 Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)一連のタブと、詳細領域が大きく、詳細領域にコンテンツを読み込む各タブで構成されます。


## <a name="related-links"></a>関連リンク

- [ページの変数](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
