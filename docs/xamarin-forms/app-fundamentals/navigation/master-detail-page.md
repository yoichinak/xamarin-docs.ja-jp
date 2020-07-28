---
title: Xamarin.Forms のマスター詳細ページ
description: Xamarin.Forms の MasterDetailPage は、2 つの関連する情報ページ、つまり項目を表示するマスター ページと、マスター ページ上の項目に関する詳細を表示する詳細ページを管理するページです。 この記事では、MasterDetailPage を使用する方法と、情報ページ間を移動する方法について説明します。
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3a65e9bb90f01bcb5e0b1182a21d998e2335da9a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934525"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms のマスター詳細ページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage)

_Xamarin.Forms の MasterDetailPage は、2 つの関連する情報ページ、つまり項目を表示するマスター ページと、マスター ページ上の項目に関する詳細を表示する詳細ページを管理するページです。この記事では、MasterDetailPage を使用する方法と、情報ページ間を移動する方法について説明します。_

## <a name="overview"></a>概要

通常、マスター ページには、次のスクリーンショットのように、項目の一覧が表示されます。

[![マスター ページのコンポーネント](master-detail-page-images/masterpage-components.png)](master-detail-page-images/masterpage-components-large.png#lightbox "マスター ページのコンポーネント")

項目の一覧の場所は各プラットフォームで同じであり、いずれかの項目を選択すると対応する詳細ページに移動します。 さらに、マスター ページのナビゲーション バーに含まれるボタンを使用してアクティブな詳細ページに移動することもできます。

- iOS では、ナビゲーション バーはページの上部にあり、詳細ページに移動するボタンが含まれます。 さらに、マスター ページを左にスワイプすることで、アクティブな詳細ページに移動できます。
- Android では、ナビゲーション バーはページの上部にあり、タイトル、アイコン、詳細ページに移動するボタンが表示されます。 アイコンは、Android プラットフォーム固有プロジェクトにおいて `MainActivity` クラスを修飾する `[Activity]` 属性で定義されています。 さらに、マスター ページを左にスワイプするか、画面右端の詳細ページをタップするか、画面の下部にある *[戻る]* ボタンをタップすることによって、アクティブな詳細ページに移動できます。
- ユニバーサル Windows プラットフォーム (UWP) では、ナビゲーション バーはページの上部にあり、詳細ページに移動するボタンが含まれます。

次のスクリーンショットのように、詳細ページには、マスター ページで選択されている項目に対応するデータと、詳細ページの主要コンポーネントが表示されます。

![詳細ページのコンポーネント](master-detail-page-images/detailpage-components.png)

詳細ページにはナビゲーション バーが含まれ、その内容はプラットフォームによって異なります。

- iOS では、ナビゲーション バーはページの上部に存在し、タイトルが表示されています。また、詳細ページのインスタンスが [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスにラップされている場合は、マスター ページに戻るボタンがあります。 さらに、マスター ページには、詳細ページを右にスワイプすることによって戻ることができます。
- Android では、ナビゲーション バーはページの上部にあり、タイトル、アイコン、マスター ページに戻るボタンが表示されます。 Android プラットフォーム固有プロジェクトでは、アイコンは `MainActivity` クラスを修飾する `[Activity]` 属性で定義されています。
- UWP では、ナビゲーション バーはページの上部にあり、タイトルが表示され、マスター ページに戻るボタンがあります。

### <a name="navigation-behavior"></a>ナビゲーションの動作

マスター ページと詳細ページの間のナビゲーション エクスペリエンスの動作は、プラットフォームによって異なります。

- iOS では、マスター ページが左から "*スライド*" してくると、詳細ページは右にスライドしますが、詳細ページの左の部分はまだ表示されています。
- Android では、詳細ページとマスター ページは相互に "*オーバーレイ*" されます。
- UWP では、マスター ページが左からスライドして詳細ページの一部を覆います。ただし、[`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) プロパティが `Popover` に設定されている場合です。 詳細については、「[詳細ページの表示動作を制御する](#controlling-the-detail-page-display-behavior)」をご覧ください。

横モードでの動作も同様ですが、iOS と Android のマスター ページは縦モードのマスター ページと同じ幅なので、表示される詳細ページの部分が多くなります。

ナビゲーション動作の制御について詳しくは、「[詳細ページの表示動作を制御する](#controlling-the-detail-page-display-behavior)」をご覧ください。

## <a name="creating-a-masterdetailpage"></a>MasterDetailPage を作成する

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) には [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) プロパティと [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) プロパティ (どちらも [`Page`](xref:Xamarin.Forms.Page) 型) が含まれ、それぞれ、マスター ページと詳細ページの取得と設定に使用されます。

> [!IMPORTANT]
> [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) はルート ページとして設計されており、他のページの種類の子ページとして使用すると、予期されていない一貫性のない動作が発生する可能性があります。 さらに、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) のマスター ページは常に [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスにすること、および詳細ページは [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)、`ContentPage` のインスタンスのみで設定することをお勧めします。 これは、すべてのプラットフォームで一貫したユーザー エクスペリエンスにするのに役立ちます。

次の XAML コード例では、[`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) プロパティと [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) プロパティを設定する [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) を示します。

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

次のコード例は、同じ [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) を C# で作成します。

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

[`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) プロパティには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスが設定されます。 [`MasterDetailPage.Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) プロパティには、`ContentPage` インスタンスを含む [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) が設定されます。

### <a name="creating-the-master-page"></a>マスター ページを作成する

次に示す XAML のコード例は、`MasterPage` オブジェクトの宣言です。このオブジェクトは、[`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) プロパティを通して参照されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             IconImageSource="hamburger.png"
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

ページは、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティに `MasterPageItem` インスタンスの配列を設定することによって XAML のデータを設定された [`ListView`](xref:Xamarin.Forms.ListView) で構成されます。 各 `MasterPageItem` では、`Title`、`IconSource`、および `TargetType` プロパティが定義されています。

各 `MasterPageItem` を表示するため、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) が [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティに割り当てられます。 `DataTemplate` には、[`Image`](xref:Xamarin.Forms.Image) と [`Label`](xref:Xamarin.Forms.Label) で構成される [`ViewCell`](xref:Xamarin.Forms.ViewCell) が含まれます。 各 `MasterPageItem` について、[`Image`](xref:Xamarin.Forms.Image) では `IconSource` プロパティの値が表示され、[`Label`](xref:Xamarin.Forms.Label) では `Title` プロパティの値が表示されます。

ページでは、[`Title`](xref:Xamarin.Forms.Page.Title) プロパティと [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) プロパティが設定されます。 詳細ページにタイトル バーがある場合は、アイコンが詳細ページに表示されます。 iOS では、詳細ページのインスタンスを [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスにラップすることによって、これを有効にする必要があります。

> [!NOTE]
> [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) ページではその [`Title`](xref:Xamarin.Forms.Page.Title) プロパティを設定する必要があります。設定しないと例外が発生します。

次に示すのは、C# で作成された同等のページのコード例です。

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

    IconImageSource = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

次のスクリーンショットでは、各プラットフォームでのマスター ページを示します。

![マスター ページの例](master-detail-page-images/masterpage.png)

### <a name="creating-and-displaying-the-detail-page"></a>詳細ページを作成して表示する

`MasterPage` インスタンスに含まれる `ListView` プロパティによって公開されるその [`ListView`](xref:Xamarin.Forms.ListView) インスタンスにより、`MainPage` の [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) インスタンスは [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントを処理するためのイベント ハンドラーを登録できます。 これにより、`MainPage` のインスタンスは、[`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) プロパティに、選択された `ListView` 項目を表示するページを設定できます。 次に示すのは、イベント ハンドラーのコード例です。

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

`OnItemSelected` メソッドでは、次の操作が実行されます。

- [`ListView`](xref:Xamarin.Forms.ListView) のインスタンスから [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) を取得し、それが `null` でない場合は、詳細ページに、`MasterPageItem` の `TargetType` プロパティに格納されているページ種類の新しいインスタンスを設定します。 `MasterPage` の [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) プロパティを通して参照されているアイコンが iOS の詳細ページに表示されるように、ページの種類を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のインスタンス内にラップします。
- 次に `MasterPage` が表示されるときにどの `ListView` 項目も選択されていないように、[`ListView`](xref:Xamarin.Forms.ListView) の選択済み項目を `null` に設定します。
- [`MasterDetailPage.IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) プロパティを `false` に設定することにより、ユーザーに詳細ページを表示します。 このプロパティは、マスター ページまたは詳細ページが表示されるかどうかを制御します。 マスター ページを表示するには `true` に、詳細ページを表示するには `false` に、設定する必要があります。

次のスクリーンショットでは、`ContactPage` の詳細ページを示します。これは、マスター ページで選択された後に表示されます。

![詳細ページの例](master-detail-page-images/detailpage.png)

### <a name="controlling-the-detail-page-display-behavior"></a>詳細ページの表示動作を制御する

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) によるマスター ページと詳細ページの管理方法は、アプリケーションがスマートフォンまたはタブレットのどちらで実行されているか、デバイスの向き、および [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) プロパティの値によって異なります。 このプロパティでは、詳細ページの表示方法が決まります。 次の値を指定できます。

- **Default** – ページは、プラットフォームの既定値を使用して表示されます。
- **Popover** – 詳細ページがマスター ページをカバーまたは部分的にカバーします。
- **Split** – マスター ページが左側に、詳細ページが右側に表示されます。
- **SplitOnLandscape** – デバイスが横長の向きの場合は、分割画面が使用されます。
- **SplitOnPortrait** – デバイスが縦長の向きの場合は、分割画面が使用されます。

次の XAML コードの例では、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) で [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) プロパティを設定する方法を示します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

次のコード例は、同じ [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) を C# で作成します。

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

ただし、[`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) プロパティの値は、タブレットまたはデスクトップで実行されているアプリケーションのみに影響します。 スマートフォンで実行されているアプリケーションは、常に *Popover* 動作になります。

## <a name="summary"></a>まとめ

この記事では、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) を使用する方法と、情報ページ間を移動する方法を示しました。 Xamarin.Forms の `MasterDetailPage` は、2 つの関連する情報ページ、つまり項目を表示するマスター ページと、マスター ページ上の項目に関する詳細を表示する詳細ページを管理するページです。

## <a name="related-links"></a>関連リンク

- [ページの変数](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
