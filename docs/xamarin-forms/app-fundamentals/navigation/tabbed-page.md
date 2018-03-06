---
title: "タブ ページ"
description: "Xamarin.Forms TabbedPage は、詳細領域へのコンテンツの読み込みの各タブで、タブより大きな詳細領域の一覧で構成されます。 この記事では、ページのコレクションを移動する、TabbedPage を使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 97ca114f1160168c7fd9439e31dc475bc37b0467
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="tabbed-page"></a>タブ ページ

_Xamarin.Forms TabbedPage は、詳細領域へのコンテンツの読み込みの各タブで、タブより大きな詳細領域の一覧で構成されます。この記事では、ページのコレクションを移動する、TabbedPage を使用する方法を示します。_

## <a name="overview"></a>概要

次のスクリーン ショットに示さ、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)プラットフォームごとに。

![](tabbed-page-images/tab1.png "TabbedPage 例")

次のスクリーン ショットは、各プラットフォームで、タブ形式に注目します。

![](tabbed-page-images/tabbedpage-components.png "TabbedPage タブ コンポーネント")

レイアウト、 [ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)とそのタブは、プラットフォームに依存します。

- Ios の場合、画面の下部にあるタブの一覧が表示されます、詳細領域上。 各タブにも、30 倍の透過性は、通常の解決と 30 PNG、60 x 60 高解像度、および 90 x 90 iPhone 6 の対象となるアイコン画像の解像度とします。 複数の 5 つのタブがある場合、*詳細* タブが表示され、使用できるその他のタブにアクセスします。 Xamarin.Forms のアプリケーションでイメージの読み込みの詳細については、次を参照してください。[イメージを操作](~/xamarin-forms/user-interface/images.md)です。 アイコンの要件の詳細については、次を参照してください。[タブ付きアプリケーションの作成](~/ios/user-interface/controls/creating-tabbed-applications.md)です。

    > [!NOTE]
  > なお、`TabbedRenderer`の iOS がある、オーバーライド可能な`GetIcon` タブのアイコンを指定したソースから読み込みに使用できるメソッドです。 このオーバーライドでは、上にアイコンとして SVG イメージを使用すること、`TabbedPage`です。 さらに、アイコンの選択と選択されていないバージョンを指定することができます。

- Android で、画面の上部にあるタブの一覧が表示される、詳細領域の下。 タブ名は自動的に大文字で入力し、1 つの画面に収まるようが多すぎますがある場合、ユーザーがタブのコレクションをスクロールできます。

    > [!NOTE]
  > ある AppCompat を Android で使用する場合の各タブもアイコンが表示されます、注意してください。 さらに、 `TabbedPageRenderer` Android AppCompat には、オーバーライド可能な`SetTabIcon`カスタムから タブのアイコンの読み込みに使用できるメソッド`Drawable`です。 このオーバーライドでは、上にアイコンとして SVG イメージを使用すること、`TabbedPage`です。

- Windows Phone で画面の上部にあるタブの一覧が表示されます、詳細領域の下。 名前は小文字、およびユーザーに自動的に変換 タブは、1 つの画面に収まるようが多すぎますがある場合、タブのコレクションをスクロールできます。
- 要因 - Windows タブレット フォームにタブが表示されない常に、ユーザーが方向にスワイプ ダウンする必要があります (または右クリックし、マウスが取り付けられている場合) 内のタブを表示する、 `TabbedPage` (下図のように)。

![](tabbed-page-images/windows-tabs.png "Windows 上の TabbedPage タブ")

## <a name="creating-a-tabbedpage"></a>TabbedPage を作成します。

2 つの方法は、作成に使用できる、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [追加](#Populating_a_TabbedPage_with_a_Page_Collection)、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)子のコレクションを[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)などのコレクション オブジェクト[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンス。
- [割り当てる](#Populating_a_TabbedPage_with_a_Template)コレクションには、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/)プロパティと割り当て、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)を[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/)のページを返すプロパティコレクション内のオブジェクト。

両方の方法で、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)ユーザーは、それぞれのタブを選択すると、各ページが表示されます。

> [!NOTE]
> お勧めする、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)を代入する[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)と[ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンスのみです。 これは、すべてのプラットフォームで一貫性のあるユーザー エクスペリエンスを確保するのに役立ちます。

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>ページのコレクションを使用して TabbedPage を設定します。

XAML コード例を次に、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)子のコレクションを設定して構築された[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)オブジェクト。

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

次のコード例は、該当するショートカットを示しています。 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) c# で作成します。

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)には、2 つの子[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)オブジェクト。 最初の子は、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンス、および 2 番目のタブは、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)を含む、`ContentPage`インスタンス。

> [!NOTE]
> **注**: [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) UI 仮想化をサポートしていません。 そのため、パフォーマンスが受ける場合、`TabbedPage`多数の子要素が含まれています。

次のスクリーン ショットに示さ、 `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)上に表示されるインスタンス、*今日* タブ。

![](tabbed-page-images/today-page.png "TabbedPage コンテンツ ページ")

選択すると、*スケジュール* タブの表示、 `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)でラップされているインスタンス、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)インスタンスとに表示される、次のスクリーン ショット:

![](tabbed-page-images/schedule-page.png "TabbedPage NavigationPage")

レイアウトについて、 [ `NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)を参照してください[を実行するナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)です。

> [!NOTE]
> 配置してもかまいません。 中に、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)に、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)、配置することは推奨されていません、`TabbedPage`に、`NavigationPage`です。 これは、理由は、ios の場合、`UITabBarController`のラッパーとして常には、機能、`UINavigationController`です。 詳細については、次を参照してください。[ビュー コント ローラーのインターフェイスを組み合わせて](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html)ios 開発者ライブラリです。

#### <a name="navigation-inside-a-tab"></a>タブ内での操作

ナビゲーションを実行できる 2 番目のタブからを呼び出すことによって、 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/)メソッドを[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)のプロパティ、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンス、次のコード例で示したようにします。

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

これにより、`UpcomingAppointmentsPage` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。 これは、次のスクリーン ショットで示されます。

![](tabbed-page-images/navigationpage.png "タブ内での操作")

ナビゲーションを使用して実行の詳細については、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)クラスを参照してください[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)です。

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>テンプレートを使用して TabbedPage を設定します。

XAML コード例を次に、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)を割り当てることによって構築された、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)を[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/)のページを返すプロパティコレクション内のオブジェクト:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)設定によってデータが格納されて、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/)分離コード ファイルのコンス トラクター内のプロパティ。

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

次のコード例は、該当するショートカットを示しています。 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) c# で作成します。

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

各タブが表示されます、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)の系列を使用する[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)と[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)タブのデータを表示するインスタンス。次のスクリーン ショットの内容を表示する、 *Tamarin* タブ。

![](tabbed-page-images/tab3.png "テンプレートを使用して TabbedPage を設定します。")

そのタブの内容を表示し、別のタブを選択します。

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) UI 仮想化をサポートしていません。 そのため、パフォーマンスが受ける場合、`TabbedPage`多数の子要素が含まれています。

詳細については、 [ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)を参照してください[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)Charles Petzold Xamarin.Forms 書籍のです。

## <a name="summary"></a>まとめ

この記事では、TabbedPage を使用して、ページのコレクションを移動する方法が示されています。 Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)タブより大きな詳細領域の一覧、詳細領域へのコンテンツの読み込みの各タブで構成されます。


## <a name="related-links"></a>関連リンク

- [ページの種類](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
