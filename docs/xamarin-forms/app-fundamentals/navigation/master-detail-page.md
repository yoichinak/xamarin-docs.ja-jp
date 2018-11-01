---
title: Xamarin.Forms のマスター/詳細ページ
description: Xamarin.Forms MasterDetailPage は、項目を表示するマスター ページとマスター ページの項目に関する詳細を表示するための詳細ページ、情報の 2 つの関連ページを管理するページです。 この記事では、MasterDetailPage を使用し、情報のページ間を移動する方法について説明します。
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 25f6cf341fcf47d5dc5320f73855bb2a4e29a9e8
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675524"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms のマスター/詳細ページ

_Xamarin.Forms MasterDetailPage は、項目を表示するマスター ページとマスター ページの項目に関する詳細を表示するための詳細ページ、情報の 2 つの関連ページを管理するページです。この記事では、MasterDetailPage を使用し、情報のページ間を移動する方法について説明します。_

## <a name="overview"></a>概要

通常、次のスクリーン ショットに示すように、同様に、マスター ページで項目の一覧が表示されます。

[![](master-detail-page-images/masterpage-components.png "マスター ページのコンポーネント")](master-detail-page-images/masterpage-components-large.png#lightbox "マスター ページのコンポーネント")

項目の一覧の場所は、各プラットフォームで同一と対応する詳細ページに移動しますが、項目のいずれかを選択します。 さらに、マスター ページは、アクティブな詳細ページに移動するのに使用できるボタンを含むナビゲーション バーも機能します。

- Ios では、ナビゲーション バーは、ページの上部にあるし、詳細ページに移動するボタンがあります。 さらに、アクティブな詳細ページは、マスター ページを左にスワイプするに移動できます。
- Android では、ナビゲーション バーは、ページの上部にあるし、詳細ページにタイトル、アイコン、および移動するボタンが表示されます。 アイコンが定義されている、`[Activity]`を装飾する属性、 `MainActivity` Android のプラットフォーム固有プロジェクト内のクラス。 さらに、active の詳細ページに移動する、画面の右端にある詳細ページをタップして、タップしておよびマスター ページを左にスワイプすると、*戻る*画面の下部にあるボタン。
- ユニバーサル Windows プラットフォーム (UWP) で、ナビゲーション バーは、ページの上部に存在して詳細ページに移動するボタンがあります。

ページで、マスターで選択した項目に対応する詳細ページ表示データと、詳細ページの主要なコンポーネントの次のスクリーン ショットに表示されます。

![](master-detail-page-images/detailpage-components.png "詳細ページのコンポーネント")

詳細ページには、内容がプラットフォームに依存するは、ナビゲーション バーが含まれています。

- Ios では、ナビゲーション バーが存在、ページの上部にあると、タイトルを表示し、マスター ページに戻るボタンの詳細ページのインスタンスにラップされている、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)インスタンス。 さらに、マスター ページは、右側の詳細ページをスワイプするに返されることができます。
- Android では、ナビゲーション バーは、ページの上部にあるし、タイトル、アイコン、およびマスター ページに戻るボタンが表示されます。 アイコンが定義されている、`[Activity]`を装飾する属性、 `MainActivity` Android のプラットフォーム固有プロジェクト内のクラス。
- 、UWP では、ナビゲーション バーは、ページの上部にある、、タイトルを表示およびとマスター ページに戻るボタンがあります。

### <a name="navigation-behavior"></a>ナビゲーションの動作

マスター/詳細ページ間のナビゲーションの操作の動作は、プラットフォームに依存します。

- Ios では、詳細ページ*スライド*左、および詳細の左の部分からマスター ページのスライドとして右側にページが表示されています。
- 詳細とマスター ページは、android では、*オーバーレイ*相互にします。
- 詳細とマスター ページは、UWP の*スワップ*します。

同様の動作は、iOS と Android でマスター ページは、詳細ページの詳細が表示されますので縦モードの場合、マスター ページとしてのような幅にする点を除いて、横向きモードで実行されます。

ナビゲーションの動作を制御する方法の詳細については、次を参照してください。[詳細ページの表示動作を制御する](#Controlling_the_Detail_Page_Display_Behavior)します。

## <a name="creating-a-masterdetailpage"></a>MasterDetailPage を作成します。

A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)を含む[ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)と[ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)型の両方がプロパティ[ `Page` ](xref:Xamarin.Forms.Page)を取得し、マスター/詳細ページをそれぞれ設定に使用されます。

> [!IMPORTANT]
> A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)ように、ルート ページを設計および使用するように他のページの種類の子ページが予期しない、一貫性のない動作になる可能性があります。 さらにすることが推奨のマスター ページ、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)は常に、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス、および詳細ページを代入のみか[ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)、および`ContentPage`インスタンス。 これは、すべてのプラットフォームで一貫したユーザー エクスペリエンスを確保するのに役立ちます。

次の XAML コード例は、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)設定、 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)と[ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)プロパティ。

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

次のコード例は、相当するものを示しています。 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) c# で作成します。

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

[ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)プロパティに設定されて、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス。 [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)プロパティに設定されて、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)を含む、`ContentPage`インスタンス。

### <a name="creating-the-master-page"></a>マスター ページの作成

XAML のコード例を次の宣言を示しています、`MasterPage`を通じて参照される、オブジェクト、 [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)プロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

ページから成る、 [ `ListView` ](xref:Xamarin.Forms.ListView)を設定して XAML でデータの設定をその[ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティの配列を`MasterPageItem`インスタンス。 各`MasterPageItem`定義`Title`、 `IconSource`、および`TargetType`プロパティ。

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)に割り当てられている、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)それぞれを表示するためのプロパティ`MasterPageItem`します。 `DataTemplate`が含まれています、 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)で構成される、 [ `Image` ](xref:Xamarin.Forms.Image)と[ `Label`](xref:Xamarin.Forms.Label)します。 [ `Image` ](xref:Xamarin.Forms.Image)が表示されます、`IconSource`プロパティの値、および[ `Label` ](xref:Xamarin.Forms.Label)が表示されます、`Title`ごとのプロパティ値`MasterPageItem`します。

ページがその[ `Title` ](xref:Xamarin.Forms.Page.Title)と[ `Icon` ](xref:Xamarin.Forms.Page.Icon)プロパティを設定します。 詳細ページのタイトル バーを持っていれば、[詳細] ページで、アイコンが表示されます。 これは iOS での詳細ページのインスタンスをラップすることによって有効にする必要があります、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)インスタンス。

> [!NOTE]
> [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master)ページする必要がありますが、 [ `Title` ](xref:Xamarin.Forms.Page.Title)プロパティは、次の設定、または例外が発生します。

次のコード例では、c# で作成した同等のページを示します。

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

次のスクリーン ショットでは、各プラットフォームでマスター ページを表示します。

![](master-detail-page-images/masterpage.png "マスター ページの例")

### <a name="creating-and-displaying-the-detail-page"></a>作成して、詳細ページを表示します。

`MasterPage`インスタンスに含まれる、`ListView`を公開するプロパティの[ `ListView` ](xref:Xamarin.Forms.ListView)インスタンスように、 `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)インスタンスを登録できます、処理するイベント ハンドラー、 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)イベント。 これにより、`MainPage`を設定するインスタンス、 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)プロパティを表す、選択したページを`ListView`項目。 次のコード例では、イベント ハンドラーを示します。

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected`メソッドは、次の操作を実行します。

- 取得、 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)から、 [ `ListView` ](xref:Xamarin.Forms.ListView)インスタンスし、なっていないことを指定された`null`、詳細ページ、に格納されているページの種類の新しいインスタンスを設定`TargetType`のプロパティ、`MasterPageItem`します。 ページの型がラップされて、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)アイコンを通じて参照されていることを確認するインスタンス、 [ `Icon` ](xref:Xamarin.Forms.Page.Icon)プロパティを`MasterPage`iOS での詳細ページが表示されます。
- 選択された項目、 [ `ListView` ](xref:Xamarin.Forms.ListView)に設定されている`null`なしのように、`ListView`項目が次回の選択、`MasterPage`が表示されます。
- ユーザーに設定して詳細ページが表示されます、 [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented)プロパティを`false`します。 このプロパティは、マスターまたは詳細ページが表示されるかどうかを制御します。 設定する必要があります`true`、マスター ページを表示して`false`詳細ページを表示します。

次のスクリーン ショットに示す、`ContactPage`詳細ページで、マスター ページで選択されているされた後に表示されています。

![](master-detail-page-images/detailpage.png "詳細ページの例")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>詳細ページの表示動作を制御します。

どの[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)マスター/詳細ページを管理、アプリケーションがスマート フォンまたはタブレット、デバイスの向きの値で実行するかどうかによって異なります、 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)プロパティ。 このプロパティは、詳細ページの表示方法を決定します。 使用可能な値は次のとおりです。

- **既定**– プラットフォームの既定値を使用して、ページが表示されます。
- **ポップ オーバー** – 詳細ページでは、またはマスター ページを部分的にカバーします。
- **分割**: マスター ページが左に表示し、詳細ページは、右にします。
- **SplitOnLandscape** – デバイスが横方向の場合、分割画面を使用します。
- **SplitOnPortrait** – デバイスが縦向きの場合、分割画面を使用します。

次の XAML コード例は、設定する方法を示します、 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)プロパティを[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

次のコード例は、相当するものを示しています。 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) c# で作成します。

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

ただしの値、 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)プロパティでは、タブレットやデスクトップで実行されているアプリケーションのみに影響します。 常に、スマート フォンで実行されるアプリケーションが、*ポップ オーバー*動作します。

## <a name="summary"></a>まとめ

この記事では、使用する方法を示しました、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)し情報のページ間を移動します。 Xamarin.Forms`MasterDetailPage`は、項目を表示するマスター ページとマスター ページの項目に関する詳細を表示する詳細ページ 2 つのページの関連情報を管理するページです。


## <a name="related-links"></a>関連リンク

- [ページの変数](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
